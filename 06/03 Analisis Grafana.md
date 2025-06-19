
# Gambaran Umum Grafana

**Grafana** adalah sebuah platform open-source untuk pemantauan, visualisasi, dan pemberian peringatan (alerting) pada data yang dikumpulkan dari berbagai sumber seperti basis data, sistem log, aplikasi, dan lainnya. Grafana digunakan untuk membuat **dashboard** interaktif yang menampilkan visualisasi data secara real-time dan memantau sistem.

## Fungsi Utama Grafana:
1. **Monitoring dan Observability**: Memantau performa aplikasi dan sistem secara real-time.
2. **Visualisasi Data**: Menyediakan berbagai cara untuk menampilkan data, termasuk grafik, tabel, dan diagram.
3. **Alerting**: Mengirim peringatan berdasarkan ambang batas yang telah ditentukan pada metrik penting.

Monitoring Dashboard :
- **Hasura HTTP Graphql**
- **Hasura Health**

Tugas:
1. Lakukan Hit di API Hasura mas Ferdy
2. Lakukan Analiasa Panel

## 1. Hasura HTTP Graphql
**Dashboard** ini menyediakan gambaran menyeluruh tentang performa query dan mutation di Hasura, termasuk metrik seperti latency, error rate, dan transfer data. Anda dapat menggunakan data ini untuk memantau performa aplikasi secara real-time dan melakukan analisis jika ada masalah performa atau error. 

