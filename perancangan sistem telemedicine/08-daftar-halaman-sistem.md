# 8. Daftar Halaman Sistem (Sitemap & Navigation Flow)

Dokumen ini mendaftar seluruh halaman antarmuka web (*User Interface*) yang akan diimplementasikan oleh *programmer frontend*, lengkap dengan alur navigasi (*routing flow*) yang menghubungkan masing-masing halaman berdasarkan **Peran (Role)** pengguna.

---

## 8.1. Halaman Publik & Autentikasi (Guest)

**Daftar Halaman:**
1. **Landing Page (Beranda)**: Halaman depan publik berisi informasi platform, keunggulan layanan, dan promosi (Voucher).
2. **Login & Register**: Halaman otentikasi terpusat untuk semua *role* (Pasien, Dokter, Apotek, Admin).
3. **Lupa Password**: Halaman pemulihan akun (*recovery*).

**Alur Navigasi (Flow):**
`Landing Page` ➔ Klik "Daftar/Masuk" ➔ `Register / Login` ➔ Berhasil masuk ➔ Redirect otomatis ke `Dashboard` masing-masing *role*.

---

## 8.2. Portal Pasien

**Daftar Halaman:**
1. **Dashboard Pasien**: Halaman utama yang menampilkan ringkasan jadwal konsultasi mendatang dan status pesanan aktif.
2. **Katalog Layanan & Booking**: Halaman pemilihan paket medis dan pengaturan waktu luang (memilih jadwal dokter).
3. **Formulir Skrining (Anamnesis)**: Halaman *wizard* (*multi-step form*) untuk menjawab kuesioner medis dasar.
4. **Halaman Pembayaran (Tagihan)**: Antarmuka *Payment Gateway* dan kolom *input voucher* diskon (Berlaku untuk dua kondisi: Tagihan 1 - Konsultasi & Tagihan 2 - Obat).
5. **Ruang Konsultasi Terpadu**: Antarmuka utama yang memuat ruang *Chat* Multimedia (dengan fungsi *voice note*, gambar, video) dan *iframe* video Jitsi opsional jika ada undangan *video call*.
6. **Riwayat Medis & Resep**: Halaman *Read-Only* untuk melihat hasil diagnosis (SOAP) dan mengunduh PDF *E-Prescription*.
7. **Pelacakan Pemesanan**: Halaman status logistik pengiriman obat (*Tracking Resi*).
8. **Customer Support & Reporting**: Halaman fasilitas *Live Chat* ke Admin dan formulir pelaporan konten (Kepatuhan PSE).

**Alur Navigasi Utama (Patient Journey Flow):**
`Dashboard` ➔ Klik "Mulai Program" ➔ `Katalog Layanan & Booking` ➔ `Formulir Skrining (Anamnesis)` ➔ Lolos Skrining ➔ `Halaman Pembayaran (Tagihan 1)` ➔ Lunas ➔ Menunggu Waktu Jadwal ➔ `Ruang Konsultasi Virtual` ➔ Sesi Selesai ➔ Dialihkan ke `Halaman Pembayaran (Tagihan 2)` ➔ Lunas ➔ `Pelacakan Pemesanan`.

*(Catatan: Pasien dapat mengakses halaman `Riwayat Medis` dan `Customer Support` kapan saja dari menu navigasi utama).*

---

## 8.3. Portal Medis (Dokter)

**Daftar Halaman:**
1. **Verifikasi Legalitas (KYC)**: Halaman *upload* paksa untuk dokumen STR dan SIP bagi dokter yang baru mendaftar.
2. **Dashboard Dokter**: Halaman ringkasan konsultasi hari ini dan metrik pendapatan.
3. **Manajemen Jadwal**: Halaman kalender interaktif untuk mengatur slot ketersediaan waktu praktik.
4. **Ruang Konsultasi Terpadu**: Antarmuka *Side-by-side* yang memuat *Chat* Multimedia, opsi video *Jitsi*, data Anamnesis Pasien, dan Formulir Pengisian **SOAP** (Rekam Medis).
5. **Penerbitan Resep (E-Prescription)**: Halaman lanjutan pengisian resep obat keras yang terintegrasi Tanda Tangan Elektronik (TTE).
6. **Partner Wallet**: Halaman pendaftaran rekening bank tujuan dan pencairan (*withdrawal*) saldo.

