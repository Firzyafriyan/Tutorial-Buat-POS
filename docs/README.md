# Dokumentasi Sistem Aplikasi Point of Sales (POS)

Selamat datang di dokumentasi resmi analisis proyek **Aplikasi Point of Sales (POS)** berbasis **Laravel 8**.

---

## 📌 Ikhtisar Proyek

Aplikasi Point of Sales (POS) ini dirancang khusus untuk memfasilitasi operasional ritel, kasir, manajemen inventaris, dan pencatatan keuangan harian toko secara terintegrasi. Sistem ini dilengkapi fitur otomatisasi seperti cetak barcode produk, kartu member pelanggan, manajemen supplier, pencatatan pembelian stok, transaksi kasir real-time (POS), kalkulasi laba/rugi, serta cetak struk/nota (thermal & invoice A4).

- **Framework**: Laravel 8 (PHP ^7.3 | ^8.0)
- **Frontend / UI**: AdminLTE v2 (Bootstrap 3, jQuery, DataTables, Font Awesome)
- **Database**: MySQL / MariaDB
- **Autentikasi & RBAC**: Laravel Jetstream / Fortify + Custom Middleware `CekLevel` (Admin & Kasir)
- **Reporting & Printing**: DomPDF (`barryvdh/laravel-dompdf`) & Barcode Generator (`milon/barcode`)

---

## 📂 Struktur Dokumentasi

Dokumentasi ini dibagi menjadi beberapa bagian tematik yang komprehensif untuk memudahkan pemahaman arsitektur, basis data, fungsionalitas, hingga pemeliharaan sistem:

| Dokumen | Deskripsi | Tautan |
| :--- | :--- | :--- |
| **Arsitektur & Teknologi** | Detail tech stack, dependensi, arsitektur MVC, middleware, dan struktur direktori. | [arsitektur-dan-teknologi.md](file:///c:/Dev/Projects/Tutorial-Buat-POS/docs/arsitektur-dan-teknologi.md) |
| **Basis Data & Relasi (ERD)** | Kamus data, skema tabel, diagram relasi entitas (Mermaid ERD), dan relasi model Eloquent. | [basis-data-dan-relasi.md](file:///c:/Dev/Projects/Tutorial-Buat-POS/docs/basis-data-dan-relasi.md) |
| **Fitur & Alur Bisnis** | Penjelasan modul POS, flow transaksi kasir, pembelian restock, laporan keuangan, dan matriks hak akses. | [fitur-dan-alur-bisnis.md](file:///c:/Dev/Projects/Tutorial-Buat-POS/docs/fitur-dan-alur-bisnis.md) |
| **Instalasi & Penggunaan** | Prasyarat sistem, langkah instalasi lokal, migrasi seeder, akun default, dan manual operasional. | [instalasi-dan-panduan-penggunaan.md](file:///c:/Dev/Projects/Tutorial-Buat-POS/docs/instalasi-dan-panduan-penggunaan.md) |
| **Analisis Kode & Rekomendasi** | Evaluasi keamanan, transaksi database (`DB::transaction`), skalabilitas, dan roadmap modernisasi sistem. | [analisis-kode-dan-rekomendasi.md](file:///c:/Dev/Projects/Tutorial-Buat-POS/docs/analisis-kode-dan-rekomendasi.md) |

---

## 👥 Pengguna Utama Sistem

1. **Administrator (Level 1)**: Memiliki kendali penuh terhadap konfigurasi toko, manajemen master data (produk, kategori, supplier, member), transaksi pembelian, rekap laporan keuangan, dan manajemen user.
2. **Kasir (Level 2)**: Berfokus pada operasional penjualan kasir di kasir (POS), scan barcode produk, pemilihan member, penerimaan uang pembayaran, dan pencetakan struk penjualan.
