# Model 2: Aplikasi Telemedicine dengan Konsultasi dan Resep Obat

## Tujuan
Menjelaskan model bisnis aplikasi telemedicine yang melibatkan konsultasi dokter, pembuatan resep digital, dan pengiriman obat keras. Model ini cocok untuk layanan kesehatan berbasis program pengobatan seperti penurunan berat badan.

## Ringkasan Singkat
Model ini adalah **platform telemedicine** lengkap, bukan hanya toko online. Platform menggabungkan:
- **Pendaftaran pasien dengan skrining klinis**
- **Konsultasi dokter multimedia** (chat + opsi video)
- **Rekam medis elektronik** dengan formulir SOAP
- **Resep digital (e-prescription)** yang aman
- **Pembayaran dua tahap** (konsultasi + obat)
- **Manajemen apotek & logistik** untuk pengiriman obat
- **Kepatuhan PSE dan regulasi kesehatan**

## Inti Framework Model
1. **Sistem Pasien**: Pasien mendaftar dengan WhatsApp dan mengisi anamnesis.
2. **Sistem Dokter**: Dokter diverifikasi oleh admin, membuat jadwal, menerima konsultasi.
3. **Konsultasi**: Chat multimedia terintegrasi di browser; opsi video bila diperlukan.
4. **Resep Digital**: Dokter membuat resep PDF, dilindungi watermark atau QR code.
5. **Apotek & Fulfillment**: Apotek memproses resep, mengemas obat sesuai aturan, mengirim ke pasien.
6. **Pembayaran**: Sistem memproses pembayaran konsultasi dan obat melalui gateway.
7. **Notifikasi**: WhatsApp dan internal chat memberi update otomatis.

## Apa yang Dibutuhkan
- Akun pasien, dokter, apotek, admin
- Formulir anamnesis dan skrining otomatis
- Sistem chat medis dengan multimedia
- Modul resep digital dan manajemen farmasi
- Integrasi dengan payment gateway
- Integrasi logistik dan resi otomatis
- Fitur compliance PSE dan dokumen audit

## Alur Sederhana
```mermaid
flowchart TD
  A[Pasien Daftar dan Isi Anamnesis] --> B[Validasi Skrining Otomatis]
  B --> C[Pilih Jadwal Konsultasi]
  C --> D[Bayar Biaya Konsultasi]
  D --> E[Masuk ke Chat Dokter (Teks / Multimedia)]
  E --> F[Dokter Menyelesaikan SOAP dan Buat Resep Digital]
  F --> G[Pembayaran Obat setelah Resep Jadi]
  G --> H[Apotek Proses Resep & Buat Paket Obat]
  H --> I[Kirim Paket via Logistik dengan Resi]
  I --> J[Pasien Terima Obat dan Selesai]
```

## Penjelasan untuk Non-Teknologi
Model ini mirip dengan layanan klinik digital. Pasien tidak perlu pergi ke rumah sakit atau install banyak aplikasi. Semua proses dilakukan melalui website:
- Pasien mendaftar dan menjawab pertanyaan medis sederhana.
- Kemudian memilih dokter dan jadwal konsultasi.
- Konsultasi dilakukan lewat chat, dan jika perlu, via video langsung dalam browser.
- Dokter membuat resep, dan obat dikirim ke alamat pasien.
- Semua tahap pembayaran dikontrol oleh sistem sehingga pasien mengikuti aturan dan apotek dapat memproses resep dengan benar.

## Estimasi Waktu Pembuatan
- **Durasi: 6–7 bulan**
- Breakdown:
  - 1 bulan analisis regulasi, arsitektur, dan rencana kepatuhan
  - 2 bulan pengembangan fitur pasien, dokter, dan chat
  - 1 bulan pengembangan resep digital dan manajemen apotek
  - 1 bulan integrasi pembayaran, logistik, dan notifikasi
  - 1 bulan uji coba, validasi medis, dan compliance testing

## Kelebihan Model Ini
- Bisa melayani produk yang memerlukan resep dan pemantauan medis
- Memberikan nilai tambah layanan kesehatan digital
- Tepat untuk program pengobatan tertentu, seperti penurunan berat badan
- Lebih kuat dalam hal kepercayaan jika dikaitkan dengan dokter dan farmasi

## Keterbatasan Model Ini
- Pengembangan lebih lama dan lebih kompleks
- Butuh proses verifikasi dokter/apotek dan kepatuhan regulasi lebih ketat
- Biaya operasional lebih tinggi karena melibatkan tenaga medis dan pengelolaan resep
