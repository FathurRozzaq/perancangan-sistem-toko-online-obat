# 5. Architecture (Sequence Diagram)

Dokumen ini memuat *Sequence Diagram* untuk memvisualisasikan interaksi teknis antar komponen dalam sistem Telemedicine. Pendekatan arsitektur yang digunakan adalah **Modular Monolithic** berbasis standar desain **MVC (*Model-View-Controller*)** dengan kerangka kerja Laravel dan Livewire.

Partisipan standar yang digunakan pada diagram ini meliputi:
1. **View**: Merepresentasikan lapisan *Frontend* (Komponen *Blade* atau *Livewire*).
2. **Controller**: Merepresentasikan pengendali permintaan HTTP dan pusat logika (*Business Logic/Service*).
3. **Model**: Merepresentasikan *Entity* basis data (*Eloquent ORM*).
4. **Eksternal API**: Layanan pihak ketiga (*Payment Gateway*, *WhatsApp*, dll).

---

## 5.1. Pendaftaran & Verifikasi KYC
Diagram ini memvisualisasikan alur pendaftaran pengguna, mulai dari Pasien yang mendaftar via WhatsApp hingga Dokter/Apotek yang mengunggah dokumen legalitas (KYC) dan persetujuan Admin.

```mermaid
sequenceDiagram
    autonumber
    actor Pasien
    actor Medis as Dokter/Apotek
    participant View as View (Livewire)
    participant Controller as Controller
    participant Model as Model (Entity)
    participant WA as WhatsApp API
    
    %% Alur Pasien
    Pasien->>View: Input Nama & No. WhatsApp
    View->>Controller: POST /register
    Controller->>Controller: Validasi Input
    Controller->>Model: User::create()
    Model-->>Controller: Return User Instance
    Controller->>WA: Request OTP/Welcome Message
    WA-->>Pasien: Pesan Selamat Datang
    Controller-->>View: Redirect ke Dasbor Pasien
    
    %% Alur Medis (Dokter/Apotek)
    Medis->>View: Input Data & Upload STR/SIP/SIA
    View->>Controller: POST /register-partner
    Controller->>Controller: Validasi Dokumen
    Controller->>Model: User::create() & PartnerProfile::create(status: Pending)
    Model-->>Controller: Return Entity
    Controller-->>View: Status Menunggu Verifikasi
    
    %% Proses Admin
    actor Admin
    Admin->>View: Buka Dasbor KYC
    View->>Controller: GET /admin/kyc-pending
    Controller->>Model: PartnerProfile::where('status', 'Pending')
    Model-->>Controller: Return Data
    Controller-->>View: Tampilkan List Pending
    Admin->>View: Klik Approve (Setujui)
    View->>Controller: POST /admin/kyc-approve/{id}
    Controller->>Model: update(status: Active)
    Model-->>Controller: Success
    Controller->>WA: Notifikasi Akun Aktif
    WA-->>Medis: "Akun Anda telah Aktif"
    Controller-->>View: Refresh List
```

---

## 5.2. Alur Utama: Pemesanan, Transaksi Ganda (*Double Billing*), dan RME
Diagram *End-to-End* dari tahap Pasien memilih jadwal hingga terbitnya resep elektronik.

```mermaid
sequenceDiagram
    autonumber
    actor Pasien
    participant View as View (Livewire)
    participant Controller as Controller
    participant Model as Model (Entity)
    participant PG as Midtrans (Payment Gateway)
    participant Jitsi as Jitsi API
    actor Dokter
```

---

```mermaid
sequenceDiagram
    autonumber
    participant PG as Midtrans (Payment Gateway)
    participant Controller as Controller
    participant Model as Model (Entity)
    actor Apotek
    participant View as View (Livewire)
    participant Biteship as Biteship (Logistik API)
    actor Pasien
    
    %% Pembayaran Obat Selesai
    Pasien->>PG: Bayar Tagihan 2
    PG-->>Controller: Webhook: Tagihan 2 Lunas
    Controller->>Model: Invoice::update(Paid) & PharmacyOrder::create()
    
    %% Proses Apotek
    Apotek->>View: Buka Daftar Pesanan
    View->>Controller: GET /pharmacy/orders
    Controller->>Model: PharmacyOrder::where('status', 'Pending')
    Model-->>Controller: Order List
    Controller-->>View: Render Data Pesanan
    Apotek->>View: Centang Checklist Kemasan (Mandatory)
    View->>Controller: POST /pharmacy/pack
    Controller->>Model: PharmacyOrder::update(Packed)
    Model-->>Controller: Success
    Controller-->>View: Enable Tombol Request Pickup
    
    %% Pengiriman
    Apotek->>View: Klik Request Pickup
    View->>Controller: POST /pharmacy/ship
    Controller->>Biteship: Create Order (AWB Request)
    Biteship-->>Controller: AWB Number & Tracking ID
    Controller->>Model: Shipment::create(AWB)
    Model-->>Controller: Success
    Controller-->>View: Tampilkan Resi Cetak
    
    %% Pelacakan
    Biteship-->>Controller: Webhook: Update Status Pengiriman
    Controller->>Model: Shipment::update(Status)
    Controller->>WA: Trigger Pesan Notifikasi Kurir
    WA-->>Pasien: Notifikasi WA Status Paket
```

---

## 5.4. Kalkulasi Bagi Hasil & Pencairan Dana (Payout)
Diagram pengelolaan keuangan otomatis untuk distribusi komisi ke dompet mitra.

```mermaid
sequenceDiagram
    autonumber
    participant Cron as Background Job
    participant Controller as Controller
    participant Model as Model (Entity)
    actor Mitra as Dokter/Apotek
    participant View as View (Livewire)
    participant PG as Midtrans (Payment Gateway)
    
    %% Proses Bagi Hasil
    Cron->>Controller: Trigger Settlement(Order Selesai)
    Controller->>Model: CommissionSetting::all()
    Model-->>Controller: Skema Komisi
    Controller->>Controller: Kalkulasi Porsi Platform vs Mitra
    Controller->>Model: PartnerWallet::addBalance()
    
    %% Input Rekening & Pencairan
    Mitra->>View: Input Rekening Bank
    View->>Controller: POST /wallet/bank
    Controller->>PG: Midtrans Account Inquiry API
    PG-->>Controller: Account Holder Name
    Controller->>Model: PartnerProfile::updateBank()
    Controller-->>View: Tampilkan Nama Pemilik Valid
    
    Mitra->>View: Request Withdrawal (Penarikan)
    View->>Controller: POST /wallet/withdraw
    Controller->>Model: Wallet::deductBalance() & Withdrawal::create()
    Model-->>Controller: Withdrawal Instance
    Controller->>PG: Midtrans Disbursement API (Transfer)
    PG-->>Controller: Webhook: Transfer Berhasil
    Controller->>Model: Withdrawal::update(Success)
```
