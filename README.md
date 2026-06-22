# 📋 SIGAP (Sistem Informasi Pengaduan & Aspirasi Publik)

**SIGAP** adalah aplikasi desktop native berbasis Python yang menggunakan **CustomTkinter** untuk merancang sistem manajemen pengaduan fasilitas umum terpadu. Sistem ini menghubungkan Warga secara langsung dengan aparatur pemerintah (Kelurahan, Kecamatan, hingga Kota) secara berjenjang (eskalasi terstruktur).

Aplikasi ini mengusung arsitektur berbasis MVC (*Model-View-Controller*) mini yang terstruktur, aman dengan enkripsi password, serta dilengkapi visualisasi analitik interaktif untuk administrator kota.

---

## 🌟 Fitur Utama

*   **Eskalasi Laporan Berjenjang**: Workflow pelaporan dinamis mulai dari Warga (`Menunggu`) → diproses Kelurahan (`Diproses Kelurahan` / Eskalasi) → diproses Kecamatan (`Diproses Kecamatan` / Eskalasi) → diproses Kota (`Diproses Kota`) → `Selesai` atau `Ditolak`.
*   **Klasifikasi Prioritas Otomatis**: Secara cerdas menganalisis kata kunci (*keywords*) pada deskripsi laporan (seperti *"ambruk"*, *"banjir"*, *"kebakaran"* untuk **Tinggi**; *"berlubang"*, *"rusak"*, *"bocor"* untuk **Sedang**) saat dikirim oleh Warga untuk mempermudah prioritas penanganan.
*   **Laporan Anonim**: Warga dapat memilih opsi kirim sebagai *Anonim* untuk menjaga privasi keamanan identitas mereka dari warga lain dan admin tingkat bawah. Namun, identitas asli tetap dapat diakses oleh Admin Kota untuk transparansi dan akuntabilitas.
*   **Sistem Dukungan (Upvote) Aspirasi**: Warga dapat melihat laporan publik yang aktif di wilayah kelurahannya dan memberikan dukungan (*upvote*). Laporan dengan dukungan terbanyak akan naik ke daftar prioritas teratas.
*   **Lampiran Foto Ganda**: Mendukung unggahan foto bukti kerusakan oleh Warga (`foto_laporan`) dan unggahan foto hasil pekerjaan/penyelesaian oleh admin penanggung jawab (`foto_selesai`).
*   **Sesi Inactivity Timeout**: Fitur keamanan perbankan yang otomatis mengeluarkan pengguna jika tidak ada aktivitas mouse/keyboard selama 30 menit. Dilengkapi dialog peringatan *countdown* 60 detik sebelum otomatis logout.
*   **Dashboard Analitik Interaktif**: Halaman Admin Kota dilengkapi dengan grafik analisis visual (Pie, Bar, dan Line Chart) menggunakan **Matplotlib** yang di-embed langsung ke dalam interface CustomTkinter.
*   **Dukungan Dual-Theme**: Pergantian tema *Light/Dark Mode* yang mulus dan responsif di seluruh panel navigasi sidebar.

---

## 📁 Struktur Proyek

```text
SIGAP/
├── assets/                  # Aset gambar, logo, dan ikon aplikasi
├── config/                  # Pengaturan aplikasi, database, dan daftar wilayah
│   ├── database.py          # Wrapper koneksi MySQL
│   ├── settings.py          # Konstanta global (kategori, prioritas keywords, tema)
│   └── wilayah.py           # Data wilayah administratif (Kecamatan & Kelurahan)
├── controllers/             # Logic controller penanganan aksi
│   ├── auth_controller.py   # Logika login, registrasi, dan ganti password
│   └── laporan_controller.py# Logika manipulasi laporan, upvote, dan histori
├── database/                # Skema dasar database & skrip migrasi
│   ├── setup_db.py          # Skrip inisialisasi & seeder akun
│   ├── migrate_v1_1.py      # Migrasi fitur pendukung & prioritas
│   └── migrate_v1_2_foto.py # Migrasi fitur lampiran foto
├── models/                  # Data Access Object (DAO) query database
│   ├── user_model.py        # Query manipulasi data user & otentikasi
│   └── laporan_model.py     # Query manipulasi data laporan & grafik analitik
├── packaging/               # Konfigurasi build pyinstaller & installer compiler
│   ├── sigap_warga.spec     # Build spec Warga (tanpa matplotlib, ukuran lebih kecil)
│   ├── sigap_admin.spec     # Build spec Admin (dengan matplotlib & numpy)
│   ├── installer_warga.iss  # Inno Setup Script untuk installer Warga (.exe)
│   └── installer_admin.iss  # Inno Setup Script untuk installer Admin (.exe)
├── utils/                   # Fungsi pembantu dan helper grafik
│   ├── chart_helper.py      # Pembungkus canvas matplotlib untuk Tkinter
│   └── helpers.py           # Format tanggal, pembersih input, dll.
├── views/                   # Interface pengguna (CustomTkinter UI)
│   ├── auth/                # Halaman Login & Registrasi
│   ├── components/          # Komponen UI reusable (sidebar, data table, dialog, dll.)
│   ├── warga/               # Dashboard tracking & form pengaduan Warga
│   ├── admin_kelurahan/     # Dashboard validasi awal kelurahan
│   ├── admin_kecamatan/     # Dashboard koordinasi & eskalasi kecamatan
│   └── admin_kota/          # Dashboard master analitik kota
├── requirements.txt         # Daftar pustaka / dependensi Python
├── main.py                  # Entry point utama aplikasi
└── README.md                # Dokumentasi proyek
```

