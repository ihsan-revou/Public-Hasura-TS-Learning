**BAB 2: Model Deployment Kubernetes**

# **Pendahuluan**
Langkah pertama dalam menggunakan Kubernetes di lingkungan produksi adalah memastikan Kubernetes dapat berjalan dengan baik. Ini mencakup pemasangan sistem untuk menyediakan klaster Kubernetes serta pengelolaan pembaruan di masa mendatang. Berbeda dengan instalasi perangkat lunak biasa, Kubernetes sangat bergantung pada infrastruktur yang digunakan. Oleh karena itu, instalasi perangkat lunak dan infrastruktur harus direncanakan secara bersamaan.

Bab ini membahas berbagai pertimbangan sebelum menerapkan Kubernetes, termasuk penggunaan layanan terkelola dibandingkan membangun klaster sendiri. Jika menggunakan layanan terkelola, sebagian besar isi bab ini mungkin tidak relevan, tetapi tetap berguna untuk memahami berbagai alat yang tersedia dalam deployment Kubernetes.

---

# **Layanan Terkelola vs. Membangun Sendiri**

## **Layanan Terkelola (Managed Services)**
Menggunakan layanan Kubernetes terkelola mengurangi beban kerja tim teknik karena banyak aspek manajemen klaster ditangani oleh penyedia layanan cloud. Beberapa manfaat layanan terkelola meliputi:
- Pengurangan upaya rekayasa teknis dalam mengelola siklus hidup Kubernetes.
- Kontrol plane Kubernetes yang siap digunakan tanpa harus mengelola skalabilitas dan ketersediaannya secara langsung.
- Integrasi dengan layanan lain dalam ekosistem penyedia cloud, seperti IAM (Identity and Access Management), CloudWatch, dan Fargate (untuk AWS).

Layanan terkelola mirip dengan penggunaan database terkelola: jika fokus utama adalah aplikasi bisnis, menggunakan layanan terkelola memungkinkan pengembang untuk berfokus pada pengembangan aplikasi tanpa harus menangani administrasi infrastruktur.

Namun, layanan terkelola memiliki beberapa keterbatasan, seperti:
- Ketergantungan pada vendor tertentu (vendor lock-in).
- Keterbatasan fleksibilitas dalam mengkonfigurasi Kubernetes.
- Potensi inkonsistensi dalam fitur yang tersedia di berbagai penyedia cloud.

## **Membangun Klaster Kubernetes Sendiri**
Membangun Kubernetes sendiri memberikan fleksibilitas penuh dalam mengonfigurasi dan mengelola klaster. Beberapa manfaat utama dari pendekatan ini meliputi:
- Tidak tergantung pada penyedia layanan tertentu.
- Dapat menerapkan fitur Kubernetes secara lebih luas dan mendalam.
- Memiliki kontrol penuh terhadap versi Kubernetes yang digunakan.
- Dapat menerapkan kebijakan yang konsisten di berbagai infrastruktur, baik di cloud maupun di pusat data sendiri.

Namun, ada tantangan yang harus dihadapi, seperti:
- Membutuhkan pemahaman mendalam tentang Kubernetes.
- Membutuhkan tim dengan pengalaman teknis tinggi.
- Membutuhkan investasi sumber daya yang besar untuk membangun dan mengelola infrastruktur.

# **Memilih Pendekatan yang Tepat**
Jika mengalami kesulitan dalam memahami Kubernetes atau merasa bahwa manajemen sistem terdistribusi terlalu berisiko, layanan terkelola bisa menjadi pilihan terbaik. Namun, jika fleksibilitas lebih diutamakan dan ada kepercayaan rendah terhadap penyedia cloud, membangun Kubernetes sendiri adalah pilihan yang lebih tepat.

Jika memilih layanan terkelola, beberapa bagian bab ini dapat dilewati, kecuali bagian tentang add-ons dan mekanisme pemicu.

---

# **Automasi dalam Deployment Kubernetes**
Baik menggunakan layanan terkelola maupun membangun klaster sendiri, automasi menjadi elemen kunci dalam deployment Kubernetes. Automasi mengurangi biaya operasional, meningkatkan stabilitas, dan menghilangkan potensi kesalahan manusia dalam proses instalasi dan pemeliharaan klaster.

## **Menggunakan Installer Kubernetes**
Ada banyak installer Kubernetes yang tersedia, baik yang open-source maupun yang berbayar. Beberapa installer dapat dengan mudah digunakan untuk menyiapkan klaster Kubernetes hanya dengan beberapa klik. Jika pilihan ini sesuai dengan kebutuhan dan anggaran, maka menggunakan installer prebuilt bisa menjadi solusi yang efisien.

## **Automasi Kustom**
Meskipun menggunakan installer, sering kali tetap diperlukan beberapa automasi kustom untuk mengintegrasikan Kubernetes dengan sistem yang ada. Namun, membangun sistem deployment dari nol hanya direkomendasikan jika:
- Terdapat lebih dari satu atau dua engineer yang didedikasikan untuk tugas ini.
- Tim memiliki pengalaman mendalam dalam Kubernetes.
- Ada kebutuhan khusus yang tidak dapat dipenuhi oleh installer prebuilt atau layanan terkelola.

---

**Dokumentasi dari Hal 23-27**
