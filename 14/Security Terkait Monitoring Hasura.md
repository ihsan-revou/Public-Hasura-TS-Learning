# Security dalam Monitoring Hasura

Keamanan dalam monitoring Hasura sangat penting untuk memastikan sistem tetap aman dari akses tidak sah, kebocoran data, dan serangan siber.  

Dalam troubleshooting keamanan monitoring Hasura, pendekatan yang disebutkan di atas membantu mendeteksi, mencegah, dan menangani insiden keamanan yang dapat menyebabkan kegagalan sistem atau kebocoran data. Berikut ini adalah bagaimana setiap aspek tersebut berkontribusi dalam troubleshooting:

---

## 1. Firewall
Firewall digunakan untuk menyaring lalu lintas jaringan berdasarkan aturan keamanan.

### Membatasi Akses ke Hasura dan Endpoint Monitoring
- Pastikan hanya **IP tertentu** yang dapat mengakses:
  - Hasura API (`/v1/graphql`)
  - Hasura Console
  - Monitoring endpoints (Prometheus, OpenTelemetry)
- Contoh aturan firewall di **iptables**:
  
  ```bash
  iptables -A INPUT -p tcp --dport 8080 -s <ALLOWED_IP> -j ACCEPT
  iptables -A INPUT -p tcp --dport 8080 -j DROP
  ```

### Mencegah Serangan DDoS atau API Abuse
- Gunakan **rate limiting** dengan firewall WAF seperti:
  - AWS WAF / Cloudflare WAF
  - ModSecurity (Apache/Nginx)

### Proteksi terhadap OpenTelemetry & Prometheus
- Pastikan monitoring endpoints tidak dapat diakses publik:
  ```bash
  iptables -A INPUT -p tcp --dport 4317 -s <INTERNAL_NETWORK> -j ACCEPT
  iptables -A INPUT -p tcp --dport 4317 -j DROP
  ```

### Troubleshooting Use Case:
- Jika Hasura API atau endpoint monitoring tidak bisa diakses, periksa aturan firewall untuk memastikan hanya IP yang diizinkan dapat mengaksesnya.
- Jika ada indikasi serangan DDoS atau API abuse, lihat log firewall untuk memeriksa apakah ada lonjakan trafik yang mencurigakan.
- Jika ada kesalahan koneksi OpenTelemetry atau Prometheus, pastikan aturan firewall mengizinkan komunikasi dengan jaringan internal.

### Troubleshooting Steps:
- Gunakan `iptables -L -v -n` untuk melihat aturan firewall yang aktif.
- Gunakan `netstat -tulnp` untuk memeriksa apakah port yang dibutuhkan dalam keadaan listening.
- Periksa rate limiting pada WAF (misalnya AWS WAF atau Cloudflare WAF) jika ada permintaan yang diblokir secara otomatis.

---

## 2. DNS Security
DNS digunakan untuk mengarahkan request ke Hasura dan layanan terkait.

### Gunakan Private DNS untuk Monitoring
- Hindari **public DNS** untuk endpoint monitoring.
- Contoh setup di **Kubernetes CoreDNS**:
  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: allow-dns
  spec:
    podSelector:
      matchLabels:
        app: hasura
    policyTypes:
    - Egress
    egress:
    - to:
      - namespaceSelector:
          matchLabels:
            name: kube-system
      ports:
      - protocol: UDP
        port: 53
  ```

### Proteksi terhadap DNS Hijacking
- Gunakan **DNSSEC (DNS Security Extensions)**.
- Pastikan domain Hasura tidak menggunakan wildcard DNS.

### Mencegah DNS-based Data Exfiltration
- Blokir **DNS tunneling** dengan firewall.
- Monitor log DNS untuk mendeteksi anomali.

### Troubleshooting Use Case:
- Jika monitoring tidak dapat diakses, cek apakah DNS yang digunakan adalah private DNS.
- Jika terjadi serangan DNS hijacking (misalnya traffic Hasura diarahkan ke domain lain), lakukan DNS lookup (`nslookup` atau `dig`) untuk memastikan domain masih mengarah ke IP yang benar.
- Jika ada potensi exfiltrasi data via DNS tunneling, cek log DNS untuk permintaan yang tidak wajar.

### Troubleshooting Steps:
- Gunakan `nslookup your-domain.com` atau `dig your-domain.com` untuk memeriksa resolusi DNS.
- Periksa apakah DNSSEC diaktifkan dengan `dig DNSKEY your-domain.com`.
- Cek log DNS resolver untuk anomali, misalnya domain yang tidak dikenal mengirim permintaan yang besar dalam waktu singkat.

---

## 3. Network Policy
Dalam Kubernetes, **Network Policy** digunakan untuk mengontrol komunikasi antar **Pods** dan **Namespaces**.

### Membatasi Akses Layanan Hasura
- Pastikan hanya **frontend** atau **API Gateway** yang bisa mengakses Hasura:
  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: hasura-network-policy
  spec:
    podSelector:
      matchLabels:
        app: hasura
    ingress:
    - from:
      - podSelector:
          matchLabels:
            app: frontend
      ports:
      - protocol: TCP
        port: 8080
  ```

