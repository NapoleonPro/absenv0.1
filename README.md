# Absen Otomatis USK

**Kalau terbantu, kasi star plis karena star bikin developer merasa dihargai (dan semangat maintain code-nya , cape jir).**

> *"Karena mahasiswa yang baik adalah mahasiswa yang hadir, tapi mahasiswa yang genius adalah yang bikin bot untuk hadir sambil rebahan (masi aja ada orang yang bayar untuk jasa absen online)"*

Automated attendance system untuk mahasiswa Universitas Syiah Kuala yang seren lupa absen karena masih ngantuk atau lagi asik scroll TikTok. Dibuat oleh mahasiswa malas yang capek bangun pagi cuma buat pencet tombol absen.

## Features

* **Auto-login** ke SIMKULIAH USK (karena males ngingat password setiap hari)
* **Smart scheduling** dengan GitHub Actions (gratis pula, untung ada Microsoft)
* **Multi-session support** untuk yang jadwalnya chaos kayak hidup kita
* **Error handling** yang lumayan robust (hasil pengalaman sering gagal login)
* **Screenshot evidence** buat debugging atau bukti ke dosen kalau sistem error lagi
* **Push notification** via ntfy - notifikasi langsung ke HP kalau absen berhasil/gagal

## Quick Start

### Prerequisites

* Akun GitHub (yang masih inget passwordnya)
* NPM dan PASSWORD yang kalian pakai untuk login simkuliah
* Doa agar WiFi kampus tidak lemot
* Niat untuk tidak bolos kuliah fisik (ini yang paling susah)

### Setup Instructions

1. **Fork/Clone repository ini**
   ```bash
   git clone https://github.com/NapoleonPro/absenv0.1.git
   ```
2. **Setup GitHub Secrets**
   * Buka Settings > Secrets and variables > Actions (kalau lupa dimana, googling aja)
   * Tambahkan secrets berikut:
     * `NPM`: Nomor Pokok Mahasiswa kalian (yang sering lupa pas ditanya dosen)
     * `PASSWORD`: Password SIMKULIAH
3. **Setup Push Notification (Optional serius, karena butuh effort sikit)**
   * Download **ntfy app** di HP (lebih recommended) atau akses ntfy.sh di web
   * Ganti topic di file `absen.py` dari `akbar-permana-absen-sukses` ke topic kalian sendiri
   * Topic bebas asal unik, contoh: `nama-kalian-absen` atau `npm-kalian-bot`
   * Subscribe ke topic yang sama di ntfy app
4. **Activate Workflow**
   * Buka tab Actions di repository
   * Enable workflows (tinggal klik doang kok, gampang)
   * Duduk santai sambil ngopi dan berharap bot nya gak ngambek

## Schedule

Bot akan berjalan otomatis sesuai jadwal kuliah:

* ​**Selasa**​: 08:05 & 10:50 WIB
* ​**Rabu**​: 08:05, 10:50 & 14:05 WIB
* ​**Kamis**​: 14:05 & 17:00 WIB
* ​**Jumat**​: 10:50 WIB

*Schedule jam kuliah kalian dapat disesuaikan di file `.github/workflows/absen.yml`(schedule sekarang punya gwehch so jangan diutak-atik nanti rusak)*

## Tech Stack

* **Python 3.12** - Karena developer males upgrade dari 3.11 tapi YOLO
* **Selenium WebDriver** - Biar bisa klik-klik otomatis kayak tangan kita
* **GitHub Actions** - CI/CD gratis, siapa yang nolak coba?
* **Chrome Headless** - Browser siluman yang kerja tanpa kelihatan
* **ntfy.sh** - Push notification gratis tapi ya perlu setup sikit lah

## Setup Notifikasi

Bot udah dilengkapi push notification via ntfy biar kalian tau status absen tanpa perlu buka GitHub Actions.

### Cara Setup:

1. **Download ntfy app** di HP (Android/iOS) atau buka https://ntfy.sh di browser
2. **Ganti topic** di file `absen.py`:
   ```python
   # Cari baris ini dan ganti topic-nya:ntfy.sh/akbar-permana-absen-sukses    # ganti jadi topic kalianntfy.sh/akbar-permana-absen-info      # ganti jadi topic kalian  ntfy.sh/akbar-permana-absen-error     # ganti jadi topic kalian
   ```
3. **Subscribe** ke topic yang sama di ntfy app
4. **Done!** Sekarang bakal dapet notif kalau:
   * ✅ Absen berhasil
   * ℹ️ Tidak ada jadwal
   * ❌ Ada error

**Recommended:** Pake HP karena notif langsung masuk, kalau web harus dibuka terus atau Install WPAnya.

## Local Development (Optional)

*Cuma buat yang mau debugging atau development, mayoritas user gak perlu ini.*

```bash
# Setup virtual environment (buat yang peduli sama dependency hell)
python -m venv mymyenv
source mymyenv/bin/activate  # Linux/Mac
# atau
mymyenv\Scripts\activate     # Windows (wtf is this os)

# Install dependencies (semoga gak error)
pip install -r requirement.txt

# Setup environment variables (jangan lupa!)
cp .env.example .env
# Edit .env dengan credentials  (kocak si kalo lupa)

# Run manual test (doain jalan ya)
python absen.py
```

## Roadmap

### Near Future (ketika developer udah gak malas lagi)

* **Multi-account support** - Tinggal nambahin loop doang sih, tapi males mikir error handlingnya
* **Custom schedule per user** - Soalnya jadwal setiap mahasiswa beda-beda kayak kehidupan
* **Telegram/Discord notifications** - Biar tau kalau bot berhasil atau lagi mogok

### Far Future (ketika developer dapet pencerahan atau kena deadline)

* **GUI Desktop Application** - Buat yang alergi terminal hitam-hitam (akwoakao)

## Disclaimer

* Tool ini dibuat untuk **educational purposes** dan membantu mahasiswa pelupa kayak developer
* Gunakan dengan bijak dan tetap ikut kuliah beneran (jangan cuma ngandalin bot doang ya kocak)
* Developer tidak bertanggung jawab jika:
  * Dosen jadi curiga kenapa kalian tiba-tiba rajin absen tepat waktu
  * Sistem USK upgrade security terus bot-nya jadi gabisa login
  * kalian ketagihan bikin automation buat hal lain sampe lupa ngerjain tugas
  * Bot-nya lagi bad mood dan gamau jalan (namanya juga teknologi)

## Contributing

Pull requests are welcome! Apalagi kalau ada yang mau:

* Fix bugs yang developer udah tau tapi males benerin
* Nambahin fitur keren (yang developer kepikiran tapi lupa dicatet)
* Improve documentation (ini paling dibutuhin, soalnya developer jelek nulis dokumentasi)
* Kasih Mie Ayam ❤️atau doa biar developer semangat coding lagi

## License

MIT License - Bebas dipake, dimodifikasi, bahkan dijual (tapi kasih tau developer ya, siapa tau bisa bagi hasil buat beli kopi)

## Developer Notes

*"Code yang bagus adalah code yang bisa kita pahami di masa depan ketika kita sendiri udah lupa sama sekali kenapa kalian bikin ini dan lagi desperate nyari solusi yang sama"*

Dibuat dengan seporsi Mie Ayam❤️ (dehel), ☕, dan sedikit frustrasi oleh mahasiswa malas tapi pintar yang pengen lulus tepat waktu tanpa ribet.

---



