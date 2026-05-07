# 9.5 Wireframe: Portal Manajemen (Admin & Superadmin)

Dokumen ini memuat *low-fidelity text wireframe* untuk 6 antarmuka level tinggi yang dikhususkan bagi **Admin dan Superadmin**.

---

## 1. Global Dashboard
**Tujuan:** Memberikan pandangan level atas mengenai performa sistem (*Helicopter View*).

```text
================================================================================
[ HEADER: Telemedisin Superadmin ]                           [ Notifikasi SLA ]
================================================================================
[ SIDEBAR ] | [ KONTEN UTAMA: Dasbor Statistik ]
- Dasbor    |  
- KYC Mitra |  [ KARTU 1: Total Pasien Aktif = 1.250 ]
- SLA Nudge |  [ KARTU 2: Total Konsultasi Selesai (Bulan Ini) = 450 ]
- Paket     |  [ KARTU 3: GMV / Total Transaksi (Bulan Ini) = Rp 120.000.000 ]
- Keuangan  |  
- Hukum PSE |  >> Grafik Tren Pemesanan Obat
            |  [ ................. GRAFIK GARIS (LINE CHART) ................. ]
================================================================================
```

---

## 2. Approval KYC (Verifikasi Mitra)
**Tujuan:** Dasbor untuk memeriksa dan menyetujui pendaftaran spesialis medis/farmasi.

```text
================================================================================
[ KONTEN UTAMA: Persetujuan Legalitas Mitra ]

Filter Status: [ Menunggu Verifikasi (Pending) v ]

+------------------------------------------------------------------------------+
| Nama Pengguna | Role    | No. Registrasi (STR/SIA) | Dokumen   | Aksi        |
+------------------------------------------------------------------------------+
| Dr. Budi      | Dokter  | 123456789012345          | [ LIHAT ] | [ APPROVE ] |
| Apotek Sehat  | Apotek  | 555556666677777          | [ LIHAT ] | [ APPROVE ] |
+------------------------------------------------------------------------------+

*Catatan: Tombol APPROVE akan mengubah status akun mitra menjadi 'Active'.
================================================================================
```

---

## 3. SLA & Nudge Monitor
**Tujuan:** Dasbor pemantauan (*monitoring*) keterlambatan jadwal agar admin dapat melakukan *Nudge* (Peringatan via WhatsApp) ke pihak terkait secara cepat.

```text
================================================================================
[ KONTEN UTAMA: SLA Monitoring (Keterlambatan) ]

>> Indikator Peringatan Dini (Warning):
+------------------------------------------------------------------------------+
| ID/Aktor            | Masalah SLA (Keterlambatan)      | Aksi (WhatsApp Nudge) |
+------------------------------------------------------------------------------+
| Dr. Andi (#KNS-991) | Telat masuk video call > 5 Menit | [ KIRIM PENGINGAT WA ]|
| Apotek Sehat (#554) | Telat packing obat > 2 Jam       | [ KIRIM PENGINGAT WA ]|
+------------------------------------------------------------------------------+
================================================================================
```

---

## 4. Manajemen Layanan & Voucher
**Tujuan:** CRUD paket layanan penurunan berat badan dan pemberian kode diskon promosi.

```text
================================================================================
[ KONTEN UTAMA: Manajemen Katalog & Promo ]

>> 1. Daftar Paket Layanan (Konsultasi)
[ + BUAT PAKET BARU ]
+------------------------------------------------------------------------------+
| Nama Paket           | Harga Konsultasi | Status Aktif | Aksi                |
+------------------------------------------------------------------------------+
| Konsultasi Standar   | Rp 150.000       | [ ON  ]      | [ EDIT ] [ HAPUS ]  |
| Konsultasi Eksekutif | Rp 250.000       | [ OFF ]      | [ EDIT ] [ HAPUS ]  |
+------------------------------------------------------------------------------+

>> 2. Daftar Voucher Diskon
[ + BUAT VOUCHER BARU ]
Kode: [ SEHAT20  ] | Tipe: [ Persentase (%) ] | Nilai: [ 20 ] | Quota: [ 100 ]
================================================================================
```

---

## 5. Manajemen Keuangan & Bagi Hasil
**Tujuan:** Mengatur persentase komisi platform dan melihat mutasi keluar/masuk (Payment Gateway).

```text
================================================================================
[ KONTEN UTAMA: Pengaturan Komisi & Mutasi Finansial ]

>> Konfigurasi Bagi Hasil (Platform Fee Settings)
Potongan Komisi Dokter (%): [ 15 ] %   [ SIMPAN ]
Potongan Komisi Apotek (%): [  5 ] %   [ SIMPAN ]

>> Riwayat Mutasi (*Double Billing & Withdrawal*)
+------------------------------------------------------------------------------+
| ID Ref     | Tipe Transaksi | Nominal       | Status Gateway | Aksi          |
+------------------------------------------------------------------------------+
| INV-1001   | Pembayaran Masuk| + Rp 120.000 | [ PAID ]       | [ DETAIL ]    |
| WD-0992    | Penarikan Keluar| - Rp 4.500.00 | [ SUCCESS ]    | [ DETAIL ]    |
+------------------------------------------------------------------------------+
================================================================================
```

---

## 6. Kepatuhan Hukum & PSE
**Tujuan:** Fitur khusus untuk memenuhi aturan pemerintah terkait *Takedown* konten (UGC) dan *Audit Trail* pelaporan Aparat Penegak Hukum (APH).

```text
================================================================================
[ KONTEN UTAMA: Pusat Kepatuhan Hukum PSE ]

>> 1. Moderasi Konten (UGC Takedown Request)
+------------------------------------------------------------------------------+
| Pelapor  | Terlapor | Alasan Laporan         | Bukti       | Aksi Moderasi   |
+------------------------------------------------------------------------------+
| Siti A.  | Dr. Budi | Pelanggaran Etik       | [ Lihat ]   | [ SUSPEND AKUN ]|
+------------------------------------------------------------------------------+

>> 2. Sistem Rekam Jejak (Audit Trail)
Gunakan fitur ini untuk mengekspor data forensik yang diminta Penegak Hukum.
Pilih Entitas: [ Users       v ]
Tanggal Mulai: [ 01/10/2026  ]   Tanggal Akhir: [ 30/10/2026 ]
[ EKSPOR LOG KE .CSV ]
================================================================================
```
