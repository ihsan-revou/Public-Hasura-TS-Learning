# Panduan Arsitektur

## Sistem CI/CD dan Build (Control Plane)
Control plane dari supergraph sangat penting untuk memungkinkan pemilik domain menghubungkan domain mereka ke supergraph dengan lancar. Sistem ini memastikan bahwa domain tetap terintegrasi dan tersinkronisasi saat terjadi perubahan.

### Komponen pada Control Plane
Terdapat tiga komponen utama dalam control plane supergraph:

1. **Domain Itu Sendiri**
   - Merupakan sumber utama data atau API.

2. **Subgraph**
   - Berfungsi sebagai konektor perantara yang menghubungkan domain dengan supergraph.

3. **Supergraph**
   - Lapisan integrasi utama yang menggabungkan beberapa subgraph untuk menyediakan platform akses data terpadu dan komposisi API.

### Komponen dalam Control Plane Supergraph
Untuk menjaga sinkronisasi antara supergraph dan domain yang mendasarinya, control plane harus mendefinisikan dan mengelola proses Software Development Life Cycle (SDLC) berikut:

- **Kontrol Versi**: Untuk melacak perubahan dan memastikan kompatibilitas ke belakang.
- **Continuous Integration dan Continuous Deployment (CI/CD)**: Mengotomatiskan pembaruan pada subgraph saat domain berkembang.
- **Pengujian dan Validasi**: Untuk mencegah perubahan yang merusak dan menjaga kontrak API tetap berkualitas tinggi.

## Data Plane Terdistribusi
Data plane terdistribusi dari supergraph sangat penting untuk memberikan akses berperforma tinggi ke domain upstream. Hal ini memastikan bahwa:

- **Produsen API** dapat memelihara domain mereka secara efisien tanpa beban pemeliharaan tak terduga di masa depan.
- **Metrik Performa** seperti latensi, ukuran payload, dan konkurensi dioptimalkan untuk pengguna akhir.

Dengan menyeimbangkan konektivitas, performa, dan skalabilitas, data plane terdistribusi mendukung operasi supergraph yang lancar, menjadikannya arsitektur yang kokoh dan berkelanjutan.
