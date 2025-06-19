# Arsitektur dan Topologi

Bagian ini mencakup keputusan arsitektur yang memiliki implikasi luas bagi sistem yang kita gunakan untuk menyediakan dan mengelola klaster Kubernetes kita. Ini termasuk model penerapan untuk etcd dan pertimbangan unik yang harus kita perhatikan untuk komponen tersebut. Di antara topik-topik ini adalah bagaimana kita mengatur berbagai klaster yang dikelola ke dalam tingkatan berdasarkan tujuan tingkat layanan (SLO) mereka. Kami juga akan melihat konsep pool node dan bagaimana mereka dapat digunakan untuk berbagai tujuan dalam suatu klaster. Terakhir, kami akan membahas metode yang dapat kita gunakan untuk manajemen federasi klaster kita dan perangkat lunak yang kita terapkan ke dalamnya.

## Model Penerapan etcd
Sebagai basis data untuk objek dalam klaster Kubernetes, etcd memerlukan perhatian khusus. etcd adalah penyimpanan data terdistribusi yang menggunakan algoritma konsensus untuk menjaga salinan status klaster kita di beberapa mesin. Ini memperkenalkan pertimbangan jaringan bagi node dalam klaster etcd agar mereka dapat mempertahankan konsensus tersebut dengan andal melalui koneksi jaringan mereka. etcd memiliki persyaratan latensi jaringan yang unik yang perlu dirancang dengan mempertimbangkan topologi jaringan. Kami akan membahas topik tersebut dalam bagian ini dan juga melihat dua pilihan arsitektur utama dalam model penerapan etcd: dedicated versus colocated serta apakah akan dijalankan dalam kontainer atau diinstal langsung di host.

### Pertimbangan Jaringan
Pengaturan default dalam etcd dirancang untuk latensi dalam satu pusat data. Jika kita mendistribusikan etcd di beberapa pusat data, kita harus menguji rata-rata round-trip antara anggota dan menyesuaikan interval heartbeat serta waktu pemilihan untuk etcd jika diperlukan. Kami sangat tidak menyarankan penggunaan klaster etcd yang tersebar di berbagai wilayah. Jika menggunakan beberapa pusat data untuk meningkatkan ketersediaan, pusat data tersebut setidaknya harus berada dalam jarak dekat dalam satu wilayah.

### Dedicated versus Colocated
Pertanyaan umum tentang penerapan etcd adalah apakah memberikan mesin khusus untuk etcd atau mengkolokasikannya pada mesin kontrol dengan server API, scheduler, dan controller manager. Hal pertama yang perlu dipertimbangkan adalah ukuran klaster yang akan kita kelola, yaitu jumlah node pekerja yang akan kita jalankan per klaster. Jika kita memiliki klaster besar dengan lebih dari 50 node pekerja, lebih baik menggunakan mesin khusus untuk etcd guna menghindari kontensi sumber daya dengan komponen kontrol lainnya. Untuk klaster kecil, menggunakan model colocated akan menghemat biaya dan overhead manajemen. Jika jumlah pekerja lebih dari 200, sebaiknya kita menggunakan klaster etcd khusus.

### Kontainerisasi versus Instalasi di Host
Pertanyaan berikutnya adalah apakah akan menginstal etcd langsung di mesin atau menjalankannya dalam kontainer. Jika kita menjalankan etcd dalam mode colocated, disarankan untuk menjalankannya dalam kontainer. Jika kita menjalankan etcd pada mesin khusus, kita memiliki opsi untuk menginstalnya langsung di host atau menjalankannya dalam kontainer dengan menggunakan kubelet dan manifes statis. Menggunakan pola yang sama dengan komponen kontrol lainnya dapat bermanfaat, tetapi pilihan ini lebih merupakan preferensi pribadi.

## Tingkatan Klaster
Mengorganisasi klaster berdasarkan tingkatan adalah pola yang hampir universal. Biasanya, tingkatan ini mencakup pengujian, pengembangan, staging, dan produksi. Setiap tingkatan memiliki SLO dan SLA yang berbeda serta tujuan yang berbeda dalam jalur menuju produksi.

