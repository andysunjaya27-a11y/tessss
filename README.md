# SJK-ETABSRebarPro v2.2

> Otomasi desain penulangan balok beton bertulang berbasis output ETABS  
> **PT Struclab Jaya Konsultindo**

[![SNI 2847:2019](https://img.shields.io/badge/SNI-2847%3A2019-blue)](https://bsn.go.id)
[![SNI 1726:2019](https://img.shields.io/badge/SNI-1726%3A2019-blue)](https://bsn.go.id)
[![License](https://img.shields.io/badge/License-Proprietary-red)](LICENSE)
[![Deploy](https://img.shields.io/badge/Deploy-GitHub%20Pages-brightgreen)](https://pages.github.com)

---

## Tentang Aplikasi

SJK-ETABSRebarPro adalah aplikasi **single-file HTML** yang berjalan sepenuhnya di browser — tidak perlu server, tidak perlu instalasi. Aplikasi membaca file model ETABS (`.e2k`/`.et`) dan dua file Excel export ETABS, lalu secara otomatis menjalankan engine desain penulangan sesuai SNI dan menghasilkan semua file output yang dibutuhkan.

```
ETABS  ──►  .e2k + Flexural.xlsx + Shear.xlsx
                          │
                 SJK-ETABSRebarPro
                          │
         ┌────────────────┼────────────────┐
         ▼                ▼                ▼
   Excel Penulangan   DXF per Story   E2K Final
```

---

## Fitur

- **M2 — Grouping:** Klasifikasi G/B dari topologi joint, penamaan G1/G2.../B1/B2... urut dimensi, suffix kumulatif lintas zona (G3-1, G3-2, ...)
- **M3 — Longitudinal:** Engine SNI 9.6 + 18.6.3, pilih tulangan paling mendekati kebutuhan, 1 base diameter + add-bar 1 step di atas
- **M4 — Sengkang:** Engine SNI 22.5, iterasi D8–D16, pilih paling efisien
- **Upload sekaligus:** Flexural + Shear diproses dalam 1 langkah
- **Output Excel:** Format tabel penulangan per zona, panel berdampingan (G1-1 | G1-2 | G2 ...)
- **Output DXF:** Section view per story, siap buka di AutoCAD/ZWCAD
- **Output E2K:** FRAMESECTION + CONCRETESECTION + FRAMESECASSIGN diupdate otomatis sesuai suffix
- **Auth:** Login 4 peran (Owner/Admin/Senior Eng/Junior Eng) dengan password terenkripsi

---

## Cara Deploy ke GitHub Pages

### 1. Fork atau clone repo ini

```bash
git clone https://github.com/<username>/sjk-etabs-rebar-pro.git
cd sjk-etabs-rebar-pro
```

### 2. Pastikan struktur repo

```
repo/
├── index.html      ← aplikasi utama (1 file)
└── README.md
```

### 3. Aktifkan GitHub Pages

1. Buka **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: **main** / **(root)**
4. Klik **Save**

### 4. Akses aplikasi

```
https://<username>.github.io/<repo-name>/
```

> Biasanya aktif dalam 1–3 menit setelah push pertama.

---

## Cara Pakai

| Langkah | Keterangan |
|---|---|
| 1. Login | Pilih peran → masukkan password |
| 2. Project Setup | Isi nama project → upload file `.e2k` atau `.et` |
| 3. Parameter | Set fc', fy, cover, diameter, zona grouping |
| 4. Desain Tulangan | Upload Flexural + Shear Excel → Jalankan Engine |
| 5. Download | Excel, DXF, E2K v1, E2K Final, Laporan .txt |

### Login Pertama Kali

Saat pertama kali, Owner dan Admin belum punya password. Klik tautan **"Buat sekarang"** yang muncul di form login untuk membuat password pertama kali.

---

## Format File Input

### File E2K / ET

File teks model ETABS. Langsung upload tanpa modifikasi.  
Versi ETABS yang didukung: **v20, v21, v22, v23**

### Flexural_Reinf.xlsx

Export dari ETABS:  
**Design → Concrete Frame Design → Tabulate → Concrete Beam Flexure Envelope**

Kolom yang dibutuhkan: `Story` | `Label` | `Section` | `Location` | `As Top` | `As Bot`

### Shear_Reinf.xlsx

Export dari ETABS:  
**Design → Concrete Frame Design → Tabulate → Concrete Beam Shear Envelope**

Kolom yang dibutuhkan: `Story` | `Label` | `Section` | `Location` | `Shear Force` | `VRebar Av/s`

> Nama kolom dideteksi otomatis — tidak perlu rename header.  
> Satuan mm² atau m² dideteksi dan dikonversi otomatis.

---

## File Output

| File | Format | Keterangan |
|---|---|---|
| `SJK_<Project>_Penulangan.xlsx` | Excel | Tabel penulangan per zona + summary |
| `SJK_<Project>_SECTION_<story>.dxf` | DXF R12 | Section view per story |
| `SJK_<Project>_E2K_v1.e2k` | Teks | E2K + tulangan longitudinal |
| `SJK_<Project>_E2K_Final.e2k` | Teks | E2K + longitudinal + sengkang |
| `SJK_<Project>_Laporan.txt` | Teks | Laporan ringkas + daftar FAIL |

---

## Standar & Referensi

| Standar | Judul |
|---|---|
| SNI 2847:2019 | Persyaratan Beton Struktural untuk Bangunan Gedung |
| SNI 1726:2019 | Tata Cara Perencanaan Ketahanan Gempa |
| SNI 1727:2020 | Beban Desain Minimum untuk Bangunan Gedung |

---

## Tech Stack

- **HTML5 + CSS3 + Vanilla JavaScript** — zero framework
- **SheetJS (xlsx) v0.18.5** — parsing & generate Excel (via CDN)
- **localStorage** — penyimpanan auth lokal di browser
- **DXF R12 ASCII** — output gambar

---

## Catatan

- Semua data diproses **di browser pengguna** — tidak ada data yang dikirim ke server
- Password disimpan dalam bentuk **hash** di localStorage
- Aplikasi membutuhkan koneksi internet hanya untuk load SheetJS dari CDN (saat pertama kali buka)

---

*© 2026 PT Struclab Jaya Konsultindo — Confidential — Internal Use Only*
