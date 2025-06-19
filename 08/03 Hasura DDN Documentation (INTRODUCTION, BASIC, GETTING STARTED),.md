# INTRODUCTION  
Hasura DDN (*Data Delivery Network*) adalah platform yang mempermudah pengembangan dan pengelolaan API data modern, terutama yang bersifat terfederasi (menggabungkan data dari berbagai sumber). Dengan alat yang kuat dan alur kerja berbasis kode, Hasura DDN menyederhanakan akses dan integrasi data, menjadikan pengelolaan API lebih efisien dan mudah. Fitur utamanya termasuk kemudahan dalam menghubungkan berbagai sumber data, otomatisasi, dan peningkatan performa API, sehingga mempercepat siklus pengembangan aplikasi tanpa memerlukan banyak konfigurasi manual.

# BASICS  
Hasura DDN memudahkan penghubungan sumber data ke Hasura Engine menggunakan standar metadata fleksibel dan konektor data native. Arsitektur ini, disebut *data supergraph*, memungkinkan pendekatan berbasis kode penuh dengan API yang langsung tersedia.
  
Dengan Hasura DDN, kita bisa dengan mudah menggabungkan data dari berbagai sumber, mengatasi silo data. DDN memberikan visibilitas jelas terhadap kueri, kontrol keamanan yang kuat, dan sistem yang andal. Alur kerja pengembangan jadi lebih efisien dengan alat CI/CD, performa optimal lewat kueri native, sambil memastikan keamanan data dan logika bisnis tetap terpusat.

## Background  
Hasura DDN dibangun untuk mengatasi hambatan umum dalam pengembangan aplikasi modern, terutama dalam menyediakan akses data yang aman dan efisien. Dengan arsitektur *data supergraph*, Hasura DDN meningkatkan produktivitas tim dan developer dengan menyederhanakan pengelolaan API dan federasi data.

Beberapa tantangan yang diatasi Hasura DDN adalah:
- Pengembangan produk yang lambat.
- Kesulitan dalam menggabungkan sumber data dan komposisi API.
- Kompleksitas dan penyebaran layanan mikro yang berlebihan.
- Tata kelola yang tidak terstruktur dan kolaborasi yang sulit.
- Keamanan yang rumit.
- Kinerja yang terganggu dan sulit ditingkatkan.
- Kurangnya visibilitas kueri.
- Beragam kebutuhan konsumen API.
  
**Untuk Pengembang API**, Hasura DDN menawarkan:
- Pemodelan domain kaya fitur untuk data kompleks.
- Komposisi data yang mudah dari berbagai sumber.
- Pengaturan keamanan yang mudah dipahami.
- Pengalaman developer yang dioptimalkan.
- Fleksibilitas dalam pemilihan bahasa pemrograman.
  
**Untuk Tim Pemelihara API**, Hasura DDN memprioritaskan:
- Efisiensi kinerja dan operasional.
- Peluncuran tanpa gangguan.
- CI/CD federasi untuk kemandirian tim.
- Topologi terdistribusi untuk skalabilitas global.
- Onboarding cepat dan penemuan domain data.
  
**Untuk Konsumen API**, Hasura DDN memberikan:
- API yang andal dan terdokumentasi.
- Konsistensi pengambilan data dari berbagai sumber.
- Kemudahan dalam menemukan dan mencoba API.
  
Hasura DDN mempercepat pengembangan API modern yang lebih cepat, kuat, fleksibel, dan mudah diskalakan.

## Core Concepts  
Perkembangan microservices dan API yang dibutuhkan oleh sebuah produk menyebabkan tantangan baru bagi pengembang dan arsitek. Di era modern ini, lapisan data semakin kompleks, bukan hanya satu database, tetapi koleksi dari berbagai database, layanan, dan API.
  
Hasura DDN memperkenalkan konsep supergraph data untuk membantu mengelola kompleksitas ini.
  
### Kondisi Lapisan Data  
Bayangkan beberapa tim dalam sebuah perusahaan bekerja dengan sistem yang berbeda (basis data, API, dll.). Misalnya:  
- **Product Management Team** mengelola basis data relasional untuk data produk.
- **User Experience Team** menggunakan basis data MongoDB untuk informasi pengguna.
- **Search Optimization Team** bekerja dengan layanan pencarian seperti Algolia.
- **Finance and Transactions Team** menangani pembayaran melalui Stripe.
- **Logistics and Shipping Team** mengelola pengiriman melalui layanan seperti ShipStation.
- **Data Science and Recommendations Team** menggunakan basis data vektor (seperti Weaviate) untuk saran yang dipersonalisasi.
  
Setiap tim ini menggunakan API, skema, dan metode yang berbeda. Fragmentasi ini menyulitkan pengembangan aplikasi dan membutuhkan keterlibatan arsitek data yang ahli untuk mengintegrasikan dan mengelola sumber data yang beragam tersebut.
  
### Subgraph  
Setiap layanan yang digunakan dalam contoh aplikasi e-commerce ini disebut **subgraph**. Subgraph adalah lebih dari sekadar sumber data, ini mencakup semua metadata yang dibutuhkan untuk beroperasi secara mandiri dan kemudian terhubung sebagai bagian dari API terpadu.

Untuk pengembang API, ini berarti mereka bisa membangun API dari berbagai sumber secara sederhana dan deklaratif. Setiap subgraph juga dapat menggunakan aturan akses, mekanisme autentikasi, dan logika bisnis khusus untuk menciptakan API yang aman dan tangguh.

### Supergraph  
**Supergraph** adalah kerangka kerja yang menghubungkan semua subgraph menjadi satu API yang aman. Ini memungkinkan aplikasi untuk menggunakan berbagai sumber data, layanan, API pihak ketiga, dan logika bisnis dalam satu lapisan yang terpadu.

Manfaatnya bagi tim pengelola data adalah mereka hanya perlu mengelola satu API terpadu daripada banyak sumber data yang berbeda. Bagi konsumen API, mereka hanya perlu mengakses satu endpoint untuk semua data yang dibutuhkan, sehingga aplikasi mereka menjadi lebih sederhana.

### Cara Membangun Subgraph dan Supergraph  
Untuk membangun subgraph, pengembang cukup menghubungkan sumber data mereka menggunakan konektor data. Hasura DDN menyediakan konektor data untuk berbagai layanan populer, atau kita dapat membuat konektor sendiri. Ketika subgraph sudah terhubung, Hasura secara otomatis membangun supergraph dari semua sumber data yang ada, memungkinkan hubungan antar sumber data serta kontrol akses yang fleksibel.

