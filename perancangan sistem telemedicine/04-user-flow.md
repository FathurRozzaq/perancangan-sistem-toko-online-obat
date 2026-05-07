# 4. User Flow (Alur Pengguna)

Dokumen ini merinci alur interaksi pengguna dengan Sistem Telemedicine, baik dalam bentuk narasi teks maupun representasi visual menggunakan diagram alir (Flowchart).

---

## 4.1. Alur Pengguna: Pasien (End-to-End Journey)

**Deskripsi Teks:**
1. **Registrasi:** Pasien mendaftar menggunakan nomor WhatsApp aktif.
2. **Klaim Promosi:** Pasien mengklaim *voucher* diskon (melalui kode atau klik *banner*).
3. **Pemilihan Layanan & Cek Stok:** Pasien memilih paket layanan penurunan berat badan. Sistem akan mengecek ketersediaan stok obat:
   - Jika **Stok Habis**: Pasien diblokir dari pemesanan. Sistem menampilkan pesan maaf dan mencatat profil pasien. Ketika stok sudah diisi kembali oleh Apotek, sistem mengirim pesan *Auto Follow-up* via WhatsApp agar pasien melanjutkan pesanan.
   - Jika **Stok Tersedia**: Pasien diizinkan melanjutkan.
4. **Anamnesis Mandiri:** Pasien wajib mengisi kuesioner medis (*smart routing* akan menolak pasien jika terindikasi komorbid).
5. **Tagihan 1 (Jasa Medis):** Pasien masuk ke halaman pembayaran 1, memilih jadwal konsultasi, serta dapat menerapkan *voucher* (diskon Tagihan 1).
6. **Validasi & Menunggu Jadwal:** Sistem memvalidasi pembayaran via *Webhook*. Pasien menunggu jadwal *booking* tiba.
7. **Konsultasi (Multimedia Chat):** Saat jadwal tiba, Pasien dapat saling mengirim pesan *multimedia* (teks, suara, gambar, video) dengan Dokter via fitur *Rich Chat*. Apabila dibutuhkan, Dokter dapat mengirimkan *Video Call Invite*. Jika diundang, Pasien dapat menerima undangan tersebut dan masuk ke ruang Jitsi untuk melakukan panggilan video opsional dengan Dokter.
8. **E-Prescription & Tagihan 2:** Dokter menerbitkan resep. Pasien menerima tagihan kedua untuk penebusan obat dan dapat menerapkan *voucher* (diskon Tagihan 2).
9. **Fulfillment & Logistik:** Pembayaran 2 divalidasi. Apotek memproses resep. Pasien dapat melacak pengiriman melalui pembaruan resi asinkron hingga obat sampai.

**Diagram Alir (Pasien):**

```mermaid
flowchart TD
    A([Mulai: Registrasi No. WA]) --> B[Lihat Katalog Layanan]
    B --> C[Klaim Voucher Diskon]
    C --> D[Pilih Paket & Jadwal]
    D --> E{Cek Stok Obat}
    
    E -- Habis --> F[Tampil Pesan Maaf & Pemblokiran]
    F --> G[Sistem Simpan Riwayat Antrean]
    G --> H[Stok Diperbarui -> Auto Follow-up via WA]
    H --> D
    
    E -- Tersedia --> I[Isi Anamnesis Mandiri]
    I --> J{Skrining Klinis}
    
    J -- Risiko Komorbid --> K([Dialihkan/Ditolak])
    J -- Lolos Skrining --> L[Pembayaran Tagihan 1 Konsultasi]
    L --> M[Terapkan Voucher Diskon]
    M --> N[Validasi Payment Gateway Webhook]
    N --> O[Ruang Virtual Aktif sesuai Jam Booking]
    
    O --> P1[Mulai Komunikasi via Rich Chat]
    P1 --> P2[Menerima Invite Video Call dari Dokter]
    P2 --> P[Masuk Ruang Sesi Video via Jitsi]
    P --> Q[Menerima E-Prescription PDF]
    Q --> R[Pembayaran Tagihan 2 Penebusan Obat]
    R --> S[Terapkan Voucher Diskon]
    S --> T[Validasi Payment Gateway Webhook]
    
    T --> U[Proses Apotek & Pencetakan Resi]
    U --> V[Pelacakan Resi Asinkron]
    V --> W([Selesai: Obat Diterima])
```

