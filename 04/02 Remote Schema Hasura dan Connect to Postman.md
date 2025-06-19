
# Menambahkan Remote Schema di Hasura

Langkah pertama untuk menambahkan *Remote Schema* pada Hasura adalah dengan masuk ke menu **Remote Schema**. Kemudian, klik tombol **Add** pada bagian kiri navigasi untuk menambahkan remote schema. Setelah itu, Anda akan diarahkan ke tampilan seperti berikut:

![image](https://github.com/user-attachments/assets/e92a9af5-7175-4680-8a18-3b56ae909410)

    
## Mengisi Remote Schema
1. Pada kolom **Remote Schema Name**, masukkan nama yang sesuai atau yang Anda inginkan untuk menandai schema.
2. Pada kolom **GraphQL Service URL**, masukkan URL Service yang mengarahkan ke GraphQL yang ingin Anda *remote* dari Hasura. Misalnya, Anda dapat memasukkan nama dan URL seperti berikut ini.

## Kustomisasi Root Field
Selanjutnya, gulir ke bagian bawah untuk **Customization GraphQL** dan tambahkan **Customization Root Field Namespace**. Hal ini bertujuan agar tabel pada schema disimpan dalam satu namespace sehingga tidak bercampur atau berantakan dengan schema lainnya. Kalian dapat merujuk pada gambar berikut.

![image](https://github.com/user-attachments/assets/ec499c25-d1bd-4806-af29-d3a11614fa02)

Jika pengaturan sudah selesai, klik **Save** untuk mengimplementasikan. 
  
## Menguji Remote Schema dengan Query
Setelah selesai menambahkan remote schema, cobalah untuk melakukan *query* GraphQL pada schema tersebut. Anda bisa masuk ke menu **API** dan menuliskan langsung *Query GraphQL* yang tersedia untuk dijalankan.

### Contoh Query:
```graphql
query MyQuery {
  todosGraphql {
    todos {
      done
      id
      text
      user {
        id
        name
      }
    }
  }
}
```

Hasil dari query akan muncul di bagian bawah seperti pada gambar berikut.

![image](https://github.com/user-attachments/assets/6a5e4fc3-d36c-4c5d-a17e-006ce0864c7f)
  
  
---

# Menghubungkan Hasura ke Postman

### 1. Unduh Postman
Langkah pertama adalah mengunduh Postman Desktop melalui tautan berikut ini: [https://www.postman.com/downloads/](https://www.postman.com/downloads/). Mengapa perlu menggunakan Postman Desktop? Walaupun Postman Web dapat digunakan, ia memerlukan *Postman Agent* untuk menghubungkan Hasura yang diprivate. Oleh karena itu, saya menggunakan Postman Desktop untuk mempermudah prosesnya.

### 2. Install dan Login
Setelah berhasil diunduh, lakukan instalasi Postman Desktop. Setelah itu, buat akun Postman terlebih dahulu dan login ke Postman Desktop. Setelah masuk, Anda akan melihat tampilan awal Postman seperti berikut.

![Screenshot (192)](https://github.com/user-attachments/assets/e47e04d2-7189-4454-903a-7f5d155a4b74)

  
### 3. Membuat API Request Baru
Untuk membuat *API Request* baru, klik tombol **New Request** pada pojok kanan atas. Anda akan diarahkan ke tampilan untuk membuat permintaan baru. Atau, Anda juga dapat mengklik tombol **New** pada *workspace* yang Anda gunakan, lalu pilih tipe permintaan seperti **HTTP Request**.

![Screenshot (194)](https://github.com/user-attachments/assets/867a522e-485b-4019-b1ad-3067c0ff4aa8)

Setelah itu kalian akan diarahkan kepada tampilan seperti berikut.

![Screenshot (193)](https://github.com/user-attachments/assets/505d323d-40e1-48a9-833d-be7bf1578428)
  

### 4. Ubah Request dari GET ke POST
Setelah masuk ke halaman *request*, ubah metode *request* dari **GET** menjadi **POST** seperti tampilan berikut.

![Screenshot (195)](https://github.com/user-attachments/assets/beb70a90-6adb-4fa3-b51d-f1d3bc8696d7)

  
### 5. Masukkan URL GraphQL Hasura
Selanjutnya, masukkan URL GraphQL Hasura yang ingin Anda koneksikan ke Postman. Jika Anda menggunakan *admin secret* (x-hasura-admin-secret) untuk mengakses Hasura, Anda bisa memasukkannya ke bagian **Headers** seperti contoh berikut. *Admin secret* ini dapat ditemukan di menu **API** di Hasura Anda.

![Screenshot (196)](https://github.com/user-attachments/assets/b52bf82f-9a72-4131-addc-f26503e2164b)

  
### 6. Menjalankan Query GraphQL
Masukkan *query* GraphQL yang tersedia dan yang ingin Anda jalankan pada kolom **Body**. Pastikan Anda memilih tipe **GraphQL** sebagai format *body*. Anda dapat merujuk pada gambar di bawah ini untuk lebih jelasnya.  

![Screenshot (197)](https://github.com/user-attachments/assets/4e675b99-aa77-4535-ba2a-764f03ef5c98)
  

### 7. Contoh Query
Di sini saya mencoba memasukkan *query* untuk menampilkan seluruh data pada tabel *todos* yang di-*join* dengan tabel *user*. 

#### Contoh Query:
```graphql
query MyQuery {
  todosGraphql {
    todos {
      done
      id
      text
      user {
        id
        name
      }
    }
  }
}
```

Hasil dari query akan muncul di bagian bawah seperti pada gambar berikut.  
  
![Screenshot (200)](https://github.com/user-attachments/assets/b11805c1-db6e-42fb-aad4-7b613e25e8c4)

# Menghubungkan Hasura ke Postman Menggunakan GraphQL

Selain menggunakan metode HTTP, Anda juga dapat menghubungkan Hasura ke Postman dengan memanfaatkan fitur bawaan Postman untuk GraphQL. Berikut langkah-langkahnya:

### 1. Memilih Mode GraphQL pada Postman

- Buka Postman dan buat request baru.
- Alih-alih memilih metode `HTTP`, pilihlah `GraphQL` seperti gambar di bawah ini.

![Screenshot (238)](https://github.com/user-attachments/assets/d2a6893b-a7c9-4fa7-99c6-82a248bfd2e3)


### 2. Konfigurasi URL dan Header

Setelah memilih mode `GraphQL`, Anda akan diarahkan ke halaman konfigurasi request GraphQL seperti berikut.

![Screenshot (239)](https://github.com/user-attachments/assets/4b2ff14a-d865-4531-bfe5-9d3977ea4ce1)

    
- Masukkan URL Hasura Anda pada kolom URL.
- Tambahkan header `x-hasura-admin-secret` dan isi dengan value yang sesuai.

## 3. Menampilkan Query yang Tersedia

Pada halaman query, klik tombol `Use GraphQL Introspection` untuk melihat query yang tersedia pada Hasura. Dengan fitur ini, Anda dapat mengeksplorasi skema GraphQL dan query yang dapat dijalankan.

![Screenshot (240)](https://github.com/user-attachments/assets/419c2303-d5f6-4d22-8ad5-349183c13ebf)


## 4. Mengeksplorasi Query GraphQL

Setelah melakukan introspection, Postman akan menampilkan query explorer yang memungkinkan Anda melihat skema GraphQL yang tersedia pada Hasura.

- Pilih query yang ingin dijalankan, kemudian salin query tersebut ke kolom query editor.

![Screenshot (241)](https://github.com/user-attachments/assets/d40cd684-d4d9-42f2-854b-eb4080e66d02)


## 5. Menjalankan Query

Setelah memasukkan query pada editor, klik tombol `Query` untuk menjalankan query. Hasil dari query yang dijalankan akan ditampilkan pada panel `Response` di Postman.

![Screenshot (242)](https://github.com/user-attachments/assets/342e444f-d471-4a90-8a6e-578a62f19925)


Dengan langkah-langkah di atas, Anda dapat dengan mudah menghubungkan Hasura ke Postman dan menjalankan query GraphQL secara langsung tanpa menggunakan metode HTTP.
