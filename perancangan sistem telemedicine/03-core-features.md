# 3. Core Features (MVP)

Bagian ini merangkum fitur-fitur utama yang wajib dikembangkan pada iterasi pertama (Minimum Viable Product / MVP) agar sistem telemedicine dapat beroperasi secara *end-to-end* sesuai dengan *requirements* dan batasan arsitektur.

## 1. Modul Manajemen Pengguna & Otorisasi
- **Registrasi & Autentikasi (WhatsApp Mandatory)**: Fitur pembuatan akun dan *login* yang diwajibkan menggunakan verifikasi **Nomor WhatsApp aktif**. Syarat ini bersifat mutlak agar fitur pengingat eskalasi (*Direct WhatsApp Alert*) dan perlindungan antrean (*Auto Follow-Up*) dapat berjalan.
- **Modul KYC (Know Your Customer) Medis**: Fitur alur penangguhan (*pending approval*) bagi pendaftar peran Dokter (wajib unggah dokumen STR & SIP) dan Apotek (wajib unggah SIA & SIPA) hingga divalidasi keabsahannya oleh Admin/Superadmin.
- **Sistem Role-Based Access Control (RBAC)**: Pembatasan hak akses *dashboard* berdasarkan 6 peran pengguna:
  - **Pasien**: Hanya melihat fitur pemesanan, riwayat, profil sendiri, indikator dasar ketersediaan paket (Tersedia/Habis), serta memiliki alat pelaporan konten (*User Reporting*).
  - **Dokter**: Mengelola jadwal praktik, melakukan konsultasi, menerbitkan resep, mengakses laporan pendapatan pribadi, serta melihat indikator ketersediaan obat (Tersedia/Habis).
  - **Apotek**: Memantau pesanan obat, memvalidasi resep, mengurus pengemasan (termasuk konfirmasi keamanan kemasan) dan logistik, memantau pemasukan, serta menjadi penanggung jawab utama untuk **memperbarui kuantitas stok obat** di sistem.
  - **Admin**: Memantau operasional harian, SLA, memvalidasi dokumen legalitas mitra medis (KYC), melihat ketersediaan stok obat, serta mengirimkan pengingat eskalasi.
  - **Admin Chat**: Menangani komunikasi dan pertanyaan dari Pasien via fitur *chat*.
  - **Superadmin**: Mengakses semua data dan kontrol administratif penuh. Mencakup manajemen keuangan, *bypass* stok darurat, wewenang *takedown* konten, dan **bertindak sebagai Narahubung Hukum** yang berhak mengekspor data audit untuk instansi Penegak Hukum.

## 2. Modul Skrining Klinis (Anamnesis) & Pemesanan
- **Katalog Layanan**: Halaman untuk menampilkan paket-paket layanan penurunan berat badan beserta harganya.
- **Formulir Anamnesis Mandiri**: Fitur pengisian kuesioner medis wajib setiap kali Pasien melakukan *booking*.
- **Otomatisasi Skrining (Smart Routing)**: Sistem validasi aturan bisnis yang menolak atau memberikan peringatan kepada Pasien apabila jawaban anamnesis terindikasi memiliki komorbiditas yang berisiko, mencegah mereka melanjutkan ke pembayaran paket ini.
- **Manajemen Jadwal & *Booking***: Antarmuka bagi Pasien untuk melihat ketersediaan slot waktu Dokter dan melakukan reservasi.
- **Mekanisme Pembatalan Jadwal**:
  - Pasien dapat membatalkan pesanan (sebelum pembayaran/konsultasi).
  - Sistem otomatis membatalkan pesanan jika *invoice* tidak dibayar setelah batas waktu tertentu.
  - Admin/Superadmin memiliki opsi untuk membatalkan jadwal secara manual jika diperlukan (misalnya, dokter berhalangan mendadak).

