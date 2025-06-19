# Struktur Direktori dan File pada Linux

## Sistem

File-file Linux diorganisasi secara logis dalam bentuk hierarki untuk mempermudah administrasi dan pengenalan. Organisasi ini dipertahankan dalam ratusan direktori yang berada di wadah besar bernama sistem file. Red Hat Enterprise Linux mengikuti **Filesystem Hierarchy Standard (FHS)** untuk organisasi file, yang menggambarkan nama, lokasi, dan izin untuk berbagai jenis file dan direktori.

Sistem file Linux berisi file dan subdirektori. Subdirektori, yang juga disebut direktori anak, berada di bawah direktori induk. Struktur direktori Linux dianalogikan sebagai pohon terbalik, di mana akar pohon adalah root dari direktori, cabang adalah subdirektori, dan daun adalah file. Root direktori direpresentasikan oleh karakter garis miring maju (`/`). Garis miring juga digunakan sebagai pemisah direktori dalam sebuah path, contohnya: `/etc/rc.d/init.d/functions`.

Pada contoh di atas:
- Direktori `etc` adalah anak dari root (`/`).
- Direktori `rc.d` adalah anak dari `etc`.
- Direktori `init.d` adalah anak dari `rc.d`.
- File `functions` adalah daun dari `init.d`.

Setiap direktori memiliki direktori induk dan anak, kecuali root (tidak memiliki induk) dan subdirektori tingkat terendah (tidak memiliki anak).

**Catatan:** Istilah subdirektori digunakan untuk direktori yang memiliki direktori induk.
  
---

## Direktori Tingkat Atas
 
The term subdirectory is used for a directory that has a parent directory.

![tempsnip](https://github.com/user-attachments/assets/4958c8e5-f600-4135-9ae4-c3b379bc397d)

  
Direktori tingkat atas utama di bawah `/` ditampilkan pada **Gambar di atas**. Beberapa direktori ini menyimpan data statis, sedangkan yang lain menyimpan informasi dinamis. 

- **Data statis**: Konten file yang tetap tidak berubah kecuali dimodifikasi secara eksplisit.
- **Data dinamis/variabel**: Konten file yang diperbarui sesuai kebutuhan proses sistem.

Direktori statis biasanya berisi perintah, file konfigurasi, rutinitas pustaka, file kernel, dan file perangkat. Sedangkan direktori dinamis menyimpan file log, status, file sementara, dll.

### Kategori Sistem File

RHEL mendukung berbagai jenis sistem file yang dapat dikategorikan menjadi tiga grup utama:
1. **Berbasis Disk**: Dibuat di media fisik seperti hard drive atau USB flash drive.
2. **Berbasis Jaringan**: Sistem file berbasis disk yang dibagikan melalui jaringan untuk akses jarak jauh.
3. **Berbasis Memori**: Virtual; dibuat saat sistem startup dan dihapus saat sistem mati.

Sistem file berbasis disk dan jaringan menyimpan informasi secara persisten, sedangkan data di sistem file berbasis memori hilang saat reboot.

### Sistem File Root (`/`), Berbasis Disk

Root direktori adalah sistem file tingkat atas dalam FHS dan berisi banyak direktori tingkat lebih tinggi yang menyimpan informasi spesifik. Beberapa direktori kunci:
- **`/etc`**: Menyimpan file konfigurasi sistem.
- **`/root`**: Lokasi direktori home untuk pengguna root.
- **`/mnt`**: Digunakan untuk mount sistem file sementara.

### Sistem File Boot (`/boot`), Berbasis Disk

Direktori `/boot` berisi kernel Linux, file pendukung boot, dan file konfigurasi boot.

### Direktori Home (`/home`)

Direktori `/home` dirancang untuk menyimpan direktori home pengguna dan konten pengguna lainnya.

### Direktori Opsional (`/opt`)

Direktori `/opt` digunakan untuk menyimpan perangkat lunak tambahan yang mungkin perlu diinstal pada sistem.

### Direktori Sumber Daya Sistem UNIX (`/usr`)

Berisi sebagian besar file sistem. Subdirektori penting:
- **`/usr/bin`**: Perintah yang dapat dieksekusi pengguna.
- **`/usr/sbin`**: Perintah administrasi sistem.
- **`/usr/lib` & `/usr/lib64`**: Pustaka bersama.
- **`/usr/local`**: Tempat menyimpan perintah dan alat yang diunduh atau dikembangkan.
- **`/usr/share`**: Menyimpan manual, dokumentasi, dll.

### Direktori Variabel (`/var`)

Direktori `/var` berisi data yang sering berubah saat sistem berjalan. Subdirektori penting:
- **`/var/log`**: Menyimpan file log sistem.
- **`/var/spool`**: Menyimpan antrian pekerjaan.
- **`/var/tmp`**: File sementara yang bertahan lebih lama.

### Direktori Sementara (`/tmp`)

Direktori `/tmp` digunakan untuk menyimpan file sementara yang dibuat oleh program.

### Sistem File Perangkat (`/dev`), Virtual

Direktori `/dev` menyimpan node perangkat untuk perangkat keras fisik dan virtual.

### Sistem File Procfs (`/proc`), Virtual

Direktori `/proc` digunakan untuk menyimpan informasi tentang status kernel yang sedang berjalan.

### Sistem File Runtime (`/run`), Virtual

Direktori `/run` menyimpan data proses yang sedang berjalan di sistem.

### Sistem File (`/sys`), Virtual

Direktori `/sys` menyimpan informasi tentang perangkat keras, driver, dan fitur kernel tertentu.

---

## Melihat Hierarki Direktori

Perintah `tree` digunakan untuk menampilkan hierarki direktori dan file. Beberapa opsi umum:

| Opsi | Deskripsi |
|------|-----------|
| `-a` | Menampilkan file tersembunyi |
| `-d` | Hanya menampilkan direktori |
| `-h` | Menampilkan ukuran file dalam format ramah manusia |
| `-f` | Menampilkan path lengkap untuk setiap file |
| `-p` | Menampilkan izin file |

Contoh penggunaan perintah `tree`:
1. Menampilkan hanya direktori di direktori home root:
   ```bash
   tree -d /root
   ```

  ![image](https://github.com/user-attachments/assets/5a48cfb9-8bd0-4f79-9b12-a77c9af020d5)

   
2. Menampilkan file di direktori `/etc/sysconfig` dengan izin, ukuran, dan path lengkap:
   ```bash
   tree -p -h -f /etc/sysconfig
   ```

   ![image](https://github.com/user-attachments/assets/dfe25ef4-a1c6-472d-84a7-b42879e1a5f9)


Jalankan `man tree` untuk melihat manual perintah `tree`.
  
**Dokumentasi terakhir `Viewing Directory Hierarchy` Halaman:106**
