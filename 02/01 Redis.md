# Redis: Server Struktur Data

Redis sering disebut sebagai server struktur data. Ini berarti Redis menyediakan akses ke struktur data yang dapat diubah melalui serangkaian perintah, yang dikirim menggunakan model server-klien dengan soket TCP dan protokol sederhana. Proses yang berbeda dapat melakukan kueri dan memodifikasi struktur data yang sama secara bersama-sama.

## Properti Khusus Struktur Data Redis:

1. **Redis menyimpan data di disk**, meskipun data selalu disajikan dan dimodifikasi di dalam memori server. Ini membuat Redis cepat dan juga non-volatil.
2. **Efisiensi memori**: Struktur data dalam Redis diimplementasikan dengan efisiensi memori, yang berarti struktur data di Redis akan cenderung menggunakan lebih sedikit memori dibandingkan model struktur data yang sama dalam bahasa pemrograman tingkat tinggi.
3. **Fitur mirip basis data**: Redis menawarkan fitur seperti replikasi, tingkat durabilitas yang dapat diatur, clustering, dan ketersediaan tinggi.

## Fitur Redis Lainnya:

Redis dapat dianggap sebagai versi lebih kompleks dari Memcached, di mana operasi bukan hanya sekadar SET dan GET, tetapi operasi yang bekerja dengan tipe data kompleks seperti List, Set, dan struktur data terurut.

### Contoh Penggunaan Redis:

Berikut adalah beberapa perintah Redis yang umum digunakan:

```bash
redis> set foo bar
OK
redis> get foo
"bar"
redis> incr mycounter
(integer) 1
redis> incr mycounter
(integer) 2
```

## Redis Community Edition

Redis OSS berganti nama menjadi Redis Community Edition (CE) dengan rilis v7.4. Redis Ltd. juga menawarkan Redis Software untuk skala enterprise dan Redis Cloud sebagai layanan terkelola di Google Cloud, Azure, dan AWS.

## Cara Menggunakan Redis

Setelah Redis dibangun, Anda bisa menjalankannya dengan perintah:

```bash
% cd src
% ./redis-server
```

Jika Anda ingin memberikan konfigurasi khusus, jalankan perintah berikut:

```bash
% ./redis-server /path/to/redis.conf
```

## Perintah Redis CLI

Redis CLI digunakan untuk bermain-main dengan Redis. Cobalah menjalankan beberapa perintah berikut setelah Redis berjalan:

```bash
% cd src
% ./redis-cli
redis> ping
PONG
```

Anda dapat menemukan daftar perintah Redis yang tersedia di [sini](https://redis.io/commands).

## Instalasi Redis

Untuk menginstal Redis ke direktori `/usr/local/bin`, cukup gunakan perintah:

```bash
% make install
```

Anda juga bisa menginstal Redis di direktori lain dengan menggunakan:

```bash
% make PREFIX=/some/other/directory install
```

## Contribusi Kode

Jika Anda berkontribusi ke proyek Redis, semua kontribusi Anda harus tunduk pada lisensi Redis. Redis Software menggunakan lisensi RSALv2/SSPL untuk semua versi Redis Community Edition mulai dari v7.4.x dan seterusnya.

## Struktur Kode Redis

Berikut adalah penjelasan singkat tentang struktur direktori Redis:

- **src**: Implementasi Redis dalam bahasa C.
- **tests**: Tes unit Redis, ditulis dalam Tcl.
- **deps**: Library yang digunakan oleh Redis.
- **utils**: Berisi skrip untuk menjalankan tes Redis dan melakukan instalasi.

File utama dalam Redis adalah `server.c` yang berisi fungsi `main()` dan berbagai fungsi lain yang bertanggung jawab untuk menjalankan server Redis, seperti `initServerConfig()` dan `initServer()`.

### Daftar Fungsi Penting Redis:

- `createClient()`: Alokasi dan inisialisasi klien baru.
- `addReply()`: Menambahkan data balasan ke struktur klien.
- `readQueryFromClient()`: Membaca data dari klien dan memprosesnya.
- `freeClient()`: Menghapus klien dan membebaskan sumber daya yang digunakan.

## Replikasi dan AOF (Append Only File)

Redis mendukung replikasi master-replica dan penyimpanan durabel menggunakan file AOF. Proses dump data ke disk dilakukan oleh proses terpisah yang dibuat menggunakan `fork()`.

Fungsi penting dalam replikasi adalah `replicationFeedSlaves()` yang mengirimkan perintah ke replika agar dataset tetap sinkron dengan master.
