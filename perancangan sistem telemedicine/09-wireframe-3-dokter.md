# 9.3 Wireframe: Portal Medis (Dokter)

Dokumen ini memuat *low-fidelity text wireframe* untuk 6 antarmuka spesifik yang digunakan oleh **Dokter**.

---

## 1. Verifikasi Legalitas (KYC)
**Tujuan:** Mengunci fungsionalitas aplikasi hingga dokter mengunggah dokumen legalitas.

```text
================================================================================
[ HEADER: Sistem Telemedisin - Verifikasi Dokter ]
================================================================================
[ KONTEN TENGAH ]
  PERHATIAN: Akun Anda sedang ditinjau.
  Silakan lengkapi profil medis dan dokumen legal Anda agar dapat berpraktik.

  Nomor STR (Surat Tanda Registrasi):
  [ 123456789012345 ]

  Nomor SIP (Surat Izin Praktik):
  [ 098765432109876 ]

  Spesialisasi:
  [ Spesialis Gizi Klinik (Sp.GK) v]

  Upload Dokumen Asli (PDF/JPG):
  [ Pilih File... ] (SIP_DrAndi.pdf)

  [ KIRIM DOKUMEN UNTUK DIVERIFIKASI ADMIN ]
================================================================================
```

---

## 2. Dashboard Dokter
**Tujuan:** Memberikan ringkasan tugas dan saldo harian.

```text
================================================================================
[ HEADER: Portal Dokter ]                          [ Status: ONLINE ] [ Profil ]
================================================================================
[ SIDEBAR ] | [ KONTEN UTAMA ]
- Dasbor    |  Selamat Datang, Dr. Andi!
- Jadwal    |  +-------------------------------------------------------------+
- Dompet    |  | Saldo Pendapatan Anda: Rp 4.500.000       [ TARIK DANA ]    |
            |  +-------------------------------------------------------------+
            |  
            |  >> Jadwal Konsultasi Anda Hari Ini (Ada 2 Antrean):
            |  +-------------------------------------------------------------+
            |  | 14:00 | Pasien: Budi S. | ID: #KNS-9912 | [ MULAI KONSUL ]  |
            |  | 15:30 | Pasien: Siti A. | ID: #KNS-9913 | [ MENUNGGU ]      |
            |  +-------------------------------------------------------------+
================================================================================
```

---

## 3. Manajemen Jadwal Praktik
**Tujuan:** Interaksi pengaturan waktu kapan dokter bersedia menerima pesanan konsultasi.

```text
================================================================================
[ KONTEN UTAMA: Pengaturan Jadwal Praktik ]

>> Buat Ketersediaan Waktu (Slot):
Pilih Tanggal: [ 12 Okt 2026 ]
Jam Mulai: [ 10:00 ]   Jam Selesai: [ 16:00 ]
Durasi per sesi: [ 30 Menit ]
[ BUAT JADWAL OTOMATIS ]

>> Daftar Slot yang Tersedia:
[ 10:00 - 10:30 ] (Kosong)   [ Hapus ]
[ 10:30 - 11:00 ] (Dipesan oleh Siti A.) -> *Tidak bisa dihapus*
[ 14:00 - 14:30 ] (Dipesan oleh Budi S.)
[ 14:30 - 15:00 ] (Kosong)   [ Hapus ]
================================================================================
```

---

## 4. Ruang Konsultasi Terpadu (*Rich Chat & Side-by-side Video*)
**Tujuan:** Layar *super-app* agar dokter bisa bertukar pesan multimedia, mengundang *video call*, dan mengetik diagnosis (RME) secara bersamaan.

```text
================================================================================
[ HEADER: Mode Konsultasi Aktif ]                     [ SLA Timer: 14:59 ] [ Keluar ]
================================================================================
[ KOLOM KIRI (50% Lebar Layar) ]   | [ KOLOM KANAN (50% Lebar Layar) ]
                                   |
+--------------------------------+ | +-------------------------------------------+
| [ AREA RICH CHAT DENGAN PASIEN]| | | TAB 1: ANAMNESIS PASIEN                   |
| Anda: Halo, silakan kirim foto.| | +-------------------------------------------+
| Pasien: [Gambar: keluhan.jpg]  | | BB: 85kg | TB: 165cm | Gol. Darah: O        |
|                                | | Keluhan: Susah turun berat badan, lemas     |
| [====================] [KIRIM] | |                                             |
| [Kirim Lampiran/Voice Note]    | | +-------------------------------------------+
|                                | | | TAB 2: REKAM MEDIS (SOAP)           [Aktif]|
|  [ INVITE PASIEN VIDEO CALL ]  | | +-------------------------------------------+
+--------------------------------+ | [S] Subjective: _________________________   |
 (Saat video aktif, area ini     | | [O] Objective: __________________________   |
 terbagi antara chat dan video)  | | [A] Assessment: _________________________   |
                                   | [P] Plan: _______________________________   |
                                   |                                             |
                                   |            [ SIMPAN SOAP & LANJUT RESEP ]   |
================================================================================
```

---

## 5. Penerbitan Resep (E-Prescription)
**Tujuan:** Form wajib setelah RME (SOAP) diisi untuk membubuhkan resep digital dan tanda tangan elektronik (TTE).

```text
================================================================================
[ KONTEN UTAMA: Penerbitan E-Prescription ]

Pasien: Budi Santoso | ID Konsultasi: #KNS-9912

>> Input Resep Obat Keras (Pencarian Katalog Obat Apotek)
[ Dropdown Cari Obat: Orlistat 120mg...        v ]
Dosis:      [ 3 x Sehari 1 Kapsul                ]
Instruksi Khusus: [ Diminum bersama makanan berlemak ]
[ + TAMBAH OBAT KE RESEP ]

+------------------------------------------------------------------------------+
| DAFTAR RESEP SEMENTARA:                                                      |
| 1. Orlistat 120mg (3x1) - Diminum bersama makanan                            |
+------------------------------------------------------------------------------+

[X] Saya menjamin bahwa resep ini bebas dari sediaan Narkotika & Psikotropika.
[X] Saya membubuhkan Tanda Tangan Elektronik (TTE) pada PDF yang akan di-generate.

                                       [ GENERATE PDF & KIRIM TAGIHAN KE PASIEN ]
================================================================================
```

---

## 6. Partner Wallet (Dompet Dokter)
**Tujuan:** Pengaturan saldo pencairan komisi (bagi hasil).

```text
================================================================================
[ KONTEN UTAMA: Saldo & Pencairan Dana ]

Saldo Tersedia: Rp 4.500.000

Rekening Tujuan (Telah Divalidasi API Inquiry):
Bank: BCA
Nomor: 1234567890
Atas Nama: DR. ANDI SAPUTRA
[ Ubah Rekening ]

>> Penarikan Dana (Withdrawal)
Nominal: [ Rp 4.500.000 ]
[ TARIK DANA SEKARANG ]

>> Riwayat Pencairan:
- 01 Okt 2026: Rp 5.000.000 (Sukses)
- 12 Okt 2026: Rp 4.500.000 (Pending)
================================================================================
```
