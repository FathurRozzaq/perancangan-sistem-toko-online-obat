# 2. Requirements

Dokumen ini mendefinisikan persyaratan tingkat tinggi (high-level requirements) yang harus dipenuhi oleh Sistem *Digital Health Commerce* PT PRABAVA Udaya Sejahtera untuk mencapai tujuannya. Persyaratan ini dibagi menjadi *Functional Requirements* (Fungsional) dan *Non-Functional Requirements* (Non-Fungsional).

## 2.1 Functional Requirements (Persyaratan Fungsional)

Sistem harus mampu melakukan fungsi-fungsi utama berikut:

1. **Manajemen Pengguna & Akses (Identity & Access Management)**
   - Sistem harus mendukung registrasi, autentikasi, dan otorisasi pengguna. Pendaftaran akun Pelanggan sangat disarankan menggunakan atau mengaitkan Nomor WhatsApp yang aktif untuk menjamin keandalan pengiriman *Consumption Reminder* dan notifikasi transaksi.
   - Sistem harus memiliki *Role-Based Access Control* (RBAC) dengan 3 peran utama: **Superadmin** (PT PRABAVA), **Mitra Afiliasi** (terbagi atas tier Basic, Premium, Leader), dan **Pelanggan/Pasien**.

2. **Katalog Produk & Kepatuhan BPOM**
   - Sistem harus menampilkan katalog obat herbal berbasis 10 paten.
   - **Kepatuhan Klaim**: Halaman produk harus memiliki tata letak informasi yang dikendalikan oleh protokol *approval* internal. Konten tidak boleh memuat klaim yang menyesatkan (*misleading*) atau di luar izin edar BPOM terkait obat bahan alam.
   - **Lokalisasi Mata Uang**: Seluruh harga produk dan perhitungan finansial dalam sistem harus menggunakan Rupiah (IDR) sebagai mata uang dasar (*base currency*) dengan presisi 0 (nol) desimal, sehingga menampilkan angka bulat (contoh: Rp 150.000).

3. **Keranjang Belanja, Checkout, & Formulir Alamat Terstruktur**
   - Sistem harus memiliki alur pembelian yang mulus (*seamless*).
   - **Restrukturisasi Formulir Alamat**: Komponen alamat pada profil dan *checkout* **wajib** menggunakan sistem *dependent dropdowns* berjenjang (Provinsi -> Kota/Kabupaten -> Kecamatan).
   - Data wilayah pada *dropdown* ini harus dipetakan dan disinkronkan secara presisi dengan basis data wilayah (ID Area) milik penyedia logistik (Biteship). Isian teks bebas (free-text) untuk kota/kecamatan dilarang demi mencegah kegagalan API tarif.

4. **Integrasi Logistik Dinamis (Biteship)**
   - Sistem harus terintegrasi dengan REST API Biteship untuk kalkulasi ongkos kirim.
   - Saat *checkout*, sistem secara otomatis mengirimkan total berat pesanan dan ID Area ke API Biteship untuk menampilkan pilihan kurir lokal (seperti JNE, J&T, SiCepat) beserta tarif riil.
   - Sistem harus mendukung pembuatan nomor resi (*Airway Bill*/AWB) secara otomatis ketika pesanan diproses oleh Admin.

5. **Integrasi Sistem Pembayaran (Midtrans)**
   - Sistem harus memiliki modul pembayaran kustom (Package Laravel) yang terintegrasi langsung dengan API Midtrans (Snap atau Core API), menggantikan opsi bawaan Stripe/PayPal.
   - Sistem harus menyediakan saluran pembayaran lokal seperti Virtual Account (BCA, Mandiri, BRI, BNI), E-Wallet (GoPay, OVO), dan QRIS.
   - Sistem wajib menyediakan rute *Webhook* untuk mendengarkan dan memvalidasi pembaruan status transaksi secara *real-time* dari peladen Midtrans ke basis data pesanan.

6. **Notifikasi Transaksional via WhatsApp Gateway**
   - Sistem tidak lagi mengandalkan protokol SMTP email sebagai jalur notifikasi utama.
   - Sistem harus mengembangkan modul pengamat (*Observer*) yang mendengarkan pemicu peristiwa transaksi (contoh: `checkout.order.save.after`).
   - Sistem wajib terintegrasi dengan API *WhatsApp Gateway* untuk mengirimkan pesan otomatis berupa ringkasan pesanan, instruksi pembayaran, dan nomor resi pengiriman secara instan ke perangkat pelanggan.

