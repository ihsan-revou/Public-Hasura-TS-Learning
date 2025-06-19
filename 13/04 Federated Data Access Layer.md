# Federated Data Access Layer

## Apa itu Federated Data Access Layer (FDAL)?
Federated Data Access Layer (FDAL) adalah lapisan akses data yang memungkinkan akses yang fleksibel dan aman ke berbagai sumber data yang dikelola secara independen.

---

## Konsumen Umum FDAL
Konsumen FDAL mencakup berbagai kebutuhan, mulai dari akses data realtime dengan latensi rendah dan kapasitas tinggi hingga akses data analitik. Berikut adalah kategori konsumen FDAL:

- **Pengalaman produk berlapis & mikroservis**
- **BFFs (Backend for Frontends)**
- **Produk & layanan internal** seperti pelaporan dan Business Intelligence (BI)
- **Pengembang pihak ketiga**
- **Aplikasi AI dan LLMs (Large Language Models)** yang sedang berkembang

![image](https://github.com/user-attachments/assets/ff72171a-94d7-467c-b58c-1a9aa45f160f)

Gambar berikut menjelaskan arsitektur Federated Data Access Layer (FDAL) yang berfungsi untuk menghubungkan berbagai sumber data secara fleksibel dan aman, sambil memastikan kepatuhan terhadap model domain dan kebutuhan bisnis.

### Komponen-Komponen

#### 1. Produk, Microservices, APIs, Internal Apps, dan LLMs/AI
- Bagian atas gambar menggambarkan berbagai konsumen dari FDAL, seperti:
  - **Produk/microservices/BFFs (Backend for Frontends)**: Aplikasi atau layanan internal.
  - **APIs**: Konsumen yang berinteraksi melalui API standar.
  - **Internal Apps**: Misalnya, aplikasi analitik atau pelaporan internal.
  - **LLMs/AI**: Aplikasi berbasis kecerdasan buatan yang menggunakan FDAL untuk mengakses data.

#### 2. Federated Data Access Layer
- Lapisan ini bertindak sebagai penghubung antara konsumen di atas dan berbagai domain data di bawahnya.
- FDAL menyediakan akses ke data secara real-time, konsisten, dan sesuai dengan kebijakan otorisasi.

#### 3. Domain (data + business logic)
- Di bawah FDAL, terdapat beberapa domain yang mewakili sumber data yang terpisah tetapi tetap terintegrasi melalui FDAL.
- Setiap domain mencakup:
  - **Data**: Data mentah atau informasi dari sumbernya.
  - **Business Logic**: Logika bisnis spesifik domain, seperti validasi, transformasi, dan otorisasi.
    - **Validasi**: Contohnya, memeriksa apakah input data dari konsumen sesuai dengan aturan bisnis (misalnya, email harus memiliki format yang valid).
    - **Transformasi**: Mengubah format data mentah menjadi struktur yang sesuai untuk kebutuhan konsumen (misalnya, mengonversi tanggal dari format lokal ke UTC).
    - **Otorisasi**: Membatasi akses ke data tertentu berdasarkan peran pengguna atau kebijakan organisasi (misalnya, hanya admin yang dapat mengakses data sensitif).  
- **Tim**: Masing-masing domain dikelola oleh tim yang independen, memungkinkan federasi dan kolaborasi antar domain.

#### 4. Koneksi Antar Domain
- Garis-garis antar elemen di setiap domain menunjukkan hubungan atau integrasi antar elemen data di dalam domain tersebut.
- Garis horizontal antar domain menunjukkan adanya `orkestrasi atau agregasi` data lintas domain.

### Tujuan Arsitektur

- **Federasi Data**: Menghubungkan sumber data tanpa perlu memindahkan atau menduplikasi data.
- **Efisiensi & Kepatuhan**: Mengurangi biaya, mencegah data duplikasi, dan meningkatkan kualitas data dengan kontrol otorisasi terpusat.
- **Kolaborasi**: Mendukung model operasi lintas domain yang stabil dan terstandardisasi.
- **Skalabilitas**: Memungkinkan pengembangan independen oleh tim domain masing-masing.

---

## Manfaat FDAL
### 1. Menurunkan Biaya dan Meningkatkan Efisiensi
- Menghindari pemindahan data di berbagai platform dan lapisan (terutama ETL (Extract, Transform, dan Load) yang prematur).
- Meningkatkan konsistensi dan kualitas data.
- Mencegah duplikasi data.

### 2. Meningkatkan Kepatuhan dan Tata Kelola
- Menerapkan model otorisasi yang konsisten, dan jika diperlukan, terpusat.
- Memberikan akses realtime ke data segera setelah data dibuat.
- Membuat API standar untuk berbagai jenis beban kerja.

### 3. Meningkatkan Produktivitas dan Kolaborasi
- Memberikan API yang stabil untuk memisahkan lapisan penyimpanan dan pemodelan fisik dari pemodelan domain.
- Menyediakan model operasional untuk mengintegrasikan logika bisnis lintas domain ("process layer" atau "orchestration layer").
- Mendefinisikan harapan performa & stabilitas sebagai SLA.
- Mengintegrasikan dinamika pasar: analitik penggunaan dan kolaborasi produser/konsumen untuk meningkatkan sumber data yang mendasarinya.

---

## Checklist Platform Supergraph
Ada 6 aspek utama untuk membangun FDAL dengan referensi arsitektur supergraph. Platform supergraph dan stack teknologinya harus mendukung kemampuan berikut:

### I. Model Semantik Terpadu
Model semantik terpadu untuk memahami berbagai entitas di dalam supergraph (sumber daya, metode bisnis, kebijakan otorisasi).

### II. Pemodelan Data & Logika Bisnis
- **Subgraph connectors** memungkinkan pemodelan domain atau API dengan memanfaatkan bahasa sumber data yang mendasarinya.
- Menambahkan logika bisnis untuk transformasi, validasi, dan otorisasi.

Subgraph harus bersifat **"thin"** sehingga karakteristik performanya ditentukan oleh sumber data yang mendasarinya secara prediktif, dan memungkinkan menjadi cukup **"thick"** untuk mengintegrasikan logika bisnis yang diperlukan.

Konsep **"thick" (tebal)** dan **"thin" (tipis)** dalam konteks ini merujuk pada seberapa banyak logika bisnis yang diimplementasikan di lapisan subgraph:

- **Subgraph tipis (thin):**  
Subgraph hanya bertindak sebagai lapisan perantara untuk meneruskan data dari sumber data ke konsumen dengan sedikit atau tanpa manipulasi.
  - Karakteristik performa dari subgraph ini sangat dipengaruhi langsung oleh performa sumber data yang mendasarinya.
  - **Contoh:** Mengakses tabel database langsung tanpa transformasi atau validasi tambahan.

- **Subgraph tebal (thick):**  
Subgraph mengintegrasikan logika bisnis seperti validasi, transformasi data, atau kebijakan otorisasi sebelum meneruskan data ke konsumen.
  - Subgraph ini memproses data lebih banyak, sehingga tidak sepenuhnya bergantung pada performa sumber data yang mendasarinya.
  - Contoh: Menambahkan validasi format email sebelum mengembalikan data pengguna.

### III. Otorisasi
Mesin kebijakan otorisasi yang mendukung:
1. **Komposisi aturan/kebijakan**:
   - FDAL memungkinkan penggabungan berbagai aturan atau kebijakan keamanan. Contoh: Pengguna A hanya dapat mengakses data pada jam kerja, sementara pengguna B memiliki akses penuh kapan saja.
2. **Kontrol akses tingkat kolom (field/column level visibility)**:
   - Memastikan pengguna hanya bisa melihat kolom tertentu dari data. Contoh:
     - Administrator dapat melihat semua kolom dalam tabel pengguna (nama, email, gaji).
     - Staf HR hanya dapat melihat nama dan email tanpa akses ke kolom gaji.
3. **Kontrol akses tingkat entitas (row/entity level access control)**:
   - Mengatur akses ke baris data tertentu berdasarkan kriteria. Contoh:
     - Guru hanya dapat melihat data siswa di kelas yang mereka ajar.
     - Supervisor hanya dapat melihat laporan dari tim yang mereka kelola.
4. **Integrasi optimal dengan pengambilan data (projection & predicate push-down)**:
   - FDAL dirancang agar otorisasi berjalan efisien. Contoh: Query ke database langsung dibatasi untuk hanya mengambil data yang diperbolehkan, sehingga menghemat sumber daya.

#### Contoh Praktis:
Misalkan ada sebuah aplikasi sekolah dengan tabel siswa:

| ID | Nama | Kelas | Nilai | Email |
|----|------|-------|-------|-------|

- Guru hanya boleh melihat **Nama**, **Kelas**, dan **Nilai** untuk siswa di kelas mereka.
- Administrator dapat melihat semua kolom tanpa batasan.
- Sistem FDAL akan:
  - Menyembunyikan kolom **Email** dari guru.
  - Mengambil hanya baris siswa yang ada di kelas guru tersebut.

Sistem ini membantu memastikan keamanan data, mengurangi risiko pelanggaran privasi, dan meningkatkan efisiensi akses.

### IV. Desain API
#### Desain API standar yang dapat menangani (Beban Kerja yang Didorong Domain):
- **Querying resource & memanggil metode bisnis**: API harus mampu menangani permintaan yang berfokus pada sumber daya yang dikelola oleh domain tertentu, termasuk akses data dan pemrosesan terkait. Metode bisnis merujuk pada logika yang lebih kompleks yang menggabungkan beberapa operasi atau aturan bisnis untuk memanipulasi data. API perlu memastikan bahwa keduanya dapat dijalankan dengan efisien dan mudah diakses.
  
- **Operasi filter, sorting, pagination, & agregasi pada resource**: Untuk mempermudah pengambilan data yang relevan, API harus memungkinkan filterisasi berdasarkan kriteria tertentu, pengurutan data, dan pembagian hasil ke dalam halaman (pagination) untuk mencegah overload data dalam satu permintaan. Agregasi juga perlu didukung untuk menganalisis dan menyusun data dalam bentuk ringkasan yang lebih informatif (misalnya, menghitung rata-rata atau jumlah).

- **Konvensi untuk beban kerja relational, event, KV, dan graph**: API perlu memiliki pola atau konvensi yang berbeda untuk mengelola beban kerja berdasarkan jenis sumber data. Relational untuk data berbasis tabel, event untuk aliran data berbasis kejadian, KV (Key-Value) untuk penyimpanan data pasangan kunci-nilai, dan graph untuk data berbasis hubungan antar entitas (seperti GraphQL). Pengelolaan yang konsisten akan mempermudah pengembangan dan pemeliharaan API.
  
#### Kebutuhan API di Lapisan Agregasi & Orkestrasi (Beban Kerja Konsumen):
- **Penggabungan data (joining data)**: API harus dapat menggabungkan data dari berbagai sumber atau domain untuk memberikan respons yang lebih komprehensif kepada pengguna, misalnya dengan cara menggabungkan data yang terkait dari dua tabel atau lebih dalam sistem basis data. Ini biasanya membutuhkan pengelolaan relasi antar data dan pengoptimalan query untuk efisiensi.

- **Nested filtering, sorting, pagination, & agregasi**: Pada tingkat agregasi dan orkestrasi, API perlu mendukung filter bersarang, yang memungkinkan pengguna untuk memfilter data berdasarkan kriteria yang lebih kompleks dan mendalam. Sorting dan pagination juga tetap penting untuk mengelola volume data yang besar, sementara agregasi akan memberikan informasi yang lebih terperinci, seperti rata-rata atau total yang dihitung dari data yang digabungkan.

- **Logika bisnis orkestrasi yang memerlukan querying resource atau metode dari beberapa domain**: API juga harus mendukung orkestrasi logika bisnis yang kompleks, yaitu penggabungan data dan pemrosesan dari beberapa domain atau layanan berbeda. Contoh, API bisa menarik data dari layanan pengguna dan produk secara bersamaan untuk menyusun laporan yang relevan. Hal ini memerlukan desain API yang mampu menangani permintaan yang melibatkan berbagai sumber daya dan logika pemrosesan yang lebih kompleks.

### V. Performa & Observabilitas
#### Perencanaan Query:
- **Visibilitas/penjelasan di lower environment (development, staging)**: Pada tahap pengembangan dan staging, sangat penting untuk menyediakan alat bagi pengembang untuk memantau, menganalisis, dan memahami kinerja query yang dieksekusi. Ini mencakup kemampuan untuk mendiagnosis masalah seperti query yang lambat atau pemrosesan data yang tidak efisien.
  
- **Tracing di production**: Di lingkungan produksi, tracing sangat penting untuk melacak perjalanan setiap permintaan API dari awal hingga akhir. Dengan tracing, pengembang dapat mengidentifikasi bottleneck atau titik masalah lainnya dalam eksekusi query dan respons API.

- **Optimisasi spesifik sumber data untuk mengurangi beban pada sumber data upstream**: API harus dapat mengoptimalkan pengambilan data berdasarkan sumber data yang digunakan, seperti menggunakan indeks database, cache, atau mekanisme lain untuk mengurangi beban pada sumber data upstream. Ini membantu menjaga kinerja dan menghindari pemborosan resource.

#### Pajak Latensi:
- **Mengukur dan mengoptimalkan latensi tambahan di atas sumber data yang mendasarinya**: Meskipun sumber data bisa memiliki latensi intrinsik (misalnya, pengambilan data dari database), API harus dapat mengukur dan mengurangi latensi tambahan yang timbul dari pemrosesan API itu sendiri. Misalnya, menambah lapisan cache atau menggunakan strategi asinkron untuk mengoptimalkan respons waktu nyata dapat membantu menurunkan latensi.

### VI. Kepemilikan Terfederasi
- **CI/CD independen untuk pemilik domain (sumber data)**: Setiap domain atau sumber data dalam sistem harus dapat melakukan pengujian, integrasi, dan pengiriman secara mandiri melalui proses CI/CD yang terpisah. Ini memberikan kebebasan untuk setiap tim mengelola siklus hidup pengembangan mereka tanpa bergantung pada tim lain, memungkinkan iterasi yang lebih cepat dan pengelolaan risiko yang lebih baik.

- **Komposabilitas (agregasi, orkestrasi) lintas domain**: API yang terdesentralisasi harus memungkinkan penggabungan dan orkestrasi data antar berbagai domain. Misalnya, data dari domain pelanggan dapat digabungkan dengan data transaksi dari domain produk untuk menyediakan laporan komprehensif, memungkinkan fleksibilitas dan skalabilitas dalam sistem.

- **Alat analitik & kolaborasi untuk produsen dan konsumen**: Untuk memastikan bahwa API dapat digunakan secara efektif oleh produsen (pengelola domain) dan konsumen (pengguna atau aplikasi), alat analitik yang memberikan wawasan tentang kinerja dan penggunaan API harus tersedia. Selain itu, platform kolaborasi yang mendukung komunikasi dan umpan balik antar pengembang dan pengguna API akan meningkatkan kualitas dan adopsi API.

---

## Noted:
**Orkestrasi** dalam konteks ini berarti mengelola alur kerja atau interaksi antar domain untuk menyelesaikan suatu tugas atau layanan, sementara agregasi berarti menggabungkan data dari berbagai domain untuk menyediakan informasi yang terintegrasi atau komprehensif.

**Contoh Orkestrasi:**
Misalnya, aplikasi e-commerce memerlukan alur yang melibatkan beberapa domain:

1. Domain Produk: Menyediakan detail barang.
2. Domain Pengguna: Memvalidasi kredensial pengguna.
3. Domain Pembayaran: Memproses pembayaran.
4. Domain Pengiriman: Mengatur pengiriman barang.
  
**Orkestrasi** terjadi ketika Federated Data Access Layer mengatur alur kerja ini, memastikan setiap domain diakses secara berurutan dan sesuai dengan logika bisnis (misalnya, memproses pembayaran hanya setelah barang tersedia dalam stok).

**Contoh Agregasi:**
Misalnya, dalam laporan analitik:

1. Domain Penjualan: Menyediakan data penjualan per produk.
2. Domain Pemasaran: Menyediakan data kampanye iklan.
3. Domain Keuangan: Menyediakan data pendapatan.
  
**Agregasi** terjadi ketika FDAL menggabungkan data dari ketiga domain tersebut untuk menghasilkan laporan performa bisnis, seperti pendapatan per kampanye iklan.

Dengan adanya orkestrasi dan agregasi, Federated Data Access Layer mempermudah akses data yang terstruktur sekaligus memastikan kelancaran operasi lintas domain.