**Alur Navigasi Utama (Doctor Workflow):**
`Register` ➔ `Halaman Verifikasi KYC` (Akun terkunci/Pending) ➔ Di-approve Admin ➔ `Dashboard Dokter` ➔ `Manajemen Jadwal` ➔ (Sistem menerima Booking) ➔ Jadwal Tiba ➔ `Ruang Konsultasi Terpadu` (Video Call & mengisi SOAP) ➔ Klik Lanjut ➔ `Penerbitan Resep` ➔ Selesai ➔ Saldo masuk ke `Partner Wallet`.

---

## 8.4. Portal Farmasi (Apotek)

**Daftar Halaman:**
1. **Verifikasi Legalitas (KYC)**: Halaman *upload* dokumen legalitas (SIA & SIPA).
2. **Dashboard Apotek**: Halaman ringkasan antrean pesanan logistik tertunda dan saldo pendapatan.
3. **Manajemen Inventaris**: Halaman tabel (*DataTables*) untuk mengatur stok obat ("Tersedia/Habis"). Harga dikunci jika diatur oleh pusat.
4. **Manajemen Pemenuhan (Fulfillment)**: Halaman daftar pesanan obat baru yang masuk beserta lampiran PDF resep.
5. **Pengemasan & Logistik**: Modal/halaman detail pesanan yang mewajibkan pengecekan *mandatory checklist* segel kemasan keras, sebelum memunculkan tombol *Request Pickup* untuk cetak resi kurir.
6. **Partner Wallet**: Halaman pengelolaan pencairan dana penjualan obat.

**Alur Navigasi Utama (Pharmacy Workflow):**
`Register` ➔ `Halaman Verifikasi KYC` ➔ Di-approve Admin ➔ `Dashboard Apotek` ➔ `Manajemen Inventaris` (Update Stok) ➔ (Sistem melempar pesanan lunas dari pasien) ➔ `Manajemen Pemenuhan (Fulfillment)` ➔ Klik Proses Pesanan ➔ `Pengemasan & Logistik` (Centang Syarat Kemasan) ➔ Klik Request Pickup ➔ Resi Tercetak ➔ Saldo masuk ke `Partner Wallet`.

---

## 8.5. Portal Manajemen (Admin & Superadmin)

**Daftar Halaman:**
1. **Global Dashboard**: Halaman pemantauan metrik finansial platform (GMV, pendapatan komisi, pertumbuhan pengguna).
2. **Approval KYC**: Halaman verifikasi dan persetujuan penolakan pendaftaran Dokter dan Apotek.
3. **SLA & Nudge Monitor**: Dasbor *real-time* untuk memantau keterlambatan tiket/jadwal dengan tombol untuk men-*trigger* kiriman peringatan WhatsApp.
4. **Manajemen Layanan & Voucher**: Halaman CRUD (*Create, Read, Update, Delete*) untuk pengaturan paket program dan manajemen kuota/diskon *voucher*.
5. **Manajemen Keuangan**: Halaman konfigurasi Bagi Hasil (*Commission Settings* persentase/nominal) dan riwayat keseluruhan transaksi pembayaran.
6. **Kepatuhan Hukum (PSE)**: 
   - Dasbor *Takedown*: Eksekusi penutupan konten hasil laporan Pasien.
   - Ekspor Audit Log: Dasbor ekstraksi *Audit Trail* sistem.

**Alur Navigasi Utama (Management Workflow):**
Tidak ada alur (*flow*) sekuensial yang mengikat. Admin & Superadmin memiliki kebebasan penuh menavigasi sistem melalui **Sidebar Utama** menuju halaman fungsional apa pun kapan saja, dengan titik mula berada di `Global Dashboard`.