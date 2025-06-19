# Mekanisme Pemicu

Sekarang setelah kita melihat semua aspek yang perlu diselesaikan dalam model deployment Kubernetes Anda, penting untuk mempertimbangkan mekanisme pemicu yang mengotomatiskan proses instalasi dan manajemen, dalam bentuk apa pun itu. Baik Anda menggunakan layanan Kubernetes terkelola, installer yang sudah jadi, atau otomatisasi khusus yang dibangun dari nol, cara Anda memicu pembuatan klaster, penskalaan klaster, dan peningkatan klaster sangatlah penting.

Installer Kubernetes umumnya memiliki alat CLI yang dapat digunakan untuk memulai proses instalasi. Namun, jika alat tersebut digunakan secara terpisah, Anda tidak memiliki satu sumber kebenaran atau catatan inventaris klaster. Mengelola inventaris klaster menjadi sulit ketika Anda tidak memiliki daftar inventaris tersebut.

Pendekatan GitOps telah menjadi populer dalam beberapa tahun terakhir. Dalam pendekatan ini, sumber kebenaran adalah repositori kode yang berisi konfigurasi untuk klaster yang dikelola. Ketika konfigurasi untuk klaster baru dikomit, otomatisasi dipicu untuk menyediakan klaster baru. Ketika konfigurasi yang ada diperbarui, otomatisasi dipicu untuk memperbarui klaster, misalnya untuk meningkatkan jumlah node pekerja atau melakukan peningkatan Kubernetes dan add-ons klaster.

Pendekatan lain yang lebih sesuai dengan Kubernetes adalah merepresentasikan klaster dan dependensinya dalam sumber daya khusus Kubernetes dan kemudian menggunakan operator Kubernetes untuk merespons keadaan yang dideklarasikan dalam sumber daya khusus tersebut dengan menyediakan klaster. Ini adalah pendekatan yang digunakan oleh proyek seperti Cluster API. Dalam hal ini, sumber kebenaran adalah sumber daya Kubernetes yang disimpan dalam etcd di klaster manajemen. Namun, sering kali terdapat beberapa klaster manajemen untuk berbagai wilayah atau tingkatan. Di sini, pendekatan GitOps dapat digunakan bersama-sama, di mana manifes sumber daya klaster disimpan dalam kontrol sumber dan pipeline mengirimkan manifes ke klaster manajemen yang sesuai. Dengan cara ini, Anda mendapatkan keuntungan dari pendekatan GitOps dan pendekatan asli Kubernetes.

---

## Ringkasan

Saat mengembangkan model deployment untuk Kubernetes, pertimbangkan dengan cermat layanan terkelola atau installer Kubernetes yang sudah ada (gratis dan berlisensi) yang dapat Anda manfaatkan. Jadikan otomatisasi sebagai prinsip utama dalam semua sistem yang Anda bangun. Pahami dengan baik semua arsitektur dan topologi yang terlibat, terutama jika Anda memiliki persyaratan yang tidak umum. Pikirkan tentang dependensi infrastruktur dan cara mengintegrasikannya ke dalam proses deployment Anda. Pertimbangkan dengan saksama cara mengelola mesin yang akan membentuk klaster Anda. Pahami komponen terkontainerisasi yang akan membentuk control plane dari klaster Anda. Kembangkan pola yang konsisten untuk menginstal add-ons klaster yang akan menyediakan fitur penting bagi platform aplikasi Anda. Versikan platform Anda dan siapkan jalur manajemen serta peningkatan day-2 sebelum Anda menempatkan beban kerja produksi pada klaster Anda.

---

**Dokumentasi dari halaman 60-61**

---