## 3. Modul Konsultasi Multimedia Terintegrasi
- **Chat Multimedia Interaktif**: Sesi konsultasi utama dilakukan melalui *chat* multimedia (teks, suara, gambar, video) terintegrasi langsung di *browser*. Video meeting bukan merupakan kewajiban bagi setiap sesi konsultasi.
- **Fitur Invite Video Call**: Dokter memiliki wewenang dan akses tombol khusus di dalam ruang konsultasi/chat untuk memanggil dan mengundang pasien agar bergabung ke dalam sesi video (opsional namun tersedia bila dibutuhkan secara klinis).
- **Rich Chat Dokter-Pasien**: Fitur pesan multimedia terintegrasi di mana Pasien dan Dokter dapat saling berkirim teks, pesan suara (voice note), gambar klinis, maupun lampiran video pendek sebelum atau selama sesi berlangsung.
- ***Side-by-side Doctor Dashboard***: Tampilan antarmuka khusus bagi Dokter saat sesi berlangsung, yang menampilkan *streaming* video dan riwayat *chat* di satu sisi, serta data anamnesis dan **Formulir RME (SOAP)** di sisi lain untuk referensi klinis yang cepat.

## 4. Modul Rekam Medis Elektronik (RME)
- **Formulir Catatan Medis (SOAP)**: Antarmuka yang dapat diisi secara asinkron oleh Dokter (baik saat sesi *video call* berlangsung maupun setelah sesi ditutup). Sistem mengunci (disable) form penerbitan E-Prescription sebelum SOAP diselesaikan.
- **Historical Medical Record Dashboard**: Dasbor riwayat yang memfasilitasi Dokter untuk melihat jejak pengobatan terdahulu, serta memfasilitasi Pasien untuk melihat hasil pemeriksaannya dengan batasan akses *read-only* (hanya baca).

## 5. Modul E-Prescription (Resep Digital)
- **Formulir Peresepan Deskriptif**: Antarmuka bagi Dokter untuk memasukkan instruksi pemakaian dan jenis obat keras (tanpa perlu repot mencari kodifikasi ICD) setelah konsultasi selesai. **Fitur ini dilengkapi validasi ketat yang memblokir penulisan resep sediaan Narkotika, Psikotropika, dan Injeksi sesuai regulasi.**
- **Generator Dokumen Resep PDF**: Fitur *background job* yang secara instan meracik *input* peresepan Dokter menjadi sebuah dokumen resmi berformat PDF.
- **Validasi Keaslian Resep (TTE)**: Sistem mengotomatiskan pembubuhan Tanda Tangan Elektronik (TTE) dari Dokter yang bertugas beserta *watermark* atau kode QR unik pada dokumen PDF untuk menjamin legalitas bahwa resep diterbitkan secara sah dan terikat pada sesi konsultasi spesifik.

## 6. Modul Transaksi & Pembayaran (*Double Billing*)
- **Pembuatan Tagihan (Invoicing) Dinamis**: Sistem yang otomatis menerbitkan dua tagihan terpisah dalam satu siklus layanan:
  - Tagihan 1: Biaya jasa konsultasi (memicu aktivasi ruang konsultasi/chat). Ruang konsultasi yang dipicu oleh pembayaran ini hanya akan bisa diakses sesuai dengan jadwal jam *booking* yang telah ditentukan.
  - Tagihan 2: Biaya penebusan obat (memicu proses pemenuhan ke farmasi).
- **Manajemen Penggunaan Voucher**: Antarmuka di halaman pembayaran yang memungkinkan Pasien untuk menerapkan *voucher* diskon yang telah mereka klaim untuk mengurangi total tagihan (sistem memvalidasi kecocokan *voucher* terhadap Tagihan 1 atau Tagihan 2).
- **Integrasi Webhook Payment Gateway**: Koneksi sistem ke penyedia pembayaran (seperti Midtrans/Xendit) untuk otomatis memperbarui status transaksi (*Pending* -> *Paid*) segera setelah Pasien mentransfer dana, tanpa perlu verifikasi manual.

