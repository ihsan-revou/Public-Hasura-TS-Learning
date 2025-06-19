## **Pembaruan (Upgrades)**  

Manajemen siklus hidup klaster sangat berkaitan dengan penerapan klaster. Sistem penerapan klaster tidak harus memperhitungkan pembaruan di masa depan; namun, ada cukup banyak hal yang saling terkait sehingga lebih baik untuk mempertimbangkannya sejak awal. Setidaknya, strategi pembaruan Anda harus ditetapkan sebelum masuk ke produksi. Mampu menerapkan platform tanpa kemampuan untuk memperbarui dan memeliharanya sangatlah berisiko. Ketika Anda melihat beban kerja produksi berjalan pada versi Kubernetes yang jauh tertinggal dari rilis terbaru, itu adalah akibat dari pengembangan sistem penerapan klaster yang digunakan di produksi sebelum sistem tersebut memiliki kemampuan pembaruan.  

Saat pertama kali masuk ke produksi dengan beban kerja yang menghasilkan pendapatan, anggaran teknik yang besar akan digunakan untuk menangani fitur-fitur yang masih kurang atau kendala yang muncul. Seiring waktu, fitur-fitur tersebut akan ditambahkan dan masalah-masalah diatasi, tetapi intinya adalah bahwa fitur-fitur tersebut akan menjadi prioritas utama sementara strategi pembaruan hanya menjadi pekerjaan yang tertunda. Oleh karena itu, persiapkan anggaran lebih awal untuk permasalahan yang akan muncul setelah implementasi (day-2 concerns). Anda di masa depan akan berterima kasih atas keputusan ini.  

Dalam membahas pembaruan, kita akan melihat cara memberi versi pada platform untuk memastikan bahwa dependensi dipahami dengan baik, baik untuk platform itu sendiri maupun untuk beban kerja yang menggunakannya. Kita juga akan membahas cara merencanakan rollback jika terjadi kesalahan serta pengujian untuk memastikan bahwa semuanya berjalan sesuai rencana. Terakhir, kita akan membandingkan dan membahas strategi pembaruan Kubernetes secara spesifik.  

---

## **Pemberian Versi pada Platform (Platform Versioning)**  

Pertama, versi-kan platform Anda dan dokumentasikan versi dari semua perangkat lunak yang digunakan dalam platform tersebut. Ini mencakup versi sistem operasi pada mesin serta semua paket yang diinstal, seperti runtime kontainer. Tentu saja, ini juga mencakup versi Kubernetes yang digunakan. Selain itu, pastikan untuk mencatat versi dari setiap add-on yang ditambahkan ke dalam platform aplikasi Anda.  

Umumnya, beberapa tim mengikuti versi Kubernetes sebagai versi platform mereka sehingga semua orang tahu bahwa platform versi 1.18 menggunakan Kubernetes versi 1.18 tanpa perlu berpikir atau mencarinya lagi. Namun, hal yang lebih penting daripada metode yang digunakan adalah memastikan bahwa versi selalu terdokumentasi dan digunakan secara konsisten.  

Jika menggunakan konvensi penomoran versi semantik (semantic versioning), sebaiknya platform memiliki nomor versinya sendiri yang independen dari komponen lainnya. Misalnya, jika Anda memperbarui runtime kontainer karena ada kerentanan keamanan, perubahan tersebut harus tercermin dalam versi platform. Dalam versi semantik, ini biasanya berarti perubahan pada angka versi bugfix. Namun, ini bisa membingungkan jika platform masih menggunakan format versi yang sama dengan Kubernetes, seperti v1.18.5 → v1.18.6. Oleh karena itu, lebih baik memberikan nomor versi platform yang independen.  

---

## **Bersiap untuk Kegagalan (Plan to Fail)**  

Mulailah dengan asumsi bahwa sesuatu akan salah selama proses pembaruan. Bayangkan diri Anda dalam situasi pemulihan dari kegagalan besar, dan gunakan kekhawatiran tersebut sebagai motivasi untuk bersiap menghadapinya.  

Otomatisasi proses pencadangan dan pemulihan sumber daya Kubernetes, baik dengan snapshot langsung dari etcd maupun dengan pencadangan menggunakan Velero melalui API. Lakukan hal yang sama untuk data persisten yang digunakan oleh aplikasi Anda. Pastikan juga ada rencana pemulihan bencana untuk aplikasi dan dependensi penting Anda.  