![image](https://github.com/user-attachments/assets/df77d06a-4b4b-4316-9869-b0b884fec371)

### 1. Total Queries
**Fungsi**: Metrik ini menunjukkan jumlah total query yang dieksekusi di dashboard atau panel tertentu. Ini adalah metrik penting untuk mengamati **beban sistem** dan mengidentifikasi potensi bottleneck.  

![image](https://github.com/user-attachments/assets/4d69ab3b-ec32-4c2d-8f4b-fe2cca3864fc)

  
Angka ini menunjukkan total permintaan GraphQL yang diajukan ke server Hasura dalam periode waktu yang dipilih. Di sini terlihat bahwa ada 54 permintaan.  

### 2. Query Latency (P95):
**Fungsi**: Menyediakan metrik yang berguna untuk mengukur performa aplikasi. P95 lebih mencerminkan latensi dalam skenario dunia nyata dibandingkan rata-rata (mean), karena ia memperhitungkan lonjakan dan outlier yang dapat mempengaruhi pengalaman pengguna.  

![image](https://github.com/user-attachments/assets/eaadc894-a339-4dd4-b1df-ad42ffc5adbc)


Bagian ini menunjukkan latency (waktu tunda) untuk permintaan query GraphQL yang masuk. Angka "P95" artinya latensi di percentil ke-95, yaitu latensi maksimum yang dialami 95% dari total query. Di sini ditampilkan bahwa latensinya adalah 42.2 ms.  
  
### 3. Total Mutations:
**Fungsi**: Berguna untuk memahami seberapa sering data berubah di sistem. Banyaknya mutation bisa berarti pengguna aktif memodifikasi data atau aplikasi banyak melakukan operasi backend.

![image](https://github.com/user-attachments/assets/65682042-7646-40f1-ae7c-b179d198c4e0)

  
Menunjukkan jumlah total mutation (modifikasi data) yang dilakukan melalui query GraphQL dalam periode waktu yang dipilih. Di sini ada satu mutation.  

### 4. Mutation Latency (P95):
**Fungsi**: Latensi rendah di sini berarti mutation diproses dengan cepat, yang penting dalam aplikasi yang membutuhkan data yang selalu diperbarui secara real-time atau hampir real-time.

![image](https://github.com/user-attachments/assets/77681e56-155d-48a3-bc06-39bdf49e73bf)


Mirip dengan Query Latency, bagian ini menunjukkan latency dari operasi mutation dengan nilai percentile ke-95. Latensinya saat ini adalah 9.50 ms.

### 5. Top Queries:
**Fungsi**: Membantu mengidentifikasi query mana yang paling sering digunakan atau mengonsumsi banyak sumber daya. Ini bermanfaat untuk mengoptimalkan atau memperbaiki performa dari query tersebut.

![image](https://github.com/user-attachments/assets/612be44e-051a-418d-8a3a-69dbf5e5e481)


Menampilkan layanan atau operasi GraphQL yang paling sering digunakan selama periode yang dipilih. Dalam contoh ini, layanan "hasuraferdy" yang melakukan dua query.

### 6. Top Mutations:
**Fungsi**: Berguna untuk mengetahui operasi perubahan data yang paling sering dilakukan. Misalnya, jika ada mutation spesifik yang sering terjadi, Anda bisa memantau apakah ada masalah kinerja terkait.

![image](https://github.com/user-attachments/assets/984a5776-daa6-40fe-b7fa-b6b2979787e4)

  
Dalam hal ini adalah daftar mutation yang paling sering dilakukan dalam periode waktu yang ditentukan, tetapi di sini tidak ada data yang ditampilkan.  

### 7.Query Request Rate:
**Fungsi**: Berguna untuk melacak seberapa banyak permintaan yang masuk ke server dalam kurun waktu tertentu. Lonjakan pada request rate bisa menunjukkan lonjakan aktivitas pengguna.

![image](https://github.com/user-attachments/assets/1ab116d5-6079-4b80-9c32-f1b61d0810e9)


Grafik ini menampilkan jumlah permintaan query yang diajukan ke server Hasura per satuan waktu. Pada grafik ini, tidak ada permintaan yang terlihat pada interval waktu terakhir.

### 8. Mutation Request Rate:
**Fungsi**: Memantau frekuensi perubahan data. Metrik ini bisa membantu memahami kapan aplikasi melakukan banyak operasi perubahan data, yang mungkin berdampak pada kinerja.  

![image](https://github.com/user-attachments/assets/199137ec-1a69-4cbd-a88e-4f669cd835a0)


Grafik ini menampilkan frekuensi permintaan mutation yang dilakukan selama periode waktu tertentu. Pada gambar ini juga tidak ada data yang terlihat.

### 9. Query Error Rate:
**Fungsi**: Sangat penting untuk melacak error. Jika tingkat error tinggi, ini bisa menunjukkan masalah pada aplikasi, misalnya karena query yang tidak valid atau masalah dengan infrastruktur.

![image](https://github.com/user-attachments/assets/2fc9a2d1-2f58-495f-9384-9459fd5f2329)


Grafik ini menunjukkan frekuensi error yang terjadi selama pemrosesan query GraphQL. Pada gambar ini, tidak ada error yang tercatat.

### 10. Mutation Error Rate:
**Fungsi**: Memantau error dalam mutation penting karena kegagalan mutation bisa menyebabkan data yang salah atau tidak konsisten. Hal ini juga dapat mengganggu pengalaman pengguna aplikasi.

![image](https://github.com/user-attachments/assets/b04d5463-a083-413d-b09a-a575fe81dc5e)


Menunjukkan jumlah error yang terjadi selama operasi mutation. Tidak ada error yang tercatat di sini.

### 11. Query Latency (P95) (Detail per waktu):
**Fungsi**: Bermanfaat untuk melihat pola perubahan latensi. Misalnya, lonjakan pada waktu tertentu mungkin menunjukkan waktu beban puncak atau masalah performa yang bersifat sementara.

![image](https://github.com/user-attachments/assets/14a60a49-ced2-4c22-b2cd-98f9b876f479)


Grafik ini memetakan latensi query ke waktu selama periode tertentu, membantu Anda melihat bagaimana latensi berubah sepanjang waktu. Ada beberapa lonjakan latensi, tetapi secara umum tetap rendah.

### 12. Mutation Latency (P95) (Detail per waktu):
**Fungsi**: Berguna untuk memastikan operasi mutation tetap cepat dan tidak mengalami penundaan selama penggunaan.

![image](https://github.com/user-attachments/assets/1bc7bcd2-74dd-4a36-98d5-7a3af5714b9b)

  
Grafik yang sama seperti pada Query Latency, tetapi untuk operasi mutation. Di sini mutation tampaknya jarang terjadi, dengan beberapa titik data pada grafik.

### 13. HTTP Connections:
**Fungsi**: Memantau jumlah koneksi membantu mengidentifikasi beban pada server. Peningkatan tajam dalam jumlah koneksi mungkin mengindikasikan peningkatan lalu lintas atau adanya masalah dengan penanganan sesi koneksi.

![image](https://github.com/user-attachments/assets/fae5fd6f-c0e8-4a96-9444-f83a86ebb2df)


Grafik ini menunjukkan jumlah koneksi HTTP yang sedang berlangsung antara klien dan server Hasura. Jumlahnya bervariasi, tetapi sebagian besar stabil di sekitar 1 koneksi.

### 14. Cache Request Rate:
**Fungsi**: Pemanfaatan cache yang baik bisa meningkatkan performa aplikasi. Ketiadaan data di sini mungkin menunjukkan bahwa cache tidak aktif atau tidak dioptimalkan.

![image](https://github.com/user-attachments/assets/325f9108-c87c-4160-96de-b9a648f233a8)


Grafik ini seharusnya menunjukkan permintaan yang masuk ke cache, tetapi saat ini tidak ada data yang tercatat di sini.

### 15. HTTP Data Transfer:
**Fungsi**: Metrik ini bisa memberikan wawasan tentang berapa banyak data yang dikirim dan diterima aplikasi. Jika ada transfer data yang besar, itu bisa menjadi indikator penggunaan berat oleh pengguna atau operasi besar di aplikasi.

![image](https://github.com/user-attachments/assets/8af0adb0-f420-43c7-ad19-3ab4db95d89f)


Grafik ini menampilkan jumlah data yang ditransfer melalui HTTP selama periode waktu tertentu. Tampak ada fluktuasi pada transfer data di berbagai waktu.

### 16. Action Data Transfer:
**Fungsi**: Memantau action di Hasura membantu memastikan bahwa data yang terkait dengan operasi spesifik berjalan dengan lancar dan efisien.

![image](https://github.com/user-attachments/assets/2a303715-2383-42d5-9a80-4d0c34404c42)


Ini menampilkan data yang ditransfer terkait dengan action di Hasura. Namun, tidak ada data yang tercatat pada grafik ini.

### 17. Source (Dropdown):
**Fungsi**: Memilih sumber metrik untuk ditampilkan di dashboard. Misalnya, jika Anda mengumpulkan metrik dari beberapa monitoring system seperti Prometheus atau InfluxDB, Anda dapat memilih salah satunya.

![image](https://github.com/user-attachments/assets/4e291bec-b956-4b40-93e1-571c5f27af07)


Filter ini memungkinkan pengguna untuk memilih sumber data (source) dari mana metrik diambil. Dalam konteks ini, metrik yang digunakan berasal dari Prometheus. Dengan filter ini, Anda bisa memutuskan untuk melihat metrik dari sumber yang berbeda jika ada beberapa sumber data yang terhubung.

### 18. Job (Dropdown):
**Fungsi**: Anda bisa memilih job yang ingin dipantau performanya. Jika Anda menjalankan beberapa aplikasi pada Kubernetes cluster, Anda dapat memilih hanya satu aplikasi untuk dilihat.

![image](https://github.com/user-attachments/assets/1c9a400a-7f53-4e36-8f99-91527ce48585)


Filter ini memberikan opsi untuk memilih pekerjaan (job) tertentu yang dimonitor oleh Prometheus. Sebuah job biasanya mengacu pada aplikasi atau proses spesifik yang terpantau, misalnya aplikasi Hasura atau PostgreSQL.

### 19. Instance (Dropdown):
**Fungsi**: Memilih instansi tertentu untuk difokuskan. Ini memudahkan pengguna dalam mengisolasi masalah performa pada satu server atau node tertentu.

![image](https://github.com/user-attachments/assets/7744e9fd-589f-4f39-9234-9d02ba86b5e5)


Filter ini memungkinkan pengguna untuk memilih instansi (instance) tertentu dari sebuah job. Sebagai contoh, jika Anda menjalankan beberapa instansi aplikasi Hasura di beberapa node, Anda dapat memilih salah satu node atau server tertentu yang ingin Anda pantau.

### 20. Time Range Selector (Last 15 Minutes):
**Fungsi**: Memilih rentang waktu tertentu untuk melihat bagaimana performa aplikasi atau sistem dalam periode tersebut. Ini berguna untuk melihat perubahan performa atau potensi masalah dalam interval waktu tertentu.

![image](https://github.com/user-attachments/assets/d55df755-28ba-424c-a807-c6679817f725)


Filter ini mengatur rentang waktu (time range) dari data yang ditampilkan pada dashboard. Secara default, dashboard akan menampilkan data dari 15 menit terakhir, namun pengguna bisa memilih rentang waktu lainnya, seperti 1 jam terakhir, 24 jam terakhir, atau membuat rentang waktu kustom.

### 21. Operation Type (Dropdown):
**Fungsi**: Sangat berguna untuk memisahkan jenis operasi yang ingin Anda pantau. Misalnya, jika Anda hanya ingin melihat metrik dari Query tanpa melibatkan Mutation atau Subscription, filter ini sangat membantu.

![image](https://github.com/user-attachments/assets/164121b8-63a7-448f-8c04-b477dc46f78b)


Ini memungkinkan pengguna untuk memfilter jenis operasi GraphQL yang sedang dipantau, seperti Query, Mutation, atau Subscription.

### 22. Operation Name (Dropdown):
**Fungsi**: Berguna saat Anda ingin memantau kinerja atau kesehatan dari operasi GraphQL tertentu, seperti operasi getUser atau createPost.

![image](https://github.com/user-attachments/assets/d7db70fc-a334-426d-bddc-f8b12e1bd85d)


Dropdown ini memungkinkan pengguna memilih berdasarkan nama operasi yang spesifik. Dalam GraphQL, operasi sering diberi nama untuk memudahkan identifikasi.

### 23. Query Hash (Dropdown):
**Fungsi**: Dapat digunakan untuk melacak kinerja query unik secara spesifik jika ada banyak query yang mirip tetapi ingin dipantau secara terpisah.

![image](https://github.com/user-attachments/assets/87b7d75d-a28b-4601-986e-0e2f6470bc7b)


Filter ini memisahkan metrik berdasarkan hash dari query yang digunakan. Query hash merupakan identifikasi unik dari query GraphQL yang dijalankan.

### 24. 5s (Refresh Interval):
**Fungsi**: Berguna untuk memastikan dashboard terus diperbarui dengan data terbaru secara otomatis. Interval ini bisa diubah untuk penyegaran yang lebih lambat atau lebih cepat sesuai kebutuhan.

![image](https://github.com/user-attachments/assets/b25e04ef-cee5-4774-a561-32bcab3c9d8b)


Dropdown ini mengatur interval waktu penyegaran otomatis dashboard, dalam hal ini setiap 5 detik.

### 25. Refresh Dashboard (Button):
**Fungsi**: Berguna jika Anda ingin langsung mendapatkan data terbaru tanpa menunggu interval penyegaran otomatis.

![image](https://github.com/user-attachments/assets/8e64b7bc-1e49-41d4-baaa-9c266967dfcf)


Tombol ini digunakan untuk menyegarkan dashboard secara manual.

### 26. Time Range Zoom Out (Button):
**Fungsi**: Membantu Anda mendapatkan gambaran yang lebih luas mengenai data metrik, misalnya dari rentang waktu yang lebih lama, untuk menganalisis tren performa.

![image](https://github.com/user-attachments/assets/f9af677a-cc47-483f-96d7-b5367a890544)


Tombol ini memperbesar rentang waktu tampilan data.

### 27. Save Dashboard (Button):
**Fungsi**: Sangat berguna jika Anda telah mengonfigurasi berbagai filter dan tampilan yang ingin disimpan untuk referensi atau penggunaan di masa depan.

![image](https://github.com/user-attachments/assets/abbccf77-8ff5-44ad-a3ce-957aa67568fe)


Fitur ini digunakan untuk menyimpan pengaturan dashboard yang telah Anda sesuaikan.

### 28. Dashboard Settings (Button):
**Fungsi**: Fitur ini memberikan kontrol lebih lanjut untuk mengedit struktur dashboard atau konfigurasi visual yang lebih mendalam.

![image](https://github.com/user-attachments/assets/156a3edd-85fc-4473-bafd-7e794ec0ec20)


Ini memungkinkan akses ke pengaturan dashboard seperti tata letak, penyesuaian metrik, atau pengelolaan data yang lebih mendetail.

### 29. Dropdown Add

![image](https://github.com/user-attachments/assets/30e09b59-454d-432d-9d5d-23f95697fe5d)


Dropdown Add ini digunakan untuk menambahkan elemen baru pada dashboard Grafana. Berikut penjelasan setiap opsinya:

#### 1. Visualization:
**Fungsi**: Setelah memilih ini, Anda dapat menentukan tipe visualisasi data yang diinginkan seperti grafik, bar, tabel, heatmap, dan lain-lain, sesuai dengan data yang akan dimonitor.
Opsi ini digunakan untuk menambahkan panel visualisasi baru pada dashboard.

#### 2. Row:
**Fungsi**: Berguna untuk mengorganisir panel-panel pada dashboard secara rapi dengan menempatkannya dalam baris tertentu. Ini membantu dalam pengelompokan panel yang terkait.
Opsi ini digunakan untuk menambahkan baris baru pada layout dashboard.

#### 3. Import from library:
**Fungsi**: Sangat berguna jika Anda ingin menggunakan panel yang sudah ada atau standar perusahaan yang telah disimpan dalam pustaka, sehingga tidak perlu membuat panel dari awal.
Opsi ini memungkinkan Anda untuk mengimpor panel dari pustaka panel yang telah disimpan atau dibuat sebelumnya.

#### 4. Paste panel:
**Fungsi**: Ini berguna saat Anda ingin menduplikasi panel dari dashboard lain atau dari lokasi lain di dalam dashboard yang sama.
Opsi ini memungkinkan Anda untuk menempelkan panel yang sudah di-copy sebelumnya.

## 2. Hasura Health
**Dashboard** ini berfokus pada kesehatan dan performa layanan Hasura, khususnya dalam memastikan metadata konsisten, memeriksa status kesehatan sumber daya, serta memonitor koneksi ke database PostgreSQL. Memantau status ini membantu memastikan bahwa layanan berjalan dengan baik dan siap untuk menangani permintaan dengan efektif.

![image](https://github.com/user-attachments/assets/36983f5f-f4bc-4aba-b23f-c368b2d672ee)

### 1. Metadata Status:
**Fungsi**: Ini penting karena metadata di Hasura digunakan untuk mengelola skema GraphQL dan pengaturan seperti hubungan antar tabel. Status konsisten memastikan tidak ada konflik atau masalah.

![image](https://github.com/user-attachments/assets/ff2ef1e6-63d9-4b13-bb52-f0f8428dcb5d)


Panel ini menunjukkan status metadata di Hasura, dengan status "CONSISTENT" yang berarti bahwa metadata yang digunakan oleh Hasura sinkron dan tidak ada ketidaksesuaian.

### 2. Health Check using Infinity (OK):
**Fungsi**: Pengecekan kesehatan menggunakan Infinity dapat menunjukkan apakah sumber daya yang terkait masih berfungsi dengan baik dan dapat berkomunikasi dengan Hasura tanpa masalah.

![image](https://github.com/user-attachments/assets/2cc2b09d-5c09-4aa9-905e-ae025bd5c03b)


Ini menunjukkan hasil pengecekan kesehatan menggunakan Infinity yang terhubung ke Hasura. Status "OK" berarti pengecekan kesehatan berhasil dan tidak ada masalah.

### 3. Health Check using blackbox (No Data):
**Fungsi**: Pengecekan ini biasanya mengindikasikan apakah alat atau layanan eksternal dapat berkomunikasi dengan baik. Ketiadaan data mungkin menunjukkan bahwa tes belum dilakukan atau ada masalah dengan alat monitoring.

![image](https://github.com/user-attachments/assets/2f032b21-b0f4-4bb8-9e2d-316403a8fe60)


Panel ini seharusnya menunjukkan hasil pengecekan kesehatan menggunakan blackbox, tetapi saat ini tidak ada data yang tersedia.

### 4. Source Health Check:
**Fungsi**: Sumber data yang sehat berarti bahwa Hasura dapat berkomunikasi dengan basis data tanpa masalah. Ini penting untuk memastikan semua data dapat diakses dengan lancar.

![image](https://github.com/user-attachments/assets/b4e547b1-df9c-4fa5-b14d-a355fb4361d1)


Panel ini menampilkan status kesehatan sumber data yang terhubung ke Hasura, dalam hal ini ada dua sumber yaitu default (hasuraferdy) dan testsejuta (hasuraferdy) yang keduanya dalam status "OK".

### 5. Metadata Version:
**Fungsi**: Ini membantu melacak perubahan pada metadata. Setiap perubahan pada skema atau pengaturan akan meningkatkan versi metadata.

![image](https://github.com/user-attachments/assets/b3180022-d2ad-4a7d-824e-02207de33998)


Menunjukkan versi metadata yang sedang digunakan oleh Hasura. Dalam gambar ini, versinya adalah 292.

### 6. Health Check Latency:
**Fungsi**: Memantau latensi pengecekan kesehatan penting untuk mengetahui seberapa cepat sistem dapat menanggapi masalah, yang memengaruhi performa secara keseluruhan.

![image](https://github.com/user-attachments/assets/9a7be9a1-8c64-4147-843d-2979efc2a791)


Panel ini seharusnya menunjukkan waktu tunda (latency) dari pengecekan kesehatan. Namun, pada gambar ini, tidak ada data yang tersedia.

### 7. Postgres Connections:
**Fungsi**: Jumlah koneksi ke PostgreSQL menunjukkan seberapa banyak aplikasi atau layanan yang sedang berinteraksi dengan database. Lonjakan jumlah koneksi bisa menunjukkan adanya peningkatan beban atau masalah yang harus segera ditangani.

![image](https://github.com/user-attachments/assets/35831a99-47f6-48ea-97a4-1f5e7f350f5b)


Grafik ini menunjukkan jumlah koneksi aktif ke PostgreSQL yang terkait dengan sumber testsejuta dan default dalam kurun waktu yang ditentukan.

### 8. Source (Dropdown):
**Fungsi**: Dengan memilih source, pengguna dapat memfokuskan tampilan dashboard hanya pada data yang dikumpulkan oleh sumber tertentu. Ini memudahkan pengguna untuk memantau layanan yang relevan tanpa perlu melihat data dari semua sumber sekaligus.

![image](https://github.com/user-attachments/assets/e8e05142-0783-470a-b1b6-79cafb013950)


Dropdown ini memungkinkan pengguna memilih source data mana yang ingin ditampilkan. Source di sini merujuk pada sistem monitoring seperti Prometheus, yang bertanggung jawab mengumpulkan dan menyajikan data metrik.

### 9. Job (Dropdown):
**Fungsi**: Memilih job tertentu membantu pengguna untuk mempersempit fokus pemantauan hanya pada proses-proses spesifik yang penting bagi mereka, seperti hanya melihat job untuk database tanpa melihat aplikasi.

![image](https://github.com/user-attachments/assets/74cc2423-6f42-4c83-a256-f6f952595a7b)


Dropdown ini menyediakan pilihan untuk memilih job tertentu yang dipantau. Job merujuk pada proses atau layanan tertentu yang dikonfigurasi dalam monitoring, seperti proses aplikasi Hasura atau database PostgreSQL.

### 10. Instance (Dropdown):
**Fungsi**: Menggunakan filter instance membantu memantau performa dan kesehatan dari satu server tertentu, tanpa terganggu oleh data dari server lainnya.

![image](https://github.com/user-attachments/assets/22cfa64b-64f5-4be6-9c0d-37b61d5fd82c)


Dropdown ini memungkinkan pengguna memilih instance atau server spesifik yang sedang dipantau. Ini berguna ketika ada beberapa instansi yang dikelola, seperti beberapa server Hasura yang berjalan secara paralel.

### 11. Time Range Selector (Last 15 Minutes):
**Fungsi**: Fitur ini berguna untuk menganalisis tren performa atau kejadian dalam rentang waktu tertentu, membantu pengguna untuk melihat pola atau masalah yang baru saja terjadi.

![image](https://github.com/user-attachments/assets/732c5237-2ca8-4e7d-b290-6f8b553244de)


Dropdown ini memungkinkan pengguna memilih jangka waktu data yang ditampilkan, seperti 15 menit terakhir, 1 jam terakhir, atau bahkan rentang waktu kustom.

### 12. 5s (Refresh Interval):
**Fungsi**: Berguna untuk memastikan dashboard terus diperbarui dengan data terbaru secara otomatis. Interval ini bisa diubah untuk penyegaran yang lebih lambat atau lebih cepat sesuai kebutuhan.

![image](https://github.com/user-attachments/assets/b25e04ef-cee5-4774-a561-32bcab3c9d8b)


Dropdown ini mengatur interval waktu penyegaran otomatis dashboard, dalam hal ini setiap 5 detik.

### 13. Refresh Dashboard (Button):
**Fungsi**: Berguna jika Anda ingin langsung mendapatkan data terbaru tanpa menunggu interval penyegaran otomatis.

![image](https://github.com/user-attachments/assets/8e64b7bc-1e49-41d4-baaa-9c266967dfcf)


Tombol ini digunakan untuk menyegarkan dashboard secara manual.

### 14. Time Range Zoom Out (Button):
**Fungsi**: Membantu Anda mendapatkan gambaran yang lebih luas mengenai data metrik, misalnya dari rentang waktu yang lebih lama, untuk menganalisis tren performa.

![image](https://github.com/user-attachments/assets/f9af677a-cc47-483f-96d7-b5367a890544)


Tombol ini memperbesar rentang waktu tampilan data.

### 15. Save Dashboard (Button):
**Fungsi**: Sangat berguna jika Anda telah mengonfigurasi berbagai filter dan tampilan yang ingin disimpan untuk referensi atau penggunaan di masa depan.

![image](https://github.com/user-attachments/assets/abbccf77-8ff5-44ad-a3ce-957aa67568fe)


Fitur ini digunakan untuk menyimpan pengaturan dashboard yang telah Anda sesuaikan.

### 16. Dashboard Settings (Button):
**Fungsi**: Fitur ini memberikan kontrol lebih lanjut untuk mengedit struktur dashboard atau konfigurasi visual yang lebih mendalam.

![image](https://github.com/user-attachments/assets/156a3edd-85fc-4473-bafd-7e794ec0ec20)


Ini memungkinkan akses ke pengaturan dashboard seperti tata letak, penyesuaian metrik, atau pengelolaan data yang lebih mendetail.

### 17. Dropdown Add

![image](https://github.com/user-attachments/assets/30e09b59-454d-432d-9d5d-23f95697fe5d)


Dropdown Add ini digunakan untuk menambahkan elemen baru pada dashboard Grafana. Berikut penjelasan setiap opsinya:

#### 1. Visualization:
**Fungsi**: Setelah memilih ini, Anda dapat menentukan tipe visualisasi data yang diinginkan seperti grafik, bar, tabel, heatmap, dan lain-lain, sesuai dengan data yang akan dimonitor.
Opsi ini digunakan untuk menambahkan panel visualisasi baru pada dashboard.

#### 2. Row:
**Fungsi**: Berguna untuk mengorganisir panel-panel pada dashboard secara rapi dengan menempatkannya dalam baris tertentu. Ini membantu dalam pengelompokan panel yang terkait.
Opsi ini digunakan untuk menambahkan baris baru pada layout dashboard.

#### 3. Import from library:
**Fungsi**: Sangat berguna jika Anda ingin menggunakan panel yang sudah ada atau standar perusahaan yang telah disimpan dalam pustaka, sehingga tidak perlu membuat panel dari awal.
Opsi ini memungkinkan Anda untuk mengimpor panel dari pustaka panel yang telah disimpan atau dibuat sebelumnya.

#### 4. Paste panel:
**Fungsi**: Ini berguna saat Anda ingin menduplikasi panel dari dashboard lain atau dari lokasi lain di dalam dashboard yang sama.
Opsi ini memungkinkan Anda untuk menempelkan panel yang sudah di-copy sebelumnya.

## 3. Additional Menu Dashboard

![Screenshot (278)](https://github.com/user-attachments/assets/731dc79d-78ce-4e34-b49b-e5c0705022d3)

Dropdown ini menyediakan berbagai opsi untuk berinteraksi dengan panel di Grafana. Berikut penjelasan dari masing-masing fitur:

### 1. View:
**Pengertian**: Opsi ini memungkinkan Anda untuk melihat panel dalam mode tampilan.  
**Fungsi**: Menampilkan panel tanpa opsi edit, hanya untuk melihat data yang ada. Anda juga bisa menekan tombol shortcut v untuk langsung masuk ke mode ini.

### 2. Edit:
**Pengertian**: Mengaktifkan mode pengeditan untuk panel.  
**Fungsi**: Membuka editor panel di mana Anda bisa mengubah pengaturan visualisasi, sumber data, atau query yang mendasari panel. Shortcut e dapat digunakan untuk akses cepat.

### 3. Share:
**Pengertian**: Menampilkan opsi untuk membagikan panel atau dashboard.  
**Fungsi**: Anda bisa membagikan panel dengan membuat link atau menyematkannya ke halaman lain. Ada juga opsi untuk membagikan dashboard secara keseluruhan. Shortcut p s bisa digunakan untuk membuka fitur ini.

### 4. Explore:
**Pengertian**: Memungkinkan Anda untuk mengeksplorasi lebih lanjut data yang ada pada panel.  
**Fungsi**: Menggunakan mode eksplorasi data di Grafana, di mana Anda bisa melihat lebih detail query dan data yang terkait. Shortcut x dapat digunakan untuk masuk ke mode eksplorasi ini.

### 5. Inspect:
**Fungsi**: Anda bisa melihat data mentah, query yang digunakan, atau log terkait dengan panel ini. Fitur ini sering digunakan untuk troubleshooting. Shortcut i memungkinkan akses cepat ke fitur ini.  

![Screenshot (276)](https://github.com/user-attachments/assets/bdf46464-bdfd-412a-865a-36114506944e)

Membuka menu untuk menginspeksi data yang digunakan dalam panel.  

Menu Inspect ini menyediakan opsi untuk menganalisis lebih dalam tentang data yang ditampilkan pada panel di Grafana. Berikut adalah penjelasan dari setiap sub-menu di dalamnya:

#### 1. Data:
**Pengertian**: Menampilkan data mentah yang digunakan oleh panel ini.  
**Fungsi**: Anda bisa melihat hasil query yang sedang digunakan dalam bentuk tabel, grafik, atau format mentah lainnya. Fitur ini bermanfaat untuk melihat apakah data yang diterima sudah sesuai dengan yang diharapkan atau tidak. Fitur ini sering digunakan untuk menganalisis data mentah dari query yang diproses dalam panel. Selain itu, fitur ini mempermudah pengguna untuk mendapatkan laporan data dalam format yang dapat dibagikan atau dianalisis lebih lanjut di luar Grafana.

![image](https://github.com/user-attachments/assets/d179b61c-7d61-43a8-8163-27dc8cc639d4)


Fitur ini adalah bagian dari Inspect panel di Grafana yang digunakan untuk menganalisis lebih dalam data yang ditampilkan di panel. Dalam tab Data dari fitur Inspect, ada beberapa opsi dan informasi yang bisa Anda eksplorasi:
  
##### 1. Show Data Frame:  
**Fungsi**: Dropdown ini memungkinkan Anda memilih frame data yang ingin dilihat. Dalam contoh gambar, hanya ada satu frame data yang disebut "Total (1)".  
**Penggunaan**: Anda bisa memilih frame yang berisi data yang relevan, terutama jika ada beberapa frame yang tersedia di panel.
  
##### 2. Formatted Data:
**Fungsi**: Ketika opsi ini diaktifkan, data yang ditampilkan akan diformat sesuai dengan aturan tampilan dan override yang diterapkan di panel asli.    
**Penggunaan**: Menampilkan data dalam format yang lebih rapi sesuai pengaturan panel, berguna untuk visualisasi yang lebih mudah dipahami.
  
##### 3. Download CSV:
**Fungsi**: Anda dapat mengunduh data yang sedang ditampilkan dalam format CSV untuk dianalisis lebih lanjut di Excel atau alat spreadsheet lainnya.  
**Penggunaan**: Berguna ketika Anda ingin melakukan analisis lanjutan atau menyimpan data untuk pelaporan.

##### 4. Download for Excel:
**Fungsi**: Opsi ini memungkinkan untuk mendownload file CSV dengan format khusus yang lebih mudah diimpor ke dalam Excel, termasuk header tambahan.  
**Penggunaan**: Membuat proses download lebih nyaman untuk pengguna Excel, terutama untuk memudahkan manipulasi data.

##### 5. Data Table:
**Fungsi**: Tabel yang menampilkan data aktual dari query yang dilakukan oleh panel. Dalam contoh, data berisi Time (waktu pengambilan data) dan Total (nilai total dari data yang dikumpulkan).
**Penggunaan**: Berguna untuk melihat rincian hasil query secara langsung, terutama untuk menganalisis pola data dalam kurun waktu tertentu.

#### 2. Query:
**Pengertian**: Menunjukkan query yang digunakan untuk menghasilkan data pada panel.  
**Fungsi**: Berguna untuk memeriksa dan memvalidasi query yang Anda gunakan untuk mengambil data. Ini termasuk query yang digunakan dari sumber data seperti Prometheus, InfluxDB, atau lainnya.

![image](https://github.com/user-attachments/assets/0cd66a56-414e-49ba-b2cf-d66ed7b0c2a2)

Gambar di atas menunjukkan antarmuka Query Inspector di Grafana. Fitur ini digunakan untuk memantau dan memeriksa query Prometheus yang dijalankan terhadap data dari Hasura. Berikut penjelasan dari elemen-elemen yang ada di layar tersebut:

##### 1. Total Queries
Bagian ini menampilkan jumlah total query yang dijalankan, yaitu dua query dengan total waktu eksekusi 84 ms.

##### 2. Query Inspector Tabs
Ada beberapa tab seperti:
- **Data:** Menampilkan hasil data dari query yang dijalankan.
- **Stats:** Menunjukkan statistik terkait query.
- **JSON:** Menampilkan representasi JSON dari query yang dijalankan.
- **Query:** Menampilkan sintaks query Prometheus yang digunakan.

##### 3. Query Inspector
Menunjukkan detail dari query yang dijalankan pada panel:
- **Expr:** Ini adalah ekspresi PromQL (Prometheus Query Language) yang digunakan:
    ```promql
    SUM(increase(hasura_graphql_requests_total{operation_type="query", instance=~"10\.100\.13\.24:8889", job=~"hasuraferdy"}[1800s]))
    ```
    Ini berarti query tersebut menghitung total permintaan (request) GraphQL yang berhasil (`response_status="success"`) selama periode waktu tertentu (dalam detik `[1800s]`) untuk instance `10.100.13.24:8889` pada job `hasuraferdy`.

- **Step:** Menunjukkan interval langkah waktu untuk query, yaitu 5 detik.

##### 4. Request Details
Bagian ini menunjukkan detail dari request yang dikirimkan ke API, termasuk:
- **URL:** `/api/ds/query`
- **Method:** `POST`
- **Data:**
    - `queries` (Array): Berisi dua query yang dikirimkan.
    - `range`: Menunjukkan rentang waktu yang digunakan dalam query.
    - `from` dan `to`: Menunjukkan timestamp waktu mulai dan berakhir dari query yang dijalankan.

##### 5. Response Details
Menampilkan objek response yang diterima dari server, yang biasanya berisi data hasil query atau pesan error jika ada kesalahan.

##### Penggunaan Fitur ini
Fitur ini biasanya digunakan untuk:
- Menganalisis performa query.
- Mengecek hasil query dalam format JSON untuk memudahkan debugging.
- Memeriksa permintaan (request) yang dikirimkan ke server backend (Prometheus) untuk memvalidasi konfigurasi query.
- Melihat statistik jumlah query yang dijalankan dan hasil yang diterima dalam berbagai format (Data, JSON).

Secara keseluruhan, *Query Inspector* di Grafana ini sangat membantu untuk memeriksa dan mengoptimalkan query yang digunakan pada sistem monitoring.

#### 3. Panel JSON:
**Pengertian**: Menampilkan konfigurasi JSON dari panel tersebut.  
**Fungsi**: Berguna untuk melihat dan mengedit konfigurasi panel secara manual, seperti pengaturan visualisasi, query, dan data lainnya. JSON ini bisa disalin dan digunakan untuk menduplikasi panel di dashboard lain. 

![image](https://github.com/user-attachments/assets/d1f741bc-5713-4f27-82df-7a602c5179c5)


Gambar di atas menunjukkan tampilan *JSON Inspector* pada Grafana. Fitur ini digunakan untuk melihat representasi JSON dari konfigurasi panel yang ada di Grafana. Berikut adalah penjelasan dari elemen-elemen yang terlihat:

##### 1. Panel JSON Viewer
Di bagian ini, pengguna dapat melihat dan memodifikasi representasi JSON dari panel Grafana yang sedang aktif. JSON ini mencakup semua pengaturan dan konfigurasi panel, seperti sumber data, gaya tampilan, format angka, dan banyak lagi.

##### 2. JSON Structure Details
Pada JSON yang ditampilkan, beberapa informasi penting dapat diperhatikan:

- **`datasource`**:
    ```json
    {
        "type": "prometheus",
        "uid": "bacf5660-d711-4cea-9045-7349e2548deb"
    }
    ```
    Bagian ini menunjukkan bahwa *datasource* yang digunakan adalah Prometheus. `uid` adalah identifikasi unik dari *datasource* tersebut.

- **`description`**:
    ```json
    "description": "Number of GraphQL Query requests"
    ```
    Deskripsi ini memberikan informasi bahwa panel ini dirancang untuk menampilkan jumlah permintaan (request) GraphQL yang dilakukan. Deskripsi ini biasanya digunakan untuk memberikan konteks atau informasi tambahan terkait dengan panel tersebut.

- **`fieldConfig`**:
    Bagian ini berisi konfigurasi untuk tampilan dan pemformatan field yang ada di panel. Beberapa sub-elemen penting:
    - **`thresholds`**:
        Mengatur batas nilai yang digunakan untuk menentukan warna atau indikasi tertentu pada nilai data.
        ```json
        "mode": "absolute",
        "steps": [
            {
                "color": "green",
                "value": null
            }
        ]
        ```
        Mode ini menggunakan `absolute` dan menetapkan warna hijau untuk nilai data. Nilai `null` menunjukkan bahwa semua data dianggap valid tanpa ada batasan (threshold) tertentu yang diterapkan.

    - **`color`**:
        ```json
        {
            "mode": "thresholds"
        }
        ```
        Menentukan mode pewarnaan pada panel sesuai dengan `thresholds` yang telah ditentukan sebelumnya.

    - **`decimals`** dan **`unit`**:
        - `decimals`: Mengatur jumlah desimal yang akan ditampilkan.
        - `unit`: Menentukan satuan dari data yang ditampilkan, dalam kasus ini `none` menunjukkan bahwa tidak ada satuan khusus.

- **`overrides`**:
    Menampilkan pengaturan khusus atau pengecualian untuk field-field tertentu. Bagian ini kosong (`[]`) yang berarti tidak ada konfigurasi pengecualian yang diterapkan.

##### 3. Editing Options
- **Select Source**: Pengguna dapat memilih sumber panel JSON lain jika ada lebih dari satu panel yang sedang dibuka atau diedit.
- **Apply Button**: Tombol ini digunakan untuk menerapkan perubahan yang dilakukan pada JSON tersebut ke panel Grafana yang sedang aktif.

##### Penggunaan Fitur ini:
- Memungkinkan pengguna untuk melihat dan memodifikasi detail konfigurasi panel secara manual.
- Berguna untuk melakukan pengaturan lanjutan atau debugging jika ada kesalahan pada tampilan atau data yang tidak sesuai.
- Memudahkan dalam menyalin pengaturan panel dari satu dashboard ke dashboard lain atau untuk melakukan impor/ekspor konfigurasi panel.

Fitur ini biasanya digunakan oleh pengguna yang sudah familiar dengan struktur JSON dan memerlukan kontrol yang lebih presisi terhadap pengaturan panel di Grafana.

#### 4. Stats:
Fitur Stats ini membantu pengguna untuk mengevaluasi performa query yang dijalankan pada panel, termasuk waktu yang dibutuhkan dan volume data yang ditarik. Ini dapat digunakan untuk mengoptimalkan query jika performa atau waktu load dianggap terlalu lama.

![image](https://github.com/user-attachments/assets/d958a010-a76f-4c05-8294-c0dae3364f89)


Fitur yang ditampilkan pada gambar ini adalah bagian dari Inspect Panel di Grafana, lebih tepatnya di tab Stats. Fitur ini menyediakan ringkasan statistik dari query yang dijalankan pada panel.

##### 1. Total request time:
**Fungsi**: Menunjukkan waktu total yang dibutuhkan untuk mengeksekusi semua query yang terkait dengan panel ini.  
**Penggunaan**: Berguna untuk memahami seberapa cepat panel dapat menarik data. Dalam contoh, query membutuhkan 68 ms.

##### 2. Data processing time:
**Fungsi**: Waktu yang diperlukan untuk memproses data setelah data diterima. Ini mencakup waktu untuk memformat atau mengolah data agar sesuai dengan kebutuhan panel.  
**Penggunaan**: Menganalisis efisiensi pemrosesan data, di sini membutuhkan sekitar 0.100 ms.

##### 3. Number of queries:
**Fungsi**: Menampilkan jumlah query yang dijalankan untuk menghasilkan data yang ditampilkan di panel.  
**Penggunaan**: Berguna untuk melihat kompleksitas panel. Dalam contoh, ada 2 query yang dijalankan.

##### 4. Total number rows:
**Fungsi**: Menunjukkan jumlah total baris data yang dihasilkan oleh query.  
**Penggunaan**: Informasi ini berguna untuk memahami volume data yang ditampilkan. Di contoh ini, ada 361 baris data.
  
Dengan menu Inspect, Anda dapat memahami lebih dalam tentang bagaimana panel bekerja, data apa yang ditarik, dan bagaimana panel dikonfigurasikan, yang sangat membantu dalam troubleshooting atau optimasi.

### 6. More:
**Fungsi**: Biasanya berisi fitur tambahan seperti menggandakan panel, menyalin pengaturan, atau mengkloning panel.

![Screenshot (277)](https://github.com/user-attachments/assets/ca396ea9-65c0-4dce-b8bd-cf23d24aafe5)

Menampilkan opsi tambahan yang berkaitan dengan panel.
  
Menu More... di dalam panel Grafana memberikan beberapa opsi tambahan untuk mengelola panel tersebut. Berikut penjelasan dari tiap fitur di dalamnya:
  
#### 1. Duplicate:
**Pengertian**: Membuat salinan panel yang sama di dashboard saat ini.  
**Fungsi**: Berguna jika Anda ingin membuat panel yang identik dan memodifikasinya tanpa mengubah panel asli. Misalnya, untuk melihat data yang sama tetapi dengan visualisasi yang berbeda.

#### 2. Copy:
**Pengertian**: Menyalin konfigurasi panel.  
**Fungsi**: Mengambil seluruh pengaturan dari panel ini dan menyimpannya ke clipboard Anda, sehingga bisa dipaste ke dashboard lain atau digunakan untuk memodifikasi panel di tempat lain.

#### 3. Create library panel:
**Pengertian**: Menjadikan panel ini sebagai panel pustaka (library panel).  
**Fungsi**: Anda dapat membuat panel ini tersedia secara umum di berbagai dashboard lain. Jika Anda mengubah panel pustaka, semua tempat yang menggunakan panel ini juga akan diperbarui.

#### 4. Get help:
**Pengertian**: Menyediakan opsi untuk bantuan lebih lanjut terkait panel.  
**Fungsi**: Opsi ini membantu pengguna untuk mendapatkan dokumentasi atau panduan terkait penggunaan atau konfigurasi panel di Grafana.
  
Fitur More... ini sangat berguna untuk manajemen panel lanjutan dan penghematan waktu ketika ingin menggunakan kembali atau menduplikasi panel dengan cepat.

### 7. Remove:
**Pengertian**: Menghapus panel dari dashboard.  
**Fungsi**: Opsi ini akan menghapus panel dari dashboard saat ini. Shortcut p r bisa digunakan untuk menghapus panel dengan cepat.
Dropdown ini dirancang agar pengguna dapat dengan mudah mengelola, mengedit, dan menganalisis data pada panel mereka di dashboard Grafana.
