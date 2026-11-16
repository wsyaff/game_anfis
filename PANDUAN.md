# Buku Panduan RespiQuest

Panduan untuk guru/pengelola: mengunggah game ke GitHub, mengaktifkan deploy otomatis, mengganti soal lewat spreadsheet, dan memahami cara penilaian.

---

## 1. Isi Paket

```
respiquest/
├── .github/workflows/deploy.yml   ← robot deploy otomatis (JANGAN dihapus)
├── soal/Soal_RespiQuest.xlsx      ← BANK SOAL (edit file ini)
├── scripts/xlsx_to_json.py        ← pengubah spreadsheet → data game (dipakai otomatis)
├── public/                        ← ikon, manifest (agar bisa dipasang di layar utama HP), data soal
├── src/                           ← kode game
├── index.html, package.json, vite.config.ts, bun.lock ...
├── README.md
└── PANDUAN.md                     ← dokumen ini
```

> Folder `.github` bersifat tersembunyi (namanya diawali titik). Pastikan ikut terunggah, karena di dalamnya ada file `deploy.yml`.

---

## 2. Mengunggah ke GitHub (tanpa perlu Git)

### Langkah A. Siapkan akun & repository
1. Daftar/masuk di <https://github.com>.
2. Klik **+** (kanan atas) → **New repository**.
3. Isi nama, misalnya `respiquest`. Pilih **Public** (GitHub Pages gratis memerlukan repository publik). Klik **Create repository**.

### Langkah B. Ekstrak ZIP lalu unggah
1. Ekstrak `RespiQuest_GitHub.zip` di komputer Anda. Anda akan mendapat satu folder berisi file-file di atas.
2. Di halaman repository yang baru, klik **uploading an existing file** (atau **Add file → Upload files**).
3. Buka folder hasil ekstrak, **pilih semua isi di dalamnya** (bukan foldernya), lalu seret ke halaman GitHub.
   - Windows: tampilkan item tersembunyi lewat *View → Show → Hidden items*, supaya `.github` ikut terpilih.
   - macOS: tekan `Cmd + Shift + .` di Finder untuk menampilkan file tersembunyi.
4. Tunggu semua file selesai terunggah, tulis pesan (mis. "Upload RespiQuest"), pilih **Commit directly to the main branch**, klik **Commit changes**.
5. Pastikan di repository terlihat folder `.github`, `soal`, `src`, `public`, `scripts`. Jika `.github` tidak muncul, unggah ulang folder itu saja (Add file → Upload files → seret folder `.github`).

> Cara alternatif memakai Git di komputer:
> ```bash
> cd folder-hasil-ekstrak
> git init -b main
> git add .
> git commit -m "Upload RespiQuest"
> git remote add origin https://github.com/USERNAME/NAMA-REPO.git
> git push -u origin main
> ```

---

## 3. Mengaktifkan GitHub Pages (sekali saja)

1. Di repository, buka **Settings → Pages**.
2. Pada **Build and deployment → Source**, pilih **GitHub Actions**.
3. Buka tab **Actions**. Workflow **Deploy RespiQuest** akan berjalan sendiri (sekitar 2-4 menit). Jika Anda mengaktifkan Pages setelah unggahan pertama dan workflow sempat gagal, buka workflow tersebut lalu klik **Re-run all jobs** (atau **Run workflow**).
4. Setelah berlogo centang hijau, alamat game muncul di **Settings → Pages**, bentuknya:

   `https://USERNAME.github.io/NAMA-REPO/`

Bagikan alamat itu ke siswa. Setiap kali ada perubahan yang diunggah ke GitHub, game diperbarui otomatis.

---

## 4. Mengedit Soal di Spreadsheet

File: `soal/Soal_RespiQuest.xlsx` (bisa dibuka di Excel, LibreOffice, atau Google Sheets).

### Tab yang tersedia

| Tab | Isi | Jumlah soal bawaan |
|---|---|---|
| `Petunjuk` | Cara pakai + tabel hitung otomatis jumlah soal & poin per soal | – |
| `Pengaturan` | KKM (nilai minimal tuntas), bawaan 75 | – |
| `Misi_1` … `Misi_5` | Soal tantangan tiap misi petualangan 3D | 2 per misi |
| `Diagnosis_Klinis` | Soal diagnosis 4 kasus pasien (Lab IPPA) | 4 |
| `Kuis_Evaluasi` | Kuis akhir | 10 |