---

## 🛠️ Stack Teknologi

*   **Bahasa Pemrograman**: [Python 3.10+](https://www.python.org/)
*   **UI Framework**: [CustomTkinter (v5.2.0+)](https://github.com/TomSchimansky/CustomTkinter)
*   **Mesin Database**: MySQL (direkomendasikan via [Laragon](https://laragon.org/) / XAMPP)
*   **Driver Konektor**: `mysql-connector-python`
*   **Kriptografi**: `bcrypt` (untuk hashing password aman)
*   **Visualisasi Data**: `matplotlib` (khusus dashboard Admin Kota)
*   **Manipulasi Gambar**: `Pillow` (PIL)
*   **Alat Kompilasi**: `PyInstaller` & `Inno Setup Compiler`

---

## ⚙️ Cara Instalasi & Menjalankan

### 1. Kloning Repositori & Masuk Direktori
```bash
git clone <repository-url>
cd SIGAP
```

### 2. Instalasi Dependensi Python
Pastikan virtual environment telah aktif, kemudian jalankan:
```bash
pip install -r requirements.txt
```

### 3. Konfigurasi & Setup Database
1. Pastikan layanan server MySQL Anda (Laragon/XAMPP) sudah dalam keadaan berjalan (*Running*).
2. Sesuaikan konfigurasi database Anda di [settings.py](file:///run/media/gley/gleyy/gley/tygas/Projek%20Desktop/SIGAP/config/settings.py) jika diperlukan (misalnya host, port, user, password).
3. Jalankan skrip setup database untuk membuat skema secara otomatis beserta seeding awal data admin:
```bash
python database/setup_db.py
```

### 4. Menjalankan Aplikasi
Setelah database terbuat dan siap, jalankan aplikasi menggunakan perintah berikut:
```bash
python main.py
```

---

## 👥 Akun Uji Coba Default (Seeders)

Setelah menjalankan `setup_db.py`, sistem akan membuat beberapa akun admin default dengan password bawaan **`admin123`**:

| Nama Akun / Wilayah | Username / Email | Role |
| :--- | :--- | :--- |
| **Warga Demo** *(Dapat mendaftar sendiri)* | *(Gunakan menu Register)* | `Warga` |
| **Admin Kelurahan Wenang Utara** | `admin.wenangutara@sigap.id` | `admin_kelurahan` |
| **Admin Kelurahan Sario** | `admin.sario@sigap.id` | `admin_kelurahan` |
| **Admin Kecamatan Wenang** | `admin.kec.wenang@sigap.id` | `admin_kecamatan` |
| **Admin Kecamatan Sario** | `admin.kec.sario@sigap.id` | `admin_kecamatan` |
| **Admin Kota Manado** | `admin.kota@sigap.id` | `admin_kota` |

---

## 📦 Panduan Kompilasi Aplikasi (.exe)

Proyek ini telah dikonfigurasi untuk memisahkan build aplikasi client (Warga) dengan build admin untuk mereduksi ukuran file distribusi secara signifikan:

### Kompilasi dengan PyInstaller
Jalankan perintah berikut di terminal **root proyek**:

*   **Build Executable Admin** (Menyertakan visualisasi grafik):
    ```bash
    pyinstaller packaging/sigap_admin.spec
    ```
*   **Build Executable Warga** (Mengecualikan `matplotlib` dan `numpy` demi menghemat ukuran file):
    ```bash
    pyinstaller packaging/sigap_warga.spec
    ```

Hasil build executable mandiri akan terbentuk di dalam direktori `dist/SIGAP_Admin` dan `dist/SIGAP_Warga`.

### Pembuatan File Setup Installer (Inno Setup)
Gunakan aplikasi **Inno Setup Compiler** di Windows untuk membuka file skrip konfigurasi berikut dan klik menu **Compile**:
*   `packaging/installer_warga.iss` (untuk installer Warga)
*   `packaging/installer_admin.iss` (untuk installer Admin)

Installer hasil kompilasi akhir berformat `.exe` setup akan tersimpan di dalam folder `output/`.
