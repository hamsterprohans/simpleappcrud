# W8 - Integrasi CRUD Next.js dengan MongoDB

Aplikasi CRUD (Create, Read, Update, Delete) sederhana berbasis Next.js 13 yang menyimpan data produk ke MongoDB Atlas menggunakan Mongoose. README ini adalah panduan langkah demi langkah untuk menjalankannya di komputer lokal.

> Repository ini di-fork dari [ShakirAliSWE/nextjs-crud](https://github.com/ShakirAliSWE/nextjs-crud).

## Fitur

- **Create**: menambahkan produk baru.
- **Read**: menampilkan daftar produk.
- **Update**: mengubah data produk.
- **Delete**: menghapus produk dengan konfirmasi.

## Teknologi

- Next.js 13
- MongoDB Atlas
- Mongoose
- React Hook Form
- Bootstrap

## Prasyarat

- Node.js dan npm
- Git
- VS Code
- Koneksi internet (untuk MongoDB Atlas)

---

## Langkah demi Langkah

### A. Mengambil source code

1. **Fork** repository https://github.com/hamsterprohans/simpleappcrud.git melalui tombol **Fork** di GitHub. Setelah selesai, kamu akan punya salinan repository di akun GitHub-mu sendiri dengan alamat:

   ```
   https://github.com/<username-github-kamu>/simpleappcrud.git
   ```

2. Buka **VS Code**.
3. **Clone repository hasil fork-mu** (bukan repository asli milik `hamsterprohans`). Ganti `<username-github-kamu>` dengan username GitHub-mu:

   ```bash
   git clone https://github.com/<username-github-kamu>/simpleappcrud.git
   ```

4. Masuk ke folder project:

   ```bash
   cd nextjs-crud
   ```

   > Jika folder `nextjs-crud` ada di dalam folder hasil clone, masuk dulu ke folder hasil clone tersebut.

5. Install dependency:

   ```bash
   npm install
   ```

   Pastikan proses selesai tanpa error dan folder `node_modules` muncul.

### B. Menyiapkan MongoDB Atlas

6. Buka https://www.mongodb.com/atlas.
7. **Sign up** (daftar).
8. **Buat akun** dan project/cluster baru (paket gratis M0 sudah cukup).
9. **Simpan kredensial** database user (username dan password). Password hanya ditampilkan sekali saat dibuat, jadi catat baik-baik.
10. Masuk ke **MongoDB Atlas** dashboard.
11. Buka menu **Clusters** (Database).
12. Klik **Connect** pada cluster kamu.
13. Pilih **Drivers**, lalu salin connection string (**MONGODB_URL**). Bentuknya seperti:

    ```
    mongodb+srv://<username>:<password>@<cluster>.mongodb.net/<nama_database>
    ```

14. Buat file `.env` di root folder `nextjs-crud`, lalu isi dengan connection string tadi. Ganti `<username>` dan `<password>` sesuai kredensial kamu:

    ```env
    MONGODB_URL="mongodb+srv://<username>:<password>@<cluster>.mongodb.net/<nama_database>"
    ```

    > Nama variabel **harus `MONGODB_URL`** karena itu yang dibaca di [app/libs/mongoDB.js](app/libs/mongoDB.js). Jika password mengandung karakter khusus (`@`, `:`, `/`, `#`), ubah ke bentuk URL-encoded.
    >
    > File `.env` berisi kredensial. **Jangan di-commit** ke GitHub (pastikan ada di `.gitignore`).

15. Di Atlas, buka tab **Security**, bagian **Database Access** dan **Network Access**.
16. Masuk ke **IP Access List**.
17. Klik **Add IP Address**.
18. Pada **Access List Entry**, masukkan `0.0.0.0/0` (izinkan dari mana saja).

    > Cukup untuk keperluan belajar. Untuk produksi, batasi ke IP yang spesifik.
    > Tunggu status entri menjadi **Active** sebelum lanjut.

### C. Menjalankan aplikasi

19. Di folder `nextjs-crud`, jalankan build:

    ```bash
    npm run build
    ```

20. Jalankan development server:

    ```bash
    npm run dev
    ```

21. Buka link yang muncul di terminal (biasanya http://localhost:3000).
22. Coba **tambah (store) data** produk lewat form.
23. Data berhasil tersimpan. Kamu bisa memeriksanya di Atlas melalui **Browse Collections**, atau langsung di daftar produk pada aplikasi.

---

## Troubleshooting

| Gejala | Penyebab | Solusi |
| --- | --- | --- |
| `The uri parameter to openUri() must be a string, got "undefined"` | Variabel `MONGODB_URL` tidak ada di `.env` (misalnya namanya `MONGODB_URI`) | Pastikan nama variabel persis `MONGODB_URL`, lalu restart `npm run dev` |
| `Could not connect to any servers ... IP that isn't whitelisted` | IP belum ada di IP Access List, atau IP berubah | Tambahkan IP saat ini atau `0.0.0.0/0` di Network Access, tunggu **Active** |
| `Operation products.find()/insertOne() buffering timed out after 10000ms` | Koneksi ke database gagal, query menunggu sampai timeout | Selesaikan dua masalah di atas terlebih dahulu |
| `bad auth` / `Authentication failed` | Username atau password salah | Periksa user di **Database Access** dan isi `.env` |

> Setelah mengubah `.env`, selalu **restart** `npm run dev` karena Next.js hanya membaca file tersebut saat start.

## Catatan

- Peringatan `allowedDevOrigins` pada `next.config.js` saat `npm run dev` tidak berbahaya (opsi itu belum dikenal di Next.js 13).
- Data disimpan pada collection `products` (lihat [app/models/products.js](app/models/products.js)).
