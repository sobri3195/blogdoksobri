# Ruang Tumbuh — Blog Editorial React

Blog Indonesia modern, mobile-first, SEO-friendly, dan siap dikonfigurasi untuk Google AdSense. Nama, niche, warna, dan target pembaca saat ini memakai identitas contoh **Ruang Tumbuh** (produktivitas sehat untuk pembaca digital Indonesia) karena brief masih menggunakan placeholder. Semua dapat diganti tanpa mengubah arsitektur.

## Stack dan struktur

- Vite, React, TypeScript, Tailwind CSS, React Router
- Route-level code splitting, dark mode, responsive layout, keyboard focus
- Metadata per halaman, JSON-LD, sitemap, robots, RSS, manifest
- Artikel terstruktur di `src/content/articles.ts` (format konten data yang aman, tanpa HTML mentah)

```text
src/
  components/      # AdSlot, SEO, cards, consent, formulir
  content/         # enam artikel dan profil penulis
  layouts/         # header, footer, navigasi
  lib/             # konfigurasi situs
  pages/           # seluruh halaman dan route
  styles/          # Tailwind/global styles
  types/           # tipe TypeScript
public/
  images/
  ads.txt
  robots.txt
  sitemap.xml
  rss.xml
  site.webmanifest
```

## Persyaratan dan instalasi

Node.js 20+ dan npm 10+ disarankan.

```bash
npm install
cp .env.example .env.local
npm run dev
```

Build produksi dan preview:

```bash
npm run build
npm run preview
```

## Environment variables

| Nama | Contoh | Keterangan |
|---|---|---|
| `VITE_SITE_URL` | `https://domainanda.com` | URL publik tanpa slash akhir |
| `VITE_SITE_NAME` | `Ruang Tumbuh` | Nama situs |
| `VITE_ADSENSE_CLIENT` | `ca-pub-XXXXXXXXXXXXXXXX` | ID klien AdSense; kosongkan sebelum disetujui |

Variabel `VITE_*` bersifat publik di bundle; jangan menaruh secret/API key. Identitas dasar juga bisa diubah di `src/lib/config.ts`, warna di `tailwind.config.js`, dan teks beranda di `src/pages/Home.tsx`.

## Menambah artikel

Tambahkan objek bertipe `Article` di `src/content/articles.ts`. Isi semua field: judul, slug unik, excerpt, URL gambar dan alt, penulis, tanggal ISO, kategori, tags, waktu baca, blok konten, dan referensi. Konten dirender sebagai node React—bukan `dangerouslySetInnerHTML`—sehingga HTML asing tidak dieksekusi. Jika berpindah ke Markdown/MDX, gunakan parser yang menonaktifkan HTML mentah atau sanitasi dengan rehype-sanitize.

Setelah menambah artikel, tambahkan URL final ke `public/sitemap.xml` dan item yang relevan ke `public/rss.xml`. Untuk proyek berskala besar, generasikan kedua file saat build.

## Metadata dan SEO

`SEO.tsx` menetapkan title, description, canonical, Open Graph, Twitter Card, robots, serta JSON-LD. Pastikan `VITE_SITE_URL` benar. Artikel menghasilkan BlogPosting dan BreadcrumbList; beranda menghasilkan WebSite/SearchAction; profil menghasilkan Person. Ubah placeholder domain pada `robots.txt`, `sitemap.xml`, dan `rss.xml` sebelum rilis. Untuk social preview terbaik, gunakan gambar absolut 1200×630 px.

## AdSense dan consent

`AdSlot` memuat script hanya sekali, hanya pada production, hanya jika `VITE_ADSENSE_CLIENT` ada, dan hanya setelah consent advertising. Development atau konfigurasi kosong menampilkan placeholder dengan ruang tercadangkan untuk mencegah CLS.

1. Ajukan situs setelah konten dan halaman legal lengkap. Persetujuan sepenuhnya keputusan Google dan **tidak dijamin**.
2. Isi `VITE_ADSENSE_CLIENT` di Vercel.
3. Ganti ID `pub-XXXXXXXXXXXXXXXX` di `public/ads.txt` dengan publisher ID asli (format ads.txt tidak memakai `ca-`).
4. Ganti ID slot contoh `1111111111`–`6666666666` pada pemanggilan `AdSlot`.
5. Verifikasi banner consent/Consent Mode v2 dengan CMP tersertifikasi jika diwajibkan di wilayah operasi.
6. Jangan mengklik iklan sendiri, membuat klik otomatis, atau menyamarkan navigasi sebagai iklan.

Kategori consent Necessary, Analytics, dan Advertising sudah disiapkan secara lokal. Implementasi produksi tetap harus disesuaikan dengan regulasi, pilihan CMP, wilayah pembaca, dan kebijakan Google terbaru.

## Deploy ke Vercel

1. Push repository ke GitHub/GitLab/Bitbucket.
2. Import proyek di Vercel.
3. Framework preset: **Vite**, build command: `npm run build`, output: `dist`.
4. Tambahkan environment variables untuk Production/Preview.
5. Deploy. `vercel.json` menyediakan SPA rewrite agar refresh pada route tidak 404 serta header keamanan dasar.
6. Pada **Project Settings → Domains**, tambahkan domain, ikuti DNS record Vercel, lalu ubah `VITE_SITE_URL` dan semua URL placeholder file publik. Deploy ulang.

Untuk Content Security Policy yang ketat, susun allowlist berdasarkan layanan yang benar-benar dipakai (termasuk domain Google AdSense bila aktif) dan uji sebelum menerapkan.

## Formulir

Newsletter dan kontak menyediakan validasi serta status aksesibel, namun sengaja tidak mengirim data tanpa backend. Hubungkan ke layanan pilihan (misalnya endpoint serverless sendiri), tambahkan proteksi spam/rate limit, perbarui kebijakan privasi, dan jangan menaruh secret di frontend.

## Checklist sebelum publikasi / pengajuan AdSense

- [ ] Ganti nama, niche, deskripsi, warna, penulis, email, dan konten contoh dengan identitas pemilik.
- [ ] Set domain asli di env, robots, sitemap, RSS, canonical, dan Search Console.
- [ ] Gunakan gambar milik sendiri/berlisensi dan optimalkan ke WebP/AVIF lokal.
- [ ] Pastikan minimal konten orisinal yang memadai serta navigasi/legal dapat diakses.
- [ ] Sambungkan dan uji formulir; perbarui kebijakan data.
- [ ] Ganti publisher ID `ads.txt`, client ID, dan seluruh slot ID contoh.
- [ ] Uji consent di semua wilayah target; gunakan CMP tersertifikasi jika perlu.
- [ ] Audit iklan agar tidak berlebihan atau berdekatan dengan navigasi/interaksi.
- [ ] Jalankan Lighthouse, cek Core Web Vitals, broken links, mobile, keyboard, dan contrast.
- [ ] Pastikan kebijakan Google Publisher terbaru dipatuhi; jangan menjanjikan kelulusan.

## Yang masih harus diisi pemilik

Identitas dalam placeholder brief, domain nyata, publisher/slot AdSense, aset gambar final, alamat kontak, endpoint formulir/newsletter, pilihan analytics/CMP, detail badan usaha, serta penyesuaian dokumen legal untuk yurisdiksi operasional. File legal di proyek adalah struktur awal, bukan nasihat hukum.
