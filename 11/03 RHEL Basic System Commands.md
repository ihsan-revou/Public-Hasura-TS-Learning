# Dokumentasi Perintah Dasar Sistem di RHEL

## Pengantar
RHEL menyediakan ratusan perintah yang dapat digunakan untuk berbagai keperluan, dari yang sederhana hingga kompleks. Perintah-perintah ini dapat dikombinasikan untuk membentuk struktur yang inovatif sesuai kebutuhan. Beberapa perintah memiliki sedikit opsi, sementara lainnya dapat memiliki lebih dari 70 opsi. Dokumentasi ini memberikan pemahaman tentang cara membentuk perintah serta demonstrasi penggunaan perintah-perintah dasar yang umum digunakan di Linux.

---

## Memulai Sesi Terminal Jarak Jauh
Untuk menjalankan perintah di prompt terminal, Anda memerlukan akses ke sesi terminal. Berikut adalah representasi prompt perintah untuk masuk ke sistem sebagai pengguna root:
  
![image](https://github.com/user-attachments/assets/d43028df-d921-47f1-9f9f-d559883a6605)

  
```bash
[root@server1 ~]#
```

- **[root@server1 ~]#:** Menampilkan username yang sedang login (contoh: root), hostname sistem (contoh: server1), dan lokasi direktori saat ini (contoh: ~).
- Akhiran prompt:
  - `#`: Root user.
  - `$`: Pengguna biasa.

Perintah diketik di posisi kursor dan dijalankan dengan menekan tombol Enter.

---

## Memahami Mekanisme Perintah
Format dasar perintah Linux adalah:

```bash
command option(s) argument(s)
```

- **Option (opsional):** Mengubah perilaku perintah. 
  - Format pendek: diawali dengan `-` (contoh: `-l`, `-a`).
  - Format panjang: diawali dengan `--` (contoh: `--all`).
- **Argument (opsional/mandatory):** Target untuk perintah tersebut.

Contoh struktur perintah:

- `#ls`: Tanpa opsi, argumen default adalah direktori saat ini.
- `#ls -l`: Satu opsi, argumen default adalah direktori saat ini.
- `#ls -al`: Dua opsi, tidak ada argumen eksplisit; argumen defaultnya adalah nama direktori saat ini.
- `#ls --all`: Satu opsi, tidak ada argumen eksplisit; argumen defaultnya adalah nama direktori saat ini.
- `#ls -l directory_name`: Satu opsi dan satu argumen eksplisit.

---

## Perintah Dasar Linux

### 1. **Menampilkan File dan Direktori (ls)**
Perintah `ls` digunakan untuk menampilkan daftar file dan direktori. Berikut beberapa opsi umum:

| Opsi | Deskripsi |
|------|-----------|
| `-a` | Menampilkan file/direktori tersembunyi. |
| `-l` | Menampilkan daftar panjang dengan informasi detail. |
| `-ld` | Menampilkan detail direktori tanpa isi. |
| `-lh` | Menampilkan ukuran file dalam format ramah manusia. |
| `-lt` | Menyortir berdasarkan tanggal dan waktu (terbaru di atas). |
| `-ltr` | Menyortir berdasarkan tanggal dan waktu (terlama di atas). |
| `-R` | Menampilkan isi direktori dan subdirektori secara rekursif. |

**Contoh:**

- `ls`: Menampilkan daftar file di direktori saat ini dengan asumsi Anda berada di direktori /root.

  ![image](https://github.com/user-attachments/assets/c13334a1-e1d7-4eb6-b11d-3a8181087e71)

- `ls -l`: Menampilkan daftar panjang file/direktori di direktori saat ini.

  ![image](https://github.com/user-attachments/assets/38cf2d0c-f97a-482c-95a6-681c93488136)

  
- `ls -la`: Menampilkan semua file (termasuk tersembunyi) dengan detail.

  ![image](https://github.com/user-attachments/assets/02a2e60b-d148-46d4-8ce0-8be23355a354)

  
- `ls -lh`: Menampilkan ukuran file dalam format yang lebih mudah dipahami.

![image](https://github.com/user-attachments/assets/b0f8aaaf-0255-46f9-b24a-f9639158c9d6)


- `ls -lt`: Menyortir berdasarkan tanggal dan waktu (terbaru di atas).

![image](https://github.com/user-attachments/assets/9f7c64d9-576d-4d96-afb8-a0c10cc5a03d)


- `ls -R`: Menampilkan isi direktori dan subdirektori secara rekursif.
  
![image](https://github.com/user-attachments/assets/20a8ba54-90e6-41fc-9939-72d6895ebedb)


Gunakan `man ls` untuk melihat manual perintah `ls` beserta semua opsinya.

---

### 2. **Menampilkan Lokasi Kerja Saat Ini (pwd)**
Perintah `pwd` menampilkan lokasi direktori saat ini.

**Contoh:**

```bash
pwd
```

![image](https://github.com/user-attachments/assets/b273d667-4b57-4b59-b021-196a9ceddfc0)

  
Output menunjukkan jalur absolut direktori tempat Anda berada.

---

### 3. **Navigasi Direktori (cd)**
Gunakan perintah `cd` untuk berpindah antar direktori.

- **Jalur absolut:** Dimulai dengan `/`.
- **Jalur relatif:** Tidak dimulai dengan `/`.

**Contoh:**

- `cd ..`: Naik satu tingkat ke direktori induk.

![image](https://github.com/user-attachments/assets/27e1f265-e642-4d80-983b-3408c3e103c1)


Urutan perintah di atas menampilkan lokasi saat ini (/root output dari pwd), lalu naik satu tingkat (cd.. relatif terhadap lokasi saat ini), dan akhirnya verifikasi lokasi baru (/ keluaran pwd). Kita mungkin ingin menggunakan jalur absolut (cd /) alih-alih (cd ..) untuk menuju ke bagian atas pohon direktori (direktori induk dari /root).
  
- `cd /etc/sysconfig`: Berpindah ke direktori sysconfig di /etc: gunakan path absolut (/etc/sysconfig) atau relatif (etc/sysconfig).

![image](https://github.com/user-attachments/assets/28310265-e859-4e40-a737-ba9fef7a74b0)
  

- `cd /usr/bin`: Berpindah ke direktori `/usr/bin`.

![image](https://github.com/user-attachments/assets/499346ff-91ac-46c5-a181-0378c9e8fd69)


- `cd ~`: Kembali ke direktori home.

![image](https://github.com/user-attachments/assets/73fb45bf-5685-4d18-ab29-81b22b3a79f4)


- `cd -`: Berpindah antara direktori saat ini dan sebelumnya.
  
![image](https://github.com/user-attachments/assets/e15227da-6e07-42c1-956b-c289c0355c82)
  
  Contoh menunjukkan penggunaan karakter tanda hubung (-) dengan perintah cd untuk beralih antara direktori saat ini dan sebelumnya.

---

### 4. **Identifikasi File Perangkat Terminal (tty)**
Perintah `tty` digunakan untuk mengidentifikasi terminal aktif saat ini.

**Contoh:**

```bash
tty
```
  
![image](https://github.com/user-attachments/assets/72b05b72-bdbf-4aa3-8699-b7cc96433aee)


Output menunjukkan file perangkat terminal aktif, misalnya `/dev/pts/0`.

---

### 5. **Melihat Uptime dan Beban CPU (uptime)**
Perintah `uptime` menampilkan waktu sistem, durasi aktif, jumlah pengguna, dan rata-rata beban CPU.

**Contoh:**

```bash
uptime
```
  
![image](https://github.com/user-attachments/assets/56fa1ac9-b914-4ace-98fa-6f9fa476849d)


Output:
- Waktu sistem.
- Durasi aktif.
- Jumlah pengguna login.
- Rata-rata beban CPU dalam 1, 5, dan 15 menit terakhir.

---

### 6. **Membersihkan Layar Terminal (clear)**
Perintah `clear` membersihkan layar terminal.

  ![image](https://github.com/user-attachments/assets/c91a11f9-1f6d-4ae4-8648-376820e11c8f)


**Alternatif:** Gunakan shortcut `Ctrl+l`.

---

### 7. **Menentukan Jalur Perintah (which, whereis, type)**
Untuk mengetahui jalur absolut suatu perintah:

**Contoh:**

```bash
which ls
whereis ls
type ls
```

![image](https://github.com/user-attachments/assets/72a9511e-7b9c-4c76-9768-98b3c8c3528d)

  
Semua perintah di atas akan menunjukkan lokasi file eksekusi perintah `ls`, misalnya `/usr/bin/ls`.

---

### 8. **Melihat Informasi Sistem (uname)**
Perintah `uname` menampilkan informasi sistem dasar.

**Contoh:**

- `uname`: Menampilkan nama OS.
- `uname -a`: Menampilkan semua informasi sistem.
  
![image](https://github.com/user-attachments/assets/5638c6b7-12ef-424a-81cd-628e88c2b864)


**Informasi yang diberikan:**
- Nama kernel.
- Hostname.
- Rilis kernel.
- Waktu build kernel.
- Nama perangkat keras.
- Tipe prosesor.
- Nama OS.

---

### 9. **Melihat Spesifikasi CPU (lscpu)**
Perintah `lscpu` menampilkan informasi arsitektur CPU, mode operasi, vendor, model, kecepatan, cache, dan dukungan virtualisasi.

**Contoh:**

```bash
lscpu
```
  
![image](https://github.com/user-attachments/assets/77e1f556-8d32-4223-8aa3-142453084ed5)


Output mencakup:
- Arsitektur CPU.
- Mode operasi (32-bit/64-bit).
- Model CPU.
- Kecepatan CPU (MHz).
- Ukuran dan level cache (L1d, L1i, L2, L3).

---

**Dokumentasi sampai pada `Viewing CPU Specs` halaman: 116**
