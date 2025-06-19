
# Penjelasan Umum

- **Blackbox** dalam pengujian adalah pendekatan untuk memeriksa sistem tanpa mengetahui detail internalnya. Dalam monitoring, Blackbox digunakan untuk memantau performa layanan dari luar, fokus pada hasil keluaran sistem.
- Di **Grafana**, Blackbox bekerja dengan **Prometheus Blackbox Exporter** untuk melakukan pemeriksaan kesehatan terhadap aplikasi atau layanan, seperti HTTP, TCP, DNS, dan ICMP ping.

# Blackbox

- **Blackbox** adalah komponen penting untuk memantau kesehatan layanan secara eksternal tanpa masuk ke dalam sistem.
- **Blackbox Exporter** adalah Prometheus Exporter yang menjalankan berbagai probe (pengujian) untuk memastikan layanan atau aplikasi berfungsi dengan baik.

Blackbox digunakan untuk memonitor:

- Layanan HTTP/HTTPS
- Layanan TCP
- Layanan DNS
- ICMP ping (memeriksa respons host)

Ini berguna untuk memastikan layanan tersedia dan merespons sesuai dengan harapan dari sudut pandang pengguna.

# Blackbox di Grafana

- **Health Check using Blackbox** di Grafana memantau layanan seperti server atau API dengan cara mensimulasikan permintaan yang dilakukan pengguna nyata. Jika respons sesuai harapan, layanan dianggap sehat.
- Blackbox Exporter melakukan pemeriksaan endpoint seperti GraphQL atau REST API dan hasilnya dikirim ke **Prometheus**, kemudian divisualisasikan di **Grafana**.

# Alur Kerja Blackbox Exporter

