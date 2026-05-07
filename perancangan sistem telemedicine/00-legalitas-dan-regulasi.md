# Regulasi dan Legalitas Platform Telemedicine

Memiliki **Tanda Daftar Penyelenggara Sistem Elektronik (TDPSE)** merupakan landasan legalitas yang tepat untuk memulai operasional platform digital. Berdasarkan regulasi kesehatan dan peredaran obat di Indonesia, khususnya ketentuan dari Kementerian Kesehatan dan BPOM, berikut adalah panduan teknis dan operasional:

## 1. Tata Cara Kerja Sama dengan Dokter
Sebagai penyelenggara sistem elektronik, platform bertindak sebagai fasilitator yang mempertemukan pasien dengan tenaga medis.

* **Legalitas Dokter:** Perlu dipastikan bahwa setiap dokter yang bergabung dalam sistem memiliki **Surat Tanda Registrasi (STR)** dan **Surat Izin Praktik (SIP)** yang masih aktif. SIP ini umumnya berafiliasi pada Fasilitas Pelayanan Kesehatan fisik tempat dokter tersebut berpraktik.
* **Bentuk Kerja Sama:** Perjanjian Kerja Sama dapat dilakukan secara langsung antar entitas bisnis (dengan klinik atau rumah sakit tempat dokter bernaung) maupun melalui kemitraan individual dengan dokter yang bersangkutan, bergantung pada model bisnis yang diterapkan.
* **Alur Penapisan Pasien:** Penerapan tahapan penapisan (screening) wajib pada awal alur aplikasi sangat direkomendasikan. Fitur ini memastikan pasien diarahkan ke halaman dokter umum atau dokter spesialis yang paling sesuai dengan keluhan medisnya sebelum proses konsultasi dimulai.
* **Metode Konsultasi (Rich Chat & Video Call):** Berdasarkan regulasi Kemenkes dan KKI, video call tidak bersifat wajib secara hukum nasional. Oleh karena itu, **sebagai standar operasional layanan (SOP) platform ini, sesi konsultasi utama dilakukan melalui *chat* multimedia (teks, pesan suara, gambar, video) antara pasien dan dokter.** Untuk mendukung kelancaran komunikasi dan bila dibutuhkan pemeriksaan visual yang lebih spesifik, platform menyediakan fitur bagi dokter untuk mengajak (menginisiasi) *video call* secara opsional langsung dari dalam antarmuka aplikasi.
* **Proses Peresepan Obat (E-Prescription):** Jika dari hasil anamnesis interaktif (*video call* dibantu *chat*) dokter menyimpulkan pasien membutuhkan obat, dokter berwenang menerbitkan resep elektronik. Resep ini diterbitkan oleh dokter, bukan diminta secara sepihak oleh pasien. Segala tindakan peresepan wajib dicatat dalam Rekam Medis Elektronik (RME). **Catatan Penting:** Regulasi melarang peresepan obat golongan narkotika, psikotropika, dan sediaan injeksi melalui layanan telemedisin.

## 2. Kerja Sama dengan Apotek Mitra
Kerja sama dengan apotek yang sudah memiliki legalitas penuh sangat dimungkinkan tanpa harus mendirikan dan mengurus Izin Apotek secara mandiri. Model bisnis agregator ini lazim diimplementasikan oleh penyelenggara telemedisin skala besar.

* **Kedudukan Platform:** Platform elektronik murni berkedudukan sebagai Penyelenggara Sistem Elektronik (PSE) yang menjembatani penerusan resep dokter kepada apotek mitra.
* **Syarat Apotek Mitra:** Apotek yang menjadi mitra kerja sama diwajibkan mengantongi **Surat Izin Apotek (SIA)** yang sah serta dikelola oleh **Apoteker Penanggung Jawab Apotek (APA)** dengan **Surat Izin Praktik Apoteker (SIPA)** yang aktif.
* **Kewajiban Profesi Apoteker:** Meskipun transaksi pemesanan diproses melalui aplikasi, kewenangan profesional untuk memvalidasi resep, meracik, dan menyerahkan obat mutlak berada pada apoteker di apotek mitra tersebut.

## 3. Alur Pengiriman Obat Keras dan Kerja Sama Ekspedisi
Pengiriman obat keras (berlogo lingkaran merah dengan huruf K) yang diresepkan secara daring diatur secara ketat dengan mengacu pada Peraturan BPOM mengenai peredaran obat secara daring.

* **Pemilihan Jasa Ekspedisi:** Pengiriman dapat menggunakan jasa ekspedisi umum (seperti JNE, J&T, maupun layanan kurir instan) atau kurir internal. Tidak terdapat kewajiban untuk menggunakan instansi logistik tertentu.
* **Perizinan Ekspedisi:** Perusahaan ekspedisi tidak diwajibkan memiliki izin khusus untuk membawa obat. Namun demikian, tanggung jawab penuh atas keamanan mutu produk selama pengiriman tetap berada pada pihak apotek dan penyelenggara platform.

**Alur Pengiriman Obat Keras yang Sesuai Regulasi:**

1. **Validasi Resep:** Resep elektronik dari sistem aplikasi masuk ke pangkalan data apotek mitra untuk divalidasi keabsahannya oleh apoteker.
2. **Pengemasan Tertutup (Wajib):** Apoteker menyiapkan sediaan obat dan mengemasnya dalam wadah yang tertutup rapat serta tidak tembus pandang demi menjaga kerahasiaan medis pasien. Kemasan terluar tidak diperkenankan menampilkan rincian nama obat (cukup memuat nama pasien, alamat tujuan, dan instruksi penyimpanan dasar).
3. **Segel Keamanan:** Paket pengiriman wajib disegel khusus untuk memastikan isi tidak dibuka atau dimanipulasi oleh pihak yang tidak berkepentingan selama masa transit.
4. **Pemberian Edukasi:** Apoteker wajib melampirkan etiket tertulis di dalam paket mengenai aturan pakai obat, serta menyediakan akses komunikasi langsung melalui platform bagi pasien yang membutuhkan penjelasan lebih lanjut.
5. **Pelacakan dan Serah Terima:** Modul Order & Pengiriman pada arsitektur sistem dapat dimaksimalkan untuk memantau status pergerakan kurir secara seketika (real-time). Paket obat keras wajib diserahkan secara langsung kepada pasien yang identitasnya tertera pada resep, atau kepada perwakilan pasien yang berwenang.
