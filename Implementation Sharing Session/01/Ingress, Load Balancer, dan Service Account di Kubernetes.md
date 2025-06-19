# Penjelasan Ingress, LoadBalancer, dan Service Account di Kubernetes

## 1. Ingress

Ingress adalah komponen dalam Kubernetes yang mengatur akses masuk ke dalam cluster dari luar (external access), biasanya melalui HTTP dan HTTPS. Ingress memungkinkan Anda mendefinisikan aturan routing untuk mengarahkan trafik ke service tertentu di dalam cluster berdasarkan URL path atau host.

* **Fungsi:** Mengatur *routing rules* (aturan pengarah lalu lintas) untuk trafik HTTP/HTTPS dari luar menuju ke dalam layanan (Service) di cluster Kubernetes.
* Pada gambar:
  * Ingress menerima trafik dari **Ingress-managed load balancer**.
  * Berdasarkan *host* dan *path* (contoh: `example.com/bar`, `foo.example.com`), Ingress meneruskan trafik ke **Service** yang sesuai.
* **Manfaat utama:** Menyediakan *single entry point* untuk banyak layanan (mirip reverse proxy), serta mendukung SSL termination dan rewrite rules.

### Fitur utama:

* Routing berbasis URL dan host
* Mendukung TLS (HTTPS)
* Load balancing HTTP
* Terminasi SSL

### Contoh sederhana konfigurasi Ingress:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: example-service
            port:
              number: 80
```

## 2. LoadBalancer

LoadBalancer adalah salah satu jenis Service di Kubernetes yang digunakan untuk mengekspos aplikasi ke internet. Ketika Service bertipe LoadBalancer dibuat, cloud provider akan secara otomatis membuat load balancer eksternal (misalnya AWS ELB atau GCP Load Balancer).

* **Fungsi:** Menyediakan satu IP publik/eksternal yang bisa diakses client dari luar cluster, dan mendistribusikan trafik masuk ke backend Ingress controller di dalam cluster.
* Dalam konteks Kubernetes, **Ingress-managed Load Balancer** biasanya otomatis dibuat oleh cloud provider (seperti AWS ELB, GCP LB, atau Azure LB) ketika kamu menggunakan jenis Service `LoadBalancer`.
* Pada gambar:
  * Load balancer berada di depan Ingress dan meneruskan semua permintaan dari client ke Ingress controller.

### Ciri khas:

* Mengarahkan trafik dari IP publik ke service di dalam cluster
* Umumnya digunakan untuk aplikasi yang harus diakses dari luar
* Membutuhkan dukungan dari cloud provider

### Contoh Service LoadBalancer:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
  - port: 80
    targetPort: 8080
```

## 3. Service Account

* **Fungsi Service:** Mengabstraksi akses ke sekumpulan **Pod**. Service memastikan permintaan diteruskan ke Pod yang benar dan bisa melakukan *load balancing* di antara Pod yang sama.
* Pada gambar:
  * Setiap domain/path yang diarahkan oleh Ingress dikaitkan dengan satu **Service**.
  * Service tersebut lalu meneruskan trafik ke Pod-Pod terkait yang menjalankan aplikasinya.

* **Service Account** di Kubernetes digunakan untuk mengontrol hak akses (RBAC) aplikasi (Pod) saat berinteraksi dengan API Kubernetes (misal: akses configmap, secret, dsb).

Service Account di Kubernetes digunakan untuk memberikan identitas kepada pod agar dapat mengakses resource API Kubernetes. Setiap pod dijalankan dengan service account tertentu, yang digunakan untuk otentikasi ke Kubernetes API Server.

### Fungsi utama:

* Otentikasi internal antar komponen
* Memberikan akses terbatas berdasarkan RBAC (Role-Based Access Control)
* Default service account otomatis dibuat di setiap namespace

### Contoh pembuatan Service Account:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-service-account
```

### Contoh penggunaan pada Pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  serviceAccountName: my-service-account
  containers:
  - name: my-container
    image: my-image
```

---

Catatan ini memberikan gambaran dasar tentang bagaimana komponen-komponen ini bekerja di dalam ekosistem Kubernetes dan perannya dalam mengelola trafik dan keamanan.
