# Catatan Perubahan

Semua perubahan penting di Acelogic Agents. Installer tersedia di halaman [Releases](https://github.com/acelogicg/Acelogic-Agents-Releases/releases).

English: [CHANGELOG.md](CHANGELOG.md)

## 1.0.14 — 10 Oktober 2026

Rilis ini membuat provider OpenAI-compatible bisa mengirim header sendiri, merapikan pengaturan koneksi, dan mengajarkan kebiasaan coding ke agent.

### Baru

- **Header kustom untuk provider OpenAI-compatible.** Settings → Koneksi → *Header kustom* menambahkan header HTTP di setiap permintaan ke provider itu, untuk layanan yang butuh header sendiri seperti `X-Api-Key`, `api-key`, atau ID organisasi. Header bernama `Authorization` menggantikan token API. Nilai header disamarkan seperti token dan ikut terhapus lewat *Hapus koneksi*.
- **Skill Acelogic Conventions.** Skill bawaan berisi konvensi untuk backend Node.js/TypeScript, frontend React/Vite, dan aplikasi Flutter: dependensi, struktur folder, konfigurasi `.env`, format error, migrasi database, dan deploy PM2/nginx. Agent memuatnya saat membuat proyek atau menambah fitur, dan skill ini selalu tersedia walaupun pemilihan skill mempersempit daftarnya.

### Ditingkatkan

- **OpenAI jadi satu.** Settings → Koneksi kini hanya punya dua provider: *Kompatibel OpenAI* dan *OpenAI*. Di OpenAI, pilih metode masuk: *API key* atau *Akun ChatGPT (OAuth)*.
- **Kode dari agent lebih rapi.** Agent tidak lagi menulis komentar di kode, mengikuti konvensi repo yang sudah ada, tidak menaruh konfigurasi dan secret di kode, tidak menampilkan detail internal ke pengguna akhir, dan tidak commit, push, atau deploy kecuali diminta. Proyek baru dimulai dari stack bawaan (pnpm, TypeScript, Express, Prisma, PM2; Vite + React; Flutter).
- **Settings → Preferensi.** Bagian *Perilaku* kini bernama *Preferensi*, karena isinya pilihan bahasa, tema, memori, dan cache.

## 1.0.13 — 5 Oktober 2026

Rilis ini memungkinkan melampirkan berkas apa pun ke pesan, menjaga cache aplikasi tetap ringan, dan membuat kode serta link di jawaban mudah dipakai.

### Baru

- **Lampirkan berkas.** Tombol penjepit kertas di composer kini bisa melampirkan berkas apa pun, bukan hanya gambar. Berkas juga bisa di-drag ke composer atau di-paste. Maksimal 8 lampiran per pesan.
  - Berkas teks sampai 256 KB (kode, Markdown, JSON, CSV, log…) dikirim bersama pesan, jadi model apa pun bisa membacanya.
  - Berkas yang lebih besar atau biner (PDF, ZIP, Word…) dikirim berupa path-nya, lalu agent membacanya dengan tool-nya.
  - Model vision tetap menerima gambar sebagai gambar.
- **Pembersihan cache otomatis.** Settings → Perilaku → *Bersihkan cache otomatis* menghapus cache aplikasi setiap 30 hari secara default (pilihan 7, 14, 30, 60, atau 90 hari). *Bersihkan sekarang* menampilkan ukuran cache saat ini dan langsung menghapusnya. Riwayat chat, memori, pengaturan, dan model suara yang sudah diunduh tetap aman.
- **Salin kode.** Blok kode di jawaban punya tombol *Salin*, dan kode atau command inline seperti `npm run build` bisa disalin dengan sekali klik.
- **Link bisa diklik.** Link di jawaban, termasuk link Markdown, terbuka di browser default.

### Ditingkatkan

- Lampiran tampil di dalam kotak pesan sebagai kartu rapi berisi nama, jenis, dan ukuran berkas, serta mengikuti tema yang dipilih.

### Diperbaiki

- Kode inline di jawaban tampil persis seperti aslinya. Nama seperti `my_var_name` dulu kehilangan garis bawahnya.
- Berkas yang di-drop di luar kotak pesan tidak lagi menimpa jendela aplikasi, dan link tidak bisa lagi terbuka di jendela aplikasi baru.

## 1.0.12 — 4 Oktober 2026

Rilis ini membuat jelas kenapa agent berhenti, dan memperbaiki jawaban yang hilang di runtime ChatGPT/OpenCode.

### Baru

- **Pesan error provider yang jelas.** Saat provider AI menolak permintaan, chat menyebutkan penyebab dan langkah berikutnya, lalu menampilkan pesan asli dari provider:
  - kena rate limit, berikut lama menunggu jika provider memberitahukannya
  - kuota atau saldo habis
  - API key ditolak, atau akses ke model tidak diizinkan
  - model atau endpoint tidak ditemukan
  - percakapan melebihi batas konteks model
  - provider timeout, sibuk, atau mengalami gangguan
  - server model tidak bisa dihubungi (misalnya LM Studio belum jalan)
- **Status saat mencoba ulang.** Kedua runtime mencoba ulang sendiri saat kena rate limit atau provider sibuk. Selama menunggu, status bar dan composer menampilkan alasannya, dengan hitung mundur bila provider memberitahukan waktunya, misalnya *Kena rate limit provider — mencoba lagi dalam 18 dtk*.
- **Hapus koneksi OpenAI-compatible tersimpan.** Settings → Koneksi → *Hapus koneksi* membuang URL dan token API yang tersimpan, termasuk sisa pengaturan versi lama yang dulu muncul lagi setelah aplikasi dibuka ulang.

### Diperbaiki

- **Jawaban tidak lagi hilang di ChatGPT dan model lain yang memakai runtime OpenCode.** Mulai pesan kedua dalam satu sesi, jawaban kadang sudah dibuat model tetapi tidak pernah tampil, dan giliran berakhir tanpa pesan. Aplikasi kini menunggu aliran event runtime tersambung sebelum mengirim, dan melengkapi teks yang masih terlewat dari hasil akhir.
- Jika model selesai tanpa jawaban sama sekali, chat kini memberi tahu, bukan kosong begitu saja.
- Runtime OpenCode diberi waktu 90 detik untuk menyala (sebelumnya 15), sehingga pembukaan pertama di komputer lambat atau yang sedang dipindai antivirus tidak lagi gagal dengan *startup timed out*. Jika tetap terjadi, pesan errornya menunjuk ke antivirus dan log OpenCode.
- Ditanya soal suara, agent sebelumnya menjawab aplikasi tidak punya fitur itu. Sekarang agent mengenal fitur aplikasinya sendiri (dikte, live voice, berbagi layar dan kamera, panel pratinjau, terminal, dan lainnya) dan menunjukkannya.

## 1.0.11 — 1 Oktober 2026

### Baru

- Menu klik kanan di composer (Tempel, Potong, Salin, Pilih Semua) dan Salin untuk teks yang dipilih di gelembung chat.
- Tempel screenshot dan gambar langsung dari clipboard ke composer.

### Ditingkatkan

- Pemakaian token lebih rendah dan stabil: sesi yang dilanjutkan tidak lagi mengirim ulang seluruh riwayat, dan prompt sistem tetap sama di setiap giliran sehingga cache provider tetap bekerja.

### Diperbaiki

- Pesan error lebih jelas saat runtime agent gagal dijalankan, dan folder workspace diperiksa sebelum sesi dimulai.

## 1.0.10 — 25 September 2026

Rilis ini menekan biaya token setiap pesan, membuat pekerjaan panjang di model lokal lebih stabil, dan menambahkan panel pratinjau untuk file dan halaman web.

### Baru

- **Panel pratinjau.** Tombol `</>` di topbar membuka panel di samping chat, dengan dua jenis tab:
  - **File.** Klik file mana pun di pohon workspace untuk membukanya di editor kode dengan syntax highlighting. Simpan dengan `Ctrl+S`. Line ending (CRLF/LF) tidak diubah. Kalau file sudah berubah di disk sejak dibuka, penyimpanan ditolak supaya perubahan dari luar tidak tertimpa. Markdown langsung tampil ter-render dan bisa dialihkan ke kode sumbernya. Gambar tampil sebagai gambar. Setelah agent selesai satu giliran, tab yang belum Anda edit dimuat ulang dengan hasil kerjanya.
  - **Browser.** Buka dev server lokal, situs web, atau file HTML dari workspace. Alamat singkat juga diterima: `5173` otomatis menjadi `http://localhost:5173`. Tersedia tombol mundur, maju, muat ulang, DevTools, dan buka di browser sistem. Halaman berjalan dalam sandbox tanpa akses ke aplikasi.
- **Satu daftar model untuk semua provider yang tersambung.** Model dari server Kompatibel OpenAI, API key OpenAI, dan akun ChatGPT dimuat bersamaan, dikelompokkan per provider. Memilih model otomatis berpindah ke provider-nya. Tab provider di Settings → Koneksi kini hanya untuk mengatur masing-masing provider. Kalau satu provider sedang offline, model dari provider lain tetap tampil dan error provider tersebut ditampilkan di Settings.
- **Agent bisa menambahkan repositori ke workspace.** Minta agent menambahkan folder lokal atau meng-clone repositori, dan hasilnya langsung masuk daftar workspace. Hanya tersedia di mode Full akses. Dialog kepercayaan workspace tetap muncul sebelum config dari repositori tersebut dijalankan.
- **Agent tahu fitur aplikasi mana yang sedang mati.** Kalau permintaan butuh fitur yang dinonaktifkan (alat web, kontrol perangkat, MCP, embedding, memori, atau Full akses), agent menyebut fiturnya dan letak persis tombolnya dengan label yang sama seperti di aplikasi. Agent tidak lagi mencari jalan pintas atau mengaku berhasil padahal tidak.

### Peningkatan

- **Pemakaian token per pesan jauh lebih kecil.** Sapaan sederhana sebelumnya memakan sekitar 18 ribu token, sekarang sekitar 6 ribu. Agent kini memakai system prompt yang lebih ringkas dan hanya menerima tool yang benar-benar dipakai. Daftar skill hanya berisi skill milik aplikasi. Server MCP hanya ikut dikirim saat pesan membutuhkannya: pertanyaan tentang struktur kode, pesan yang menyebut nama server atau tool-nya, lanjutan dari giliran yang sudah memakainya, atau yang cocok dengan tool-nya menurut embedding (bila embedding aktif).
- **Setiap giliran yang mengubah file diakhiri ringkasan.** Kalau agent berhenti tanpa menjelaskan hasil editnya, agent diminta satu kali untuk menjelaskan tiap file yang berubah, apa yang diubah dan bagaimana caranya, serta pengecekan apa saja yang benar-benar dijalankan. Kalau agent tetap diam, aplikasi menampilkan daftar file yang berubah beserta jumlah baris yang ditambah dan dihapus.
- **Model lokal yang lambat tidak lagi terputus.** Memproses prompt panjang di hardware lokal bisa memakan lebih dari lima menit. Sebelumnya, koneksi diputus tepat di menit kelima dan permintaan diulang dari awal beberapa kali sebelum gagal. Sekarang koneksi dijaga tetap hidup selama model bekerja, dengan batas tunggu hingga 30 menit di runtime Claude. Berlaku untuk kedua runtime.
- **Sambung ulang otomatis.** Kalau provider sempat tidak bisa dihubungi (sedang restart, atau membalas 502/503/504), permintaan dicoba lagi setelah 1, 3, dan 7 detik, dan baris status menampilkan *Menyambung ulang ke provider…*. Gangguan hingga sekitar 30 detik pulih sendiri tanpa memutus giliran.

### Perbaikan

- Berpindah dari provider lain ke ChatGPT bisa tertahan di loading tanpa model yang muncul. Penyebabnya: pengecekan model me-restart runtime ChatGPT saat status akun dan daftar model masih diambil.
- Berganti provider saat daftar model masih dimuat bisa terlewat, sehingga daftarnya tidak diperbarui sampai di-refresh manual.

### Perubahan

- Tool subagent dihapus dari runtime Claude. Setiap subagent mengirim ulang seluruh prompt, sehingga pemakaian token di model lokal berlipat. Pencarian sekarang dikerjakan langsung oleh agent utama.

## Versi sebelumnya

Catatan untuk 1.0.9 ke bawah ada di [halaman rilis](https://github.com/acelogicg/Acelogic-Agents-Releases/releases) masing-masing versi.
