# 11. Sequence Diagram (Detailed Design)

Dokumen ini memuat **Sequence Diagram** yang merupakan bagian dari tahap *Desain Detail* pada metodologi ICONIX Process. Diagram ini menerjemahkan interaksi konseptual dari *Robustness Diagram* menjadi spesifikasi teknis yang mengalokasikan fungsi (perilaku) ke objek-objek spesifik. 

Mengingat sistem ini akan dibangun di atas ekosistem **Bagisto (Laravel + Vue.js)**, diagram ini memodelkan objek *Controller* sebagai *Laravel Controller/Service* dan *Entity* sebagai *Eloquent Model*, lengkap dengan pemanggilan API pihak ketiga.

---

## 11.1. UC-01: Melakukan Checkout Pesanan

Diagram ini mendetailkan proses perhitungan ongkos kirim dinamis melalui API Biteship dan pengalihan (redirect) ke *payment gateway* Midtrans.

```mermaid
sequenceDiagram
    autonumber
    actor C as Customer
    participant V as CheckoutView (Boundary)
    participant Ctrl as CheckoutController
    participant BAPI as BiteshipService (API)
    participant O as Order (Entity)
    participant MAPI as MidtransService (API)

    C->>V: Pilih Alamat & Lanjut ke Kurir
    V->>Ctrl: getShippingRates(destination_id, weight)
    Ctrl->>BAPI: callBiteshipAPI(payload)
    BAPI-->>Ctrl: return courier_rates (JNE, SiCepat, dll)
    Ctrl-->>V: renderRateOptions()
    
    C->>V: Pilih Kurir & Klik "Bayar Sekarang"
    V->>Ctrl: placeOrder(cart_data, payment_method)
    
    %% Alokasi Perilaku: Simpan Pesanan
    Ctrl->>O: save()
    O-->>Ctrl: order_id
    
    %% Integrasi Midtrans
    Ctrl->>MAPI: getSnapToken(order_id, gross_amount)
    MAPI-->>Ctrl: return snap_token
    
    Ctrl-->>V: redirect to Midtrans Snap URL
    C->>V: Lakukan Pembayaran di halaman Snap
```

---

## 11.2. UC-02: Mengajukan Penarikan Komisi (Withdrawal)

Diagram ini mengilustrasikan logika validasi batas minimum penarikan (*Minimum Payout*) dan pembuatan rekam (*record*) pengajuan.

```mermaid
sequenceDiagram
    autonumber
    actor A as Mitra Afiliasi
    participant V as AffiliateDashboardView
    participant Ctrl as PayoutController
    participant AC as AffiliateCommission (Entity)
    participant PR as PayoutRequest (Entity)
    
    A->>V: Akses Halaman Penarikan
    V->>Ctrl: showWithdrawalForm()
    
    %% Alokasi Perilaku: Validasi Saldo
    Ctrl->>AC: getTotalBalance(affiliate_id)
    AC-->>Ctrl: total_balance
    
    alt total_balance >= 100.000
        Ctrl-->>V: displayWithdrawalForm(total_balance)
        
        A->>V: Input Nominal & Submit Form
        V->>Ctrl: submitRequest(amount, bank_details)
        
        %% Alokasi Perilaku: Buat Request
        Ctrl->>PR: create(status='Pending Verification')
        PR-->>Ctrl: request_id
        Ctrl-->>V: redirectWithMessage("Pengajuan Berhasil")
        
    else total_balance < 100.000
        Ctrl-->>V: redirectWithError("Saldo tidak mencukupi")
    end
```

---

## 11.3. UC-03: Menyetujui Konten Produk Obat (BPOM)

Diagram ini menjabarkan *Approval Flow* dengan penanganan skenario alternatif (Penolakan produk karena *overclaim*).

```mermaid
sequenceDiagram
    autonumber
    actor AC as Admin Compliance
    participant V as AdminProductView
    participant Ctrl as ProductApprovalController
    participant P as Product (Entity)
    
    AC->>V: Buka Detail Draf Produk
    V->>Ctrl: show(product_id)
    Ctrl->>P: find(product_id)
    P-->>Ctrl: product_data
    Ctrl-->>V: renderProductDetails()
    
    AC->>V: Review Konten
    
    alt Keputusan: Setujui
        AC->>V: Klik "Approve"
        V->>Ctrl: approve(product_id)
        %% Alokasi Perilaku: Ubah status menjadi Live
        Ctrl->>P: update(status='published', rejection_note=null)
        Ctrl-->>V: showSuccess("Produk dipublikasikan")
        
    else Keputusan: Tolak
        AC->>V: Klik "Reject" & Isi Catatan
        V->>Ctrl: reject(product_id, note)
        %% Alokasi Perilaku: Kembalikan ke Staf dengan Catatan
        Ctrl->>P: update(status='revision', rejection_note=note)
        Ctrl-->>V: showSuccess("Draf dikembalikan untuk revisi")
    end
```

---
**Pembaruan Domain Model yang Teridentifikasi (Discovery):**
Dari pembedahan di atas, kita mengidentifikasi atribut dan metode baru yang harus ditambahkan ke rancangan Class Diagram di tahap berikutnya:
- Entitas `Order`: Fungsi `save()`
- Entitas `AffiliateCommission`: Fungsi `getTotalBalance()`
- Entitas `PayoutRequest`: Fungsi `create()`, dan atribut `status`
- Entitas `Product`: Atribut `status` (berisi *published*, *draft*, *revision*) dan `rejection_note`. Fungsi `update()`.
