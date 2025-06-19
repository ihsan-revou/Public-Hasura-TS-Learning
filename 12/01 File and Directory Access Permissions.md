---
## Manajemen File Tingkat Lanjut

Izin ditetapkan pada file dan direktori untuk mencegah akses dari pengguna yang tidak sah. Pengguna dikelompokkan ke dalam tiga kategori berbeda. Setiap kategori pengguna kemudian diberikan izin yang diperlukan. Izin dapat diubah menggunakan salah satu dari dua metode yang tersedia. Masker pengguna dapat ditentukan untuk pengguna individu sehingga file dan direktori baru yang mereka buat selalu mendapatkan izin yang sudah ditentukan sebelumnya. Setiap file di Linux memiliki pemilik dan grup.

RHEL menawarkan tiga bit izin tambahan untuk mengontrol akses pengguna ke file yang dapat dieksekusi tertentu dan direktori bersama. Direktori dengan salah satu bit ini dapat digunakan untuk kolaborasi grup. Direktori yang dapat ditulis secara publik atau grup juga dapat dikonfigurasi dengan salah satu bit ini untuk mencegah penghapusan file oleh pengguna yang bukan pemiliknya.

Terdapat alat di RHEL yang sangat membantu dalam mencari file di lokasi yang ditentukan menggunakan berbagai opsi untuk menentukan kriteria pencarian. Alat ini dapat dikonfigurasi untuk menjalankan aksi pada file output yang ditemukan.

**Access Control Lists (ACL)** memungkinkan administrator menerapkan atribut keamanan tambahan pada file dan direktori untuk pengguna atau grup pengguna tertentu. Atribut ini diterapkan di atas izin akses standar Linux untuk pemilik file dan anggota grup. Direktori dapat memiliki pengaturan ACL default yang diterapkan untuk memungkinkan berbagi konten antar pengguna tanpa harus mengubah izin pada setiap file dan subdirektori yang dibuat di dalamnya.

---
## Izin Akses File dan Direktori

Linux adalah sistem operasi multi-pengguna yang memungkinkan ratusan pengguna untuk masuk dan bekerja secara bersamaan. Selain itu, OS memiliki ratusan ribu file dan direktori yang harus dijaga keamanannya untuk memastikan operasi sistem dan aplikasi yang sukses dari sudut pandang keamanan. Mengingat faktor-faktor ini, sangat penting untuk mengatur akses pengguna ke file dan direktori serta memberikan hak yang sesuai agar mereka dapat menjalankan fungsinya tanpa membahayakan keamanan sistem. Kontrol izin pada file dan direktori ini juga disebut sebagai **hak akses pengguna**.

---
## Menentukan Izin Akses

Izin akses pada file dan direktori memungkinkan kontrol administratif atas pengguna (kelas izin) yang dapat mengaksesnya dan pada tingkat apa (jenis izin). Izin file dan direktori yang dibahas di bagian ini disebut sebagai izin standar **ugo/rwx**.

---
## Kelas Izin

Pengguna dikategorikan ke dalam tiga kelas unik untuk menjaga keamanan file melalui hak akses. Kelas ini adalah:
- **User (u)**: Pemilik file.
- **Group (g)**: Sekelompok pengguna dengan kebutuhan akses yang sama.
- **Other (o)**: Semua pengguna lain di sistem (publik).

Terdapat juga kelas pengguna khusus yang disebut **all (a)**, yang mewakili kombinasi dari ketiga kelas pengguna di atas.

---
## Jenis Izin

Izin mengontrol tindakan apa yang dapat dilakukan pada file atau direktori, serta oleh siapa. Ada tiga jenis bit izin — **read (r)**, **write (w)**, dan **execute (x)** — yang memiliki perilaku berbeda untuk file dan direktori:
- **File**:
  - **Read (r)**: Melihat dan menyalin file.
  - **Write (w)**: Memodifikasi file.
  - **Execute (x)**: Menjalankan file.
- **Direktori**:
  - **Read (r)**: Melihat konten dengan perintah `ls`.
  - **Write (w)**: Membuat, menghapus, dan mengganti nama file atau subdirektori.
  - **Execute (x)**: Memasuki direktori dengan perintah `cd`.

Jika izin **read**, **write**, atau **execute** tidak diinginkan, karakter tanda hubung (**-**) digunakan untuk merepresentasikan ketiadaan izin tersebut.

---
## Mode Izin

Mode izin digunakan untuk **menambahkan (+)**, **mencabut (-)**, atau **menetapkan (=)** jenis izin pada kelas izin. Anda dapat melihat pengaturan izin pada file dan direktori dengan perintah **ls -l** dalam tampilan panjang. Informasi ini terdapat di kolom pertama output, contohnya:

```
- rwx rw- r--
```

- Karakter pertama menunjukkan tipe file:
  - `-` : File reguler.
  - `d` : Direktori.
  - `l` : Tautan simbolik.
  - `c` : File perangkat karakter.
  - `b` : File perangkat blok.
  - `p` : Named pipe.
  - `s` : Socket.

- Sembilan karakter berikutnya (dibagi menjadi tiga kelompok) menunjukkan izin **read (r)**, **write (w)**, dan **execute (x)** untuk kelas pengguna:
  - **User** (pemilik file).
  - **Group** (grup pengguna).
  - **Other** (publik).

Tanda hubung **-** digunakan untuk menunjukkan penolakan izin pada tingkat tertentu.

---
## Mengubah Bit Izin Akses

Perintah **chmod** digunakan untuk mengubah hak akses. Perintah ini berfungsi sama pada file dan direktori. **chmod** hanya dapat digunakan oleh root atau pemilik file, dan dapat mengubah izin dengan dua cara:
- **Notasi simbolik**: Menggunakan kombinasi huruf (**ugo/rwx**) dan simbol (**+**, **-**, **=**) untuk menambahkan, mencabut, atau menetapkan bit izin.
- **Notasi oktal**: Menggunakan sistem penomoran tiga digit (0-7) untuk mengekspresikan izin. Nilai oktal dijelaskan dalam Tabel di bawah ini:

| Nilai Oktal | Notasi Biner | Notasi Simbolik | Penjelasan              |
|------------|-------------|----------------|-------------------------|
| 0          | 000         | ---            | Tanpa izin              |
| 1          | 001         | --x            | Izin eksekusi           |
| 2          | 010         | -w-            | Izin tulis              |
| 3          | 011         | -wx            | Izin tulis dan eksekusi |
| 4          | 100         | r--            | Izin baca               |
| 5          | 101         | r-x            | Izin baca dan eksekusi  |
| 6          | 110         | rw-            | Izin baca dan tulis     |
| 7          | 111         | rwx            | Izin baca, tulis, dan eksekusi |

**Tabel Notasi Izin Oktal**

Dalam Tabel  diatas, setiap **1** mewakili **r**, **w**, atau **x**, dan setiap **0** mewakili tanda hubung (**-**) yang menunjukkan ketiadaan izin. Gambar di bawah menunjukkan bobot yang terkait dengan setiap posisi digit dalam model penomoran oktal tiga digit:

![image](https://github.com/user-attachments/assets/81e35b1a-e606-4603-8952-ea637add1408)

  
**Gambar Bobot Izin**

- Posisi paling kanan memiliki bobot **1**.
- Posisi tengah memiliki bobot **2**.
- Posisi paling kiri memiliki bobot **4**.

Sebagai contoh, jika kita menetapkan izin **6**, ini akan sesuai dengan posisi kiri dan tengah (4 + 2). Demikian pula, izin **2** hanya menunjukkan posisi tengah.
