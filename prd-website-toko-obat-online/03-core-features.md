# 3. Core Features (MVP)

Bagian ini merangkum fitur-fitur utama yang wajib dikembangkan pada iterasi pertama (Minimum Viable Product / MVP) untuk memastikan peluncuran *Digital Health Commerce* PT PRABAVA Udaya Sejahtera berjalan sukses. 

Untuk menjaga arsitektur agar tetap skalabel dan aman (*upgrade-safe*), implementasi fitur dibagi menjadi dua demarkasi utama: **Fitur Sistem Bawaan Bagisto** (fungsionalitas *out-of-the-box* yang hanya butuh konfigurasi) dan **Modul Kustom Baru** (fitur spesifik bisnis yang dibangun sebagai ekstensi/*Package* terpisah).

---

## A. Fitur Sistem Bawaan Bagisto (Core Framework)

Fitur-fitur ini akan memaksimalkan kapabilitas bawaan (*native*) dari kerangka kerja Bagisto dengan penyesuaian (konfigurasi) minimal.

### 1. Manajemen Katalog & Produk (Product Management)
- **Tipe Produk Beragam**: Memanfaatkan tipe *Simple Product* untuk obat herbal reguler dan *Bundle Product* untuk pembuatan paket hemat pengobatan (misal: "Paket Terapi 1 Bulan").
- **Kategori dan Atribut**: Menyusun hierarki kategori kesehatan dan memanfaatkan sistem atribut (*Custom Attributes*) untuk menambahkan informasi khusus seperti "Nomor Izin Edar BPOM", "Dosis Anjuran", dan "Komposisi" secara terstruktur.

### 2. Manajemen Pesanan & Keranjang (Order & Cart Management)
- **Keranjang Belanja (*Shopping Cart*)**: Fungsi bawaan untuk menampung produk, menghitung subtotal, dan menerapkan aturan pajak (jika ada).
- **Alur Checkout Dasar**: Pemanfaatan kerangka antarmuka *checkout* standar yang nantinya akan dicegat (*intercept*) oleh modul Midtrans dan Biteship.
- **Manajemen Pesanan (*Order Management*)**: Dasbor Admin bawaan untuk melacak status pesanan (*Pending, Processing, Completed, Canceled*), pembuatan *invoice*, dan manajemen pengiriman (*shipment*).

### 3. Manajemen Pelanggan (Customer Management)
- **Registrasi dan Profil**: Pendaftaran akun pengguna dasar dan pengelolaan profil pelanggan.
- **Grup Pelanggan (Customer Groups)**: Pengelompokan basis pelanggan (*General, VIP*, dsb) yang dapat digunakan untuk segmentasi pemasaran dasar.

### 4. Konfigurasi Lokalisasi (Mata Uang & Bahasa)
- **Basis Mata Uang (IDR)**: Penyesuaian pengaturan mata uang sistem untuk menggunakan Rupiah (IDR) secara mutlak tanpa penggunaan desimal (presisi "0").
- **Lokalisasi Antarmuka**: Penerjemahan *file language* bawaan sistem (contoh: mengubah "Cart" menjadi "Keranjang", "Checkout" menjadi "Pembayaran") ke dalam bahasa Indonesia baku.

### 5. Sistem Promosi Dasar
- **Catalog Rules & Cart Rules**: Pemanfaatan *rule engine* bawaan untuk mengatur diskon harga otomatis pada periode tertentu atau memberikan potongan harga jika pelanggan memenuhi syarat jumlah belanja tertentu di keranjang.

---

## B. Modul Kustom Baru (Package Development)

Fitur-fitur di bawah ini **TIDAK TERSEDIA** secara bawaan dan harus dikembangkan secara khusus (*custom development*) menggunakan pola *Package Development* Laravel yang di-*inject* ke dalam sistem Bagisto.

### 1. Modul Integrasi Sistem Pembayaran (Midtrans Package)
- **Pencegatan Alur Bawaan**: Modul ini menonaktifkan pembayaran bawaan (Stripe/PayPal) dan mengarahkan antarmuka *checkout* ke antarmuka Midtrans (Snap atau Core API).
- **Opsi Pembayaran Lokal**: Menyediakan metode pembayaran Virtual Account (BCA, Mandiri, BNI, BRI), E-Wallet (GoPay, OVO), dan QRIS.
- **Validasi Otomatis (Webhook)**: Pembuatan *endpoint/route* khusus untuk menerima pembaruan (*callback*) dari peladen Midtrans yang secara instan merubah status tagihan menjadi *Paid* (Lunas) tanpa konfirmasi manual dari admin.

### 2. Modul Sistem Logistik Dinamis (Biteship Package)
- **Restrukturisasi Formulir Alamat (Dependent Dropdowns)**: Menggantikan *input* teks bebas pada formulir profil dan *checkout* dengan sistem *dropdown* bersarang (Provinsi -> Kota/Kabupaten -> Kecamatan). Data wilayah ini disinkronkan presisi dengan basis data "Area ID" milik Biteship.
- **Kalkulasi Ongkos Kirim (*Live Rate*)**: Modul pengiriman (*Shipping Method*) yang mengekstrak data ID Wilayah pelanggan dan total berat keranjang, lalu mengirimkannya ke REST API Biteship untuk memunculkan daftar kurir lokal (JNE, J&T, SiCepat) beserta ongkos kirim aktual secara *real-time*.
- **Otomatisasi Resi (Auto-AWB)**: Integrasi penciptaan nomor resi (Airway Bill) otomatis langsung dari dasbor *Shipment* Bagisto saat Admin memproses pemenuhan pesanan.

### 3. Modul Notifikasi Transaksional (WhatsApp Gateway)
- **Observer Berbasis Peristiwa (*Event*)**: Pembuatan *listener/observer* kustom pada kerangka kerja Bagisto (mengikat pada *event* seperti `checkout.order.save.after` atau status pembaruan *shipment*).
- **Penggantian SMTP Eksternal**: Integrasi dengan API WhatsApp pihak ketiga untuk mengirimkan pesan otomatis (Instruksi Pembayaran, Bukti Pesanan, Nomor Resi, dan Notifikasi Batal) langsung ke nomor WhatsApp Pelanggan, menggantikan kebergantungan pada *email*.

### 4. Modul Sistem Afiliasi Berjenjang (Affiliate System)
- **Generator Referral (Unique Link)**: Fungsionalitas pembuatan kode atau tautan URL khusus secara otomatis untuk setiap pengguna yang mendaftar sebagai Mitra Afiliasi.
- **Pelacakan Atribusi Presisi (Anti-Fraud)**: Mekanisme penyimpanan *cookies* atau *session* khusus yang mencatat dari afiliasi mana seorang pembeli berasal, untuk memastikan komisi dikreditkan pada pihak yang benar.
- **Sistem Komisi Berjenjang (Tiering)**: Algoritma yang menghitung besaran persentase komisi secara dinamis bergantung pada status (*tier*) Mitra Afiliasi (contoh: Basic 10%, Premium 15%, Leader 15% + 5% Bonus Jaringan Tim).
- **Manajemen Payout**: Dasbor terpisah bagi Mitra Afiliasi untuk melihat saldo komisi, memonitor total klik, konversi transaksi, serta tombol untuk mengajukan penarikan dana (*withdrawal*).

### 5. Modul Keuangan & Bagi Hasil (Profit Sharing Dashboard)
- **Kalkulator Laba Bersih Tertanam**: Mesin hitung yang bekerja di latar belakang pada setiap transaksi sukses, yang berfungsi mengurangi Nilai Penjualan Kotor dengan parameter tetap (HPP, Biaya Maklon, Ongkos Kirim, Biaya Payment Gateway, dan Potongan Komisi Afiliasi).
- **Dasbor Transparansi *Stakeholder***: Antarmuka *backend* khusus yang menampilkan laporan *real-time* persentase pembagian royalti kepada pemegang paten (Prof. Taruna), pengembang formulasi (Fahrul Nurkolis), dan PT PRABAVA Udaya Sejahtera secara rinci untuk pertanggungjawaban internal.

### 6. Modul Kecerdasan Buatan (AI) & Personalisasi Pelanggan
- **Sistem Pengingat Pintar (*Consumption Reminder*)**: Fitur unggulan yang secara otomatis menghitung masa habis konsumsi berdasarkan isi pesanan pelanggan, lalu mengirimkan pengingat interaktif melalui WhatsApp Gateway (contoh: "Halo, ini waktunya mengonsumsi produk X Anda").
- **Automasi Perjalanan (*Journey Automation*) & Predictive Ordering**: Integrasi algoritma berbasis kecerdasan buatan (*Large Language Models/LLM* pihak ketiga) yang melacak siklus belanja pelanggan untuk memprediksi kapan mereka harus membeli obat kembali (*repurchase*), dan secara otomatis mengirimkan tawaran pemesanan ulang ke nomor pelanggan.
- **Kepatuhan Klaim Konten (Approval Flow)**: Modifikasi pada sistem manajemen konten (*CMS*) Bagisto untuk menambahkan alur persetujuan (*approval flow*). Produk dan konten edukasi herbal tidak akan bisa diterbitkan sebelum disetujui oleh otoritas medis/regulator internal untuk mencegah pelanggaran klaim BPOM.
