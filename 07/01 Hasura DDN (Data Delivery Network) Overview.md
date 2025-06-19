
# Hasura DDN (Data Delivery Network) Overview

## 1. Apa itu Hasura DDN?
Hasura DDN (Data Delivery Network) adalah fitur terbaru di Hasura versi 3 (V3) yang memperluas kemampuan Hasura sebagai platform GraphQL untuk menghubungkan berbagai sumber data dengan cara yang lebih efisien. DDN dirancang untuk menangani kebutuhan skala besar dan arsitektur microservices dengan meningkatkan throughput data dan memastikan performa tinggi pada integrasi multi-sumber data. Fitur ini juga mempermudah pengelolaan data dari berbagai sumber, baik database relasional, non-relasional, maupun sumber data lain.

## 2. Arsitektur dan Cara Kerja
Hasura DDN bekerja dengan mengelola data dari beberapa sumber secara terdesentralisasi. DDN mengandalkan prinsip GraphQL Federation, yang memungkinkan data dari berbagai service diakses seolah-olah berasal dari satu sumber. DDN memetakan semua sumber data ke dalam satu endpoint yang dioptimalkan, memungkinkan aplikasi untuk melakukan query secara terdistribusi dengan cara yang lebih cepat dan aman.

### Cara Kerja:
1. **Pengumpulan Sumber Data**: Hasura DDN mengumpulkan data dari berbagai database atau API.
2. **GraphQL Federation**: Sumber data diintegrasikan ke dalam satu federasi GraphQL yang terdistribusi.
3. **Optimasi Query**: Query yang diterima oleh DDN dipecah secara optimal sehingga hanya data yang relevan diproses dan dikirim.
4. **Caching dan Pengoptimalan Latency**: Hasura DDN dilengkapi dengan caching bawaan untuk mempercepat pengiriman data dan mengurangi latency.

## 3. Perbedaan dengan Hasura V2
- **Hasura DDN (V3)**:
  - Mendukung GraphQL Federation dan integrasi multi-sumber data.
  - Performa query yang lebih baik dengan optimasi dan caching.
  - Skala yang lebih besar untuk menangani aplikasi dengan traffic tinggi.
  - Kemampuan untuk mengakses berbagai sumber data dalam satu query yang terfederasi.

- **Hasura V2**:
  - Fokus pada akses GraphQL ke satu atau beberapa database.
  - Tidak memiliki kemampuan GraphQL Federation.
  - Caching dan optimasi tidak sekuat di Hasura DDN.
  - Lebih cocok untuk aplikasi kecil hingga menengah dengan beban kerja yang lebih ringan.

## 4. Fitur yang Ada dan Tidak Ada di V3 dan V2

### Fitur di Hasura V3 (DDN):
- **GraphQL Federation**: Mendukung federasi data dari berbagai sumber.
- **Caching Terdistribusi**: Otomatis melakukan caching untuk mempercepat query.
- **Pengoptimalan Query**: Lebih efisien dalam pengelolaan query besar.
- **Support Multi-cloud**: Bisa diintegrasikan dengan berbagai penyedia cloud secara bersamaan.
- **Scaling**: Dukungan untuk scaling yang lebih besar dibandingkan V2.

### Fitur yang Tidak Ada di V3 (DDN):
- Fitur standar yang ada di V2 seperti basic query dan mutation tetap ada, namun dengan tambahan fitur federation dan caching.

### Fitur di Hasura V2:
- **Basic GraphQL Queries & Mutations**: Mendukung query sederhana untuk database tertentu.
- **Subscription**: Mendukung subscription real-time.
- **Remote Joins**: Menghubungkan berbagai data dengan Remote Joins tanpa federasi penuh.

### Fitur yang Tidak Ada di V2:
- **GraphQL Federation**: Hanya ada di V3.
- **Caching Terdistribusi**: Hanya ada di V3.

## 5. Keunggulan dan Kekurangan

### Keunggulan Hasura V3 (DDN):
- **Performa Lebih Baik**: Dengan caching dan optimasi query.
- **Skala yang Lebih Besar**: Mampu menangani aplikasi dengan traffic tinggi dan berbagai sumber data.
- **GraphQL Federation**: Kemampuan untuk menghubungkan berbagai sumber data dengan lebih efisien.
- **Integrasi Cloud**: Dukungan untuk integrasi multi-cloud secara lebih mulus.

### Kekurangan Hasura V3 (DDN):
- **Kompleksitas**: Lebih kompleks dibandingkan V2 dan membutuhkan konfigurasi yang lebih rumit.
- **Kebutuhan Resource Lebih Besar**: Membutuhkan resource yang lebih besar untuk caching dan federation.

### Keunggulan Hasura V2:
- **Sederhana dan Mudah**: Mudah digunakan untuk aplikasi skala kecil hingga menengah.
- **Ringan**: Tidak memerlukan resource yang besar.
- **Real-time Subscription**: Mendukung subscription real-time untuk aplikasi yang membutuhkan notifikasi instan.

### Kekurangan Hasura V2:
- **Skalabilitas Terbatas**: Tidak ideal untuk aplikasi yang membutuhkan skala besar.
- **Tidak Ada Federation**: Tidak mendukung federasi multi-sumber data.
- **Performa Caching**: Tidak memiliki caching terdistribusi.

