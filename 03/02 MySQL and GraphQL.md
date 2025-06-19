# Panduan Menggunakan DBeaver dan Hasura GraphQL untuk Pengelolaan Data

## 1. Instalasi DBeaver

Untuk menambah kolom di database, kalian dapat menggunakan DBeaver sebagai alat bantu. Langkah pertama adalah menginstal DBeaver pada perangkat kalian. Kalian bisa mengunduhnya melalui tautan berikut:  
[Unduh DBeaver](https://dbeaver.io/download/)  

## 2. Membuat Koneksi ke MySQL

Setelah DBeaver terinstal, buat koneksi baru ke MySQL dan masukkan detail setup host atau URL JDBC.   

![image](https://github.com/user-attachments/assets/e1956996-0865-44d9-83f2-9b375d2882d1)

  
![image](https://github.com/user-attachments/assets/876150a1-e42f-45a9-9501-8c3dd3e63f23)

    
Lakukan tes koneksi sebelum mengklik **Finish** untuk memastikan koneksi berhasil. 

![image](https://github.com/user-attachments/assets/22003f61-b7a0-476a-8937-5c22286b753c)
 

Jika koneksi sudah terhubung, kalian dapat melakukan berbagai tindakan, seperti:

- **Create Table:** Membuat tabel baru.
- **Add Column:** Menambah kolom dan menentukan tipe data.
- **Primary/Unique Key:** Menetapkan primary key atau unique key.
- **Foreign Key:** Membuat foreign key untuk menghubungkan tabel.

Berikut tampilan untuk membuat tabel baru di Bbeaver:  

![image](https://github.com/user-attachments/assets/dcad5dc9-f4b5-400f-92fa-0a005db11e2c)

  
Berikut adalah tampilan di DBeaver untuk menambahkan kolom baru pada tabel di database MySQL, sekaligus mengatur tipe data serta menetapkan primary key atau unique key:  

![image](https://github.com/user-attachments/assets/487f8ffb-afe1-418d-b2f6-33d79a193190)


Berikut adalah tampilan di DBeaver untuk menambahkan foreign key pada tabel di database MySQL yang sudah dibuat, guna membentuk relasi antar tabel serta menjaga integritas data:  

![image](https://github.com/user-attachments/assets/d5429388-96e9-4090-84e3-5b27bd4ef5e1)


## 3. Memasukkan Data Menggunakan GraphQL di Hasura

Setelah tabel dibuat di DBeaver, kalian dapat menggunakan perintah **mutation insert** pada GraphQL di Hasura untuk memasukkan data ke tabel tersebut. Pastikan untuk memeriksa apakah tabel dan relasi foreign key sudah terdeteksi (tracked) di halaman data. Jika belum, kalian harus melakukan tracking terlebih dahulu.

### Contoh Mutation Insert

```graphql
mutation insert_attendance {
  insert_attendance(objects: {
    student_id: 1, 
    class_id: 3, 
    date: "2024-09-11", 
    status: "present"
  }) {
    affected_rows
    returning {
      id
      status
      date
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/59337d45-70ad-4ea7-a585-412c8756f8e5)

  
### Mutation Insert Multiple Data  

```graphql
mutation insert_students {
  insert_students(objects: [
    { name: "Charlie Brown", grade: "9", address: "101 Pine Street" },
    { name: "Diana Prince", grade: "9", address: "102 Pine Street" },
    { name: "Evan Taylor", grade: "8", address: "103 Cedar Lane" },
    { name: "Fiona Davis", grade: "8", address: "104 Cedar Lane" },
    { name: "George Miller", grade: "7", address: "105 Birch Road" },
    { name: "Helen Clark", grade: "7", address: "106 Birch Road" },
    { name: "Ian Lee", grade: "6", address: "107 Elm Street" },
    { name: "Julia Roberts", grade: "6", address: "108 Elm Street" }
  ]) {
    affected_rows
    returning {
      id
      name
    }
  }
}
```
  
Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/b1e9ee2d-b740-498a-af31-b1dec6cad70a)

    
## 4. Update Data Menggunakan GraphQL
   
Untuk memperbarui data yang sudah diinput, kalian dapat menggunakan perintah mutation update pada GraphQL.  

  ### Contoh Mutation Update

```graphql
mutation update_attendance {
  update_attendance(where: { id: { _eq: 1 } }, _set: { status: "absent" }) {
    affected_rows
    returning {
      id
      status
      date
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/b710f249-d708-4bac-8330-b153f65c1afc)

    
### Mutation Update Multiple Data  

Untuk memperbaiki banyak data yang sudah diinput secara bersamaan dapat menggunakan perintah mutation update GraphQL seperti berikut.

```graphql
mutation {
  update_attendance_1: update_attendance(where: {id: {_eq: 35}}, _set: {id: 5}) {
    affected_rows
  }
  update_attendance_2: update_attendance(where: {id: {_eq: 36}}, _set: {id: 6}) {
    affected_rows
  }
  update_attendance_3: update_attendance(where: {id: {_eq: 37}}, _set: {id: 7}) {
    affected_rows
  }
  update_attendance_4: update_attendance(where: {id: {_eq: 38}}, _set: {id: 8}) {
    affected_rows
  }
  update_attendance_5: update_attendance(where: {id: {_eq: 39}}, _set: {id: 9}) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/dc745765-5484-49d6-b12a-960f8cc136f0)

  
Jika kalian ingin memperbaiki banyak data yang sudah diinput secara bersamaan tetapi memiliki set value yang sama, kalian dapat menggunakan perintah mutation update GraphQL seperti berikut.

```graphql
mutation {
  update_attendance(
    where: {
      id: { _in: [5, 6, 7, 8, 9, 10] }
    }
    , _set: {status: "absent"}
  ) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/d275569d-9897-4b93-89f1-bfa7b1e0a1a1)

    
## 5. Menghapus Data Menggunakan GraphQL
   
Untuk menghapus data, gunakan perintah mutation delete. Kalian juga bisa menghapus beberapa data sekaligus.

  ### Contoh Mutation Delete

```graphql
mutation delete_attendance {
  delete_attendance(where: { id: { _eq: 1 } }) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/7f1aa752-0c79-4535-bcab-506188703740)

    
### Mutation Delete Multiple Data  

```graphql
mutation {
  delete_attendance(
    where: {
      id: { _in: [5, 6, 7] }
    }
  ) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/0868d7ab-a4e4-4296-9a99-8d2e50d86d21)

    

## 6. Mengambil Data Menggunakan GraphQL
   
Untuk membaca atau mengambil data dari database, kalian bisa menggunakan perintah query.

  ### Contoh Query Mengambil Data Berdasarkan Kriteria Tertentu

```graphql
query get_attendance {
  attendance(where: { class_id: { _eq: 3 }, date: { _eq: "2024-09-11" } }) {
    student_id
    status
    date
    student {
      name
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/f79da4e4-04ac-4ff2-a8a9-beac8b3a5146)

    
### Contoh Query Mengambil Semua Data

```graphql
query {
  attendance {
    id
    student_id
    class_id
    date
    status
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/fff10cd6-5dd1-455e-92a9-c65182b6bee5)

      
Fungsi utama query ini adalah untuk mengambil data dari database, baik itu secara keseluruhan maupun berdasarkan filter tertentu seperti pada contoh di atas. Dalam contoh yang telah disebutkan, data dari tabel attendance diambil untuk ditampilkan kepada pengguna atau aplikasi. Selain itu perintah ini dapat Memfilter Data Berdasarkan Kriteria Tertentu: Dengan menambahkan filter (where), kamu bisa mengambil data yang spesifik, seperti data siswa yang hadir pada hari tertentu atau data kehadiran dalam rentang waktu tertentu. Contoh sebelumnya adalah pengmabilan data menggunakan filter class dan date. 

Setelah Anda melakukan perintah CRUD, pastikan data telah masuk atau terbarui dengan memeriksa data di tab Data pada Hasura, seperti yang terlihat pada gambar berikut:

![image](https://github.com/user-attachments/assets/43f51ccd-f039-48fe-b2ef-eaee30d89643)

  
Kalian juga dapat memeriksa Foreign Key Relationships yang telah dibuat sebelumnya melalui DBeaver atau di Hasura. Caranya, masuk ke menu Data, kemudian pilih database yang ingin kalian cek relasinya. Setelah itu, masuk ke bagian Foreign Key Relationships untuk melihat foreign key dan relasi antar tabel. Tampilan di Hasura akan terlihat seperti gambar berikut:

![image](https://github.com/user-attachments/assets/04054441-fbae-4d64-948d-5e91312239d0)


Gambar di atas menunjukkan dua jenis relasi yang ada pada database yang dipilih, yaitu:
- Object Relationships (one-to-one)
- Array Relationships (one-to-many)

Pastikan relasi antar tabel yang kalian inginkan atau sudah kalian buat pada DBeaver sebelumnya sudah berada dalam kondisi **"track"**. Jika belum, kalian bisa masuk ke tab **Untrack**, kemudian klik **Track** pada relasi yang ingin diimplementasikan.
  
Kalian juga dapat melihat relasi antar tabel pada DBeaver. Kalian cukup pilih database atau table yang ingin kalian lihat relasinya setelah itu pilih ERD untuk mengakses relasi antar tabel seperti pada gambar berikut.  

![image](https://github.com/user-attachments/assets/f4e4bcc7-57e3-4ee1-b687-03c3f44e3d35)

  
