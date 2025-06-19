# Infrastruktur

## Penerapan Kubernetes

Penerapan Kubernetes adalah proses instalasi perangkat lunak yang bergantung pada infrastruktur TI. Sebuah kluster Kubernetes dapat dijalankan di laptop menggunakan mesin virtual atau kontainer Docker, tetapi ini hanya simulasi untuk keperluan pengujian. Untuk penggunaan produksi, berbagai komponen infrastruktur perlu ada dan sering kali disiapkan sebagai bagian dari penerapan Kubernetes itu sendiri.

Sebuah kluster Kubernetes yang siap untuk produksi membutuhkan sejumlah komputer yang terhubung ke jaringan. Untuk konsistensi terminologi, kita akan menggunakan istilah *mesin* untuk komputer-komputer ini, yang bisa berupa virtual atau fisik. Yang penting adalah kemampuan untuk menyediakan mesin-mesin ini, serta metode yang digunakan untuk mengaktifkannya.

Kita mungkin perlu membeli perangkat keras dan menginstalnya di pusat data, atau cukup meminta sumber daya dari penyedia cloud untuk menjalankan mesin sesuai kebutuhan. Apa pun prosesnya, kita memerlukan mesin dan jaringan yang dikonfigurasi dengan benar, yang perlu diperhitungkan dalam model penerapan kita.

Sebagai bagian penting dari upaya otomatisasi kita, pertimbangkan dengan cermat otomatisasi manajemen infrastruktur. Hindari operasi manual seperti mengklik formulir dalam wizard online. Sebaliknya, gunakan sistem deklaratif yang memanggil API untuk mencapai hasil yang sama. Model otomatisasi ini membutuhkan kemampuan untuk menyediakan server, jaringan, dan sumber daya terkait sesuai permintaan, seperti yang ditawarkan oleh penyedia cloud seperti Amazon Web Services, Microsoft Azure, atau Google Cloud Platform.

Namun, tidak semua lingkungan memiliki API atau antarmuka pengguna web untuk menyiapkan infrastruktur. Banyak beban kerja produksi berjalan di pusat data dengan server yang dibeli dan dipasang oleh perusahaan yang menggunakannya. Ini perlu dilakukan sebelum komponen perangkat lunak Kubernetes diinstal dan dijalankan. Oleh karena itu, penting untuk membedakan model dan pola yang berlaku dalam setiap kasus.

## Bare Metal vs Virtualisasi

Ketika mengeksplorasi Kubernetes, banyak yang mempertanyakan apakah lapisan mesin virtual masih diperlukan. Bukankah kontainer sudah menyediakan fungsionalitas yang sama? Apakah ini berarti menjalankan dua lapisan virtualisasi? Jawabannya adalah, tidak selalu. Kubernetes dapat berjalan sukses baik di atas bare metal maupun lingkungan virtualisasi. Memilih medium yang tepat harus dilakukan berdasarkan masalah yang ingin diselesaikan dan tingkat kematangan tim dalam teknologi terkait.

Revolusi virtualisasi telah mengubah cara dunia menyusun dan mengelola infrastruktur. Dahulu, tim infrastruktur menggunakan metode seperti PXE booting, mengelola konfigurasi server, dan menyediakan perangkat keras tambahan seperti penyimpanan. Lingkungan virtualisasi modern menyembunyikan semua ini di balik API, memungkinkan penyediaan, perubahan, dan penghapusan sumber daya tanpa perlu mengetahui perangkat keras yang mendasarinya.

Model ini telah terbukti efektif di pusat data dengan vendor seperti VMware serta di cloud, di mana sebagian besar komputasi berjalan di atas hipervisor. Dengan kemajuan ini, banyak operator infrastruktur dalam dunia cloud-native mungkin tidak pernah berurusan dengan permasalahan perangkat keras dasar.

