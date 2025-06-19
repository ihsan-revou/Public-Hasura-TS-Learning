# Panduan Desain Skema API

## Standardisasi

Desain skema API Supergraph harus menciptakan konvensi standar pada atribut berikut:

### Atribut Standardisasi dan Kemampuannya

| Atribut Standardisasi | Kemampuan |
|------------------------|-----------|
| **S1** | Memisahkan model (resources) dan perintah (methods) |

**Contoh:**
- *Model* adalah kumpulan data yang dapat di-query secara standar tanpa tergantung sumber data tertentu.
- *Perintah* adalah metode yang terkait dengan logika bisnis tertentu dan dapat mengembalikan referensi ke model atau perintah lainnya.

```graphql
# Cara standar untuk mengambil daftar penulis
query GetAuthors {
  authors {
    id
    name
  }
}

# Metode spesifik untuk mencari penulis
query findAuthors {
  search_authors(args: {search: "Einstein"}) {
    id
    name
  }
}
```

| **S2** | Penyaringan model |

**Contoh:**
Mengambil daftar artikel yang diterbitkan tahun ini:
```graphql
query articlesThisYear {
  articles(where: {publishDate: {_gt: "2024-01-01"}}) {
    id
    name
  }
}
```

| **S3** | Pengurutan model |

**Contoh:**
Mengambil daftar artikel yang diurutkan berdasarkan tanggal publikasi secara terbalik:
```graphql
query sortedArticles {
  article(order_by: {publishDate: desc}) {
    id
    title
    author_id
  }
}
```

| **S4** | Paginasi model |

**Contoh:**
Melakukan paginasi pada daftar dengan 20 objek per halaman dan mengambil halaman ke-3:
```graphql
query sortedArticlesThirdPage {
  article(order_by: {publishDate: desc}, offset: 40, limit: 20) {
    id
    title
    author_id
  }
}
```

| **S5** | Agregasi model pada bidang tertentu |

**Contoh:**
Mengambil jumlah penulis dan rata-rata usia mereka:
```graphql
query authorStatistics {
  author_aggregate {
    aggregate {
      count # dukungan agregasi dasar pada model apa pun
      avg { # dukungan pada bidang numerik
        age
      }
    }
  }
}
```

### Referensi Sebelumnya
- **Panduan Desain API Google Cloud:**
  - *Resource*: API yang berorientasi pada sumber daya dimodelkan sebagai hierarki sumber daya.
  - *Method*: Sumber daya dimanipulasi melalui sekumpulan metode kecil.

---

## Komposabilitas

API Supergraph biasanya adalah API GraphQL/JSON. Berbagai tingkat komposabilitas API dijelaskan di tabel berikut:

### Atribut Komposabilitas dan Kemampuannya

| Atribut Komposabilitas | Kemampuan | Deskripsi |
|-------------------------|-----------|-----------|
| **C1** | Menggabungkan data | Menggabungkan data terkait seperti "foreign key" join |

**Contoh:**
Mengambil daftar penulis dan artikel mereka:
```graphql
query authorWithArticles {
  author {
    id
    name
    articles {
      id
      title
    }
  }
}
```

| **C2** | Penyaringan bersarang | Menyaring induk berdasarkan properti entitas anak |

**Contoh:**
Mengambil daftar penulis yang telah menerbitkan artikel tahun ini:
```graphql
query recentlyActiveAuthors {
  author(where: {articles: {publishDate: {_gt: "2024-01-01"}}}) {
    id
    name
  }
}
```

| **C3** | Pengurutan bersarang | Mengurutkan induk berdasarkan properti entitas anak |

**Contoh:**
Mengambil daftar artikel yang diurutkan berdasarkan nama penulisnya:
```graphql
query sortedArticles {
  article(order_by: {author: {name: asc}}) {
    id
    title
  }
}
```

| **C4** | Paginasi bersarang | Mengambil daftar induk dan anak dengan paginasi |

**Contoh:**
Mengambil halaman kedua daftar penulis dan halaman pertama artikel mereka yang diurutkan berdasarkan judul:
```graphql
query paginatedAuthorsWithSortedPaginatedArticles {
  author(offset: 10, limit: 20) {
    id
    name
    articles(offset: 0, limit: 25, order_by: {title: asc}) {
      title
      publishDate
    }
  }
}
```

| **C5** | Agregasi bersarang | Mengagregasi anak/induk dalam konteks induk/anak |

**Contoh:**
Mengambil daftar penulis dan jumlah artikel yang ditulis oleh masing-masing:
```graphql
query prolificAuthors {
  author(limit: 10) {
    id
    name
    articles_aggregate {
      count
    }
  }
}
```

Atribut-atribut komposabilitas ini meningkatkan kemampuan untuk melakukan komposisi mandiri dan mengurangi kebutuhan akan agregasi manual.
