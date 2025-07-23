# Desain API Lengkap - Aplikasi Pinjaman Online

---

## 1. Autentikasi & Pengguna

### `POST /auth/register`

Mendaftarkan pengguna baru sesuai alur **Register Screen**.

| Properti | Keterangan |
| :--- | :--- |
| **Method** | `POST` |
| **URL** | `/api/v1/auth/register` |
| **Auth** | Tidak perlu |

#### Request Body
```json
{
  "name": "Refda anugrah",
  "email": "refda.anugrah@example.com",
  "phone_number": "081234567890",
  "password": "PasswordSuperKuat123"
}
```
#### Success Response (201 Created)
```json
{
  "message": "Registrasi berhasil, silakan lanjutkan upload dokumen.",
  "data": {
    "user_id": "a1b2c3d4-e5f6-7890-1234-567890abcdef"
  }
}
```
#### Failed Response (400 Bad Request)
```json
{
  "status": "error",
  "code": "BAD_REQUEST",
  "message": "Permintaan tidak valid. Pastikan semua data yang dibutuhkan telah diisi dengan benar."
}
```
#### Failed Response (422 Unprocessable Entity)
```json
{
  "status": "error",
  "code": "VALIDATION_ERROR",
  "message": "Data yang diberikan tidak valid.",
  "errors": {
    "email": [
      "Alamat email ini sudah terdaftar."
    ],
    "password": [
      "Password minimal harus 8 karakter."
    ]
  }
}
```

## 1. Autentikasi & Pengguna
