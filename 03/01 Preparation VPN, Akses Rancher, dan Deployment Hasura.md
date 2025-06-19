# Panduan Koneksi VPN, Akses Rancher, dan Deployment Hasura

## 1. Koneksi ke VPN
Langkah pertama adalah menghubungkan ke VPN menggunakan Pritunl. Dokumentasi untuk penggunaan Pritunl dapat dilihat di link berikut:  
[Dokumentasi Pritunl](https://docs.google.com/document/d/12sWyat7xh3zwIUXlOqk2NLGKayTl3mjG/edit?usp=sharing&ouid=103129292174620109877&rtpof=true&sd=true)

## 2. Akses Rancher Kantor
Setelah terhubung ke VPN, akses Rancher yang telah dibuat sebelumnya.

## 3. Deployment Hasura di Kubernetes
Setelah masuk ke Rancher, akses Kubernetes dan masuk ke namespace masing-masing. Lakukan deployment Hasura dengan konfigurasi YAML yang bisa diunduh dari:  
[Deployment YAML Hasura](https://raw.githubusercontent.com/hasura/graphql-engine/stable/install-manifests/kubernetes/deployment.yaml)

Berikut adalah contoh file YAML untuk deployment Hasura:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: hasura
    hasuraService: custom
  name: hasura
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hasura
  template:
    metadata:
      labels:
        app: hasura
    spec:
      containers:
      - image: hasura/graphql-engine:v2.42.0
        imagePullPolicy: IfNotPresent
        name: hasura
        env:
        - name: HASURA_GRAPHQL_DATABASE_URL
          value: postgres://username:password@hostname:port/dbname
        - name: HASURA_GRAPHQL_ENABLE_CONSOLE
          value: "true"
        - name: HASURA_GRAPHQL_DEV_MODE
          value: "true"
        ports:
        - name: http
          containerPort: 8080
          protocol: TCP
        livenessProbe:
          httpGet:
            path: /healthz
            port: http
        readinessProbe:
          httpGet:
            path: /healthz
            port: http
        resources: {}
```
Sesuaikan **namespace**, **name**, **app**, **username**, **password**, **hostname**, **port**, dan **database name** di file YAML deployment dengan informasi kalian.  
Jalankan perintah kubectl apply untuk melakukan proses deployment.  

## 4. Membuat Service untuk Hasura  
Gunakan file YAML berikut untuk membuat service Hasura, yang bisa diakses dari:
[Service YAML Hasura](https://raw.githubusercontent.com/hasura/graphql-engine/stable/install-manifests/kubernetes/svc.yaml)  

Contoh isi file YAML untuk service Hasura adalah:

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: hasura
  name: hasura
  namespace: default
spec:
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
  selector:
    app: hasura
  type: LoadBalancer
```

Sesuaikan **app**, **name**, **namespace**, dan **port** untuk menghindari konflik dengan port lain.  

## 5. Mengaktifkan Hasura Enterprise  
Secara default, Hasura bukan versi enterprise sehingga tidak dapat terhubung ke database. Untuk mengatasinya, tambahkan inject key sebagai berikut:  
  
```yaml
- name: HASURA_GRAPHQL_EE_LICENSE_KEY
  valueFrom:
    secretKeyRef:
      key: HASURA_GRAPHQL_EE_LICENSE_KEY
      name: hasura-code-inject
```
  
Untuk keamanan tambahan, tambahkan secret key untuk admin dan key inject sebagai berikut:  

```yaml
- name: HASURA_GRAPHQL_ADMIN_SECRET
  valueFrom:
    secretKeyRef:
      key: admin-secret
      name: hasura-secret
```
  
## 6. Deploy Hasura Data Connector Agent  
Untuk menghubungkan Hasura dengan MySQL, buat GraphQL Data Connector Agent dengan file YAML berikut:  

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hasura-graphql-data-connector
  namespace: default
  labels:
    app: hasura-graphql-data-connector
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hasura-graphql-data-connector
  template:
    metadata:
      labels:
        app: hasura-graphql-data-connector
    spec:
      containers:
        - image: hasura/graphql-data-connector:v2.43.0
          name: hasura-graphql-data-connector
          ports:
            - containerPort: 8081
              protocol: TCP
```
  
Kemudian buat service untuk agent ini dengan YAML berikut:  

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hasura-graphql-data-connector
  namespace: default
spec:
  type: LoadBalancer
  selector:
    app: hasura-graphql-data-connector
  ports:
    - port: 8991
      targetPort: 8081
      protocol: TCP
```  
  
Sesuaikan port dan pastikan tidak bentrok dengan service lain.  

## 7. Menghubungkan Hasura ke Connector Data Agent.  
Untuk menghubungkan Hasura ke MySQL, gunakan connector data agent yang telah di deploy sebelumnya. Berikut adalah contoh URL yang kupakai untuk koneksi ke connector data agent:

```bash
http://10.100.14.9:8991/
```
  
## 8. Menghubungkan Hasura ke MySQL  
Untuk menghubungkan Hasura ke MySQL, gunakan JDBC. Berikut adalah contoh URL untuk koneksi MySQL:

```bash
jdbc:mysql://10.100.13.205:3306/Ihsan?user=Ihsan&password=Ihsan
```

Sesuaikan **IP address**, **port**, **database name**, **username**, dan **password** sesuai dengan konfigurasi kalian.  
Hasura kini dapat menggunakan MySQL sebagai database.  
