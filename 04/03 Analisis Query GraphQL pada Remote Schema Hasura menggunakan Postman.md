
# Analisis Query GraphQL

## Schema: todosGraphql

### Query 1: GetAllTodos
```graphql
query GetAllTodos {
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
  
**Penjelasan**: 
Query ini digunakan untuk mengambil semua data dari tabel `todos` beserta data usernya dikarenakan query diatas dijoinkan dengan tabel `users`. Setiap todo yang diambil akan menampilkan informasi status (`done`), ID todo (`id`), teks todo (`text`), serta data pengguna yang terkait dengan todo tersebut (ID dan nama pengguna).

Berikut adalah hasil ketika diterapkan pada GraphQL di Postman.

![Screenshot (200)](https://github.com/user-attachments/assets/e16a6d7a-6aed-442a-8a32-1208e294ee73)


Dari hasil di atas dapat diketahui bahwa jumlah data pada tabel `todos` adalah satu dan berelasi dengan satu data pada tabel `user`

### Query 2: GetAllUsers
```graphql
query GetAllUsers {
  todosGraphql {
    users {
      id
      name
    }
  }
}
```
  
**Penjelasan**: 
Query ini mengambil seluruh data pengguna (`users`) dari schema `todosGraphql`. Hanya dua atribut yang ditampilkan, yaitu ID pengguna (`id`) dan nama pengguna (`name`).

Berikut adalah hasil ketika diterapkan pada GraphQL di Postman.

![Screenshot (201)](https://github.com/user-attachments/assets/326ca41a-df17-4fb0-8945-ad99712d6b1b)


Dari hasil di atas dapat diketahui bahwa jumlah data pada tabel `users` adalah satu.
  
### Query 3: todoByPK
```graphql
query todoByPK {
  todosGraphql {
    todo(id: "1") {
      done
      id
      text
      user {
        id
        name
      }
    }
    users {
      id
    }
  }
}
```
  
**Penjelasan**:
Query ini mengambil data todo berdasarkan `id` tertentu, dalam hal ini todo dengan ID "1". Informasi yang diambil meliputi status (`done`), ID (`id`), teks todo (`text`), dan data pengguna terkait (`id` dan `name`). Selain itu, query ini juga mengambil semua ID pengguna yang ada di dalam tabel `users`.

Berikut adalah hasil ketika diterapkan pada GraphQL di Postman.

![Screenshot (203)](https://github.com/user-attachments/assets/cddfba59-04fc-4deb-9052-612ccda574f4)
  

Dari hasil di atas dapat diketahui bahwa data dengan `id = 1` tabel `todos` adalah **user** dengan nama **ferdy**.
  
### Query 4: userByPK
```graphql
query userByPK {
  todosGraphql {
    user(id: "1") {
      id
      name
    }
  }
}
```
  
**Penjelasan**:
Query ini digunakan untuk mengambil data pengguna berdasarkan ID pengguna tertentu, dalam hal ini pengguna dengan ID "1". Informasi yang ditampilkan hanya berupa ID (`id`) dan nama pengguna (`name`).

Berikut adalah hasil ketika diterapkan pada GraphQL di Postman.

![Screenshot (204)](https://github.com/user-attachments/assets/b8d7d623-46ce-4ea1-84d9-c8d8faab5bc5)


Dari hasil di atas dapat diketahui bahwa data dengan `id = 1` tabel `users` adalah **user** dengan nama **ferdy**.
   
---

## Schema: springbootGraphql

### Query 1: countAuthorsAndTutorials
```graphql
query countAuthorsAndTutorials {
  springbootGraphql {
    countAuthors
    countTutorials
  }
}
```

**Penjelasan**:
Query ini digunakan untuk menghitung total jumlah penulis (`countAuthors`) dan jumlah tutorial (`countTutorials`) di schema `springbootGraphql`.

Berikut adalah hasil ketika diterapkan pada GraphQL di Postman.

![Screenshot (205)](https://github.com/user-attachments/assets/40d65029-97ac-4666-ac39-a1353fba4711)

  
Dari hasil di atas dapat diketahui bahwa terdapat 1 row pada tabel `Authors` dan 2 rows pada tabel `Tutorials`.
  
### Query 2: getAllAuthors
```graphql
query getAllAuthors {
  springbootGraphql {
    findAllAuthors {
      age
      id
      name
    }
  }
}
```
  
**Penjelasan**:
Query ini mengambil semua data penulis (`findAllAuthors`). Setiap penulis akan menampilkan informasi berupa umur (`age`), ID penulis (`id`), dan nama penulis (`name`).

Berikut adalah hasil ketika diterapkan pada GraphQL di Postman.

![Screenshot (206)](https://github.com/user-attachments/assets/bd1ea91c-97ec-4d53-84da-f6216b2e6c85)


Dari hasil di atas dapat diketahui bahwa terdapat 1 `Author` dengan data `usia = 27`, `id = 1`, dan `nama = "bezkoder"`.
  
### Query 3: getAllTutorials
```graphql
query getAllTutorials {
  springbootGraphql {
    findAllTutorials {
      description
      id
      title
      author {
        age
        id
        name
      }
    }
  }
}
```
  
**Penjelasan**:
Query ini digunakan untuk mengambil semua tutorial dari schema `springbootGraphql`. Untuk setiap tutorial, informasi yang ditampilkan meliputi deskripsi (`description`), ID tutorial (`id`), judul tutorial (`title`), dan informasi penulis yang terkait (umur, ID, dan nama penulis).

Berikut adalah hasil ketika diterapkan pada GraphQL di Postman.

![Screenshot (208)](https://github.com/user-attachments/assets/bb753465-18ca-4662-85dd-e30dc30d0d91)

  
Dari hasil di atas dapat diketahui bahwa terdapat 2 `Tutorials` dengan satu `Author` yang sama.  
  
