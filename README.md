# 📷 Aplikasi Presensi Guru QR Code

Aplikasi ini digunakan untuk mencatat kehadiran guru secara otomatis dengan memindai QR Code menggunakan kamera laptop atau perangkat lain.  
Data presensi dikirim langsung ke **Google Spreadsheet** melalui **Google Apps Script Web App**.

---

## ✨ Fitur Utama
- **Scan QR Code** menggunakan kamera perangkat (via `html5-qrcode`).
- **Mode Presensi**: Masuk dan Pulang.
- **Jam & Tanggal Realtime** ditampilkan di layar.
- **Notifikasi Status** presensi (berhasil, sudah absen, atau error).
- **Batas Presensi**: 1 kali per guru per mode per hari.
- **Sound Effect** beep saat QR terbaca.
- **Desain Minimalis & Responsif** — muat dalam 1 layar tanpa scroll.

---

## 📂 Struktur Folder
├── index.html # File utama aplikasi
├── assets/
│ └── beep.mp3 # Suara beep saat QR terbaca
└── README.md # Dokumentasi singkat

---

## 🚀 Cara Menggunakan
1. **Upload** semua file ke GitHub Pages atau server yang mendukung HTTPS.
2. Pastikan file **`beep.mp3`** ada di folder `assets/`.
3. **Ganti URL Web App** di `index.html` dengan URL Google Apps Script Anda:
   ```javascript
   const webAppURL = "URL_WEB_APP_ANDA";
   Buka aplikasi di browser yang mendukung kamera.
4. Pilih Mode Masuk/Pulang, klik Start, lalu arahkan QR Code ke kamera.
5. Lihat status presensi di layar.
   
---

## 🔧 Konfigurasi Google Apps Script
Gunakan skrip Apps Script untuk menerima parameter data dan mode, lalu simpan ke Google Spreadsheet.

Pastikan Web App dipublikasikan dengan akses "Anyone" agar bisa menerima request dari aplikasi.

---

## 📜 Lisensi
Proyek ini dibuat untuk keperluan internal sekolah.
Penggunaan bebas untuk tujuan non-komersial.
