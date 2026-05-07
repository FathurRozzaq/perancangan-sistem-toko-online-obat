# Analisis Kebutuhan Website: PT PRABAVA Udaya Sejahtera

Berdasarkan dokumen model bisnis "Pengembangan Bisnis Obat Herbal Berbasis 10 Paten", berikut adalah analisis komprehensif mengenai kebutuhan fungsional dan sistem dari website AI yang akan dikembangkan.

## 1. Arsitektur & Konsep Utama Platform
*   **Tipe Platform:** *Digital Health Commerce* eksklusif (Direct-to-Consumer / D2C).
*   **Pendekatan Distribusi:** Menjadi satu-satunya saluran penjualan resmi untuk menjaga eksklusivitas merek, menghindari perang harga, dan mengontrol penuh narasi produk. Platform ini memutus ketergantungan pada *marketplace* eksternal.
*   **Fungsi Inti:** Edukasi kesehatan terukur, transaksi langsung, personalisasi berbasis AI, CRM terintegrasi, dan pengelolaan jaringan afiliasi.

## 2. Manajemen Pengguna & Hak Akses (Role Management)
Website membutuhkan pembagian peran pengguna yang jelas:
*   **Pelanggan/Pasien (Customer):** Memiliki akses untuk membaca edukasi produk, melakukan konsultasi awal berbasis AI, mendapatkan rekomendasi, melakukan pembelian, melacak pesanan, dan mengelola profil kesehatan dasar.
*   **Mitra Afiliasi (Affiliate):** Memiliki dasbor khusus untuk mengambil tautan/kode referral unik, memantau metrik performa (klik, konversi), melihat komisi yang didapat, dan mengelola jaringan tim (khusus level *Leader*).
*   **Administrator (PT PRABAVA):** Memiliki kontrol penuh (Super Admin) atas manajemen inventaris produk, moderasi konten edukasi (kepatuhan BPOM), manajemen pesanan, pencairan komisi afiliasi, dan analitik data terpusat.

## 3. Rincian Fitur Utama

### A. Sistem E-Commerce Terintegrasi
*   **Katalog Produk Terstandarisasi:** Halaman produk yang informatif namun dibatasi secara ketat oleh regulasi klaim BPOM (objektif dan tidak menyesatkan).
*   **Keranjang Belanja & Checkout:** Alur pembelian yang mudah (*seamless*).
*   **Integrasi Payment Gateway:** Pembayaran otomatis melalui Transfer Bank (Virtual Account), E-Wallet, atau Kartu Kredit.
*   **Integrasi Logistik/Kurir:** Penghitungan ongkos kirim otomatis berdasarkan alamat dan pelacakan resi pengiriman secara *real-time*.
*   **Sistem Bundling:** Fitur untuk mengelompokkan beberapa varian produk menjadi paket dengan harga khusus.

### B. Modul Kecerdasan Buatan (AI) & Personalisasi
*   **Sistem Rekomendasi Pintar (AI Recommendation):** Memberikan saran produk berdasarkan input kebutuhan umum atau riwayat pelanggan.
*   **Automasi Perjalanan Pelanggan (Customer Journey Automation):**
    *   *Welcome sequence* untuk menyambut pelanggan baru.
    *   Edukasi berkala yang dikirimkan secara otomatis.
*   **Sistem Pengingat (Consumption Reminder):** Fitur unggulan berupa notifikasi otomatis kepada pelanggan untuk mengingatkan waktu konsumsi produk. Hal ini meningkatkan kepatuhan pasien dan memicu pembelian ulang (*retention/repurchase*).
*   **Analitik Perilaku Pembelian:** Algoritma yang mempelajari kebiasaan belanja pengguna untuk memprediksi kapan mereka butuh membeli lagi (*predictive ordering*).
*   **Konsultasi Awal (AI Chatbot/Triage):** Asisten virtual untuk menjawab pertanyaan dasar (*customer service*) dan mengarahkan ke produk yang tepat.

