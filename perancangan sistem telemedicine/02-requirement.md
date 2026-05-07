# 2. Requirements

Dokumen ini mendefinisikan persyaratan tingkat tinggi (high-level requirements) yang harus dipenuhi oleh Sistem Telemedicine untuk mencapai tujuannya. Persyaratan ini dibagi menjadi *Functional Requirements* (Fungsional) dan *Non-Functional Requirements* (Non-Fungsional).

## 2.1 Functional Requirements (Persyaratan Fungsional)

Sistem harus mampu melakukan fungsi-fungsi utama berikut:

1. **Manajemen Pengguna & Akses (Identity & Access Management)**
   - Sistem harus mendukung registrasi, autentikasi, dan otorisasi pengguna. **Pendaftaran akun Pasien diwajibkan menggunakan Nomor WhatsApp yang aktif** guna menjamin keandalan pengiriman notifikasi, pengingat (*nudge*), dan pesan tindak lanjut (*follow-up*).
   - **Kepatuhan KYC Medis**: Pendaftaran untuk peran **Dokter** diwajibkan mengunggah Surat Tanda Registrasi (STR) dan Surat Izin Praktik (SIP) yang aktif. Pendaftaran peran **Apotek** diwajibkan mengunggah Surat Izin Apotek (SIA) dan Surat Izin Praktik Apoteker (SIPA). Akun pendaftar akan berstatus *pending* dan tidak dapat beroperasi hingga dokumen diverifikasi secara manual oleh Admin/Superadmin.
   - Sistem harus memiliki *Role-Based Access Control* (RBAC) dengan 6 peran utama: **Superadmin** (dapat mengakses seluruh fitur dari semua *role*, termasuk bertindak sebagai Narahubung Hukum), **Admin** (mengawasi operasional dan memverifikasi dokumen medis), **Pasien**, **Dokter**, **Apotek**, dan **Admin Chat**.

2. **Katalog Layanan & Skrining Klinis (Anamnesis)**
   - Sistem harus menampilkan katalog paket layanan penurunan berat badan.
   - Pasien diwajibkan untuk membuat akun terlebih dahulu sebelum dapat mengakses dan melakukan pengisian anamnesis mandiri.
   - Formulir anamnesis mandiri ini wajib diisi setiap kali Pasien akan melakukan pemesanan (booking) layanan, sehingga satu Pasien dapat memiliki banyak data rekam anamnesis sesuai dengan riwayat pesanannya.
   - Sistem harus memiliki validasi logika dasar yang menolak atau mengalihkan Pasien apabila indikasi medis (berdasarkan pengisian anamnesis) menunjukkan risiko/kontraindikasi terhadap program (skrining otomatis).

3. **Penjadwalan, Konsultasi Interaktif, & Pembatalan**
   - Pasien harus dapat memilih jadwal konsultasi yang tersedia.
   - Sistem harus mendukung mekanisme pembatalan: pembatalan manual oleh Pasien, pembatalan otomatis oleh sistem (jika Pasien tidak membayar dalam tenggang waktu), dan pembatalan sepihak oleh Admin.
   - **Sesi konsultasi utama dilakukan melalui chat multimedia** (teks, suara, gambar, video) yang interaktif terintegrasi di halaman web tanpa mengharuskan pengguna menginstal aplikasi tambahan. *Video meeting* tidak diwajibkan untuk setiap sesi.
   - Dokter memiliki wewenang dan fasilitas di dalam aplikasi untuk **mengajak (menginisiasi) video call** kepada pasien jika dibutuhkan pemeriksaan visual langsung.
   - Sistem harus dapat memunculkan data rekam medis/anamnesis secara berdampingan (side-by-side) dengan antarmuka video dan *chat* pada layar Dokter.

4. **Rekam Medis Elektronik (RME)**
   - Sistem harus memiliki formulir Rekam Medis berformat SOAP (Subjective, Objective, Assessment, Plan) terintegrasi pada layar kerja Dokter.
   - Dokter dapat mulai mengisi formulir SOAP saat panggilan video berlangsung maupun setelah panggilan berakhir.
   - Sistem mewajibkan penyelesaian pengisian SOAP ini sebelum Dokter diizinkan menerbitkan Resep Digital (E-Prescription).
   - Pasien hanya diberikan akses baca (*read-only*) terhadap riwayat hasil diagnosis SOAP mereka di halaman profil.

