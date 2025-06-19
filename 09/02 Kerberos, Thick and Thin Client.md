# Penjelasan Mekanisme Kerberos dan KDC Server, serta Perbedaan Thick Client dan Thin Client

## 1. Mekanisme Kerberos dan Apa Itu KDC Server?

Kerberos adalah protokol otentikasi jaringan yang dirancang untuk menyediakan komunikasi yang aman dalam jaringan yang tidak aman, seperti internet. Mekanisme Kerberos bekerja dengan cara mengonfirmasi identitas pengguna melalui penggunaan tiket yang hanya dapat digunakan dalam waktu terbatas, serta mengenkripsi komunikasi yang terjadi antara klien dan server. Berikut adalah langkah-langkah dasar dalam mekanisme Kerberos:

### Langkah-langkah Kerberos:

Kerberos adalah protokol yang terdiri dari tiga entitas, yaitu:
- **Klien** merupakan entitas yang berusaha meminta layanan serta memberikan identitasnya.
- **Application Server/Server Target** adalah suatu layanan yang ingin diakses oleh klien (atau pengguna).
- **Key Distribution Center (KDC)** adalah pihak ketiga terpercaya yang bertugas selama tahap autentikasi. Pada Active Directory, setiap pengontrol domain bertindak sebagai KDC.

Protokol Kerberos memiliki tiga subprotokol agar dapat melakukan aksinya:
- **Authentication Service (AS) Exchange:** yang digunakan oleh Key Distribution Center (KDC) untuk menyediakan Ticket-Granting Ticket (TGT) kepada klien dan membuat kunci sesi logon.
- **Ticket-Granting Service (TGS) Exchange:** yang digunakan oleh KDC untuk mendistribusikan kunci sesi layanan dan tiket yang diasosiasikan dengannya.
- **Client/Server (CS) Exchange:** yang digunakan oleh klien untuk mengirimkan sebuah tiket sebagai pendaftaran kepada sebuah layanan.

Penjelasan tambahan:
- **TGT (Ticket Granting Ticket):**
   - Tiket sementara yang dikeluarkan oleh KDC setelah autentikasi awal berhasil.
   - Digunakan untuk meminta tiket layanan ke TGS tanpa perlu mengirim ulang password.