Untuk aplikasi kompleks yang bersifat stateful dan terdistribusi, sekadar memulihkan status aplikasi dan sumber daya Kubernetes tanpa mempertimbangkan urutan dan dependensi tidak akan cukup. Pikirkan berbagai kemungkinan kegagalan dan kembangkan sistem pemulihan otomatis untuk mengatasinya, lalu uji coba sistem tersebut.  

Pertimbangkan juga jalur rollback dengan cermat. Jika pembaruan menimbulkan kesalahan atau gangguan yang tidak dapat segera didiagnosis, opsi rollback akan menjadi asuransi yang baik. Namun, dalam beberapa kasus, rollback bukanlah solusi terbaik. Misalnya, jika pembaruan sudah dilakukan cukup jauh, membatalkan semua perubahan sebelumnya bisa menjadi keputusan yang buruk. Pahami titik-titik di mana rollback tidak lagi menjadi opsi dan tentukan strategi sebelum melakukan operasi secara langsung di lingkungan produksi.  

---

## **Pengujian Integrasi (Integration Testing)**  

Memiliki sistem pemberian versi yang terdokumentasi dengan baik adalah satu hal, tetapi cara mengelola versi tersebut adalah hal lain. Sistem berbasis Kubernetes sangat kompleks, dan memastikan semua komponen bekerja bersama sebagaimana mestinya adalah tantangan besar. Tidak hanya kompatibilitas antara komponen platform yang penting, tetapi juga kompatibilitas antara aplikasi yang berjalan di platform dengan platform itu sendiri harus diuji dan dikonfirmasi.  

Uji unit (unit testing) untuk semua komponen platform sangat penting, tetapi uji integrasi (integration testing) sama pentingnya, meskipun jauh lebih menantang. Salah satu alat yang dapat membantu dalam pengujian ini adalah **Sonobuoy**, yang biasanya digunakan untuk menjalankan uji konformitas Kubernetes guna memastikan bahwa semua komponen klaster berfungsi dengan benar.  

Selain menggunakan alat seperti Sonobuoy, Anda harus mengembangkan plug-in pengujian khusus untuk fitur platform Anda, menguji operasi yang kritis bagi kebutuhan organisasi, dan menjalankan pengujian ini secara rutin. Salah satu caranya adalah dengan menggunakan **Kubernetes CronJob** untuk menjalankan sebagian atau seluruh suite pengujian secara berkala. Hasil pemindaian ini dapat diekspos sebagai metrik yang dapat ditampilkan dalam dasbor dan dikonfigurasi untuk peringatan otomatis.  

---

## **Strategi Pembaruan Kubernetes (Kubernetes Upgrade Strategies)**  

Ada tiga strategi utama dalam memperbarui platform berbasis Kubernetes:  

1. **Penggantian klaster (Cluster replacement)**  
2. **Penggantian node (Node replacement)**  
3. **Pembaruan langsung di tempat (In-place upgrades)**  

Kita akan membahasnya berdasarkan biaya dan risiko, dari strategi dengan biaya tertinggi tetapi risiko terendah hingga strategi dengan biaya terendah tetapi risiko tertinggi. Tidak ada solusi yang cocok untuk semua kasus, sehingga pemilihan strategi harus mempertimbangkan anggaran, toleransi risiko, dan kebutuhan spesifik.  

Ketiga strategi ini tidak saling eksklusif—Anda dapat menggabungkannya. Misalnya, Anda dapat melakukan pembaruan langsung untuk klaster etcd yang berdedikasi dan mengganti node untuk sisa klaster Kubernetes. Namun, sebaiknya gunakan strategi yang sama di seluruh lingkungan untuk memastikan metode yang diterapkan di produksi telah diuji terlebih dahulu di lingkungan pengembangan dan staging.  

---

### Penggantian Cluster

