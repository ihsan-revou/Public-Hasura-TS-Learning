# Monitoring Hasura pada Kubernetes

## Gambaran Umum
Dokumen ini membahas aspek utama dalam melakukan monitoring terhadap Hasura di lingkungan Kubernetes. Komponen dan metrik inti sangat penting untuk memastikan layanan yang berjalan di server dioptimalkan untuk performa, terutama ketika mengintegrasikan Hasura dengan aplikasi *mobile frontend* (mengikuti REST API, bukan GraphQL).

## Aspek Monitoring Utama
Untuk memastikan server berjalan dengan baik, perlu dilakukan monitoring pada area berikut:

1. **RAM, CPU, dan Disk:** Ini adalah sumber daya dasar yang memungkinkan layanan berjalan.
2. **Jaringan (Network):** Penting untuk memonitor bandwidth jaringan, latency, dan throughput agar komunikasi antara layanan dari lokasi yang berbeda dapat berjalan dengan lancar.

### Latency
Latency di Hasura mengacu pada waktu yang dibutuhkan untuk memproses permintaan klien dan memberikan respons kembali. Dalam aplikasi yang berbasis Hasura, yang biasanya menggunakan GraphQL untuk berkomunikasi dengan database dan layanan lain, latency sangat penting karena langsung memengaruhi performa aplikasi.

Latency dibagi menjadi dua jenis, yaitu `latency query Hasura` dan `latency request API/Transaction`.
  
1. **Latency Query Hasura** adalah waktu yang dibutuhkan untuk memproses permintaan (query) GraphQL ke database. Ini mengukur seberapa cepat Hasura dapat mengambil data dari database.
  
2. **Latency Request API/Transaction** adalah waktu yang dibutuhkan untuk mengirim permintaan dari *frontend* (misalnya, aplikasi *mobile*) ke server Hasura, memprosesnya dengan GraphQL, dan mengembalikan respons ke *frontend*.
  
Contohnya, jika `latency query Hasura` adalah 20 ms (waktu yang dibutuhkan untuk GraphQL memproses query ke database) dan `latency request API` adalah 200 ms (waktu yang dibutuhkan dari *frontend* ke server Hasura dan kembali), maka `total latency` akan menjadi 220 ms (20 ms + 200 ms).
  
Ini berarti total waktu yang diperlukan untuk mendapatkan respons penuh dari permintaan API adalah 220 ms.
   
### Throughput
Throughput merujuk pada jumlah transaksi yang diproses dalam satu unit waktu tertentu. Fokusnya adalah pada volume permintaan yang bisa ditangani oleh server dalam satu detik, menit, atau jam. Monitoring throughput membantu dalam memahami beban sistem secara keseluruhan.  
**Contoh:**   
Jumlah satuan transaksi yang diproses dalam satu periode waktu. Misalnya, 250 transaksi per detik (TPS - Transactions Per Second).
  
Jika throughput untuk 1 data sebesar 2 MB, maka total throughput dapat mencapai 500 MB per detik tergantung pada jumlah transaksi yang diproses. Contoh: jika ada 250 transaksi per detik, dan masing-masing transaksi mengirimkan data sebesar 2 MB, maka total throughput akan menjadi 500 MB/detik.
  
Penjelasan yang lebih jelas: Throughput adalah ukuran jumlah data atau transaksi yang dapat diproses dalam satu periode waktu tertentu. Dalam konteks ini, throughput diukur dalam satuan transaksi per detik (TPS) atau total volume data yang diproses (misalnya, dalam megabyte atau gigabyte per detik).

### CPU (Central Processing Unit)
CPU bertanggung jawab untuk memproses permintaan. CPU biasanya terdiri dari core dan socket. Contohnya:

