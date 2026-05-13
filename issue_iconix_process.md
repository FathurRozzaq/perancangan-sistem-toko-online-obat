# Issue: Implementasi ICONIX Process pada Dokumentasi PRD

## Deskripsi
Dalam rangka meningkatkan kualitas perancangan sistem dan memastikan transisi yang terstruktur dari pendefinisian kebutuhan (*use case*) menuju implementasi kode, dokumentasi PRD saat ini perlu diperluas dengan mengadopsi kerangka kerja **ICONIX Process**. 

Pendekatan *use case-driven* ini akan memperjelas desain arsitektur dan interaksi antar objek sistem.

## Tujuan
Membuat serangkaian dokumen pemodelan UML sesuai fase-fase ICONIX Process yang terintegrasi dan konsisten dengan dokumen *Product Requirements Document* (PRD) yang sudah ada.

## Task List (Sub-Issues)
- [x] **Task 1: Pembuatan Domain Model**
  - Buat dokumen `prd-website-toko-obat-online/08-domain-model.md`.
  - Identifikasi entitas objek dunia nyata (Customer, Order, Affiliate, Product, dll).
  - Buat visualisasi domain model konseptual (tanpa metode).

- [x] **Task 2: Pemodelan Use Case Diagram & Deskripsi**
  - Buat dokumen `prd-website-toko-obat-online/09-use-case-model.md`.
  - Buat Use Case Diagram komprehensif untuk Aktor (Pelanggan, Mitra Afiliasi, Admin).
  - Tuliskan skenario deskriptif (*Basic Course* & *Alternate Courses*) untuk setiap Use Case.

- [x] **Task 3: Analisis Robustness (Preliminary Design)**
  - Buat dokumen `prd-website-toko-obat-online/10-robustness-diagram.md`.
  - Konversi teks skenario Use Case menjadi visualisasi interaksi *Boundary*, *Controller*, dan *Entity* (*noun-verb-noun*).

- [ ] **Task 4: Desain Detail - Sequence Diagram**
  - Buat dokumen `prd-website-toko-obat-online/11-sequence-diagram.md`.
  - Jabarkan interaksi alokasi perilaku antar objek berdasarkan waktu, mempertimbangkan arsitektur MVC (Bagisto/Laravel).

- [ ] **Task 5: Finalisasi Class Diagram**
  - Buat dokumen `prd-website-toko-obat-online/12-class-diagram.md`.
  - Mutakhirkan Domain Model dengan menambahkan atribut data dan metode operasional (fungsi) dari hasil pembedahan Sequence Diagram.

## Kriteria Penerimaan (Acceptance Criteria)
- Kelima dokumen berhasil diselesaikan dan ditempatkan dalam direktori `prd-website-toko-obat-online`.
- Visualisasi UML dibuat menggunakan *Markdown-native tools* (misal: Mermaid.js) atau alat visual (PlantUML/Draw.io) yang terlampir dengan baik.
- Terdapat jejak keterhubungan (*traceability*) yang jelas mulai dari *Domain Model* awal hingga bermuara ke *Class Diagram* teknis.
