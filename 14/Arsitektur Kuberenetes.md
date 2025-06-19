# **Arsitektur Kubernetes dan Cara Kerjanya**

Kubernetes adalah sistem orkestrasi container yang membantu mengelola aplikasi yang berjalan dalam container secara otomatis. Arsitektur Kubernetes terdiri dari beberapa komponen utama yang bekerja sama untuk mengatur, mengontrol, dan menjalankan aplikasi secara efisien.

---

## **1. Arsitektur Kubernetes**

Arsitektur Kubernetes terdiri dari **dua bagian utama**:

1. **Control Plane (Master Node)**
2. **Worker Nodes**

![image](https://github.com/user-attachments/assets/5ae03c58-4ca0-466f-83d9-6368fe1f49ca)

Gambar di atas menunjukkan bagaimana komponen Control Plane dan Worker Nodes bekerja bersama dalam arsitektur Kubernetes. 

- **Control Plane** terdiri dari API Server, Scheduler, Controller Manager, dan Etcd untuk mengelola seluruh klaster.
- **Worker Nodes** bertugas menjalankan container dan memastikan pod bekerja dengan baik, menggunakan komponen seperti Kubelet, Kube Proxy, dan Container Runtime.

### **A. Control Plane (Master Node)**

Control Plane adalah pusat kendali dari klaster Kubernetes. Ia bertanggung jawab untuk mengatur status klaster, menjadwalkan pod, serta mengelola komunikasi antara komponen.

#### **Komponen Control Plane**:

1. **API Server (`kube-apiserver`)**
    - Menyediakan **RESTful API** untuk berkomunikasi dengan komponen internal dan eksternal dalam klaster Kubernetes.
    - Semua perintah dari `kubectl`, Dashboard, atau API eksternal diterima oleh API Server sebelum diteruskan ke komponen lainnya.
    - Berperan sebagai pintu gerbang utama yang menangani permintaan administrasi dan operasional dalam klaster Kubernetes.

2. **Controller Manager (`kube-controller-manager`)**
    - Bertanggung jawab untuk menjalankan berbagai kontrol dalam Kubernetes, termasuk:
        - **Node Controller** → Memantau status node dan mengambil tindakan jika node mengalami kegagalan.
        - **Replication Controller** → Memastikan jumlah replika pod tetap sesuai dengan yang didefinisikan dalam deployment.
        - **Endpoint Controller** → Menghubungkan service dengan pod yang sesuai, memungkinkan komunikasi dalam klaster tetap berjalan dengan lancar.
        - **Service Account & Token Controller** → Mengelola akun layanan dan token autentikasi yang digunakan oleh pod untuk berkomunikasi dengan API Server.

3. **Scheduler (`kube-scheduler`)**
    - Menentukan di **node mana** sebuah pod akan dijalankan, berdasarkan berbagai faktor seperti:
        - Resource yang tersedia (CPU, memori, storage)
        - Affinity dan anti-affinity
        - Node taints dan tolerations
        - Workload prioritas
    - Scheduler memastikan beban kerja terdistribusi secara optimal dalam klaster untuk menghindari **resource contention**.

4. **Etcd** (Key-Value Store)
    - **Menyimpan semua konfigurasi dan status klaster** dalam bentuk key-value store yang terdistribusi.
    - Bertindak sebagai **database pusat** yang menyimpan informasi tentang pod, service, konfigurasi, dan policy Kubernetes.
    - Etcd direplikasi di seluruh control plane untuk memastikan **ketersediaan tinggi (High Availability/HA)** dan **konsistensi data**.

---

### **B. Worker Node**

Worker Nodes menjalankan aplikasi dalam container. Setiap worker node memiliki agen yang memastikan pod berjalan dengan baik.

#### **Komponen Worker Node**:

1. **Kubelet**
    - Agen utama di setiap node yang bertanggung jawab untuk menjalankan pod dan berkomunikasi dengan API Server.
    - Memantau status pod dan memastikan bahwa container berjalan sesuai dengan spesifikasi yang diberikan dalam YAML deployment.
    - Mengunduh dan menjalankan container berdasarkan instruksi dari API Server.

2. **Container Runtime**
    - Bertanggung jawab untuk menjalankan container dalam pod.
    - Contoh container runtime yang dapat digunakan adalah **Docker**, **containerd**, atau **CRI-O**.
    - Mengelola siklus hidup container, termasuk start, stop, dan restart berdasarkan instruksi dari Kubelet.

3. **Kube Proxy**
    - Mengatur jaringan dalam klaster Kubernetes, termasuk routing traffic antar pod dan ke dunia luar.
    - Mengimplementasikan mekanisme load balancing untuk mendistribusikan traffic ke beberapa instance pod dalam satu service.
    - Mengatur aturan firewall dan Network Policy agar pod dapat berkomunikasi dengan aman.
  
### **C. UI dan CLI pada Kubernetes**

#### **UI (User Interface)**

- **UI** pada Kubernetes biasanya merujuk ke antarmuka grafis seperti **Kubernetes Dashboard** atau tools pihak ketiga seperti **Rancher**.  
- Dengan UI, pengguna dapat:
  - Melihat status klaster, pod, deployment, dan service.
  - Melakukan scaling pod atau deployment secara visual.
  - Mengatur resource tanpa perlu menulis command manual.
  - Memantau log dan event dalam klaster dengan mudah.
- Antarmuka ini cocok untuk pengguna yang lebih menyukai pendekatan visual dan interaktif.

#### **CLI (Command Line Interface)**

- **CLI** merujuk pada alat seperti `kubectl`, yang merupakan command-line tool utama untuk berinteraksi dengan klaster Kubernetes.
- Melalui CLI, pengguna dapat menjalankan perintah untuk:
  - Membuat, menghapus, atau memodifikasi resource Kubernetes.
  - Mengecek status pod, node, atau service dalam klaster.
  - Menerapkan file konfigurasi (YAML/JSON) dengan perintah seperti `kubectl apply`.
  - Melakukan debug dengan log dari pod.
- CLI memberikan fleksibilitas dan kontrol penuh bagi pengguna yang lebih nyaman dengan terminal atau ingin mengotomatiskan pekerjaan menggunakan skrip.


---

## **2. Cara Kerja Kubernetes**

### **A. Proses Deploy Aplikasi dalam Kubernetes**

1. **Developer membuat deployment** menggunakan `kubectl apply -f deployment.yaml` atau melalui API.
2. **API Server menerima request** dan menyimpannya di etcd.
3. **Scheduler memilih node** yang cocok untuk menjalankan pod berdasarkan resource yang tersedia.
4. **Kubelet pada worker node mengambil tugas** dari API Server dan meminta container runtime untuk menjalankan pod.
5. **Container Runtime menarik image container** dari registry (misalnya Docker Hub) dan menjalankannya di dalam pod.
6. **Kube Proxy mengatur komunikasi** antara pod dalam satu klaster maupun ke eksternal dengan menerapkan policy yang sesuai.
7. **Controller Manager memastikan pod tetap berjalan** sesuai dengan yang ditentukan dalam deployment. Jika ada pod yang gagal atau mati, sistem akan membuat ulang pod baru.

### **B. Cara Kubernetes Mengatur Traffic dan Load Balancing**

1. **Service Discovery** → Kubernetes menggunakan mekanisme DNS internal untuk menghubungkan pod dengan service yang sesuai.
2. **Load Balancer** → Service dalam Kubernetes dapat dikonfigurasi untuk membagi traffic ke beberapa pod menggunakan mekanisme round-robin atau lainnya.
3. **Ingress Controller** → Untuk traffic dari luar klaster, Kubernetes menggunakan Ingress Controller (misalnya Traefik atau Nginx) untuk menangani traffic masuk berdasarkan aturan yang telah didefinisikan.

---

## **3. Fitur Utama Kubernetes**

- **Self-healing** → Jika pod mati, Kubernetes akan membuat ulang secara otomatis.
- **Scaling otomatis** → Bisa menambah/mengurangi jumlah pod berdasarkan traffic.
- **Load Balancing** → Mengarahkan traffic ke pod yang tersedia.
- **Service Discovery** → Pod dalam satu klaster bisa saling berkomunikasi tanpa konfigurasi manual.
- **Rolling Updates & Rollbacks** → Kubernetes memungkinkan deployment baru dilakukan secara bertahap tanpa downtime, serta dapat kembali ke versi sebelumnya jika terjadi masalah.
- **Resource Quotas** → Administrator dapat menentukan batas penggunaan resource agar workload tidak menghabiskan seluruh kapasitas klaster.

---
---
---

# **Arsitektur Kubernetes dan Implementasi dalam SLA**

## **1. Pendahuluan**
Kubernetes adalah sistem orkestrasi container yang dapat dikonfigurasi untuk memenuhi **Service Level Agreement (SLA)**, memastikan **ketersediaan (availability), keandalan (reliability), dan performa (performance)** layanan sesuai kebutuhan bisnis.

---

## **2. Arsitektur Kubernetes untuk SLA**

### **A. High Availability (HA) Cluster**
Agar tidak ada single point of failure, arsitektur Kubernetes harus mendukung High Availability (HA) dengan:

- **Multiple Control Plane Nodes** → Menggunakan etcd yang direplikasi.
- **Load Balancer untuk API Server** → Menggunakan HAProxy atau Cloud Load Balancer.
- **Multiple Worker Nodes** → Pod dipindahkan ke node lain jika terjadi kegagalan.

📌 **Contoh SLA:**
> *"SLA menjamin 99.9% uptime dengan arsitektur Kubernetes yang memiliki redundansi pada master dan worker node."*

---

### **B. Auto Scaling untuk Menjaga Performa**
Agar sistem tetap optimal dalam menghadapi lonjakan beban:

- **Horizontal Pod Autoscaler (HPA)** → Menambah/mengurangi jumlah pod berdasarkan CPU/memory usage.
- **Cluster Autoscaler** → Menyesuaikan jumlah node secara dinamis.

📌 **Contoh SLA:**
> *"SLA menjamin latensi API di bawah 100ms dengan autoscaling yang aktif berdasarkan beban sistem."*

---

### **C. Multi-Region & Multi-Cluster Deployment**
Untuk mencapai **99.99% availability**, Kubernetes dapat dikonfigurasi dengan:

- **Global Load Balancer** untuk mendistribusikan traffic.
- **Kubernetes Federation** untuk mengelola banyak klaster.
- **Database Replication** untuk menjaga ketersediaan data antar-region.

📌 **Contoh SLA:**
> *"SLA menjamin bahwa jika satu region mengalami kegagalan, sistem tetap berjalan dengan failover otomatis ke region lain dalam waktu kurang dari 30 detik."*

---

## **3. Observability & Monitoring untuk SLA**

### **A. Logging & Monitoring**
- **Prometheus + Grafana** → Memantau metrik sistem.
- **Elasticsearch + Fluentd + Kibana (EFK)** → Menganalisis log aplikasi.

### **B. Distributed Tracing**
- **OpenTelemetry atau Jaeger** → Menelusuri performa API dari ujung ke ujung.

📌 **Contoh SLA:**
> *"SLA menjamin deteksi anomali dalam waktu kurang dari 5 menit menggunakan observability tools."*

---

## **4. Disaster Recovery & Backup**
Jika SLA mencakup pemulihan dari kegagalan, strategi berikut diterapkan:

- **Backup otomatis menggunakan Velero**.
- **Database Replication & PITR (Point-In-Time Recovery)**.
- **Failover Cluster** dengan **RTO (Recovery Time Objective)** yang jelas.

📌 **Contoh SLA:**
> *"SLA menjamin pemulihan data dalam waktu kurang dari 15 menit jika terjadi kehilangan data kritis."*

---

## **5. Kesimpulan**
Untuk memenuhi SLA yang tinggi, Kubernetes harus memiliki:  
✅ **High Availability**  
✅ **Autoscaling**  
✅ **Multi-Region Deployment**  
✅ **Monitoring & Observability**  
✅ **Disaster Recovery**  

📌 **Contoh SLA yang dapat diterapkan:**
- *99.9% uptime*
- *Latency API <100ms*
- *Recovery dalam 15 menit*

