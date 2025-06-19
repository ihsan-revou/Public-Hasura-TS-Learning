# Strategi dan Konsep Inti
Pendekatan supergraph bertujuan untuk membangun "flywheel" akses dan pasokan data guna meningkatkan akses mandiri (self-service) ke data dan API secara bertahap.

## Flywheel Platform Supergraph

![image](https://github.com/user-attachments/assets/99f0d7aa-b555-44aa-bc4f-f32f249501ca)


### I. **CONNECT Domains**
Pemilik domain (atau pemilik data, atau produsen API) harus dapat menghubungkan domain mereka ke platform dengan mudah. Tantangan utama dalam membangun supergraph adalah resistensi terhadap perubahan dari pemilik domain, yang sering kali tidak ingin harus membangun, mengoperasikan, dan memelihara lapisan API tambahan seperti GraphQL server. Kekhawatiran ini valid dan harus ditangani dengan strategi platform supergraph dan arsitektur referensi supergraph.

Dua implikasi utama untuk siklus hidup dan runtime konektor subgraph:

1. **CI/CD Konektor Subgraph**:
   - Ketika pemilik domain mengubah domain mereka, kontrak API yang diterbitkan melalui supergraph engine harus tetap sinkron dengan overhead minimal bagi pemilik domain.
   - Proses SDLC, manajemen perubahan, atau CI/CD pemilik domain harus mencakup pembaruan kontrak API mereka (misalnya: versioning), mencegah perubahan yang merusak, dan menjaga dokumentasi tetap mutakhir.

2. **Performa Konektor Subgraph**:
   - Konektor subgraph tidak boleh mengurangi performa dibandingkan dengan akses langsung ke domain yang mendasarinya.
   - Karakteristik performa API diukur melalui latensi, ukuran payload, dan konkurensi.

Menjamin proses CI/CD yang lancar dan konektivitas berkinerja tinggi memberikan kepercayaan kepada pemilik domain bahwa mereka dapat menghubungkan domain mereka ke platform supergraph dan mengubah domain mereka tanpa rasa takut.

Ini membuka konektivitas mandiri bagi pemilik domain.

### II. **CONSUME APIs**
Konsumen API harus dapat menemukan dan menggunakan API tanpa memerlukan integrasi manual, agregasi, atau komposisi API sebanyak mungkin. Konsumen API memiliki beberapa kebutuhan umum saat menangani endpoint API tetap atau kueri data tertentu:

- Mengambil proyeksi data yang berbeda untuk mencegah over-fetching.
- Menggabungkan data dari berbagai tempat untuk mencegah under-fetching.
- Memfilter, mempaginasi, mengurutkan, dan mengagregasi data dari berbagai tempat.

Untuk memberikan pengalaman API yang benar-benar mandiri, ada dua persyaratan utama:

1. **Desain API yang Dapat Dikomposisi**:
   - API yang disajikan oleh supergraph engine harus memungkinkan komposisi sesuai permintaan. GraphQL adalah API yang hebat untuk mengekspresikan semantik komposisi, tetapi terlepas dari format API yang digunakan, desain API yang terstandar dan dapat dikomposisi adalah persyaratan kritis.

2. **Portal API**:
   - Pencarian, penemuan, dan dokumentasi berkualitas tinggi untuk API dan model API yang mendasarinya sangat penting untuk memungkinkan konsumsi mandiri.
   - Semakin banyak informasi yang tersedia bagi konsumen API, semakin baik. Contohnya: Data lineage, kebijakan otorisasi, dll.

Ini membuka konsumsi mandiri bagi konsumen API.

### III. **DISCOVER Demand**
Memahami bagaimana konsumen API menggunakan domain mereka dan mengidentifikasi kebutuhan yang belum terpenuhi sangat penting bagi produsen API. Wawasan ini memungkinkan produsen API untuk meningkatkan domain mereka dan menemukan pemilik domain baru untuk menghubungkan domain mereka ke supergraph.

Hal ini memerlukan dua kemampuan utama dari platform supergraph untuk menciptakan budaya yang berpusat pada konsumen dan gesit:

1. **Analitik Konsumsi API, Skema API, dan Portal**:
   - Supergraph mirip dengan pasar (marketplace) dan perlu memberikan wawasan kepada pemilik dan produsen pasar untuk membantu meningkatkan pasar bagi konsumen.

2. **Integrasi Ekosistem**:
   - Platform supergraph harus dapat berintegrasi dengan alat komunikasi dan katalog yang ada, terutama untuk membantu memahami permintaan yang tidak terpenuhi dari konsumen API.

Ini menutup siklus dan memungkinkan platform supergraph menciptakan siklus keberhasilan yang berkelanjutan bagi produsen dan konsumen.
