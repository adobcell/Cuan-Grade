# DEROID SCRAP / UNBRICKER NOTA — Source Project

Project Android Studio untuk varian **Member** dan **Admin**.

## Perubahan V3: nota hanya untuk penjual

- Nota servis/pelanggan (halaman dashboard dan `NotaActivity`) dihapus. Satu-satunya nota adalah nota pembelian dari penjual.
- Perbaikan: `formatRp` tidak pernah didefinisikan sehingga tombol WA nota selalu error; kini sudah ada.
- Nomor nota otomatis dan unik (`#SCRAP-YYMMDD-001`), tidak lagi tetap `#SCRAP-8841`.
- Nama/nomor penjual tidak lagi terisi contoh (0812-3456-7890), supaya WA tidak terkirim ke nomor palsu.
- Nama barang di-escape, jadi karakter seperti `<` atau `&` tidak merusak tampilan nota.
- Item bisa diubah (tombol pensil), draft nota tersimpan otomatis, ada Riwayat Nota (buka/cetak ulang, kirim WA, hapus) dan tombol Simpan & Nota Baru.
- Google Sheets: satu baris per nota (diperbarui, tidak dobel), plus backup lengkap harian. Pasang ulang `Code.gs` dan deploy versi baru.

## Perubahan pada paket perbaikan

- Memperbaiki payload Gemini Vision agar memakai nama properti REST API yang benar: `inlineData` dan `mimeType`.
- Mengarahkan OCR Gemini ke model `gemini-2.5-flash`.
- Memperjelas pesan error HTTP dari Google API agar penyebab kegagalan lebih mudah diketahui.
- Memvalidasi mode dan panjang API Key sebelum menyimpan ke Android SharedPreferences.
- Menyinkronkan konfigurasi OCR native ke cache LocalStorage dan memverifikasi hasil simpan API Key.
- Menyamakan validasi nomor WhatsApp untuk nota pembelian scrap dan nota servis.
- Menyesuaikan Gradle Wrapper ke Gradle 8.9 agar sejalan dengan Android Gradle Plugin 8.7.3.
- Mempertahankan tampilan, aset, database, dan fitur aplikasi yang sudah ada.

## Membuka di Android Studio

1. Ekstrak ZIP ini ke folder biasa, misalnya `DeroidScrapFixed`.
2. Buka Android Studio → **Open** → pilih folder hasil ekstrak (folder yang berisi `settings.gradle`).
3. Pastikan Android SDK Platform 35 terpasang melalui SDK Manager.
4. Gunakan JDK 17 atau JDK yang disarankan Android Studio untuk AGP 8.7.3.
5. Tunggu Gradle Sync selesai. Koneksi internet diperlukan saat pertama kali mengunduh Gradle dan dependensi.
6. Untuk APK Member pilih task `app > Tasks > build > assembleMemberDebug`.
7. Untuk APK Admin pilih task `app > Tasks > build > assembleAdminDebug`.

APK debug akan dibuat di:

- `app/build/outputs/apk/member/debug/`
- `app/build/outputs/apk/admin/debug/`

## Pengaturan API OCR

- Offline ML Kit tidak memerlukan API Key.
- Gemini Vision memerlukan Gemini API Key yang aktif dan akses ke Generative Language API.
- Google Cloud Vision memerlukan API Key Google Cloud dan Cloud Vision API yang aktif pada project Google Cloud.
- API Key disimpan di SharedPreferences aplikasi pada perangkat. Jangan membagikan APK yang berisi atau mengungkap API Key pribadi.

## Pengiriman nota WhatsApp

Tombol nota menyusun teks lalu membuka WhatsApp dengan nomor tujuan dan isi pesan. Pengguna tetap perlu menekan **Kirim** di WhatsApp. Aplikasi tidak dapat memastikan pesan terkirim hanya dari keberhasilan membuka WhatsApp. Pastikan nomor pelanggan aktif dan menggunakan nomor WhatsApp dengan kode negara Indonesia.

## Status verifikasi paket

- Pemeriksaan sintaks JavaScript inline pada `scrap_engine.html`: lulus.
- Pemeriksaan source dan perubahan konfigurasi: selesai.
- Build APK pada lingkungan penyusunan paket ini: belum dapat dijalankan karena Gradle 8.9 perlu diunduh dari internet, sedangkan akses jaringan build tidak tersedia. Jalankan Gradle Sync/build di Android Studio dengan koneksi internet.
