# 10. Robustness Diagram (Preliminary Design)

Dokumen ini memuat **Robustness Diagram** sebagai bagian dari tahap *Analisis dan Desain Awal* (Preliminary Design) pada metodologi ICONIX Process. Diagram ini menjembatani fungsionalitas (*Use Case*) dengan rancangan teknis (*Sequence & Class Diagram*) dengan cara membedah skenario ke dalam tiga jenis objek:
1. **Boundary Object**: Antarmuka pengguna (contoh: Halaman Web, Form).
2. **Controller Object**: Fungsi logika bisnis atau aturan pemrosesan.
3. **Entity Object**: Entitas data dari *Domain Model* yang disimpan di basis data.

Aturan utama pemodelan (*noun-verb-noun*):
- *Boundary* tidak boleh berkomunikasi langsung dengan *Entity*. Harus melalui *Controller*.
- *Actor* hanya boleh berinteraksi dengan *Boundary*.

---

## 10.1. UC-01: Melakukan Checkout Pesanan

Diagram ini membedah skenario pelanggan yang menyelesaikan pembelian, memilih kurir, dan menuju pembayaran.

```mermaid
flowchart TD
    %% Definisi Bentuk Objek
    classDef actor fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef boundary fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef controller fill:#fff3e0,stroke:#f57c00,stroke-width:2px,shape:rect,rx:20,ry:20;
    classDef entity fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,shape:cylinder;

    %% Objek
    A((Customer)):::actor
    B1[/Halaman Keranjang/]::boundary
    B2[/Halaman Checkout/]::boundary
    B3[/Midtrans Snap Gateway/]::boundary
    
    C1(Muat Form Checkout):::controller
    C2(Kalkulasi Ongkir Biteship):::controller
    C3(Proses Pesanan):::controller
    
    E1[(CartItem)]:::entity
    E2[(Address)]:::entity
    E3[(Order)]:::entity

    %% Interaksi
    A -->|Klik Lanjut Pembayaran| B1
    B1 --> C1
    C1 -->|Baca Data| E1
    C1 -->|Tampilkan| B2
    
    A -->|Pilih Alamat Lengkap| B2
    B2 --> C2
    C2 -->|Validasi| E2
    C2 -.->|Panggil API| Biteship[API Biteship]
    C2 -->|Tampilkan Tarif| B2
    
    A -->|Klik Bayar Sekarang| B2
    B2 --> C3
    C3 -->|Simpan Transaksi| E3
    C3 -->|Redirect| B3
```

**Validasi Desain (Disambiguation):**
- Objek `CartItem`, `Address`, dan `Order` terkonfirmasi ada di Domain Model.
- Dibutuhkan fungsi *Controller* spesifik `Kalkulasi Ongkir Biteship` yang menghubungkan form alamat dengan respon tarif.

---

## 10.2. UC-02: Mengajukan Penarikan Komisi (Withdrawal)

Diagram ini membedah skenario Mitra Afiliasi yang menarik saldo komisi mereka.

```mermaid
flowchart TD
    classDef actor fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef boundary fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef controller fill:#fff3e0,stroke:#f57c00,stroke-width:2px,shape:rect,rx:20,ry:20;
    classDef entity fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,shape:cylinder;

    %% Objek
    A((Mitra Afiliasi)):::actor
    B1[/Dasbor Afiliasi/]::boundary
    B2[/Form Penarikan Dana/]::boundary
    
    C1(Validasi Saldo & Limit):::controller
    C2(Simpan Pengajuan):::controller
    C3(Notifikasi Admin):::controller
    
    E1[(AffiliateCommission)]:::entity
    E2[(PayoutRequest)]:::entity

    %% Interaksi
    A -->|Klik Tarik Komisi| B1
    B1 --> C1
    C1 -->|Hitung Total| E1
    C1 -->|Jika >= Rp100.000| B2
    
    A -->|Input Nominal & Submit| B2
    B2 --> C2
    C2 -->|Buat Record| E2
    C2 --> C3
    C3 -.->|Kirim Alert| AdminSystem[Sistem Admin]
```

**Validasi Desain (Disambiguation):**
- Objek `AffiliateCommission` dan `PayoutRequest` terkonfirmasi ada di Domain Model.
- Fungsi *Controller* mendeteksi kondisi alternatif: "Jika Saldo Tidak Mencukupi, Form Penarikan tidak akan ditampilkan".

---

## 10.3. UC-03: Menyetujui Konten Produk Obat (BPOM)

Diagram ini membedah alur persetujuan (*approval flow*) konten oleh Admin Compliance.

```mermaid
flowchart TD
    classDef actor fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef boundary fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef controller fill:#fff3e0,stroke:#f57c00,stroke-width:2px,shape:rect,rx:20,ry:20;
    classDef entity fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,shape:cylinder;

    %% Objek
    A((Admin Compliance)):::actor
    B1[/Daftar Draf Produk/]::boundary
    B2[/Detail Draf Produk/]::boundary
    
    C1(Ambil Data Draf):::controller
    C2(Proses Persetujuan):::controller
    C3(Proses Penolakan & Catatan):::controller
    
    E1[(Product)]:::entity

    %% Interaksi
    A -->|Akses Halaman| B1
    B1 --> C1
    C1 -->|Baca Status=Draft| E1
    C1 -->|Tampilkan Detail| B2
    
    A -->|Review Konten| B2
    B2 -->|Klik Setujui| C2
    C2 -->|Update Status=Published| E1
    
    B2 -->|Klik Tolak + Catatan| C3
    C3 -->|Update Status=Revisi| E1
```

**Validasi Desain (Disambiguation):**
- Objek `Product` memiliki atribut tersirat berupa `status` (Draft/Published/Revisi) dan `rejection_note` yang harus ditambahkan pada tahap penyempurnaan *Class Diagram* nantinya.
- Alur ini memisahkan secara jelas kontroler persetujuan (*Approve*) dan penolakan (*Reject*).
