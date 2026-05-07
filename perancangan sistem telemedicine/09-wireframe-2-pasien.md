# 9.2 Wireframe: Portal Pasien

Dokumen ini memuat *low-fidelity text wireframe* untuk 8 halaman spesifik yang diakses oleh **Pasien** setelah melakukan *login*.

---

## 1. Dashboard Pasien
**Tujuan:** Memberikan ringkasan informasi jadwal terdekat dan status pengiriman obat dengan cepat.

```text
================================================================================
[ HEADER: Logo Telemedisin ]                 [ Search ] [ Notifikasi ] [ Profil ]
================================================================================
[ SIDEBAR ] | [ KONTEN UTAMA ]
- Beranda   | 
- Katalog   |  Halo, Budi Santoso!
- Riwayat   |  +-------------------------------------------------------------+
- Lacak     |  | BANNERS PROMO: Diskon 20% Paket Konsultasi Berat Badan      |
- Support   |  +-------------------------------------------------------------+
            |  
            |  >> Konsultasi Mendatang:
            |  +-------------------------------------------------------------+
            |  | Tgl: 12 Okt 2026, 14:00 | Dr. Andi (Sp.Gizi) | [MASUK RUANG] |
            |  +-------------------------------------------------------------+
            |
            |  >> Status Pesanan Obat Aktif (#ORD-5541):
            |  [ Packing ] ---- [ Ready Pickup ] ---- [> In Transit ] ---- [ Delivered ]
            |  Resi: BTS-12345 (JNE) | Estimasi Tiba: Besok
================================================================================
```

---

## 2. Katalog Layanan & Booking
**Tujuan:** Memilih paket layanan dan menjadwalkan sesi konsultasi dokter.

```text
================================================================================
[ KONTEN UTAMA ]
Pilih Paket Layanan Medis:
[ KARTU 1: Paket Konsultasi Standar (Rp 150.000) ]
[ KARTU 2: Paket Konsultasi Eksekutif (Rp 250.000) ]

>> Pilih Dokter & Jadwal Tersedia (Untuk Paket Standar):
+-------------------------------------------------------------+
| Dr. Andi (Sp.Gizi) | Rating: 4.9                            |
| Pilih Slot Kosong:                                          |
| [ 12 Okt 14:00 ] [ 12 Okt 15:00 ] [ 13 Okt 10:00 ]          |
+-------------------------------------------------------------+
                                      [ LANJUTKAN KE ANAMNESIS ]
================================================================================
```

---

## 3. Formulir Skrining (Anamnesis)
**Tujuan:** Kuesioner medis *wizard* yang wajib diisi sebelum pembayaran.

```text
================================================================================
[ HEADER: Form Anamnesis Medis ]                            [ Langkah 1 dari 2 ]
================================================================================
[ KONTEN TENGAH ]
Apakah Anda memiliki keluhan terkait gagal jantung atau gangguan ginjal?
( ) Ya
(X) Tidak

Berapa Berat Badan dan Tinggi Badan Anda saat ini?
BB: [ 85 ] kg
TB: [ 165] cm
-> BMI Anda: 31.2 (Obesitas Kelas I)

Apakah Anda sedang mengonsumsi obat-obatan lain secara rutin?
[ ____________________________________________ ]

                                 [ KEMBALI ]      [ SIMPAN & MENUJU PEMBAYARAN ]
================================================================================
```

---

## 4. Halaman Pembayaran (Tagihan 1 & 2)
**Tujuan:** Melakukan pembayaran (*Checkout*) baik untuk biaya Konsultasi (Tagihan 1) maupun Penebusan Obat (Tagihan 2).

```text
================================================================================
[ KONTEN TENGAH ]
+-------------------------------------------------------------+
| TAGIHAN KONSULTASI MEDIS (#INV-1001)                        |
|                                                             |
| Layanan: Paket Konsultasi Standar                           |
| Waktu  : 12 Okt 2026, 14:00                                 |
| Subtotal                 Rp 150.000                         |
|                                                             |
| Masukkan Voucher: [ SEHAT20      ] [ TERAPKAN ]             |
| Diskon (20%)             -Rp 30.000                         |
|                                                             |
| TOTAL BAYAR:             Rp 120.000                         |
|                                                             |
| Pilih Metode Pembayaran:                                    |
| ( ) Transfer Bank (BCA VA)                                  |
| (x) E-Wallet (Gopay)                                        |
|                                                             |
| [ BAYAR SEKARANG ] -> Memicu integrasi Webhook Midtrans     |
+-------------------------------------------------------------+
================================================================================
```

