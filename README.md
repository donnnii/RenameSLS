# RenameSLS

![Screenshot](https://bps3320.my.id/image/Screenshot%202026-09-03%20142953.png)

Alat rename otomatis untuk gambar scan peta SLS berbasis OCR. Aplikasi membaca ID **16 digit angka** dari gambar scan, merotasi gambar agar teks tegak lurus, lalu menamai ulang file sesuai ID tersebut.

Fully portable — satu folder, berjalan tanpa perlu menginstal Python maupun Tesseract OCR.

## Fitur

- **OCR otomatis** menggunakan Tesseract OCR yang sudah dibundel di dalam folder.
- **Deteksi ID 16 digit** — semua angka, tanpa batasan awalan tertentu.
- **Auto-rotasi 4 orientasi** (0/90/180/270 derajat) — gambar hasil disimpan dalam posisi tegak lurus.
- **Proses paralel adaptif** — jumlah worker menyesuaikan kemampuan PC secara otomatis (inti CPU & RAM), sehingga tetap ringan di komputer berspesifikasi rendah.
- Hasil rename dipindahkan ke folder `output`.
- File yang **gagal** ditemukan ID-nya dibiarkan tetap di folder `input` untuk direname manual.
- **Cegah duplikat** — jika ID sudah ada di folder `output`, file dilewati (LEWATI).
- **Portable** — tidak perlu instalasi Python, Tesseract, atau dependensi lain.

## Cara Pakai

1. Salin seluruh folder ini ke lokasi mana pun di komputer Anda.
2. Letakkan semua gambar scan (`.png`, `.jpg`, `.jpeg`) ke dalam folder `input`.
3. Jalankan `RenameSLS.exe`.
4. Hasilnya ada di folder `output` (file sudah direname dan diputar tegak lurus). Gambar yang gagal terdeteksi ID-nya tetap berada di folder `input`.

> **Penting:** gunakan `RenameSLS.exe`, bukan `rename.py`. File `.py` hanya untuk pengembangan.

## Menyesuaikan Jumlah Proses Paralel

Secara default aplikasi memilih jumlah worker yang aman untuk PC Anda. Untuk memaksakan jumlah tertentu, atur variabel lingkungan `RENAMESLS_WORKERS` sebelum menjalankan:

```powershell
$env:RENAMESLS_WORKERS = 8
.\RenameSLS.exe
```

Nilai 1 berarti berjalan sekuensial (paling hemat sumber daya).

## Struktur Folder

```
RenameSLS/
├── RenameSLS.exe        # Program utama
├── input/               # Tempat meletakkan gambar yang akan diproses
├── output/              # Hasil rename (dibuat otomatis saat pertama dijalankan)
├── tesseract/           # Tesseract OCR + data bahasa (dibundel)
└── PIL/                 # Plugin Pillow (dibundel)
```

## Persyaratan Sistem

- Windows 64-bit.
- Tidak perlu menginstal Python, Tesseract, atau pustaka lain.

## Catatan Antivirus

Karena aplikasi ini berupa executable tanpa tanda tangan digital (unsigned) yang dibuat dari Python, **Windows Defender atau antivirus lain terkadang menandainya sebagai false positive** ("potentially unwanted software" / deteksi `!ml` berbasis heuristik). Ini bukan virus.

Jika muncul peringatan, tambahkan folder ini ke pengecualian (exclusion) Windows Defender:

*Windows Security → Virus & threat protection → Manage settings → Exclusions → Add or remove exclusions → Add folder* → pilih folder `RenameSLS`.

Atau via PowerShell (Administrator):

```powershell
Add-MpPreference -ExclusionPath 'C:\path\ke\RenameSLS'
```
