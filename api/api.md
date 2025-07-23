# Desain API Lengkap - Aplikasi Pinjaman Online

---

## 1. Autentikasi

### `POST /auth/register`

Pendaftaran pengguna baru.

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

## 2. Dokumen Pengguna

### `POST /documents/upload`

Endpoint untuk mengelola unggahan dokumen KTP dan selfie.

| Properti | Keterangan |
| :--- | :--- |
| **Method** | `POST` |
| **URL** | `/api/v1/documents/upload` |
| **Auth** | (Diperlukan) Bearer Token |

#### Request Menggunakan Multipart/form-data
Mengunggah file KTP dan selfie setelah registrasi

#### Request Body
```json
{
"ktp_image": "ktp.jpg",
"selfie_image": "selfie.jpg",
}
```
#### Success Response (201 Created)
```json
{
  "status": "success",
  "message": "Dokumen berhasil diunggah dan sedang menunggu verifikasi."
}
```

#### Failed Response (422 Unprocessable Entity)
````json
{
  "status": "error",
  "code": "VALIDATION_ERROR",
  "message": "Data yang diberikan tidak valid.",
  "errors": {
    "ktp_image": [
      "File harus berupa gambar (jpeg, png, jpg).",
      "Ukuran file tidak boleh lebih dari 2MB."
    ]
  }
}
````


## 3. Pinjaman (Loan)

### `POST /api/v1/loans/apply`
Mengajukan pinjaman baru, sesuai alur Form Pengajuan. Pengguna hanya bisa melakukan ini jika tidak memiliki pinjaman aktif.

| Properti | Keterangan |
| :--- | :--- |
| **Method** | `POST` |
| **URL** | `/api/v1/loans/apply` |
| **Auth** | (Diperlukan) Bearer Token |

#### Request Body
```json
{
  "amount": 5000000,
  "tenor_months": 6
}
```
#### Success Response (202 Accepted)
```json
{
  "status": "success",
  "message": "Pengajuan pinjaman Anda telah diterima dan sedang diproses.",
  "data": {
    "loan_id": "f9e8d7c6-b5a4-3210-fedc-ba9876543210",
    "status": "pending_approval"
  }
}
```
#### Failed Response (409 Conflict)
```json
{
  "status": "error",
  "code": "ACTIVE_LOAN_EXISTS",
  "message": "Anda tidak dapat mengajukan pinjaman baru karena masih ada pinjaman yang aktif."
}
```

### `GET /api/v1/loans/summary`
Digunakan untuk mengecek status pinjaman aktif dan memutuskan tampilan yang harus muncul.

| Properti | Keterangan |
| :--- | :--- |
| **Method** | `GET` |
| **URL** | `/api/v1/loans/summary` |
| **Auth** | (Diperlukan) Bearer Token |

#### Success Response - Jika Ada Pinjaman Aktif
```json
{
  "status": "success",
  "data": {
    "loan_id": "f9e8d7c6-b5a4-3210-fedc-ba9876543210",
    "status": "active",
    "remaining_debt": 4166667,
    "next_installment": {
      "amount": 833333,
      "due_date": "2025-08-23"
    }
  }
}
```
#### Success Response - Jika Tidak Ada Pinjaman
```json
{
  "status": "success",
  "data": null
}
```


### `GET /api/v1/loans/{id}/installments`
Mendapatkan rincian semua cicilan untuk sebuah pinjaman, digunakan pada layar Tampilan Detail Pinjaman.


| Properti | Keterangan |
| :--- | :--- |
| **Method** | `GET` |
| **URL** | `/api/v1/loans/{id}/installments` |
| **Auth** | (Diperlukan) Bearer Token |

#### Path Parameter
{id}: Wajib diisi dengan loan_id yang didapat dari endpoint /loans/summary.

Contoh: /api/v1/loans/f9e8d7c6-b5a4-3210-fedc-ba9876543210/installments

#### Success Response (200 OK)
```json
{
  "status": "success",
  "message": "Rincian cicilan berhasil diambil.",
  "data": {
    "loan_id": "f9e8d7c6-b5a4-3210-fedc-ba9876543210",
    "installments": [
      {
        "installment_number": 1,
        "amount": 1000000,
        "due_date": "2025-07-23",
        "status": "paid"
      },
      {
        "installment_number": 2,
        "amount": 1000000,
        "due_date": "2025-08-23",
        "status": "pending"
      },
      {
        "installment_number": 3,
        "amount": 1000000,
        "due_date": "2025-09-23",
        "status": "pending"
      }
    ]
  }
}
```
