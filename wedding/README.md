# Undangan Pernikahan — Pajar & Tuti

Website undangan siap pakai. Isinya cuma 2 hal: `index.html` (seluruh situs) dan folder `assets/` (foto-foto kalian). Tidak perlu instalasi apa pun untuk melihatnya — klik dua kali `index.html` sudah bisa dibuka di browser.

---

## 1. Cara paling gampang untuk publish (gratis, 5 menit)

Situs ini adalah file statis biasa, jadi bisa di-hosting gratis di banyak tempat. Yang paling gampang buat pemula:

**Netlify Drop**
1. Buka https://app.netlify.com/drop
2. Seret (drag & drop) seluruh folder `wedding` (yang isinya `index.html` + `assets`) ke halaman itu
3. Netlify langsung kasih link seperti `https://nama-acak.netlify.app` — itu link undangan kalian, bisa langsung disebar

Alternatif lain yang juga gratis: Vercel, GitHub Pages, atau Firebase Hosting.

---

## 2. Supaya link bisa personal per tamu

Tambahkan `?to=NamaTamu` di akhir link. Contoh:

```
https://undangan-kalian.netlify.app/?to=Budi%20%26%20Keluarga
```

Nanti di halaman pembuka otomatis tertulis "Kepada Yth. Bapak/Ibu/Saudara/i **Budi & Keluarga**". Spasi ditulis `%20`, tanda `&` ditulis `%26`.

---

## 3. Menambahkan lagu "The Way You Look At Me — Nyoman Paul"

Saya tidak bisa menyertakan file lagunya langsung karena itu karya berhak cipta milik orang lain. Supaya tombol musik di pojok kanan bawah bisa memutar lagu itu:

1. Siapkan file mp3 lagu tersebut yang memang kalian miliki secara sah (beli/unduh resmi)
2. Ganti namanya jadi persis: `song.mp3`
3. Taruh file itu di dalam folder `assets/` (sejajar dengan foto-foto)
4. Selesai — situs otomatis memutarnya begitu tamu klik "Buka Undangan" atau tombol musik

Kalau file belum ada, tombol musik tetap muncul tapi tidak akan bersuara (tidak error, cuma diam).

---

## 4. Supaya ucapan & daftar hadiah tersimpan untuk SEMUA tamu (bukan cuma di HP masing-masing)

Saat ini, kalau `firebaseConfig` di dalam `index.html` masih kosong, situs otomatis memakai penyimpanan lokal di browser masing-masing tamu — artinya ucapan yang mereka kirim cuma tersimpan di HP mereka sendiri, dan tidak akan terlihat oleh tamu lain atau oleh kalian. Ini cukup untuk sekadar melihat tampilannya, tapi belum "beneran jalan" untuk dipakai di hari-H.

Supaya semua ucapan & daftar hadiah tersimpan di satu tempat dan bisa dilihat semua orang (termasuk kalian), pakai **Firebase Firestore** — gratis untuk skala undangan pernikahan. Langkahnya:

1. Buka https://console.firebase.google.com, login dengan akun Google, klik **Add project** → beri nama bebas (misal `undangan-pajar-tuti`) → ikuti langkah sampai selesai
2. Di dashboard project, klik ikon **`</>`** (Web app) → beri nama app → klik **Register app**
3. Firebase akan menampilkan kode seperti ini:
   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "undangan-pajar-tuti.firebaseapp.com",
     projectId: "undangan-pajar-tuti",
     storageBucket: "undangan-pajar-tuti.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };
   ```
   Salin nilai-nilai itu.
4. Di menu kiri Firebase, klik **Build → Firestore Database → Create database** → pilih **Start in test mode** → pilih lokasi server (pilih yang dekat, misal `asia-southeast2 (Jakarta)`) → Create
5. Buka file `index.html` di editor teks apa saja (Notepad, VS Code, dll), cari bagian ini (gunakan Ctrl+F, cari kata `firebaseConfig`):
   ```js
   const firebaseConfig = {
     apiKey: "",
     authDomain: "",
     projectId: "",
     storageBucket: "",
     messagingSenderId: "",
     appId: ""
   };
   ```
   Ganti tanda kutip kosong `""` dengan nilai yang kalian salin dari langkah 3.
6. Simpan file, lalu upload ulang ke hosting (ulangi langkah drag & drop Netlify di atas)

Setelah ini, setiap ucapan dan konfirmasi hadiah yang dikirim tamu akan tersimpan permanen di Firestore dan langsung muncul (real-time) di semua perangkat yang membuka undangan — termasuk untuk kalian pantau.

**Catatan keamanan:** "Test mode" di Firestore membuat data bisa ditulis siapa saja selama 30 hari — cukup aman untuk keperluan undangan pernikahan yang jangka waktunya pendek. Kalau mau lebih aman, kalian bisa atur Firestore Rules supaya cuma bisa menambah data baru (create) dan tidak bisa menghapus/mengubah data orang lain — tapi ini opsional, tidak wajib untuk situs tetap berjalan.

---

## 5. Mengganti isi (teks, foto, tanggal, dll)

Semua teks dan pengaturan ada di satu file `index.html`, dicari gampang karena teksnya sama persis dengan yang tampil di layar. Beberapa yang paling sering diubah:

| Yang mau diubah | Cari kata kunci ini di `index.html` |
|---|---|
| Nomor rekening / nama pemilik | `1300 0264 9298 6` dan `a.n. Tuti Herawati` (untuk tombol salin, ubah juga angka di dalam `copyBtn.addEventListener`, cari `1300026492986`) |
| Alamat lokasi acara | `Kp. Mulyasari Cibisoro` |
| Tanggal & jam acara | `06 September 2026` dan cari `2026-09-06T08:00:00` (dipakai hitung mundur & kalender) |
| Nama orang tua mempelai | cari `Bapak Asep Muslihat` / `Bapak Ato Suharto` |
| Foto-foto | ganti file di folder `assets/` tapi **nama filenya harus tetap sama** (`photo-forest-bride.jpg`, dst), atau ganti nama filenya di `index.html` sekalian |
| Kalimat ucapan penutup | cari `Merupakan suatu kehormatan` |

---

## 6. Struktur file

```
wedding/
├── index.html          ← seluruh website (HTML + CSS + JS jadi satu)
├── README.md            ← file ini
└── assets/
    ├── photo-forest-bride.jpg
    ├── photo-forest-groom.jpg
    ├── photo-beach-black.jpg
    ├── photo-sunset-face.jpg
    ├── photo-sunset-embrace.jpg
    ├── photo-formal-lake.jpg
    └── song.mp3          ← belum ada, tambahkan sendiri (lihat bagian 3)
```

Selamat menempuh hidup baru, Pajar & Tuti! 🤍
