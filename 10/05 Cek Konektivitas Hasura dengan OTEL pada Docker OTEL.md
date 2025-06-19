# Cek Konektivitas Hasura dengan OTEL pada Docker Log OTEL

Kali ini kita akan melakukan pengecekan terhadap Docker OTEL. Apakah Docker OTEL telah terhubung dengan Hasura. Sebelum itu, kita akses terlebih dahulu server dimana Docker OTEL dibangun.
  
![image](https://github.com/user-attachments/assets/7b11d9c3-0099-4634-b6f5-8d34fbe76f55)

Setelah berhasil mengakses server. Kita masuk ke langkah-langkah untuk melakukan cek Konetivitas OTEL dengan Hasura menggunakan Docker Log. Ikuti langkah-langkah berikut:

## 1. Cek nama container docker OTEL terlebih dahulu.

Kita bisa cek nama container docker OTEL menggunakan command `docker ps`.

```bash
docker ps
``` 

![image](https://github.com/user-attachments/assets/719b6330-39d8-4974-9a7c-fb0e314742ab)
  


## 2. Jalankan log Docker OTEL.
  
Setelah melakukan pengecekan nama container, selanjutnya kita lakukan pengecekan melalui log dengan perintah `docker logs`.

```bash
docker logs NAMA_ATAU_ID_CONTAINER
```

  


  