![image](https://github.com/user-attachments/assets/5965efe0-9d4d-4412-ae25-900e8bb10039)


1. **AS Request (Step 1):**
   - Klien mengirimkan permintaan autentikasi (AS Request) ke KDC Server.
   - Permintaan ini berisi identitas klien (misalnya, username) dan informasi terkait waktu.

2. **AS Response dan Pengeluaran TGT (Step 2):**
   - KDC Server memverifikasi identitas klien menggunakan database autentikasi.
   - Jika berhasil diverifikasi, KDC Server memberikan TGT (Ticket Granting Ticket) kepada klien.
   - TGT ini dienkripsi dengan kunci rahasia KDC dan hanya dapat digunakan oleh klien yang sesuai.

3. **TGS Request (Step 3):**
   - Klien mengirimkan TGT yang diterimanya ke KDC Server dengan permintaan untuk mengakses layanan tertentu.
   - Permintaan ini disebut TGS Request (Ticket Granting Service Request).

4. **TGS Response dan Tiket Layanan (Step 4):**
   - KDC Server memverifikasi TGT dan mengecek apakah klien berhak mengakses layanan yang diminta.
   - Jika valid, KDC Server mengeluarkan tiket layanan (Service Ticket) untuk layanan tujuan.
   - Tiket ini dienkripsi dan hanya dapat dibaca oleh server tujuan.

5. **CS Request (Step 5):**
   - Klien mengirimkan Service Ticket kepada Server Target sebagai bagian dari permintaan untuk mengakses layanan.
   - Tiket ini membuktikan bahwa klien telah diautentikasi oleh KDC.

6. **Server Target Memberikan Akses (Step 6):**
   - Server Target memverifikasi tiket layanan menggunakan kunci yang dibagikan dengan KDC.
   - Jika tiket valid, Server Target memberikan akses ke layanan yang diminta oleh klien.

### Apa Itu KDC Server?
KDC (Key Distribution Center) adalah server yang berperan sebagai otoritas pusat dalam sistem Kerberos. KDC bertanggung jawab untuk mengelola dan mengeluarkan tiket otentikasi untuk klien dan server. KDC terdiri dari tiga komponen utama:
- **Authentication Server (AS):** Bagian yang pertama kali menerima permintaan dari klien untuk memulai proses otentikasi.
- **Ticket Granting Server (TGS):** Setelah klien terotentikasi, TGS memberikan tiket layanan yang diperlukan untuk mengakses layanan di dalam jaringan.
- **Database Utama (db):** Database untuk kunci rahasia bagi pengguna dan layanan yang dikelola.

KDC menjadi komponen penting dalam Kerberos karena menjamin integritas dan keamanan komunikasi antara klien dan server.

---

## 2. Perbedaan antara Thick Client dan Thin Client

![thin](https://github.com/user-attachments/assets/e22b5f69-be84-4935-9ae8-9fe754bb7cf1)


Penjelasan terkait arsitektur klien-server dua tingkat (two-tier architecture) dengan variasi pengelolaan tugas antara klien dan server adalah sebagai berikut:

- (a) Antarmuka pengguna bergantung pada terminal di sisi klien:
   - Hanya antarmuka dasar yang berada di sisi klien.
   - Semua kendali presentasi dilakukan di server, termasuk logika aplikasi.
   - Cocok untuk sistem terminal lama atau thin client.
- (b) Antarmuka pengguna penuh di klien:
   - Klien menjalankan seluruh perangkat lunak antarmuka pengguna.
   - Server hanya menangani data dan logika aplikasi utama.
   - Biasanya digunakan di aplikasi desktop yang memanfaatkan koneksi ke server.
- (c) Sebagian logika aplikasi di klien:
   - Sebagian kecil aplikasi, seperti validasi formulir, ditempatkan di klien.
   - Server tetap menangani sebagian besar logika dan data.
   - Meningkatkan efisiensi komunikasi jaringan.
- (d) dan (e) Untuk mesin klien yang kuat:
   - Klien menangani lebih banyak tugas (misalnya, antarmuka, validasi, sebagian besar aplikasi).
   - Server berfungsi sebagai penyedia data.
   - Cocok untuk perangkat modern dengan sumber daya besar.

### Apa Itu Thick Client dan Thin Client?

- **Thick Client (Fat Client):** Merupakan perangkat keras atau perangkat lunak yang memiliki lebih banyak kemampuan untuk melakukan pemrosesan di sisi pengguna. Dalam hal ini, aplikasi dan data biasanya disimpan secara lokal di perangkat tersebut. Fat client lebih di bebankan pada client. Client harus menyediakan user interface, aplikasi dan aplikasi database. Sehingga client di tuntut untuk mempunyai perangkat yang cukup bagus. Sedangkan server hanya menyediakan database yang nanti akan di panggil oleh pihak client.
- **Thin Client:** Merupakan perangkat yang bergantung pada server untuk sebagian besar prosesnya. Thin client tidak menyimpan aplikasi atau data secara lokal, melainkan mengakses aplikasi dan data yang berada di server atau cloud. Thin client lebih di bebankan pada server.  Server di tuntut harus menyediakan aplikasi, databases dan user interface yang akan di gunakan oleh pihak client. Sehingga pada pihak client hanya menyediakan peringkat keras untuk melakukan koneksi kepada server.

### Perbedaan Thick Client dan Thin Client

| Aspek                  | Thick Client                              | Thin Client                               |
|------------------------|-------------------------------------------|------------------------------------------|
| **Pemrosesan**          | Melakukan banyak pemrosesan secara lokal | Sebagian besar pemrosesan dilakukan di server |
| **Penyimpanan Data**    | Data dan aplikasi disimpan lokal di perangkat | Data dan aplikasi disimpan di server atau cloud |
| **Kebutuhan Jaringan**  | Dapat berfungsi tanpa koneksi internet atau jaringan yang stabil | Memerlukan koneksi jaringan yang selalu aktif dan stabil |
| **Biaya Implementasi**  | Biasanya lebih mahal karena memerlukan perangkat keras yang lebih kuat | Lebih murah karena perangkat keras lebih sederhana |
| **Manajemen & Pemeliharaan** | Pemeliharaan perangkat lebih kompleks karena masing-masing perangkat harus diatur | Pemeliharaan lebih mudah karena banyak pemrosesan ada di server |
| **Keamanan**            | Lebih rentan terhadap kehilangan data atau serangan, karena data disimpan lokal | Lebih aman karena data tidak disimpan lokal dan dikelola di server yang lebih terpusat |

### Kelebihan dan Kekurangan

#### Thick Client:
**Kelebihan:**
- Lebih mandiri, karena dapat melakukan banyak fungsi tanpa bergantung pada server.
- Pengguna dapat bekerja meskipun terputus dari jaringan.
- Apabila terjadi kerusakan pada server client masih bisa digunakan dan lebih memiliki privasi.

**Kekurangan:**
- Membutuhkan perangkat keras yang lebih kuat dan lebih mahal.
- Memiliki manajemen dan pemeliharaan yang lebih rumit, karena perangkat harus dikelola satu per satu.

#### Thin Client:
**Kelebihan:**
- Lebih murah dalam hal perangkat keras dan manajemen.
- Lebih mudah untuk dipelihara, karena sebagian besar proses dilakukan di server.
- Lebih aman karena data disimpan secara terpusat.

**Kekurangan:**
- Bergantung pada koneksi jaringan yang stabil untuk dapat bekerja karena kita melakukan interaksi secara terus menerus dengan server sehingga tidak boleh terjadi *loss connection*.
- Fungsionalitas terbatas, tergantung pada kapasitas dan kemampuan server.

---
