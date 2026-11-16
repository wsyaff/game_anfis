# RespiQuest: Ekspedisi Sistem Pernapasan

Game petualangan 3D anatomi & fisiologi sistem pernapasan untuk kelas X.
Responsif untuk HP (potret & landscape), soal dikelola lewat spreadsheet, dan otomatis ter-deploy ke GitHub Pages.

- **Panduan lengkap (upload, deploy, edit soal, penilaian):** lihat [`PANDUAN.md`](./PANDUAN.md)
- **Bank soal:** [`soal/Soal_RespiQuest.xlsx`](./soal/Soal_RespiQuest.xlsx)

## Ringkas

1. Upload seluruh isi folder ini ke repository GitHub.
2. `Settings → Pages → Source: GitHub Actions`.
3. Tunggu workflow **Deploy RespiQuest** selesai (tab *Actions*). Alamat game: `https://USERNAME.github.io/NAMA-REPO/`.
4. Ganti soal: edit spreadsheet, upload ulang ke folder `soal/` dengan nama yang sama. Game ter-update otomatis.

## Menjalankan di komputer (opsional)

```bash
npm install
pip install openpyxl     # hanya bila ingin mengonversi spreadsheet secara lokal
npm run soal             # xlsx -> public/data/soal.json
npm run dev              # http://localhost:3000
```
