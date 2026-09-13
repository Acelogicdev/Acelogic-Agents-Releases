# Acelogic Agents — Releases

Repositori ini hanya berisi installer resmi **Acelogic Agents**. Kode sumber aplikasi tidak disimpan di sini.

This repository only hosts the official **Acelogic Agents** installers. The application source code is not published here.

## Unduh / Download

Ambil versi terbaru di halaman **[Releases](https://github.com/acelogicg/Acelogic-Agents-Releases/releases/latest)**.

| Sistem | File |
|---|---|
| Windows (x64) | `Acelogic-Agents-<versi>-x64-Setup.exe` |
| Linux (x64) | `Acelogic-Agents-<versi>-x86_64.AppImage` atau `.deb` |

## Instalasi

**Windows** — jalankan `Setup.exe`, pilih folder instalasi, lalu buka Acelogic Agents dari Start Menu atau shortcut desktop.

**Linux (AppImage)**

```bash
chmod +x Acelogic-Agents-*.AppImage
./Acelogic-Agents-*.AppImage
```

**Linux (deb)**

```bash
sudo apt install ./Acelogic-Agents-*.deb
```

## Pembaruan otomatis

Aplikasi yang sudah terinstal mengecek halaman Releases ini secara berkala, mengunduh versi baru di latar belakang, lalu menawarkan **Restart & perbarui**. Jika tidak di-restart, pembaruan dipasang saat aplikasi ditutup. Status pembaruan bisa dilihat di **Settings → Pembaruan**.

Installed apps check this Releases page periodically, download new versions in the background, and offer **Restart & update**. Otherwise the update is applied when the app is closed.

> File `latest.yml`, `latest-linux.yml`, dan `.blockmap` di setiap rilis dipakai oleh pembaruan otomatis — jangan dihapus.