![image](https://github.com/user-attachments/assets/d7c33fb4-ba6b-49db-991a-8b0de6f9289b)


1. **Prometheus** mengirim HTTP request ke Blackbox Exporter dengan informasi modul dan target yang diuji (misalnya HTTP, ICMP, DNS).
2. **Blackbox Exporter** menjalankan probe sesuai instruksi dan mengumpulkan data dari target (misalnya kode status HTTP atau waktu respons).
3. Hasil probe dikirim kembali ke Prometheus dalam format metrik.
4. Target yang diuji bisa berupa aplikasi, server, atau layanan yang ingin dipantau kesehatannya.

Kesimpulan: Prometheus menginisiasi pengujian melalui Blackbox Exporter untuk memonitor target, dan data hasil pengujian dikembalikan ke Prometheus untuk divisualisasikan di Grafana.

# Integrasi Blackbox dengan Grafana

Langkah-langkah integrasi:

1. Install **Prometheus Blackbox Exporter**.
2. Konfigurasikan Prometheus untuk Blackbox Exporter.
3. Visualisasikan metrik di Grafana, seperti uptime, latency, atau status HTTP.

Panduan lebih lanjut dapat diikuti di [geekflare.com](https://geekflare.com/monitor-website-with-blackbox-prometheus-grafana/).



Penulis: Arun Lal  
Tanggal: 4 April 2024

# Panduan Langkah Demi Langkah Setup Blackbox Exporter di Kubernetes

Kita akan membahas bagaimana cara melakukan setup Blackbox Exporter di Kubernetes. Dengan mengikuti panduan ini, kita akan dapat memonitor endpoint aplikasi atau server menggunakan Blackbox Exporter, yang merupakan salah satu Prometheus Exporter untuk mengumpulkan metrik probe seperti HTTP, TCP, ICMP, dll.

## Mengapa Blackbox Exporter Penting?

Kubernetes sering kali memerlukan akses ke sumber daya eksternal seperti basis data dan API. Jika ada masalah pada sumber daya tersebut, Kubernetes tidak akan dapat mengaksesnya. Oleh karena itu, kita harus memonitor endpoint sumber daya tersebut agar dapat mengidentifikasi dan memperbaiki masalah lebih awal.

Selain untuk pemecahan masalah, Blackbox Exporter juga membantu meningkatkan performa Kubernetes dengan memonitor waktu request dan response antara sumber daya, sehingga memungkinkan untuk mengidentifikasi masalah latensi dan melakukan penyesuaian untuk meningkatkan performa.

## Langkah-langkah Setup Blackbox Exporter di Kubernetes

Berikut repositori yang berisi file YAML konfigurasi. Silakan gunakan repositori berikut untuk memudahkan setup ini:

[https://github.com/arunlalp/kubernetes-blackbox-exporter.git](https://github.com/arunlalp/kubernetes-blackbox-exporter.git)

Setup ini adalah bagian dari pengaturan monitoring Prometheus di EKS cluster kami. Di set up ini menggunakan namespace `monitoring` yang sudah ada. Jika kita belum memiliki namespace khusus untuk resource monitoring kita, buatlah namespace sebelum memulai setup.

### Langkah 1: Membuat ConfigMap

Buat file `blackbox-configmap.yaml` dan tambahkan konfigurasi berikut:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: blackbox-exporter-config
  namespace: monitoring
data:
  config.yml: |
    modules:
      http_endpoint:
        prober: http
        timeout: 5s
        http:
          valid_http_versions: ["HTTP/1.1", "HTTP/2.0"]
          valid_status_codes: [200, 204]
          no_follow_redirects: false
          preferred_ip_protocol: "ip4"
```

Pada bagian metadata, ganti nama namespace sesuai kebutuhan kita. Pada bagian data, Blackbox Exporter bekerja berdasarkan modul, di sini hanya digunakan satu modul berbasis endpoint HTTP.
  
Terapkan ConfigMap ke cluster dengan `kubectl apply -f blackbox-configmap.yaml`.
  
Jika kita ingin melihat daftar configmaps di monitor namespace. Jalankan perinta `kubectl get deployments -n monitoring -o wide`

### Langkah 2: Membuat Blackbox Deployment

Buat file `blackbox-deployment.yaml` dan tambahkan konfigurasi berikut:

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blackbox-exporter
  namespace: monitoring
spec:
  replicas: 1
  selector:
    matchLabels:
      app: blackbox-exporter
  template:
    metadata:
      labels:
        app: blackbox-exporter
    spec:
      containers:
      - name: blackbox-exporter
        image: prom/blackbox-exporter:latest
        args:
          - "--config.file=/etc/blackbox-exporter/config.yml"
        ports:
        - containerPort: 9115
        volumeMounts:
        - name: config-volume
          mountPath: /etc/blackbox-exporter
      volumes:
      - name: config-volume
        configMap:
          name: blackbox-exporter-config
```
  
Terapkan deployment dengan `kubectl apply -f blackbox-deployment.yaml`.
  
Jangan lupa menambahkan nama namespace di bagian metadata. Pada bagian spec, digunakan gambar terbaru Blackbox `prom/blackbox-exporter:latest`, dan port default Blackbox adalah `9115`.
  
### Langkah 3: Membuat Service untuk Blackbox Exporter

Buat file `blackbox-service.yaml` dan tambahkan konfigurasi berikut:

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: blackbox-exporter
  namespace: monitoring
spec:
  selector:
    app: blackbox-exporter
  type: NodePort
  ports:
    - port: 9115
      targetPort: 9115
      nodePort: 30500
```

Sesuaikan namespace dan perhatikan bagian `spec`. Pada konfigurasi ini menggunakan tipe `NodePort` agar bisa mengakses Blackbox Exporter melalui Internet. Port yang digunakan adalah `30500`. `NodePort` yang di sini adalah `30500`, jika tidak, ia akan memilih beberapa port acak antara kisaran `30000-32767`.

Terapkan service dengan `kubectl apply -f blackbox-service.yaml`.  
  
Untuk melihat daftar layanan di namespace monitoring, gunakan perintah `kubectl get svc -n monitoring -o wide`  

Sekarang, kita dapat mengakses dashboard Blackbox Exporter melalui internet. Untuk itu, kita memerlukan IP publik dari salah satu node dan nomor port 30500.

Output yang akan kita dapatkan adalah sebagai berikut:

![image](https://github.com/user-attachments/assets/898b1bd7-c389-411e-86c5-d0b72be6556d)


### Langkah 4: Menambahkan Job Blackbox di Prometheus

Tambahkan job untuk Blackbox Exporter di dalam file konfigurasi Prometheus.
  
Temukan configmap dengan perintah `kubectl get configmaps -n monitoring`
  
Edit configmap tersebut dengan perintah `kubectl edit configmap prometheus-server-conf -n monitoring`

Tambahkan konfigurasi berikut di bawah bagian `scrape_configs`:
  
```yaml
- job_name: 'blackbox-exporter-metrics'
  metrics_path: /probe
  params:
    module: [http_endpoint]
  static_configs:
    - targets:
      - https://www.google.com
      - devopscube.com
      - https://prometheus.io
  relabel_configs:
    - source_labels: [__address__]
      target_label: __param_target
    - source_labels: [__param_target]
      target_label: instance
    - target_label: __address__
      replacement: "blackbox-exporter.monitoring.svc:9115"
```

Setelah konfigurasi selesai, deploy perubahan ke Prometheus dengan perintah `kubectl apply -f config-map.yaml`
  
Terkadang diperlukan waktu beberapa menit untuk melihat perubahan di dasbor Prometheus.
  
![image](https://github.com/user-attachments/assets/79649059-093d-40a0-88e9-92d1c04a6b41)
  
  
Jika perubahan tidak muncul di dashboard Prometheus, lakukan rollout restart pada deployment Prometheus dengan perintah `kubectl rollout restart deployment prometheus-deployment -n monitoring`
  
### Langkah 5: Membuat Query dari Prometheus

Gunakan metrik `probe_http_status_code` di Prometheus untuk melihat hasil monitoring.

**Prometheus output**

![image](https://github.com/user-attachments/assets/362baa86-6fdf-4fef-b005-c1aad84ff846)
  

### Langkah 6: Visualisasi Metrik Menggunakan Grafana

Gunakan dashboard template di Grafana untuk memvisualisasikan metrik probe dari Blackbox Exporter.

**Grafana output**
  
![image](https://github.com/user-attachments/assets/cb3fd0e1-921a-47cc-a922-c0108b69933a)
