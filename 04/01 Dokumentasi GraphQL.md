# Dokumentasi GraphQL

Dokumentasi ini mencakup **GraphQL** dan komponen seperti `limit`, `offset`, `where`, `order_by`, `aggregate`, `by_pk`, `and`, `or`, `not`, serta operator perbandingan lainnya. Termasuk contoh penerapannya dan penambahan operator baru seperti `_ilike`, `_like`, dan `contains`.

## 1. Limit & Offset

- **limit**: Membatasi jumlah data yang dikembalikan.
- **offset**: Melewati sejumlah record pertama dan menampilkan sisanya mulai dari posisi tertentu.

### Contoh:

```graphql
query getLimitedSchedules {
  schedules(limit: 5, offset: 2) {
    id
    day_of_week
    start_time
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:
  
![image](https://github.com/user-attachments/assets/277c0a3b-d33d-4b09-b780-5c8dc1ece39f)


 
Query ini mengambil 5 data dimulai dari baris ketiga.  

## 2. Klausa Where

Klausa where digunakan untuk memfilter data berdasarkan kondisi tertentu.  
  
- **eq**: Sama dengan (=)
- **neq**: Tidak sama dengan (<>)
- **_gt**: Lebih besar dari (>)
- **_gte**: Lebih besar atau sama dengan (>=)
- **_lt**: Lebih kecil dari (<)
- **_lte**: Lebih kecil atau sama dengan (<=)
- **_in**: Nilai harus ada di dalam daftar
- **_nin**: Nilai tidak boleh ada di dalam daftar
- **_is_null**: Memeriksa apakah kolom memiliki nilai NULL
- **_like**: Melakukan pencarian string berdasarkan pola yang sensitif terhadap huruf besar/kecil.
- **_ilike**: Melakukan pencarian string berdasarkan pola yang tidak sensitif terhadap huruf besar/kecil (case-insensitive).
- **_contains**: Memeriksa apakah array berisi nilai tertentu.

### Contoh:

```graphql
query getSchedulesByDay {
  schedules(where: { day_of_week: { _eq: "Monday" } }) {
    id
    start_time
    end_time
  }
}
```
  
Berikut adalah hasil ketika diterapkan pada GraphQL:
  
![image](https://github.com/user-attachments/assets/05c4a7bf-7b2d-4dec-b9e4-ad47ddc4a061)


  
Query ini mengambil semua jadwal pada hari Senin.   
  
### Menggunakan Operator Like dan ILike: 

```graphql
query searchSchedulesByTeacher {
  schedules(
    where: { teacher: { name: { _like: "%Brown%" } } }
  ) {
    id
    day_of_week
    teacher {
      name
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/d388255f-4f91-436b-8aba-4e2501414c11)

  
Mengambil jadwal yang memiliki guru dengan nama mengandung "Brown", sensitif terhadap huruf besar/kecil.

```graphql
query searchSchedulesByTeacherIlike {
  schedules(
    where: { teacher: { name: { _ilike: "%orange%" } } }
  ) {
    id
    day_of_week
    teacher {
      name
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/28f7725a-30ef-4fa9-8563-68a1b6b0da93)


   
Mengambil jadwal yang memiliki guru dengan nama mengandung "orange", tidak sensitif terhadap huruf besar/kecil.  

### Menggunakan Contains: 
  
```graphql
query getSchedulesByTeacherIds {
  schedules(where: {teacher: {name: {contains: "Gray"}}}) {
    id
    day_of_week
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/a9c95e3f-b1fb-4fab-91cc-93dd40a2fdf3)


  
Mengambil semua jadwal yang memiliki `teacher name` Gray.  

## 3. Order By  

Digunakan untuk mengurutkan data berdasarkan kolom tertentu dalam urutan menaik (`asc`) atau menurun (`desc`).  

### Contoh:  

```graphql
query getSchedulesOrdered {
  schedules(order_by: { start_time: asc }) {
    id
    start_time
    end_time
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/9ed3b578-b467-4921-a6ed-8d4abd74e2bf)


  
Mengambil semua jadwal diurutkan berdasarkan `start_time` secara menaik.  

## 4. Aggregate  

Digunakan untuk melakukan fungsi agregat seperti menghitung jumlah (`count`), rata-rata (`avg`), jumlah total (`sum`), dan sebagainya.

### Contoh:  

```graphql
query getSchedulesAggregate {
  schedules_aggregate {
    aggregate {
      count
      avg {
        class_id
      }
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/c2cda4fb-288d-407f-9dc7-1ffaf49ff784)

  

Hasil dari query ini akan memberikan gambaran umum tentang jumlah data dan nilai rata-rata untuk kolom `class_id` dalam `schedules`.

## 5. Berdasarkan Primary Key (by_pk)

Mengambil data berdasarkan primary key.

### Contoh:  

```graphql
query getScheduleById {
  schedules_by_pk(id: 22) {
    id
    day_of_week
    start_time
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/812d0cb1-2317-4f28-8fd7-9f46925cb4c7)


  
Mengambil jadwal dengan primary key `id = 22`.  

## 6. Operator Logika (_and, _or, _not)

`AND`, `OR`, dan `NOT` digunakan untuk menggabungkan beberapa kondisi filter.
- **_and**: Kondisi logika AND.
- **_or**: Kondisi logika OR.
- **_not**: Kondisi logika NOT.

### Contoh:  

```graphql
query getSchedulesWithOrCondition {
  schedules(
    where: {
      _or: [
        { day_of_week: { _eq: "Monday" } },
        { day_of_week: { _eq: "Friday" } }
      ]
    }
  ) {
    id
    day_of_week
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/23ada8c7-64dd-45e8-86b4-78ae30e6639a)


  
Mengambil jadwal pada hari Senin atau Jumat.  

## 7. Operator Perbandingan (_eq, _neq, _gt, _gte, _lt, _lte, _in, _nin, _is_null, _like, _ilike, contains)

Operator pembanding digunakan untuk membandingkan nilai dalam query.
- **_eq**: Sama dengan.
- **_neq**: Tidak sama dengan.
- **_gt**: Lebih besar dari.
- **_gte**: Lebih besar atau sama dengan.
- **_lt**: Lebih kecil dari.
- **_lte**: Lebih kecil atau sama dengan.
- **_in**: Nilai harus ada dalam daftar.
- **_nin**: Nilai tidak boleh ada dalam daftar.
- **_is_null**: Memeriksa apakah nilai kolom NULL.

### Contoh:  

```graphql
query getSchedulesByCondition {
  schedules(where: { start_time: { _gt: "08:00:00" } }) {
    id
    start_time
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/9da40296-d548-482e-b098-f77c726dfc64)


  
Mengambil jadwal yang dimulai setelah pukul 08:00.  

### 8. String Matching (`_like`, `_ilike`, `contains`)  
  
- `_like` digunakan untuk pencocokan pola dengan case-sensitive.
- `_ilike` digunakan untuk pencocokan pola tanpa memperhatikan besar-kecil huruf.
- `contains` digunakan untuk memeriksa apakah suatu string mengandung nilai tertentu.

**Contoh:**
```graphql
query GetTeachersByName {
  teachers(where: { name: { _ilike: "%yellow%" } }) {
    id
    name
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/7ea5bee8-d34b-48b8-a33b-eee77d717c46)



## 9. Mutasi (Insert, Update, Delete)

### `insert`, `update`, dan `delete`
- `insert` digunakan untuk memasukkan data baru.
- `update` digunakan untuk mengubah data yang sudah ada.
- `delete` digunakan untuk menghapus data.
  
### Menambahkan Data (Insert):  

```graphql
mutation insertSchedule {
  insert_schedules(objects: {
    day_of_week: "Monday",
    start_time: "09:00:00",
    end_time: "10:00:00",
    teacher_id: 1
  }) {
    returning {
      id
      day_of_week
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/13274916-1678-480e-b9d6-26ea0ad7b088)


  
Menambahkan jadwal baru untuk hari Senin pukul 09:00 sampai 10:00.

### Memperbarui Data (Update):

```graphql
mutation updateSchedule {
  update_schedules(
    where: { id: { _eq: 23 } },
    _set: { day_of_week: "Friday" }
  ) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/37075e9c-9179-4272-88c3-3f95181831ac)


  
Memperbarui jadwal dengan `id = 23` menjadi hari Jumat.  

### Menghapus Data (Delete):

```graphql
mutation deleteSchedule {
  delete_schedules(where: { id: { _eq: 31 } }) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/1aab168d-764c-48d4-98e6-1ce5d9d4dac2)


  
Menghapus jadwal dengan `id = 31`.

## 10. affected_rows dan returning dalam Mutation

### **affected_rows**
- Mengembalikan jumlah baris yang terpengaruh oleh operasi **mutation**.

### **returning**
- Mengembalikan data yang dimodifikasi setelah operasi **mutation**.

### Contoh:
```graphql
mutation {
  update_schedules(where: { id: { _eq: 24 } }, _set: { day_of_week: "Friday" }) {
    affected_rows
    returning {
      id
      day_of_week
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/74832e9b-6f26-493c-b676-836e07af7057)



### Error Handling
Jika salah satu dari `affected_rows` atau `returning` tidak digunakan dalam query, error tidak akan terjadi. Keduanya opsional tergantung dari kebutuhan pengguna. Tetapi, harus ada salah satu dari keduanya untuk mengetahui respon dari mutasi.

### Contoh:
```graphql
mutation {
  update_schedules(where: {id: {_eq: 25}}, _set: {day_of_week: "Friday"}) {
    returning {
      id
      day_of_week
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:
  
![image](https://github.com/user-attachments/assets/a4ae5261-a722-41de-a624-e87447ea16bc)



Kita dapat menjelajahi situs web [graphql.com](https://graphql.com/learn/mutations/) untuk contoh lebih lanjut dan penjelasan lebih mendalam tentang bagaimana konsep-konsep ini diterapkan dalam praktik.  

## 11. Query Join GraphQL

Di GraphQL, **join** dalam basis data dilakukan melalui **relationships** antar tabel. Dalam Hasura GraphQL, misalnya, kamu bisa melakukan query dengan `join` antara tabel menggunakan **relationships** yang telah ditentukan pada metadata. Ini memungkinkan kamu untuk meng-query data yang terkait dalam satu permintaan.

### Misalkan:
- Kamu punya dua tabel: `teachers` dan `schedules`.
- Tabel `schedules` memiliki foreign key `teacher_id` yang berhubungan dengan `id` di tabel `teachers`.

### Query untuk melakukan join antar dua tabel tersebut:

```graphql
query GetSchedulesWithTeacherInfo {
  schedules {
    id
    day_of_week
    start_time
    end_time
    teacher {
      id
      name
      email
    }
  }
}
```
  
Berikut adalah hasil ketika diterapkan pada GraphQL: 

![image](https://github.com/user-attachments/assets/1bbb4a0c-0d09-4d8f-ac8c-da3ddff99b68)

  
#### Penjelasan:
- **schedules**: Query ini mengambil data dari tabel `schedules`.
- **teacher**: Ini adalah hubungan antara tabel `schedules` dan `teachers` melalui kolom `teacher_id`. Dengan menggunakan hubungan ini, kita bisa mengambil data dari tabel `teachers` yang terkait, seperti `id`, `name`, dan `email`.

### Contoh query dengan kondisi (`where`) dan sorting (`order_by`):

```graphql
query GetSchedulesForMonday {
  schedules(where: {day_of_week: {_eq: "Monday"}}, order_by: {start_time: asc}) {
    id
    start_time
    end_time
    teacher {
      name
    }
  }
}
```
  
Berikut adalah hasil ketika diterapkan pada GraphQL: 

![image](https://github.com/user-attachments/assets/7e6206b6-5255-4f66-a8fe-2a9bccbfdf0b)


  
#### Penjelasan tambahan:
- **where**: Filter data berdasarkan kondisi tertentu, di sini `day_of_week` harus bernilai `"Monday"`.
- **order_by**: Mengurutkan hasil query berdasarkan `start_time` secara ascending.

Dengan cara ini, kamu bisa melakukan query yang mirip dengan `join` dalam SQL dengan mengambil data dari tabel-tabel terkait di GraphQL.  
  