---

## 4.2. Alur Pengguna: Dokter

**Deskripsi Teks:**
1. **Registrasi & Verifikasi KYC:** Dokter mendaftar akun dan wajib mengunggah dokumen legalitas (STR & SIP). Akun berstatus *pending* hingga disetujui Admin.
2. **Pengaturan Jadwal:** Setelah akun aktif, Dokter *login* dan mengatur ketersediaan slot jadwal praktik.
3. **Visibilitas Ketersediaan:** Dokter dapat melihat apakah status obat sedang "Tersedia" atau "Habis" di sistem.
4. **Persiapan Konsultasi:** Dokter menerima notifikasi jadwal konsultasi yang sudah terbayar oleh Pasien.
5. **Sesi Konsultasi (Rich Chat & Side-by-side):** Dokter berkomunikasi dengan Pasien via *Rich Chat* (mengirim/menerima teks, suara, gambar, video). Apabila diperlukan pemeriksaan visual, Dokter dapat menekan tombol *Invite Video Call*. Jika pasien bergabung ke video, layar Dokter akan menampilkan *streaming* video berdampingan dengan riwayat *chat*, form Anamnesis, dan formulir SOAP.
6. **Pengisian Rekam Medis (SOAP):** Saat sesi berlangsung atau setelah sesi ditutup, Dokter wajib mengisi catatan diagnosis klinis berformat SOAP. Sistem mengunci fitur peresepan sebelum formulir ini selesai.
7. **Penerbitan Resep:** Selesai merekam catatan medis, Dokter menerbitkan resep obat keras secara deskriptif (tanpa ICD, dan mematuhi validasi larangan peresepan narkotika/injeksi) yang otomatis diubah menjadi PDF *watermarked*.
8. **Bagi Hasil & Pencairan Dana:** Dokter memantau saldo pendapatan di Dasbor *Partner Wallet*. Dokter dapat mendaftarkan nomor rekening (divalidasi otomatis oleh sistem) lalu melakukan pencairan dana mandiri (*withdrawal*).
9. **Koordinasi:** Jika ada kendala, Dokter dapat menggunakan *Internal Chat* untuk berkoordinasi dengan Admin.

**Diagram Alir (Dokter):**

```mermaid
flowchart TD
    A([Mulai: Registrasi & Unggah STR/SIP]) --> A1{Verifikasi KYC Admin}
    A1 -- Ditolak --> A2([Perbaiki Dokumen])
    A1 -- Disetujui --> B[Atur Jadwal Praktik]
    B --> C[Menerima Booking Konsultasi]
    C --> D1[Komunikasi Awal via Rich Chat]
    D1 --> D2[Kirim Invite Video Call ke Pasien]
    D2 --> D[Pasien Bergabung ke Ruang Virtual Jitsi]
    D --> E[Lihat Video, Chat, Anamnesis, & Isi SOAP]
    E --> F[Konsultasi Selesai]
    F --> F1[Finalisasi Pengisian SOAP]
    F1 --> G[Tulis & Terbitkan E-Prescription]
    G --> H[Sistem Kalkulasi Komisi Otomatis]
    H --> I([Cek Saldo di Partner Wallet])
```

---

## 4.3. Alur Pengguna: Apotek

