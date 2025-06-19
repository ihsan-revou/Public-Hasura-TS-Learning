# Cek Konektivitas Hasura dengan OTEL pada Pod Hasura

1. **Buka Rancher**
2. **Buka Kubernetes**
3. **Masuk ke pod Hasura yang ingin kalian cek konektivitasnya dengan OTEL**
4. **Lalu klik titik tiga pada ujung pods dan pilih `Execute Shell`**
5. **Kita kaan diarahkan pada tampilah seperti berikut**
  ![WhatsApp Image 2024-12-06 at 14 40 45_44310103](https://github.com/user-attachments/assets/7cbcaf22-356e-4479-991d-5bec5a7f58ed)
6. **Jalankan command untuk cek konektivitas**
   
   ```bash
   nc -vz 10.100.13.239 4318
   ```

  10.100.13.239 adalah endpoint otel sedangkan 4318 adalah port otelnya. Lebih tepatnya seperti pada gambar dibawah ini.

  ![WhatsApp Image 2024-12-06 at 14 39 50_49f10f15](https://github.com/user-attachments/assets/2be398b9-a266-41b8-b573-6b5dd1bf559c)


  Response `nc: connect to 10.100.13.239 port 4318 (tcp) failed: Connection refused` menandakan Hasura belum berhasil terhubung dengan OTEL. Sedangkan jika berhasil responsenya adalah seperti beriku.

  ![Capture (1)](https://github.com/user-attachments/assets/c4a7579a-4702-4768-863e-e78bb4ecc42c)