5. **Resep Digital (E-Prescription)**
   - Sistem harus memungkinkan Dokter meresepkan obat keras penurun berat badan secara deskriptif (tanpa kodifikasi ICD). **Sistem harus memiliki batasan validasi ketat yang melarang input dan peresepan obat golongan Narkotika, Psikotropika, serta sediaan Injeksi.**
   - Sistem harus dapat *generate* dokumen resep secara otomatis dalam bentuk digital (PDF).
   - Dokumen resep (PDF) harus memuat bukti Tanda Tangan Elektronik (TTE) dari Dokter yang bersangkutan sebagai legalitas, serta dilengkapi dengan fitur pengaman keaslian seperti *watermark* atau kode QR.

6. **Transaksi Finansial Ganda (*Double Billing*)**
   - Sistem harus mampu mengakomodasi dua tahap pembayaran yang terpisah:
     1. Pembayaran pertama: Biaya jasa medis / konsultasi (dibayar sebelum sesi dimulai).
     2. Pembayaran kedua: Biaya penebusan obat / farmasi (dibayar setelah Dokter menerbitkan resep).
   - Sistem harus terintegrasi dengan Payment Gateway pihak ketiga (seperti Midtrans atau Xendit) untuk memvalidasi status pembayaran secara otomatis via Webhook.

7. **Pemenuhan Obat (Fulfillment) & Integrasi Logistik**
   - Sistem harus dapat secara otomatis meneruskan (*forward*) pesanan obat yang telah lunas beserta dokumen resep PDF ke sistem manajemen farmasi pusat.
   - **Keamanan Pengemasan (Regulasi BPOM)**: Sistem harus mewajibkan Apotek untuk mencentang persetujuan (*mandatory checklist*) bahwa paket obat telah dikemas secara tertutup rapat, tidak tembus pandang, dan disegel khusus (serta menyertakan etiket di dalam kemasan) sebelum tombol *Request Pickup Kurir* dapat ditekan.
   - Sistem harus terintegrasi dengan penyedia API logistik (seperti Biteship) untuk otomatis mencetak resi pengiriman (AWB).
   - Sistem harus mampu melacak dan menampilkan pembaruan status pengiriman paket (tracking) kepada pasien secara *asinkron*.

8. **Sistem Notifikasi**
   - Sistem harus mampu mengirimkan notifikasi *email* atau pesan (misalnya melalui WhatsApp Gateway) untuk informasi krusial seperti tagihan pembayaran, pengingat jadwal, dan *update* pengiriman obat.

9. **Sistem Komunikasi Terpadu (Rich Chat & Internal Chat)**
   - **Rich Chat Dokter-Pasien**: Sistem harus menyediakan fasilitas *chat* langsung antara Dokter dan Pasien terkait sesi konsultasi mereka. Fitur ini harus mendukung pengiriman *multimedia* berupa **teks, pesan suara (voice note), gambar, dan video**.
   - Sistem harus menyediakan fasilitas komunikasi dua arah melalui fitur *chat* bagi Pasien untuk menghubungi *Admin Chat* guna menangani keluhan atau pertanyaan (didukung oleh *AI Chatbot* untuk FAQ).
   - Sistem juga harus menyediakan jalur komunikasi internal yang memungkinkan peran **Dokter** dan **Apotek** untuk melakukan *chat* secara langsung dengan **Admin** terkait koordinasi pekerjaan, kendala sistem, atau masalah logistik.

10. **Sistem Pengingat & Pemantauan SLA Operasional (Internal Nudge)**
   - Sistem harus memiliki penetapan standar **Service Level Agreement (SLA)** atau batas waktu maksimal penyelesaian tugas untuk setiap peran operasional (contoh: Dokter harus masuk ruang virtual maksimal 5 menit dari jadwal, Apotek harus mengemas obat maksimal 2 jam sejak pesanan masuk, Admin Chat wajib membalas pesan dalam 10 menit).
   - Sistem harus memfasilitasi peran Superadmin dan Admin untuk mengawasi status SLA ini. Jika ada tugas yang melewati batas waktu, mereka dapat mengirimkan peringatan (Nudge/Reminder) kepada peran yang bersangkutan (seperti Dokter, Apotek, atau Pasien).
   - Peringatan harus dapat dikirim dalam bentuk notifikasi internal di dalam aplikasi (in-app notification).
   - Superadmin dan Admin harus dapat mengirimkan pesan peringatan khusus secara instan ke nomor WhatsApp milik pengguna yang bersangkutan melalui integrasi WhatsApp Gateway.

