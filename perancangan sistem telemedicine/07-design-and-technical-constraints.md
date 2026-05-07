# 7. Design and Technical Constraints

Dokumen ini mendefinisikan batasan teknis, arsitektur dasar, dan asumsi yang wajib dipatuhi oleh tim pengembang dalam membangun sistem Telemedicine.

## 7.1. System Goals (Tujuan Sistem)
Tujuan utama dari arsitektur teknis ini adalah membangun platform telemedisin yang:
1. **Reaktif dan Cepat (High-Performance)**: Mampu menangani komunikasi antarmuka secara instan layaknya *Single Page Application* tanpa perlu mengembangkan API *frontend-backend* terpisah.
2. **Kepatuhan dan Keamanan Tingkat Tinggi**: Menjamin keselamatan privasi data pasien dan rekam medis elektronik sesuai regulasi hukum PSE dan Kemenkes.
3. **Modular dan Terukur (Scalable)**: Memungkinkan pengembangan tahap awal secara cepat dengan pendekatan monolitik, namun cukup terstruktur untuk dipecah menjadi layanan mikro (*microservices*) di kemudian hari.
4. **Resiliensi Operasional (Reliability)**: Memastikan proses berat (seperti pembuatan dokumen atau integrasi pihak ketiga) tidak membebani interaksi utama pengguna.

## 7.2. Design dan Batasan Teknis yang Harus Diikuti
Pengembangan wajib mematuhi parameter arsitektur berikut:

- **Pendekatan Arsitektur Utama**: **Modular Monolithic**. Basis kode (*codebase*) tunggal yang membagi fitur menjadi modul-modul independen (Modul KYC, Modul Konsultasi, Modul Transaksi, dll).
- **Framework Utama**: **Laravel** (PHP).
- **Frontend Stack**: **Livewire** (dipadukan dengan Alpine.js dan TailwindCSS jika diperlukan) untuk menghasilkan reaktivitas *server-side*.
- **Relational Database**: Penggunaan **MySQL** atau **PostgreSQL** diwajibkan untuk menjamin kepatuhan ACID (*Atomicity, Consistency, Isolation, Durability*) yang krusial bagi transaksi keuangan (*Double Billing*) dan Rekam Medis (RME).
- **Design Pattern**: Wajib mengimplementasikan **Repository Pattern** atau **Service Pattern**. Logika bisnis tidak boleh diletakkan secara langsung di dalam *Controller*. Tujuannya agar kode mudah dites (*Unit Testing*) dan sangat mudah dirawat (*maintainable*).
- **Asynchronous Processing**: Sistem **wajib menggunakan Message Broker/Queue (seperti Redis)** untuk menangani beban tugas latar belakang (*background jobs*), mencakup:
  - Pembuatan dokumen PDF (E-Prescription).
  - Pengiriman Email dan WhatsApp Notifikasi.
  - Komunikasi Webhook dengan Payment Gateway dan API Logistik.
- **Penyimpanan Objek (Object Storage)**: Penyimpanan berkas statis (seperti foto medis pasien, PDF resep dokter, dokumen STR/SIA Apotek) **wajib ditempatkan di luar server aplikasi utama**, menggunakan layanan *Object Storage* eksternal (seperti AWS S3 atau Google Cloud Storage) untuk efisiensi dan keamanan peladen.

## 7.3. Assumptions and Dependencies (Asumsi dan Ketergantungan)
Sistem memiliki ketergantungan pada *Third-Party API* (Layanan Pihak Ketiga) guna menggeser beban infrastruktur yang berat:

- **Ketergantungan Video Streaming**: Sistem berasumsi bahwa beban panggilan video tidak akan ditanggung oleh peladen lokal. Sistem bergantung pada integrasi API **Jitsi** (via Iframe) untuk menyediakan ruang tatap muka virtual (*video conference*).
- **Ketergantungan Payment Gateway**: Validasi pembayaran (Tagihan Konsultasi & Tagihan Obat), hingga proses pencairan dana (*Payout/Disbursement*) diasumsikan sepenuhnya diurus oleh Payment Gateway modern (seperti **Midtrans** atau **Xendit**). Peladen internal hanya memantau status akhir via komunikasi *Webhook*.
- **Ketergantungan Logistik**: Penghitungan ongkos kirim dan pencetakan nomor resi secara otomatis bergantung penuh pada ketersediaan Agregator Logistik pihak ketiga (seperti **Biteship** atau **RajaOngkir**).
- **Ketergantungan Pesan Instan**: Pengingat SLA dan fitur *Nudge* internal berasumsi pada tersedianya layanan integrasi **WhatsApp Gateway**.

### 7.3.1. Standar Keamanan & Perlindungan Data Elektronik
Sesuai dengan pedoman PSE Lingkup Privat, aplikasi ini diwajibkan secara hukum untuk menjaga dan melindungi integritas, ketersediaan, dan kerahasiaan Data Elektronik serta Data Pribadi. Kondisi ini mewajibkan:
1. **Confidentiality (Kerahasiaan) At-Rest**: Seluruh data privasi (khususnya riwayat Anamnesis, Catatan Medis SOAP, dan data Profil) wajib dienkripsi saat tersimpan di dalam *database* menggunakan algoritma standar militer **AES-256**.
2. **Confidentiality In-Transit**: Seluruh komunikasi lalu lintas data antara klien dan peladen wajib melalui jalur terenkripsi **TLS/SSL**.
3. **Proteksi Ketersediaan (Availability)**: Peladen web publik wajib mengimplementasikan lapisan perlindungan **Rate Limiting** untuk mencegah gangguan *Brute Force* atau serangan DDoS tingkat dasar.
4. **Content Security Policy (CSP)**: *Header* keamanan CSP pada aplikasi web harus dikonfigurasi secara ketat dan spesifik. Aplikasi hanya diizinkan untuk memuat *script* atau eksekusi *iframe* dari domain API yang telah disetujui secara eksplisit (seperti Jitsi, Xendit, dan Midtrans) guna memitigasi serangan penyuntikan eksternal (XSS).
