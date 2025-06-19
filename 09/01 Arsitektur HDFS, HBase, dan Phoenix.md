# Penjelasan Arsitektur HDFS, HBase, dan Phoenix

## Alur Data dan Sistem

![WhatsApp Image 2024-11-29 at 09 00 44_025f95f8](https://github.com/user-attachments/assets/86be33c4-f236-4178-8b80-25fc93e2ac79)


### Komponen Utama
1. **fawzi.avsc**  
   - Data input berupa file JSON:
     ```json
     {
       "nama": "fawzi",
       "umur": 17
     }
     ```

2. **HDFS (Hadoop Distributed File System)**  
   - Tempat penyimpanan awal file dalam sistem terdistribusi.  
   - Data dari HDFS dikonversi menjadi format NoSQL untuk diproses di HBase.

3. **HBase**  
   - Database NoSQL skala besar yang mengambil data dari HDFS.  
   - Data di sini dikelola untuk mendukung kueri cepat dan efisien.

4. **Phoenix**  
   - Lapisan query berbasis SQL untuk mengakses data HBase.  
   - Mendukung dua jenis koneksi:
     - **Thin Client (Port 8765)**: Untuk akses ringan.  
     - **Thick Client (Port 2181)**: Untuk akses yang lebih intensif.

5. **Aktor (Pengguna)**  
   - Pengguna akhir dapat berinteraksi dengan data melalui Phoenix, baik menggunakan aplikasi desktop maupun aplikasi berbasis web.

---

## HBase Cluster dan Koordinasi

![WhatsApp Image 2024-11-29 at 09 01 01_c41e9eb2](https://github.com/user-attachments/assets/564129ae-a07a-4f01-bf85-ce6e96247268)


### Komponen Tambahan
1. **Pengguna**  
   - Representasi aplikasi atau klien yang ingin mengakses data HBase.

2. **API Gateway atau Middleware**  
   - Menangani akses pengguna dan mengarahkan permintaan ke HBase Cluster.

3. **HBase Cluster**  
   - **Master Node**: Bertanggung jawab atas distribusi data dan metadata.  
   - **Follower Node**: Menyimpan data aktual yang digunakan oleh aplikasi.  
   - **Zookeeper**: Mengelola sinkronisasi antar node, memastikan kelancaran komunikasi, dan menangani failover.

---

## Alur Kerja Sistem
1. **Penyimpanan Data**:
   - Data dalam `fawzi.avsc` dimasukkan ke HDFS.
   - Dari HDFS, data dikonversi menjadi format NoSQL dan disimpan di HBase.

2. **Akses Data**:
   - Pengguna mengirim permintaan melalui Phoenix.
   - Permintaan diarahkan ke API Gateway, lalu ke HBase Cluster.
   - Master Node mengatur pengambilan data dari Follower Node.
   - Zookeeper memastikan komunikasi antar node tetap sinkron.

3. **Pengembalian Data**:
   - Data yang diambil dari HBase dikirim kembali ke pengguna melalui Phoenix.

---

## Fungsi Utama Sistem
- Menyimpan data secara terdistribusi dengan HDFS.  
- Mengelola dan memproses data skala besar dengan HBase.  
- Memudahkan akses data menggunakan SQL melalui Phoenix.  
- Menjamin sinkronisasi dan ketersediaan data dengan Zookeeper.
