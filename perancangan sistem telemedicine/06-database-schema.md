# 6. Database Schema (Entity Relationship Diagram)

Dokumen ini mendefinisikan struktur penyimpanan data (*database*) untuk sistem Telemedicine berbasis *Relational Database* (MySQL/PostgreSQL).
Karena sistem ini menaungi proses yang kompleks, struktur tabel dikelompokkan menjadi 6 sub-sistem/domain yang saling berinteraksi guna memudahkan pemahaman desain (*Maintainability*).

---

## 6.1. Diagram Korelasi Antar Sub-sistem (High-Level)
Diagram ini memberikan gambaran besar bagaimana ke-6 domain basis data terhubung dan berinteraksi dalam satu kesatuan sistem.

```mermaid
flowchart TD
    %% Define Sub-sistem
    A((1. Pengguna & KYC))
    B((2. Layanan & Pemesanan))
    C((3. Medis & Resep))
    D((4. Inventaris & Logistik))
    E((5. Finansial & Dompet))
    F((6. Kepatuhan PSE & Chat))

    %% Relasi Alur Data
    A -->|Melakukan Booking| B
    B -->|Menghasilkan Tagihan| E
    B -->|Memicu Konsultasi| C
    C -->|Menerbitkan Resep Obat| D
    D -->|Memicu Tagihan Obat| E
    A -->|Dilaporkan UGC| F
    A -->|Komunikasi via| F
    
    style A fill:#e1f5fe,stroke:#0288d1,stroke-width:2px
    style B fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style C fill:#e8f5e9,stroke:#388e3c,stroke-width:2px
    style D fill:#fce4ec,stroke:#c2185b,stroke-width:2px
    style E fill:#fff8e1,stroke:#fbc02d,stroke-width:2px
    style F fill:#ede7f6,stroke:#512da8,stroke-width:2px
```

---

## 6.2. ERD Domain 1: Pengguna, Profil & Legalitas (KYC)
Sub-sistem ini mengelola autentikasi pengguna beserta detail profil medis. Termasuk pula penyimpanan dokumen legalitas (KYC) milik Dokter dan Apotek yang diperlukan untuk verifikasi operasional.

```mermaid
erDiagram
    users {
        bigint id PK
        string name
        string email UK
        string whatsapp_number UK
        string password
        enum role "patient, doctor, pharmacy, admin, superadmin"
        enum status "active, pending, suspended"
        timestamp created_at
    }

    patient_profiles {
        bigint id PK
        bigint user_id FK
        date dob "Tanggal Lahir"
        enum gender "M, F"
        text address
        string emergency_contact
        enum blood_type "A, B, AB, O"
    }

    partner_profiles {
        bigint id PK
        bigint user_id FK
        string str_number "Nomor STR Dokter"
        string sip_number "Nomor SIP Dokter"
        string sia_number "Nomor SIA Apotek"
        string sipa_number "Nomor SIPA Apoteker"
        string specialization
        string kyc_document_url
        enum kyc_status "pending, approved, rejected"
    }

    users ||--o| patient_profiles : "memiliki (has)"
    users ||--o| partner_profiles : "memiliki (has)"
```

---

## 6.3. ERD Domain 2: Katalog, Jadwal & Skrining (Anamnesis)
Sub-sistem yang menyimpan master data layanan penurunan berat badan, ketersediaan jadwal dokter, proses reservasi (*booking*), serta kuesioner medis pasien (Anamnesis).

```mermaid
erDiagram
    service_packages {
        bigint id PK
        string name
        text description
        decimal price_consultation
        decimal price_medicine
        boolean is_active
    }

    doctor_schedules {
        bigint id PK
        bigint doctor_id FK "users.id"
        date schedule_date
        time start_time
        time end_time
        boolean is_booked
    }

    bookings {
        bigint id PK
        bigint patient_id FK "users.id"
        bigint doctor_schedule_id FK
        bigint service_package_id FK
        enum status "pending, paid, cancelled, completed"
        timestamp created_at
    }

    anamnesis_records {
        bigint id PK
        bigint booking_id FK
        bigint patient_id FK "users.id"
        json questions_answers "Format kuesioner dinamis"
        enum screening_status "passed, warning, blocked"
    }

    service_packages ||--o{ bookings : "dipesan dalam (ordered in)"
    doctor_schedules ||--o| bookings : "direservasi oleh (reserved by)"
    bookings ||--o| anamnesis_records : "mewajibkan (requires)"
```

---

## 6.4. ERD Domain 3: Rekam Medis Elektronik (RME) & Resep Digital
Sub-sistem klinis yang menangani sesi ruang virtual, form RME/SOAP, serta pembuatan entitas Resep Elektronik (*E-Prescription*) ber-TTE.

```mermaid
erDiagram
    consultations {
        bigint id PK
        bigint booking_id FK
        string jitsi_room_url
        timestamp start_time
        timestamp end_time
        enum status "scheduled, ongoing, finished"
    }

    electronic_medical_records {
        bigint id PK
        bigint consultation_id FK
        bigint doctor_id FK "users.id"
        text subjective "S - Keluhan pasien"
        text objective "O - Hasil pemeriksaan klinis"
        text assessment "A - Diagnosis"
        text plan "P - Rencana tindakan"
        timestamp retention_date "Wajib retensi 25 tahun"
    }

    e_prescriptions {
        bigint id PK
        bigint consultation_id FK
        bigint doctor_id FK "users.id"
        text notes
        string pdf_url
        string tte_signature "Tanda Tangan Elektronik"
        timestamp issued_at
    }

    prescription_items {
        bigint id PK
        bigint prescription_id FK
        bigint medicine_id FK
        string dosage "Dosis pemakaian"
        string instructions "Instruksi khusus"
        integer quantity
    }

    consultations ||--o| electronic_medical_records : "dicatat dalam (recorded in)"
    consultations ||--o| e_prescriptions : "menghasilkan (generates)"
    e_prescriptions ||--|{ prescription_items : "memuat (contains)"
```

