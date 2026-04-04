# SimKuliah Bot" an

> *"Solusi malas berasal dari pemikiran yang malas dibuat ama orang malas"*
> *— orang malas*

<div align="center">

![Python](https://img.shields.io/badge/Python-3.12-blue?style=flat-square&logo=python)
![Selenium](https://img.shields.io/badge/Selenium-4.x-43B02A?style=flat-square&logo=selenium)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-Automated-2088FF?style=flat-square&logo=github-actions)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-USK_SimKuliah-red?style=flat-square)

</div>

---

## ⚡ Cara Kerja Singkat (serius)

```
Jadwal kuliah tiba  →  GitHub Actions aktif  →  Bot login SimKuliah
                                                       ↓
HP dapat notif  ←  ntfy.sh  ←  Absen berhasil diklik ✓
     (walopun masih teler)
```

Bot ini melakukan 3 hal yang seharusnya kalian lakukan sendiri tapi jelas tidak akan:

1. **Login** ke SimKuliah — otomatis, tanpa perlu kalian sentuh apapun
2. **Klik tombol absen** (+ tombol konfirmasi)
3. **Kirim notifikasi** ke HP — supaya kalian setidaknya tahu bahwa hari ini ada kuliah, emang orang malas

---

## 🇮🇩 Panduan Lengkap

> Panduan ini ditulis sedetail mungkin karena pengalaman menunjukkan bahwa pengguna repo ini tidak bisa diberikan instruksi yang ambigu. Ikuti langkah demi langkah. Jangan skip. Kalian sudah cukup banyak skip hal-hal penting dalam hidup.

### Prasyarat

Sebelum mulai, pastikan kalian punya:

- Akun **GitHub** (gratis) → [daftar di sini](https://github.com/signup) — anak IT ga punya github?, jadi simpan kode proyek selama ini di lokal?, watafak
- Akun **SimKuliah** USK yang aktif — semoga masih ingat passwordnya
- HP Android/iOS untuk notifikasi

---

### Langkah 1 — Fork Repositori Ini

> Fork = menyalin repo ini ke akun GitHub kalian sendiri. Mirip plagiat, tapi legal dan justru dianjurkan.

1. Klik tombol **Fork** di pojok kanan atas halaman ini
2. Klik **Create fork**
3. Selesai — tiga klik, dan ini mungkin pencapaian teknis terbesar hari ini

---

### Langkah 2 — Isi Data Rahasia (Secrets)

Bot butuh NPM dan password. Jangan ditulis langsung di kode — selain tidak aman, itu juga tingkat kemalasan yang bahkan membuat pembuat repo ini geleng-geleng kepala.

Gunakan **GitHub Secrets**. Ini fitur khusus untuk menyimpan data sensitif, dan ya, kalian perlu belajar cara pakainya. Tidak ada yang bisa menggantikan langkah ini.

**Cara masuk ke halaman Secrets:**
```
Repo → Settings → Secrets and variables → Actions → New repository secret
```

Tambahkan **3 secret** berikut. Satu per satu. Jangan sekaligus, karena memang tidak bisa.

| Nama Secret | Isi dengan | Contoh |
|-------------|-----------|--------|
| `NPM` | NPM kalian | `2108107010001` |
| `PASSWORD` | Password SimKuliah  | `passwordkalian123` |
| `NTFY_TOPIC` | 2 kata identifier unik pilihan kalian | `budi-absen` |

> **Catatan soal `NTFY_TOPIC`:** Ini adalah identifier unik kalian — bebas diisi apa saja, asal tidak sama dengan orang lain. Bot akan otomatis menambahkan `-bisa`, `-info`, dan `-gagal` di belakangnya. Jadi kalau kalian isi `budi-absen`, notif akan dikirim ke `budi-absen-bisa`, `budi-absen-info`, dan `budi-absen-gagal`. Jangan dikosongkan — kalau kosong, kalian tidak akan dapat notifikasi sama sekali, dan bot bisa gagal diam-diam tanpa kalian tahu.

---

### Langkah 3 — Setup Notifikasi di HP

Gratis. Tidak butuh akun. Satu-satunya hal yang diminta adalah kalian install satu aplikasi — sesuatu yang biasanya kalian lakukan tanpa pikir panjang untuk hal-hal yang jauh kurang berguna.

**a. Install app ntfy:**

- Android → [Google Play](https://play.google.com/store/apps/details?id=io.heckel.ntfy)
- iOS → [App Store](https://apps.apple.com/app/ntfy/id1625396347)

**b. Subscribe ke 3 topik berikut** (ganti `budi-absen` dengan `NTFY_TOPIC` milik kalian):

| Topik | Artinya |
|-------|---------|
| `budi-absen-bisa` | ✅ Absen berhasil — lanjut tidur, kalian aman |
| `budi-absen-info` | ℹ️ Bot jalan tapi tidak ada tombol absen — dosen belum buka, bukan salah kalian |
| `budi-absen-gagal` | 💀 Error — ini saatnya kalian melakukan sesuatu sendiri, untuk sekali ini |

Cara subscribe: buka ntfy → ikon `+` → ketik nama topik → Subscribe. Empat langkah. Kalian pasti bisa.

---

### Langkah 4 — Update Jadwal Kuliah

Bot tidak bisa baca pikiran. Dia butuh file `jadwal_cache.json` untuk tahu kapan harus jalan. Tanpa file ini, bot tidak tahu kalian kuliah — dan jujur, kalian sering lupa juga.

**Ada 2 cara:**

#### Cara A — Generate Otomatis *(untuk yang malas tapi masih punya batas)*

```
Tab Actions → Refresh Jadwal Cache → Run workflow → Run workflow
```

Bot akan login, ambil seluruh jadwal semester, simpan otomatis. Ulangi tiap awal semester — atau satu kali seumur semester, lalu lupakan. Persis seperti niat olahraga.

#### Cara B — Edit Manual *(untuk kalian yang entah kenapa memilih jalan susah)*

Buka `jadwal_cache.json`, tambahkan entry sesuai format ini:

```json
{
  "pertemuan": 1,
  "kode_mk": "STIK3034",
  "nama_mk": "PATTERN RECOGNITION AND MACHINE LEARNING",
  "dosen": "Prof M. Ammar Fasya, M.T.",
  "tanggal_str": "12-02-2026",
  "tanggal": "2026-02-12",
  "hari": "Kamis",
  "ruang": "A25-203",
  "jam": "08.00 - 09.40"
}
```

> **Penting:** `"tanggal"` harus format `YYYY-MM-DD`. Salah format, bot tidak jalan, absen kosong, itu salah kalian sepenuhnya. `"tanggal_str"` boleh diisi sembarang — label doang, tidak ada yang membaca.

---

### Langkah 5 — Sesuaikan Jadwal Cron

Ini bagian yang butuh sedikit usaha otak. Cron berjalan dalam waktu **UTC** — kurangi 7 jam dari WIB. Aku tahu kalian tidak suka matematika, makanya sudah dibuatkan tabelnya.

**Rumus:** `WIB - 7 jam = UTC`

| Jam WIB | Jam UTC | Cron |
|---------|---------|------|
| 08.00 | 01.00 | `0 1 * * [hari]` |
| 10.45 | 03.45 | `45 3 * * [hari]` |
| 14.00 | 07.00 | `0 7 * * [hari]` |
| 16.35 | 09.35 | `35 9 * * [hari]` |

**Kode hari:** `1` = Senin, `2` = Selasa, `3` = Rabu, `4` = Kamis, `5` = Jumat, `6` = Sabtu, `0` = Minggu

Edit file `.github/workflows/absen.yml`. Sesuaikan dengan jadwal kuliah kalian yang — aku yakin — sudah kalian hafal karena selalu hadir. *Ahem.*

**Contoh — kuliah Kamis jam 08.00 WIB** (jam di mana kalian biasanya baru memejamkan mata):

```yaml
- cron: '0 1 * * 4'
```

**Contoh — Rabu double sesi:**

```yaml
- cron: '0 7 * * 3'   # 14.00 WIB
- cron: '35 9 * * 3'  # 16.35 WIB
```

---

### Langkah 6 — Test Pertama Kali

Sebelum menyerahkan tanggung jawab akademik kepada sekumpulan cron job, tes dulu. Ini satu-satunya langkah yang benar-benar membutuhkan perhatian — tolong hadir secara penuh, setidaknya untuk ini.

1. Buka tab **Actions** di repo kalian
2. Klik **Absensi Otomatis** di sidebar kiri
3. Klik **Run workflow** → **Run workflow**
4. Tunggu ~1-2 menit — ya, kalian perlu menunggu. Tidak ada yang instan kecuali mie.

**Baca hasilnya:**

- 🟢 `Absen berhasil` → semua beres, kalian resmi bisa rebahan dengan hati tenang
- 🟡 `tidak ada tombol absen` → bot jalan normal, tidak ada jadwal aktif saat ini. Santai
- 🔴 `Login gagal` → cek NPM dan password di Secrets. Kemungkinan besar ada typo karena diketik sambil ngantuk

---

### Cara Kerja Bot (Lebih Detail)

Setiap kali bot aktif, dia melakukan ini:

1. Cek `jadwal_cache.json` — apakah hari ini ada jadwal kuliah?
2. Kalau ada → login SimKuliah → klik tombol absen → kirim notif ke HP kalian yang mungkin sedang telungkup di suatu tempat
3. Kalau tidak ada → kirim notif info → berhenti dengan anggun

Bot tidak pernah bolos. Bot tidak pernah lupa. Bot tidak perlu diingatkan dua kali.

---

### Refresh Jadwal Tiap Semester

Awal semester baru = jadwal baru = kalian perlu refresh cache. Caranya:

```
Tab Actions → Refresh Jadwal Cache → Run workflow
```

Atau biarkan workflow otomatis yang jalan tiap Februari dan Agustus. Satu hal dalam hidup kalian yang benar-benar berjalan sendiri tanpa perlu kalian urus — manfaatkan sebaik mungkin.

---

### Troubleshooting

Karena pasti ada yang tidak berjalan mulus, dan kemungkinan besar itu bukan salah botnya:

| Masalah | Kemungkinan penyebab | Solusi |
|---------|---------------------|--------|
| Login gagal terus | NPM atau password salah di Secrets | Cek lagi. Pelan-pelan. Jangan buru-buru. |
| Tidak ada tombol absen padahal ada jadwal | Dosen belum buka sesi absen | Tunggu. Dosen juga manusia. Beda dengan kalian yang bahkan hadir saja perlu diwakilkan. |
| Notif tidak masuk | Nama NTFY_TOPIC tidak cocok | Pastikan nama topik di app **sama persis** dengan yang di Secrets. Huruf kapital beda = topik beda. |
| Bot tidak jalan sesuai jadwal | Cron salah timezone | Sudah ada tabelnya di Langkah 5. Baca lagi. |
| `jadwal_cache.json` kosong | Lupa refresh | Jalankan workflow Refresh Jadwal Cache. Tidak ada shortcut. |

---

### Penjelasan File

Untuk yang penasaran isi repo ini ngapain aja — apresiasi rasa ingin tahu kalian yang langka:

| File | Fungsi |
|------|--------|
| `absen_runner.py` | Script utama — tulang punggung seluruh operasi ini |
| `refresh_jadwal.py` | Scrape jadwal semester dari SimKuliah |
| `jadwal_cache.json` | Database jadwal. Satu-satunya entitas di sini yang tahu jadwal kuliah kalian lebih baik dari kalian |
| `.github/workflows/absen.yml` | Jadwal cron — yang kerja keras sementara kalian tidak |
| `.github/workflows/refresh.yml` | Workflow refresh jadwal tiap awal semester |

---

### Jalankan Lokal *(khusus yang mau repot)*

Kalau mau coba di komputer sendiri, silakan. Ini justru membuktikan bahwa kalian tidak sesantai yang kalian kira.

```bash
# Clone repo
git clone https://github.com/NapoleonPro/absenv0.1.git
cd absenv0.1

# Install dependencies
pip install selenium python-dotenv

# Buat file .env
echo "NPM=npm_kalian" > .env
echo "PASSWORD=password_kalian" >> .env
echo "NTFY_TOPIC=topic_kalian" >> .env

# Refresh jadwal dulu
python refresh_jadwal.py

# Baru jalankan absen
python absen_runner.py
```

> Butuh Google Chrome terinstall. Kalau belum ada, install dulu — ya, kalian harus melakukan satu hal lagi.

---

### Tech Stack

| Komponen | Kegunaan |
|----------|----------|
| Python 3.12 | Runtime utama |
| Selenium | Otomatisasi browser — melakukan klik-klik yang seharusnya tanggung jawab kalian |
| Chrome headless | Browser yang bekerja diam-diam di balik layar, tidak seperti kalian |
| python-dotenv | Baca `.env` saat development lokal |
| GitHub Actions | Scheduler gratis yang lebih rajin dari pengguna repo ini |
| ntfy.sh | Satu-satunya yang masih mau memberi kabar ke HP kalian |

---

### ⚠️ Disclaimer

Bot ini dibuat untuk keperluan pribadi. Developer tidak bertanggung jawab atas segala konsekuensi akademik — termasuk tapi tidak terbatas pada: absen tetap kosong karena ada bug yang tidak dilaporkan, dosen yang tiba-tiba kenal muka kalian di lorong padahal kalian "selalu hadir", atau IPK yang tetap tidak naik meski kehadiran sudah 100%.

- SimKuliah bisa update tampilannya kapan saja — kalau bot tiba-tiba tidak jalan, itu bukan hantu, cek dulu perubahannya
- **Selalu verifikasi manual** bahwa absensi kalian tercatat. Bot sudah kerja keras, giliran kalian cek sebentar
- Bot tidak 100% sempurna — tapi setidaknya dia selalu mencoba. Beda dengan kalian.

<div align="center">

Made with too much coffee and too little sleep by [NapoleonPro](https://github.com/Napoleonnnnn)

Kalau membantu, kasih ⭐ plis . bantu untuk portfolio, hehe. moga masuk surga, aamiin.

</div>