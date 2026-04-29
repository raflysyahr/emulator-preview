# 🖥️ RespView — Responsive Viewport Debugger

> **Preview tampilan desktop, tablet, dan mobile dari browser Android — tanpa laptop.**

[![HTML](https://img.shields.io/badge/HTML-Single%20File-orange?style=flat-square&logo=html5)](./responsive-preview.html)
[![No Dependencies](https://img.shields.io/badge/Dependencies-None-brightgreen?style=flat-square)]()
[![Works on Android](https://img.shields.io/badge/Android-Compatible-blue?style=flat-square&logo=android)]()

---

## 📋 Daftar Isi

- [Tentang](#-tentang)
- [Demo](#-demo)
- [Cara Pakai](#-cara-pakai)
  - [Mode URL](#mode-1-url)
  - [Mode HTML Code](#mode-2-html-code)
- [Preset Viewport](#-preset-viewport)
- [Tips & Workflow](#-tips--workflow)
  - [Termux + ngrok](#-termux--ngrok)
  - [IP Lokal via WiFi](#-ip-lokal-via-wifi)
- [FAQ](#-faq)

---

## 🧩 Tentang

**RespView** adalah single-file HTML tool untuk melihat tampilan website kamu pada berbagai ukuran layar — langsung dari browser Android. Cocok untuk developer yang membangun web di Termux, Codespaces, atau environment tanpa akses laptop.

**Masalah yang diselesaikan:**
- Mode desktop di browser Android tetap render seperti mobile (portrait narrow)
- Tidak bisa lihat breakpoint CSS `min-width: 1024px` / `1280px` ter-trigger
- Debugging layout desktop butuh laptop atau layanan screenshot online

**Solusi RespView:**
- Load URL atau paste HTML → preview di dalam iframe dengan dimensi eksak yang kamu tentukan
- Zoom otomatis menyesuaikan layar HP kamu
- Resize bebas untuk test semua breakpoint

---

## 🚀 Cara Pakai

### Instalasi

Tidak perlu install apapun. Cukup download satu file:

```
responsive-preview.html
```

Buka di browser Android (Chrome, Firefox, Kiwi, dll).

---

### Mode 1: URL

Gunakan mode ini untuk preview project yang sedang berjalan secara lokal.

```
Tab: URL → masukkan URL → klik ▶ LOAD
```

> ⚠️ **Penting:** Browser memblokir `localhost` dan `127.0.0.1` dari iframe.
> Kamu harus expose server dengan salah satu cara di bawah.

---

### Mode 2: HTML Code

Gunakan mode ini untuk test cepat tanpa perlu expose server.

```
Tab: HTML CODE → paste HTML kamu → klik ▶ RENDER HTML
```

Cocok untuk:
- Test satu komponen / halaman static
- Preview cepat tanpa jalankan dev server
- Debug layout problem yang sudah diisolasi

---

## 📐 Preset Viewport

| Tombol | Dimensi | Kategori |
|--------|---------|----------|
| 🖥 `1280×800` | 1280 × 800 px | Desktop standard |
| 🖥 `1440×900` | 1440 × 900 px | Desktop widescreen |
| 🖥 `1920×1080` | 1920 × 1080 px | Full HD |
| 📱 `768×1024` | 768 × 1024 px | Tablet portrait |
| 📱 `1024×768` | 1024 × 768 px | Tablet landscape |
| 📲 `375×812` | 375 × 812 px | Mobile (iPhone X) |
| **Custom** | Input manual W/H | Bebas |

**Kontrol tambahan:**
- `⟳` — Rotate viewport (tukar W ↔ H)
- `−` / `+` — Zoom out / in
- `FIT` — Auto-zoom menyesuaikan layar
- Drag sudut kanan bawah frame — resize bebas

---

## 💡 Tips & Workflow

### 🔧 Termux + ngrok

Cara paling mudah untuk expose dev server lokal ke URL publik:

**1. Install ngrok (sekali saja)**
```bash
# Di Termux
npm install -g ngrok

# Atau download binary langsung
curl -O https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-arm64.tgz
tar -xf ngrok-v3-stable-linux-arm64.tgz
```

**2. Daftarkan akun gratis di [ngrok.com](https://ngrok.com) → ambil authtoken**
```bash
ngrok config add-authtoken YOUR_TOKEN_HERE
```

**3. Jalankan dev server kamu, lalu expose**
```bash
# Terminal 1 — jalankan project (contoh React/Vite)
npm run dev

# Terminal 2 — expose ke internet
ngrok http 5173
```

**4. Copy URL dari output ngrok:**
```
Forwarding   https://abc123.ngrok-free.app → http://localhost:5173
```

**5. Paste URL tersebut ke RespView → Load**

---

### 📶 IP Lokal via WiFi

Jika HP dan komputer/server berada di jaringan WiFi yang sama:

**Cari IP lokal server:**
```bash
# Linux / Termux proot Ubuntu
ip addr show wlan0
# atau
hostname -I

# Output contoh: 192.168.1.42
```

**Jalankan dev server dengan bind ke semua interface:**
```bash
# Vite
npm run dev -- --host 0.0.0.0

# React CRA
HOST=0.0.0.0 npm start

# Python HTTP server
python3 -m http.server 8080 --bind 0.0.0.0

# Node.js / Express (pastikan listen ke 0.0.0.0, bukan 127.0.0.1)
app.listen(3000, '0.0.0.0')
```

**Akses dari RespView:**
```
http://192.168.1.42:5173
```

---

### ⚙️ Workflow Harian (Termux)

```bash
# 1. Masuk ke project
cd ~/projects/myapp

# 2. Jalankan dev server
npm run dev -- --host &

# 3. Expose dengan ngrok
ngrok http 5173
```

Buka RespView → paste ngrok URL → pilih preset 1280×800 → debug layout!

---

## ❓ FAQ

**Q: Kenapa `localhost` tidak bisa di-load?**

Browser menerapkan same-origin policy yang memblokir iframe dari mengakses `localhost` atau `127.0.0.1` milik host yang berbeda. Gunakan ngrok atau IP lokal sebagai solusinya.

---

**Q: Apakah RespView butuh koneksi internet?**

Hanya untuk memuat Google Fonts (tampilan UI). Fungsi core (render HTML Code dan load IP lokal) bekerja offline.

---

**Q: Apakah JavaScript di dalam preview berjalan?**

Ya, iframe menjalankan HTML sepenuhnya termasuk JS, CSS, dan fetch ke domain yang sama.

---

**Q: Bisa preview website orang lain (URL eksternal)?**

Bisa, selama website tersebut tidak menggunakan header `X-Frame-Options: DENY` atau `Content-Security-Policy: frame-ancestors 'none'`. Banyak website publik memblokir embedding.

---

**Q: Kenapa hasilnya tetap mirip mobile padahal sudah set 1280px?**

Pastikan website kamu menggunakan CSS media queries berbasis `width` (bukan `device-width`). Jika menggunakan `<meta name="viewport">` dengan `width=device-width`, layout akan mengacu ke lebar perangkat fisik, bukan iframe. RespView bekerja paling baik untuk test CSS breakpoint biasa.

---

## 📁 Struktur File

```
.
└── responsive-preview.html   # Semua fitur dalam satu file, zero dependencies
```

---

## 🛠️ Dibuat dengan

- Vanilla HTML / CSS / JavaScript
- Font: [Orbitron](https://fonts.google.com/specimen/Orbitron) + [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono)
- Zero framework, zero build step

---

## 📄 License

MIT — bebas digunakan, dimodifikasi, dan didistribusikan.