---

## 5. Ruang Konsultasi (Rich Chat & Video)
**Tujuan:** Ruang interaksi sebelum dan selama *video call*, di mana Pasien dapat mengirim foto/suara dan menerima undangan *video call* dari Dokter.

```text
================================================================================
[ HEADER: Konsultasi Aktif dengan Dr. Andi ]           [ Waktu Sisa: 14 Menit ]
================================================================================
[ KONTEN UTAMA: Area Rich Chat ]
 Dr. Andi : Halo, silakan kirimkan foto yang menjelaskan keluhan Anda. (14:02)
 Anda     : [ LAMPIRAN GAMBAR: keluhan.jpg ] (14:04)
          : Ini dok fotonya. (14:04)

 +-------------------------------------------------------------------------+
 | PANGGILAN VIDEO MASUK...                                                |
 | Dr. Andi mengajak Anda untuk bertatap muka.                             |
 | [ TERIMA (GABUNG KE VIDEO JITSI) ]                                      |
 +-------------------------------------------------------------------------+

[==================== AREA KETIK PESAN ====================] [KIRIM]
[ (O) Lampirkan Gambar/Video ]  [ (M) Tahan untuk Voice Note ]
================================================================================

Setelah "TERIMA" diklik, area chat bergeser menyamping dengan Video Jitsi:
================================================================================
[ KIRI: VIDEO JITSI (DOKTER BESAR, PASIEN KECIL) ] | [ KANAN: CHAT BERJALAN ]
[ Mic ] [ Cam ] [ End Call ]                       | [ Ketik pesan... ]
================================================================================
```

---

## 6. Riwayat Medis & Resep (Read-Only)
**Tujuan:** Arsip rekam medis (SOAP) dan resep yang dapat diunduh kapan saja oleh Pasien.

```text
================================================================================
[ SIDEBAR ] | [ KONTEN UTAMA: Riwayat Konsultasi ]
- Beranda   |
- Katalog   |  Riwayat Konsultasi Terakhir: 12 Okt 2026
- Riwayat   |  +-------------------------------------------------------------+
- Lacak     |  | Dokter : Dr. Andi (Sp.Gizi)                                 |
- Support   |  | Diagnosis Utama (Assessment): Obesitas Tingkat I            |
            |  | Pesan Dokter (Plan): Defisit Kalori 500kkal/hari, Olahraga  |
            |  |                                                             |
            |  | Resep Obat:                                                 |
            |  | 1. Orlistat 120mg (3x1)                                     |
            |  |                                                             |
            |  | [ UNDUH PDF E-PRESCRIPTION (TERTANDA TANGAN) ]              |
            |  +-------------------------------------------------------------+
================================================================================
```

---

## 7. Pelacakan Pemesanan (Tracking)
**Tujuan:** Melihat proses Apotek hingga kurir mengantar paket.

```text
================================================================================
[ KONTEN UTAMA: Status Pemesanan Obat ]
Order ID: #ORD-5541
Resi Pengiriman: BTS-12345 (Kurir Ekspedisi: JNE)

Status Perjalanan:
[✓] 12:00 - Pembayaran Obat Diterima
[✓] 12:15 - Apotek Menyiapkan dan Melakukan Pengemasan (Segel Medis)
[✓] 12:45 - Kurir Mengambil Paket
[ ] 14:00 - Paket sedang dalam perjalanan menuju alamat Anda
[ ] 18:00 - Paket telah diterima

Alamat Tujuan:
Jl. Sudirman No.1, Jakarta Pusat.
================================================================================
```

---

## 8. Customer Support & Reporting PSE
**Tujuan:** Tempat pelaporan kendala (CS) dan konten bermasalah (*Takedown request*).

```text
================================================================================
[ KONTEN UTAMA: Pusat Bantuan & Laporan ]

Apa yang ingin Anda sampaikan?
( ) Pertanyaan seputar layanan
( ) Pelaporan bug / masalah transaksi
(x) Laporkan Perilaku Tenaga Medis (Aduan PSE)

Jelaskan Kendala Anda:
[ Dokter berkata kurang pantas saat sesi video... _________________ ]
[ Lampirkan Bukti Gambar (Opsional) ]

[ KIRIM LAPORAN KE ADMIN/MODERATOR ]
================================================================================
```