**Deskripsi Teks:**
1. **Registrasi & Verifikasi KYC:** Apotek mendaftar akun dan wajib mengunggah dokumen legalitas (SIA & SIPA). Akun berstatus *pending* hingga disetujui Admin.
2. **Pembaruan Stok:** Setelah akun aktif, Apotek *login* dan memasukkan jumlah (*input* kuantitas) stok obat yang tersedia.
3. **Penerimaan Pesanan:** Apotek menerima notifikasi pesanan setelah Tagihan 2 Pasien berstatus *Paid*.
4. **Validasi & Fulfillment:** Apotek melihat resep PDF, memvalidasi keabsahannya, lalu mulai menyiapkan obat.
5. **Keamanan & Logistik:** Apotek diwajibkan mencentang *checklist* kepatuhan (kemasan tertutup rapat, tidak tembus pandang, disegel khusus). Setelah dicentang, Apotek dapat mengatur *pickup* kurir (terintegrasi API Biteship untuk cetak resi otomatis).
6. **Bagi Hasil & Pencairan Dana:** Apotek memantau saldo pendapatan penjualan di *Partner Wallet*. Apotek mendaftarkan nomor rekening untuk divalidasi, lalu melakukan pencairan (*withdrawal*) langsung ke bank.
7. **Koordinasi:** Apotek dapat menghubungi Admin melalui *Internal Chat* jika ada kendala pasokan atau pengiriman.

**Diagram Alir (Apotek):**

```mermaid
flowchart TD
    A([Mulai: Registrasi & Unggah SIA/SIPA]) --> A1{Verifikasi KYC Admin}
    A1 -- Ditolak --> A2([Perbaiki Dokumen])
    A1 -- Disetujui --> B[Update Kuantitas Stok Obat]
    B --> C{Pemberitahuan Pesanan Masuk}
    C --> |Tagihan 2 Lunas| D[Validasi E-Prescription PDF]
    D --> E[Proses Pengemasan Obat]
    E --> F{Centang Syarat Kemasan Tertutup & Segel}
    F -- Ya --> G[Request Pickup Kurir API Logistik]
    G --> H[Cetak Resi AWB]
    H --> I[Sistem Kalkulasi Komisi Otomatis]
    I --> J([Cek Saldo di Partner Wallet])
```

---

## 4.4. Alur Pengguna: Superadmin & Admin Operasional

**Deskripsi Teks:**
1. **Konfigurasi Keuangan (Superadmin):** Superadmin mengatur *Commission Settings* (pembagian hasil) dan *Discount Split* (pembagian beban subsidi voucher) antara platform dan mitra.
2. **Manajemen Stok Darurat (Superadmin):** Jika Apotek berkendala, Superadmin dapat melakukan *bypass* untuk mengupdate stok, atau menekan tombol *Hold* untuk menyetop seluruh penjualan.
3. **Pemantauan SLA (Admin/Superadmin):** Mengawasi indikator warna (merah/kuning) pada tiket, jadwal konsultasi, atau waktu pengemasan.
4. **Eskalasi & Pengingat:** Jika terjadi keterlambatan tugas dari peran lain, Admin/Superadmin dapat menekan tombol *In-App Nudge* atau memicu *Direct WhatsApp Alert* kepada pengguna terkait.
5. **Verifikasi KYC (Admin):** Memvalidasi dokumen STR/SIP Dokter dan SIA/SIPA Apotek yang baru mendaftar agar akun mereka dapat aktif.
6. **Manajemen Chat:** Admin mengelola tiket obrolan internal (dari Dokter/Apotek) serta melihat eskalasi percakapan *Customer Support* dari Pasien (yang ditangani oleh Admin Chat).
7. **Kepatuhan PSE (Superadmin):** Superadmin bertindak sebagai Narahubung Hukum yang merespons Laporan Pengguna (*UGC*), mengeksekusi Pemutusan Akses (*Takedown*) maksimal 1x24 jam, serta menarik rekaman *Audit Trail* bagi keperluan Instansi Penegak Hukum (maksimal 5 hari).

**Diagram Alir (Superadmin & Admin):**