### Memblokir Akses ke OpenTelemetry dari Luar
- Pastikan hanya Pods tertentu yang bisa mengakses **OTLP Collector**:
  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: allow-otel-internal
  spec:
    podSelector:
      matchLabels:
        app: otel-collector
    ingress:
    - from:
      - podSelector:
          matchLabels:
            app: hasura
      ports:
      - protocol: TCP
        port: 4317
  ```

### Troubleshooting Use Case:
- Jika Hasura tidak bisa diakses oleh frontend, cek apakah Network Policy di Kubernetes membatasi akses ke pod Hasura.
- Jika OpenTelemetry tidak menerima data dari Hasura, pastikan hanya pod Hasura yang diizinkan untuk mengaksesnya.
- Jika ada potensi akses tidak sah, gunakan `kubectl get networkpolicy` untuk melihat aturan yang diterapkan.

### Troubleshooting Steps:
- Gunakan `kubectl describe networkpolicy hasura-network-policy` untuk melihat detail aturan akses.
- Jalankan `kubectl exec -it <POD> -- nc -zv <DESTINATION_IP> <PORT>` untuk menguji konektivitas antara pod.
- Jika perlu, matikan sementara **Network Policy** untuk menguji apakah konektivitas kembali normal.

---

## 4. Authentication & Authorization pada Monitoring
- **Gunakan Auth untuk Prometheus & OpenTelemetry**
  - Tambahkan **Basic Auth atau OAuth2** di Prometheus.
  - Gunakan **Role-Based Access Control (RBAC)** di Grafana.
- **Gunakan API Gateway (Traefik/Kong/Nginx) sebagai Reverse Proxy**
  - Pastikan Hasura dan monitoring services hanya bisa diakses via **API Gateway**.
  - Gunakan **JWT atau API Keys** untuk keamanan.
 
### Troubleshooting Use Case:
- Jika login ke **Prometheus** atau **Grafana** gagal, cek apakah Basic Auth atau OAuth2 dikonfigurasi dengan benar.
- Jika Hasura tidak bisa diakses via API Gateway (Traefik/Kong/Nginx), pastikan API key atau JWT yang digunakan valid.
- Jika ada indikasi akses tidak sah, cek log autentikasi untuk melihat siapa yang mencoba masuk.

### Troubleshooting Steps:
- Gunakan `kubectl logs <POD>` untuk melihat log autentikasi di Grafana/Prometheus.
- Periksa konfigurasi API Gateway dengan `kubectl get ingress -n kube-system`.
- Coba akses Hasura dengan token yang valid menggunakan `curl -H "Authorization: Bearer <TOKEN>"`.

---

## 5. Audit Logs & Logging Security
- **Aktifkan Query Logs di Hasura** untuk mendeteksi anomali.
- **Integrasi dengan SIEM** (Splunk, ELK, Datadog) untuk analisis keamanan.
- **Gunakan OpenTelemetry untuk mendeteksi serangan API**.

### Troubleshooting Use Case:
- Jika ada indikasi penyalahgunaan API, analisis **Hasura Query Logs** untuk melihat permintaan yang mencurigakan.
- Jika terjadi kebocoran data, periksa log dari SIEM (Splunk, ELK, Datadog) untuk melihat aktivitas pengguna.
Jika OpenTelemetry tidak bisa mengekspor data, cek log collector untuk melihat apakah ada kesalahan autentikasi atau koneksi.

### Troubleshooting Steps:
- Gunakan `hasura metadata export` untuk mengekstrak log metadata.
- Periksa log API dengan `kubectl logs -f <HASURA_POD>`.
- Jika menggunakan OpenTelemetry, lihat log dengan `kubectl logs -f <OTEL_POD>`.

## 6. Proteksi terhadap Serangan API & Query Overload
- Gunakan **Rate Limiting & Query Timeouts** untuk mencegah **GraphQL Abuse**.
- **Hardened GraphQL API Security** dengan introspection **disabled** di production.

### Troubleshooting Use Case:
- Jika Hasura mengalami **high CPU usage**, periksa apakah ada query berat atau serangan **GraphQL abuse**.
- Jika Hasura tiba-tiba lambat, cek apakah **introspection query** diaktifkan di production.
- Jika ada indikasi API abuse, gunakan **rate limiting** untuk membatasi jumlah request.
  
### Troubleshooting Steps:
- Gunakan `hasura console` untuk melihat query yang berjalan lama.
- Aktifkan **query timeout** dan cek performa dengan EXPLAIN ANALYZE.
- Gunakan API Gateway untuk menerapkan rate limiting, misalnya di Kong:
  
```yaml
Salin
Edit
plugins:
- name: rate-limiting
  config:
    minute: 100
    policy: local
```

---

## 7. Alerting & Incident Response
- **Gunakan Prometheus AlertManager** untuk memantau **anomali pada request atau query execution time**.
- **Buat playbook incident response** untuk troubleshooting security.

### Troubleshooting Use Case:
- Jika ada **serangan API**, pastikan **Prometheus AlertManager** dikonfigurasi untuk mengirim notifikasi.
- Jika query execution time meningkat drastis, gunakan alerting untuk mendeteksi anomali.
- Jika terjadi kebocoran data, buat **incident response playbook** untuk menangani insiden dengan cepat.

### Troubleshooting Steps:
- Cek apakah alert di **Prometheus AlertManager** berfungsi dengan `curl http://alertmanager:9093/api/v1/alerts`.
- Periksa notifikasi di Slack/Email untuk setiap alert yang dikonfigurasi.
- Gunakan `kubectl describe pod <HASURA_POD>` untuk melihat resource yang digunakan dan kemungkinan bottleneck.

---

## Glossary

| Security Layer | Fungsi |
|---------------|--------|
| **Firewall** | Membatasi akses ke Hasura & monitoring services |
| **DNS Security** | Mencegah DNS hijacking, spoofing, dan data exfiltration |
| **Network Policy** | Mengontrol komunikasi antar layanan di Kubernetes |
| **Authentication & Authorization** | Membatasi akses ke Prometheus, OpenTelemetry, dan Hasura |
| **Audit Logging & SIEM** | Menganalisis anomali & deteksi serangan |
| **Rate Limiting & Query Timeout** | Mencegah API abuse & serangan DDoS |
| **Alerting & Incident Response** | Memantau & merespons anomali dengan cepat |

