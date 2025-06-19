---
# Operasi File dan Direktori
Bagian ini menjelaskan berbagai operasi manajemen yang dapat dilakukan pada file dan direktori. Operasi ini mencakup pembuatan, menampilkan isi, menyalin, memindahkan, mengganti nama, dan menghapus file serta direktori. Operasi ini dapat dilakukan oleh pengguna biasa yang memiliki hak akses yang sesuai. Pengguna root dapat melakukan tugas-tugas ini pada file atau direktori apa pun dalam sistem, terlepas dari siapa pemiliknya. Jika terjadi kekurangan hak akses, pesan error akan ditampilkan.

---
## Membuat File dan Direktori
File dapat dibuat dengan berbagai cara menggunakan perintah yang berbeda; namun, hanya ada satu perintah untuk membuat direktori.

### Membuat File Kosong Menggunakan `touch`
Perintah `touch` digunakan untuk membuat file kosong. Jika file sudah ada, perintah ini hanya memperbarui timestamp-nya agar sesuai dengan tanggal dan waktu sistem saat ini. Jalankan perintah berikut sebagai root di direktori home pengguna root untuk membuat file1, lalu gunakan perintah `ls` untuk memverifikasinya:

![image](https://github.com/user-attachments/assets/8dc95ed0-5eb8-4762-b4b3-eecc035d704b)

  
Kolom ke-5 (kolom ukuran) dalam output menunjukkan nilai 0, yang berarti file1 dibuat dengan ukuran nol byte. Jika Anda menjalankan kembali perintah `touch` pada file ini setelah beberapa menit, timestamp baru akan diterapkan padanya.

![image](https://github.com/user-attachments/assets/e123c2d5-17ed-4482-8722-eec04f4044e5)

  
Perintah `touch` memiliki beberapa opsi menarik. Opsi `-d` dan `-t` memungkinkan Anda untuk mengatur tanggal dan waktu tertentu pada file; opsi `-a` dan `-m` memungkinkan Anda mengubah waktu akses atau modifikasi file ke waktu sistem saat ini; dan opsi `-r` mengatur waktu modifikasi file berdasarkan file referensi. Contoh:
- Untuk mengatur tanggal file1 ke 20 September 2019:
  
  ![image](https://github.com/user-attachments/assets/f78739bd-a4c8-45d8-8a92-a6f250d41c1f)

- Untuk mengubah waktu modifikasi file1 ke waktu sistem saat ini:
  
  ![image](https://github.com/user-attachments/assets/b08b2961-d8a3-4fb0-8be3-11bb9ce95c81)


### Membuat File Pendek Menggunakan `cat`
Perintah `cat` memungkinkan Anda membuat file teks pendek. Tanda sudut tutup `>` harus digunakan untuk mengalihkan output ke file yang ditentukan (misalnya catfile1):

  ![image](https://github.com/user-attachments/assets/e5146404-503f-4384-9ed5-7ac72204a965)


Saat Anda menjalankan perintah tersebut, sistem akan menunggu input dari Anda. Ketikkan beberapa teks. Tekan tombol Enter untuk membuka baris baru dan lanjutkan mengetik. Setelah selesai, tekan Ctrl+d untuk menyimpan teks di catfile1 dan kembali ke prompt perintah. Verifikasikan pembuatan file dengan perintah `ls`.

### Membuat File Menggunakan `vim`
Anda dapat menggunakan editor `vim` untuk membuat dan memodifikasi file teks dalam ukuran apa pun. Lihat bagian sebelumnya dalam bab ini untuk panduan menggunakan `vim`.

### Membuat Direktori Menggunakan `mkdir`
Perintah `mkdir` digunakan untuk membuat direktori. Perintah ini akan menampilkan output jika dijalankan dengan opsi `-v`. Contoh berikut menunjukkan pembuatan direktori bernama dir1 di direktori home pengguna root:

![image](https://github.com/user-attachments/assets/38863dbe-f0fb-4f9f-a5ab-58ec8e6e4b82)


Anda dapat membuat hierarki subdirektori dengan opsi `-p` (parent). Contoh:
  
```bash
mkdir -p dir2/perl/perl5
```

![image](https://github.com/user-attachments/assets/3573f3f5-0bbc-444f-b915-5a8d6b3a3fb3)

  
Perhatikan penempatan opsi dalam dua contoh ini. Banyak perintah di Linux menerima format serupa.

---
## Menampilkan Isi File
RHEL menawarkan berbagai alat untuk menampilkan isi file. Isi direktori hanyalah file dan subdirektori yang dikandungnya. Gunakan perintah `ls` untuk melihat isi direktori. Untuk melihat isi file, Anda dapat menggunakan perintah seperti `cat`, `more`, `less`, `head`, dan `tail`.

### Menggunakan `cat`
`cat` digunakan untuk menampilkan isi file teks. Biasanya digunakan untuk file pendek. Contoh:
  
```bash
cat .bash_profile
```

![image](https://github.com/user-attachments/assets/df486645-afb3-4e67-9794-3e65de1d322c)

  
Anda dapat menambahkan opsi `-n` untuk melihat output dalam format bernomor.

### Menggunakan `tac`
`tac` menampilkan isi file teks secara terbalik. Contoh:
```bash
tac .bash_profile
```

![image](https://github.com/user-attachments/assets/ae8abfb4-8b94-4086-8ced-c02fb656aa28)

  
### Menggunakan `less` dan `more`
`less` dan `more` adalah filter teks untuk melihat file teks panjang satu halaman dalam satu waktu. `less` lebih canggih dibandingkan `more`. Contoh:
  
```bash
less /etc/profile
more /etc/profile
```

![image](https://github.com/user-attachments/assets/2679f0d5-2232-4caf-a131-0d05a6f28d9e)

  
Navigasi menggunakan tombol seperti di bawah:

| Tombol        | Fungsi                              |
|---------------|-------------------------------------|
| Spasi / `f`   | Scroll maju satu layar             |
| Enter         | Scroll maju satu baris             |
| `b`           | Scroll mundur satu layar           |
| `d`           | Scroll maju setengah layar         |
| `h`           | Menampilkan bantuan                |
| `q`           | Keluar                             |
| `/string`     | Mencari string maju                |
| `?string`     | Mencari string mundur (hanya `less`)|
| `n`           | Temukan string berikutnya          |
| `N`           | Temukan string sebelumnya (hanya `less`) |

Jika `/usr/bin/znew` tidak tersedia, gunakan `/etc/profile` sebagai gantinya.
  
### Menggunakan `head` dan `tail`
`head` menampilkan beberapa baris awal dari file teks, defaultnya 10 baris pertama. Contoh:
  
```bash
head /etc/profile
```

![image](https://github.com/user-attachments/assets/d6f0c07a-2c3e-4b0c-af27-947d2fea2e52)

  
`tail` menampilkan 10 baris terakhir secara default. Contoh:
  
```bash
tail /etc/profile
```

![image](https://github.com/user-attachments/assets/7aaf40bd-f0af-49e1-a206-99e0dec50884)

  
Untuk melihat pembaruan file log secara real-time, gunakan opsi `-f` pada `tail`.

![image](https://github.com/user-attachments/assets/eda8e3d2-62a4-4d13-b372-ff7908bb901a)

  
---
## Menghitung Kata, Baris, dan Karakter pada File Teks
Perintah `wc` (word count) digunakan untuk menampilkan jumlah baris, kata, dan karakter dalam file teks. Contoh:
  
```bash
wc /etc/profile
```

![image](https://github.com/user-attachments/assets/99b6cde6-6965-4f0e-8223-0c1039da8344)

  
Opsi yang tersedia:

| Opsi | Aksi                                |
|------|-------------------------------------|
| `-l` | Menampilkan jumlah baris            |
| `-w` | Menampilkan jumlah kata             |
| `-c` | Menampilkan jumlah byte             |
| `-m` | Menampilkan jumlah karakter         |

---
## Menyalin File dan Direktori
Operasi penyalinan menduplikasi file atau direktori. Gunakan perintah `cp` dengan berbagai opsinya.

### Menyalin File
Untuk menyalin file dalam direktori yang sama:
  
```bash
cp file1 newfile1
```

![image](https://github.com/user-attachments/assets/5f487112-0816-4867-8ce4-5c7a7e4c49a5)

  
Untuk menyalin file ke direktori lain:
  
```bash
cp file1 dir1/
```

![image](https://github.com/user-attachments/assets/453b669c-bcbe-43c2-97e2-953d22727dbf)

  
Gunakan opsi `-i` untuk meminta konfirmasi sebelum menimpa file:
  
```bash
cp -i file1 dir1/
```

![image](https://github.com/user-attachments/assets/edd01f78-ab4c-42dc-8214-c3f0eee0a9d0)

  
### Menyalin Direktori
Gunakan opsi `-r` untuk menyalin seluruh pohon direktori. Contoh:
  
```bash
cp -r dir1 dir2
```

![image](https://github.com/user-attachments/assets/e4b21c15-59e6-4e32-9811-d0480c590236)

  
Opsi `-p` dapat digunakan untuk mempertahankan atribut file atau direktori.

---
## Memindahkan dan Mengganti Nama File dan Direktori
Perintah `mv` digunakan untuk memindahkan atau mengganti nama file dan direktori.

### Memindahkan dan Mengganti Nama File
Contoh memindahkan file:
  
```bash
mv file1 dir1/
```

![image](https://github.com/user-attachments/assets/2097e3af-5600-4906-a285-8e4557359c04)

  
Contoh mengganti nama file:
```bash
mv newfile1 newfile2
```

![image](https://github.com/user-attachments/assets/22baae87-81d4-4833-9088-25becace0906)

  
### Memindahkan dan Mengganti Nama Direktori
Untuk memindahkan direktori:
  
```bash
mv dir1 dir2/
```

![image](https://github.com/user-attachments/assets/836b47da-f038-45c0-88c6-83f5df506215)

  
Untuk mengganti nama direktori:
  
```bash
mv dir2 dir20
```

![image](https://github.com/user-attachments/assets/ff3041f2-2d74-45cd-9b5c-f8bfd1f12f70)

  
---
## Menghapus File dan Direktori
Gunakan perintah `rm` untuk menghapus file atau direktori.

### Menghapus File
Contoh:
  
```bash
rm newfile2
```

![image](https://github.com/user-attachments/assets/d163d872-5d54-461b-8fac-337929e9f636)

  
Untuk meminta konfirmasi sebelum menghapus, gunakan opsi `-i`.

### Menghapus Direktori
Gunakan opsi `-r` untuk menghapus direktori beserta isinya:
  
```bash
rm -r dir1
```

![image](https://github.com/user-attachments/assets/ebc5ef24-fa0a-43ac-8efb-a0f0b6074d22)

  
**Dokumentasi dari halaman 142 sampai pada terakhir `Removing Files and Directories` Halaman:151**