### Pengujian
Klaster dalam tingkatan pengujian bersifat sementara (ephemeral) dan sering kali memiliki waktu hidup (TTL) yang menghapusnya secara otomatis setelah beberapa waktu, biasanya kurang dari seminggu. Tidak ada SLO atau SLA untuk klaster ini.

### Pengembangan
Klaster pengembangan bersifat permanen dan sering kali multi-tenant. Mereka memiliki fitur yang sama dengan klaster produksi tetapi digunakan untuk pengujian integrasi pertama. Klaster ini memiliki SLO tetapi tidak memiliki perjanjian formal terkait keandalannya.

### Staging
Klaster staging juga bersifat permanen dan digunakan untuk pengujian integrasi akhir sebelum rilis ke produksi. Mereka sering memiliki SLO yang mirip dengan klaster pengembangan dan mungkin memiliki SLA jika digunakan oleh pelanggan eksternal atau pemangku kepentingan lainnya.

### Produksi
Klaster produksi digunakan untuk aplikasi yang menghadapi pelanggan dan menghasilkan pendapatan. Hanya versi perangkat lunak yang telah diuji dengan stabil yang dijalankan di sini. SLO yang jelas dan terdefinisi dengan baik digunakan dan dipantau, sering kali dengan SLA yang mengikat secara hukum.

## Node Pools

Node pools adalah cara untuk mengelompokkan tipe-tipe node dalam satu klaster Kubernetes. Node-node ini dapat dikelompokkan berdasarkan karakteristik unik mereka atau berdasarkan peran yang mereka jalankan. Penting untuk memahami keuntungan dan kerugian dalam menggunakan node pools sebelum masuk ke detail lebih lanjut.

Sering kali, perbandingan utama adalah antara menggunakan beberapa node pools dalam satu klaster versus membuat klaster yang berbeda dan terpisah. Jika kita menggunakan node pools, kita perlu menggunakan **Node selectors** pada workload agar mereka ditempatkan di node pool yang sesuai. Kita juga kemungkinan perlu menggunakan **Node taints** untuk mencegah workload tanpa **Node selectors** agar tidak secara tidak sengaja ditempatkan di lokasi yang salah. Selain itu, skalabilitas node dalam klaster menjadi lebih kompleks karena sistem harus memantau pool yang berbeda dan melakukan skala masing-masing secara terpisah.

Sebaliknya, jika kita menggunakan klaster yang terpisah, kita harus mengelola lebih banyak klaster dan memastikan workload ditempatkan pada klaster yang tepat. Berikut adalah ringkasan keuntungan dan kerugian menggunakan node pools:

| Keuntungan | Kerugian |
|------------|---------|
| Mengurangi jumlah klaster yang harus dikelola | Membutuhkan **Node selectors** untuk workload |
| Mengurangi jumlah target klaster untuk workload | **Node taints** perlu diterapkan dan dikelola |
| | Operasi skala klaster menjadi lebih kompleks |

### Jenis Node Pools

#### Node Pool Berdasarkan Karakteristik

Node pool berbasis karakteristik terdiri dari node yang memiliki komponen atau atribut tertentu yang dibutuhkan oleh jenis workload tertentu. Contoh dari node pool ini adalah:
- Node dengan unit pemrosesan grafis (GPU) khusus.
- Node dengan tipe antarmuka jaringan tertentu.
- Node dengan rasio memori terhadap CPU yang spesifik.

Semua karakteristik ini mempengaruhi jenis workload yang dapat dijalankan secara optimal dalam klaster. Jika kita menjalankan berbagai jenis workload dalam satu klaster, kita perlu mengelompokkan node berdasarkan karakteristiknya ke dalam node pools.

#### Node Pool Berdasarkan Peran

Node pool berbasis peran digunakan untuk memastikan bahwa workload dengan fungsi tertentu tidak mengalami konflik sumber daya. Contoh umum dari node pool ini adalah node yang dikhususkan untuk lapisan **ingress** dalam klaster.

