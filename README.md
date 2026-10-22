# Portal Kesehatan Jiwa Komunitas — UPTD Puskesmas Damai

Alat bantu skrining dan pendampingan kesehatan jiwa untuk kader posyandu dan tenaga kesehatan.
Dikembangkan oleh dr. Lely Sustantine Totalia, Peserta Didik PPDS KKLP FK UPH.

## Isi folder

| File | Fungsi |
|---|---|
| `index.html` | Halaman depan portal (daftar alat) |
| `PHQ4.html` | Formulir skrining PHQ-4 sesuai instrumen Kemenkes (persetujuan, identitas, alamat), hasil masuk ke Google Sheet (lihat langkah 3) |
| `KaderSehatJiwa.html` | Sistem skrining PHQ-4 kader: skrining, rujukan & verifikasi, rekap diagnosis komunitas (perlu Google Sheet, lihat bawah) |
| `SRQ20.html` | Kalkulator SRQ-20 (verifikasi di Puskesmas) |
| `PHQ9.html` | Kalkulator PHQ-9 (derajat depresi) |
| `GAD7.html` | Kalkulator GAD-7 (derajat cemas) |
| `PanduanKader.html` | Ringkasan materi pelatihan kader |
| `KKLP.jpg`, `fkuph.jpg`, `logo-puskesmas.png`, `logo-uph.png`, `alur-rujukan.png` | Gambar dan logo |

`Code.gs` **tidak** diunggah ke GitHub. File itu ditempel di Google Apps Script (langkah 2).

## Langkah 1 — Terbitkan di GitHub Pages

1. Buat akun di github.com (gratis) bila belum punya.
2. Klik **New repository**, beri nama misalnya `KesehatanJiwaDamai`, pilih **Public**, lalu **Create repository**.
3. Klik **uploading an existing file** (atau **Add file → Upload files**), seret semua file di folder ini (kecuali `Code.gs`), lalu **Commit changes**.
4. Buka **Settings → Pages** → *Source*: **Deploy from a branch** → Branch **main**, folder **/ (root)** → **Save**.
5. Tunggu 1–2 menit. Portal dapat dibuka di `https://NAMAAKUN.github.io/KesehatanJiwaDamai/`.

Kalkulator SRQ-20, PHQ-9, GAD-7, dan Panduan langsung bisa dipakai. Sistem skrining kader membutuhkan langkah 2.

## Langkah 2 — Hubungkan sistem skrining kader ke Google Sheet

1. Dengan akun Google Puskesmas/pengelola program, buat Google Sheet baru (jangan dibagikan publik).
2. Menu **Extensions → Apps Script**, hapus isi editor, tempel seluruh isi `Code.gs`, simpan.
3. **Project Settings → Script Properties**, tambahkan:
   - `KADER_CODE` = kode untuk kader
   - `ADMIN_CODE` = kode untuk petugas Puskesmas (verifikasi dan hapus data)
4. **Deploy → New deployment → Web app**: *Execute as* **Me**, *Who has access* **Anyone** → **Deploy** → salin URL berakhiran `/exec`.
5. Di GitHub, buka `KaderSehatJiwa.html` → ikon pensil (Edit), cari
   `const API_URL = "TEMPEL_URL_WEB_APP_DI_SINI";`
   ganti dengan URL dari langkah 4, lalu **Commit changes**.

Bagikan alamat portal dan `KADER_CODE` kepada kader. `ADMIN_CODE` hanya untuk pengelola program dan dokter.

## Langkah 3 — Hubungkan formulir PHQ-4 (Kemenkes) ke Google Sheet

Formulir ini memakai Google Sheet **terpisah** dan file `Code-PHQ4.gs` (jangan diunggah ke GitHub).

1. Dengan akun Google Puskesmas, buat Google Sheet baru, misalnya **Skrining PHQ-4 Puskesmas Damai**. Jangan dibagikan publik.
2. **Extensions → Apps Script**, hapus isi editor, tempel seluruh isi `Code-PHQ4.gs`, simpan.
3. Di menu atas editor pilih fungsi **siapkanSheet**, klik **Run**, lalu izinkan akses. Sheet `PHQ4` (data) dan `Rekap` (rumus otomatis) akan dibuat.
4. **Deploy → New deployment → Web app**: *Execute as* **Me**, *Who has access* **Anyone** → **Deploy** → salin URL berakhiran `/exec`.
5. Di GitHub, buka `PHQ4.html` → Edit, cari `const SHEET_URL = "TEMPEL_URL_WEB_APP_DI_SINI";`, ganti dengan URL tersebut, lalu **Commit changes**.

Nomor WhatsApp kontak (0811-5448-787) dapat diganti pada baris `const WA = "628115448787";` di PHQ4.html (format 62 tanpa angka 0 di depan).

Formulir ini tidak memakai kode akses: siapa pun yang membuka tautan dapat mengisi (cocok untuk QR di loket atau posyandu). Skrip hanya **menambah baris** dan tidak dapat membaca data, jadi data warga (NIK, nama, alamat) hanya terlihat dari dalam Google Sheet. Kolom **Tindak lanjut** dan **Catatan petugas** diisi petugas langsung di Sheet.

Bila `Code-PHQ4.gs` diubah, buat **Deploy → Manage deployments → Edit → Version: New version** agar URL tetap sama.

## Catatan

- Kalkulator SRQ-20/PHQ-9/GAD-7 menghitung di perangkat dan tidak menyimpan jawaban.
- Sistem kader hanya menyimpan kode warga, usia, jenis kelamin, wilayah, dan jawaban; nama dan NIK tetap di formulir kertas.
- Bila kader menyimpan tanpa sinyal, data dikirim otomatis saat sinyal kembali.
- Untuk mengganti kode akses, ubah Script Properties.
