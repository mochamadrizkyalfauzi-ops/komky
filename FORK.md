# KomikKu — fork dari Mihon v0.19.7

Proyek ini adalah turunan (fork) dari [Mihon](https://github.com/mihonapp/mihon) v0.19.7
(commit 54d2c7b), dilisensikan Apache License 2.0 (lihat `LICENSE`).
Sesuai lisensi, file `LICENSE` dipertahankan dan perubahan dicatat di bawah.

## Perubahan dari Mihon asli (tahap 1: rebrand saja, fungsi tetap sama)

| Apa | File | Isi perubahan |
|---|---|---|
| ID aplikasi | `app/build.gradle.kts` | `app.mihon` -> `id.komikku.app` |
| Nama aplikasi | `i18n/src/commonMain/moko-resources/base/strings.xml` | `Mihon` -> `KomikKu` |
| Nama proyek Gradle | `settings.gradle.kts` | `Mihon` -> `KomikKu` |
| Ikon peluncur & notifikasi | `app/src/main/res/drawable/ic_launcher_*.xml`, `ic_mihon.xml`, `app/src/debug/res/drawable/` | Diganti ikon buku terbuka (desain baru) |
| Daftar paket telemetri | `telemetry/.../TelemetryConfig.kt` | Menyesuaikan ID baru (telemetri nonaktif secara default) |

Nama paket Kotlin (`eu.kanade.*`, `mihon.*`) sengaja TIDAK diubah di tahap ini agar
kode tetap aman dibuild. Redirect login tracker (`mihon://...`) juga dibiarkan.

## Yang perlu kamu ganti sendiri (satu tempat masing-masing)
- Nama tampilan: `i18n/.../base/strings.xml` (`app_name`).
- ID aplikasi: `app/build.gradle.kts` (`applicationId`). Ganti `id.komikku.app`
  dengan ID milikmu, misalnya `com.namakamu.komik`.
- Warna ikon: `ic_launcher_background.xml` (`#5B3FD9`).

## Cara build (di komputer kamu)
Kebutuhan: Android Studio terbaru, JDK 17, koneksi internet (untuk unduh dependensi).

1. Ekstrak ZIP, lalu di Android Studio pilih **File > Open** dan buka folder proyek.
2. Tunggu Gradle Sync selesai (pertama kali bisa beberapa menit). Android Studio akan
   menawarkan unduh Android SDK (compileSdk 36) bila belum ada, setujui saja.
3. Pilih build variant **debug**: menu **Build > Select Build Variant**.
4. Jalankan **Build > Build APK(s)**, atau lewat terminal:
   `./gradlew assembleDebug` (Windows: `gradlew.bat assembleDebug`).
5. APK ada di `app/build/outputs/apk/debug/`. Pakai yang bertulisan `universal`
   bila ada, atau yang sesuai arsitektur HP-mu (biasanya `arm64-v8a`).
6. APK debug punya ID `id.komikku.app.dev` dan ikon oranye (beda dari rilis).

Untuk APK rilis kamu perlu keystore sendiri (tanda tangan). Bisa dibahas nanti.

## Catatan
- Pengecek update aplikasi dimatikan (default Mihon: hanya aktif dengan flag `enable-updater`),
  supaya tidak menawarkan update dari Mihon asli.
- Aplikasi ini hanya pembaca. Tidak ada sumber komik bawaan. Ekstensi dipasang pengguna sendiri.

## Build lewat GitHub Actions (tanpa Android Studio)
Workflow ada di `.github/workflows/apk.yml`. Workflow bawaan Mihon (release, update website)
sudah dihapus karena butuh rahasia/akses milik tim Mihon.

1. Buat repo baru di GitHub (kosong, tanpa README).
2. Dari folder proyek: `git init`, `git add .`, `git commit -m "KomikKu awal"`,
   `git branch -M main`, `git remote add origin <URL-repo-kamu>`, `git push -u origin main`.
   (Atau pakai tombol Source Control di VS Code / GitHub Desktop.)
3. Buka repo di GitHub, tab **Actions**. Build jalan otomatis setiap push, atau klik
   **Build APK > Run workflow** untuk menjalankan manual.
4. Setelah centang hijau (sekitar 10-20 menit), buka run tersebut, bagian **Artifacts**,
   unduh `KomikKu-debug-apk` (berupa ZIP berisi APK), ekstrak, lalu pasang di HP.