```mermaid
flowchart TD
    A([Superadmin/Admin Login]) --> B{Pilih Modul Dasbor}
    
    B --> |Keuangan| C[Atur Commission Settings & Discount Split]
    C --> D[Pantau Global Finance Dashboard]
    
    B --> |Inventaris| E[Bypass Update Stok Obat]
    E --> F[Tombol Hold Penjualan Darurat]
    
    B --> |Operasional| G[Pantau Indikator SLA Merah/Kuning]
    G --> H{Ada Keterlambatan/Kendala?}
    H -- Ya --> I[Kirim In-App Nudge]
    H -- Kritis --> J[Kirim Direct WhatsApp Alert]
    H -- Tidak --> K[Verifikasi KYC Tenaga Medis]
    
    B --> |Kepatuhan PSE| L[Tinjau Laporan Konten UGC]
    L --> M{Melanggar Hukum?}
    M -- Ya --> N[Eksekusi Takedown maks 24 Jam]
    
    B --> |Penegakan Hukum| O[Permintaan Data dari Instansi]
    O --> P[Ekspor Audit Trail maks 5 Hari]
```

---

## 4.5. Alur Dukungan Pelanggan (Admin Chat & AI)

**Deskripsi Teks:**
1. **Pasien Mengirim Pesan:** Pasien membuka *widget chat* untuk bertanya.
2. **AI Chatbot (Auto-responder):** AI mendeteksi pertanyaan. Jika bersifat umum (FAQ), AI langsung merespons.
3. **Eskalasi ke Agen:** Jika pertanyaan spesifik atau AI gagal merespons, obrolan diteruskan secara otomatis kepada agen manusia (*Admin Chat*).
4. **Penanganan Keluhan:** *Admin Chat* merespons dan menyelesaikan kendala Pasien.

**Diagram Alir (Customer Support):**

```mermaid
flowchart TD
    A([Pasien Mengirim Pesan Chat]) --> B[Diterima oleh AI Chatbot]
    B --> C{Apakah Pertanyaan Umum/FAQ?}
    C -- Ya --> D[AI Memberikan Jawaban Otomatis]
    C -- Tidak/Kompleks --> E[Eskalasi ke Dasbor Admin Chat]
    E --> F[Admin Chat Merespons Pasien]
    F --> G([Kendala Terselesaikan])
```

---

## 4.6. Alur Registrasi Rekening & Pencairan Dana (Payout)

**Deskripsi Teks:**
1. **Registrasi Rekening:** Dokter atau Apotek mendaftarkan Bank dan Nomor Rekening melalui dasbor *Partner Wallet*.
2. **Validasi Account Inquiry:** Sistem secara *real-time* melakukan panggilan ke API Payment Gateway untuk memvalidasi keberadaan nomor rekening tersebut. Sistem kemudian memunculkan *Nama Pemilik Rekening* asli ke layar.
3. **Konfirmasi Pengguna:** Pengguna memastikan apakah nama yang muncul sudah sesuai (guna mencegah salah transfer), lalu menyimpannya.
4. **Pencairan Mandiri (Self-Service):** Dokter atau Apotek mengeklik tombol tarik saldo dari *Partner Wallet*.
5. **Eksekusi Transfer:** Sistem (via *Disbursement API*) memproses transfer dana bagi hasil langsung ke rekening bank pengguna.
6. **Pencairan Paksa (Force Payout):** Dalam situasi darurat atau kebutuhan tak terduga, Superadmin dapat mengakses Dasbor Keuangan dan memicu pencairan saldo bagi hasil secara paksa langsung ke rekening mitra yang sudah tervalidasi.

**Diagram Alir (Payout):**

```mermaid
flowchart TD
    A([Mulai: Buka Partner Wallet]) --> B[Input Nama Bank & Nomor Rekening]
    B --> C[Sistem Hit API Account Inquiry]
    C --> D{Apakah Rekening Valid?}
    
    D -- Tidak --> E[Tampil Error: Rekening Tidak Ditemukan]
    D -- Ya --> F[Tampil Nama Pemilik Rekening Asli]
    
    F --> G[Pengguna Konfirmasi & Simpan Rekening]
    
    G --> H[Pengguna Klik Tarik Saldo Pencairan]
    H --> I[Sistem Proses Disbursement Transfer]
    I --> J([Saldo Masuk ke Rekening Tujuan])
    
    K([Superadmin: Force Payout]) -.->|Kondisi Tak Terduga| I
```
