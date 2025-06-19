---
# **File Linking**
Setiap file dalam sistem file memiliki banyak atribut yang ditetapkan pada saat pembuatan. Atribut-atribut ini disebut sebagai metadata file, yang akan berubah ketika file diakses atau dimodifikasi. Metadata file mencakup beberapa informasi seperti jenis file, ukuran, izin akses, nama pemilik, nama grup pemilik, waktu akses/modifikasi terakhir, jumlah link, jumlah blok yang dialokasikan, dan pointer ke lokasi penyimpanan data. Metadata ini memerlukan 128 byte ruang untuk setiap file. Ruang kecil ini disebut sebagai inode (index node) file.

Inode diberikan pengenal numerik unik yang digunakan oleh kernel untuk mengakses, melacak, dan mengelola file. Untuk mengakses inode dan data yang ditunjuknya, sebuah nama file diberikan untuk mengenali dan mengaksesnya. Pemetaan antara inode dan nama file disebut sebagai **link**. Penting untuk dicatat bahwa inode tidak menyimpan nama file dalam metadata-nya; pemetaan antara nama file dan nomor inode disimpan dalam metadata direktori tempat file berada.

Menghubungkan file atau direktori akan membuat instance tambahan dari file atau direktori tersebut, tetapi semuanya pada akhirnya menunjuk ke lokasi data fisik yang sama dalam pohon direktori. File yang dihubungkan mungkin memiliki nomor inode dan metadata yang identik atau berbeda tergantung pada jenis link yang digunakan.

Terdapat dua cara untuk membuat link file dan direktori di RHEL (Red Hat Enterprise Linux), yaitu **hard link** dan **soft link**. Link dapat dibuat antara file atau direktori, tetapi **tidak** antara file dan direktori.

---
## **Hard Link**
**Hard link** adalah pemetaan antara satu atau lebih nama file dengan nomor inode yang sama, membuat semua file yang di-hard link tidak dapat dibedakan satu sama lain. Hal ini berarti semua file yang di-hard link memiliki metadata yang sama. Perubahan pada metadata file dan isinya dapat dilakukan melalui salah satu nama file.

**Contoh:** Dua nama file—`file10` dan `file20`—keduanya berbagi nomor inode **10176147**. Masing-masing nama file pada dasarnya adalah hard link yang menunjuk ke inode yang sama.

