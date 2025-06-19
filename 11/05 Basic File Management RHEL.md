# Basic File Management

Linux mendukung berbagai jenis file yang diidentifikasi berdasarkan jenis data yang disimpan. File-file ini bisa berupa teks biasa atau format biner, yang merupakan jenis file paling umum. Ada juga file lain yang menyimpan informasi perangkat atau hanya menunjuk ke data yang sama di disk. Pemahaman yang baik tentang jenis-jenis file Linux penting bagi pengguna dan administrator Linux.

## Jenis File Umum di Linux
RHEL mendukung tujuh jenis file:
1. **File reguler**
2. **Direktori**
3. **Block special device file**
4. **Character special device file**
5. **Symbolic link**
6. **Named pipe**
7. **Socket**

Dua jenis pertama adalah yang paling umum digunakan. File perangkat digunakan oleh sistem operasi untuk berkomunikasi dengan perangkat periferal. Symbolic link sering digunakan, sedangkan named pipe dan socket digunakan untuk komunikasi antar proses.

Linux tidak memerlukan ekstensi file untuk mengidentifikasi jenis file. Perintah `file` dan `stat` dapat digunakan untuk mengetahui jenis data dalam file selain perintah `ls`.

## File Reguler
File reguler dapat berupa teks atau data biner. File ini mungkin berisi skrip shell atau perintah dalam bentuk biner. Saat Anda menjalankan perintah `ls` pada direktori, entri file reguler akan diawali dengan tanda hubung (-). Contohnya:

```bash
-rw-r--r--  1 root root 1234 Jan 1 12:34 contoh.txt
```

