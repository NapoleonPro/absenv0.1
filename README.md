# SimKuliah Bot

> *"Solusi yang keren berasal dari pemikiran yang malas"*  
> *— kamu, sambil rebahan*

<div align="center">

![Python](https://img.shields.io/badge/Python-3.12-blue?style=flat-square&logo=python)
![Selenium](https://img.shields.io/badge/Selenium-4.x-43B02A?style=flat-square&logo=selenium)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-Automated-2088FF?style=flat-square&logo=github-actions)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-USK_SimKuliah-red?style=flat-square)

**Bot absensi otomatis untuk [simkuliah.usk.ac.id](https://simkuliah.usk.ac.id)**  
Jalan sendiri via GitHub Actions — gratis, tanpa server, tanpa VPS, tanpa kamu harus angkat badan dari kasur.

[🇮🇩 Bahasa Indonesia](#-panduan-lengkap) · [🇬🇧 English](#-english-guide)

</div>

---

## ⚡ Cara Kerja (Singkat)

```
Jadwal kuliah tiba  →  GitHub Actions aktif  →  Bot login SimKuliah
                                                       ↓
HP kamu dapat notif  ←  ntfy.sh  ←  Absen berhasil diklik ✓
     (kamu masih tidur)
```

Bot ini melakukan 3 hal yang seharusnya kamu lakukan sendiri tapi jelas tidak akan:

1. **Login** ke SimKuliah — otomatis, tanpa perlu kamu sentuh apapun
2. **Klik tombol absen** (+ tombol konfirmasi, karena SimKuliah tidak semudah itu percaya kehadiran orang seperti kamu)
3. **Kirim notifikasi** ke HP — supaya kamu setidaknya tahu bahwa hari ini ada kuliah, meski tidak hadir secara rohani

---

## 🇮🇩 Panduan Lengkap

> Panduan ini ditulis sedetail mungkin karena pengalaman menunjukkan bahwa pengguna repo ini tidak bisa diberikan instruksi yang ambigu. Ikuti langkah demi langkah. Jangan skip. Kamu sudah cukup banyak skip hal-hal penting dalam hidup.

### Prasyarat

Sebelum mulai, pastikan kamu punya:

- Akun **GitHub** (gratis) → [daftar di sini](https://github.com/signup) — kalau belum punya, ini momen langka kamu melakukan sesuatu yang produktif
- Akun **SimKuliah** USK yang aktif — semoga kamu masih ingat passwordnya
- HP Android/iOS untuk notifikasi — opsional, tapi tanpanya kamu tidak akan pernah tahu apakah bot berhasil atau apakah kamu diam-diam sudah tidak tercatat hadir sejak tiga minggu lalu

---

### Langkah 1 — Fork Repositori Ini

> Fork = menyalin repo ini ke akun GitHub kamu sendiri. Mirip plagiat, tapi legal dan justru dianjurkan.

1. Klik tombol **Fork** di pojok kanan atas halaman ini
2. Klik **Create fork**
3. Selesai — tiga klik, dan ini mungkin pencapaian teknis terbesar hari ini

---

### Langkah 2 — Isi Data Rahasia (Secrets)

Bot butuh NPM dan password kamu. Jangan ditulis langsung di kode — selain tidak aman, itu juga tingkat kemalasan yang bahkan membuat pembuat repo ini geleng-geleng kepala.

Gunakan **GitHub Secrets**. Ini fitur khusus untuk menyimpan data sensitif, dan ya, kamu perlu belajar cara pakainya. Tidak ada yang bisa menggantikan langkah ini.

**Cara masuk ke halaman Secrets:**

```
Repo kamu → Settings → Secrets and variables → Actions → New repository secret
```

Tambahkan **3 secret** berikut. Satu per satu. Jangan sekaligus, karena memang tidak bisa.

| Nama Secret | Isi dengan | Contoh |
|-------------|-----------|--------|
| `NPM` | NPM kamu | `2108107010001` |
| `PASSWORD` | Password SimKuliah kamu | `passwordkamu123` |
| `NTFY_TOPIC` | Nama unik pilihanmu, bebas | `budi-absen-usk` |

> **Catatan soal `NTFY_TOPIC`:** Boleh dikosongkan kalau tidak mau notifikasi. Tapi konsekuensinya: kamu tidak akan tahu kalau bot gagal, absensimu kosong, dan dosen sudah tiga kali panggil namamu di depan kelas sementara kamu masih di kasur menganggap semuanya beres.

---

### Langkah 3 — Setup Notifikasi di HP

Gratis. Tidak butuh akun. Satu-satunya hal yang diminta adalah kamu install satu aplikasi — sesuatu yang biasanya kamu lakukan tanpa pikir panjang untuk hal-hal yang jauh kurang berguna.

**a. Install app ntfy:**

- Android → [Google Play](https://play.google.com/store/apps/details?id=io.heckel.ntfy)
- iOS → [App Store](https://apps.apple.com/app/ntfy/id1625396347)

**b. Subscribe ke 3 topik berikut** (ganti `budi-absen-usk` dengan `NTFY_TOPIC` milikmu):

| Topik | Artinya |
|-------|---------|
| `budi-absen-usk-bisa` | ✅ Absen berhasil — lanjut tidur, kamu aman |
| `budi-absen-usk-info` | ℹ️ Bot jalan tapi tidak ada tombol absen — dosen belum buka, bukan salahmu |
| `budi-absen-usk-gagal` | 💀 Error — ini saatnya kamu melakukan sesuatu sendiri, untuk sekali ini |

Cara subscribe: buka ntfy → ikon `+` → ketik nama topik → Subscribe. Empat langkah. Kamu pasti bisa.

---

### Langkah 4 — Update Jadwal Kuliah

Bot tidak bisa baca pikiran. Dia butuh file `jadwal_cache.json` untuk tahu kapan harus jalan. Tanpa file ini, bot tidak tahu kamu punya kuliah — dan jujur, kamu sendiri sering lupa juga.

**Ada 2 cara:**

#### Cara A — Generate Otomatis *(untuk kamu yang malas tapi masih punya batas)*

```
Tab Actions → Refresh Jadwal Cache → Run workflow → Run workflow
```

Bot akan login, ambil seluruh jadwal semester, simpan otomatis. Ulangi tiap awal semester — atau satu kali seumur semester, lalu lupakan. Persis seperti niat olahraga.

#### Cara B — Edit Manual *(untuk kamu yang entah kenapa memilih jalan susah)*

Buka `jadwal_cache.json`, tambahkan entry sesuai format ini:

```json
{
  "pertemuan": 1,
  "kode_mk": "STIK3034",
  "nama_mk": "PATTERN RECOGNITION AND MACHINE LEARNING",
  "dosen": "Rizka Ramadhana, M.T.",
  "tanggal_str": "12-02-2026",
  "tanggal": "2026-02-12",
  "hari": "Kamis",
  "ruang": "A25-203",
  "jam": "08.00 - 09.40"
}
```

> **Penting:** `"tanggal"` harus format `YYYY-MM-DD`. Salah format, bot tidak jalan, absen kosong, itu salahmu sepenuhnya. `"tanggal_str"` boleh diisi sembarang — label doang, tidak ada yang membaca.

---

### Langkah 5 — Sesuaikan Jadwal Cron

Ini bagian yang butuh sedikit usaha otak. Cron berjalan dalam waktu **UTC** — kurangi 7 jam dari WIB. Aku tahu kamu tidak suka matematika, makanya sudah dibuatkan tabelnya.

**Rumus:** `WIB - 7 jam = UTC`

| Jam WIB | Jam UTC | Cron |
|---------|---------|------|
| 08.00 | 01.00 | `0 1 * * [hari]` |
| 10.45 | 03.45 | `45 3 * * [hari]` |
| 14.00 | 07.00 | `0 7 * * [hari]` |
| 16.35 | 09.35 | `35 9 * * [hari]` |

**Kode hari:** `1` = Senin, `2` = Selasa, `3` = Rabu, `4` = Kamis, `5` = Jumat, `6` = Sabtu, `0` = Minggu

Edit file `.github/workflows/absen.yml`. Sesuaikan dengan jadwal kuliahmu yang — aku yakin — sudah kamu hafal karena selalu hadir. *Ahem.*

**Contoh — kuliah Kamis jam 08.00 WIB** (jam di mana kamu biasanya baru memejamkan mata):

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

Sebelum menyerahkan tanggung jawab akademikmu kepada sekumpulan cron job, tes dulu. Ini satu-satunya langkah yang benar-benar membutuhkan perhatianmu — tolong hadir secara penuh, setidaknya untuk ini.

1. Buka tab **Actions** di repo kamu
2. Klik **Absensi Otomatis** di sidebar kiri
3. Klik **Run workflow** → **Run workflow**
4. Tunggu ~1-2 menit — ya, kamu perlu menunggu. Tidak ada yang instan kecuali mie.

**Baca hasilnya:**

- 🟢 `Absen berhasil` → semua beres, kamu resmi bisa rebahan dengan hati tenang
- 🟡 `tidak ada tombol absen` → bot jalan normal, tidak ada jadwal aktif saat ini. Santai
- 🔴 `Login gagal` → cek NPM dan password di Secrets. Kemungkinan besar ada typo karena diketik sambil ngantuk

---

### Cara Kerja Bot (Lebih Detail)

Setiap kali bot aktif, dia melakukan ini:

1. Cek `jadwal_cache.json` — apakah hari ini ada jadwal kuliah?
2. Kalau ada → login SimKuliah → klik tombol absen → kirim notif ke HP kamu yang mungkin sedang telungkup di suatu tempat
3. Kalau tidak ada → kirim notif info → berhenti dengan anggun

Bot tidak pernah bolos. Bot tidak pernah lupa. Bot tidak perlu diingatkan dua kali. Renungkan itu.

---

### Refresh Jadwal Tiap Semester

Awal semester baru = jadwal baru = kamu perlu refresh cache. Caranya:

```
Tab Actions → Refresh Jadwal Cache → Run workflow
```

Atau biarkan workflow otomatis yang jalan tiap Februari dan Agustus. Satu hal dalam hidupmu yang benar-benar berjalan sendiri tanpa perlu kamu urus — manfaatkan sebaik mungkin.

---

### Troubleshooting

Karena pasti ada yang tidak berjalan mulus, dan kemungkinan besar itu bukan salah botnya:

| Masalah | Kemungkinan penyebab | Solusi |
|---------|---------------------|--------|
| Login gagal terus | NPM atau password salah di Secrets | Cek lagi. Pelan-pelan. Jangan buru-buru. |
| Tidak ada tombol absen padahal ada jadwal | Dosen belum buka sesi absen | Tunggu. Dosen juga manusia. Beda dengan kamu yang bahkan hadir saja perlu diwakilkan. |
| Notif tidak masuk | Nama NTFY_TOPIC tidak cocok | Pastikan nama topik di app **sama persis** dengan yang di Secrets. Huruf kapital beda = topik beda. |
| Bot tidak jalan sesuai jadwal | Cron salah timezone | Sudah ada tabelnya di Langkah 5. Baca lagi. |
| `jadwal_cache.json` kosong | Lupa refresh | Jalankan workflow Refresh Jadwal Cache. Tidak ada shortcut. |

---

### Penjelasan File

Untuk yang penasaran isi repo ini ngapain aja — apresiasi rasa ingin tahumu yang langka:

| File | Fungsi |
|------|--------|
| `absen_runner.py` | Script utama — tulang punggung seluruh operasi ini |
| `refresh_jadwal.py` | Scrape jadwal semester dari SimKuliah |
| `jadwal_cache.json` | Database jadwal. Satu-satunya entitas di sini yang tahu jadwal kuliahmu lebih baik dari kamu |
| `.github/workflows/absen.yml` | Jadwal cron — yang kerja keras sementara kamu tidak |
| `.github/workflows/refresh.yml` | Workflow refresh jadwal tiap awal semester |

---

### Jalankan Lokal *(khusus yang mau repot)*

Kalau mau coba di komputer sendiri, silakan. Ini justru membuktikan bahwa kamu tidak sesantai yang kamu kira.

```bash
# Clone repo
git clone https://github.com/NapoleonPro/absenv0.1.git
cd absenv0.1

# Install dependencies
pip install selenium python-dotenv

# Buat file .env
echo "NPM=npm_kamu" > .env
echo "PASSWORD=password_kamu" >> .env
echo "NTFY_TOPIC=topic_kamu" >> .env

# Refresh jadwal dulu
python refresh_jadwal.py

# Baru jalankan absen
python absen_runner.py
```

> Butuh Google Chrome terinstall. Kalau belum ada, install dulu — ya, kamu harus melakukan satu hal lagi.

---

### Tech Stack

| Komponen | Kegunaan |
|----------|----------|
| Python 3.12 | Runtime utama |
| Selenium | Otomatisasi browser — melakukan klik-klik yang seharusnya tanggung jawabmu |
| Chrome headless | Browser yang bekerja diam-diam di balik layar, tidak seperti kamu |
| python-dotenv | Baca `.env` saat development lokal |
| GitHub Actions | Scheduler gratis yang lebih rajin dari pengguna repo ini |
| ntfy.sh | Satu-satunya yang masih mau memberi kabar ke HP kamu |

---

### ⚠️ Disclaimer

Bot ini dibuat untuk keperluan pribadi. Developer tidak bertanggung jawab atas segala konsekuensi akademik — termasuk tapi tidak terbatas pada: absen tetap kosong karena ada bug yang tidak dilaporkan, dosen yang tiba-tiba kenal mukamu di lorong padahal kamu "selalu hadir", atau IPK yang tetap tidak naik meski kehadiran sudah 100%.

- SimKuliah bisa update tampilannya kapan saja — kalau bot tiba-tiba tidak jalan, itu bukan hantu, cek dulu perubahannya
- **Selalu verifikasi manual** bahwa absensimu tercatat. Bot sudah kerja keras, giliran kamu cek sebentar
- Bot tidak 100% sempurna — tapi setidaknya dia selalu mencoba. Beda dengan kamu.

---

## 🇬🇧 English Guide

> This section exists because apparently even the English-speaking lazy deserve automation too.

### Prerequisites

- A free **GitHub** account → [sign up here](https://github.com/signup)
- An active **SimKuliah USK** account — hopefully you still remember the password
- Android/iOS phone for notifications (optional, but skipping this means you'll never know when the bot fails and you've quietly been absent for a month)

---

### Step 1 — Fork This Repository

Click **Fork** → **Create fork**. That's it. Two clicks. You've already done more today than expected.

### Step 2 — Add GitHub Secrets

Go to: **Settings → Secrets and variables → Actions → New repository secret**

| Secret Name | What to Put | Example |
|-------------|-------------|---------|
| `NPM` | Your student ID | `2108107010001` |
| `PASSWORD` | Your SimKuliah password | `yourpassword` |
| `NTFY_TOPIC` | A unique name of your choosing | `budi-absen-usk` |

Don't skip this. There is no workaround. This is the one step that actually requires you to do something.

### Step 3 — Set Up Push Notifications (Strongly Recommended)

1. Install **ntfy** → [Android](https://play.google.com/store/apps/details?id=io.heckel.ntfy) / [iOS](https://apps.apple.com/app/ntfy/id1625396347)
2. Subscribe to 3 topics (replace `budi-absen-usk` with your topic):
   - `budi-absen-usk-bisa` — attendance recorded. Go back to sleep, you've earned it
   - `budi-absen-usk-info` — bot ran, no attendance button. Not your fault this time
   - `budi-absen-usk-gagal` — something broke. This is the one notification that requires a human response — that human being you

### Step 4 — Load Your Schedule

Run **Refresh Jadwal Cache** from the Actions tab. The bot will scrape your full semester schedule automatically. Do this once per semester — which is probably the only calendar awareness you'll demonstrate all semester.

Or manually edit `jadwal_cache.json`. Each entry needs `"tanggal"` in `YYYY-MM-DD` format. If you get that wrong, the attendance gap is entirely on you.

### Step 5 — Set Your Cron Schedule

Edit `.github/workflows/absen.yml`. GitHub Actions runs on **UTC** — subtract 7 hours from WIB. A conversion table is provided in the Indonesian section because, yes, we anticipated this would be an issue.

### Step 6 — Test It

Go to **Actions → Absensi Otomatis → Run workflow**. Wait 2 minutes. Read the logs. If login failed, check your Secrets for typos — a very human kind of mistake.

---

### How It Works

1. Bot checks `jadwal_cache.json` — is there class today?
2. If yes → logs in → clicks attend → notifies your phone, which is probably face-down somewhere
3. If no → sends info notification → stops. The bot has better time management than you.

---

### Disclaimer

This project is for personal use only. The developer accepts no responsibility for academic consequences, attendance records, or the quiet existential dread of realizing a bot has been more present at your university than you have. Always verify your attendance on SimKuliah. The bot tries its best — a concept worth reflecting on.

---

<div align="center">

Made with too much coffee and too little sleep by [NapoleonPro](https://github.com/NapoleonPro)

Kalau membantu, kasih ⭐ dong — gratis kok. Sekali klik. Bahkan kamu pasti bisa.

</div>