11. **Manajemen Keuangan & Bagi Hasil (Revenue Sharing)**
    - Sistem harus menyediakan dasbor keuangan pusat bagi *Superadmin* untuk memantau total pemasukan pasien (dari Tagihan 1 dan Tagihan 2) serta status pencairan (*payout*).
    - Sistem harus memiliki fitur pengaturan bagi hasil (*Commission Settings*), di mana *Superadmin* berwenang untuk menentukan skema potongan platform secara dinamis, baik berdasarkan besaran nominal tetap maupun persentase dari setiap layanan Dokter dan penjualan Apotek. (Peran lain seperti Admin dan Admin Chat mendapatkan gaji tetap sehingga tidak termasuk dalam sistem bagi hasil transaksi ini).
    - Sistem harus mampu menghitung nilai bagi hasil secara otomatis pada setiap siklus transaksi yang selesai, merujuk pada aturan konfigurasi yang telah diatur oleh Superadmin.
    - Sistem harus menyediakan dasbor pendapatan/dompet secara individual bagi *Dokter* dan *Apotek*, di mana mereka hanya dapat melihat rekapitulasi hak pendapatan/komisi mereka masing-masing.
    - **Pencairan Dana Mandiri (Self-Service Payout)**: *Dokter* dan *Apotek* harus dapat melakukan penarikan saldo (*withdrawal*) dari dompet mereka ke rekening bank pribadi/perusahaan secara mandiri.
    - **Validasi Rekening Bank (Account Inquiry)**: Sebelum melakukan pencairan, *Dokter* dan *Apotek* wajib mendaftarkan nomor rekening bank. Sistem harus terintegrasi dengan API pihak ketiga (misalnya fitur *Account Inquiry* dari Payment Gateway) untuk memverifikasi keabsahan nomor rekening secara *real-time* dan menampilkan otomatis Nama Pemilik Rekening untuk mencegah kesalahan transfer dana.
    - **Pencairan Paksa oleh Superadmin (Force Payout)**: Untuk mengantisipasi keadaan darurat atau kebutuhan tak terduga, *Superadmin* harus memiliki wewenang untuk mengeksekusi pencairan saldo secara manual langsung ke rekening mitra (Dokter/Apotek) yang sudah tervalidasi.

12. **Sistem Promosi dan Voucher Diskon**
    - Sistem harus menyediakan fitur *Voucher* yang dapat diklaim oleh Pasien (melalui input kode manual atau mengeklik *banner* promosi) dan digunakan pada halaman pembayaran.
    - Sistem harus membedakan kategori *voucher* secara spesifik, yakni khusus untuk Tagihan 1 (Konsultasi) atau khusus untuk Tagihan 2 (Obat).
    - *Superadmin* harus dapat mengatur besaran diskon pada *voucher* dalam bentuk persentase (%) maupun potongan nominal (Rp).
    - **Subsidi Diskon (*Discount Split*)**: Sistem harus memungkinkan *Superadmin* untuk mengatur proporsi penanggungan biaya diskon. *Superadmin* dapat mengonfigurasi berapa porsi beban diskon yang ditanggung (disubsidi) oleh pendapatan platform, dan berapa porsi yang memotong pendapatan mitra (Dokter atau Apotek). Aturan ini krusial untuk menjaga transparansi kesepakatan bagi hasil.

13. **Manajemen Inventaris Obat (Inventory Management)**
    - *Apotek* memiliki wewenang utama untuk menentukan dan memperbarui jumlah (kuantitas) stok obat yang dialokasikan untuk dijual melalui sistem *web*.
    - *Superadmin* dan *Admin* dapat melihat jumlah angka stok yang tersedia secara rinci. *Superadmin* juga diberikan akses khusus untuk mengubah stok secara manual (sebagai langkah *fallback* jika peran Apotek mengalami kendala teknis/operasional).
    - *Dokter* dan *Pasien* hanya diberikan visibilitas ketersediaan dalam bentuk status sederhana ("Tersedia" atau "Habis"), tanpa menampilkan angka pasti dari stok tersebut.
    - **Pencegahan Pemesanan (Stock Hold)**: *Superadmin* berwenang menghentikan (*hold*) aktivitas penjualan. Apabila stok obat berstatus "Habis", sistem otomatis memblokir Pasien agar tidak dapat melakukan pemesanan layanan maupun mengisi formulir anamnesis, dan menampilkan pesan permohonan maaf atas ketidaktersediaan layanan.
    - **Auto Follow-Up**: Sistem harus menyimpan data riwayat Pasien yang mencoba memesan namun gagal akibat kehabisan stok. Saat stok obat kembali diperbarui (tersedia), sistem secara otomatis mengirimkan notifikasi *follow-up* berupa ajakan memesan kembali melalui integrasi pesan *WhatsApp Gateway* dan *Web Chat* Pasien tersebut.