### C. Manajemen Afiliasi Berjenjang (Affiliate System)
*   **Generator Referral:** Sistem yang secara otomatis membuat *unique link* atau kode promo untuk setiap mitra afiliasi yang mendaftar.
*   **Pelacakan Atribusi (Tracking System):** Mencatat secara akurat pelanggan mana yang mendaftar dan membeli melalui tautan afiliasi tertentu (menjaga agar komisi tidak salah sasaran).
*   **Sistem Komisi Berjenjang (Tiering):**
    *   *Affiliate Basic:* Komisi dasar (misal 10% dari omzet penjualan).
    *   *Affiliate Premium:* Peningkatan komisi (misal 15%) setelah mencapai target penjualan tertentu.
    *   *Affiliate Leader:* Komisi pribadi (15%) ditambah bonus tim/jaringan (3-5%).
*   **Manajemen Payout (Pencairan):** Sistem untuk merekapitulasi total pendapatan afiliasi dan memfasilitasi proses transfer komisi.

### D. Kepatuhan (Compliance) & Manajemen Konten
*   **CMS Terstruktur:** Sistem manajemen konten untuk membangun *Landing Page* dinamis dan menerbitkan artikel edukasi.
*   **Protokol Approval Konten:** Alur persetujuan internal yang ketat sebelum konten atau detail produk tayang di website, demi mencegah klaim yang berlebihan atau melanggar aturan BPOM terkait obat bahan alam.

### E. Customer Relationship Management (CRM) & Database
*   **Kepemilikan Data Lapis Pertama (First-Party Data):** Database terpusat yang menyimpan nama, kontak, demografi, dan riwayat kesehatan/pembelian pengguna.
*   **Segmentasi:** Mengelompokkan pelanggan (misal: "Pembeli Produk A", "Pelanggan Aktif", "Pelanggan Pasif") untuk target *email marketing* atau promosi.

### F. Manajemen Keuangan & Pembagian Laba Bersih (Profit Sharing)
Sistem *backend* khusus yang secara transparan mencatat dan menghitung alokasi keuntungan:
*   **Kalkulator Laba Bersih Operasional:** Menghitung otomatis pendapatan dengan mengurangi omzet penjualan terhadap biaya HPP, maklon, operasional, logistik, *payment gateway*, dan komisi afiliasi.
*   **Dasbor Transparansi Bagi Hasil:** Menampilkan persentase pembagian laba bersih secara *real-time* kepada *stakeholder* (PT PRABAVA, Penyedia Paten, Konsultan Produksi, Dana Pengembangan, dan Cadangan Risiko).
*   **Laporan Keuangan & Riwayat Pencairan:** Laporan berkala terkait riwayat pencairan dana ke masing-masing pihak untuk menjaga akuntabilitas kerja sama.

## 4. Persyaratan Pengembangan Sesuai Roadmap Bisnis

*   **Persiapan (Pre-Launch / Tahap 3):**
    *   Website belum untuk berjualan.
    *   Fokus pada pembuatan *Landing Page* edukasi, pengumpulan prospek/database (*waitlist*), dan pendaftaran sistem afiliasi.
    *   Klaim produk masih sangat dibatasi hingga Izin Edar BPOM keluar.
*   **Peluncuran (Launch / Tahap 4):**
    *   Aktivasi modul keranjang belanja, *payment gateway*, dan logistik untuk 1-2 produk unggulan perdana.
    *   AI mulai diaktifkan penuh untuk rekomendasi dan *reminder*.
    *   Penghitungan komisi afiliasi berjalan secara aktual.
*   **Ekspansi (Tahap 5):**
    *   Penyempurnaan model AI menggunakan big data yang telah terkumpul.
    *   Pengembangan komunitas pelanggan (forum/ulasan tertutup) di dalam platform.

## Kesimpulan

Berdasarkan model bisnis yang dirancang, website PT PRABAVA ini bukanlah sekadar "toko online" standar. Website ini merupakan **Sistem Mesin Pemasaran Berbasis Data**. 

Tantangan terbesar secara teknis dalam pembuatannya adalah:
1. Membangun sistem afiliasi (*referral & tracking*) yang presisi dan *anti-fraud*.
2. Membangun model AI yang benar-benar bisa memetakan dan merespons interaksi pelanggan secara natural (*journey automation*).
3. Mengunci keamanan informasi agar sistem promosi dan narasi produk sama sekali tidak menyalahi aturan ketat BPOM mengenai obat tradisional/bahan alam.