![Screenshot (259)](https://github.com/user-attachments/assets/0a5917c6-6675-4ff1-bd5e-3d5fab2f1e7d)

 
- CPU dengan 2 socket dan 4 core per socket akan memiliki total 8 core.
- Hasura melakukan banyak proses yang intensif di CPU.

#### Dampak Penggunaan CPU
Jika penggunaan CPU mencapai 100%, kinerja sistem akan melambat secara drastis. Oleh karena itu, penting untuk memonitor dan menyesuaikan sumber daya sesuai kebutuhan.

### RAM (Random Access Memory)
RAM menyimpan data sementara untuk eksekusi yang cepat. Jika penggunaan RAM melebihi kapasitas yang tersedia, sistem dapat mematikan proses dengan sinyal kill 9, yang dikenal sebagai **Out of Memory (OOM)** di lingkungan Java.

#### Mencegah Kesalahan OOM
Untuk mencegah kesalahan OOM, penting untuk menetapkan ambang batas (`threshold`) penggunaan memori dan memastikan sistem tidak melebihi RAM yang dialokasikan.

![Screenshot](https://github.com/user-attachments/assets/f4aad37f-da49-4cee-a612-2febf34687f8)

  
### Disk
Disk digunakan untuk menyimpan data jangka panjang atau permanen, namun tidak dapat melakukan operasi eksekusi. Kafka, misalnya, melakukan banyak proses berbasis disk, sementara Hasura lebih banyak menggunakan CPU.

### Monitoring Jaringan
Metrik jaringan yang penting meliputi:
- **Latency:** Mengukur penundaan dalam transmisi data antar layanan.
- **Throughput:** Memantau volume data yang ditransfer per unit waktu.

## Mendapatkan Metrik Hasura Menggunakan Postman
Setelah Anda melakukan deploy Hasura dan mengonfigurasi file YAML yang diperlukan, Anda dapat mengambil metrik melalui Postman.

### Langkah-langkah Mendapatkan Metrik Hasura:
1. Deploy Hasura dengan menerapkan konfigurasi YAML yang dibutuhkan.
   Langkah pertama yaitu menyalakan endpoint metric karena secara default, endpoint tersebut masih disable. berikut adalah env untuk enable metric :

```
HASURA_GRAPHQL_ENABLED_APIS=metadata,graphql,config,metrics
```

```
HASURA_GRAPHQL_METRICS_SECRET=<secret>
```

  amankan juga endpoint metric dengan token bearer yang diinginkan.   

  Berikut merupakan contoh configuration yang ditambahkan pada yaml deployment Hasura.

  ![image](https://github.com/user-attachments/assets/79277069-272d-4794-81e1-fa1bb1060798)
 
  **Note:** Metric Secret dapat diatur terlebih dahulu pada secret setelah itu panggil di deployment.  
  
3. Gunakan URL endpoint metrik Hasura di Postman.
   Gunakan URL Hasura Console kalian dan tambahkan `/v1/metrics` pada akhir url seperti berikut.
   **Contoh:**

```
   http://10.100.14.7:8085/v1/metrics
```

4. Set Method menjadi GET
     
5. Tambahkan header berikut ke dalam permintaan:
   - **Key:** Authorization
   - **Value:** Bearer `metric-secret` (ganti `metric-secret` dengan token secret metrics Anda yang sebenarnya).
   Implementasinya seperti pada gambar berikut.

![image](https://github.com/user-attachments/assets/2cf13e57-32be-4f2c-a36a-899402960fac)


6. Kirimkan Request
   Klik button `Send` untuk mengirimkan request HTTP ke Hasura Metrics. Hasil akan muncul seperti gambar berikut.

![image](https://github.com/user-attachments/assets/872c3c28-8423-4654-a0f6-cb0c28fd1102)


   Analisa metrics ada di dokumen berikutnya.
   
## Kesimpulan
Dengan memonitor CPU, RAM, Disk, dan sumber daya jaringan secara efektif, Anda dapat memastikan performa optimal untuk aplikasi Hasura yang berjalan di Kubernetes.