## 7. Modul Pemenuhan (Fulfillment) & Logistik
- ***Forwarding* Pesanan Farmasi**: Mekanisme otomatis yang merangkum data pesanan obat yang sudah dibayar beserta lampiran PDF e-prescription, untuk diteruskan ke dasbor/sistem apotek pusat.
- **Konfirmasi Keamanan Pengemasan**: Fitur *mandatory checklist* di mana Apotek diwajibkan menyetujui bahwa kemasan telah tertutup rapat, tidak tembus pandang, dan disegel khusus (sesuai regulasi obat keras) sebelum tombol "Request Pickup" logistik terbuka.
- **Pembuatan Resi Otomatis (Auto-AWB)**: Integrasi dengan API Agregator Logistik (seperti Biteship) untuk mencetak nomor resi kurir setelah apotek menekan tombol "Dikemas".
- **Pelacakan Pengiriman (Live Tracking)**: Fitur bagi Pasien untuk melacak pergerakan paket obat mereka di dalam aplikasi secara *asinkron*.

## 8. Modul Notifikasi Terpusat
- **Notifikasi Transaksional Asinkron**: Modul berbasis *queue* (seperti Redis) yang bertugas mengirimkan pesan ke Pasien via Email atau integrasi WhatsApp Gateway, mencakup:
  - Tautan tagihan pembayaran (menghindari keterlambatan bayar).
  - Tautan *meeting* dan pengingat waktu (H-1 jam sebelum konsultasi).
  - Pembaruan status pengiriman (nomor resi dan kurir yang bertugas).

## 9. Modul Komunikasi Terpadu (Live Chat & Internal Chat)
- **Customer Support Chat**: Antarmuka *chat* waktu nyata (*real-time*) bagi Pasien untuk menghubungi *Admin Chat*, dilengkapi asisten cerdas (AI Chatbot) sebagai *auto-responder* FAQ.
- **Internal Coordination Chat**: Jalur *chat* khusus yang memungkinkan **Dokter** dan **Apotek** bertukar pesan langsung dengan **Admin** untuk kelancaran koordinasi operasional (misalnya pertanyaan seputar SLA, jadwal, atau kendala logistik).
- **Dasbor Manajemen Chat**: Dasbor khusus terpusat untuk memfasilitasi tim Admin mengelola tiket percakapan, baik yang masuk dari Pasien (eksternal) maupun dari staf medis/logistik (internal).

## 10. Modul Eskalasi & Pemantauan SLA (*Nudge System*)
- **Penetapan Batas Waktu SLA (Service Level Agreement)**: Sistem secara internal memiliki perhitungan *countdown* atau batas waktu penyelesaian tugas operasional (misalnya: durasi toleransi keterlambatan Dokter, batas waktu pengemasan di Apotek, atau kecepatan respon *chat*).
- **Pemantauan SLA Visual**: Fitur indikator penanda otomatis (misalnya berubah warna menjadi merah/kuning) pada dasbor Superadmin dan Admin untuk menyoroti tiket, jadwal, atau pesanan yang terindikasi melanggar tenggat waktu (SLA) yang disepakati.
- **In-App Nudge**: Tombol khusus pada dasbor Admin dan Superadmin untuk mengirimkan notifikasi internal (peringatan sistem) secara instan ke layar pengguna yang bersangkutan (Dokter, Apotek, atau Pasien).
- **Direct WhatsApp Alert**: Ekstensi fungsi pengingat yang memungkinkan Superadmin dan Admin mengetik dan memicu pesan peringatan yang langsung dikirim ke nomor WhatsApp pengguna melalui WhatsApp Gateway tanpa harus berpindah aplikasi.

## 11. Modul Manajemen Keuangan & Pencairan Dana (Payout)
- **Global Finance Dashboard**: Pusat kontrol bagi Superadmin untuk memantau nilai kotor seluruh transaksi (*Gross Merchandise Value*), daftar tagihan terbayar, dan pencatatan riwayat arus kas.
- **Konfigurasi Bagi Hasil (Commission Settings)**: Antarmuka bagi Superadmin untuk mengatur besaran keuntungan platform secara dinamis, baik menggunakan persentase (%) maupun nominal nilai tetap (Rp), untuk diterapkan pada jasa medis Dokter (Tagihan 1) maupun margin penjualan produk Apotek (Tagihan 2).
- **Automated Commission Split**: Kalkulator internal yang berjalan di latar belakang untuk secara otomatis menghitung potongan komisi (berdasarkan *Commission Settings* terbaru) dan mendistribusikan sisa penghasilan bersih ke dompet Dokter dan Apotek setiap kali transaksi selesai.
- **Dompet Mitra & Registrasi Rekening**: Menu khusus bagi Dokter dan Apotek untuk memantau saldo. Di fitur ini mereka diwajibkan mendaftarkan rekening bank pencairan. Modul ini ditenagai **Account Inquiry API** yang mengecek nomor rekening secara otomatis dan mencocokkan nama pemilik aslinya demi menghindari transfer salah sasaran.
- **Sistem Pencairan Fleksibel (Withdrawal)**: Fitur yang memberdayakan Dokter dan Apotek untuk menarik saldo secara mandiri (*self-service*). Modul ini juga dilengkapi fungsi **Force Payout** bagi Superadmin yang memungkinkan transfer dana secara instan ke rekening mitra saat menghadapi kondisi darurat.

