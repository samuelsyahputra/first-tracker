# Tracker Pengeluaran (PWA)

Aplikasi tracker pengeluaran mandiri berbentuk PWA (Progressive Web App) statis yang berjalan di atas GitHub Pages, menggunakan Google Apps Script & Google Sheet sebagai database/API. Seluruh frontend publik dan token disimpan di memori klien.

## Langkah Setup & Deploy

### 1. Buat Token Keamanan
Buat password acak yang kuat (minimal 24 karakter). Anda bisa membuatnya melalui password generator atau jalankan perintah ini di console/terminal:
`Math.random().toString(36).slice(2) + Math.random().toString(36).slice(2) + Math.random().toString(36).slice(2)`
Simpan token ini, kita akan menggunakannya di langkah selanjutnya.

### 2. Deploy Backend (Google Apps Script)
1. Buka Google Sheet baru bernama "Tracker Pengeluaran".
2. Klik **Extensions > Apps Script**.
3. Salin dan tempel semua kode dari file `Code.gs` ke editor, lalu simpan (Ctrl+S / Cmd+S).
4. Di panel kiri proyek, klik roda gigi **Project Settings**.
5. Scroll ke bawah ke bagian **Script Properties** > klik **Add script property**.
6. Isi Property dengan `APP_TOKEN` dan isikan token yang Anda buat di Langkah 1 sebagai nilainya. Simpan properti.
7. Klik tombol biru **Deploy > New deployment** di kanan atas.
8. Pilih tipe: **Web app**.
   - Execute as: **Me**
   - Who has access: **Anyone** (Ini wajib agar GitHub Pages bisa mengakses API-nya tanpa login Google, tenang saja, sudah diproteksi `APP_TOKEN`).
9. Klik Deploy. Salin **Web app URL** yang berakhiran `/exec`.

### 3. Deploy Frontend (GitHub Pages)
*(Catatan: pastikan Anda telah membuat ikon aplikasi di folder `/icons/` seperti `icon-192.png`, `icon-512.png`, dan `icon-180.png` untuk Apple Touch Icon).*
1. Buat repositori **Publik** di GitHub.
2. Push file `index.html`, `manifest.webmanifest`, `sw.js`, `.gitignore`, dan folder `icons` ke repo Anda.
3. Buka tab **Settings** di repo Anda > pilih **Pages** di sebelah kiri.
4. Di bagian Build and deployment, pilih source **Deploy from a branch**.
5. Pilih branch `main` dan folder `/ (root)`, lalu klik Save.
6. Tunggu beberapa menit hingga mendapatkan URL GitHub Pages Anda.

### 4. Install di HP (Add to Home Screen)
1. Buka URL GitHub Pages Anda di browser HP (Safari untuk iPhone, Chrome untuk Android).
2. Pilih opsi **Bagikan > Add to Home Screen** (Tambahkan ke Layar Utama).
3. Tutup browser dan buka aplikasi dari ikon baru di home screen Anda. Aplikasi kini tampil fullscreen layaknya aplikasi asli tanpa membuka tab baru!
4. Pada layar Setup yang pertama kali muncul, masukkan **URL Apps Script** dan **App Token** Anda. Klik Hubungkan.

### 5. Daftar Uji (Checklist Keamanan)
- [ ] Cek repositori: Pastikan kata "script.google.com" atau token rahasia Anda tidak tertulis di file `index.html` atau file manapun di GitHub.
- [ ] Masukkan token yang salah di layar Setup aplikasi HP, pastikan ditolak.
- [ ] Buat transaksi, pastikan data berhasil masuk ke Google Sheet tanpa dobel.

### 6. Maintenance & Pembaruan
- **Mengganti Token**: Buka Script Properties di GAS, ganti nilai `APP_TOKEN`. Buka aplikasi di HP, buka menu Tren, scroll ke paling bawah dan klik **Reset Pengaturan API**, lalu masukkan token baru.
- **Update Versi Cache**: Jika Anda mengubah kode CSS/JS di `index.html`, naikkan variabel `CACHE_NAME = 'trk-v2'` di `sw.js` agar HP pengguna segera memuat versi terbaru tanpa harus menghapus cache browser manual.