# Analisis Platform Bagisto sebagai Dasar Sistem E-Commerce PT PRABAVA

Berdasarkan penelusuran terhadap repositori GitHub ([bagisto/bagisto](https://github.com/bagisto/bagisto)) dan dokumentasi resmi Bagisto ([docs.bagisto.com](https://docs.bagisto.com/)), berikut adalah analisis fitur-fitur yang disediakan oleh Bagisto dan kesesuaiannya dengan kebutuhan bisnis PT PRABAVA Udaya Sejahtera.

## 1. Tinjauan Umum Bagisto
Bagisto adalah framework e-commerce *open-source* (gratis di bawah lisensi MIT) yang dibangun menggunakan teknologi modern:
*   **Backend:** Laravel (PHP Framework)
*   **Frontend:** Vue.js (JavaScript Framework) / Tailwind CSS
*   Bagisto didesain untuk skalabilitas tinggi, memiliki dokumentasi pengembang (*DevDocs*) yang komprehensif, dan mendukung konsep *Headless Commerce* (misalnya menggunakan Next.js).

## 2. Fitur Bawaan Bagisto (Core Features)
Bagisto sudah menyediakan fitur dasar toko online yang sangat mapan *out-of-the-box*, meliputi:
*   **Manajemen Katalog & Produk:** Mendukung pembuatan berbagai jenis produk (*Simple, Configurable, Grouped, Bundle, Virtual, Downloadable*), manajemen kategori berlapis, dan atribut khusus.
*   **Keranjang Belanja & Checkout:** Alur pembelian (*checkout flow*) standar e-commerce yang mulus (*seamless*).
*   **Manajemen Pesanan (Order Management):** Pelacakan status pesanan, pembuatan *invoice*, dan manajemen pengiriman (*shipment*).
*   **Manajemen Pelanggan (Customer Management):** Grup pelanggan, registrasi, profil pengguna, dan riwayat pesanan.
*   **Lokalisasi & Multi-Currency:** Dukungan banyak bahasa dan mata uang secara native.
*   **Promosi & Diskon:** Sistem *rule engine* untuk membuat aturan harga katalog (*catalog rules*) dan aturan keranjang (*cart rules*).

## 3. Kesesuaian dengan Kebutuhan Spesifik PT PRABAVA
Berikut adalah analisis *gap* (kesenjangan) antara fitur bawaan Bagisto dengan dokumen `analisis_kebutuhan_website.md` PT PRABAVA:

### A. Sistem E-Commerce Terintegrasi & Penyesuaian Pasar Indonesia
*   **Status Bagisto:** **Memerlukan Penyesuaian Modul (Custom Packages).**
*   **Analisis:** Meskipun kebutuhan dasar (katalog, keranjang) telah didukung, terdapat lima rincian penyesuaian teknis yang **wajib** diimplementasikan pada kerangka kerja Bagisto agar sistem beroperasi optimal di Indonesia:
    1. **Integrasi Sistem Pembayaran (Midtrans):** Secara bawaan Bagisto menggunakan Stripe/PayPal. Diwajibkan mengembangkan modul pembayaran kustom (Package Laravel) yang terintegrasi dengan API Midtrans (Snap/Core API) untuk mencegat alur checkout dan menyediakan opsi Virtual Account, E-Wallet (GoPay, OVO), dan QRIS, lengkap dengan rute Webhook.
    2. **Integrasi Sistem Logistik (Biteship):** Diperlukan modul *Shipping Method* khusus yang berkomunikasi dengan REST API Biteship. Saat checkout, sistem mengirim berat keranjang dan ID wilayah ke Biteship untuk menampilkan tarif aktual kurir lokal (JNE, J&T, SiCepat). Disarankan mencakup fitur pembuatan resi otomatis (AWB).
    3. **Restrukturisasi Formulir Alamat Pengiriman:** Mengubah isian teks bebas bawaan menjadi sistem *dependent dropdowns* berjenjang (Provinsi -> Kota/Kabupaten -> Kecamatan) yang dipetakan dengan basis data wilayah (ID area) dari Biteship agar presisi pengiriman terjamin.
    4. **Konfigurasi Mata Uang & Lokalisasi:** Menetapkan Rupiah Indonesia (IDR) sebagai *base currency* tanpa desimal (0 desimal) untuk menampilkan harga bulat (contoh: "Rp 150.000"). Menerjemahkan bahasa *frontend* (Cart, Checkout) menjadi bahasa Indonesia yang baku.
    5. **Penyesuaian Notifikasi (WhatsApp):** Mengganti ketergantungan pada email (SMTP) dengan mengembangkan modul *Observer* pada *event* transaksi (`checkout.order.save.after`) untuk mengirim notifikasi WhatsApp otomatis berisi ringkasan pesanan, instruksi pembayaran, dan resi.

### B. Modul Kecerdasan Buatan (AI) & Personalisasi
*   **Status Bagisto:** **Tersedia Parsial (Didukung via Integrasi).**
*   **Analisis:** Bagisto memiliki panduan dan kapabilitas untuk diintegrasikan dengan *Large Language Models* (LLM) seperti ChatGPT, Gemini, LLaMA, dll. Integrasi ini bisa mencakup AI Chatbot, pembuatan deskripsi produk otomatis, dan mesin rekomendasi. Namun, fitur sangat spesifik seperti **Sistem Pengingat (Consumption Reminder)** dan urutan edukasi otomatis (*Welcome Sequence*) perlu dikembangkan secara khusus (*custom development*) di atas layanan Laravel (*Jobs/Commands*).

### C. Manajemen Afiliasi Berjenjang (Affiliate System)
*   **Status Bagisto:** **Tidak Tersedia Secara Native (Membutuhkan Ekstensi/Kustom).**
*   **Analisis:** Bagisto versi dasar tidak menyertakan modul afiliasi. Fitur pembuatan *link referral* unik, pelacakan atribusi penjualan, dan struktur komisi berjenjang (*Basic, Premium, Leader*) harus dibangun sebagai paket ekstensi (*custom module*) yang di-inject ke dalam sistem Bagisto.

### D. Manajemen Kepatuhan (Compliance) BPOM
*   **Status Bagisto:** **Memerlukan Penyesuaian Alur (Custom Workflow).**
*   **Analisis:** Bagisto memiliki *Access Control List* (ACL) untuk membatasi peran Admin. Namun, untuk memastikan protokol persetujuan (*approval flow*) sebelum produk atau konten tayang secara publik demi menghindari pelanggaran klaim khasiat obat herbal BPOM, diperlukan modifikasi kecil pada modul manajemen produk dan *Content Management System* (CMS) Bagisto.

### E. Manajemen Keuangan & Pembagian Laba Bersih (Profit Sharing)
*   **Status Bagisto:** **Tidak Tersedia (Harus Custom Build).**
*   **Analisis:** Sistem kalkulator laba bersih operasional (memotong biaya maklon, HPP, payment gateway, pajak), perhitungan royalti paten, dan dasbor transparansi pembagian laba untuk pemangku kepentingan (PT PRABAVA, Fahrul Nurkolis, Prof. Taruna) **tidak didukung** secara bawaan oleh Bagisto. Fitur finansial ini merupakan *business logic* yang unik milik PT PRABAVA dan wajib dibangun (*hard-coded*) sebagai modul dasbor keuangan independen di *backend* Bagisto.

## 4. Kapabilitas Pengembang (Developer Architecture)
Berdasarkan panduan *DevDocs* (`devdocs.bagisto.com`), platform ini didesain sangat modular dan ramah pengembang (*developer-friendly*):
*   **Package Development:** Bagisto memungkinkan kita untuk membangun fungsionalitas baru (seperti modul *Profit Sharing* dan *Affiliate*) dalam bentuk *Package* independen. Artinya, kode inti (core) Bagisto tidak perlu disentuh, sehingga sistem tetap aman saat ada *update* versi.
*   **REST & GraphQL API:** Tersedia secara bawaan. Ini sangat krusial jika di masa depan PT PRABAVA ingin membuat aplikasi *mobile* terpisah atau menggunakan frontend *Headless*.
*   **Custom Product Types & Payment Methods:** Kita bisa mengembangkan tipe produk khusus (misalnya "Produk Paten Herbal" yang mewajibkan input ID BPOM) dan integrasi *payment gateway* lokal secara mandiri.

## 5. Kesimpulan & Rekomendasi
Menggunakan **Bagisto** sebagai *core engine* (fondasi) untuk sistem *Digital Health Commerce* PT PRABAVA adalah **pilihan arsitektural yang sangat ideal**. Mengingat Bagisto berbasis Laravel, tim pengembang memiliki kebebasan penuh untuk melakukan kustomisasi kode tanpa batas. Penggunaan Bagisto akan memangkas waktu pengembangan fitur e-commerce dasar hingga 70%.

**Rekomendasi Revisi untuk Dokumen PRD (`issue.md`):**
Dokumen PRD harus mencerminkan bahwa kita *tidak* membangun sistem dari nol (*from scratch*), melainkan mengkustomisasi Bagisto. Hal ini akan mengubah paradigma penulisan PRD:
1.  **Pada `05-architecture.md`:** Tegaskan bahwa fondasi sistem menggunakan Bagisto (Laravel + Vue.js) dengan pendekatan *Package Development* untuk fitur kustom.
2.  **Pada `03-core-features.md`:** Buat demarkasi (pemisahan) yang jelas antara fitur yang menggunakan "Sistem Bawaan Bagisto" (seperti *Checkout, Order Management*) dan fitur yang merupakan "Modul Kustom Baru" (Sistem Afiliasi Berjenjang, Dasbor Bagi Hasil, Modul Pengingat AI).
3.  **Pola Pengembangan:** Instruksikan agar fitur-fitur baru (seperti Profit Sharing dan Afiliasi) didesain sebagai arsitektur *Modular/Package* di dalam Bagisto agar inti sistem tetap bersih (*upgrade-safe*).