### Kolom pada tab Misi dan Kuis

| Kolom | Keterangan |
|---|---|
| Pertanyaan | Teks soal |
| Opsi A, B, C, D | Pilihan jawaban. Minimal A dan B. Opsi E opsional. Isi berurutan, tanpa celah |
| Jawaban Benar | Huruf **A–E** (pilih dari dropdown). Harus menunjuk opsi yang terisi |
| Petunjuk (hint) | Tampil saat siswa menekan "Buka Petunjuk" (hanya tab Misi) |
| Pembahasan | Penjelasan setelah siswa menjawab |
| Topik | (hanya tab Kuis) mis. Anatomi / Fisiologi / Lab Klinis. Dipakai untuk rekap per topik |

Tab `Diagnosis_Klinis` memakai kolom **No Kasus (1–4)**, Opsi A–E, Jawaban Benar, dan Pembahasan.

### Aturan penting
- **Jangan** mengganti nama tab, menghapus/mengubah baris judul kolom (baris 3), atau menyisipkan kolom baru.
- **Menambah soal:** tulis di baris kosong tepat di bawah soal terakhir, tanpa baris kosong di antaranya. Jumlah soal per tab boleh bertambah/berkurang (minimal 1 soal per tab Misi, dan Kuis).
- Kasus pasien (hasil inspeksi, palpasi, perkusi, auskultasi) tersimpan di program. Pada tab `Diagnosis_Klinis` yang boleh diubah hanya opsi, kunci jawaban, dan pembahasan. Pastikan kunci tetap sesuai gejala pasien.
- Cerita misi dan teks narasi tidak ada di spreadsheet (tetap di program).

### Mengunggah soal yang sudah diedit
1. Simpan sebagai **.xlsx** dengan nama yang sama: `Soal_RespiQuest.xlsx`.
2. Di GitHub, buka folder `soal/` → **Add file → Upload files** → seret file hasil edit (menimpa yang lama) → **Commit changes**.
3. Tunggu 2-4 menit sampai workflow di tab **Actions** berlogo centang hijau. Muat ulang game.

### Jika workflow gagal (tanda silang merah)
Buka tab **Actions** → klik run yang gagal → klik langkah **Ubah spreadsheet soal menjadi data game**. Pesan galat menyebut tab dan baris yang salah, contoh:

```
[Tab Misi_2, baris 4] Kolom "Jawaban Benar" berisi "Z" tetapi harus huruf yang menunjuk ke opsi terisi (A-D).
```
Perbaiki sel tersebut, unggah ulang. Selama gagal, game versi sebelumnya tetap tampil (tidak rusak).

---

## 5. Cara Penilaian

- Semua soal dari semua bagian (Misi 1–5, Diagnosis Klinis, Kuis Evaluasi) dijumlahkan.
- **Poin per soal = 100 ÷ total seluruh soal.**
- Soal benar mendapat poin penuh; soal salah atau belum dijawab = 0.
- **Nilai Akhir = jumlah soal benar × poin per soal.** Semua benar = **100**.

Contoh dengan soal bawaan (24 soal): tiap soal 4,17 poin. 19 benar → 79,2. Jika Anda mengubah jumlah soal menjadi 30, tiap soal otomatis 3,33 poin.

Tab **Petunjuk** pada spreadsheet menampilkan perhitungan jumlah soal dan poin per soal secara otomatis agar Anda bisa mengeceknya.

Di game, buka tab **Kuis & Nilai**:
- **Rekap Nilai Seluruh Bagian** menampilkan nilai akhir 0–100, status **Tuntas / Belum Tuntas** (berdasarkan KKM di tab `Pengaturan`), serta benar/total dan poin tiap bagian.
- Nilai per topik kuis dihitung otomatis dari kolom Topik.
- **Exit Ticket** (PNG atau TXT) memuat nama, kelas, nilai akhir, status, rincian per bagian, dan refleksi siswa. Minta siswa mengunduh dan mengirim ke Anda sebagai bukti hasil.

Hal yang perlu diketahui:
- Jawaban terakhir yang dihitung. Bila siswa menekan "Ulangi Misi", "Ulangi Kuis", atau "Ulangi Kasus Ini", jawaban bagian itu diganti.
- Progres dan jawaban tersimpan di **perangkat & browser siswa** (tidak dikirim ke server). Berganti HP/browser = mulai dari awal. Untuk perangkat bersama, siswa dapat menekan **"Hapus progres & nilai di perangkat ini"** di layar judul.
- Jika Anda mengubah soal di spreadsheet, jawaban lama di perangkat siswa otomatis dibersihkan (nama dan kelas tetap tersimpan) agar nilai tidak keliru.
- Karena tidak ada server, game tidak mengumpulkan nilai seluruh siswa secara otomatis. Pengumpulan nilai memakai Exit Ticket.

