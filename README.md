# Rekap Nilai

Aplikasi web sederhana untuk mencatat dan menghitung nilai akhir mata kuliah per semester, sekaligus IPK kumulatif. Bisa dipakai langsung di browser HP tanpa install, atau di-install jadi aplikasi (PWA) biar bisa dipakai offline.

## Isi Folder

```
rekap-nilai/
├── index.html          # aplikasi utama (satu-satunya file yang WAJIB ada)
├── manifest.json        # konfigurasi PWA (nama app, ikon, warna)
├── sw.js                 # service worker, bikin app bisa dibuka offline
├── icon-192.png          # ikon untuk Android home screen
├── icon-512.png          # ikon untuk splash screen Android
└── apple-touch-icon.png  # ikon untuk iOS "Add to Home Screen"
```

> Kalau cuma mau pakai `index.html` di browser biasa (tanpa fitur install/offline), file itu bisa dibuka sendirian. Tapi kalau mau fitur install & offline aktif, semua file di atas harus disimpan dalam satu folder yang sama, lalu di-host (misalnya lewat Netlify, Vercel, atau GitHub Pages).

## Fitur

- **Multi semester** — tambah, ganti nama, pindah, dan hapus semester lewat tab di sidebar. Tiap semester punya set mata kuliah dan bobot sendiri-sendiri (karena tiap semester bisa beda bobot penilaiannya).
- **Kartu per mata kuliah** — atur bobot penilaian dan nilai per komponen (tanpa perlu input SKS).
- **Bobot fleksibel** — persentase Kehadiran/Tugas/UTS/UAS bisa diubah bebas per matkul, dengan peringatan kalau totalnya belum 100%.
- **Nilai akhir & huruf otomatis** — dihitung real-time setiap ada perubahan input.
- **IP per semester** — ditampilkan di header, otomatis mengikuti semester yang sedang aktif/dipilih.
- **Grafik nilai per semester** — bar chart nilai akhir semua matkul di semester aktif.
- **Sebaran nilai** — donut chart jumlah matkul per huruf (A/B/C/D/E) dari seluruh semester.
- **Tersimpan otomatis** — data disimpan di `localStorage` HP, tetap ada walau app ditutup/reload.
- **Bisa di-install** — muncul banner "Install" di Android, atau instruksi "Add to Home Screen" di iOS.
- **Bisa offline** — setelah pertama kali dibuka (dan di-install), app tetap bisa diakses tanpa internet.

## Rumus Perhitungan

**Nilai Akhir per matkul** dihitung sebagai rata-rata tertimbang:

```
Nilai Akhir = (Kehadiran×bobot + Tugas×bobot + UTS×bobot + UAS×bobot) ÷ total bobot
```

**Skala huruf** (default, bisa diubah lewat kode di bagian `SCALE` dalam `index.html` kalau kampusmu beda):

| Rentang Nilai | Huruf | Poin |
|---|---|---|
| 85 – 100 | A | 4.0 |
| 70 – 84,9 | B | 3.0 |
| 60 – 69,9 | C | 2.0 |
| 45 – 59,9 | D | 1.0 |
| 0 – 44,9 | E | 0.0 |

**IP Semester** — karena data SKS tiap matkul seringkali tidak dipegang mahasiswa (biasanya cuma ada di KRS/transkrip resmi kampus), app ini menghitung IP semester **tanpa bobot SKS**, yaitu rata-rata biasa antar matkul dalam satu semester. Dua angka ditampilkan sekaligus di header:

```
IP Semester (skala 0-4) = Σ(poin huruf tiap matkul) ÷ jumlah matkul di semester itu
Rata-rata Nilai (skala 0-100) = Σ(nilai akhir tiap matkul) ÷ jumlah matkul di semester itu
```

> Catatan: kalau nanti kamu punya data SKS resmi (misalnya dari transkrip/KRS), angka IP di sini bisa sedikit berbeda dari IP resmi kampus, karena kampus biasanya menghitungnya dengan pembobotan SKS per matkul.

## Cara Pakai

1. Buka `index.html` di browser HP (Chrome/Safari).
2. Tambah semester lewat tombol **"+ Semester"** di sidebar (opsional, sudah ada Semester 1 secara default).
3. Tambah mata kuliah lewat tombol **"+ Tambah Mata Kuliah"**.
4. Isi nama matkul, SKS, bobot tiap komponen, dan nilai tiap komponen.
5. Nilai akhir, huruf, grafik, dan IPK otomatis ter-update.

## Cara Install ke HP (opsional)

**Android (Chrome):**
Buka app → akan muncul banner "Install Rekap Nilai" di bagian bawah → tap **Install**.

**iOS (Safari):**
Buka app → tap tombol **Share** (kotak dengan panah ke atas) → pilih **Add to Home Screen**.

Setelah di-install, app akan punya ikon sendiri di home screen dan bisa dibuka tanpa koneksi internet.

## Catatan Teknis

- Tidak butuh server backend — semua logika jalan di browser (vanilla JavaScript, tanpa framework).
- Data tersimpan lokal di HP masing-masing (tidak tersinkron ke perangkat lain / cloud).
- Menghapus cache/data browser akan menghapus juga data nilai yang tersimpan — pastikan dicatat di tempat lain kalau perlu backup.
- Font diambil dari Google Fonts (Fraunces, Inter, IBM Plex Mono) — butuh koneksi internet saat pertama kali load agar font tampil sempurna; setelahnya tersimpan di cache.