Manfaat dari penggunaan node pool berbasis peran:
- Mengisolasi workload dari konflik sumber daya.
- Menyederhanakan model jaringan.
- Membatasi eksposur node tertentu terhadap lalu lintas eksternal.

Namun, perlu diperhatikan bahwa membuat node pool harus berdasarkan alasan yang kuat. Kubernetes sudah cukup kompleks, jadi jangan menambah kompleksitas yang tidak perlu.

---

## Federasi Klaster

Federasi klaster secara umum merujuk pada cara untuk mengelola semua klaster Kubernetes secara terpusat. Banyak organisasi menemukan bahwa mereka tidak hanya memiliki satu klaster Kubernetes, tetapi memiliki banyak klaster yang tersebar di berbagai lokasi.

### Manajemen Klaster

Manajemen klaster melibatkan penggunaan klaster Kubernetes untuk mengelola klaster lainnya. Salah satu proyek populer yang digunakan untuk tujuan ini adalah **Cluster API**, yang menyediakan sumber daya Kubernetes khusus seperti **Cluster** dan **Machine** untuk merepresentasikan klaster lain dan komponennya.

Biasanya, organisasi memiliki klaster manajemen khusus untuk produksi guna memisahkan masalah operasional antara lingkungan produksi dan non-produksi. Namun, hal ini menambah kompleksitas tambahan dalam pengelolaan klaster.

Salah satu tantangan utama dalam manajemen klaster adalah **Cluster Autoscaler**, yang bertugas menambahkan atau menghapus node berdasarkan kebutuhan workload. **Cluster Autoscaler** biasanya berjalan di dalam klaster yang diskalakan, tetapi jika pengelolaan node dilakukan dari klaster manajemen, ini dapat menciptakan ketergantungan eksternal. Jika klaster manajemen tidak tersedia pada saat dibutuhkan, maka klaster yang membutuhkan skala dapat mengalami masalah.

![image](https://github.com/user-attachments/assets/0f4aa307-f5c5-4dd8-bec6-d8a8ed891d69)


Salah satu strategi untuk mengatasi masalah ini adalah dengan menjalankan **Cluster API** di dalam workload cluster itu sendiri. Dengan cara ini, klaster dapat melakukan autoscaling tanpa ketergantungan pada klaster manajemen.

![image](https://github.com/user-attachments/assets/0eef7301-1a9f-4ab0-acb3-5e5ee5a0a6ea)


### Observabilitas

Ketika mengelola banyak klaster, salah satu tantangan terbesar adalah mengumpulkan metrik dari infrastruktur yang tersebar dan menyatukannya dalam satu tempat. **Prometheus** adalah proyek open-source yang banyak digunakan untuk mengumpulkan dan menyimpan metrik.

Strategi federasi dalam Prometheus memungkinkan:
- Server Prometheus federasi mengumpulkan subset metrik dari server Prometheus di tingkat bawah.
- Metrik dapat dikumpulkan secara regional dan kemudian digabungkan secara global.
- Memungkinkan pemantauan skala besar dengan mengurangi beban komputasi pada klaster pusat.

### Penerapan Perangkat Lunak yang Terfederasi

Manajemen deployment perangkat lunak ke banyak klaster adalah tantangan lain yang perlu diatasi dalam federasi klaster. Salah satu pendekatan yang sedang dikembangkan oleh komunitas Kubernetes adalah **KubeFed**, yang memungkinkan federasi berbagai tipe sumber daya Kubernetes seperti **Namespace** dan **Deployment**.

Dengan **KubeFed**, kita dapat:
- Membuat **FederatedDeployment** dalam satu klaster manajemen.
- Menyebarkan deployment yang sama ke beberapa klaster sekaligus.

Namun, pada saat ini, banyak organisasi masih mengandalkan alat CI/CD yang dikonfigurasi untuk menargetkan klaster yang berbeda dalam proses deployment workload mereka.

---

**Dokumentasi dari halaman 28-35**

---
