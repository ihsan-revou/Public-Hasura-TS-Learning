# Pengantar Linux

Linux adalah sistem operasi open-source berbasis UNIX yang banyak digunakan di server, perangkat tertanam, dan desktop. Linux dikenal karena stabilitas, keamanan, dan fleksibilitasnya.

---

## 1. Struktur Direktori Linux

Linux memiliki struktur direktori yang hierarkis. Berikut adalah beberapa direktori penting:

| Direktori | Deskripsi                              |
|-----------|----------------------------------------|
| `/`       | Root directory, tempat semua file dimulai |
| `/home`   | Direktori untuk pengguna non-root       |
| `/var`    | Menyimpan file log dan data runtime     |
| `/etc`    | File konfigurasi sistem                |
| `/usr`    | Aplikasi pengguna dan library          |

### Ilustrasi Struktur Direktori Linux

![image](https://github.com/user-attachments/assets/bc248d47-b187-4210-a5b5-a44651f8e97e)


---

## 2. Perintah Dasar Linux

Berikut adalah beberapa perintah dasar yang sering digunakan:

| Perintah   | Fungsi                                   | Contoh                         |
|------------|------------------------------------------|--------------------------------|
| `ls`       | Menampilkan isi direktori               | `ls /home`                    |
| `cd`       | Pindah ke direktori tertentu            | `cd /var/log`                 |
| `mkdir`    | Membuat direktori baru                  | `mkdir new_folder`            |
| `rm`       | Menghapus file atau direktori           | `rm file.txt`                 |
| `cp`       | Menyalin file atau direktori            | `cp file.txt /backup/`        |
| `mv`       | Memindahkan atau mengganti nama file    | `mv file.txt newfile.txt`     |
| `chmod`    | Mengubah izin file atau direktori       | `chmod 755 script.sh`         |

---

## 3. Manajemen Proses

Linux memungkinkan manajemen proses melalui perintah seperti:

| Perintah       | Fungsi                                   |
|----------------|------------------------------------------|
| `ps`           | Menampilkan daftar proses               |
| `top`          | Menampilkan proses secara real-time     |
| `kill`         | Menghentikan proses berdasarkan PID     |
| `htop`         | Alternatif `top` dengan antarmuka interaktif |

### Contoh Tampilan `htop`
Tambahkan gambar dari tampilan terminal `htop`.

---

## 4. Manajemen Paket

### Debian/Ubuntu
Menggunakan `apt` untuk mengelola paket:
- **Update indeks paket:** `sudo apt update`
- **Instal paket:** `sudo apt install paket-name`
- **Hapus paket:** `sudo apt remove paket-name`

### Red Hat/CentOS
Menggunakan `yum` atau `dnf`:
- **Instal paket:** `sudo yum install paket-name`
- **Hapus paket:** `sudo yum remove paket-name`

---

## 5. Sistem Pengguna

Linux memiliki sistem pengguna yang kuat. Berikut adalah beberapa perintah:
- **Tambah pengguna:** `sudo adduser nama_pengguna`
- **Ganti password:** `passwd`
- **Hapus pengguna:** `sudo userdel nama_pengguna`

### Gambar Sistem Login
Tambahkan ilustrasi antarmuka login Linux.

---

## 6. Contoh Bash Script Sederhana

Bash adalah shell di Linux yang memungkinkan penulisan script untuk mengotomatisasi tugas.

```bash
#!/bin/bash
# Script sederhana untuk mencetak "Hello, Linux!"
echo "Hello, Linux!"
```

Simpan script ini dengan nama hello.sh dan buat dapat dieksekusi:

```bash
chmod +x hello.sh
./hello.sh
```

## 7. Referensi
- [Dokumentasi Ubuntu](https://ubuntu.com/)
- [The Linux Foundation](https://www.linuxfoundation.org/)
