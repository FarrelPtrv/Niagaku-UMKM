# StokIn — Manajemen Stok & Penjualan UMKM

Paket awal GitHub untuk memublikasikan versi website StokIn dan membangun APK Android dari HTML yang tersedia.

## Isi paket

- `www/index.html` — kode aplikasi StokIn.
- `capacitor.config.ts` — pengaturan nama aplikasi dan ID Android.
- `.github/workflows/build-android-apk.yml` — membuat APK debug melalui GitHub Actions.
- `.github/workflows/deploy-pages.yml` — menerbitkan website melalui GitHub Pages.

## Cara memasukkan ke GitHub

1. Unduh dan ekstrak ZIP proyek ini.
2. Buat repository baru di GitHub, misalnya `stokin-umkm`. Agar GitHub Pages dapat digunakan pada GitHub Free tanpa batasan akses khusus organisasi, pilih repository **Public**. Jangan menambahkan kata sandi, token, atau data usaha asli ke repository.
3. Cara paling mudah untuk mengunggah seluruh folder termasuk `.github`: pasang GitHub Desktop, pilih **File → Add Local Repository** jika foldernya sudah merupakan repository, atau **File → Clone Repository** untuk repository kosong lalu salin isi folder proyek ini ke folder hasil clone.
4. Di GitHub Desktop, commit perubahan lalu tekan **Push origin**. Pastikan branch bernama `main`.
5. Di halaman repository GitHub, buka **Settings → Pages**. Pada bagian **Build and deployment**, pilih **GitHub Actions** sebagai Source.

## Mendapatkan APK dari GitHub Actions

Workflow akan otomatis mencoba membangun APK setiap kali ada perubahan yang dikirim ke branch `main`.

1. Buka repository → tab **Actions**.
2. Pilih workflow **Build Android APK**. Jika belum berjalan otomatis, klik **Run workflow**.
3. Buka proses yang terbaru dan pastikan statusnya **Success**.
4. Pada bagian **Artifacts**, unduh `StokIn-APK-debug`.
5. Ekstrak ZIP artifact tersebut. File `app-debug.apk` adalah APK untuk pengujian di perangkat Android.
6. Pindahkan APK ke HP Android uji, instal, lalu uji semua menu dan transaksi.

APK yang dihasilkan adalah **debug APK untuk pengujian**, bukan rilis bertanda tangan produksi untuk Play Store. Jangan menganggap build berhasil sebelum workflow menunjukkan **Success** dan APK berhasil dipasang serta diuji.

## Mendapatkan website

Setelah GitHub Pages diaktifkan melalui **Settings → Pages → Source: GitHub Actions**, buka tab **Actions** dan pilih workflow **Publish StokIn website**. Setelah deployment berhasil, tautan website muncul pada hasil workflow atau halaman Settings → Pages.

## Build di komputer sendiri (opsional)

Persyaratan Capacitor 8: Node.js 22 atau lebih baru, Android Studio, Android SDK, dan Java yang disediakan oleh Android Studio.

Buka terminal di folder proyek lalu jalankan:

```bash
npm install
npx cap add android
npx cap sync android
npx cap open android
```

Di Android Studio, tunggu Gradle selesai sync, kemudian pilih **Build → Build Bundle(s) / APK(s) → Build APK(s)**. APK debug umumnya ada di `android/app/build/outputs/apk/debug/app-debug.apk`.

Jika folder `android` sudah dibuat sebelumnya, jangan jalankan `npx cap add android` lagi; cukup gunakan `npx cap sync android` setelah memperbarui `www/index.html`.

## Batasan penting versi sekarang

- Aplikasi menyimpan nama toko, daftar produk, dan transaksi menggunakan `localStorage`. Artinya data tersimpan di browser/WebView pada perangkat tersebut; data tidak tersinkron otomatis ke perangkat lain dan belum ada akun/login.
- Menghapus data aplikasi atau browser dapat menghapus data lokal. Buat cadangan sebelum memakai data penting.
- Kode masih memakai pustaka eksternal melalui CDN (Tailwind CSS, Lucide, Chart.js, Google Fonts), sehingga koneksi internet diperlukan agar seluruh tampilan dan grafik dapat dimuat dengan benar.
- Beberapa laporan masih berupa data contoh/perhitungan sederhana dan perlu divalidasi sebelum digunakan untuk pencatatan bisnis nyata.
- APK Android hanya berjalan pada perangkat Android. Untuk iPhone, gunakan website atau buat target iOS secara terpisah (membutuhkan Mac dan Xcode).
- Repository **Public** membuat kode sumber dapat dilihat orang lain. Jangan pernah menyimpan kredensial, kunci API rahasia, atau data pelanggan di dalamnya.

## Rekomendasi sebelum dipakai UMKM sungguhan

1. Uji barang masuk, penjualan, stok minus, edit/hapus barang, laporan, dan pemulihan saat aplikasi ditutup lalu dibuka lagi.
2. Perbaiki laporan agar angka berasal dari transaksi nyata dan tanggalnya dicatat secara konsisten.
3. Jika data perlu dipakai di banyak perangkat atau oleh banyak pegawai, tambahkan backend/database online, autentikasi, dan aturan akses per toko (misalnya Supabase dengan Row Level Security).
4. Buat backup/ekspor data, lalu uji pemulihan backup.
5. Untuk distribusi luas, buat rilis Android bertanda tangan (release keystore) dan lindungi kunci penandatanganannya; jangan masukkan kunci tersebut ke repository.