Penggantian cluster adalah solusi dengan biaya tertinggi tetapi risiko terendah. Risiko rendah ini karena mengikuti prinsip infrastruktur yang tidak dapat diubah yang diterapkan ke seluruh cluster. Pembaruan dilakukan dengan menerapkan cluster baru secara keseluruhan di samping yang lama. Beban kerja kemudian dimigrasikan dari cluster lama ke yang baru. Cluster baru yang telah diperbarui diperluas sesuai kebutuhan saat beban kerja dipindahkan, sementara node pekerja dari cluster lama dikurangi skalanya saat beban kerja berpindah. Namun, sepanjang proses pembaruan, akan ada biaya tambahan karena adanya cluster baru yang berbeda secara keseluruhan. Skalabilitas keluar dari cluster baru dan pengurangan skala dari cluster lama membantu mengurangi biaya ini. Sebagai contoh, jika Anda memperbarui cluster produksi dengan 300 node, Anda tidak perlu langsung menyediakan cluster baru dengan 300 node. Sebagai gantinya, Anda dapat memulai dengan 20 node, dan setelah beberapa beban kerja pertama dimigrasikan, Anda dapat mengurangi skala cluster lama dan memperbesar skala cluster baru untuk menampung beban kerja berikutnya.

Penggunaan autoscaling cluster dan overprovisioning dapat membuat proses ini lebih lancar, tetapi pembaruan saja mungkin bukan alasan utama untuk menggunakan teknologi tersebut. Ada dua tantangan umum saat menangani penggantian cluster.

Tantangan pertama adalah mengelola lalu lintas ingress. Saat beban kerja dimigrasikan dari satu cluster ke cluster lain, lalu lintas harus dialihkan ke cluster baru yang telah diperbarui. Ini berarti bahwa DNS untuk beban kerja yang diekspos secara publik tidak boleh mengarah ke ingress cluster secara langsung, tetapi ke load balancer layanan global (GSLB) atau reverse proxy yang kemudian mengarahkan lalu lintas ke ingress cluster. Pendekatan ini memberikan titik kontrol untuk mengelola lalu lintas masuk ke beberapa cluster.

Tantangan kedua adalah ketersediaan penyimpanan persisten. Jika menggunakan layanan penyimpanan atau perangkat penyimpanan, penyimpanan yang sama harus dapat diakses dari kedua cluster. Jika menggunakan layanan terkelola seperti layanan database dari penyedia cloud publik, Anda harus memastikan layanan yang sama tersedia di kedua cluster. Dalam pusat data pribadi, ini bisa menjadi pertanyaan terkait jaringan dan firewall. Di cloud publik, ini akan melibatkan jaringan dan zona ketersediaan. Misalnya, volume AWS EBS hanya tersedia di zona ketersediaan tertentu. Selain itu, layanan terkelola di AWS sering kali memiliki Virtual Private Cloud (VPC) tertentu yang terkait. Oleh karena itu, Anda mungkin ingin mempertimbangkan untuk menggunakan satu VPC untuk beberapa cluster. Banyak penginstal Kubernetes mengasumsikan satu VPC per cluster, tetapi ini tidak selalu menjadi model terbaik.

Selanjutnya, Anda harus mempertimbangkan migrasi beban kerja. Ini mencakup sumber daya Kubernetes itu sendiri—Deployment, Service, ConfigMap, dll. Anda dapat melakukan migrasi beban kerja dengan dua cara:

1. Menyebarkan ulang dari sumber kebenaran yang telah dideklarasikan.
2. Menyalin sumber daya yang ada dari cluster lama.

Opsi pertama melibatkan pengalihan pipeline deployment ke cluster baru dan menyebarkan kembali sumber daya yang sama. Ini mengasumsikan bahwa sumber kebenaran dari definisi sumber daya yang ada dalam kontrol versi dapat diandalkan dan tidak ada perubahan langsung yang terjadi. Namun, dalam kenyataannya, perubahan sering kali dilakukan oleh manusia, controller, dan sistem lain. Jika ini terjadi, opsi kedua diperlukan, yaitu menyalin sumber daya yang ada dan menerapkannya ke cluster baru. Alat seperti Velero sangat berguna dalam hal ini. Velero biasanya digunakan sebagai alat pencadangan, tetapi nilainya sebagai alat migrasi bahkan lebih tinggi. Velero dapat mengambil snapshot semua sumber daya dalam cluster atau subset tertentu. Dengan demikian, jika Anda memigrasikan beban kerja berdasarkan Namespace, Anda dapat mengambil snapshot setiap Namespace saat proses migrasi dan memulihkannya ke cluster baru dengan cara yang sangat andal. Velero mengambil snapshot ini bukan langsung dari datastore etcd, tetapi melalui API Kubernetes, sehingga selama Anda dapat memberikan akses ke API server untuk kedua cluster, metode ini sangat berguna.