---

## 6.5. ERD Domain 4: Inventaris & Logistik (Fulfillment)
Sub-sistem pengelolaan ketersediaan obat (Farmasi/Apotek) fisik, manajemen pengemasan (*checklist* regulasi obat keras), hingga pelacakan pengiriman kurir.

```mermaid
erDiagram
    medicines {
        bigint id PK
        bigint pharmacy_id FK "users.id"
        string name
        text description
        decimal price
        integer stock
        boolean is_hard_drug "Flag Obat Keras"
    }

    pharmacy_orders {
        bigint id PK
        bigint booking_id FK
        bigint prescription_id FK
        bigint pharmacy_id FK "users.id"
        boolean is_packaging_compliant "Checklist Segel Keras"
        enum status "pending, packing, ready, shipping, delivered"
    }

    shipments {
        bigint id PK
        bigint pharmacy_order_id FK
        string courier_name
        string awb_number "Nomor Resi Biteship"
        string tracking_url
        enum status "in_transit, delivered, returned"
    }

    medicines ||--o{ prescription_items : "direferensikan (referenced in)"
    e_prescriptions ||--o| pharmacy_orders : "diproses oleh (fulfilled by)"
    pharmacy_orders ||--o| shipments : "dikirim via (shipped via)"
```

---

## 6.6. ERD Domain 5: Finansial (Billing, Voucher, Payout)
Pusat perputaran uang. Mengelola tagihan berantai (*Double Billing*), integrasi pembayaran (Webhook), voucher promosi, kalkulasi rasio komisi platform, hingga riwayat dompet dan penarikan tunai mitra.

```mermaid
erDiagram
    invoices {
        bigint id PK
        bigint booking_id FK
        bigint user_id FK
        enum type "consultation, medicine"
        bigint voucher_id FK "Nullable"
        decimal discount_amount
        decimal total_amount
        enum payment_status "unpaid, paid"
        string payment_url
    }

    payments {
        bigint id PK
        bigint invoice_id FK
        string payment_method
        string external_pg_id "Midtrans/Xendit ID"
        decimal amount
        timestamp paid_at
    }

    vouchers {
        bigint id PK
        string code UK
        enum discount_type "percentage, fixed"
        decimal discount_value
        enum applies_to "consultation, medicine"
        integer quota
    }

    partner_wallets {
        bigint id PK
        bigint user_id FK "Doctor/Pharmacy ID"
        decimal balance
        string bank_name
        string bank_account_number
        string account_holder_name
    }

    withdrawals {
        bigint id PK
        bigint user_id FK
        decimal amount
        enum status "pending, processing, success, failed"
        string pg_disbursement_id
    }

    commission_settings {
        bigint id PK
        enum target_role "doctor, pharmacy"
        enum fee_type "percentage, fixed"
        decimal fee_value
    }

    invoices ||--o| payments : "dibayar via (paid via)"
    vouchers ||--o{ invoices : "diterapkan pada (applied to)"
    users ||--o| partner_wallets : "memiliki dompet (owns)"
    partner_wallets ||--o{ withdrawals : "memotong dari (deducts from)"
```

---

## 6.7. ERD Domain 6: Kepatuhan PSE, SLA & Komunikasi
Sub-sistem pencatatan rekam jejak aktivitas sistem (*Audit Trail*) untuk pemenuhan permintaan Aparat Hukum, pelaporan konten ilegal (UGC), serta fungsionalitas perpesanan internal dan notifikasi *nudge*.

```mermaid
erDiagram
    audit_logs {
        bigint id PK
        bigint user_id FK "Nullable (System)"
        string action "Event: create, update, takedown"
        string entity_type "Nama tabel terkait"
        bigint entity_id
        text details "JSON perubahan data"
        string ip_address
        timestamp created_at "Acuan pelaporan PSE"
    }

    user_reports {
        bigint id PK
        bigint reporter_id FK "users.id"
        string reported_content_type "chat_message, etc"
        bigint reported_content_id
        text reason "Alasan laporan konten ilegal"
        enum status "open, reviewed, takedown, dismissed"
    }

    chats {
        bigint id PK
        bigint sender_id FK "users.id"
        bigint receiver_id FK "users.id"
        enum message_type "text, image, voice, video, call_invite"
        text message "Teks atau caption"
        string media_url "URL attachment"
        timestamp read_at
    }

    notifications {
        bigint id PK
        bigint user_id FK
        string type "reminder, sla_warning, billing"
        text message
        boolean is_read
        boolean sent_via_wa "Integrasi WhatsApp Nudge"
    }

    users ||--o{ audit_logs : "melakukan (performs)"
    users ||--o{ user_reports : "melaporkan (submits)"
    users ||--o{ chats : "berkomunikasi (sends/receives)"
    users ||--o{ notifications : "menerima notif (receives)"
```