14. **Kepatuhan Penyelenggara Sistem Elektronik (PSE Lingkup Privat)**
    - Berdasarkan Permenkominfo No. 5 Tahun 2020, sistem wajib menyediakan **Sarana Pelaporan Pengguna** (*User Reporting*) pada halaman Pasien untuk melaporkan *User Generated Content* (UGC) seperti *chat* yang melanggar hukum atau meresahkan.
    - Sistem wajib memfasilitasi peran Superadmin dengan fitur/tombol eksekusi **Pemutusan Akses (Takedown)** guna menghapus atau memblokir konten terlarang maksimal dalam 1x24 jam (atau 4 jam pada kasus mendesak) sejak peringatan diterima.
    - Sistem wajib menyediakan kemampuan ekstraksi data (*Audit Trail / Data Access Portal*) guna memenuhi **Akses Pengawasan dan Penegakan Hukum Pidana** bagi instansi berwenang paling lambat 5 (lima) hari kalender. Kewenangan operasional pengolahan dan penyerahan data ini diberikan kepada *Superadmin* selaku *Narahubung Hukum* platform.

---

## 2.2 Non-Functional Requirements (Persyaratan Non-Fungsional)

Sistem harus memenuhi standar teknis, kinerja, dan keamanan sebagai berikut:

1. **Keamanan (Security) dan Privasi Data (Sangat Krusial)**
   - Sistem harus menjaga dan melindungi: integritas, ketersediaan, dan **kerahasiaan (confidentiality)** dari Data Elektronik dan Data Pribadi pasien.
   - **Enkripsi At-Rest**: Seluruh data pasien (anamnesis, resep medis, profil) harus dienkripsi menggunakan standar AES-256 saat tersimpan di *database*.
   - **Enkripsi In-Transit**: Seluruh lalu lintas data yang ditransmisikan harus dilindungi menggunakan TLS/SSL.
   - **Proteksi Jaringan**: Wajib menerapkan *Rate Limiting* pada *endpoint* publik untuk mencegah serangan *Brute Force*.
   - **Content Security Policy (CSP)**: Konfigurasi CSP harus disesuaikan secara ketat, hanya mengizinkan pemuatan *iframe* dan skrip eksternal dari domain terpercaya (seperti *payment gateway* dan Jitsi).
   - **Retensi Data Rekam Medis**: Seluruh riwayat Rekam Medis Elektronik (RME) wajib disimpan dan diamankan dengan masa retensi paling singkat 25 (dua puluh lima) tahun sejak tanggal kunjungan terakhir pasien.

2. **Keandalan (Reliability)**
   - Proses beban berat (seperti komputasi/pembuatan dokumen PDF resep, pengiriman *email* notifikasi, dan komunikasi panggilan API dengan logistik) tidak boleh memblokir alur kerja utama aplikasi. Proses ini harus dipisahkan menjadi *background jobs* menggunakan sistem antrean (seperti *Message Broker* atau Redis).

3. **Skalabilitas (Scalability)**
   - Arsitektur berbasis *Modular Monolithic* harus digunakan untuk memudahkan pengembangan awal sekaligus menyederhanakan proses jika ke depannya dipecah menjadi *microservices*.
   - Penyimpanan berkas statis atau *media* (seperti PDF resep atau foto medis) harus ditempatkan di luar peladen (server) aplikasi utama, menggunakan *Object Storage* (contoh: AWS S3 atau Google Cloud Storage) untuk mencegah kelebihan beban pada peladen.
   - Beban pemrosesan aliran video (video streaming) harus ditanggung oleh peladen terpisah (misalnya melalui Jitsi as a Service / *public server*), sehingga beban peladen aplikasi utama tetap efisien.

4. **Pemeliharaan (Maintainability)**
   - Pembuatan kode *backend* (menggunakan kerangka kerja Laravel) harus mengadopsi pola arsitektur *Repository Pattern* atau *Service Pattern*.
   - Pola kode tersebut wajib memisahkan logika bisnis dari *controller* untuk memastikan kode tetap bersih, sangat *maintainable*, dan mudah diuji secara otomatis (mendukung *Unit Testing*).