![image](https://github.com/user-attachments/assets/a8428f2a-e167-42ee-b3fe-2bb4f3fe8778)


---

### Penggantian Node

Opsi penggantian node adalah jalan tengah antara biaya dan risiko. Pendekatan ini umum digunakan dan didukung oleh Cluster API. Ini adalah opsi yang dapat diterima jika Anda mengelola cluster besar dan memahami masalah kompatibilitas dengan baik. Masalah kompatibilitas adalah salah satu risiko terbesar dalam metode ini karena Anda memperbarui control plane secara langsung dalam konteks layanan dan beban kerja cluster Anda. Jika Anda memperbarui Kubernetes secara langsung dan versi API yang digunakan oleh salah satu beban kerja Anda tidak lagi tersedia, beban kerja tersebut bisa mengalami gangguan.

Beberapa cara untuk mengurangi risiko ini adalah:

- Membaca catatan rilis Kubernetes secara menyeluruh sebelum memperbarui versi Kubernetes.
- Menguji pembaruan dengan cermat di lingkungan pengembangan dan staging sebelum menerapkannya ke produksi.
- Menghindari ketergantungan yang ketat pada API Kubernetes untuk beban kerja produksi.

Opsi penggantian node sangat menguntungkan jika Anda membangun image mesin terlebih dahulu yang telah diuji dan diverifikasi dengan baik. Dengan begitu, Anda dapat dengan cepat menghadirkan mesin baru dan menggabungkannya ke dalam cluster.

Ketika mengganti node dalam cluster, mulailah dengan control plane. Jika Anda menjalankan cluster etcd khusus, mulailah dari sana karena data persisten cluster sangat penting. Jika Anda mengalami masalah saat memperbarui node etcd pertama, Anda masih memiliki peluang untuk membatalkan pembaruan. Jika Anda memperbarui semua node pekerja dan control plane lalu menemukan masalah dalam memperbarui etcd, Anda akan berada dalam situasi di mana rollback tidak praktis.

Untuk cluster etcd khusus, pertimbangkan untuk mengganti node dengan metode pengurangan, yaitu menghapus satu node dan menambahkan pengganti yang diperbarui, daripada menambahkan node baru terlebih dahulu sebelum menghapus yang lama. Ini akan menjaga daftar anggota etcd tetap stabil.

Untuk node control plane, gunakan `kubeadm join --control-plane` pada mesin baru yang telah memiliki versi Kubernetes terbaru. Setelah setiap node baru online dan berfungsi, satu node versi lama dapat di-drain dan dihapus.

Untuk node pekerja, Anda dapat menggantinya satu per satu atau dalam kelompok. Jika cluster Anda memiliki beban kerja tinggi, tambahkan node baru sebelum menghapus node lama untuk memastikan sumber daya komputasi cukup untuk Pod yang dipindahkan.

![image](https://github.com/user-attachments/assets/fe5f89ea-3a4d-460d-91d4-f5e89bf605a6)


---

### Pembaruan di Tempat

Pembaruan di tempat cocok untuk lingkungan dengan sumber daya terbatas di mana penggantian node tidak praktis. Jalur rollback lebih sulit, sehingga risikonya lebih tinggi, tetapi ini dapat dikurangi dengan pengujian yang komprehensif.

Untuk node etcd, ikuti dokumentasi resmi, matikan setiap node satu per satu, lakukan pembaruan OS, paket, dan lain-lain, lalu hidupkan kembali node tersebut. Jika menjalankan etcd dalam container, pertimbangkan untuk menarik image terlebih dahulu sebelum mematikan node guna meminimalkan downtime.

Untuk control plane dan node pekerja, jika `kubeadm` digunakan untuk inisialisasi cluster, maka alat yang sama sebaiknya digunakan untuk pembaruan. Dokumentasi upstream memiliki panduan rinci untuk setiap versi minor Kubernetes dari 1.13 ke atas.

Seperti biasa, rencanakan kegagalan, otomatisasi sebanyak mungkin, dan uji dengan cermat sebelum menerapkan perubahan di lingkungan produksi.

---

**Dokumentasi dari halaman 52-60**

---
