# Aplikasi Presensi Guru QR Code

Aplikasi ini digunakan untuk mencatat kehadiran guru secara otomatis dengan memindai QR Code menggunakan kamera laptop atau perangkat lain.  
Data presensi dikirim langsung ke **Google Spreadsheet** melalui **Google Apps Script Web App**.

---

## Fitur Utama
- **Scan QR Code** menggunakan kamera perangkat (via `html5-qrcode`).
- **Mode Presensi**: Masuk dan Pulang.
- **Jam & Tanggal Realtime** ditampilkan di layar.
- **Notifikasi Status** presensi (berhasil, sudah absen, atau error).
- **Batas Presensi**: 1 kali per guru per mode per hari.
- **Sound Effect** beep saat QR terbaca.
- **Desain Minimalis & Responsif** — muat dalam 1 layar tanpa scroll.

---

## Struktur Folder
1. index.html # File utama aplikasi
2. assets/beep.mp3 # Suara beep saat QR terbaca
3. README.md # Dokumentasi singkat

---

## 🚀 Cara Menggunakan
1. **Upload** semua file ke GitHub Pages atau server yang mendukung HTTPS.
2. Pastikan file **`beep.mp3`** ada di folder `assets/`.
3. **Ganti URL Web App** di `index.html` dengan URL Google Apps Script Anda:

   ```javascript
   const webAppURL = "URL_WEB_APP_ANDA";
   
5. Buka aplikasi di browser yang mendukung kamera.
6. Pilih Mode Masuk/Pulang, klik Start, lalu arahkan QR Code ke kamera.
7. Lihat status presensi di layar.
   
---

## 🔧 Konfigurasi Google Apps Script
Gunakan skrip Apps Script untuk menerima parameter data dan mode, lalu simpan ke Google Spreadsheet.
Pastikan Web App dipublikasikan dengan akses "Anyone" agar bisa menerima request dari aplikasi.

```javascript
function doGet(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("DataPresensi");
  var data = e.parameter.data;
  var mode = e.parameter.mode ? e.parameter.mode.toLowerCase().trim() : "";

  if (!data || !mode) {
    return ContentService.createTextOutput("Gagal: data atau mode kosong");
  }

  var parts = data.split(" | ");
  var idGuru = parts[0] ? parts[0].trim() : "";
  var namaGuru = parts[1] ? parts[1].trim() : "";

  if (!idGuru) {
    return ContentService.createTextOutput("Gagal: ID Guru kosong");
  }

  var today = Utilities.formatDate(new Date(), "GMT+7", "yyyy-MM-dd");
  var nowTime = Utilities.formatDate(new Date(), "GMT+7", "HH:mm:ss");
  var lastRow = sheet.getLastRow();
  var foundRow = -1;

  // Cari apakah sudah ada presensi untuk ID ini pada tanggal hari ini
  if (lastRow > 1) {
    var dataPresensi = sheet.getRange(2, 1, lastRow - 1, 5).getValues();
    for (var i = 0; i < dataPresensi.length; i++) {
      var tglRow = "";
      if (Object.prototype.toString.call(dataPresensi[i][0]) === "[object Date]") {
        tglRow = Utilities.formatDate(new Date(dataPresensi[i][0]), "GMT+7", "yyyy-MM-dd");
      } else {
        tglRow = Utilities.formatDate(new Date(dataPresensi[i][0].toString()), "GMT+7", "yyyy-MM-dd");
      }
      var idRow = dataPresensi[i][1] ? dataPresensi[i][1].toString().trim() : "";
      if (tglRow === today && idRow === idGuru) {
        foundRow = i + 2; // +2 karena array mulai index 0 dan skip header
        break;
      }
    }
  }

  if (mode === "masuk") {
    if (foundRow > -1) {
      var jamMasuk = sheet.getRange(foundRow, 4).getValue();
      if (jamMasuk) {
        return ContentService.createTextOutput("Sudah presensi masuk hari ini");
      } else {
        sheet.getRange(foundRow, 4).setValue(nowTime);
        return ContentService.createTextOutput("Presensi masuk berhasil");
      }
    } else {
      sheet.appendRow([today, idGuru, namaGuru, nowTime, ""]);
      return ContentService.createTextOutput("Presensi masuk berhasil");
    }
  }

  if (mode === "pulang") {
    if (foundRow > -1) {
      var jamPulang = sheet.getRange(foundRow, 5).getValue();
      if (jamPulang) {
        return ContentService.createTextOutput("Sudah presensi pulang hari ini");
      } else {
        sheet.getRange(foundRow, 5).setValue(nowTime);
        return ContentService.createTextOutput("Presensi pulang berhasil");
      }
    } else {
      return ContentService.createTextOutput("Belum presensi masuk hari ini");
    }
  }

  return ContentService.createTextOutput("Mode tidak dikenali");
}
javascript```

---

## Lisensi
Proyek ini dibuat untuk keperluan internal sekolah.
Penggunaan bebas untuk tujuan non-komersial.