Dengan menggunakan supergraph, organisasi bisa mengurangi kerumitan infrastruktur data dan meningkatkan kolaborasi antar tim.

## Sebelum Hasura DDN
Saat bisnis berusaha memodernisasi diri, mereka menghadapi tantangan dalam menyampaikan pengalaman digital secara cepat dan efisien. Tim pengembangan produk sering kali terhambat oleh tim backend yang harus menyediakan akses data yang aman dan efisien. Hal ini membuat mereka terpaksa membuat API kustom secara manual, memicu pertumbuhan berlebih pada microservice, mengatur kebijakan keamanan yang rumit, sehingga banyak waktu terbuang dan inovasi menjadi terhambat.

![image](https://github.com/user-attachments/assets/be5e1a92-7aa6-46cc-bbf7-0cc579ad350b)


## Dengan Hasura DDN  
Dengan beralih ke arsitektur supergraph menggunakan Hasura, organisasi dapat mengurangi kerumitan infrastruktur data secara signifikan. Hasura DDN memungkinkan koneksi langsung ke berbagai sumber data menggunakan native data connectors (NDCs), sehingga kebutuhan untuk membuat API dan microservice secara manual menjadi sangat berkurang. Selain itu, kolaborasi antara tim backend dapat ditingkatkan karena mereka dapat bekerja secara independen.

Data dapat diakses dengan aman dan saling terhubung, memungkinkan pembuatan data graph terpadu yang dapat digunakan untuk berbagai kebutuhan, mulai dari aplikasi klien hingga API publik. Semua ini dapat diterapkan di infrastruktur global yang mendukung latensi minimal, skalabilitas tinggi, dan keandalan yang tidak tergoyahkan.

![image](https://github.com/user-attachments/assets/a9ba5b6d-70ef-4b94-89a7-a90ad8d1352d)


# GETTING STARTED    
Dalam beberapa langkah, kita bisa menghubungkan sumber data, mendefinisikan API, dan mulai membangun data supergraph. Alat Hasura yang kuat, seperti CLI dan konsol, memudahkan pembuatan dan pengelolaan supergraph sambil memastikan integrasi data yang mulus.

Baik menggunakan proyek sampel atau data kita sendiri, kita bisa langsung menjalankannya. Proses setup Hasura yang intuitif memungkinkan kita fokus pada pengembangan, bukan konfigurasi, sehingga kita dapat dengan cepat merilis pembaruan produk.

## Quickstart Supergraph API - Hasura DDN

### Prasyarat  

1. **Install DDN CLI**  
   Unduh CLI terbaru untuk Windows:  
   ```bash
   curl.exe -L https://graphql-engine-cdn.hasura.io/ddn/cli/v4/latest/cli-ddn-windows-amd64.exe -o ddn.exe
   ```
     
   Tambahkan executable ke PATH sistem agar bisa dijalankan dari terminal.

2. **Validasi instalasi**  
   Kita dapat memverifikasi bahwa DDN CLI diinstal dengan benar dengan menjalankan:
     
   ```bash
   ddn doctor
   ```

3. **Install Docker**  
   Pastikan Docker Compose versi 2.27.1 atau lebih baru sudah terpasang.
     
### Langkah-Langkah  

1. **Login ke CLI**
   Jalankan perintah berikut untuk autentikasi:

   ```bash
   ddn auth login
   ```
  
2. **Inisialisasi Supergraph**
   Buat direktori proyek dan inisialisasi supergraph:

   ```bash
   mkdir hasura_project && cd hasura_project
   ddn supergraph init .
   ```
  
3. **Hubungkan ke Sumber Data**
   Inisialisasi konektor data dengan memilih jenis konektor dan memasukkan variabel yang dibutuhkan:

   ```bash
   ddn connector init my_connector -i
   ```

4. **Deploy Postgres Local**
   Inisialisasi postgres secara local menggunakan docker compose:
          
   ```bash
   docker compose -f app\connector\mypostgres\compose.postgres-adminer.yaml up -d
   ```
     
5. **Introspeksi Sumber Data**
   Jalankan introspeksi sumber data:
   
   ```bash
   ddn connector introspect my_connector
   ```
   
6. **Tambahkan Sumber Daya**
   Buat metadata untuk model, command, dan relasi:
   
   ```bash
   ddn model add my_connector '*'
   ddn command add my_connector '*'
   ddn relationship add my_connector '*'
   ```
   
7. **Build Supergraph untuk Pengembangan Lokal**
   Buat build lokal supergraph:
   
   ```bash
   ddn supergraph build local
   ```
   
8. **Jalankan Supergraph**
   Mulai supergraph menggunakan Docker:
   ```bash
   ddn run docker-start
   ```
   
9. **Deploy ke Hasura DDN**
   Inisialisasi proyek di Hasura DDN:
   
   ```bash
   ddn project init
   ```
   
10. **Build dan Deploy Supergraph**
   Bangun dan deploy supergraph ke Hasura DDN:
   
   ```bash
   ddn supergraph build create
   ```

### Langkah Selanjutnya  
- **Integrasi Logika Bisnis**: Tambahkan logika khusus ke API menggunakan lambda connectors (TypeScript/Python).
- **Tambah Otorisasi**: Buat aturan akses untuk entitas di supergraph.
- **Iterasi API**: Lakukan pembaruan pada metadata Hasura saat ada perubahan pada schema sumber data.
- **Hubungkan Sumber Data**: Buat relasi antar sumber data untuk query efisien.
- **Tambah Subgraph Baru**: Mudah mengelola subgraph oleh tim berbeda dengan metadata terpisah.
  
## Eksplorasi Supergraph yang Sudah Ada - Hasura DDN  

Navigasi API sering kali terasa rumit, dari pengaturan lingkungan, visualisasi struktur API, hingga pemantauan performa. Tantangan yang dihadapi pengembang termasuk:

- **Pengaturan Manual**: Konfigurasi lingkungan API secara manual yang rentan terhadap kesalahan.
- **Kurangnya Visualisasi**: Sulit memahami struktur API tanpa alat visualisasi yang baik.
- **Keterbatasan Insight**: Pemantauan performa API memerlukan beberapa alat yang terpisah, yang bisa membuat proses terputus-putus.

Hasura menawarkan solusi dengan GUI yang mudah digunakan, disebut *console*.

### Apa itu console?
Console adalah GUI berbasis web yang memungkinkan tim untuk berinteraksi, memvisualisasikan, mengeksplorasi, dan memonitor API. Bisa digunakan baik pada instance lokal maupun proyek yang di-host.

![image](https://github.com/user-attachments/assets/5a2f74b0-d109-4ee1-b633-17a14c4a8677)


Console memudahkan pengujian query, memberikan feedback, mengeksplorasi API, serta mendiagnosa *error* atau *bottleneck*.

### Preparation Eksplorasi Supergraph

1. **Buat Akun atau Login**  
   Buat akun atau masuk ke akunmu di https://console.hasura.io.

2. **Buat Proyek atau Eksplorasi Proyek Contoh**  
   Kita bisa mengikuti quickstart untuk membuat proyekmu sendiri, atau eksplorasi proyek contoh eCommerce Supergraph API yang dibangun di Hasura DDN. Cari proyek contoh "eCommerce App" di bagian *Sample Projects*.

Mulai gunakan *explorer* untuk mengeksplorasi lebih lanjut API melalui console.  
  
### Eksplorasi Supergraph - Hasura DDN  
Menjelajahi struktur dan hubungan API seringkali menjadi tantangan bagi pengembang. Mengandalkan alat eksternal terbatas dapat menghasilkan representasi API yang tidak lengkap atau tidak akurat. Dokumentasi manual memperumit proses, rentan terhadap kesalahan, dan memperlambat pengembangan.

Hasura menyediakan *console* — GUI berbasis web yang memiliki halaman *explorer* yang menawarkan:

- **Visualisasi Komprehensif**: Representasi real-time API, termasuk hubungan dan izin.
- **Dokumentasi Otomatis**: Dokumentasi API yang selalu akurat dan diperbarui otomatis.
- **Kolaborasi yang Ditingkatkan**: Alat visual untuk meningkatkan komunikasi tim.

Console juga memungkinkan interaksi dan pemantauan API secara langsung.

#### Langkah 1. Eksplorasi Supergraph
Klik tombol *Explorer* di menu samping untuk melihat visualisasi keseluruhan supergraph. 

![image](https://github.com/user-attachments/assets/1569893a-6dc1-4386-b8b7-bda9ae24bdf9)


Kita bisa melihat subgraph, sumber data, dan hubungan di antara mereka. Ada opsi untuk mengubah tampilan sesuai kebutuhan.

#### Langkah 2. Eksplorasi Subgraph
Klik salah satu subgraph untuk melihat rinciannya, seperti konektor, model, dan perintah yang ada di dalamnya.

![image](https://github.com/user-attachments/assets/9313bfae-50c5-496f-8073-5e2026099603)


#### Langkah 3. Eksplorasi Sumber Data
Jika terdapat banyak konektor di dalam subgraph, kita bisa fokus pada satu konektor dan melihat model serta perintah terkaitnya.

![image](https://github.com/user-attachments/assets/0ff7f346-062a-42ca-b71b-710ee7c537aa)


#### Langkah 4. Eksplorasi Model atau Perintah
Console memungkinkan pencarian entitas berdasarkan nama. Misalnya, kita bisa mengetik "Carts" dan melihat detail model yang terkait. 

![image](https://github.com/user-attachments/assets/7fae9899-2888-412f-8055-66b514eb09aa)


Dari sini, kita dapat melihat visualisasi entitas, hubungan, izin, dan bahkan menjalankan contoh query.

#### Signature
Setiap model atau perintah memiliki *signature* yang menunjukkan input dan outputnya.

- **Model**: Biasanya tidak memiliki input dan mengembalikan tipe data model.
  - Contoh: `Carts() => Carts`
 
    ![image](https://github.com/user-attachments/assets/2403ec5d-9ddb-43d6-aeba-8aed6634027c)

  
- **Perintah**: Memiliki argumen input dan output berupa tipe data yang dikembalikan.
  - Contoh: `GetFruits() => [Fruit!]!`

    ![image](https://github.com/user-attachments/assets/2311f2d8-3a30-476f-8941-27acdde814d7)



#### Root Fields di GraphQL
Root fields menentukan bagaimana data dapat diakses melalui query GraphQL. Contoh untuk model Cart:

- **Select Many**: `app_carts() => Carts` - Untuk query beberapa entri cart.
- **Select Unique**: `app_cartsById(id: String!) => Carts` - Untuk query cart spesifik berdasarkan ID.

![image](https://github.com/user-attachments/assets/14380a42-2f96-4dc1-b983-f01f996f1120)


Contoh query:
```graphql
query {
  app_carts {
    id
    isComplete
  }
}

query {
  app_cartsById(id: "123") {
    id
    isComplete
  }
}
```  
  
Query pertama menggunakan kolom "Select Many" untuk mengambil beberapa keranjang, sedangkan kueri kedua menggunakan kolom "Select Unique" untuk mengambil keranjang tertentu berdasarkan ID.
  
#### Output Fields
Output fields menunjukkan struktur dan tipe data yang bisa diambil dari model atau perintah.

![image](https://github.com/user-attachments/assets/ae697c1f-333e-4565-9ce5-e10bed3fdd52)


#### Arguments
Argumen memungkinkan penyaringan, pengurutan, atau modifikasi data dalam query atau mutasi. Di Hasura, argumen membuat API lebih fleksibel.

![image](https://github.com/user-attachments/assets/9e4ebde9-c607-4eff-9933-59c6b58dafbe)


#### Relationships
Hubungan menunjukkan bagaimana model terhubung dengan model lainnya. Misalnya, model Cart memiliki hubungan dengan pengguna melalui field userId yang terhubung dengan id di tabel Users.

![image](https://github.com/user-attachments/assets/13daa706-3c15-4fbc-b8d7-f3ec5870bc25)


#### Permissions
Izin mendefinisikan kontrol akses untuk model atau perintah. Misalnya, pengguna dengan peran admin dapat melihat informasi cart tetapi tidak bisa membuat, mengubah, atau menghapus cart.

![image](https://github.com/user-attachments/assets/ed5e16cd-51ee-4f5d-85dc-3458c95f8f6c)


### Berinteraksi dengan Supergraph Kita  
Sekarang, kita bisa mulai berinteraksi dan menguji API menggunakan GraphiQL explorer.Berinteraksi dengan API seharusnya menjadi tugas yang sederhana, namun sering kali menjadi rumit dan membingungkan. Developer sering harus berpindah antar berbagai alat untuk menguji query, menambahkan header, dan melacak permintaan. Pendekatan ini tidak hanya memperlambat alur kerja, tetapi juga tidak efisien dalam proses debugging. Tanpa tracing terintegrasi, mengidentifikasi dan menyelesaikan masalah menjadi sangat memakan waktu.

Kurangnya umpan balik secara langsung juga berarti bahwa setiap perubahan pada API atau query memerlukan verifikasi manual, yang semakin memperlambat pengembangan. Menyadari masalah ini, Hasura DDN menyediakan solusi komprehensif melalui GraphiQL explorer, dirancang untuk menyederhanakan dan mempercepat proses interaksi API.

Dengan Hasura DDN, kita dapat menggunakan GraphiQL explorer untuk membuat dan menguji query langsung dalam console, dengan fitur berikut:

- **Definisi Skema**: Melihat seluruh skema GraphQL dan dokumentasi untuk semua query dan mutasi yang tersedia.
- **Pengujian Query Terintegrasi**: Menguji query dengan header dan melihat hasil secara real-time, semuanya dalam satu tempat.
- **Tracing Terbina**: Melacak query dengan cepat untuk mengidentifikasi hambatan kinerja dan mengoptimalkan secara efisien.
- **Alur Kerja Terpadu**: Menghemat waktu dan mengurangi perpindahan antar alat dengan semua yang kita butuhkan dalam satu antarmuka.


#### Langkah 1: Uji API Supergraph Kita
Mulailah dengan membuat query untuk menguji API supergraph kita. Berikut contoh query yang bisa kita gunakan dengan aplikasi e-commerce sampel:

```graphql
query UsersAndOrders {
  users_users {
    id
    name
    orders {
      id
      createdAt
      status
    }
  }
}
```

![image](https://github.com/user-attachments/assets/9baa0f45-3ec1-4d85-90b8-da4d8ca6337c)


Gunakan bagian request di sisi kiri komponen GraphiQL untuk menulis query kita. Jika kita tidak yakin apa yang harus diketik, tekan `CTRL + SPACE` untuk melihat opsi yang tersedia. Tambahkan variabel untuk query dinamis menggunakan bagian Variabel di bawah query. Respon akan muncul di sisi kanan.

#### Langkah 2: Lihat Tracing Query
Setelah menguji query Kita, klik tombol View Trace di pojok kanan bawah GraphiQL explorer untuk melihat tracing. Kita akan melihat detail seperti langkah-langkah eksekusi query, topologi, dan rencana eksekusi.

![image](https://github.com/user-attachments/assets/6d7a9bf5-01e3-467a-9855-5fa6f00ea38e)


#### Langkah 3: Jelajahi Skema API dan Dokumentasi
Navigasikan ke tab Schema untuk melihat definisi skema API kita. Setiap query diawali dengan nama subgraph dan bisa diklik untuk menghasilkan query uji. Klik ikon buku untuk menjelajahi dokumentasi API secara rinci.

#### Fitur Lanjutan
- **Switch Builds**: Di bagian atas halaman GraphiQL, kita dapat beralih antara build API yang berbeda menggunakan build manager. Sistem build Hasura DDN yang immutable memungkinkan kita membuat dan menguji API secara iteratif.
  ![image](https://github.com/user-attachments/assets/f563fb56-5bbb-4052-86ab-19b84cc85992)

- **Tambah Header**: Gunakan tabel header di bawah build manager untuk menambahkan header kustom. Secara default, `content-type` dan `hasura_cloud_pat` sudah ada. Tambahkan variabel sesi (misalnya, `x-hasura-role`) atau token otentikasi (misalnya, `Bearer JWT`) untuk menguji izin dan hubungan.
  ![image](https://github.com/user-attachments/assets/47f58725-b705-40bb-bc7c-e635000a30c7)

  
### Monitor Supergraph Kita
Setelah kita menguji API, melihat tracing, dan menjelajahi skema, pelajari cara memantau kinerja API kita.  
  
Memantau kinerja API dan mendapatkan wawasan yang dapat ditindaklanjuti sangat penting untuk menjaga sistem yang efisien dan responsif. Namun, proses ini seringkali terhambat oleh beberapa tantangan utama, seperti penggunaan alat yang terpisah sehingga sulit mengumpulkan dan menganalisis metrik secara terpadu. Keterlambatan dalam mendapatkan wawasan akibat agregasi data dan analisis dapat memperlambat pengambilan keputusan dan berdampak pada kinerja API.

Selain itu, pengaturan dan pemeliharaan alat pemantauan memerlukan upaya dan keahlian yang cukup besar, menambah kompleksitas dan beban sumber daya.

Hasura DDN menyederhanakan pemantauan kinerja dengan menyediakan:

- **Wawasan Instan**: Akses metrik dan jejak utama secara real-time di halaman *insights*.
- **Pemantauan Terintegrasi**: Pantau kinerja langsung dari *console* tanpa memerlukan alat tambahan.
- **Optimasi Proaktif**: Identifikasi dan atasi masalah kinerja dengan cepat, memastikan API *supergraph* berjalan lancar dan efisien.

#### Langkah 1. Lihat metrik kinerja
Kita dapat mengakses metrik kinerja utama untuk proyek dan setiap build:

![image](https://github.com/user-attachments/assets/4b6f74d3-5458-44e6-95d8-f83752f60b4c)


#### Langkah 2. Lihat jejak (traces)
Klik tab *Traces* di bagian atas halaman untuk melihat data jejak per permintaan, dengan rincian yang sama seperti menggunakan tombol *View Trace* di *GraphiQL explorer*.

![image](https://github.com/user-attachments/assets/54dc5c3a-253b-4f25-85b4-4ca78e889b41)

## Build a Supergraph  
Dengan gambaran lengkap tentang semua yang ditawarkan oleh *console*, saatnya untuk *Build an API*
  
### Prasyarat
Pastikan kita telah menginstal perangkat lunak berikut dengan versi minimum yang sesuai di mesin kita:

#### Langkah 1. Instal dan Otorisasi Hasura CLI
Unduh CLI sesuai dengan sistem operasi kita:

- **macOS dan Linux**: Ikuti panduan instalasi untuk sistem kita.
- **Windows**: Unduh binary `cli-ddn-windows-amd64.exe` dan jalankan.

```bash
curl.exe -L https://graphql-engine-cdn.hasura.io/ddn/cli/v4/latest/cli-ddn-windows-amd64.exe -o ddn.exe
```
Jika muncul peringatan "Unrecognized application" di Windows, klik "Run anyway".

Tambahkan executable ini ke PATH sistem kita:

1. Cari path penuh ke executable, contohnya: `C:\Program Files\cli-ddn-windows-amd64.exe`.

2. Ubah nama file ke ddn agar lebih mudah digunakan.

3. Buka System Properties:

- Tekan `Win + X`, pilih System.
- Klik `Advanced system settings` di sisi kiri.

4. Edit variabel PATH:

- Di jendela `Environment Variables`, temukan bagian `System variables` dan pilih `Path`, lalu klik `Edit`.
- Tambahkan path ke executable yang sudah diubah tadi.
  
5. Verifikasi dengan membuka Command Prompt atau PowerShell, lalu jalankan:

```bash
ddn
```

##### Login dengan CLI  
Setelah instalasi, jalankan perintah berikut untuk mengautentikasi sesi CLI dengan Hasura DDN:

```bash
ddn auth login
```
  
#### Langkah 2. Instal Ekstensi Hasura di VS Code  
Jika belum memiliki Visual Studio Code, unduh dan instal. Kemudian instal ekstensi Hasura untuk mendapatkan fitur seperti autocomplete, saran kontekstual, dan validasi langsung.

#### Langkah 3. Instal Docker Compose v2.27.1 atau lebih baru
Instal Docker untuk pengembangan lokal. Versi Docker Compose yang dibutuhkan adalah v2.27.1 atau lebih tinggi. Untuk memeriksa versi:

```bash
docker compose version
```
Unduh versi terbaru Docker [di sini](https://docs.docker.com/engine/install/).

##### Browser yang Kompatibel dengan Konsol
Beberapa pengaturan atau ekstensi browser dapat menghalangi akses ke instance lokal Hasura. Jika menemui masalah, disarankan untuk menonaktifkan pengaturan tersebut di domain `console.hasura.io`. **Chrome** dan **Firefox** direkomendasikan untuk pengalaman terbaik.

### Create a Supergraph  
Kita akan membuat sebuah supergraph.

Supergraph kita adalah gabungan dari semua subgraph, berbagai sumber data, dan logika bisnisnya. Saat kita "membangun" supergraph, kita membuat API GraphQL.

![image](https://github.com/user-attachments/assets/29cab821-32a4-437b-9f8b-9c73a58312fb)


#### Langkah-langkah
**Diperlukan:**
- DDN CLI, ekstensi VS Code, dan Docker terinstal

##### Langkah 1. Inisialisasi supergraph
Untuk menginisialisasi supergraph, dari direktori kosong, jalankan perintah berikut:
```
ddn supergraph init .
```

**Menentukan direktori**  
Alih-alih menggunakan . (penanda direktori "ini"), kita dapat menggunakan flag `--dir` dan nama direktori pada perintah `supergraph init` untuk membuat supergraph di direktori tertentu. Jika direktori tersebut tidak ada, maka akan dibuat.

##### Langkah 2. Jalankan supergraph
Dengan Docker daemon berjalan di mesin kita, kita bisa segera memulai Hasura Engine, supergraph, dan layanan pendukungnya.

Jalankan:
```
ddn run docker-start
```

**CLI dan direktori build**  

CLI secara otomatis membuat direktori build saat menjalankan perintah `ddn supergraph init`. Direktori ini berisi semua file JSON yang diperlukan untuk mengonfigurasi dan menjalankan supergraph kita. File JSON di dalamnya sudah direferensikan dalam file `docker compose.yaml`, memungkinkan kita untuk langsung memulai layanan.

Perintah `docker-start` didefinisikan di `hasura/context.yaml`. Perintah ini menetapkan variabel lingkungan `HASURA_DDN_PAT` menggunakan nilai yang diambil dari perintah `ddn auth print-pat`, kemudian menggunakan Docker Compose untuk memulai layanan Hasura yang didefinisikan di file `compose.yaml`. PAT adalah personal access token kita yang dihasilkan oleh Hasura DDN dan digunakan oleh CLI.

**Menjalankan Docker daemon sebagai pengguna non-root**  

Jika kita menjalankan Docker daemon sebagai pengguna non-root (mode rootless), versi 26.0 kini memungkinkan kita menonaktifkan isolasi host-loopback default yang mencegah komunikasi kembali ke mesin host.

Untuk menonaktifkan fitur ini, setel variabel lingkungan `DOCKERD_ROOTLESS_ROOTLESSKIT_DISABLE_HOST_LOOPBACK` ke `false` sebelum memulai daemon.

Ini akan memungkinkan komunikasi container-ke-host melalui alamat `10.0.2.2`.

Kita juga harus mengubah baris ini di file `compose.yaml` baik untuk layanan utama maupun konektor untuk mencerminkan perubahan ini:

```yaml
extra_hosts:
  - local.hasura.dev=host-gateway
```
menjadi:  

```yaml
Salin kode
extra_hosts:
  - local.hasura.dev=10.0.2.2
```

##### Langkah 3. Lihat API supergraph Kita
Di jendela terminal baru, kita bisa meluncurkan konsol proyek lokal kita menggunakan CLI.

Dari jendela terminal baru, jalankan:
```
ddn console --local
```

Ini akan membuka `https://console.hasura.io/local/graphql` dan memungkinkan kita melihat API (yang masih kosong) berjalan di mesin kita menggunakan Hasura Console!

![image](https://github.com/user-attachments/assets/3307236c-344e-426b-a6c1-f89e0bef5bc5)


**Hasura Console menampilkan skema kosong**

**Pengaturan privasi di beberapa browser**  
Pengaturan privasi, alat privasi, atau ekstensi browser kita mungkin mencegah Konsol mengakses instansi Hasura lokal kita. Hal ini bisa terjadi karena fitur yang dirancang untuk melindungi privasi dan keamanan kita. Jika kita mengalami salah satu masalah ini, disarankan untuk menonaktifkan pengaturan ini untuk domain `console.hasura.io`.

**Chrome dan Firefox** adalah browser yang direkomendasikan untuk pengalaman terbaik dengan Hasura Console termasuk untuk pengembangan lokal.

##### Langkah 4. Buat build baru untuk supergraph kita
Hasura menggunakan sistem build yang immutable yang menyediakan snapshot stabil API kita di setiap titik dalam proses pengembangan.

Mari buat build baru:
```
ddn supergraph build local
```

Perintah di atas akan meregenerasi semua JSON untuk supergraph kita. Karena kita menggunakan subcommand `local`, hasilnya akan disimpan di direktori lokal `./engine` secara default, yang direferensikan dalam `docker-compose.yaml`. Kita tidak akan melihat perubahan pada API yang dideploy saat ini, tetapi begitu kita mulai menambahkan subgraph dan sumber data, kita perlu menjalankan perintah ini untuk memperbarui API kita.

Sebagai fitur kenyamanan, saat menginisialisasi supergraph, kita juga mengatur konteks dalam CLI. Ini menghilangkan kebutuhan untuk menetapkan nilai default yang sering digunakan (seperti flag `--supergraph` atau `--subgraph`) saat menjalankan perintah CLI.

**Apa yang dilakukan oleh langkah ini?**  
CLI membuat semua file yang dibutuhkan untuk pengembangan lokal dan cloud di direktori proyek lokal kita, termasuk file docker compose untuk pengembangan lokal dan file bantuan untuk VS Code. Kita kemudian memulai Hasura Engine dan semua layanan yang dibutuhkan untuk menjalankan supergraph secara lokal dan melihat Hasura Console untuk melihat API kosong kita. Akhirnya, kita menetapkan konteks untuk supergraph dan membuat build baru — meskipun kosong — dari supergraph kita.

**Memulai supergraph baru atau bergabung dengan yang sudah ada**  
`ddn supergraph init` akan selalu menjadi perintah pertama yang kita jalankan saat memulai proyek baru. Namun, kita mungkin tiba di sini setelah diundang untuk bergabung dengan proyek yang sudah ada. Dalam kasus tersebut, supergraph kita sudah ada; kita cukup meng-clone repositori yang berisi supergraph dan melanjutkan ke bagian berikutnya.

Bagaimanapun cara kita memulai supergraph kita, kita harus membuat build dan kemudian menjalankannya menggunakan Docker. Ini adalah alur kerja tipikal saat memulai proyek baru atau melanjutkan proyek yang sudah ada.

### Membuat Subgraph  
Kita akan menambahkan subgraph ke supergraph.

Subgraph adalah cara untuk mengorganisasi data dan memungkinkan kita menghubungkan beberapa sumber data ke supergraph kita. Sebuah supergraph harus memiliki setidaknya satu subgraph, jika kita ingin menjalankannya.

#### Default

Ketika kita menginisialisasi supergraph baru, CLI akan membuat subgraph default yang disebut **app**. Namun, kita bisa mengikuti panduan ini untuk menambahkan subgraph berikutnya, memperluas API kita.

![image](https://github.com/user-attachments/assets/fb7c123d-70e0-49ac-878a-8a9f6a72ef52)


#### Langkah-langkah Membuat Subgraph

##### Syarat:
- DDN CLI, ekstensi VS Code, dan Docker terinstal
- Supergraph baru atau yang sudah ada

##### Langkah 1. Inisialisasi Subgraph

Jalankan perintah berikut, dengan menyesuaikan nama yang diinginkan dan lokasi penyimpanan metadata untuk subgraph kita:

```bash
ddn subgraph init my_subgraph --dir ./my_subgraph --target-supergraph ./supergraph.yaml
```
  
Perintah ini akan menampilkan pesan sukses (dan petunjuk untuk menambahkan konektor ke subgraph).
  
Hasura DDN meneruskan beberapa nilai ke perintah `subgraph init`:
  
**Subgraph name**: Kita menamai subgraph dengan nama `my_subgraph`.

**Directory**: `--dir`, kita menentukan direktori di mana subgraph akan dibuat. CLI akan membuat direktori ini.

**Target supergraph**: `--target-supergraph`, kita menentukan supergraph lokal dan cloud yang dibuat saat menginisialisasi supergraph.

**Apa yang dilakukan `subgraph init`?**
Perintah ini membuat direktori subgraph baru dan file subgraph.yaml. CLI juga mengedit file konfigurasi supergraph untuk menambahkan subgraph ini.

Subgraph ini adalah tempat untuk mengatur objek metadata yang mendefinisikan API kita. Tanpa konektor data, subgraph ini hanyalah placeholder yang belum banyak berfungsi.

##### Langkah 2. Set Konteks Subgraph
Selanjutnya, kita akan mengatur konteks subgraph dengan perintah berikut:

```bash
ddn context set subgraph ./my_subgraph/subgraph.yaml
```

**Apa yang dilakukan `context set subgraph`?**
Perintah ini memperbarui pasangan kunci-nilai dalam file konteks proyek di folder `.hasura`, sehingga subgraph yang digunakan adalah yang kita tentukan.

##### Langkah 3. Kustomisasi Prefiks
Untuk menghindari bentrokan antara root fields dan nama tipe GraphQL, kita dapat menyesuaikan prefiks untuk setiap subgraph. Misalnya, jika dua subgraph memiliki tipe `Users`, kita bisa menerapkan prefiks berbeda untuk membedakannya.

Tambahkan baris berikut di `subgraph.yaml`:

```yaml
kind: Subgraph
version: v2
definition:
  name: my_subgraph
  generator:
    rootPath: .
    graphqlRootFieldPrefix: my_subgraph_
    graphqlTypeNamePrefix: My_subgraph_
```
  
Dengan ini, setiap subgraph tetap unik dan tidak terjadi konflik nama.

### Menghubungkan ke Data
Kita dapat menghubungkan berbagai jenis data ke Hasura DDN.

Koneksi dilakukan melalui **data connector**, sebuah perangkat lunak yang independen dari Hasura Engine yang memfasilitasi koneksi.

Kita dapat menggunakan data connector "siap pakai" yang dibuat oleh Hasura, yang dibuat oleh komunitas, atau kita dapat membuat sendiri.

Data connector dapat digunakan langsung melalui **Hasura Connector Hub** menggunakan CLI. Connector Hub adalah daftar connector yang dikelola untuk menghubungkan sumber data kita.

Data connector mengenkapsulasi sumber data melalui layanan web dengan mengimplementasikan protokol dalam spesifikasi NDC.

Kita dapat menghubungkan sumber data cloud atau lokal melalui data connector ke Hasura DDN.

Kita dapat menggunakan CLI untuk menambahkan data connector untuk sumber data kita dan menghasilkan metadata Hasura.

#### Memperbarui Metadata Sumber Data  
Dalam siklus pengembangan sehari-hari kita, ada kemungkinan besar skema sumber data kita akan berubah. Ketika ini terjadi, kita perlu memperbarui konfigurasi data connector dan metadata supergraph agar sesuai dengan perubahan skema ini.

![image](https://github.com/user-attachments/assets/f23180e9-b6c3-4380-bd26-5276d0ca78cf)


##### Langkah 1. Introspeksi Sumber Data
**Required:**
- DDN CLI, ekstensi VS Code, dan Docker terpasang
- Supergraph baru atau yang sudah ada
- Subgraph baru atau yang sudah ada
- Connector yang sudah diinisialisasi.

Kita dapat menjalankan perintah `connector introspect` untuk membiarkan CLI introspeksi skema sumber data, memperbarui konfigurasi connector, dan juga memperbarui objek metadata `DataConnectorLink` yang sesuai agar supergraph dapat berinteraksi dengan connector.

Jalankan perintah berikut, perbarui nama yang dirujuk sesuai dengan connector kita:

```bash
ddn connector introspect my_connector
```

Setelah perintah ini dijalankan, kita dapat membuka file `my_subgraph/metadata/my_connector.hml` dan melihat skema DataConnectorLink yang sepenuhnya diperbarui untuk mencocokkan perubahan skema sumber data kita.

##### Langkah 2. Perbarui atau Tambah Model
Jika skema model yang ada berubah di sumber data kita, perbarui untuk memastikan metadata Hasura kita sesuai dengan skema sumber data.

```bash
ddn model update <model-name>
```
  
**Memiliki banyak model?**
Jika kita memiliki sejumlah besar model dan ingin memperbaruinya secara massal, Hasura DDN punya solusi untuk itu.

Jalankan perintah berikut:
```bash
ddn model update "*"
```
  
Kita akan melihat informasi output CLI tentang model mana yang sama dan mana yang telah berubah.

Sebagai alternatif, jika kita memiliki model yang perlu ditambahkan (misalnya, tabel baru di sumber data kita), kita perlu membuat file `hml` untuk sumber daya ini.

Jalankan perintah berikut:

```bash
ddn model add <connector-link-name> <collection-name>
```
  
**Memiliki banyak model baru untuk ditambahkan?**
Jika kita memiliki sejumlah besar model, perintah, atau hubungan dan ingin menambahkannya secara massal, Hasura DDN memiliki solusi yang sama seperti sebelumnya.

Jalankan perintah berikut:
```bash
ddn model add <connector-link-name> ''
ddn command add <connector-link-name> ''
ddn relationship add <connector-link-name> '*'
```
  
Kita akan melihat informasi output CLI tentang model mana yang sama dan mana yang telah berubah.

##### Apa yang dilakukan ini?
Dengan memperbarui file `my_connector.hml`, Hasura DDN telah menyediakan Hasura dengan tautan antara sumber data asli Hasura DDN dan tipe yang pada akhirnya akan Hasura DDN ekspos melalui API Hasura DDN.

Pastikan untuk membuat build baru sebelum menguji API kita:

```bash
ddn supergraph build local
```
  
### Build Supergraph & Lakukan Query  
Setelah menghubungkan sumber data dan mengekspos model, kita dapat membuat build lokal baru untuk supergraph kita. Build adalah keadaan metadata yang dikompilasi dan tidak dapat diubah yang digunakan oleh Hasura Engine untuk menjalankan API kita.

![image](https://github.com/user-attachments/assets/9b5e964f-0173-423e-a411-63594ca60fff)


**Required:**
- DDN CLI, ekstensi VS Code, dan Docker terinstal
- Proyek baru atau yang sudah ada
- Setidaknya satu subgraph
- Setidaknya satu konektor data yang berjalan
- Model dan/atau Perintah ditambahkan ke subgraph kita

#### Langkah 1: Buat build supergraph
Jalankan perintah berikut, sesuaikan jalur yang disebutkan dengan struktur direktori kita:
  
```bash
ddn supergraph build local
```
  
Ini akan membuat build supergraph kita yang terletak di direktori /engine/build.

**Jalankan mesin kita!**  
Ingin menguji supergraph kita? Pastikan layanan kita masih berjalan dengan perintah berikut:
  
```bash
ddn run docker-start
```
  
Jika kita belum menyertakan konektor kita dalam `compose.yaml`, jangan lupa untuk memulainya juga.

kita dapat memeriksa API lokal kita dengan membuka konsol Hasura menggunakan CLI:
  
```bash
ddn console --local
```
  
Pengaturan privasi di beberapa browser
Pengaturan browser kita atau alat privasi mungkin mencegah Konsol mengakses instance Hasura lokal kita. Jika kita mengalami masalah ini, disarankan untuk menonaktifkan pengaturan tersebut untuk domain console.hasura.io.

Chrome dan Firefox adalah browser yang direkomendasikan untuk pengalaman terbaik dengan Konsol Hasura, termasuk untuk pengembangan lokal.

#### Langkah 2: Tulis query pertama kita
Gunakan explorer GraphiQL untuk menulis query atau menyusunnya menggunakan menu di sisi kiri konsol. Ketika kita siap, tekan tombol run untuk mengeksekusi query kita.

Contoh, jika kita memiliki model Carts, kita dapat menjalankan query berikut:
  
```graphql
query MyFirstQuery {
  carts {
    id
    isComplete
    userId
  }
}
```
  
Semua tipe diberi namespace dengan subgraph tempat mereka berada.

**Apa yang sedang kita lakukan?**  
Saat kita mengeksekusi perintah di atas, CLI menggunakan metadata Hasura di direktori kita yang dihasilkan berdasarkan sumber data kita untuk membuat build lokal supergraph kita. Build lokal ini tidak dapat diubah dan dapat digunakan untuk menguji perubahan pada API kita.

**Langkah Selanjutnya**
Sekarang kita memiliki build dari supergraph kita, kita dapat melakukan lebih banyak hal dengannya. Berikut beberapa saran untuk melompat di sekitar bagian Getting Started sesuai minat kita:

- **Deploy ke cloud** untuk membuat API kita tersedia untuk dunia
- **Tambahkan izin** ke model kita
- **Buat relasi** antar model
- **Ubah data** kita dengan Hasura DDN
  
## Deploy Supergraph  
Kita akan mendepoloy supergraph kita ke Hasura DDN, layanan yang terdistribusi secara global, sangat tersedia, dan sangat cepat!

![image](https://github.com/user-attachments/assets/2df3adba-c8e7-40e7-ae0f-d46603513cad)


### Langkah-langkah untuk Mendepoloy Supergraph ke Hasura DDN
**Required:**
- DDN CLI
- Supergraph baru atau yang sudah ada
- Subgraph baru atau yang sudah ada
- Konektor data baru atau yang sudah ada
- Proyek baru atau yang sudah ada

#### Langkah 1: Bangun dan deploy supergraph kita
Jalankan perintah berikut:

```bash
ddn supergraph build create
```

**Proyek sudah diset dalam konteks**. Ingat, karena kita telah menyetel konteks proyek, kita tidak perlu menyertakan nama proyek sebagai flag dalam perintah.

#### Langkah 2: Jelajahi supergraph kita di Konsol Hasura
CLI akan memberikan versi build dan **Console URL Build**. Klik pada URL tersebut!

Kita dapat menjelajahi API untuk build ini di Konsol Hasura!

#### Langkah 3: Terapkan supergraph kita sebagai endpoint proyek.
Build yang diterapkan adalah build default yang disajikan oleh endpoint proyek Hasura DDN kita.

Untuk menerapkan build, jalankan:
  
```bash
ddn supergraph build apply <supergraph-build-version>
```
  
**Apa yang dilakukan pada langkah ini?**  
Ketika kita menjalankan perintah di atas, CLI menggunakan konfigurasi yang kita berikan untuk membuat build supergraph yang tidak dapat diubah di Hasura DDN. Build ini sekarang dapat diakses melalui endpoint GraphQL build dan di Konsol Hasura untuk dieksplorasi.

Rekan tim dapat menjelajahi API, berinteraksi dengan itu, dan memberikan umpan balik sebelum kita melakukan iterasi dan membuat build baru untuk pengujian. Jika kita siap, kita dapat menerapkan build agar disajikan oleh endpoint proyek. Jika kita merasa telah menerapkannya terlalu cepat, kita dapat dengan mudah menggulung kembali dengan menerapkan build yang lebih lama.

Saat ini, kita memiliki semua bahan dan pengetahuan untuk membuat supergraph yang kokoh yang menggabungkan data dari berbagai sumber dan mengagregasikannya menjadi API yang andal dan berkinerja tinggi. Sebelum pindah ke produksi, pertimbangkan sumber daya di bawah ini:

**Migrasi**  
Hasura merekomendasikan sejumlah solusi pihak ketiga untuk mengelola migrasi database. Umumnya, pengguna menerapkan migrasi melalui CI/CD dengan Flyway atau sumber daya serupa.

**Apakah Hasura mengelola migrasi?**  
Di v2, Hasura menyediakan alat migrasi bawaan. Namun, karena metadata v3 terpisah dari sumber data yang mendasarinya, kita bebas mengelola migrasi sesuai keinginan kita.

**Optimasi Kinerja**
Hasura menyediakan serangkaian alat observabilitas langsung di konsol DDN proyek. Kita dapat melihat jejak, rencana query, dan statistik penggunaan umum. Ini sangat berguna untuk mendiagnosis bottlenecks dan masalah umum dengan kinerja aplikasi kita.

**CI/CD**
Kita dapat membuat pipeline untuk deploy menggunakan alat apa pun yang kita inginkan. Direkomendasikan untuk menginisialisasi repositori git sejak awal proses pembuatan proyek dan memberikan operabilitas dengan variabel lingkungan, sehingga kita dapat mengikuti praktik terbaik alur kerja git untuk bergerak antara lingkungan pengembangan, staging, dan produksi. Selain itu, Hasura DDN menyediakan GitHub Action yang dapat dikonfigurasi untuk secara otomatis mengelola deploy kita.

## Kolaborator  
Hasura DDN memungkinkan beberapa pengguna dan tim untuk bekerja sama sebagai kolaborator pada proyek dengan memberikan peran dan izin khusus kepada setiap pengguna.

**Hanya Tersedia di DDN Base dan lebih tinggi**  
Untuk menambahkan kolaborator, proyek kita harus merupakan proyek DDN Base atau DDN Advanced.

### Peran yang Tersedia  
**Project Owner** = Memiliki semua kemampuan proyek termasuk penghapusan. Saat ini, kepemilikan proyek tidak dapat dialihkan.  
#### Supergraph  
**Admin Supergraph** = Sama dengan Project Owner, kecuali penghapusan proyek.     
**Read Only** = Pengguna dengan peran ini hanya dapat menjelajahi dan memvisualisasikan API.
#### Subgraph  
**Admin Subgraph** = Memiliki semua kemampuan terkait subgraph: membuat build subgraph, menerapkan build subgraph ke endpoint, membuat supergraph dengan patch subgraph, mengundang admin subgraph lainnya.  
**Developer** = Sama seperti Admin Subgraph, kecuali kemampuan mengundang orang lain dan menerapkan build subgraph ke endpoint.

### Cara Mengundang Kolaborator
#### Langkah 1: Navigasi ke bagian Kolaborator
Buka konsol proyek kita di [https://console.hasura.io](https://console.hasura.io) dan pilih proyek dari daftar proyek yang tersedia. Setelah proyek terbuka, klik **Pengaturan** di pojok kiri bawah, kemudian pilih **Kolaborator** dari menu Pengaturan Proyek:

![image](https://github.com/user-attachments/assets/7967ce60-318e-4c5e-95d0-686382bbe172)


#### Langkah 2: Masukkan informasi
Klik tombol **+ Undang Kolaborator** di pojok kanan atas bagian Kolaborator, masukkan alamat email kolaborator, tingkat akses supergraph atau subgraph, dan pilih peran mereka, serta untuk akses subgraph, peran mereka per subgraph.

![image](https://github.com/user-attachments/assets/8e84bbe7-9ace-4c0b-9b88-02db8f4b689f)


Pengundang akan menerima email dengan tautan yang memungkinkan mereka menerima undangan dan bergabung dengan proyek.

### Cara Menerima Undangan Kolaborasi  
#### Langkah 1: Klik tautan di email kita  
Dari email kita, klik tombol **Lihat undangan**. Ini akan mengarahkan kita ke [https://console.hasura.io](https://console.hasura.io) di mana kita dapat menerima undangan dan kemudian menjelajahi serta berkontribusi pada proyek sesuai peran kita.

#### Langkah 2: Jelajahi proyek  
Dari proyek baru kita, kita dapat menjelajahi konsol dengan:
- Menjalankan query dari GraphiQL explorer.
- Memvisualisasikan supergraph dengan tab Explorer.
- Melihat kolaborator lain yang hadir dalam proyek.

#### Langkah 3: Pelajari cara mengembangkan secara lokal  
Pemilik proyek kemungkinan memiliki repositori Git dengan konten proyek yang tersedia di layanan seperti GitHub. Untuk menjalankan supergraph secara lokal dan berkontribusi pada supergraph yang dideploy.

### Izinkan Pengguna Meminta Akses  
Kita dapat menyesuaikan pengaturan proyek kita untuk memungkinkan pengguna meminta akses saat menavigasi ke URL proyek kita. Untuk melakukan ini, klik tombol **Bagikan** di navigasi atas proyek kita dan pilih **Minta Akses**.

![image](https://github.com/user-attachments/assets/33b9558f-f520-45e6-9341-0982f11bf954)


Setiap kali seorang pengguna meminta akses, kita dapat menyetujui atau menolak permintaan tersebut dari modal ini.