![image](https://github.com/user-attachments/assets/8848fece-6760-4cac-9192-c604ce2bb60f)


Manfaat model virtualisasi mencakup:
- Pembuatan dan kloning mesin serta gambar mesin dengan mudah
- Menjalankan berbagai sistem operasi pada satu server
- Optimalisasi penggunaan server dengan menyesuaikan sumber daya berdasarkan kebutuhan aplikasi
- Mengubah pengaturan sumber daya tanpa mengganggu server
- Pengendalian perangkat keras secara programatik
- Konfigurasi jaringan dan routing unik per server

Namun, virtualisasi memiliki kompromi. Ada overhead tambahan yang timbul saat berjalan lebih jauh dari perangkat keras. Beberapa aplikasi dengan kebutuhan performa tinggi, seperti aplikasi perdagangan keuangan, lebih memilih bare metal. Selain itu, dalam komputasi edge seperti jaringan 5G, menjalankan langsung di perangkat keras mungkin lebih diinginkan.

Sekarang setelah kita menyelesaikan tinjauan singkat tentang revolusi virtualisasi, mari kita periksa bagaimana hal ini memengaruhi kita saat menggunakan Kubernetes dan abstraksi kontainer karena hal ini memaksa titik interaksi kita lebih tinggi lagi di tumpukan. Gambar berikut mengilustrasikan bagaimana hal ini terlihat dari sudut pandang operator di lapisan Kubernetes. Komputer yang mendasarinya dilihat sebagai "lautan komputasi" tempat beban kerja dapat menentukan sumber daya apa yang mereka butuhkan dan akan dijadwalkan dengan tepat.

![image](https://github.com/user-attachments/assets/82070e2a-9fa4-4e35-a982-477dde733bd4)


Penting untuk dicatat bahwa kluster Kubernetes memiliki beberapa titik integrasi dengan infrastruktur yang mendasarinya. Misalnya, banyak yang menggunakan driver CSI untuk berintegrasi dengan penyedia penyimpanan. Ada beberapa proyek manajemen infrastruktur yang memungkinkan permintaan host baru dari penyedia dan bergabung dengan kluster. Dan, yang paling umum, organisasi mengandalkan Integrasi Penyedia Cloud (CPI), yang melakukan pekerjaan tambahan, seperti menyediakan penyeimbang beban di luar kluster untuk merutekan lalu lintas di dalamnya.

### Tantangan Bare Metal

Keberhasilan di lingkungan bare metal memiliki tantangan tersendiri, seperti:
- **Node yang lebih besar**: Lebih banyak Pod per node dapat membuat operasi lebih rumit.
- **Scaling dinamis**: Cara menambah node dengan cepat berdasarkan kebutuhan beban kerja.
- **Penyediaan image**: Distribusi image mesin yang cepat untuk menjaga node tetap imutabel.
- **Tidak adanya API load balancer**: Perlu menyediakan routing dari luar kluster ke jaringan Pod.
- **Integrasi penyimpanan yang kurang canggih**: Pengelolaan penyimpanan jaringan untuk Pod.
- **Keamanan multitenant**: Tidak adanya hipervisor membuat isolasi keamanan lebih menantang.

Meskipun tantangan ini ada, berbagai proyek seperti kube-vip dan MetalLB dapat membantu mengatasi kekurangan infrastruktur bare metal.

## Ukuran Kluster

Dalam perencanaan infrastruktur Kubernetes, ukuran kluster yang akan digunakan merupakan keputusan penting. Beberapa keuntungan dari kluster yang lebih besar meliputi:
- **Pemanfaatan sumber daya lebih baik**: Mengurangi duplikasi layanan kontrol.
- **Lebih sedikit penerapan kluster**: Mengurangi kebutuhan otomatisasi penerapan.
- **Manajemen yang lebih sederhana**: Mengurangi kompleksitas alokasi dan federasi beban kerja.

Sementara itu, kluster yang lebih kecil menawarkan keuntungan seperti:
- **Dampak kegagalan lebih kecil**: Gangguan hanya mempengaruhi sebagian kecil beban kerja.
- **Fleksibilitas tenancy**: Mengurangi kebutuhan rekayasa kompleks untuk keamanan dan isolasi.
- **Lebih sedikit penyesuaian skala**: Menghindari bottleneck yang muncul dalam kluster besar.
- **Opsi peningkatan yang lebih mudah**: Memungkinkan penggantian kluster saat melakukan peningkatan.
- **Alternatif pool node**: Memudahkan pengelolaan beban kerja khusus seperti GPU atau node dengan memori tinggi.

Pemilihan ukuran kluster yang tepat bergantung pada kebutuhan teknis serta pengalaman operasional organisasi kita. Jika Kubernetes dipertimbangkan sebagai pengganti stack virtualisasi, penting untuk mengevaluasi dengan cermat peran yang akan diisi oleh Kubernetes dalam infrastruktur kita.

## Infrastruktur Komputasi

Untuk menyatakan yang sudah jelas, sebuah klaster Kubernetes memerlukan mesin. Mengelola kumpulan mesin ini adalah tujuan inti dari Kubernetes. Salah satu pertimbangan awal adalah jenis mesin yang harus dipilih. Berapa banyak inti prosesor? Berapa banyak memori? Berapa banyak penyimpanan onboard? Kualitas antarmuka jaringan seperti apa yang dibutuhkan? Apakah diperlukan perangkat khusus seperti GPU? Semua pertimbangan ini bergantung pada kebutuhan perangkat lunak yang akan dijalankan. Apakah beban kerja bersifat intensif komputasi? Atau lebih banyak menggunakan memori? Apakah kita menjalankan pembelajaran mesin atau beban kerja AI yang memerlukan GPU? Jika kasus penggunaan kita cukup umum, di mana beban kerja sesuai dengan rasio komputasi-ke-memori pada mesin tujuan umum, dan jika beban kerja kita tidak terlalu bervariasi dalam profil konsumsi sumber daya, maka ini akan menjadi latihan yang relatif sederhana. Namun, jika kita memiliki perangkat lunak yang kurang umum dan lebih beragam, maka ini akan lebih kompleks. Mari kita pertimbangkan berbagai jenis mesin untuk klaster kita:

### Mesin etcd (opsional)

Jenis mesin ini hanya diperlukan jika kita menjalankan klaster etcd khusus untuk klaster Kubernetes kita. Kami telah membahas pertimbangan ini di bagian sebelumnya. Mesin ini harus memprioritaskan kinerja baca/tulis disk, sehingga jangan pernah menggunakan hard drive lama berbasis piringan (HDD). Pertimbangkan juga untuk mendedikasikan disk penyimpanan untuk etcd, bahkan jika menjalankan etcd di mesin khusus, agar tidak terjadi persaingan penggunaan disk antara etcd dengan sistem operasi atau program lain. Selain itu, pertimbangkan kinerja jaringan, termasuk kedekatan dalam jaringan, untuk mengurangi latensi jaringan antara mesin dalam klaster etcd tertentu.

### Node Kendali (Control Plane) (wajib)

Mesin ini akan didedikasikan untuk menjalankan komponen kendali klaster. Mereka harus berupa mesin tujuan umum yang disesuaikan dengan ukuran klaster yang diantisipasi serta persyaratan toleransi kegagalan. Dalam klaster yang lebih besar, API server akan memiliki lebih banyak klien dan menangani lebih banyak lalu lintas. Ini dapat diatasi dengan lebih banyak sumber daya komputasi per mesin, atau lebih banyak mesin. Namun, komponen seperti scheduler dan controller manager hanya memiliki satu pemimpin aktif pada waktu tertentu. Peningkatan kapasitas untuk komponen ini tidak dapat dicapai dengan lebih banyak replika seperti API server. Penskalaan secara vertikal dengan lebih banyak sumber daya komputasi per mesin harus digunakan jika komponen ini kekurangan sumber daya. Selain itu, jika kita menggabungkan etcd pada mesin kendali ini, pertimbangan yang sama dengan mesin etcd yang telah disebutkan di atas juga berlaku.

### Node Pekerja (Worker Nodes) (wajib)

Ini adalah mesin tujuan umum yang menjalankan beban kerja non-kendali.

### Node yang Dioptimalkan untuk Memori (opsional)

Jika kita memiliki beban kerja dengan profil memori yang tidak sesuai dengan node pekerja tujuan umum, pertimbangkan kumpulan node yang dioptimalkan untuk memori. Misalnya, jika kita menggunakan jenis instance AWS M5 untuk node pekerja yang memiliki rasio CPU:memori 1CPU:4GiB, tetapi memiliki beban kerja yang mengonsumsi sumber daya dengan rasio 1CPU:8GiB, maka beban kerja ini akan menyisakan CPU yang tidak terpakai. Inefisiensi ini dapat diatasi dengan menggunakan node yang dioptimalkan untuk memori seperti tipe instance R5 di AWS, yang memiliki rasio 1CPU:8GiB.

### Node yang Dioptimalkan untuk Komputasi (opsional)

Sebaliknya, jika kita memiliki beban kerja yang sesuai dengan profil node yang dioptimalkan untuk komputasi, seperti tipe instance C5 di AWS dengan rasio 1CPU:2GiB, maka pertimbangkan untuk menambahkan kumpulan node dengan jenis mesin ini demi efisiensi yang lebih baik.

### Node dengan Perangkat Keras Khusus (opsional)

Permintaan perangkat keras khusus yang umum adalah GPU. Jika kita memiliki beban kerja (misalnya pembelajaran mesin) yang memerlukan perangkat keras khusus, menambahkan kumpulan node dalam klaster dan menargetkan node tersebut untuk beban kerja yang sesuai akan sangat efektif.

## Infrastruktur Jaringan

### Keterjangkauan (Routability)

Kita hampir pasti tidak ingin node klaster kita diekspos ke internet publik. Kemudahan untuk dapat terhubung ke node tersebut dari mana saja hampir tidak pernah sebanding dengan ancaman yang ditimbulkan. Sebagai solusinya, kita bisa menggunakan bastion host atau jump box yang aman untuk mengakses node klaster. Selain itu, ada lebih banyak pertimbangan terkait akses jaringan privat kita. Beberapa layanan dalam jaringan kita perlu terhubung dengan klaster kita, seperti penyimpanan, registri container internal, sistem CI/CD, DNS internal, server NTP privat, dan lainnya.

### Redundansi

Gunakan zona ketersediaan (AZ) untuk membantu menjaga waktu operasional jika memungkinkan. Dua zona ketersediaan sudah baik, tetapi tiga lebih baik. Namun, lebih dari itu bergantung pada tingkat bencana yang ingin kita persiapkan. Kita juga perlu mempertimbangkan apakah membangun redundansi untuk beban kerja atau untuk control plane klaster itu sendiri.

### Load Balancing

Kita memerlukan load balancer untuk API server Kubernetes. Jika kita dapat menyediakan load balancer secara programatik, kita dapat mengonfigurasinya sebagai bagian dari penerapan control plane klaster kita. Selain itu, jika kita memiliki beban kerja yang diekspos secara publik, kita akan memerlukan load balancer yang terpisah yang terhubung dengan ingress klaster kita.

## Strategi Automasi

Dalam mengotomatisasi komponen infrastruktur untuk klaster Kubernetes kita, ada beberapa keputusan strategis yang harus dibuat. Ini mencakup alat yang tersedia dan cara memanfaatkan Kubernetes operators untuk tujuan ini.

### Alat Manajemen Infrastruktur

Alat seperti Terraform dan CloudFormation untuk AWS memungkinkan kita menyatakan keadaan yang diinginkan untuk infrastruktur komputasi dan jaringan, lalu menerapkannya. Alat ini sangat berguna jika kita memiliki infrastruktur yang perlu direplikasi secara konsisten tanpa banyak variasi. Namun, alat ini kurang fleksibel jika infrastruktur kita menjadi sangat kompleks dan dinamis.

### Kubernetes Operators

Jika alat manajemen infrastruktur memiliki keterbatasan yang memerlukan pemrograman menggunakan bahasa pemrograman umum, maka Kubernetes operators bisa menjadi solusi. Operator Kubernetes menggunakan sumber daya khusus dan pengendali Kubernetes untuk mengelola sistem secara lebih efisien. Salah satu proyek open source yang dapat digunakan untuk ini adalah Cluster API, yang merupakan kumpulan operator Kubernetes untuk mengelola infrastruktur klaster Kubernetes.

Kubernetes menawarkan opsi luar biasa untuk mengotomatisasi manajemen penerapan perangkat lunak berbasis container. Demikian pula, Kubernetes menyediakan manfaat yang signifikan untuk strategi otomatisasi infrastruktur klaster melalui penggunaan Kubernetes operators. Pertimbangkan untuk menggunakan dan berkontribusi pada proyek seperti Cluster API untuk meningkatkan efisiensi dan fleksibilitas manajemen klaster kita.

---

**Dokumentasi dari halaman 35-46**

---
