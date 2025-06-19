# Analisis Metrik Hasura GraphQL dari APM Elastic

Analisis dilakukan dengan memeriksa data dari **`APM Elastic`** setelah menjalankan query GraphQL pada Hasura.

![image](https://github.com/user-attachments/assets/55a4cc35-a88a-4e64-9d34-7da8119bcb95)


![image](https://github.com/user-attachments/assets/70cb5bef-ec1b-49af-a1db-26aa84f63fea)


## Throughput dan Transaksi

![image](https://github.com/user-attachments/assets/86d901bf-2f38-4639-8b93-e793d1f2d783)


**APM Elastic** menyediakan pemantauan kinerja aplikasi yang komprehensif. Berikut adalah penjelasan mengenai metrik **Throughput** dan **Transaksi** yang tercatat dalam dashboard APM:

### 1. Throughput
  - **Definisi**: Throughput mengukur **jumlah transaksi atau permintaan** yang diproses oleh aplikasi dalam jangka waktu tertentu.
  - **Satuan**: Biasanya diukur dalam **transactions per minute (tpm)**.
  - **Arti**: 
      - Semakin tinggi throughput, semakin banyak permintaan yang diproses oleh aplikasi.
      - Throughput menggambarkan tingkat beban atau **load** pada aplikasi.
  - **Grafik**: Menunjukkan jumlah transaksi per menit dalam rentang waktu yang ditentukan.

**Contoh pada gambar**:
  - Untuk endpoint `/v1/graphql`, throughput tercatat **62.7 tpm**.
  - Endpoint `/v1/entitlement` menunjukkan throughput **< 0.1 tpm**, yang berarti sangat jarang diakses.

### 2. Transaksi
- **Definisi**: Transaksi adalah **unit permintaan atau API call** yang diterima oleh aplikasi dan dipantau oleh APM.
- **Fungsi**:
   - Setiap endpoint atau operasi aplikasi yang dipantau dianggap sebagai transaksi.
   - Transaksi melacak **latency** (waktu respons rata-rata), **throughput**, **error rate**, dan **impact**.
- **Rincian di tabel**:
   - **Latency (avg.)**: Waktu rata-rata yang dibutuhkan untuk menyelesaikan permintaan (dalam milidetik).
   - **Throughput**: Jumlah transaksi (request) per menit.
   - **Failed Transaction Rate**: Persentase permintaan yang gagal.
   - **Impact**: Pengaruh endpoint terhadap performa keseluruhan aplikasi.

**Contoh pada gambar**:
- **`/v1/graphql`**:
   - Latency: **2.5 ms** (sangat cepat)
   - Throughput: **62.7 tpm** (sering diakses)
   - Impact: Tinggi (endpoint ini sangat penting dalam aplikasi).
- **`/v1/entitlement`**:
   - Latency: **0.2 ms** (cepat)
   - Throughput: **< 0.1 tpm** (jarang digunakan)
   - Impact: Rendah (penggunaan endpoint sangat sedikit).

---

## Rentang Waktu

![image](https://github.com/user-attachments/assets/32515e0f-2f7e-4537-ada8-7152a30d44a8)

Berikut penjelasan mengenai **rentang waktu awal dan akhir** untuk data grafik yang terlihat di dashboard **APM Elastic** pada endpoint **`/v1/graphql`**:

### 1. Rentang Waktu pada Grafik
- Rentang waktu **awal**: Sekitar pukul **13:55:00**.
- Rentang waktu **akhir**: Sekitar pukul **14:20:00**.
- **Durasi**: Grafik ini mencakup periode **25 menit**.

### 2. Detail Data Berdasarkan Rentang Waktu
#### Grafik Latency (Kiri)
- **Awal (13:55:00 - 14:10:00)**:
   - Tidak ada aktivitas signifikan yang tercatat di awal.  
   - Waktu respons mulai muncul sekitar pukul **14:10**, dengan angka latency sekitar **2 ms**.

- **Puncak Aktivitas (14:10:00 - 14:15:00)**:  
   - Latency tetap **stabil** di kisaran **2 ms**, yang menunjukkan performa aplikasi yang konsisten meski terjadi lonjakan jumlah request.

- **Akhir (14:15:00 - 14:20:00)**:  
   - Latency menurun menjadi mendekati **0 ms**.
   - Penurunan ini terjadi karena penurunan jumlah transaksi atau throughput pada akhir periode tersebut.

#### Grafik Throughput (Kanan)
- **Awal (13:55:00 - 14:10:00)**:
   - Tidak ada transaksi yang diproses pada rentang waktu awal, yaitu **0 tpm**.

- **Puncak Aktivitas (14:10:00 - 14:15:00)**:  
   - Terjadi lonjakan throughput hingga mencapai **1,000 tpm**.
   - Hal ini menunjukkan peningkatan signifikan dalam jumlah permintaan atau transaksi yang diterima aplikasi.

- **Akhir (14:15:00 - 14:20:00)**:  
   - Throughput mulai menurun dan mencapai **0 tpm** pada akhir periode.
   - Ini menandakan bahwa permintaan mulai berkurang atau berhenti sama sekali.

---

## Latency Distribution

![image](https://github.com/user-attachments/assets/c297655e-80d8-4834-91ad-3e42073ac86d)


Berdasarkan **distribusi latency** yang terlihat pada gambar **APM Elastic**, berikut adalah penjelasan lebih lanjut:

### 1. Distribusi Latency
- **Definisi**: Distribusi **latency** mengukur waktu yang dibutuhkan untuk menyelesaikan transaksi dalam aplikasi. Grafik ini menunjukkan jumlah transaksi yang terjadi pada rentang waktu latency tertentu.
- **Sumbu X (Horizontal)**: Menampilkan **latency** dalam satuan **microsecond (µs)** hingga **millisecond (ms)**.
- **Sumbu Y (Vertikal)**: Menampilkan jumlah transaksi pada interval waktu latency tertentu.
- **Data Utama**:
   - **Current Sample**: Garis hijau menunjukkan bahwa **latency** berada di sekitar **600 µs**.
   - **95th Percentile (95p)**: Artinya, **95% dari transaksi** memiliki latency lebih rendah dari **5 ms**.

### 2. Distribusi Latency Berdasarkan Grafik
- **Rentang Utama**:
   - Sebagian besar transaksi memiliki latency **antara 400 µs hingga 5 ms**.
   - Distribusi ini membentuk dua kelompok utama:
      - **Kelompok 1**: Banyak transaksi berada pada rentang **400-900 µs**, menandakan mayoritas request memiliki latency rendah.
      - **Kelompok 2**: Terdapat lonjakan transaksi di sekitar **3-6 ms**, yang menunjukkan sebagian transaksi memerlukan waktu lebih lama.
- **Kelompok Latency Tinggi**:
   - Beberapa transaksi memiliki latency antara **20 ms hingga 300 ms**, namun jumlahnya sangat sedikit.

---

## Metadata

![image](https://github.com/user-attachments/assets/60cce8ad-7b12-4c1b-9fcc-2ab51583abb6)


![image](https://github.com/user-attachments/assets/89466d69-3162-4988-9ade-575957e05279)


![image](https://github.com/user-attachments/assets/6730ed5c-1d1e-46c3-9722-c54cce7ecc5a)


![image](https://github.com/user-attachments/assets/f940f05a-68ce-45b0-a426-b39150872506)


![image](https://github.com/user-attachments/assets/edf8dc83-72e6-4ba8-8d49-e0caac617ea3)


---

## Timeline /v1/graphql

Lihat transaksi dalam Discover
![image](https://github.com/user-attachments/assets/9967dc7c-06f9-4acc-b7cf-1b3ffbbd68ae)


```json
{
  "_index": "apm-7.17.18-transaction-000001",
  "_id": "u2l3r5MB2LTZdF1KamMS",
  "_version": 1,
  "_source": {
    "observer": {
      "hostname": "worker1.k8s.alldataint.com",
      "id": "4fa9447e-2492-4e9b-af43-0977fa67cb28",
      "type": "apm-server",
      "ephemeral_id": "f1168f81-ff63-485a-a3de-7bad197ee4f4",
      "version": "7.17.18",
      "version_major": 7
    },
    "agent": {
      "name": "otlp",
      "version": "unknown"
    },
    "trace": {
      "id": "fe75ad67ea3f85dd40b48cf819cdbaee"
    },
    "@timestamp": "2024-12-10T14:30:00Z"
    "ecs": {
      "version": "1.12.0"
    },
    "service": {
      "framework": {
        "name": "hasura",
        "version": "v2.42.0"
      },
      "name": "hasura",
      "language": {
        "name": "unknown"
      }
    },
    "event": {
      "ingested": "2024-12-10T07:27:39.026596459Z",
      "outcome": "success"
    },
    "processor": {
      "name": "transaction",
      "event": "transaction"
    },
    "transaction": {
      "duration": {
        "us": 435
      },
      "result": "Success",
      "name": "/v1/graphql",
      "id": "540bd2d697c0aa8a",
      "type": "custom",
      "sampled": true
    },
    "labels": {
      "session_variables": "{\"x-hasura-role\":\"admin\"}",
      "enduser_role": "admin",
      "graphql_operation_name": "TrigetUser",
      "request_id": "00866697-d793-42c1-a693-df18a8c34000",
      "http_response_content_length": "165"
    },
    "timestamp": {
      "us": 1733815369236887
    }
  },
  "fields": {
    "transaction.name.text": [
      "/v1/graphql"
    ],
    "service.framework.version": [
      "v2.42.0"
    ],
    "labels.request_id": [
      "00866697-d793-42c1-a693-df18a8c34000"
    ],
    "service.language.name": [
      "unknown"
    ],
    "transaction.result": [
      "Success"
    ],
    "transaction.sampled": [
      true
    ],
    "transaction.id": [
      "540bd2d697c0aa8a"
    ],
    "trace.id": [
      "fe75ad67ea3f85dd40b48cf819cdbaee"
    ],
    "processor.event": [
      "transaction"
    ],
    "agent.name": [
      "otlp"
    ],
    "event.outcome": [
      "success"
    ],
    "service.name": [
      "hasura"
    ],
    "service.framework.name": [
      "hasura"
    ],
    "processor.name": [
      "transaction"
    ],
    "transaction.duration.us": [
      435
    ],
    "observer.version_major": [
      7
    ],
    "observer.hostname": [
      "worker1.k8s.alldataint.com"
    ],
    "labels.session_variables": [
      "{\"x-hasura-role\":\"admin\"}"
    ],
    "transaction.type": [
      "custom"
    ],
    "event.ingested": [
      "2024-12-10T07:27:39.026Z"
    ],
    "observer.id": [
      "4fa9447e-2492-4e9b-af43-0977fa67cb28"
    ],
    "timestamp.us": [
      1733815369236887
    ],
    "@timestamp": [
      "2024-12-10T07:22:49.236Z"
    ],
    "labels.graphql_operation_name": [
      "TrigetUser"
    ],
    "labels.http_response_content_length": [
      "165"
    ],
    "observer.ephemeral_id": [
      "f1168f81-ff63-485a-a3de-7bad197ee4f4"
    ],
    "ecs.version": [
      "1.12.0"
    ],
    "observer.type": [
      "apm-server"
    ],
    "observer.version": [
      "7.17.18"
    ],
    "agent.version": [
      "unknown"
    ],
    "transaction.name": [
      "/v1/graphql"
    ],
    "labels.enduser_role": [
      "admin"
    ]
  }
}
```

Kode **dokumen JSON** untuk **data transaksi APM (Application Performance Monitoring)** dari Elastic APM. Data ini berisi informasi mengenai satu transaksi yang dilacak oleh **APM Server**. Berikut penjelasan setiap komponen penting dalam dokumen ini:

### **1. Informasi Observer (APM Server)**
- **`observer.hostname`**: `worker1.k8s.alldataint.com`  
   Nama host server APM yang mengamati transaksi ini.
- **`observer.id`**: ID unik dari APM server (`4fa9447e-2492-4e9b-af43-0977fa67cb28`).
- **`observer.type`**: `apm-server`  
   Menunjukkan bahwa data ini berasal dari Elastic APM server.
- **`observer.version`**: `7.17.18`  
   Versi dari APM server yang digunakan.
- **`observer.ephemeral_id`**: ID ephemeral (sementara) untuk instance server.

### **2. Informasi Agen**
- **`agent.name`**: `otlp`  
   Nama agen yang mengirim data ke APM (dalam hal ini menggunakan protokol OTLP - OpenTelemetry).
- **`agent.version`**: `unknown`  
   Versi agen tidak diketahui.

### **3. Informasi Transaksi**
- **`transaction.name`**: `/v1/graphql`  
   Nama endpoint transaksi, dalam hal ini sebuah API endpoint untuk **GraphQL**.
- **`transaction.duration.us`**: `435` µs (microseconds)  
   Waktu yang dibutuhkan untuk menyelesaikan transaksi ini (435 microdetik).
- **`transaction.result`**: `Success`  
   Hasil dari transaksi ini berhasil.
- **`transaction.id`**: `540bd2d697c0aa8a`  
   ID unik dari transaksi ini.
- **`transaction.type`**: `custom`  
   Jenis transaksi yang dilacak (disesuaikan untuk keperluan custom monitoring).
- **`transaction.sampled`**: `true`  
   Menandakan transaksi ini telah diambil sampelnya untuk pemantauan lebih dalam.

### **4. Informasi Trace**
- **`trace.id`**: `fe75ad67ea3f85dd40b48cf819cdbaee`  
   ID unik yang menghubungkan semua log, transaksi, dan jejak (trace) dalam satu siklus.

### **5. Informasi Service**
- **`service.name`**: `hasura`  
   Nama layanan atau aplikasi yang memproses transaksi ini.
- **`service.framework.name`**: `hasura`  
   Framework yang digunakan oleh layanan.
- **`service.framework.version`**: `v2.42.0`  
   Versi dari framework Hasura.

### **6. Informasi Tambahan / Label**
- **`labels.graphql_operation_name`**: `TrigetUser`  
   Nama operasi GraphQL yang dipanggil.
- **`labels.request_id`**: `00866697-d793-42c1-a693-df18a8c34000`  
   ID unik dari request HTTP.
- **`labels.enduser_role`**: `admin`  
   Role pengguna yang memanggil endpoint ini.
- **`labels.http_response_content_length`**: `165`  
   Panjang konten HTTP respons dalam byte.
- **`labels.session_variables`**: `{"x-hasura-role":"admin"}`  
   Variabel sesi yang berisi informasi role pengguna.

### **7. Informasi Event**
- **`event.ingested`**: `2024-12-10T07:27:39.026Z`  
   Waktu ketika event (transaksi) ini diterima oleh APM server.
- **`event.outcome`**: `success`  
   Status transaksi, yaitu sukses.

### **8. Informasi Waktu**
- **`@timestamp`**: `2024-12-10T07:22:49.236Z`  
   Waktu ketika transaksi ini dimulai.
- **`timestamp.us`**: `1733815369236887`  
   Waktu transaksi dalam format **microseconds** sejak epoch time.

## Latency Per Operation Name

Gunakan filter custom KQL untuk menampilkan grafik hanya `metrics` dengan `operation name` tertentu. Dalam hal ini aku mau filter by operation name dengan menuliskan di kolom search custom KQL. Berikut contoh commandnya.
  
```
labels.graphql_operation_name : "getAttendance" 
```
  
Lebih jelasnya perhatikan gambar berikut.
   
![image](https://github.com/user-attachments/assets/0f752873-2719-41f8-afc4-684a61f38328)
  
Setiap `operation name` tentu tidak hanya melakukan sekali `transaction`.Kita dapat melihat latency per transaction atau per hit seperti berikut.

![image](https://github.com/user-attachments/assets/f196641d-e464-4460-8b51-df7dbd430737)

Kita juga bisa melihat average dari keseluruhan hit/transaction dengan operation name yang sama seperti gambar berikut.
  
![image](https://github.com/user-attachments/assets/56b5145a-1362-4e95-9969-8cd1cf9849fc)

  
