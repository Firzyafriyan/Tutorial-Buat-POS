# Panduan Instalasi & Penggunaan Sistem POS

Panduan ini berisi langkah-langkah komprehensif mulai dari persiapan lingkungan (*environment*), instalasi dependensi, migrasi database, hingga panduan operasional harian bagi Administrator dan Kasir.

---

## 1. Prasyarat Sistem (*System Requirements*)

Pastikan komputer atau server telah memenuhi spesifikasi berikut:

- **PHP**: Versi `7.3`, `7.4`, atau `8.0` *(Disarankan PHP 7.4 atau 8.0)*
- **Ekstensi PHP Wajib**:
  - `pdo_mysql`, `mbstring`, `openssl`, `tokenizer`, `xml`, `ctype`, `json`, `bcmath`, `fileinfo`, `gd` *(wajib untuk render barcode & gambar dompdf)*
- **Database**: MySQL Server $\ge$ 5.7 atau MariaDB $\ge$ 10.3
- **Package Manager**:
  - [Composer](https://getcomposer.org/) $\ge$ 2.0
  - [Node.js](https://nodejs.org/) $\ge$ 14.x & NPM *(opsional jika mengompilasi ulang aset)*

---

## 2. Langkah-Langkah Instalasi

### Langkah 1: Clone atau Buka Folder Proyek
Buka terminal (PowerShell / Git Bash) di direktori kerja:
```bash
cd c:/Dev/Projects/Tutorial-Buat-POS
```

### Langkah 2: Instalasi Dependensi PHP
Jalankan composer untuk memasang dependensi vendor:
```bash
composer install
```
> [!TIP]
> Jika Anda menjalankan PHP versi 8.1 ke atas di mesin lokal dan menemui kendala dependensi lama, tambahkan opsi flag:
> `composer install --ignore-platform-reqs`

### Langkah 3: Konfigurasi File Lingkungan (`.env`)
Salin file konfigurasi contoh menjadi `.env`:
```bash
cp .env.example .env
```
Buka file `.env` dan sesuaikan kredensial koneksi database MySQL:
```ini
APP_NAME="Aplikasi POS"
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=pos_db
DB_USERNAME=root
DB_PASSWORD=
```
*(Pastikan database `pos_db` telah dibuat di MySQL phpMyAdmin / database client Anda).*

### Langkah 4: Generate Application Encryption Key
```bash
php artisan key:generate
```

### Langkah 5: Jalankan Migrasi & Database Seeder
Perintah ini akan membuat 11 tabel sistem sekaligus mengisi akun default dan pengaturan toko awal:
```bash
php artisan migrate --seed
```

### Langkah 6: Buat Symlink Storage (Jika Diperlukan)
Untuk mendukung upload foto profil, logo toko, dan kartu member:
```bash
php artisan storage:link
```

### Langkah 7: Jalankan Server Pengembangan Lokal
```bash
php artisan serve
```
Aplikasi sekarang dapat diakses melalui browser pada URL: `http://127.0.0.1:8000`.

---

## 3. Akun Pengguna Bawaan (*Default Credentials*)

Sistem telah dilengkapi dua akun awal dari database seeder ([UserTableSeeder.php](file:///c:/Dev/Projects/Tutorial-Buat-POS/database/seeders/UserTableSeeder.php)):

| Peran (Role) | Email Login | Password Default | Hak Akses Utama |
| :--- | :--- | :--- | :--- |
| **Administrator** | `admin@gmail.com` | `123` | Akses penuh ke seluruh menu sistem, laporan laba rugi, dan konfigurasi toko. |
| **Kasir** | `kasir1@gmail.com` | `123` | Akses khusus modul penjualan kasir (POS) dan cetak nota belanja. |

> [!WARNING]
> Sangat disarankan untuk segera memperbarui password bawaan akun di menu **Profil Pengguna** setelah instalasi awal.

---

## 4. Panduan Operasional Harian

### 4.1. Panduan Kasir (Operasional Penjualan)
1. **Memulai Sesi Kasir**:
   - Login menggunakan akun kasir (`kasir1@gmail.com`).
   - Masuk ke menu **Transaksi Baru** di sidebar kiri.
2. **Memasukkan Produk**:
   - Klik tombol **Pilih Produk** untuk menampilkan modal daftar barang, atau langsung ketik/scan kode barcode produk.
   - Sesuaikan kolom kuantitas (`Qty`) produk jika pelanggan membeli lebih dari 1 unit.
3. **Menerapkan Diskon Member**:
   - Jika pelanggan adalah member terdaftar, klik tombol **Pilih Member**.
   - Sistem secara otomatis menerapkan diskon toko (default 5%) pada total pembayaran.
4. **Penyelesaian Transaksi**:
   - Masukkan nominal uang yang diserahkan pelanggan pada kolom **Diterima**.
   - Periksa nilai kembalian uang yang dihitung otomatis oleh sistem di layar display besar.
   - Klik **Simpan Transaksi**.
5. **Mencetak Struk**:
   - Pilih opsi **Cetak Nota Kecil** untuk printer thermal kasir atau **Cetak Nota Besar** untuk invoice faktur berukuran PDF.

---

### 4.2. Panduan Administrator (Manajemen & Laporan)
1. **Menambah Produk Baru & Cetak Barcode**:
   - Buka menu **Produk** $\rightarrow$ klik **Tambah**.
   - Isi nama produk, kategori, harga beli (modal), harga jual retail, diskon produk, dan stok awal.
   - Centang checkbox produk yang ingin dicetak label harganya $\rightarrow$ klik **Cetak Barcode** untuk mengunduh PDF stiker barcode.
2. **Restock Barang dari Supplier (Pembelian)**:
   - Buka menu **Pembelian** $\rightarrow$ klik **Transaksi Baru**.
   - Pilih supplier yang menyuplai barang.
   - Masukkan barang yang dibeli beserta harga beli faktur.
   - Klik **Simpan Transaksi**. Stok fisik produk akan otomatis bertambah sesuai jumlah yang dibeli.
3. **Pencatatan Biaya Operasional Toko**:
   - Buka menu **Pengeluaran** $\rightarrow$ klik **Tambah**.
   - Input rincian keterangan (contoh: *Bayar Listrik & Token PLN*, *Beli Kertas Struk Thermal*) beserta nominal rupiahnya.
4. **Melihat dan Mengekspor Laporan Laba Rugi**:
   - Buka menu **Laporan**.
   - Tentukan tanggal awal dan tanggal akhir analisis.
   - Klik tombol **Export PDF** untuk mengunduh laporan pendapatan resmi yang memuat rincian omzet penjualan, pengeluaran kulakan stok, biaya operasional, dan laba bersih riil.
