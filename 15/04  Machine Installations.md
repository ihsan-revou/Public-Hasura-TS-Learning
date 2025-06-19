# Instalasi Mesin

Ketika mesin untuk klaster Anda dijalankan, mereka akan memerlukan sistem operasi, paket tertentu yang diinstal, dan konfigurasi yang ditulis. Anda juga akan memerlukan beberapa utilitas atau program untuk menentukan nilai variabel lingkungan dan lainnya, menerapkannya, serta mengoordinasikan proses memulai komponen Kubernetes yang telah dikontainerisasi.

Terdapat dua strategi utama yang umum digunakan dalam hal ini:
- Alat manajemen konfigurasi
- Citra mesin

## Manajemen Konfigurasi

Alat manajemen konfigurasi seperti Ansible, Chef, Puppet, dan Salt menjadi populer dalam lingkungan di mana perangkat lunak diinstal pada mesin virtual dan dijalankan langsung pada host. Alat-alat ini sangat baik untuk mengotomatisasi konfigurasi banyak mesin jarak jauh. Mereka mengikuti berbagai model, tetapi secara umum, administrator dapat secara deklaratif menentukan bagaimana tampilan mesin dan menerapkannya secara otomatis.

Alat manajemen konfigurasi ini memungkinkan Anda untuk menetapkan konsistensi mesin dengan andal. Setiap mesin dapat memperoleh perangkat lunak dan konfigurasi yang hampir identik, biasanya dilakukan dengan resep atau playbook deklaratif yang diperiksa dalam kontrol versi.

Namun, dalam konteks Kubernetes, mereka memiliki kekurangan dalam hal kecepatan dan keandalan dalam menambahkan node ke klaster. Jika proses penambahan node pekerja baru ke klaster menggunakan alat manajemen konfigurasi yang memerlukan instalasi paket melalui jaringan, ini akan memperlambat proses secara signifikan. Selain itu, kesalahan seperti repositori paket yang tidak tersedia atau variabel yang salah dapat menyebabkan kegagalan dalam konfigurasi, yang pada akhirnya dapat mengganggu keseluruhan proses penambahan node ke klaster.

## Citra Mesin

Menggunakan citra mesin adalah alternatif yang lebih baik. Dengan citra mesin yang sudah memiliki semua paket yang diperlukan, perangkat lunak siap dijalankan segera setelah mesin dinyalakan. Tidak ada instalasi paket yang bergantung pada jaringan atau repositori paket yang tersedia.

Keuntungan lain dari metode ini adalah Anda masih dapat menggunakan alat manajemen konfigurasi untuk membangun citra mesin. Sebagai contoh, dengan menggunakan Packer dari HashiCorp, Anda dapat menggunakan Ansible untuk membangun Amazon Machine Image (AMI) dan memiliki citra siap pakai yang dapat diterapkan ke instance kapan saja dibutuhkan.

Anda juga dapat menyimpan aset untuk pembuatan dalam kontrol versi dan memastikan bahwa semua aspek instalasi tetap deklaratif dan transparan. Saat pembaruan atau patch keamanan diperlukan, aset dapat diperbarui, dikomit, dan dijalankan secara otomatis melalui pipeline setelah penggabungan.

## Apa yang Harus Diinstal

### Sistem Operasi

Hal pertama yang diperlukan adalah sistem operasi. Distribusi Linux yang umum digunakan dan diuji dengan Kubernetes adalah pilihan yang aman. RHEL/CentOS atau Ubuntu adalah pilihan yang mudah. Jika Anda memiliki dukungan enterprise atau lebih menyukai distro lain, Anda dapat menggunakannya dengan tambahan waktu untuk pengujian dan pengembangan. Distribusi yang dirancang khusus untuk kontainer seperti Flatcar Container Linux juga merupakan pilihan yang baik.

### Runtime Kontainer

Selanjutnya, Anda memerlukan runtime kontainer seperti Docker atau containerd. Untuk menjalankan kontainer, Anda harus memiliki runtime kontainer.

### Kubelet

Berikutnya adalah kubelet, yang merupakan antarmuka antara Kubernetes dan kontainer yang dikelolanya. Kubelet diinstal pada setiap node dan mengoordinasikan kontainer yang dijalankan di dalamnya. Kubernetes sendiri berjalan dalam kontainer, tetapi kubelet berjalan sebagai proses pada host.

### Kubeadm dan Static Pods

Untuk melakukan bootstrap pada control plane Kubernetes, Anda memerlukan alat seperti kubeadm. Kubeadm adalah program command-line yang akan menghasilkan static Pod manifests untuk menjalankan komponen inti control plane Kubernetes seperti etcd, kube-apiserver, kube-scheduler, dan kube-controller-manager.

Setelah itu, kubelet akan menerima instruksi untuk membuat Pod dari manifest yang dikirimkan ke API Kubernetes. Kubeadm juga akan menghasilkan bootstrap token yang dapat digunakan untuk menghubungkan node lain ke klaster.

### Utilitas Bootstrap

Terakhir, Anda memerlukan utilitas bootstrap untuk mengelola konfigurasi runtime. Cluster API adalah proyek yang menggunakan custom resources dan controller Kubernetes untuk ini. Namun, program command-line yang diinstal di host juga bisa digunakan. Fungsinya adalah untuk menjalankan kubeadm dan mengelola konfigurasi runtime.

Sebagai contoh, di AWS, Anda dapat menggunakan user data untuk menjalankan utilitas bootstrap dan memberikan argumen yang mengontrol parameter yang diberikan ke kubeadm atau konfigurasi kubeadm.

Untuk memahami lebih lanjut tentang konfigurasi runtime yang diperlukan untuk utilitas bootstrap, lakukan instalasi Kubernetes secara manual menggunakan kubeadm. Dokumentasi resmi sangat membantu dalam hal ini.

---

**Dokumentasi dari halaman 46-49**

---
