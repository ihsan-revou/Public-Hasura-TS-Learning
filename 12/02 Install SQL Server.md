## Dokumentasi Instalasi dan Konfigurasi SQL Server di Windows

### 1. **Persiapan**

#### Spesifikasi Minimum:
- **OS**: Windows 10 atau Windows Server 2016/2019/2022.
- **RAM**: Minimum 4 GB (disarankan 8 GB ke atas).
- **Penyimpanan**: Minimum 6 GB untuk instalasi dasar.

#### Download Installer:
1. Kunjungi halaman resmi [Microsoft SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads).
2. Pilih edisi yang sesuai:
   - **Developer**: Gratis untuk pengembangan.
   - **Express**: Gratis dengan fitur terbatas.
   - **Standard** atau **Enterprise**: Berbayar.

### 2. **Instalasi SQL Server**

#### Langkah-langkah Instalasi:
1. **Jalankan Installer:**
   - Jalankan file `SQLServer2019-x64-<edition>.exe` (atau versi yang diunduh).
   - Pilih **"Basic"** untuk instalasi cepat, atau **"Custom"** untuk konfigurasi lebih lanjut.

2. **Pilih Lokasi Instalasi:**
   - Tentukan lokasi tempat SQL Server akan diinstal, lalu klik **Install**.

3. **Setup Features:**
   - Jika memilih **Custom**, pilih fitur yang akan diinstal, seperti:
     - **Database Engine Services**: Untuk menggunakan SQL Server Database.
     - **SQL Server Replication**: Untuk replikasi data.
     - **Full-Text and Semantic Extractions for Search**: Untuk pencarian teks lengkap.

4. **Pilih Instance:**
   - Gunakan instance default atau buat instance bernama (untuk penggunaan multi-instance).

5. **Konfigurasi Server:**
   - Pilih **Authentication Mode**:
     - **Windows Authentication Mode**: Hanya menggunakan akun Windows.
     - **Mixed Mode**: Menggabungkan akun Windows dan SQL Server. Masukkan password untuk akun `sa` jika memilih ini.

6. **Konfigurasi Pengguna:**
   - Tambahkan akun pengguna yang akan memiliki akses admin ke SQL Server.

7. **Mulai Instalasi:**
   - Klik **Install** dan tunggu hingga proses selesai.

### 3. **Install SQL Server Management Studio (SSMS)**

#### Langkah-langkah Instalasi:
1. **Download SSMS:**
   - Kunjungi halaman [SQL Server Management Studio](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms).

2. **Instalasi:**
   - Jalankan installer dan ikuti instruksi untuk menyelesaikan instalasi.

### 4. **Menghubungkan ke SQL Server**

1. **Buka SSMS:**
   - Jalankan SQL Server Management Studio.

2. **Login:**
   - Masukkan:
     - **Server Name**: `localhost` (atau nama instance jika menggunakan named instance).
     - **Authentication**: Windows Authentication atau SQL Server Authentication.

3. **Tes Koneksi:**
   - Klik **Connect** untuk memastikan koneksi berhasil.

### 5. **Verifikasi Instalasi**

- Buka SSMS, buat database baru, dan jalankan query sederhana:

```sql
SELECT @@VERSION;
```

### 6. **Konfigurasi Remote Access**

#### Langkah-langkah:
1. **Aktifkan Protokol TCP/IP:**
   - Buka **SQL Server Configuration Manager**.
   - Aktifkan **TCP/IP** pada bagian **SQL Server Network Configuration**.

2. **Setel Firewall:**
   - Buka **Control Panel** > **System and Security** > **Windows Defender Firewall**.
   - Tambahkan aturan baru untuk membuka port **1433**.

3. **Konfigurasi Port Forwarding di Router:**
   - Masuk ke pengaturan router.
   - Tambahkan aturan port forwarding untuk meneruskan port **1433** ke IP lokal SQL Server Anda.

4. **Uji Koneksi dari Jaringan Lain:**
   - Gunakan string koneksi seperti berikut:

```bash
sqlcmd -S [IP Publik],1433 -U [username] -P [password]
```

### 7. **Tips Keamanan**

- Gunakan autentikasi yang kuat untuk akun SQL Server.
- Pertimbangkan untuk menggunakan enkripsi TLS/SSL.
- Hindari penggunaan akun `sa` untuk akses aplikasi, gunakan akun dengan hak terbatas.