![image](https://github.com/user-attachments/assets/4c913e3e-bb7a-4e88-b58a-ec31531005c0)

  

### **Karakteristik Hard Link:**
1. Hard link **tidak dapat** melintasi batas sistem file.
2. Hard link **tidak dapat** digunakan untuk menghubungkan direktori karena pembatasan dalam Linux untuk menghindari masalah dengan beberapa perintah.

### **Contoh Perintah Hard Link:**
Untuk membuat file kosong bernama `file10` dan hard link bernama `file20` di direktori yang sama:
  
```bash
$ touch file10
$ ln file10 file20
$ ls -li
```

![image](https://github.com/user-attachments/assets/008e0fbb-6eb1-4d7e-ae2e-18fa40a8db4c)  
![image](https://github.com/user-attachments/assets/4c1a23da-f82f-4734-a823-d0d22e108b3a)

  
**Keluaran:**
Kolom **1** menampilkan nomor inode yang sama (**10176147**), sedangkan kolom **3** menampilkan jumlah link dari file yang di-hard link (misalnya `file10` menunjuk ke `file20` dan sebaliknya). Jika file asli (`file10`) dihapus, data masih dapat diakses melalui `file20`.

Setiap kali Anda menambahkan hard link ke file yang sudah ada, jumlah link akan bertambah 1. Jika hard link dihapus, jumlah link akan berkurang 1. Ketika semua hard link dihapus, jumlah link akan menjadi 0.

---
## **Soft Link**
**Soft link** (dikenal juga sebagai **symbolic link** atau **symlink**) memungkinkan satu file dikaitkan dengan file lain. Konsep ini mirip dengan **shortcut** di Microsoft Windows, di mana file sebenarnya berada di suatu tempat dalam struktur direktori, tetapi bisa ada satu atau lebih shortcut dengan nama berbeda yang menunjuk ke file tersebut.

Dengan soft link, Anda dapat mengakses file melalui nama asli maupun shortcut. Setiap soft link memiliki nomor inode unik yang menyimpan **path** menuju file yang ditautkan. Untuk symlink, jumlah link tidak bertambah atau berkurang; setiap file yang di-symlink mendapatkan nomor inode baru.

![image](https://github.com/user-attachments/assets/34ddfe86-a054-4599-a265-715a463d71b0)

  

### **Karakteristik Soft Link:**
1. Soft link **dapat melintasi** batas sistem file.
2. Soft link **dapat digunakan** untuk menghubungkan direktori karena hanya menyimpan path ke objek tujuan.
3. Ukuran soft link adalah jumlah karakter dalam path ke target file.

### **Contoh Perintah Soft Link:**
Untuk membuat soft link untuk `file10` dengan nama `soft10` di direktori yang sama:
  
```bash
$ ln -s file10 soft10
$ ls -l
```

![image](https://github.com/user-attachments/assets/302d8e40-46e8-4edf-b027-019981353304)  

  
**Keluaran:**
- Huruf **`l`** di kolom kedua menandakan bahwa `soft10` adalah symbolic link.
- Tanda panah (`->`) menunjukkan file asli yang ditautkan.

  ![image](https://github.com/user-attachments/assets/f16f1012-4568-406d-8106-9ed50a905f93)

Jika file asli (`file10`) dihapus, link `soft10` akan tetap ada tetapi menunjuk ke file yang tidak ada (broken link).

**Direktori Soft Link di RHEL 8:**
RHEL 8 memiliki empat direktori soft link di bawah `/`.

![image](https://github.com/user-attachments/assets/04a06489-5da5-4a6f-9fa9-22ebc5773b69)

  
---
## **Perbedaan antara Copying dan Linking**
Terdapat perbedaan utama antara operasi **copying** dan **linking**. Tabel berikut menjelaskan kapan harus menggunakan copy atau link (hard link dan soft link):

| **Copying**                         | **Linking**                                                                                                                                 |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| Membuat duplikat file sumber. Jika salah satu file diubah, file lain tidak akan terpengaruh. | Membuat shortcut yang menunjuk ke file sumber. File sumber dapat diakses atau dimodifikasi melalui file sumber maupun link.               |
| Setiap file hasil copy menyimpan datanya sendiri di lokasi yang unik.    | Semua file yang di-link menunjuk ke data yang sama.                                                                                        |
| Setiap file hasil copy memiliki nomor inode dan metadata unik.           | **Hard Link**: Semua file yang di-hard link memiliki nomor inode dan metadata yang sama. <br> **Symlink**: Memiliki inode unik, hanya menyimpan path ke sumber. |
| Jika file hasil copy dipindah, dihapus, atau diganti namanya, file sumber tidak terpengaruh. | **Hard Link**: Jika salah satu file dihapus, file lain dan datanya tetap ada. <br> **Symlink**: Jika sumber dihapus, link menjadi broken. |
| Copy digunakan ketika data perlu diedit secara independen.               | Link digunakan ketika akses ke sumber yang sama dibutuhkan dari beberapa lokasi.                                                          |
| Izin pada sumber dan hasil copy dikelola secara independen.              | Izin dikelola pada file sumber.                                                                                                           |

**Tabel 3-14: Copying vs Linking**

---
## **Kesimpulan**
Perbedaan-perbedaan ini penting untuk dipahami ketika Anda perlu memutuskan apakah akan menggunakan operasi **copy** atau **link** (hard link atau soft link). Pilih pendekatan yang paling sesuai dengan kebutuhan Anda berdasarkan tujuan dan karakteristik file atau direktori yang ingin dihubungkan.

---

**Dokumentasi dari halaman 151 sampai pada terakhir `Differences between Copying and Linking` Halaman:155**
