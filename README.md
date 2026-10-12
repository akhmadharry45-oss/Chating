# Chat Kelas (versi GitHub + Firebase)

Aplikasi chat kelas dalam satu file `index.html`. Isinya: chat dengan stiker dan emoji, pengumuman, jadwal pelajaran, absensi, dan tugas. Data disimpan di Firebase (paket gratis Spark sudah cukup untuk satu kelas), dan halamannya dihosting di GitHub Pages.

Ada tiga file:

- `index.html`: aplikasinya.
- `firestore.rules`: aturan keamanan database. Wajib dipasang.
- `README.md`: panduan ini.

## Cara kerja masuk

- **Admin** masuk dengan akun Google. Hanya email yang Anda tulis di `ADMIN_EMAILS` dan `firestore.rules` yang dianggap admin.
- **Siswa** tidak perlu akun. Mereka mengisi nama dan **kode kelas** yang Anda buat. Peramban mengingat mereka, jadi mereka tidak perlu mengisi lagi, kecuali data peramban dihapus.

## Langkah pemasangan

### 1. Buat proyek Firebase
1. Buka https://console.firebase.google.com dan masuk dengan akun Google admin.
2. Klik **Add project**, beri nama (misalnya `chat-kelas-5a`), Google Analytics boleh dimatikan.

### 2. Daftarkan aplikasi web
1. Di halaman proyek, klik ikon **Web** (`</>`) lalu beri nama aplikasi. Tidak perlu mencentang Firebase Hosting.
2. Firebase menampilkan blok `firebaseConfig`. Salin isinya ke `FIREBASE_CONFIG` di bagian atas script `index.html`.
3. Ganti `GANTI_DENGAN_EMAIL_ADMIN@gmail.com` di `ADMIN_EMAILS` dengan email Google admin.

### 3. Aktifkan cara masuk
1. Buka **Build > Authentication > Get started > Sign-in method**.
2. Aktifkan **Google** (pilih email dukungan proyek).
3. Aktifkan **Anonymous** (dipakai siswa).
4. Buka tab **Settings > Authorized domains** lalu **Add domain**: `NAMAPENGGUNA.github.io` (ganti dengan nama akun GitHub Anda).

### 4. Buat database dan pasang aturan
1. Buka **Build > Firestore Database > Create database**. Pilih mode **production** dan lokasi `asia-southeast2` (Jakarta).
2. Buka tab **Rules**, hapus isinya, tempel seluruh isi `firestore.rules`.
3. Ganti `GANTI_DENGAN_EMAIL_ADMIN@gmail.com` di fungsi `admins()` dengan email admin yang sama dengan langkah 2, lalu klik **Publish**.

### 5. Unggah ke GitHub Pages
1. Buat repositori baru di GitHub (publik), misalnya `chat-kelas`.
2. Unggah `index.html` ke repositori (tombol **Add file > Upload files**). `firestore.rules` dan `README.md` boleh ikut diunggah.
3. Buka **Settings > Pages**. Pada **Branch**, pilih `main` dan folder `/ (root)`, lalu **Save**.
4. Tunggu satu sampai dua menit. Alamatnya: `https://NAMAPENGGUNA.github.io/chat-kelas/`.

### 6. Pakai pertama kali
1. Buka alamat tadi, klik **Masuk dengan Google**, pakai akun admin.
2. Klik **Pengaturan**, isi nama kelas dan **kode kelas** (misalnya `bintang5a`), lalu **Simpan**.
3. Bagikan alamat halaman dan kode kelas ke siswa lewat grup yang sudah ada.

## Catatan penting

- Kunci di `FIREBASE_CONFIG` memang terlihat oleh siapa pun yang membuka halaman. Itu normal untuk Firebase. Yang menjaga data adalah `firestore.rules`, jadi pastikan aturan sudah dipublikasikan.
- Siswa yang menghapus data peramban atau pindah HP harus mengisi nama dan kode kelas lagi. Mereka akan tercatat sebagai anggota baru, jadi di daftar absen bisa muncul nama ganda.
- Siswa hanya bisa mengisi absennya sendiri, dan hanya saat sesi absensi dibuka.
- Admin bisa menghapus pesan siapa pun dari menu ketuk pesan, dan mengelola pengumuman, jadwal, absensi, dan tugas.
- Batas gratis Firebase (sekitar 50.000 pembacaan dan 20.000 penulisan per hari) cukup untuk satu kelas yang aktif. Pantau di Firebase Console > Usage.
- Setelah kelas selesai, hapus data lewat Firebase Console atau hapus proyeknya.

## Tema dan lampiran tugas

- **Tema:** tombol **Pengaturan** tersedia untuk semua orang. Mode Otomatis, Terang, atau Gelap tersimpan per perangkat. Admin juga memilih **warna kelas** yang dilihat semua anggota, lalu menekan **Simpan**.
- **Lampiran tugas:** saat menambah tugas, admin bisa menempel tautan (misalnya Google Drive) dan melampirkan sampai 3 file. Setiap file maksimal 700 KB. Foto otomatis dikecilkan. File besar sebaiknya diunggah ke Google Drive dan ditempel sebagai tautan.
- File lampiran disimpan di Firestore, bukan di Firebase Storage, supaya tetap gratis. Setelah memperbarui `index.html`, tempel juga isi `firestore.rules` yang baru ke tab Rules lalu Publish, kalau tidak lampiran file akan ditolak.

## Pasang di HP seperti aplikasi

Berkas `manifest.webmanifest`, `sw.js`, dan folder `icons` membuat halaman ini bisa dipasang di layar utama HP, lengkap dengan ikon sendiri dan tampil layar penuh tanpa kolom alamat. Unggah semuanya ke repositori GitHub bersama `index.html`, termasuk folder `icons`.

- **Android (Chrome):** buka alamat GitHub Pages, lalu klik tombol **Pasang aplikasi** di Pengaturan atau layar masuk. Kalau tombolnya belum muncul, buka menu titik tiga dan pilih **Pasang aplikasi** atau **Tambahkan ke layar utama**.
- **iPhone (Safari):** buka alamat itu di Safari, ketuk tombol **Bagikan**, lalu **Tambahkan ke Layar Utama**.

### Menjadi file APK atau masuk Play Store
1. Buka https://www.pwabuilder.com, tempel alamat GitHub Pages Anda, lalu klik **Start**.
2. Klik **Package for stores**, pilih **Android**, lalu unduh paketnya. Anda mendapat file `.apk` untuk dibagikan langsung dan file `.aab` untuk Play Store, beserta kunci penandatangan. Simpan kunci itu baik-baik.
3. Berbagi file `.apk` langsung (misalnya lewat grup WhatsApp) gratis, tetapi setiap HP harus mengizinkan pemasangan dari sumber tidak dikenal.
4. Untuk Play Store: daftar akun pengembang Google Play (biaya sekali bayar sekitar 25 dolar AS). Akun pribadi baru wajib menjalankan uji tertutup dengan minimal 12 penguji selama 14 hari berturut-turut sebelum bisa merilis ke publik.
