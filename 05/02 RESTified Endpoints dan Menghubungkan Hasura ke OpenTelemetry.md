# RESTified Endpoints

## 1. Masuk ke Menu API
Pertama, masuklah ke menu **API** di Hasura Console. Setelah itu, buatlah query GraphQL yang ingin diubah menjadi RESTful endpoint (RESTified). Anda dapat melihat query yang tersedia dengan mengeklik tombol **Explorer** yang terdapat di bagian atas field query GraphQL.

![image](https://github.com/user-attachments/assets/c41d8265-03cb-4903-b6f8-ed9e6f62c5a9)

  
## 2. Membuat REST Endpoint
Setelah menentukan query yang akan diubah, klik tombol **REST** di bagian atas field query GraphQL. Anda akan diarahkan ke halaman REST seperti pada gambar berikut.  

![image](https://github.com/user-attachments/assets/370d59d0-e1c2-49a5-95eb-6c92b9923dfe)

  
## 3. Konfigurasi REST Endpoint
- Masukkan nama untuk REST endpoint.
- Berikan deskripsi mengenai query (opsional).
- Centang box method yang ingin diubah menjadi REST endpoint (RESTified). Misalnya, pilih method `GET` untuk mempermudah developer dalam mengakses query.
- Setelah konfigurasi selesai, klik tombol **Create** untuk membuat REST endpoint.

Jika berhasil, REST endpoint yang telah Anda buat akan muncul pada daftar REST endpoint seperti gambar berikut.

![image](https://github.com/user-attachments/assets/26db2051-fd19-4f05-8205-9499eaceb849)

  
## 4. Menggunakan REST Endpoint di Postman
- Buka Postman dan masukkan link REST endpoint yang telah dibuat.
- Pilih method yang sesuai dengan REST endpoint tersebut (misalnya `GET`).
- Jangan lupa untuk menambahkan header `x-hasura-admin-secret` beserta valuenya.
- Klik tombol **Send** untuk menjalankan REST endpoint dan melihat hasilnya pada kolom `Response`.

![Screenshot (237)](https://github.com/user-attachments/assets/77b4fbec-33de-43f8-abdb-988b42a85a5a)


## Contoh REST Endpoint yang Telah Dibuat
1. **getAllRelationTodos**  
   Query ini digunakan untuk mengambil semua data pada tabel `todo` beserta semua tabel yang berelasi dengan tabel `todo`.

![image](https://github.com/user-attachments/assets/d6d970d1-611b-46f5-9996-d4c399d25520)

  
2. **getAllUsers**  
   Query ini digunakan untuk mengambil semua data pada tabel `user`.

![image](https://github.com/user-attachments/assets/1f9160f9-99bc-436f-bd46-344fa23a8ad1)

  
3. **getTodoById**  
   Query ini digunakan untuk mengambil data pada tabel `todo` berdasarkan `id` tertentu, misalnya `id = 1`.
  
![image](https://github.com/user-attachments/assets/16063056-98a9-4979-9f64-8c7a48122603)

  
4. **getUserById**  
   Query ini digunakan untuk mengambil data pada tabel `user` berdasarkan `id` tertentu, misalnya `id = 1`.

![image](https://github.com/user-attachments/assets/d567bc50-d49d-4cbc-a5d7-50d2f6d946fe)


# REST API Metode: GET dengan Parameter

Dokumen ini menjelaskan cara menggunakan metode **GET** dalam REST API dengan parameter query dan mengimplementasikannya di **Postman**.

## 1. REST API GET Request dengan Parameter

Pada metode GET, parameter biasanya dikirim melalui URL sebagai **query parameters**. Ketika menggunakan GraphQL, Anda dapat mengimplementasikan query GraphQL dalam REST API.

### Contoh URL dengan Query Parameters dan GET:
```
http://10.100.14.7:8085/api/rest/gettodobyid?id=1
```

Pada URL ini:
- Terdapat REST API GET query GraphQL yang akan dijalankan bernama 'gettodobyid'.
- Terdapat parameter yang dibutuhkan untuk query GraphQL, dalam hal ini `id`.
  
## 2. Implementasi di Postman

### Langkah-langkah:

1. **Buka Postman**.
2. **Buat Request Baru**:
   - Klik **New** dan pilih **HTTP Request**.
   - Beri nama request Anda, misalnya: `Get Todo by ID`.
   - Pilih metode **GET**.
   
3. **Masukkan URL API**:
   Di kolom URL, masukkan REST endpoint GraphQL Hasura yang menggunakan params. Contoh:
```
http://10.100.14.7:8085/api/rest/getuserbyid
```

4. **Headers**:
   Tambahkan header berikut untuk memberi tahu server bahwa Anda mengirimkan request GraphQL:
   - **Content-Type:** `application/json` (jika ingin menjalankan dengan file json).
   - **x-hasura-admin-secret:** `your-hasura-admin-secret` (jika menggunakan Hasura dengan admin secret).
  
  ![image](https://github.com/user-attachments/assets/cb37e0b5-1114-4d13-b22a-c3d2a107856a)


5. **Params**:
   Tambahkan parameter `key` and `value` untuk memberitahu server nilai dari parameter yang kita gunakan. Parameter yang kita gunakan dalam hal ini adalah `id`.
   
  ![Screenshot (255)](https://github.com/user-attachments/assets/c32d384d-cb89-4649-a7f9-33ac35a457ab)

   
7. **Kirimkan Request**:
   - Klik **Send**.
   - Anda akan melihat respons dari server, yang berisi data berdasarkan parameter `id` yang telah Anda masukkan.

### Contoh Response:

![image](https://github.com/user-attachments/assets/b1720683-d940-4dc1-96be-37af5ea290c0)

  
```json
{
    "todosGraphql": {
        "user": {
            "id": "1",
            "name": "ferdy"
        }
    }
}
```

### Penjelasan Komponen GET Request
- **Metode:** GET adalah metode HTTP yang digunakan untuk mengambil data dari server.
- **Query Parameters:** Parameter ditambahkan di akhir URL, setelah tanda tanya (`?`).

## 3. Implementasi Params di Hasura COnsole

1. **Buka Hasura**.
2. **Buat Query GraphQL Yang Menggunakan Params**:
   Berikut contoh Query GraphQL yang menggunakan params `id`.

``` graphql
query getTodoById($id: ID!) {
  todosGraphql {
    user(id: $id) {
      id
      name
    }
  }
}
```
   
4. **Masukkan Value Params Pada Query Variables**:
   Di kolom Query Variables yang terletak di bawah kolom GraphQL, masukkan variabel params dan valuenya. Contoh:
   
``` graphql  
{
  "id": 1
}
```

Hasil dari langkah-langkah di atas adalah sebagai berikut.

![image](https://github.com/user-attachments/assets/e091036f-9ccf-4b9a-a180-1df084580f30)

    
# Menghubungkan Hasura ke OpenTelemetry

Tujuan dari menghubungkan Hasura ke OpenTelemetry adalah untuk mengakses **Traces**, **Metrics**, dan **Logs** secara lebih efisien.

## 1. Masuk ke Pengaturan OpenTelemetry
Buka menu **Settings** pada Hasura Console untuk diarahkan ke halaman pengaturan. Selanjutnya, klik **OpenTelemetry Exporter** pada navigasi di sebelah kiri. Anda akan diarahkan ke halaman konfigurasi OpenTelemetry.

## 2. Konfigurasi OpenTelemetry
- Pilih **Status: Enabled** untuk mengaktifkan OpenTelemetry.
- Masukkan endpoint yang digunakan untuk `Traces`, `Metrics`, dan `Logs`.
- Klik tombol **Connect/Update** untuk menerapkan konfigurasi koneksi OpenTelemetry.

![image](https://github.com/user-attachments/assets/8b383ee9-5cb6-474b-842f-87c9d0592f43)


Adapun URL Endpoint yang saya gunakan adalah sebagai berikut:    
Traces Endpoint

```bash
http://10.100.13.205:8889/v1/traces
```

Metrics Endpoint  
  
```bash
http://10.100.13.205:8889/v1/metrics
```

Logs Endpoint  
  
```bash
http://10.100.13.205:8889/v1/logs
```
  
Dengan langkah-langkah ini, Anda dapat menghubungkan Hasura ke OpenTelemetry untuk memantau performa, error, dan logging pada aplikasi Anda secara terintegrasi. 

**NOTE:**
Kalian juga dapat melihat keberhasilan menghubungkan Hasura ke OpenTelemetry pada View Logs di pods Hasura yang telah kalian bangun. Coba lakukan refresh pods dengan scale down lalu scale up hasura pods kalian. Selanjutnya, cek view logs pada pod hasura dan temukan log mengenai connectivity Hasura dengan OpenTelemetry seperti gambar berikut.  

![image](https://github.com/user-attachments/assets/35857e24-f86f-4cbe-b4ff-9f92570b4f86)