---

## 6. Tampilan di HP

- Game otomatis menyesuaikan **potret** dan **landscape**, dan berubah mengikuti rotasi HP. Pastikan *Auto-rotate / Kunci orientasi* di HP tidak sedang terkunci.
  - **Potret:** model 3D di bagian atas, panel misi/kontrol di bawah (bisa digulir).
  - **Landscape:** model 3D di kiri, panel di kanan.
  - **Laptop/tablet besar:** tampilan lebar seperti sebelumnya.
- Kontrol sentuh: **geser** untuk memutar model, **cubit** untuk zoom, **ketuk** organ untuk memilih.
- **Pasang di layar utama (seperti aplikasi):** Android Chrome → menu ⋮ → *Tambahkan ke layar utama / Instal aplikasi*. iPhone Safari → tombol Bagikan → *Tambah ke Layar Utama*.
- iPhone: bila suara tidak keluar, matikan mode senyap (tombol samping) dan ketuk layar sekali.
- Perangkat lawas yang lambat dapat terasa berat karena model 3D. Browser yang disarankan: Chrome atau Safari versi terbaru.

---

## 7. Menjalankan di Komputer (opsional)

Prasyarat: Node.js 20 atau lebih baru (disarankan 22), Python 3 (hanya bila ingin mengonversi spreadsheet secara lokal).

```bash
npm install
pip install openpyxl
npm run soal      # membaca soal/Soal_RespiQuest.xlsx → public/data/soal.json
npm run dev       # buka http://localhost:3000
npm run build     # hasil di folder dist/
```

File `public/data/soal.json` sudah disertakan sesuai soal bawaan. Di GitHub, file ini dibuat ulang otomatis dari spreadsheet pada setiap deploy.

---

## 8. Pemecahan Masalah

| Masalah | Penyebab & solusi |
|---|---|
| Tab Actions kosong / workflow tidak jalan | Folder `.github/workflows/deploy.yml` belum terunggah. Unggah ulang folder `.github`. |
| Workflow gagal di langkah "Siapkan Pages" / "Deploy" | `Settings → Pages → Source` belum diset ke **GitHub Actions**. Set, lalu **Re-run all jobs**. |
| Workflow gagal di langkah "Ubah spreadsheet…" | Ada isian spreadsheet yang salah. Baca pesan galat (lihat bagian 4). |
| Halaman Pages 404 | Tunggu beberapa menit setelah deploy pertama. Pastikan alamat memakai nama repository yang benar. |
| Soal tidak berubah setelah upload | Pastikan nama file persis `Soal_RespiQuest.xlsx` di folder `soal/`, workflow sudah hijau, lalu muat ulang paksa halaman (Ctrl+F5). |
| Game memakai soal bawaan walau spreadsheet sudah diubah | File `data/soal.json` gagal dimuat. Pastikan workflow hijau. Game otomatis memakai soal bawaan sebagai cadangan. |
| Layar hitam / model 3D tidak muncul | Perangkat atau browser tidak mendukung WebGL. Coba Chrome terbaru dan aktifkan akselerasi perangkat keras. |
| Repository privat tidak bisa Pages | Pada akun gratis, jadikan repository **Public** (Settings → General → Change visibility). |

---

## 9. Catatan Teknis (untuk pengembang)

- Build memakai Vite + React + Three.js + Tailwind. `base: './'` membuat game berjalan di sub-path GitHub Pages apa pun.
- Alur soal: `soal/Soal_RespiQuest.xlsx` → `scripts/xlsx_to_json.py` → `public/data/soal.json` → diambil game saat dijalankan (`src/lib/questionBank.ts`). Bila gagal diambil, memakai data bawaan di `src/data/respiratoryData.ts`.
- Penilaian ada di `src/lib/questionBank.ts` (`computeScore`). Jawaban tersimpan di `localStorage` (kunci `respiquest_progress_v2`).
- Deteksi orientasi/ukuran layar: `src/hooks/useLayout.ts` (mode `desktop`, `portrait`, `landscape`).
