# Stress Test Menggunakan Postman

1. **Buka Postman**

2. **Lakukan Hit Hasura GraphQL menggunakan Postman**

Cara melakukan hit hasura grpahql menggunakan postman telah dibahas sebelumnya. Lebih jelasnya bisa merujuk pada dokumentasi [DISINI](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning-Public/blob/main/04/02%20Remote%20Schema%20Hasura%20dan%20Connect%20to%20Postman.md#menghubungkan-hasura-ke-postman-menggunakan-graphql)

3. **Save**
  
Setelah itu save terlebih dahulu dengan mengklik button save dipojok kanan atas. Jika sudah diklik kita akan diarahkan ke tamplian berikut.

![image](https://github.com/user-attachments/assets/9440da8c-1c57-469e-9282-692ff5955510)

Pilih direktori/folder collection tempat disimpannya konfigurasi HTTP/GraphQL. lalu klik save.

4. **Run Collection**

Pilih folder collection tempat disimpannya konfigurasi HTTP/GraphQL di sebelah kiri.   
Lalu klik kanan pada collection, kita akan menemukan pilihan `run collection`.   
Klik `run collection`  
Lebih jelasnya seperti gambar berikut.
  
![image](https://github.com/user-attachments/assets/77d6d576-ce34-402a-b20f-0e9749b11aee)

Kita akan diarahkan pada tampilan runner seperti berikut.
  
![image](https://github.com/user-attachments/assets/24a386f4-3428-46fc-906c-36d74853ec03)

Kita bisa pilih query yang mana aj yang mau kita jalankan secara berulang dengan mencentang box query yang sebelah kiri.

![image](https://github.com/user-attachments/assets/317e83d9-14a2-4b08-b294-01c9bca6311b)

Atur iterasi yaitu perulangan hitnya atau tepatnya berapa kali mau kita lakukan hit. Setelah itu, atur delay yaitu waktu tunggu atau jeda setelah melakukan sekali hit. Perhatikan gambar berikut lebih jelasnya.
  
![image](https://github.com/user-attachments/assets/b41fbbd8-4685-46df-8d85-bfd9a17d4f3f)

Pilih `run manually` untuk melakukan run tanpa schedule dan automatic.

![image](https://github.com/user-attachments/assets/bf0ebeb6-0120-4046-b06d-d18b96dc9bb0)

Jika sudah dirasa cukup untuk mengatur konfigurasinya, jalankan stress test dengan klik button `run new collection`. Tampilan sedang dijalankan adalah sebagai berikut.

![image](https://github.com/user-attachments/assets/94421b30-3c04-4760-bdb3-0137cf6d43ec)

Stop runner dengan klik button `stop run`.

![image](https://github.com/user-attachments/assets/c24f8f6b-b407-4954-b6bb-bc7cb4fe808e)

Terdapat informasi mengenai jumlah iterasi yang sudah berjalan serta duration, dan average respone timenya ketika selesai atau telah melakuakn runner.  
