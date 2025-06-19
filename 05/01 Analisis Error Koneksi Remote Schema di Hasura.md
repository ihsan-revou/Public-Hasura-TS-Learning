# Dokumentasi Skenario Pengujian GraphQL dan Postman

## 1. Matikan Sumber GraphQL  
Scale down deployment menjadi 0 seperti gambar berikut.
  
![WhatsApp Image 2024-09-23 at 13 31 59](https://github.com/user-attachments/assets/8783c366-1899-49f0-b170-9c119e627457)

  
**Pod/Deployment**:
- Pod: todos-graphql-868cddb77b-rh65t
- Deployment: todos-graphql
- Namespace: default

## 2. Analisa Error pada Hasura yang Meremote ke GraphQL
Cek log masing-masing pod Hasura yang terhubung dengan schema untuk melihat apakah ada error. Hasura masih jalan dengan lancar, tidak terjadi error.
  
![image](https://github.com/user-attachments/assets/48a66ce0-ba2e-4d3b-83f2-96d2fafbcc55)


## 3. Query dari Postman (Sumber GraphQL Mati)
Buat query GraphQL di Postman dan kirim permintaan.  

**Analisis**:    
  
Apakah ada error di Postman? Terdapat error pada Postman ketika menjalankan query.

![Screenshot (217)](https://github.com/user-attachments/assets/6370c2a6-4acb-459c-b7a4-ffe69c66cded)

  
Error yang ditemukan:

```json
{
    "errors": [
        {
            "message": "HTTP exception occurred while sending the request to \"http://10.100.14.10:8989/query\"",
            "extensions": {
                "path": "$",
                "code": "remote-schema-error",
                "internal": {
                    "message": "Connection failure: Network.Socket.connect: <socket: 40>: does not exist (Connection refused)",
                    "request": {
                        "host": "10.100.14.10",
                        "method": "POST",
                        "path": "/query",
                        "port": 8989,
                        "queryString": "",
                        "requestHeaders": {
                            "Content-Type": "application/json",
                            "User-Agent": "hasura-graphql-engine/v2.42.0",
                            "x-hasura-role": "admin"
                        },
                        "responseTimeout": "ResponseTimeoutMicro 60000000",
                        "secure": false
                    },
                    "type": "http_exception"
                }
            }
        }
    ]
}
```

# Analisis Error Koneksi Remote Schema di Hasura

Error di atas mengindikasikan bahwa Hasura mencoba mengakses _remote schema_ di URL `http://10.100.14.10:8989/query`, namun gagal karena koneksi ke server tersebut ditolak (_Connection refused_). Berikut analisis lebih lanjut tentang error ini:

## Komponen Error

1. **Pesan Utama**:
    - `"HTTP exception occurred while sending the request to \"http://10.100.14.10:8989/query\""` menunjukkan bahwa terjadi kesalahan HTTP ketika mengirim permintaan ke URL yang ditentukan.
  
2. **Internal Error**:
    - **`Connection failure`**: Menyatakan kegagalan koneksi saat mencoba terhubung ke alamat IP `10.100.14.10` pada port `8989`.
    - **`does not exist (Connection refused)`**: Ini berarti server di IP dan port yang dimaksud tidak menerima koneksi dari Hasura. Penyebab umum bisa karena server tidak aktif, port yang salah, atau firewall yang memblokir koneksi.

## Kemungkinan Penyebab dan Solusi

1. **Service Tidak Berjalan**:
    - Alamat IP `10.100.14.10` mungkin adalah pod atau service di dalam cluster Kubernetes. Jika pod atau service di alamat ini tidak berjalan atau belum siap, maka koneksi akan ditolak.
    - **Solusi**: Pastikan bahwa pod atau service yang terkait dengan IP `10.100.14.10` di Kubernetes berjalan dengan baik. Kamu bisa memeriksa statusnya pada kubernetes dashboard.

2. **Port Salah**:
    - Jika service yang diakses memiliki port yang berbeda, Hasura tidak akan bisa terhubung ke `8989`. Port mungkin tidak dikonfigurasi dengan benar.
    - **Solusi**: Periksa apakah port yang digunakan oleh service di alamat `10.100.14.10` sesuai. Jika port berbeda, sesuaikan pengaturan di remote schema Hasura untuk mengakses port yang benar.

3. **Network Policy atau Firewall**:
    - Jika ada _Network Policy_ di Kubernetes atau firewall eksternal yang memblokir lalu lintas ke port `8989`, Hasura tidak akan bisa terhubung.
    - **Solusi**: Pastikan tidak ada _Network Policy_ atau firewall yang membatasi akses ke service tersebut. Kamu bisa mengecek Network Policies yang diterapkan di namespace.

## Kesimpulan

- Langkah pertama adalah memeriksa apakah service yang dimaksud (`10.100.14.10:8989`) berjalan dan dapat diakses dari pod Hasura.
- Jika service berjalan, pastikan port yang digunakan benar dan tidak ada aturan jaringan yang memblokir koneksi.

Dalam kasus ini disebabkan karena `service tidak berjalan`.
  
## 4. Jalankan Lagi Sumber GraphQL  
Scale up deployment menjadi 1 seperti gambar berikut.

![Screenshot (215)](https://github.com/user-attachments/assets/faefae21-4a63-4327-ad7a-ff2556be0006)

    
## 5. Query dari Postman (Sebelum Reload Schema)
Kirim query GraphQL dari Postman tanpa melakukan reload schema.  
  
**Analisis**:   
  
Apakah ada error di Postman?
Tidak ditemukan error dapat dilihat pada gambar berikut.

![Screenshot (219)](https://github.com/user-attachments/assets/6b4a0b99-35ec-4291-b14a-5ef1cef6ae94)
  
