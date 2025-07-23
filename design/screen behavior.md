# 📱 Screen Behavior

---

## 🟢 Welcome Screen

**Tujuan:**  
Layar penyambut awal yang memberikan opsi kepada pengguna untuk **"Login"** atau **"Register"**.

---

## 📝 Register Screen

**Tujuan:**  
Form untuk pengguna baru memasukkan data diri seperti:
- Nama
- Email
- Nomor HP
- Password

**API Terkait:**  
`POST /auth/register`

---

## 📤 Upload Document Screen

**Tujuan:**  
Layar khusus bagi pengguna untuk mengunggah:
- Foto KTP
- Selfie

(dilakukan setelah proses pendaftaran berhasil)

**API Terkait:**  
`POST /documents/upload`

---

## 🔐 Login Screen

**Tujuan:**  
Halaman bagi pengguna untuk masuk ke dalam aplikasi.  
Fitur yang tersedia:
- Form Email & Password
- Tombol login biometrik (fingerprint/face ID)

**API Terkait:**  
- `POST /auth/login`  
- `POST /auth/login/biometric`

---

## 🏠 Dashboard Screen

**Tujuan:**  
Layar utama setelah login.  
Tampilan akan disesuaikan berdasarkan status pinjaman pengguna:
- Tidak memiliki pinjaman aktif
- Sedang memiliki pinjaman berjalan

**API Terkait:**  
`GET /loans/summary`

---

## 🧾 Loan Application Screen (Form Pengajuan)

**Tujuan:**  
Form pengajuan pinjaman, di mana pengguna dapat:
- Memasukkan jumlah pinjaman
- Memilih tenor (jangka waktu pinjaman)

**API Terkait:**  
`POST /loans/apply`

---

## 📄 Loan Detail Screen (Tampilan Detail Pinjaman)

**Tujuan:**  
Menampilkan rincian lengkap pinjaman aktif, termasuk:
- Sisa utang
- Jadwal cicilan berikutnya
- Riwayat pembayaran

**API Terkait:**  
`GET /loans/{id}/installments`  
> *Catatan: Tambahan API yang perlu disiapkan.*

---
