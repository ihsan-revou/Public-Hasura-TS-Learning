# Hit Elastic Menggunakan Postman
  
Kita melakukan hit elastic mengunakan Postman dengan cara menjalankan Perintah Put dan juga Post.


1. **Berikut untuk penggunaan perintah Put pada postman.**

   Ini bertujuan untuk membuat directory (index) baru pada Index Management Elastic.
   Lakukan hit dengan alamat 10.100.13.25:9200/nama-indexmu -> Klik Send
   Sebelum klik `SEND` terlebih dahulu set
     
   ![image](https://github.com/user-attachments/assets/b360251f-7ef8-47a7-8b73-30f4d00f1527)

3. **Dan berikut untuk penggunaan perintah Post pada postman.**

   Ini berfungsi untuk menambahkan dokumen pada index. Tambahkan body untuk isi dari dokumen. Dalam hal ini isi dokumen berupa file json.

  ```json
  {
      "title": "Belajar Elasticsearch",
      "content": "Menggunakan Elasticsearch dengan Postman",
      "date": "2024-12-06"
  }
  ```
   
  ![image](https://github.com/user-attachments/assets/1e8fe147-f368-4e1d-9585-db8ab0ffeb32)


3. **Apabila sudah berhasil melakukan hit akan terbaca pada bagian Data -> Index Management, seperti berikut ini.**

  ![image](https://github.com/user-attachments/assets/4d1337ff-3dde-47e4-98d8-356cda191f5f)