## 12. Modul Promosi & Manajemen Voucher
- **Klaim Voucher (Patient App)**: Fitur bagi Pasien untuk mendapatkan *voucher* diskon melalui *input* kode teks manual atau dengan mengeklik tombol *claim* interaktif pada *banner* promosi yang muncul di dalam aplikasi.
- **Konfigurasi Voucher (Superadmin)**: Dasbor pembuatan promosi, di mana Superadmin dapat mengatur tipe diskon (nominal uang/persentase) dan peruntukannya (berlaku hanya untuk Tagihan 1 - Konsultasi, atau Tagihan 2 - Obat).
- **Pengaturan Subsidi Diskon (Discount Split)**: Fitur lanjutan yang menyinkronkan *voucher* dengan modul bagi hasil. Memungkinkan Superadmin untuk menyepakati dan membagi porsi tanggungan biaya promosi. (Misal: Dari *voucher* diskon 10%, diatur agar 7% disubsidi pemotongan komisi platform, dan 3% memotong pendapatan mitra Dokter/Apotek).

## 13. Modul Manajemen Inventaris Obat (Inventory Management)
- **Pembaruan Stok Berjenjang**: Dasbor khusus bagi Apotek untuk memasukkan (*input*) angka alokasi ketersediaan obat untuk platform *web*. Superadmin memiliki menu *bypass* untuk mengatur ulang stok ini sebagai *fallback* saat keadaan darurat, termasuk fungsionalitas untuk menahan (*hold*) dan mematikan seluruh paket layanan secara instan.
- **Visibilitas Komersial Terbatas**: Fitur pembatasan keamanan antarmuka di mana angka detail stok hanya tampil pada dasbor internal logistik/manajemen (Apotek, Admin, Superadmin). Dokter dan Pasien hanya akan melihat pesan "Tersedia" atau "Stok Habis".
- **Stock-Out Prevention & Auto Follow-Up**: Proteksi sistem secara otomatis di antarmuka Pasien. Saat status "Stok Habis", tombol pemesanan akan terkunci sehingga Pasien tidak bisa mengisi formulir anamnesis awal (digantikan dengan pop-up permohonan maaf). Sistem secara pintar (*smart background job*) akan mencatat profil Pasien yang mencoba memesan tersebut, untuk kemudian dikirimi otomatis tautan layanan via WhatsApp dan sistem *Web Chat* begitu stok obat diperbarui menjadi "Tersedia" lagi.

## 14. Modul Kepatuhan Regulasi & Penyelenggara Sistem Elektronik (PSE)
- **Pelaporan Konten (User Reporting)**: Tombol *Report* bagi Pasien untuk melaporkan pesan/konten berbahaya (seperti percakapan *chat* tidak senonoh) yang dikategorikan sebagai *User Generated Content* (UGC) yang melanggar aturan Kominfo.
- **Takedown Dashboard**: Dasbor khusus bagi Superadmin untuk meninjau laporan UGC. Jika terbukti melanggar, Superadmin dapat mengeksekusi "Pemutusan Akses" (*takedown*) secara instan dengan target batas penyelesaian di bawah 1x24 jam.
- **Law Enforcement Access Portal**: Fitur pengumpulan rekaman data log (audit) guna memfasilitasi penyerahan informasi pengguna atau riwayat komunikasi kepada Aparat Penegak Hukum sesuai tenggat waktu resmi (maksimal 5 hari). Wewenang ini terpusat pada *Superadmin* sebagai *Narahubung Hukum*.
