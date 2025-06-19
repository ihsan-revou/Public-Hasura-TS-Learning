# AUTH  

## Autentikasi & Hasura

Autentikasi adalah proses untuk memverifikasi identitas pengguna. 

Hasura DDN memanfaatkan variabel sesi yang mencakup informasi seperti pengguna, peran, organisasi, dan informasi lain yang diperlukan untuk menentukan hak akses data pengguna. Dengan variabel sesi ini, kita dapat menetapkan aturan izin pada domain data untuk memberikan kontrol akses yang lebih terperinci terhadap sumber daya.

Hasura bersifat netral terhadap layanan autentikasi yang kita gunakan. Tanggung jawab untuk menghasilkan variabel sesi diserahkan kepada layanan autentikasi baru atau yang sudah ada, sehingga memberikan fleksibilitas dan berbagai pilihan untuk kebutuhan autentikasi kita.

Autentikasi dapat dikonfigurasi melalui JSON Web Tokens (JWT) atau layanan webhook, dan dapat diintegrasikan dengan penyedia apa pun yang kita pilih (misalnya, Auth0, Firebase Auth, AWS Cognito, atau solusi kustom) untuk memverifikasi pengguna dan mengatur variabel sesi yang mengontrol akses ke data.

## Otorisasi Hasura

Otorisasi menentukan apa yang dapat diakses oleh pengguna yang telah diverifikasi.

Kita dapat menetapkan izin (juga dikenal sebagai aturan kontrol akses) pada jenis keluaran, model, dan perintah dalam domain data kita. Ada tiga bentuk izin:

1. **Izin Tipe**: Menentukan bidang mana dalam ObjectType yang dapat diakses oleh peran tertentu.
2. **Izin Model**: Menentukan baris mana dari Model yang dapat diakses oleh peran tertentu.
3. **Izin Perintah**: Menentukan apakah suatu Perintah dapat dieksekusi oleh peran tertentu.

Sebuah peran akan muncul ketika didefinisikan dengan salah satu dari tiga cara ini. Setiap permintaan ke Hasura harus menyertakan variabel sesi atau peran yang diperlukan dari layanan autentikasi kita. Kehadiran dan nilai dari peran-peran ini menentukan izin mana yang berlaku untuk permintaan tersebut. Tidak ada lagi konsep peran admin super-user bawaan di Hasura DDN.

Peran dan izin di Hasura diimplementasikan di lapisan Hasura Engine. Mereka tidak memiliki hubungan langsung dengan pengguna dan peran dari sumber data mana pun.

### Contoh
Untuk contoh izin otorisasi dalam metadata, lihat halaman izin di bagian pemodelan Supergraph.

### Menguji Izin
Kita dapat menguji izin secara langsung di antarmuka API Console Hasura:

1. Tetapkan izin yang diinginkan untuk tipe, model, atau perintah tertentu dalam metadata kita.
2. Lakukan permintaan melalui antarmuka API GraphiQL Console Hasura dengan token autentikasi yang sesuai dengan variabel sesi yang diperlukan.
3. Periksa data yang dikembalikan untuk memastikan bahwa itu sesuai dengan konfigurasi izin kita.

# CONNECTORS
  
## Apa itu Connector Data?
Connector data adalah layanan HTTP yang mengekspos serangkaian API yang digunakan Hasura untuk berkomunikasi dengan sumber data. Connector data bertanggung jawab untuk menafsirkan pekerjaan yang harus dilakukan atas nama Hasura Engine, menggunakan bahasa kueri asli dari sumber data.

Connector data yang dibangun sesuai dengan Spesifikasi NDC (Native Data Connector) menggunakan salah satu SDK yang tersedia memungkinkan siapa saja untuk menghubungkan sumber data yang kaya dan sangat asli ke supergraph mereka.

Spesifikasi NDC mendefinisikan serangkaian API yang harus diimplementasikan oleh sebuah connector data. API ini, yang menerima dan mengembalikan JSON, digunakan oleh Hasura untuk berkomunikasi dengan connector data. Spesifikasi NDC juga mendefinisikan serangkaian metadata yang harus disediakan oleh connector data kepada Hasura. Metadata ini digunakan oleh Hasura untuk mengintrospeksi connector data, menghasilkan skema GraphQL, dan mengeksekusi operasi terhadap sumber data.

Kita dapat membangun connector data sendiri atau menggunakan salah satu dari banyak connector yang tersedia di Hub Connector.

## Jenis Connector
Ada dua jenis utama connector yang tersedia untuk digunakan dalam proyek Hasura DDN:

### Connector Terverifikasi
Connector terverifikasi terdaftar di Hub Connector dan dapat dideploy ke Hasura DDN menggunakan Hasura CLI.

### Connector Tidak Terverifikasi
Connector tidak terverifikasi belum diverifikasi oleh Hasura. Connector ini hanya dapat dideploy di infrastruktur kita sendiri.

## Membangun Connector
Kita dapat membangun connector sendiri menggunakan salah satu SDK Hasura. Saat ini, Hasura memiliki SDK berikut yang tersedia:

- [**Rust Data Connector SDK**](https://github.com/hasura/ndc-sdk-rs)
- [**TypeScript Data Connector SDK**](https://github.com/hasura/ndc-sdk-typescript)

### Contoh Connector
- **Connector CSV** - Implementasi spesifikasi NDC langsung. Dokumentasi spesifikasi NDC dapat ditemukan di sini, dan panduan berdasarkan pengembangan connector untuk CSV dapat ditemukan di sini. Kita dapat menggunakan panduan ini sebagai referensi untuk membangun connector kita sendiri.
- **Connector Sendgrid** - Contoh implementasi API Sendgrid dapat ditemukan di GitHub di sini.

## Konektor Apache Phoenix  
Dengan konektor ini, Hasura memungkinkan kita untuk secara instan membuat API GraphQL real-time di atas model data di Phoenix. Konektor ini mendukung fungsi-fungsi Phoenix yang tercantum dalam tabel di bawah ini, memungkinkan operasi data yang efisien dan skalabel. Selain itu, pengguna mendapatkan manfaat dari semua fitur kuat dari platform Hasura Data Delivery Network (DDN), termasuk kemampuan `query pushdown` yang mendelegasikan operasi kueri ke database, sehingga meningkatkan optimasi kueri dan performa.

### Fitur  
Berikut adalah matriks semua fitur yang didukung untuk konektor Phoenix:

| Feature                          | Supported | Notes                          |
|---------------------------------|----------|----------------------------------|
| Native Queries + Logical Models | ✅        |                                  |
| Native Mutations                | ❌        |                                  |
| Simple Object Query             | ✅        |                                  |
| Filter / Search                 | ✅        |                                  |
| Simple Aggregation              | ✅        |                                  |
| Sort                            | ✅        |                                  |
| Paginate                        | ✅        |                                  |
| Table Relationships             | ❌        |                                  |
| Views                           | ✅        |                                  |
| Remote Relationships            | ✅        |                                  |
| Custom Fields                   | ❌        |                                  |
| Mutations                       | ❌        |                                  |
| Distinct                        | ❌        |                                  |
| Enums                           | ❌        |                                  |
| Naming Conventions              | ❌        |                                  |
| Default Values                  | ❌        |                                  |
| User-defined Functions          | ❌        |                                  |

### Sebelum Memulai  
- Buat akun Hasura Cloud  
- Instal CLI  
- Instal ekstensi Hasura di VS Code  
- Buat *supergraph*  
- Buat *subgraph*

### Menggunakan Konektor  
  
Apache Phoenix connector termasuk connector tidak terverifikasi. Connector ini belum diverifikasi oleh Hasura, sehingga tidak terdaftar di Hasura Hub dan tidak bisa langsung dideploy ke Hasura DDN menggunakan Hasura CLI. Connector Phoenix hanya bisa diimplementasikan dan di-deploy di infrastruktur kita sendiri.

Meskipun belum terverifikasi, kita tetap bisa menggunakan Phoenix connector dengan mengatur konfigurasi secara manual, seperti menyediakan URL JDBC yang tepat untuk menghubungkan database Phoenix dengan Hasura. Karena belum diverifikasi, kita harus memastikan sendiri performa, keamanan, dan kompatibilitas connector ini.

Contoh pengaturan JDBC untuk Phoenix connector:

```
JDBC_URL="jdbc:phoenix:localhost:2181:/hbase"
```
  
Connector ini berlisensi di bawah Apache License 2.0, sehingga bebas digunakan dan dimodifikasi dalam infrastruktur yang kita kelola sendiri.

  
# SUPERGRAPH MODELING
**Struktur Supergraph di Hasura DDN**  

![image](https://github.com/user-attachments/assets/f7f6a79d-38fc-4f45-a81d-5efc3feca0a1)


**Penjelasan Struktur Supergraph:**  

1. **Supergraph:**
   Pada bagian paling atas terdapat entitas yang disebut `my_supergraph`, yang terdiri dari beberapa subgraph. Supergraph adalah kombinasi dari beberapa subgraph yang dihubungkan bersama untuk membentuk satu API terpadu.

2. **Subgraph:**
   - Setiap *subgraph* berisi beberapa komponen, salah satunya adalah `engine metadata`.
   - Subgraph seperti `my_subgraph`, `another_subgraph`, dan `yet_another_subgraph` memiliki struktur serupa. Masing-masing memiliki metadata mesin yang mengelola beberapa aspek seperti:
     - **Models**
     - **Relationships**
     - **Permissions**
     - **Commands**

3. **ConnectorLink:**
   Setiap subgraph dihubungkan ke `a_connector` melalui elemen yang disebut *ConnectorLink*. *ConnectorLink* ini bertindak sebagai penghubung antara metadata mesin di subgraph dan sumber data yang ada di luar.

4. **a_connector:**
   `a_connector` adalah komponen yang memungkinkan setiap subgraph terhubung ke sumber data yang berbeda. Beberapa contoh sumber data:
   - **Data Source**: Sumber data yang disambungkan ke subgraph pertama.
   - Pada subgraph lainnya, sumber data mungkin berbeda seperti data di cloud atau sumber data eksternal lainnya.

5. **Data Source:**
   - Terdapat berbagai jenis sumber data yang dapat digunakan seperti yang terlihat di sisi kanan diagram:
     - **Data Source biasa**
     - **Cloud Data Source**
     - **Star-shaped Data Source** (kemungkinan mengacu pada sumber data khusus).

Secara keseluruhan, gambar ini mengilustrasikan bagaimana beberapa subgraph dikelola dalam *Supergraph*, di mana setiap subgraph memiliki konektor yang menghubungkannya ke sumber data eksternal melalui *ConnectorLink*.

# BUSINESS LOGIC  

Dengan Hasura, kita dapat secara instan dan mudah menggabungkan logika bisnis khusus sebagai bagian dari Supergraph API menggunakan konektor Hasura Lambda.

Transformasikan, mutasi, atau perkaya data, sambungkan ke layanan yang ada, atau perluas fungsionalitas data asli kita menggunakan fungsi, semuanya dihosting oleh Hasura atau di infrastruktur kita sendiri.  

## Pengenalan Lambda Connectors

Lambda connectors memungkinkan kita dengan mudah mengimplementasikan dan meng-host logika bisnis kustom dalam proyek Hasura kita.

Dengan lambda connectors, kita cukup menulis fungsi, melacaknya, dan fungsi-fungsi tersebut secara otomatis tersedia di API kita.

## Pendekatan Tradisional
Bayangkan kita ditugaskan untuk menangani logika bisnis kustom dalam setup middleware tradisional.

Setiap hari, kita terjun ke kode kustom dan menulis skrip untuk memparsing dan merutekan permintaan yang masuk ke handler yang sesuai. Kita bahkan belum mulai menulis logika bisnis kustom.

Selanjutnya, kita perlu memperkaya data. Ini kemungkinan melibatkan mengelola beberapa API yang berbeda untuk meningkatkan informasi mentah. Setelah itu, kita dengan cermat memformat dan mentransformasi data agar sesuai dengan struktur yang diinginkan. Penanganan kesalahan memerlukan logika kustom tersendiri untuk mengelola berbagai kasus kegagalan dengan efektif. Akhirnya, kita memformat respons dan mengirimkannya kembali ke klien, berharap tidak ada yang rusak dalam prosesnya.

Langkah-langkah manual ini memakan waktu, rentan terhadap kesalahan, dan semakin sulit dipertahankan seiring dengan berkembangnya aplikasi dan tim kita. Kebutuhan konstan untuk kode kustom di setiap tahap proses bisa sangat melelahkan dan membuat frustrasi.

### Arsitektur
Setup tradisional seperti ini membutuhkan layanan terpisah yang berjalan pada berbagai komponen infrastruktur. Kita mungkin meng-host sendiri atau bergantung pada banyak penyedia cloud. Sistem terdistribusi ini menyebabkan bottleneck, meningkatkan latensi, dan memerlukan lebih banyak sumber daya untuk pemeliharaan dan penskalaan. Koordinasi antara berbagai layanan bisa menjadi tantangan, menyebabkan potensi masalah sinkronisasi dan kompleksitas dalam proses deployment.

## Dengan Hasura DDN
Sekarang, bayangkan skenario berbeda dengan Hasura DDN.

Alih-alih memparsing dan merutekan permintaan secara manual, Hasura secara otomatis menangani tugas-tugas ini, membebaskan kita dari menulis kode kustom untuk setiap permintaan yang masuk.

Pengambilan data menjadi mudah karena kita tidak lagi harus mengelola banyak API eksternal. Hasura sudah memfederasi semua sumber kita dan menghasilkan kueri GraphQL untuk kita, menyederhanakan proses secara signifikan.

Kita cukup menulis fungsi kustom dalam TypeScript atau Python, Hasura mengintrospeksinya, dan kemudian mengintegrasikan fungsi-fungsi tersebut ke dalam API GraphQL kita secara mulus.

Dengan Hasura DDN, langkah-langkah manual yang membosankan dari middleware tradisional digantikan oleh proses yang efisien dan terstruktur yang memungkinkan kita fokus pada penulisan logika bisnis yang bermakna daripada kode boilerplate.

### Arsitektur
Kami dapat meng-host logika bisnis kustom ini untuk kita. Atau, jika kita mau, kita dapat meng-host di infrastruktur kita sendiri. Bagaimanapun, proses deployment kita telah disederhanakan secara signifikan. Hasura DDN menyediakan platform terpadu yang meminimalkan latensi, mengurangi kompleksitas koordinasi layanan, dan menawarkan kemampuan penskalaan yang mulus. Alat observasi dan manajemen terintegrasi memastikan bahwa logika bisnis kita berjalan secara efisien dan andal, sambil mengekspos logika bisnis kustom kita langsung ke konsumen API.