![image](https://github.com/user-attachments/assets/d34ef966-372c-4e9c-bd78-4e6af982abfd)


Tanda hubung di kolom pertama menunjukkan file tersebut adalah file reguler. Sekarang, jalankan perintah `file` dan `stat` pada berkas ini dan lihat apa yang dilaporkan:

![image](https://github.com/user-attachments/assets/3b18ee85-e9b9-4e9c-845c-4cb493eee0a6)

  
Perintah `file` dan `stat` dapat digunakan untuk memverifikasi jenis file ini. Kedua perintah melaporkan jenis berkas secara berbeda. Perintah pertama mengembalikan jenis data spesifik yang terdapat dalam berkas (teks ASCII), dan perintah kedua hanya menyatakan bahwa berkas tersebut adalah berkas biasa.

## Direktori
Direktori adalah wadah logis yang berisi file dan subdirektori. Saat Anda menjalankan perintah `ls` pada direktori, entri direktori akan diawali dengan huruf "d". Contohnya:

```bash
drwxr-xr-x  2 root root 4096 Jan 1 12:34 contoh_direktori
```
  
![image](https://github.com/user-attachments/assets/d44becd3-d534-4ad9-9811-e82a3544a56f)
  
  
## File Perangkat (Block dan Character Special)
File perangkat di direktori `/dev` digunakan oleh sistem untuk berkomunikasi dengan perangkat keras. Ada dua jenis file perangkat:

- **Block Device**: Digunakan untuk perangkat yang mentransfer data dalam blok (misalnya, disk).
- **Character Device**: Digunakan untuk perangkat yang mentransfer data secara byte (misalnya, terminal).

Huruf "b" atau "c" di awal kolom pertama menunjukkan jenis file ini. Contohnya:

```bash
brw-r-----  1 root disk 8, 0 Jan 1 12:34 /dev/sda
crw-r-----  1 root tty 4, 0 Jan 1 12:34 /dev/console
```
  
![image](https://github.com/user-attachments/assets/2ab227b7-f3af-4103-b250-19244228c5f2)


Setiap perangkat keras seperti disk, CD/DVD, printer, dan terminal memiliki driver perangkat terkait yang dimuat dalam kernel. Kernel berkomunikasi dengan perangkat keras melalui driver perangkat masing-masing. Setiap driver perangkat diberi nomor unik yang disebut *major number*, yang digunakan kernel untuk mengenali jenisnya. Lebih lanjut, kernel memberikan *minor number* untuk setiap perangkat individual dalam kategori driver perangkat tersebut untuk mengidentifikasinya sebagai perangkat unik. Skema ini juga berlaku untuk partisi disk. Singkatnya, *major number* menunjuk ke driver perangkat, dan *number minor* menunjuk ke perangkat atau partisi unik yang dikendalikan oleh driver perangkat.
  
Dalam daftar panjang di atas, kolom 5 dan 6 menggambarkan asosiasi **major number** dan **minor number** untuk setiap contoh perangkat. Angka utama 8 menunjukkan driver perangkat blok untuk disk SATA. Demikian pula (tidak ditampilkan dalam keluaran di atas), angka utama 253 menunjukkan driver untuk pemeta perangkat (volume VDO, volume logis LVM, dll.), dan 11 menunjukkan perangkat optik.  

## Symbolic Link
Symbolic link (symlink) adalah shortcut ke file atau direktori lain. Saat Anda menjalankan perintah `ls -l` pada symbolic link, entri akan diawali dengan huruf "l" dan akan menunjukkan target link. Contohnya:

```bash
lrwxrwxrwx  1 root root 12 Jan 1 12:34 symlink -> /path/target
```

![image](https://github.com/user-attachments/assets/20f4b54d-9c8f-4dec-ae87-b25f3ac428e5)

  
## Kompresi dan Pengarsipan
Kompresi dan pengarsipan digunakan untuk menghemat ruang disk dan mempercepat proses penyalinan file secara remote. RHEL menyediakan beberapa alat bawaan untuk kebutuhan ini, seperti `gzip`, `bzip2`, dan `tar`.

### Menggunakan gzip dan gunzip
- **gzip**: Digunakan untuk mengompres file, menambahkan ekstensi `.gz` pada file.
- **gunzip**: Digunakan untuk mendekompres file yang dikompres oleh gzip.

**Contoh Pengunaan:**
  
Untuk mengompres file fstab yang terletak di direktori /etc, salin file ini ke direktori home pengguna root /root menggunakan perintah cp dan konfirmasikan dengan ls:

![image](https://github.com/user-attachments/assets/4217e946-5e9c-49e1-bea4-c362ffef5942)

  
Sekarang gunakan perintah gzip untuk mengompres file ini dan ls untuk mengonfirmasi:
  
```bash
# Kompresi file
gzip contoh.txt
```
  
![image](https://github.com/user-attachments/assets/5de9c07a-0613-4ed5-afe6-ac25fc6faca5)

  
Perhatikan bahwa file asli dikompresi, dan sekarang memiliki ekstensi .gz yang ditambahkan ke dalamnya. Jika Anda ingin melihat informasi kompresi untuk file tersebut, jalankan perintah gzip lagi dengan opsi -l:

![image](https://github.com/user-attachments/assets/00441bfd-806f-498f-a8b3-707efeb9470c)

  
Untuk mendekompresi file ini, gunakan perintah gunzip:

```bash
# Dekompresi file
gunzip contoh.txt.gz
```

![image](https://github.com/user-attachments/assets/77cb89ff-c880-47a7-b8ae-f90f78dd9e86)

  
Periksa file setelah dekompresi dengan perintah ls. Ini akan menjadi file yang sama dengan ukuran, stempel waktu, dan atribut lainnya yang sama.

### Menggunakan bzip2 dan bunzip2
- **bzip2**: Mengompres file dengan rasio kompresi yang lebih baik tetapi lebih lambat.
- **bunzip2**: Mendekompres file yang dikompres oleh bzip2.

**Contoh Penggunaan:**

Kompres file fstab lagi, tetapi kali ini dengan bzip2 dan konfirmasi dengan ls:

```bash
# Kompresi file
bzip2 contoh.txt
```
  
![image](https://github.com/user-attachments/assets/8fddedaf-76b5-4c3e-8fbb-e26a86cc11d2)

  
Perhatikan bahwa file asli dikompresi, dan sekarang memiliki ekstensi .bz2.

![image](https://github.com/user-attachments/assets/b06e97db-2a87-41a5-af80-f4f4970a483b)

  
Untuk mendekompresi file ini, gunakan perintah bunzip2:

```bash
# Dekompresi file
bunzip2 contoh.txt.bz2
```

![image](https://github.com/user-attachments/assets/223ed7f2-d7de-493a-9856-fe1dce7e408a)

  
Periksa file setelah dekompresi dengan perintah ls. File tersebut akan menjadi
file yang sama dengan ukuran, stempel waktu, dan atribut lainnya yang sama.

### Perbedaan gzip dan bzip2
- **gzip**: Lebih cepat tetapi dengan rasio kompresi yang lebih rendah.
- **bzip2**: Lebih lambat tetapi dengan rasio kompresi yang lebih tinggi.


### Menggunakan Tar
Perintah tar (tape archive) digunakan untuk membuat, menambahkan, memperbarui, mencantumkan, dan mengekstrak file atau seluruh pohon direktori ke dan dari satu file, yang disebut tarball atau tarfile. Perintah ini juga dapat mengompresi tarball setelah dibuat.
Perintah tar mendukung banyak opsi seperti yang dijelaskan pada Tabel 3-1.

| Opsi | Definisi |
|------|----------|
| -c   | Membuat tarball. |
| -f   | Menentukan nama tarball. |
| -p   | Mempertahankan izin file. Default untuk pengguna root. Tentukan opsi ini jika Anda membuat arsip sebagai pengguna biasa. |
| -r   | Menambahkan file ke akhir tarball yang belum dikompresi. |
| -t   | Mencantumkan isi tarball. |
| -u   | Menambahkan file ke akhir tarball yang belum dikompresi jika file yang ditambahkan lebih baru. |
| -v   | Mode verbose. |
| -x   | Mengekstrak atau mengembalikan dari tarball. |

Opsi -r dan -u tidak mendukung penambahan file ke tarball yang sudah dikompresi.

##### Contoh Penggunaan Tar
1. Membuat tarball bernama `/tmp/home.tar` dari seluruh direktori `/home` dengan opsi verbose (-v) dan menentukan nama arsip (-f):
2. Membuat tarball bernama `/tmp/files.tar` yang hanya berisi beberapa file tertentu dari direktori `/etc`.
3. Menambahkan file dari `/etc/yum.repos.d` ke tarball yang sudah ada `/tmp/home.tar`.
4. Mencantumkan isi tarball `home.tar`.
5. Mengembalikan file tunggal atau semua file dari tarball.

##### Kompresi dengan Tar
Perintah tar juga mendukung opsi untuk langsung mengompres file target menggunakan gzip atau bzip2 seperti yang dijelaskan di Tabel 3-2.

| Opsi | Deskripsi |
|------|-----------|
| -j   | Mengompresi tarball dengan bzip2. |
| -z   | Mengompresi tarball dengan gzip. |

### Menggunakan Vim
Editor vim adalah alat pengeditan teks interaktif yang memungkinkan Anda membuat dan memodifikasi file teks. Alat ini tersedia di semua distribusi Linux dan versi UNIX.

##### Mode Operasi Vim
1. **Command Mode**: Mode default untuk navigasi dan perintah.
2. **Input Mode**: Untuk memasukkan teks. Tekan `Esc` untuk kembali ke Command Mode.
3. **Last Line Mode**: Tekan `:` untuk menjalankan perintah lanjutan.

##### Perintah Dasar Navigasi Vim
| Perintah | Aksi |
|----------|------|
| h        | Gerak mundur satu karakter |
| j        | Gerak ke bawah satu baris |
| k        | Gerak ke atas satu baris |
| l        | Gerak maju satu karakter |
| w        | Ke awal kata berikutnya |
| b        | Ke awal kata sebelumnya |
| e        | Ke akhir kata berikutnya |
| $        | Ke akhir baris saat ini |

##### Menghapus Teks
| Perintah | Aksi |
|----------|------|
| x        | Hapus karakter di posisi kursor |
| dw       | Hapus kata atau bagian kata ke kanan |
| dd       | Hapus baris saat ini |
| :6,12d   | Hapus baris 6 sampai 12 |

##### Menyalin, Memindahkan, dan Menempelkan Teks
vim memungkinkan Anda untuk menyalin teks dan menempelkannya di lokasi yang diinginkan dalam file. Anda dapat menyalin (yank) satu karakter, satu kata, atau seluruh baris, lalu menempelkannya di tempat yang diperlukan. Fungsi salin dapat dilakukan pada beberapa karakter, kata, atau baris sekaligus. Tabel berikut menjelaskan perintah untuk menyalin, memindahkan, dan menempelkan teks.

| Perintah       | Aksi                                                                 |
|----------------|----------------------------------------------------------------------|
| yl             | Menyalin karakter saat ini ke buffer.                               |
| yw             | Menyalin kata saat ini ke buffer.                                   |
| yy             | Menyalin baris saat ini ke buffer.                                  |
| p              | Menempelkan data yang telah disalin di bawah baris saat ini.        |
| P              | Menempelkan data yang telah disalin di atas baris saat ini.         |
| :1,3co6        | Menyalin baris 1 hingga 3 dan menempelkannya setelah baris ke-6.    |
| :4,6m9         | Memindahkan baris 4 hingga 6 setelah baris ke-9.                    |

Anda dapat menambahkan angka sebelum perintah di atas (kecuali untuk perintah mode baris terakhir) untuk mengulangi aksi perintah sebanyak angka tersebut. Misalnya, `2yw` akan menyalin dua kata, `2yy` akan menyalin dua baris, dan `2p` akan menempelkan dua kali.

##### Mengubah Teks
vim menyediakan banyak perintah untuk mengubah dan memodifikasi teks seperti yang dirangkum dalam tabel berikut. Sebagian besar perintah ini akan masuk ke mode edit, jadi Anda perlu menekan tombol `Esc` untuk kembali ke mode perintah.

| Perintah | Aksi                                                                                     |
|----------|------------------------------------------------------------------------------------------|
| cl       | Mengubah karakter di lokasi kursor.                                                     |
| cw       | Mengubah kata (atau sebagian kata) di lokasi kursor hingga akhir kata.                  |
| cc       | Mengubah seluruh baris.                                                                 |
| C        | Mengubah teks dari posisi kursor hingga akhir baris.                                    |
| r        | Mengganti karakter di lokasi kursor dengan karakter yang dimasukkan setelah perintah ini.|
| R        | Menimpa atau mengganti teks pada baris saat ini.                                        |
| J        | Menggabungkan baris berikutnya dengan baris saat ini.                                   |
| xp       | Menukar posisi karakter di lokasi kursor dengan karakter di sebelah kanannya.           |
| ~        | Mengubah huruf besar menjadi kecil, atau sebaliknya, di lokasi kursor.                  |

Anda dapat menambahkan angka sebelum perintah di atas untuk mengulangi aksi perintah sebanyak angka tersebut. Misalnya, `2cc` akan mengubah seluruh baris saat ini dan baris berikutnya, dan `2r` akan mengganti karakter saat ini dan karakter berikutnya.

##### Menyimpan dan Keluar dari vim
Setelah selesai melakukan modifikasi, Anda dapat menyimpan atau membatalkan perubahan. Gunakan salah satu perintah yang tercantum di tabel berikut sesuai kebutuhan.

| Perintah  | Aksi                                                                                  |
|-----------|---------------------------------------------------------------------------------------|
| :w        | Menyimpan perubahan ke file tanpa keluar dari vim.                                   |
| :w file2  | Menyimpan perubahan ke file baru bernama `file2` tanpa keluar dari vim.              |
| :w!       | Menyimpan perubahan ke file meskipun pemilik file tidak memiliki izin tulis.         |
| :wq       | Menyimpan perubahan ke file dan keluar dari vim.                                     |
| :wq!      | Menyimpan perubahan ke file dan keluar dari vim meskipun pemilik file tidak memiliki izin tulis.|
| :q        | Keluar dari vim jika tidak ada modifikasi yang dilakukan.                            |
| :q!       | Keluar dari vim tanpa menyimpan jika ada modifikasi yang dilakukan.                  |

Tanda seru (`!`) dapat digunakan untuk mengabaikan perlindungan tulis pada file bagi pemiliknya.

**Dokumentasi terakhir `Saving and Quitting vim` Halaman:142**


