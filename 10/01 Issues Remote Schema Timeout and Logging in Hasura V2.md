
# Setup Hasura V2 di Kubernetes dengan Integrasi OpenTelemetry

## Deployment Hasura V2 di Kubernetes  

Langkah-langkah deployment Hasura V2 telah didokumentasikan pada BAB sebelumnya. Dokumentasi dapat dilihat [disini](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning-Public/blob/main/03/01%20Preparation%20VPN%2C%20Akses%20Rancher%2C%20dan%20Deployment%20Hasura.md#3-deployment-hasura-di-kubernetes).

---

## Install OpenTelemetry Collector  

### Akses Server

Kita akan menginstall **OpenTelemetry** pada server yang berbeda dengan server Hasura yang berjalan. Terlebih dahulu Login ke server yang akan dipakai menggunakan Windows Powershell atau Command Prompt.

### Install Docker

Kita akan menginstall **OpenTelemetry** menggunakan **Docker**. Pastikan **Docker** telah terinstall pada server yang akan digunakan untuk menginstall **OpenTelemetry**. Jika belum, berikut langkah-langkah untuk menginstall **Docker**.

#### Prasyarat
Pastikan Anda menggunakan sistem operasi Ubuntu atau turunannya. Panduan ini juga berlaku untuk distribusi seperti Linux Mint, tetapi Anda mungkin perlu menggunakan `UBUNTU_CODENAME` alih-alih `VERSION_CODENAME`.

---

#### Langkah-Langkah Instalasi Docker Engine

##### 1. Tambahkan Repository Apt Docker

1. **Perbarui Daftar Paket dan Instal Dependensi:**
   ```bash
   sudo apt-get update
   sudo apt-get install ca-certificates curl
   ```

2. **Tambahkan GPG Key Resmi Docker:**
   ```bash
   sudo install -m 0755 -d /etc/apt/keyrings
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   ```

3. **Tambahkan Repository Docker ke Sumber Apt:**
   ```bash
   echo      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" |      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

4. **Perbarui Kembali Daftar Paket:**
   ```bash
   sudo apt-get update
   ```

##### 2. Instal Paket Docker

1. **Instal Versi Terbaru Docker Engine:**
   ```bash
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

2. **Verifikasi Instalasi dengan Menjalankan Container `hello-world`:**
   ```bash
   sudo docker run hello-world
   ```

   Perintah ini akan mengunduh image uji coba, menjalankannya di container, dan mencetak pesan konfirmasi jika berhasil.

---

##### Catatan Tambahan
- Jika Anda menggunakan distribusi turunan Ubuntu seperti Linux Mint, pastikan Anda menggunakan `UBUNTU_CODENAME` alih-alih `VERSION_CODENAME` saat menambahkan repository Docker ke sumber Apt.

---

##$$$ Verifikasi
Jika instalasi berhasil, Anda akan melihat pesan dari container `hello-world` yang menyatakan bahwa Docker Engine telah terinstal dan berjalan dengan baik.

---
  
Baca referensi selengkapnya [disini](https://docs.docker.com/engine/install/ubuntu/).

### Install OpenTelemetry Menggunakan Docker

1. **Buat folder OTEL**

Kita akan menginstall **OTEL** pada folder **OTEL**. Sehingga, terlebih dahulu kita membuat folder OTEL.
  
```
mkdir OTEL
```

2. **Masuk ke dalam folder OTEL**

```
cd OTEL
``` 

3. **Buat file otel-collector-config.yaml**
  
Kita akan membuat konfigurasi **OTEL** secara custom sehingga terlebih dahulu membuat file **otel-collector-config.yaml**
  
```
touch otel-collector-config.yaml
```

4. **Kemudian kita edit file "otel-collector-config.yaml" untuk mengatur konfigurasi otel**
  
```
nano otel-collector-config.yaml
```

![image](https://github.com/user-attachments/assets/cfb30143-47b6-4984-8d5a-f0736b1193db)


  
Kemudian kita masukkan File Configurasi berikut ini :
```
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318
processors:
  batch:

exporters:
  debug: {}
  verbosity: detailed

extensions:
  health_check:
  pprof:
    endpoint: 0.0.0.0:1777
  zpages:
    endpoint: 0.0.0.0:55679

service:
  pipelines:
    traces:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp]
    metrics:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp]
    logs:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp]
```

![image](https://github.com/user-attachments/assets/f4daddcc-67e0-4eab-bca8-5824398afec1)

  
5. **Kemudian Save and Exit.**

Kita ketik `ctrl + x` untuk Exit. Kemudian kita pilih "yes" untuk Save.

6. **Kemudian tampilkan daftar file pada folder OTEL**

Cek daftar file secara detail dengan log level untuk melihat apakah config otel sudah dibuat dengan baik.
   
```
ls -ll
```

![image](https://github.com/user-attachments/assets/7460a383-d9dc-4d87-a088-36c383c69ae3)

  
6. **Kemudian kita jalankan Command berikut**

Jalankan command berikut menarik Docker Image dan menjalankan Kolektor dalam sebuah kontainer.

```
docker pull otel/opentelemetry-collector-contrib:0.114.0
```

Untuk memuat file konfigurasi khusus dari direktori kerja kita, pasang file tersebut sebagai volume dengan jalankan command berikut.
   
```
docker run -d -v $(pwd)/otel-collector-config.yaml:/root/OTEL/otel-collector-config.yaml otel/opentelemetry-collector-contrib:0.114.0
```

Baca referensi selengkapnya disini:  
[Install The collector](https://opentelemetry.io/docs/collector/installation/)   
[Collector Configuration](https://opentelemetry.io/docs/collector/configuration/)  
  
  
### Install "OpenTelemetry Collector" (otelcol) Menggunakan Systemd
  
```
which otelcol
```
  
Jika balasan kosong maka tidak terdapat otelcol atau otelcol tidak terinstall. Selanjutnya, install otelcol dengan mengikuti langkah-langkah berikut:

1. **Unduh Paket OpenTelemetry Collector**
Jalankan perintah berikut untuk mengunduh file `.deb` dari OpenTelemetry Collector:

```bash
curl -sL https://github.com/open-telemetry/opentelemetry-collector-releases/releases/download/v0.81.0/otelcol_0.81.0_linux_amd64.deb -o otelcol.deb
```

2. **Install Paket**
Gunakan `dpkg` untuk menginstal paket yang telah diunduh:

```bash
sudo dpkg -i otelcol.deb
```

3. **Buat File Service untuk Systemd**
Edit file service menggunakan perintah berikut:

```bash
sudo nano /etc/systemd/system/otelcol.service
```

Lalu masukkan konfigurasi berikut:

```
[Unit]
Description=OpenTelemetry Collector
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/otelcol --config=/etc/otelcol-config.yaml
Restart=always
RestartSec=10
User=otel
Group=otel

[Install]
WantedBy=multi-user.target
```

4. **Reload dan Aktifkan Service**
Reload konfigurasi systemd dan aktifkan layanan OpenTelemetry Collector:

```bash
sudo systemctl daemon-reload
sudo systemctl enable otelcol
sudo systemctl start otelcol
```

5. **Melihat Log Service**
Untuk melihat log OpenTelemetry Collector secara real-time, jalankan perintah:

```bash
journalctl -u otelcol -f
```

---

## Hubungkan Otel dengan Hasura pada Rancher.
Hubungkan otel yang telah kita buat sebelumnya dengan Hasura pada Rancher menggunakan powershell atau command prompt. Ikuti langkah berikut untuk menghubungkan hasura dengan otel:

1. **Buka Hasura pada Rancher.**  

2. **Pilih Setting.** <br/>
![image](https://github.com/user-attachments/assets/1788e698-c26d-478e-9d63-415757cffa5b)
     <br/>
![image](https://github.com/user-attachments/assets/a674ce8f-83a7-47af-8db7-609281c63075)
<br/><br/>

2. **Pilih "Open Telemetry Exporter"** <br/>
![image](https://github.com/user-attachments/assets/9f3fb652-9485-46ed-979e-817096a4e7e0)
    <br/>
![image](https://github.com/user-attachments/assets/0aaa9eb6-72e6-4dcf-99a0-dbcc5871c0ee)
    <br/><br/>

3. **Masukkan Endpoint pada Hasura di Rancher** <br/>
-> disini kita masukkan Endpoint "4318". <br/>
yang mana "4318" merupakan Port http pada Configuration File <br/> <br/>

![image](https://github.com/user-attachments/assets/92b8fb41-556c-4f49-9449-cee1aab4e4ad)
    <br/>
![image](https://github.com/user-attachments/assets/74d46ba8-d1a1-45b9-a787-338f41cebfd3)
      <br/>

## Lakukan Hit Query Hasura dan Periksa Log Otel.

### 1. Buka Hasura console dan jalankan query.

![image](https://github.com/user-attachments/assets/16e6d404-1ed2-45d8-9ce7-49f1fa7f2fed)

  
-> Query yang di-Hit tersebut mempunyai :
- nama Tabel "todo"
- dan salah satu Nama Kolom nya adalah "name" yang mempunyai nilai "ferdy"
 Kemudian kita Filter Log, dengan hanya menampilkan Log yang memiliki keyword tertentu

### 2. Kita ingin menampilkan Log yang memiliki Keyword "Nama Tabel", yaitu `todo`

```
journalctl -u otelcol --since "1 hour ago" | grep "todo"
```
![image](https://github.com/user-attachments/assets/1e4e5975-7d0d-4c5a-8c33-b68ff7d5a021)
  
Pada gambar diatas, dapat kita lihat, saat kita ingin menampilkan Log yang memiliki Keyword "Nama Tabel", 
"Log tersebut ada"

### 3. Kita ingin menampilkan Log yang memiliki Keyword "Nama Kolom", yaitu yang mempunyai nilai "ferdy" (ini value untuk kolom "name" )
  
```
journalctl -u otelcol --since "1 hour ago" | grep "ferdy"
```
  
![image](https://github.com/user-attachments/assets/6d07ff7f-0243-4a8c-ac59-308e93deafc5)
  
Pada gambar diatas, dapat kita lihat, saat kita ingin menampilkan Log yang memiliki Keyword "Nama Kolom", 
"Log tersebut tidak ada".

### Kesimpulan : 

Pada gambar diatas, dapat kita lihat, saat kita ingin menampilkan Log yang memiliki Keyword "Nama Tabel",  <br/>
"Log tersebut ada".
<br/> <br/>
tetapi Pada gambar diatas, saat kita ingin menampilkan Log yang memiliki Keyword "Nama Kolom",  <br/>
"Log tersebut tidak ada" .
<br/><br/>


Dapat kita buat dugaan, bahwa Log tersebut tidak menampilkan Secara Detail, melainkan hanya menampilkan secara garis besar.
yaitu saat Kita ingin mem-filter Log dengan "Nama Tabel", Log tersebut muncul. <br/>
Tetapi jika ingin meanampilkan Log secara lebih detail lagi, yaitu dengan mem-filter Log dengan "Nama Kolom", Log tersebut tidak muncul. <br/><br/>

-----
# Catatan Kaki: <br/>
<br/>
-> Catatan Kaki ini berisi Catatan mengenai Error yang terjadi selama kita mengerjakkan tugas ini, dan bagaimana Proses kita dalam Solve Error tersebut.
<br/>
Berikut ini penjelasannya : 

##  1) Error Command :
### Solusi : kita perbaiki Path pada Command tersebut
-> Berikut ini adalah Command yang benar setelah diperbaiki :
```
docker run -d -v $(pwd)/otel-collector-config.yaml:/root/OTEL/otel-collector-config.yaml otel/opentelemetry-collector-contrib:0.114.0
```

<br/> <br/>

-> Command tersebut, Original nya sebelum dimodifikasi, kita ambil dari File Dokumentasi,
yaitu pada Link berikut ini : <br/>
https://opentelemetry.io/docs/collector/installation/

```
docker run -v $(pwd)/config.yaml:/etc/otelcol-contrib/config.yaml otel/opentelemetry-collector-contrib:0.114.0
```

<img width="542" alt="image" src="https://github.com/user-attachments/assets/1f70a260-704a-465a-9dde-a200b2a4619d">


Pada original Command tersebut, Path nya berbeda dengan Path milik File Configurasi yaml Open Telemetry kita.
<br/><br/>

Kita menggunakan Command berikut ini untuk menge-cek Lokasi Path milik File Configurasi kita :
```
ls $(pwd)/otel-collector-config.yaml
```
[-] Nama File Konfigurasi kita adalah : `otel-collector-config.yaml`   <br/>
[-] `ls $(pwd)/`  =  merupakan Command untuk menge-cek Path untuk Lokasi File kita   <br/> <br/>
Hasil dari menjalankan Command tersebut adalah `/root/OTEL/otel-collector-config.yaml`


-> Sehingga Command tersebut kita perbaiki menjadi :
```
docker run -d -v $(pwd)/otel-collector-config.yaml:/root/OTEL/otel-collector-config.yaml otel/opentelemetry-collector-contrib:0.114.0
```
Perubahan yang kita buat pada Command tersebut adalah : <br/>
[-] Nama File Konfigurasi : 
- Nama File Konfigurasi yang kita pakai adalah : `otel-collector-config.yaml`
- Sedangkan Nama File Konfigurasi pada Link Dokumentasi adalah : `config.yaml`

[-] Path untuk Lokasi File Konfigurasi :
- Path untuk Lokasi File Konfigurasi yang kita pakai adalah : `/root/OTEL/otel-collector-config.yaml`
- Sedangkan Path untuk Lokasi File Konfigurasi pada Link Dokumentasi adalah : `/etc/otelcol-contrib/config.yaml`

<br/><br/>


## 2) Error "File Configuration Yaml" untuk Open Telemetry :
### Solusi : Kita gunakan "File Configuration Yaml untuk Open Telemetry" yang ada di Server Mas Fahryan

Ada beberapa Fase dalam memperbaiki Error "File Configuration Yaml" untuk Open Telemetry, yaitu :

#### Fase 1) Mengambil File Configuration yaml dari "Link Dokumentasi" dan digabungkan dengan yang dari "PDF"

-> File Configuration yang ada pada Link Dokumentasi adalah :
(Link Dokumentasi : https://opentelemetry.io/docs/collector/configuration/ )
```
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318
processors:
  batch:

exporters:
  otlp:
    endpoint: otelcol:4317

extensions:
  health_check:
  pprof:
  zpages:

service:
  extensions: [health_check, pprof, zpages]
  pipelines:
    traces:
      receivers: [otlp]
      processors: [batch]
      exporters: [otlp]
    metrics:
      receivers: [otlp]
      processors: [batch]
      exporters: [otlp]
    logs:
      receivers: [otlp]
      processors: [batch]
      exporters: [otlp]

```

-> Pada PDF, hanya terdapat File Configuration untuk Service, yaitu :
```
service: 
 pipelines: 
 traces: 
 receivers: [otlp] 
 processors: [batch] 
 exporters: [debug, otlp/elastic] 
 metrics: 
 receivers: [otlp] 
 processors: [batch] 
 exporters: [debug, otlp/elastic] 
 logs: 
 receivers: [otlp] 
 processors: [batch] 
 exporters: [debug, otlp/elastic]
```

-> Kemudian kita gabungkan, File Configuration yaml dari Link Dokumentasi & PDF.  <br/>
yang dari PDF hanya untuk bagian Services, yang lainnya menggunakan File Configuration dari Link Dokumentasi. <br/>
Sehingga File Configuration kita adalah :
```
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318
processors:
  batch:

exporters:
  otlp:
    endpoint: otelcol:4317

extensions:
  health_check:
  pprof:
  zpages:

service:
  pipelines:
    traces:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp/elastic]
    metrics:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp/elastic]
    logs:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp/elastic]
```

tetapi File Configuration tersebut masih error <br/><br/>


#### Fase 2) Mengganti Port 9200 pada Endpoint
#### (9200 merupakan Port Elastic Search )

Berdasarkan Error pada Fase 2, ChatGPT menyarankan menggunakan Port 9200 untuk Endpoint. <br/>
Port 9200 merupakan Port untuk 'Elastic Search' ) <br/><br/>

Sepertinya ChatGPT menyarankan kita untuk memakai Port ElasticSearch karena pada bagian Service di File Configuration PDF terdapat tulisan `otlp/elastic`.
<br/><br/>
Sehingga File Configuration kita menjadi :
```
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318
processors:
  batch:

exporters:
  debug: {}
  otlp:
    endpoint: "http://localhost:9200"

extensions:
  health_check:
  pprof:
  zpages:

service:
  pipelines:
    traces:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp]
    metrics:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp]
    logs:
receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp]
```

#### Fase 3) Port yang ada pada File Configuration, tidak ada yang aktif melakukan "LISTENING" pada Server
#### (Port 4317 , 4318 , 9200 tidak ada yang aktif melakukan "LISTENING" )
```
sudo netstat -tuln
```
kita Check menggunakan Command diatas untuk mengecek Port apa saja yang aktif melakukan "LISTENING" pada Server  <br/>
![image](https://github.com/user-attachments/assets/9f83b6f9-a9a3-4770-84b3-e14a328b313d)
![image](https://github.com/user-attachments/assets/b265819c-fc2c-45d6-b501-3bdaad65b6ee)


tetapi Port yang ada pada File Configuration, tidak ada yang aktif melakukan "LISTENING" pada Server, <br/>
yakni Port 4317 , 4318 , 9200 tidak ada yang aktif melakukan "LISTENING" pada Server, <br/>
yang mana artinya Server tidak melakukan Koneksi dengan Port 4317 , 4318 , 9200 . <br/>


<br/><br/>
Kemudian kita juga melakukan Pengecekkan Telnet, tetapi Connection Refused
![image](https://github.com/user-attachments/assets/4a54f56c-32ca-4e8b-8c0f-00645d6b16ae)


#### Fase 4) Kita menggunakan "File Configuration Open Telemetry" yang ada di Server milik Mas Fahryan

Kita Copy File Configuration yang ada di Server Mas Fahryan, kemudian kita melakukan Adjustment (Penyesuaian) sebelum kita taruh File Configuration tersebut di Server kita.

-> File Configuration milik Mas Fahryan - Versi Original (sebelum dimodifikasi) :
```
extensions:
  health_check:
  pprof:
    endpoint: 0.0.0.0:1777
  zpages:
    endpoint: 0.0.0.0:55679

receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318

  opencensus:
    endpoint: 0.0.0.0:55678

  # Collect own metrics
  prometheus:
    config:
      scrape_configs:
      - job_name: 'otel-collector'
        scrape_interval: 10s
        static_configs:
        - targets: ['0.0.0.0:8888']

  jaeger:
    protocols:
      grpc:
        endpoint: 0.0.0.0:14250
      thrift_binary:
        endpoint: 0.0.0.0:6832
      thrift_compact:
        endpoint: 0.0.0.0:6831
      thrift_http:
        endpoint: 0.0.0.0:14268

  zipkin:
    endpoint: 0.0.0.0:9411

processors:
  batch:

exporters:
  debug:
    verbosity: detailed

service:

  pipelines:

    traces:
      receivers: [otlp, opencensus, jaeger, zipkin]
      processors: [batch]
      exporters: [debug]

    metrics:
      receivers: [otlp, opencensus, prometheus]
      processors: [batch]
      exporters: [debug]

    logs:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug]

  extensions: [health_check, pprof, zpages]

```


-> Kemudian kita melakukan Adjustment pada File Configuration Mas Fahryan,
yakni dengan menghilangkan bagian yang tidak perlu, yaitu  opencensus ,  prometheus , jaeger , zipkin.
Berikut ini File Configuration setelah di-adjust :

```
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318
processors:
  batch:

exporters:
  debug: {}
  verbosity: detailed

extensions:
  health_check:
  pprof:
    endpoint: 0.0.0.0:1777
  zpages:
    endpoint: 0.0.0.0:55679

service:
  pipelines:
    traces:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp]
    metrics:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp]
 logs:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp]
```


Setelah menambahkan File Configuration tersebut, kemudian kita jalankan Command Open Telemetry selanjutnya, yaitu :
```
docker run -d -v $(pwd)/otel-collector-config.yaml:/root/OTEL/otel-collector-config.yaml otel/opentelemetry-collector-contrib:0.114.0
```
Dan berhasil menjalankan Command tersebut


## 3) Error OTECOL (OpenTelemetry Collector) belum ter-install
### Solusi : Install OTECOL

(1) Kita ingin Check apakah OTEL (Open Telemetry) sudah ter-install pada Server,
yakni dengan menggunakan Command :
```
sudo systemctl status otelcol

```
tetapi terdapat Error yaitu :
![image](https://github.com/user-attachments/assets/54a4af89-e14e-46db-b00f-e79147b23180)
Error tersebut dikarenakan kita belum meng-install OTECOL pada Sever
<br/><br/>
Sehingga untuk Solve error tersebut, kita perlu Install OTECOL. <br/>
Langkah-langkah untuk meng-install OTECOL sudah dijelaskan pada Penjelasan sebelumnya.  <br/>
