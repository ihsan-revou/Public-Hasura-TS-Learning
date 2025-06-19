# Menganalisa Metrics di Elasticsearch

## Panduan Penggunaan Elastic

Kita akan mengakses GUI Elastic menggunakan Kibana. 
  
### 1. Login ke Kibana
  
Terlebih dahulu login ke kibana untuk mengakses GUI Elastic. Masuk ke endpoint elasticsearch ganti port elastic 9200 (Port Default Elastic) menjadi 5601 (Port Default Kibana) dan lalu masukkan username dan password.
  
![screencapture-10-100-13-25-5601-login-2024-12-06-16_17_43](https://github.com/user-attachments/assets/e7f739b8-690c-47ce-ad4d-2a5a15e68568)

  
### 2. Masuk ke Discover

Setelah itu pergi ke Discover dengan cara klik `Discover` pada sidebar menu. Lebih jelasnya perhatikan gambar berikut.

![Screenshot (5) (1)](https://github.com/user-attachments/assets/38c6ca93-7e31-4113-8093-3f014b3cd54a)
  
### 3. Filter Data View
  
Kita dapat memfilter data yang ditampilkan dengan klik dropdown button di sebelah button `Data view` pada pojok kiri atas. 

![Screenshot (6)](https://github.com/user-attachments/assets/88a40089-da56-456c-b219-44528eac1644)

### 4. Munculkan Metrics
  
Setelah itu, pilih metrics untuk memfilter data yang dimunculkan hanya terkhusus pada metrics, metrics yang dimaksud adalah metrics dari sistem observasi atau monitoring aplikasi, khususnya terkait penggunaan dan performa layanan Hasura GraphQL Engine. Metrics seperti ini digunakan untuk memantau aktivitas, performa, dan status sistem yang berjalan. Perhatikan gambar berikut untuk lebih jelasnya.

![Screenshot (7)](https://github.com/user-attachments/assets/243118a8-cf76-4d37-9340-2045fd7f152b)

### 5. Fitur Filter Metrics

Kita dapat memfilter data Metrics dengan menggunakan KQL. KQL (Kibana Query Language) adalah bahasa kueri yang memungkinkan pengguna untuk membuat kueri secara cepat dan intuitif di Kibana, terutama untuk pencarian dan penyaringan data yang disimpan di Elasticsearch. Berikut panduan penggunaannya.
  
#### 1. Pilih kolom `Filter your data using KQL syntax`.
     
   ![Screenshot (7) (1)](https://github.com/user-attachments/assets/b63d3567-2253-4d55-8826-284dbe5db255)

#### 2. Ketikan field atau element yang ingin dicari atau difilter
  
Dalam hal ini kita menfilter metrics berdasarkan `host.name` atau `nama host` yang mengacu pada identitas atau nama dari server atau pod yang menangani permintaan.  

![Screenshot (8)](https://github.com/user-attachments/assets/24aeea6d-f23a-4cef-bf0d-731e035606be)

  
#### 3. Equals dan exist
  
Ketika selesai mengetikkan `field` atau `elemen` pada kolom filter, kolom filter akan memberikan `suggestion` antara `equals` dan `exist`.
  
![Screenshot (9)](https://github.com/user-attachments/assets/b9c51bff-9bb2-4338-b622-807cc2091c2c)
  
**Perbedaan `:` (Equal Some Value) dengan `:*` (Exist in Any Form)**
  
Berikut adalah perbedaan utama antara penggunaan `:` dan `:*` di **Kibana Query Language (KQL)**:
  
---

1. **`:` (Equal Some Value)**
  
- **Deskripsi:**
  Operator ini digunakan untuk mencocokkan dokumen di mana sebuah field memiliki nilai **spesifik** yang ditentukan.

- **Karakteristik:**
  - Mencari kecocokan nilai secara eksplisit.
  - Mengabaikan dokumen yang tidak memiliki field tersebut.

- **Contoh:**
  - **Mencari dokumen dengan field `status` yang bernilai `200`:**
    ```kql
    status: 200
    ```
    Artinya: Hanya dokumen yang memiliki field `status` dengan nilai **tepat** `200` yang akan ditemukan.

  - **Mencari dokumen dengan field `extension` yang bernilai `"jpg"`:**
    ```kql
    extension: "jpg"
    ```

---

2. **`:*` (Exist in Any Form)**

- **Deskripsi:**
  Operator ini digunakan untuk memeriksa apakah sebuah field **ada** di dokumen, terlepas dari nilainya.

- **Karakteristik:**
  - Tidak peduli nilai dari field, selama field tersebut ada.
  - Berguna untuk memastikan keberadaan field di dokumen.

- **Contoh:**
  - **Mencari dokumen yang memiliki field `status`, apa pun nilainya:**
    ```kql
    status:*
    ```
    Artinya: Semua dokumen yang memiliki field `status`, bahkan jika nilainya `null`, akan ditemukan.

  - **Mencari dokumen yang memiliki field `extension`:**
    ```kql
    extension:*
    ```

---

**Perbandingan Equal Some Value and Exist in Any Form**

| Aspek               | `:` (Equal Some Value)            | `:*` (Exist in Any Form)      |
|----------------------|-----------------------------------|-------------------------------|
| **Tujuan**          | Mencari dokumen dengan nilai spesifik. | Memeriksa keberadaan field.  |
| **Menyaring Nilai** | Ya, hanya dokumen dengan nilai tertentu. | Tidak, semua dokumen dengan field tersebut ditemukan. |
| **Field Tidak Ada** | Tidak mengembalikan dokumen.       | Tetap mengembalikan dokumen jika field ada. |

---

Dalam hal ini, kita memilih equals untuk hanya menampilkan data dengan nilai tertentu saja. 
  
#### 4. Spesifik Value

Ketikkan value dari field yang ingin kita cari atau filter. Namun, KQL filter akan menampilkan suggestion value yang tersedia pada metrics.
  
![Screenshot (10)](https://github.com/user-attachments/assets/78fdaefc-5efa-4223-bff1-b6ab8f3cd81d)

Disini kita memilih `host.name` dengan value `hasura-ihsan-64595f95b5-hps99:8080`. Sehingga data yang ditampilkan nantinya hanya data yang milik `hasura-ihsan-64595f95b5-hps99:8080` saja. Hasilnya dapat dilihat pada gambar di bawah ini.
  
![Screenshot (11)](https://github.com/user-attachments/assets/70b93dab-f781-4927-bd87-49080217c48c)
  
  
#### 5. Toggle Details

Kita bisa melihat detail dari sebuah metric dengan mengklik button berikut.

![Screenshot (12)](https://github.com/user-attachments/assets/4d5e3a78-636c-4f2b-bc77-4a5cff69ad78)
  
Tampilan detail dari metricsnya seperti berikut. 
  
![Screenshot (13)](https://github.com/user-attachments/assets/e1812009-520b-4a1e-b28a-cc63d511d3cf)

  
Toggle details ini untuk melihat `element` atau `field` dan `value` dari metric yang dipilih secara lebih rapih dan mudah dibaca. Kita dapat melihat atau membaca metric dalam bentuk `tabel` ataupun file `json` seperti gambar berikut.

![Screenshot (13)](https://github.com/user-attachments/assets/a14edd11-fc7e-4d1d-9364-a903ca1ccb2c)

  
#### 6. Operator Dasar dalam KQL (Kibana Query Language) 

KQL mendukung berbagai operator logika dasar seperti **AND**, **OR**, dan **NOT** untuk mempermudah pencarian dan penyaringan data di Kibana.

![Screenshot (14)](https://github.com/user-attachments/assets/05851b37-52dc-4437-9276-a30b4c5c2ea5)

---

1. **AND (Operator Logika "Dan")**
   
Digunakan untuk mencari dokumen yang memenuhi **semua** kondisi yang ditentukan.
  
**Contoh:**
  
Mencari dokumen dengan `status` bernilai `200` dan ekstensi file `jpg`:

```kql
status: 200 AND extension: "jpg"
```
  
**Artinya:** Dokumen harus memiliki field status bernilai 200 dan field extension bernilai "jpg".
  
2. **OR (Operator Logika "Atau")**
   
Digunakan untuk mencari dokumen yang memenuhi salah satu kondisi yang ditentukan.
  
**Contoh:**
  
Mencari dokumen dengan status bernilai 200 atau ekstensi file jpg:

```kql
status: 200 OR extension: "jpg"
```
  
**Artinya:** Dokumen akan dipilih jika memiliki field status bernilai 200 atau field extension bernilai "jpg".

3. **NOT (Operator Negasi)**
   
Digunakan untuk mengecualikan dokumen yang memenuhi kondisi tertentu.
  
**Contoh:**
  
Mencari dokumen dengan status yang bukan 404:
  
```kql
NOT status: 404
```
  
**Artinya:** Dokumen akan dipilih jika field status bukan bernilai 404.

4. **Kombinasi AND, OR, NOT**
   
Operator dapat digabungkan untuk membuat query yang lebih kompleks.
  
**Contoh:**
  
Mencari dokumen dengan status bernilai 200 dan ekstensi file jpg atau png:
  
```kql
status: 200 AND (extension: "jpg" OR extension: "png")
```

**Artinya:** Dokumen harus memiliki field status bernilai 200 dan field extension bernilai "jpg" atau "png".

Mencari dokumen dengan ekstensi file jpg, tetapi bukan status 404:

```kql
extension: "jpg" AND NOT status: 404
```
  
5. **Tanda Kurung (Parentheses)**
    
Digunakan untuk mengelompokkan logika dalam query agar lebih jelas atau untuk menentukan prioritas eksekusi.

**Contoh:**
Mencari dokumen dengan status 200 atau 404, dan ekstensi file jpg:

```kql
(status: 200 OR status: 404) AND extension: "jpg"
```

**Artinya:** Dokumen harus memiliki status 200 atau 404, dan ekstensi "jpg".

6. **Operator Logika Default**
   
Jika operator logika tidak ditentukan, KQL menggunakan operator AND secara default.

**Contoh:**
  
Query tanpa operator eksplisit:
  
```kql
status: 200 extension: "jpg"
```

**Artinya:** Sama seperti status: 200 AND extension: "jpg".

#### 7. Contoh Kombinasi

1. **Mencari dokumen di mana field `status` ada, tetapi nilainya bukan `404`:**
   ```kql
   status:* AND NOT status: 404
   ```

2. **Mencari dokumen di mana field `user_agent` ada dan memiliki nilai `"Chrome"`:**
   ```kql
   user_agent:* AND user_agent: "Chrome"
   ```

---

## Analisis Hasura Metrics di Elastic  

Berikut adalah penjelasan mengenai metrik yang dihasilkan oleh Hasura dalam proses observabilitasnya. Metrik-metrik ini memberikan wawasan tentang performa, status, dan operasi dari layanan.

---

### **1. Informasi Waktu**
- **`@timestamp`**: Menunjukkan waktu saat metrik dicatat dalam format UTC ISO8601.
- **Interval**: Beberapa metrik melibatkan durasi pengambilan data (misalnya, per detik atau menit).

---

### **2. Informasi Sumber Data**
- **`host.hostname`**: Nama host atau server tempat metrik dihasilkan.
- **`host.name`**: Nama lengkap atau alias untuk host.
- **`service.name`**: Nama layanan atau aplikasi (contoh: "hasura").
- **`data_stream.type`**: Jenis data, seperti `metrics`, `logs`, atau `traces`.
- **`data_stream.dataset`**: Nama dataset terkait (contoh: `generic`).
- **`data_stream.namespace`**: Namespace atau lingkungan (contoh: `default` atau `production`).

---

### **3. Jenis dan Kategori Metrik**
- **`metric_name`**: Nama metrik yang mencerminkan jenis datanya, seperti:
  - `hasura_graphql_requests_total`
  - `hasura_otel_dropped_logs`
  - `hasura_cache_request_count`
- **`operation_type`**: Jenis operasi yang sedang diukur (misalnya, `query`, `mutation`, `subscription`, atau `unknown`).
- **`reason`**: Alasan yang diberikan untuk kondisi tertentu (contoh: `buffer_full`, `send_failed`).
  - **Definisi**: 
    Field ini menjelaskan alasan spesifik untuk suatu status atau kegagalan.
  - **Nilai Umum**:
    - `buffer_full`: Terjadi ketika buffer penuh, sehingga data tidak dapat dikirim atau diterima.
    - `send_failed`: Gagal mengirim data karena alasan tertentu, seperti masalah jaringan.
    - `connection_failed`: Koneksi tidak berhasil terjalin.
  - **Contoh Penggunaan**:
    - Jika log menampilkan `reason: buffer_full`, hal ini biasanya mengindikasikan adanya kemacetan pada sistem.

  ![Screenshot (20)](https://github.com/user-attachments/assets/3d99b31c-86d6-4269-a85a-ae9409d70bb8)

- **`status`**: Status hasil operasi (misalnya, `success`, `failed`, `hit`, `miss`).
  - **Definisi**: 
    Menandakan hasil operasi yang lebih spesifik atau status cache.
  - **Nilai Umum**:
    - `hit`: Data ditemukan di cache dan digunakan.
    - `miss`: Data tidak ditemukan di cache, sehingga harus diambil dari sumber utama.
    - `success`: Operasi berhasil.
    - `failed`: Operasi gagal.
  - **Contoh Penggunaan**:
    - Log dengan `status: hit` menunjukkan bahwa data berhasil diambil dari cache, mempercepat proses.

  
![Screenshot (15)](https://github.com/user-attachments/assets/14eda52c-fff0-4555-ab45-5e37e66bb127)


---

### **4. Nilai Utama (Value)**
- **Counter**: Nilai kumulatif yang terus bertambah, seperti total permintaan atau error.
  - Contoh: `hasura_graphql_requests_total: 1`
- **Gauge**: Nilai yang dapat naik atau turun, seperti penggunaan CPU atau koneksi aktif.
- **Histogram**: Distribusi nilai untuk mengukur latency atau ukuran payload.
- **Summary**: Statistik ringkasan, seperti rata-rata atau persentil.

---

### **5. Informasi Kontekstual**
- **`operation_name`**: Nama operasi GraphQL (jika tersedia).
- **`parameterized_query_hash`**: Hash unik dari query parameterisasi.
- **`response_status`**: Status respons API (contoh: `success`, `failed`).
  - **Definisi**: 
    Field ini menunjukkan status respons dari operasi yang dilakukan oleh API Hasura atau telemetri.
  - **Nilai Umum**:
    - `success`: Operasi berhasil dilakukan.
    - `failed`: Operasi gagal.
    - `unknown`: Tidak dapat ditentukan statusnya.
  - **Contoh Penggunaan**:
    - Jika `response_status` adalah `failed`, maka penyebab kegagalan dapat dianalisis melalui field `reason`.

![Screenshot (17)](https://github.com/user-attachments/assets/91351b83-46ec-4f29-a07f-d71fce5b51f5)


- **`error_message`** (opsional): Pesan kesalahan yang terkait dengan error.

---

### **6. Metadata Teknis**
- **`_index`**: Indeks dalam sistem penyimpanan (contoh: Elasticsearch).
- **`_id`**: ID unik untuk setiap entri metrik.
- **`_version`**: Versi data jika ada modifikasi.

---

### **7. Field Khusus untuk Jenis Metrik Tertentu**
#### **GraphQL**
- **Field**: `operation_name`, `operation_type`, `response_status`, `hasura_graphql_requests_total`.

#### **Cache**
- **Field**: `hasura_cache_request_count`, `status` (apakah `hit` atau `miss`).

#### **Event**
- **Field**: `hasura_cron_events_invocation_total`, `hasura_oneoff_events_invocation_total`.

#### **Telemetry**
- **Field**: `hasura_otel_dropped_logs`, `hasura_otel_dropped_spans`, `reason`.

---

### **Contoh Field dalam Setiap Jenis Metrik**
| **Jenis Metrik**   | **Field Utama**                                                                 |
|---------------------|---------------------------------------------------------------------------------|
| **GraphQL**         | `operation_name`, `operation_type`, `response_status`, `hasura_graphql_requests_total` |
| **Cache**           | `hasura_cache_request_count`, `status`                                         |
| **Events**          | `hasura_cron_events_invocation_total`, `hasura_oneoff_events_processed_total`  |
| **Telemetry**       | `hasura_otel_dropped_logs`, `hasura_otel_dropped_spans`, `reason`              |
| **Generic Metrics** | `host.hostname`, `service.name`, `data_stream.dataset`, `@timestamp`           |

---

### **8. Jenis Subscription dalam Hasura: `live-query` vs. `streaming`**
  
![Screenshot (19)](https://github.com/user-attachments/assets/93ef9823-88f6-4b1a-b3c7-6505c140f24c)
  

Pada Hasura, field `subscription_kind` mendefinisikan jenis mekanisme subscription yang digunakan saat memanfaatkan **API Subscription**. Ada dua jenis utama: **`live-query`** dan **`streaming`**. Berikut adalah penjelasan detail mengenai masing-masing jenis:

---

#### **1. `live-query`**

##### **Definisi**
- Ini adalah pendekatan tradisional yang digunakan oleh Hasura untuk berlangganan perubahan data.
- **Live-query** melakukan polling secara periodik pada server untuk memeriksa perubahan di database.
- Ketika ada perubahan terdeteksi, data yang diperbarui akan dikirimkan ke klien.

##### **Karakteristik Utama**
- Memberikan snapshot terbaru dari hasil query kepada klien.
- Cocok untuk aplikasi yang tidak memerlukan latensi sangat rendah tetapi tetap membutuhkan data yang selalu diperbarui.
- Beban server lebih tinggi karena adanya polling konstan pada database.

##### **Kasus Penggunaan**
- Memantau data yang relatif statis, seperti total jumlah pengguna atau statistik sistem.

---

#### **2. `streaming`**

##### **Definisi**
- Jenis subscription ini memanfaatkan fitur PostgreSQL seperti **logical decoding** dan **change data capture (CDC)**.
- Dengan **streaming**, perubahan data langsung dikirim ke klien secara real-time.

##### **Karakteristik Utama**
- Memberikan pembaruan secara real-time tanpa polling, sehingga lebih efisien untuk beban kerja tinggi.
- Data dikirim secara incremental, artinya hanya perubahan (delta) yang didorong ke klien.
- Ideal untuk skenario yang membutuhkan pembaruan dengan latensi rendah.

##### **Kasus Penggunaan**
- Aplikasi dengan umpan data langsung, seperti harga saham, skor pertandingan, atau notifikasi transaksi.

---

#### **Perbandingan: `live-query` vs. `streaming`**

| **Kriteria**          | **Live-Query**                     | **Streaming**                     |
|-----------------------|-------------------------------------|------------------------------------|
| **Metode Pembaruan**  | Polling                            | Push-based (real-time)            |
| **Beban Server**      | Tinggi (karena polling)            | Rendah (efisien)                  |
| **Latensi**           | Sedang (berbasis interval polling) | Rendah (real-time)                |
| **Data yang Dikirim** | Snapshot hasil query               | Pembaruan incremental saja        |
| **Kasus Penggunaan**  | Pembaruan real-time non-kritis     | Pembaruan real-time yang krusial  |

---

#### **Cara Kerja `subscription_kind`**
- Field `subscription_kind` menentukan mekanisme yang digunakan untuk API subscription:
  1. **`live-query`**: Digunakan untuk subscription data berbasis polling.
  2. **`streaming`**: Digunakan untuk subscription data real-time berdasarkan perubahan.

---

#### **Catatan:**
- Pilih **`streaming`** jika latensi rendah dan responsivitas real-time sangat penting.
- Pilih **`live-query`** untuk aplikasi sederhana yang tidak memerlukan pembaruan segera tetapi tetap membutuhkan data yang segar.

---

### **9. Field `body`**

  ![Screenshot (18)](https://github.com/user-attachments/assets/eb708477-a92e-4854-a376-dac4e3d2cc58)

  
- **Definisi**:
  Field `body` mencakup detail atau payload yang berkaitan dengan operasi tertentu.
- **Kemungkinan Nilai** (Berdasarkan Gambar yang Diunggah):
  - **`"Closing all websocket connections as the metadata has changed"`**:
    - Artinya: Semua koneksi websocket ditutup karena metadata telah diperbarui.
    - Biasanya terjadi setelah perubahan pada konfigurasi metadata.
  - **`"Otel exporter: Delivered metrics"`**:
    - Artinya: Metode telemetri berhasil mengirim metrik ke sistem observasi.
  - **`"Otel exporter: Delivered spans"`**:
    - Artinya: Spans (jejak log distribusi) berhasil dikirim.
  - **`"Otel exporter: Failed to deliver logs"`**:
    - Artinya: Gagal mengirim log telemetri, kemungkinan karena koneksi HTTP bermasalah.
    - Alasan kegagalan biasanya terdapat pada `reason` (contoh: `ConnectionFailure`).
  - **`"Obtained telemetry sample"`**:
    - Artinya: Sampel telemetri berhasil diambil untuk diproses.

---