7. **Sistem Kecerdasan Buatan (AI) & Personalisasi**
   - **Consumption Reminder**: Sistem harus mengirimkan notifikasi pengingat otomatis (melalui WhatsApp) kepada pelanggan tentang waktu konsumsi produk untuk meningkatkan efektivitas pengobatan dan kepatuhan.
   - **AI Recommendation & Predictive Ordering**: Algoritma sistem harus mampu menyarankan produk tambahan atau memprediksi jadwal *repurchase* pelanggan berdasarkan riwayat belanja dan sisa masa konsumsi.
   - **Welcome Sequence & Triage**: Sistem harus menyediakan edukasi otomatis (*sequence*) atau *AI Chatbot* sederhana untuk menyambut pelanggan baru dan mengarahkan mereka pada solusi produk yang tepat.

8. **Sistem Manajemen Afiliasi Berjenjang (Affiliate System)**
   - **Generator Link**: Sistem harus dapat memproduksi tautan referral (*unique link*) atau kode promo khusus bagi Mitra Afiliasi.
   - **Tracking System**: Sistem harus mencatat (*anti-fraud*) setiap pelanggan yang mendaftar atau bertransaksi melalui tautan atribusi afiliasi dengan akurat.
   - **Sistem Komisi (Tiering)**: Sistem harus memiliki logika untuk menghitung komisi secara berbeda berdasarkan level (Basic, Premium, Leader), di mana level Leader dapat memperoleh komisi tambahan dari jaringan timnya.
   - **Manajemen Payout**: Sistem harus menyediakan dasbor untuk merekapitulasi pendapatan dan memfasilitasi proses penarikan saldo komisi afiliasi.

9. **Manajemen Keuangan & Dasbor Bagi Hasil (Profit Sharing)**
   - Sistem harus memiliki sistem *backend* yang bekerja menghitung laba bersih operasional secara otomatis (mengurangi omzet penjualan kotor dengan HPP, biaya maklon, biaya logistik, potongan *payment gateway*, dan komisi afiliasi).
   - Sistem harus menyediakan dasbor transparansi bagi hasil (*Profit Sharing Dashboard*) yang memvisualisasikan perhitungan royalti paten dan margin keuntungan kepada pemangku kepentingan (PT PRABAVA, pemilik paten, dana pengembangan) secara *real-time*.

10. **Customer Relationship Management (CRM) Terpusat**
    - Sistem harus mengelola secara eksklusif data lapis pertama (*first-party data*) pelanggan.
    - Sistem harus mendukung fitur segmentasi cerdas (misal: "Pelanggan Aktif", "Pembeli Produk X") untuk merancang kampanye pemasaran yang lebih tepat sasaran.

---

## 2.2 Non-Functional Requirements (Persyaratan Non-Fungsional)

Sistem harus memenuhi standar teknis, kinerja, dan keamanan sebagai berikut:

1. **Keamanan (Security) dan Privasi Data**
   - Seluruh infrastruktur basis data dan transmisi data (*in-transit* menggunakan TLS/SSL) wajib dijaga kerahasiaannya.
   - Karena menyimpan profil kesehatan dasar (triage/konsultasi awal) dan riwayat transaksi yang sensitif, sistem harus patuh pada prinsip pelindungan data pribadi (PDP).

2. **Skalabilitas dan Arsitektur Pengembangan (Upgrade-Safe)**
   - Sistem **DIWAJIBKAN** menggunakan **Bagisto** (berbasis Laravel + Vue.js) sebagai fondasi dasar e-commerce.
   - Seluruh fungsionalitas kustom yang menjadi nilai jual utama (Modul Afiliasi, Dasbor Profit Sharing, Midtrans, Biteship, dan Modul AI Reminder) harus dibangun secara terisolasi sebagai **Package Laravel / Modul Kustom** yang dipasangkan ke dalam Bagisto.
   - Kode inti (*core code*) Bagisto sangat dilarang untuk diubah secara langsung (*hard-coded*), guna memastikan platform tidak rusak saat Bagisto menerima pembaruan versi resmi dari pengembang utamanya.

3. **Kinerja (Performance) dan Keandalan**
   - Proses beban berat (pengiriman notifikasi WhatsApp massal, analisis data oleh AI, dan pencatatan komisi multi-level afiliasi) tidak boleh memblokir respons antarmuka pelanggan (*blocking*). Proses ini harus didistribusikan melalui sistem antrean (*background jobs* / Message Broker / Redis).

4. **Pemeliharaan (Maintainability)**
   - Pembuatan paket kustom harus menerapkan standar *clean code* Laravel (pola arsitektur MVC atau Service/Repository Pattern) serta memuat *API Documentation* (REST/GraphQL) untuk kemudahan pelacakan (*debugging*) oleh tim pengembang lain di masa mendatang.
