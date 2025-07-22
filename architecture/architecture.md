# Penjelasan Arsitektur Sistem

Arsitektur sistem ini dirancang menggunakan pendekatan **Microservices** untuk memastikan **skalabilitas**, **kemudahan pengembangan**, serta **pemeliharaan jangka panjang**.

---

## 🧭 Alur Permintaan

1. **Pengguna** (dari aplikasi **Mobile** atau **Web**) mengirimkan permintaan ke **Load Balancer**.
2. **Load Balancer** akan meneruskan trafik ke **API Gateway**.
3. **API Gateway** bertindak sebagai **pintu masuk tunggal (single entry point)** yang aman dan akan meneruskan permintaan ke layanan terkait.

---

## 🔀 Routing & Layanan

API Gateway akan mengarahkan setiap permintaan ke service yang sesuai:

- **Authentication Service**  
  Menangani proses login dan validasi token JWT.

- **User Service**  
  Mengelola data pengguna seperti registrasi, update profil, dll.

- **Loan Service**  
  Menjalankan logika bisnis utama terkait:
  - Pengajuan pinjaman
  - Persetujuan pinjaman
  - Status dan histori pinjaman

---

## 📤 Proses Upload Dokumen

Untuk mengunggah dokumen seperti **KTP**, sistem menggunakan **pre-signed URL**:

1. Client meminta izin upload ke **backend**.
2. Backend memberikan **pre-signed URL** untuk akses aman ke **Cloud Storage (S3)**.
3. Client mengunggah file langsung ke S3 melalui URL tersebut.
4. Setelah sukses, backend menyimpan **referensi file** ke dalam **database**.

---

## 🔔 Notifikasi & Logging

- Setiap service dapat memicu **notifikasi** ke pengguna, misalnya saat pinjaman disetujui.
- Semua aktivitas permintaan dicatat oleh sistem **logging** untuk kebutuhan **monitoring** dan **audit trail**.

---

## 🗄️ Penyimpanan Data

- Setiap service memiliki **database tersendiri** (per service database).
- Pendekatan ini mendukung:
  - **Independensi antar layanan**
  - **Ketahanan sistem**
  - **Isolasi kesalahan (fault tolerance)**

---

## 🧩 Ringkasan Komponen Utama

| Komponen             | Fungsi                                                                 |
|----------------------|------------------------------------------------------------------------|
| Load Balancer        | Mendistribusikan trafik masuk                                          |
| API Gateway          | Pintu masuk utama dan pengatur routing                                 |
| Authentication Svc   | Login, token, dan otorisasi                                            |
| User Svc             | Registrasi dan pengelolaan profil pengguna                             |
| Loan Svc             | Pengajuan dan manajemen pinjaman                                       |
| Cloud Storage (S3)   | Penyimpanan file dokumen yang diunggah pengguna                        |
| Logging System       | Mencatat semua aktivitas sistem untuk keperluan debugging dan audit    |
| Notification System  | Mengirim pesan dan status ke pengguna                                 |
| Database per Service | Menjamin desentralisasi dan skalabilitas data                          |

---

> 🛠️ Arsitektur ini mendukung pertumbuhan aplikasi jangka panjang dengan mengutamakan keamanan, fleksibilitas, dan efisiensi pengembangan.
