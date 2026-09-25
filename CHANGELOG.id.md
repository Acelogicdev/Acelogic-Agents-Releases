# Catatan Perubahan

Semua perubahan penting di Acelogic Agents. Installer tersedia di halaman [Releases](https://github.com/acelogicg/Acelogic-Agents-Releases/releases).

English: [CHANGELOG.md](CHANGELOG.md)

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
