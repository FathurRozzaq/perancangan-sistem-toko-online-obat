# 9.4 Wireframe: Portal Farmasi (Apotek)

Dokumen ini memuat *low-fidelity text wireframe* untuk 6 antarmuka operasional yang diakses oleh **Apotek/Farmasi**.

---

## 1. Verifikasi Legalitas (KYC)
**Tujuan:** Mengunci aplikasi sampai dokumen keabsahan legal apotek disetujui Admin.

```text
================================================================================
[ HEADER: Sistem Telemedisin - Verifikasi Apotek ]
================================================================================
[ KONTEN TENGAH ]
  PERHATIAN: Akses penerimaan pesanan Anda masih dikunci.
  Silakan lengkapi profil Farmasi dan dokumen legalitas.

  Nomor SIA (Surat Izin Apotek):
  [ 555556666677777 ]

  Nomor SIPA (Surat Izin Praktik Apoteker):
  [ 999998888877777 ]

  Upload Dokumen Asli (PDF/JPG):
  [ Pilih File... ] (SIA_ApotekSehat.pdf)

  [ KIRIM DOKUMEN UNTUK DIVERIFIKASI ADMIN ]
================================================================================
```

---

## 2. Dashboard Apotek
**Tujuan:** Melihat antrean pesanan logistik yang belum diproses.

```text
================================================================================
[ HEADER: Portal Farmasi ]                             [ Status Apotek: BUKA ]
================================================================================
[ SIDEBAR ] | [ KONTEN UTAMA ]
- Dasbor    |  Halo, Apotek Sehat Mandiri!
- Stok Obat |  +-------------------------------------------------------------+
- Pesanan   |  | Antrean Pesanan Obat Masuk: 3 Antrean     [ LIHAT PESANAN ] |
- Dompet    |  | Saldo Penjualan Terkini   : Rp 2.500.000  [ TARIK DANA ]    |
            |  +-------------------------------------------------------------+
            |  
            |  >> Riwayat Pesanan Selesai Hari Ini:
            |  +-------------------------------------------------------------+
            |  | #ORD-5540 | Budi S. | Dikirim (Biteship: BTS-123) | Sukses  |
            |  | #ORD-5539 | Andi M. | Dikirim (Biteship: BTS-001) | Sukses  |
            |  +-------------------------------------------------------------+
================================================================================
```

---

## 3. Manajemen Inventaris (Stok Obat)
**Tujuan:** Antarmuka pengubahan kuantitas stok sediaan obat (Harga terkunci jika diatur oleh superadmin).

```text
================================================================================
[ KONTEN UTAMA: Manajemen Inventaris ]

[ Cari Nama Obat...           ]

+------------------------------------------------------------------------------+
| Nama Obat         | Kategori    | Stok Saat Ini | Harga Obat  | Aksi         |
+------------------------------------------------------------------------------+
| Orlistat 120mg    | Obat Keras  | [ 50 ] Box    | Rp 100.000  | [ UPDATE ]   |
| Vitamin B Complex | Suplemen    | [ 10 ] Strip  | Rp  20.000  | [ UPDATE ]   |
| Phentermine 15mg  | Psikotropika| [  0 ] Box    | Rp 150.000  | (BLOCKED)    |
+------------------------------------------------------------------------------+
*Catatan: Sediaan Narkotika & Psikotropika tidak diizinkan diresepkan di sistem ini.
================================================================================
```

---

## 4. Manajemen Pemenuhan (Fulfillment)
**Tujuan:** Menampilkan daftar pesanan yang transaksinya (Tagihan 2) sudah dibayar oleh pasien.

```text
================================================================================
[ KONTEN UTAMA: Daftar Pesanan Baru ]

Filter: [ Menunggu Diproses v ]

+------------------------------------------------------------------------------+
| ID Pesanan  | Pasien   | Tanggal | Resep Dokter         | Aksi               |
+------------------------------------------------------------------------------+
| #ORD-5541   | Budi S.  | 12 Okt  | [ UNDUH PDF RESEP ]  | [ PROSES PACKING ] |
| #ORD-5542   | Siti A.  | 12 Okt  | [ UNDUH PDF RESEP ]  | [ PROSES PACKING ] |
+------------------------------------------------------------------------------+
================================================================================
```

---

## 5. Pengemasan & Logistik (Mandatory Checklist)
**Tujuan:** Halaman atau Modal yang memaksa kepatuhan hukum (*Checklist*) sebelum memanggil API kurir logistik (Biteship).

```text
================================================================================
[ KONTEN UTAMA: Pengemasan Order #ORD-5541 ]

Pasien: Budi Santoso | Resep Dokter: [Unduh E-Prescription PDF]
Obat yang harus disiapkan ke dalam kotak pengiriman:
1. Orlistat 120mg (1 Box)
2. Vitamin B Complex (1 Strip)

>> Mandatory Checklist (Kepatuhan Hukum & Logistik):
[ ] Obat telah disiapkan dan diverifikasi kelayakannya sesuai dengan Resep PDF.
[ ] Kemasan kotak dalam keadaan tertutup rapat.
[ ] Kemasan menggunakan material tidak tembus pandang (menjaga privasi medis).
[ ] Segel Keras (Cable Tie/Seal) anti-rusak telah dipasang di luar paket.

*(Tombol di bawah hanya akan aktif setelah semua checklist dicentang)*
[ REQUEST PICKUP KURIR (CETAK RESI BATESHIP) ] 
================================================================================
```

---

## 6. Partner Wallet (Dompet Apotek)
**Tujuan:** Mengelola perputaran arus kas dan mencairkan uang transaksi obat.

```text
================================================================================
[ KONTEN UTAMA: Saldo & Pencairan Dana ]

Saldo Tersedia: Rp 2.500.000

Rekening Tujuan (Telah Divalidasi API Inquiry):
Bank: BNI
Nomor: 0987654321
Atas Nama: PT APOTEK SEHAT MANDIRI
[ Ubah Rekening ]

>> Penarikan Dana (Withdrawal)
Nominal: [ Rp 2.500.000 ]
[ TARIK DANA SEKARANG ]
================================================================================
```
