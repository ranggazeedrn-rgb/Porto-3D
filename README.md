# Portofolio 3D Interaktif — Rangga Ramadiyanto Nurhakim

Website portofolio pribadi bergaya **minimalis elegan**: latar luar angkasa, scene 3D
interaktif di hero, dan kartu karya yang menampilkan **preview UI asli** dari tiap proyek.
Dibangun dengan React + Vite + React Three Fiber, tanpa aset 3D eksternal.

> Lighthouse (build produksi): **Accessibility 100 · Best Practices 100 · SEO 100**

---

## Daftar Isi

- [Fitur](#fitur)
- [Teknologi](#teknologi)
- [Menjalankan](#menjalankan)
- [Struktur Proyek](#struktur-proyek)
- [Kustomisasi](#kustomisasi)
- [Catatan Performa](#catatan-performa)
- [Aksesibilitas](#aksesibilitas)
- [Deploy](#deploy)
- [Kredit](#kredit)

---

## Fitur

### Tampilan & 3D

- **Hero 3D interaktif** — torus knot, orbit ring, dan partikel yang bereaksi terhadap
  kursor. Geometri, material, dan environment dibuat **prosedural** (tanpa file HDR).
- **Latar luar angkasa** — nebula, tiga lapis bintang dengan parallax, lapisan twinkle,
  dan meteor sesekali. Seluruhnya CSS, jadi tidak menambah biaya render WebGL.
- **Preview karya realistis** — setiap kartu proyek menampilkan mockup produk
  (dashboard, storefront, aplikasi mobile, viewport WebGL, dll.) yang dibangun dari
  HTML/CSS/SVG, lengkap dengan bingkai browser atau ponsel.
- **Tema terang & gelap** — toggle di navbar, tersimpan di `localStorage`, mengikuti
  preferensi sistem saat pertama dibuka. Tema diterapkan **sebelum paint** supaya tidak
  ada kedipan terang→gelap.

### Interaksi

- Kursor kustom dengan label kontekstual (`Lihat`, `Buka`, dll.).
- Progress bar scroll, navigasi **scroll-spy**, dan menu mobile.
- Tombol magnetik, teks peran berganti (typewriter), marquee perkakas.
- Reveal saat elemen masuk viewport, kartu proyek dengan tilt 3D.
- Filter proyek: **Semua / Web / Mobile / 3D** dengan animasi layout.
- Modal detail proyek: tutup lewat tombol, backdrop, atau `Esc`; fokus dipulihkan
  ke kartu asal; body scroll dikunci selama modal terbuka.
- Tombol **salin email** dengan toast konfirmasi, plus form kontak `mailto:`.

### Konten

- 8 proyek (4 Web, 2 Mobile, 2 3D) dengan deskripsi, tags, tahun, dan tautan.
- Skill bar dengan angka beranimasi, chip perkakas, dan timeline pengalaman.
- Statistik ringkas, tautan sosial, dan status ketersediaan.

### Responsif

- Layout menyesuaikan desktop, tablet, dan mobile (diuji pada lebar 390 px).
- Di layar kecil: menu mobile, navbar solid, dan elemen dekoratif berat
  (bintang jauh, meteor) dimatikan agar tetap ringan.

---

## Teknologi

| Kategori    | Pustaka                                       |
| ----------- | --------------------------------------------- |
| UI          | React 18, Vite 5                              |
| 3D          | Three.js, `@react-three/fiber`, `@react-three/drei` |
| Animasi     | Framer Motion                                 |
| Ikon        | lucide-react                                  |
| Styling     | CSS murni (custom properties + `data-theme`)  |
| Tipografi   | Fraunces (display) & Inter (body), Google Fonts |

---

## Menjalankan

**Prasyarat:** Node.js 18+ (disarankan LTS) dan npm.

```bash
npm install
npm run dev
```

Buka `http://localhost:5173`.

Build produksi lalu pratinjau hasilnya:

```bash
npm run build
npm run preview
```

Build menghasilkan `dist/` dengan chunk terpisah untuk `three`, `r3f`, dan `motion`
sehingga halaman utama tetap ringan.

### Script npm

| Perintah          | Fungsi                                            |
| ----------------- | ------------------------------------------------- |
| `npm run dev`     | Dev server Vite dengan HMR (port 5173)            |
| `npm run build`   | Build produksi ke `dist/`                         |
| `npm run preview` | Menyajikan hasil build secara lokal               |

> **Node portable:** jika `node` belum terdaftar di PATH, gunakan `dev.cmd` dan
> `build.cmd` yang ada di root proyek. Keduanya memanggil Node portable lokal.

---

## Struktur Proyek

```
portofolio/
├─ index.html              ← meta SEO/OG, font, script tema pra-paint
├─ vite.config.js          ← konfigurasi dev server & manual chunk
├─ dev.cmd / build.cmd     ← helper Node portable (Windows)
├─ public/
│  └─ favicon.svg
└─ src/
   ├─ components/
   │  ├─ Navbar.jsx            ← navigasi + toggle tema + menu mobile
   │  ├─ Footer.jsx
   │  ├─ Scene3D.jsx           ← scene React Three Fiber (pause saat offscreen)
   │  ├─ SpaceBackdrop.jsx     ← nebula, bintang, meteor (CSS)
   │  ├─ ProjectCard.jsx       ← kartu proyek + tilt 3D
   │  ├─ ProjectModal.jsx      ← modal detail + manajemen fokus
   │  ├─ ProjectPreview.jsx    ← 8 mockup mini-UI per proyek
   │  ├─ Cursor.jsx            ← kursor kustom
   │  ├─ ScrollProgress.jsx
   │  ├─ Magnetic.jsx
   │  ├─ Counter.jsx
   │  ├─ Typewriter.jsx
   │  ├─ Marquee.jsx
   │  ├─ Reveal.jsx
   │  └─ SectionHeading.jsx
   ├─ sections/
   │  ├─ Hero.jsx              ← intro + scene 3D + aura kursor
   │  ├─ Projects.jsx          ← filter + grid + modal
   │  ├─ Skills.jsx            ← skill bar + timeline
   │  └─ Contact.jsx           ← form, salin email, sosial
   ├─ theme/
   │  └─ ThemeContext.jsx      ← provider tema terang/gelap
   ├─ data/
   │  └─ site.js               ← SEMUA konten situs
   ├─ App.jsx
   ├─ main.jsx
   └─ index.css                ← design system, tema, latar angkasa, komponen
```

---

## Kustomisasi

### 1. Konten utama — `src/data/site.js`

| Ekspor        | Isi                                                                 |
| ------------- | ------------------------------------------------------------------- |
| `profile`     | Nama, peran, intro, email, lokasi, statistik, tools, tautan sosial  |
| `projects`    | Daftar proyek: judul, kategori, tipe, tahun, deskripsi, tags, link, accent, preview |
| `skillGroups` | Grup keahlian beserta level (0–100)                                 |
| `toolbox`     | Daftar perkakas untuk marquee                                       |
| `experience`  | Timeline pengalaman kerja                                           |
| `navLinks`    | Menu navigasi                                                       |

Beberapa hal yang perlu diganti sebelum publikasi:

- `profile.email` → alamat email asli.
- `profile.socials[].href` → URL profil GitHub/LinkedIn/Instagram.
- `projects[].link` → URL proyek asli (saat ini masih placeholder `'#'`).

### 2. Menambah proyek

```js
{
  id: 'nama-proyek',
  title: 'Nama Proyek',
  category: 'Web App',        // label kecil di kartu
  type: 'Web',                // 'Web' | 'Mobile' | '3D' → menentukan filter
  year: '2026',
  featured: false,            // true = kartu lebar penuh (satu saja)
  description: 'Ringkasan singkat proyek.',
  tags: ['React', 'TypeScript'],
  link: 'https://contoh.com',
  accent: ['#3d5ce0', '#8aa0ff'], // gradien cover kartu
  preview: 'dashboard',       // kunci preview mini-UI (lihat di bawah)
}
```

### 3. Preview mini-UI — `src/components/ProjectPreview.jsx`

Setiap preview adalah komponen kecil berbasis HTML/CSS/SVG (tanpa gambar) yang
diskalakan otomatis mengikuti lebar kartu lewat container query.

| Kunci (`preview`) | Tampilan                                            |
| ----------------- | --------------------------------------------------- |
| `dashboard`       | Dashboard analitik: sidebar, KPI, grafik, transaksi |
| `store`           | Storefront: navbar, hero promo, grid produk          |
| `profile`         | Situs company profile arsitektur                     |
| `system`          | Halaman design tokens: warna, tipografi, komponen   |
| `finance`         | Mockup iPhone aplikasi fintech                       |
| `habit`           | Mockup iPhone aplikasi habit tracker                 |
| `scene`           | Viewport WebGL gelap dengan orb + ring               |
| `typelab`         | Editor tipografi dengan glyph 3D                     |

Menambah preview baru:

1. Buat komponen di `ProjectPreview.jsx` yang mengembalikan `<Browser>` atau `<Phone>`.
2. Daftarkan di objek `previews`:

```js
const previews = {
  // ...
  invoice: Invoice, // komponen baru
}
```

3. Pakai kuncinya di data proyek: `preview: 'invoice'`.

Preview bersifat dekoratif — dibungkus `aria-hidden` di kartu dan modal agar tidak
mengganggu pembaca layar.

### 4. Tema & warna — `src/index.css`

Semua token desain berada di blok `:root`, lalu ditimpa di `[data-theme='dark']`:

```css
:root {
  --bg: #f4f2ee;
  --surface: #ffffff;
  --ink: #14161c;
  --muted: #5b616b;
  --line: rgba(20, 22, 28, 0.1);
  --accent: #3d5ce0;
  --radius-lg: 22px;
  /* ... */
}
```

Warna latar angkasa (nebula, bintang, meteor) ada di bagian `Space backdrop` pada
file yang sama.

### 5. Meta & SEO — `index.html`

Perbarui `description`, `og:title`, `og:description`, `theme-color`, dan `<title>`
sesuai identitas situs. Script kecil di `<head>` menerapkan tema tersimpan sebelum
halaman dilukis, jadi jangan dihapus.

---

## Catatan Performa

Optimasi yang sudah diterapkan:

- **Scene 3D dijeda saat tidak terlihat** — `Scene3D` menerima prop `active`;
  `Hero` memakai `IntersectionObserver` dan menghentikan `frameloop` ketika hero
  keluar dari viewport. Scroll di bagian lain tidak membebani GPU.
- **DPR dibatasi 1–1.5** agar tidak merender 3–4× piksel di layar retina.
- **Geometri & environment ringan** — tanpa `ContactShadows`, resolusi environment
  dikurangi, dan tidak ada tekstur/HDR eksternal.
- **Latar angkasa berbasis CSS** — bukan canvas WebGL kedua; animasi hanya
  `transform`/`opacity` sehingga diproses compositor.
- **Aura kursor di-throttle dengan `requestAnimationFrame`** dan mengubah
  `transform`, bukan `left`/`top`.
- **`backdrop-filter` dihapus** dari elemen hero/skills, dan navbar dibuat solid di
  layar ≤640 px.
- **Code splitting** — `three`, `r3f`, dan `framer-motion` dipisah ke chunk sendiri.
- **`prefers-reduced-motion`** mematikan animasi berulang (marquee, typewriter caret,
  scroll cue).

Hasil build saat ini (gzip):

| Aset              | Ukuran    |
| ----------------- | --------- |
| CSS               | ~10.5 kB  |
| App JS            | ~14.5 kB  |
| motion            | ~42.5 kB  |
| r3f               | ~104 kB   |
| three             | ~172 kB   |

---

## Aksesibilitas

- Struktur semantik (`header`, `nav`, `main`, `section`, `footer`) dan heading berurutan.
- Skip link, focus ring yang terlihat, serta `aria-label` pada tombol ikon.
- Modal: `role="dialog"`, `aria-modal`, fokus dipindah ke dialog, `Esc`/backdrop
  untuk menutup, fokus dikembalikan ke kartu asal.
- Kontras teks lolos audit Lighthouse pada tema terang maupun gelap.
- Animasi dihormati lewat `prefers-reduced-motion`; kursor kustom dinonaktifkan.
- Tema mengikuti `prefers-color-scheme` saat belum ada pilihan tersimpan.

---

## Deploy

Build command: `npm run build` · Output: `dist/`

**Vercel / Netlify** — hubungkan repo, framework preset Vite, tanpa konfigurasi tambahan.

**GitHub Pages** — karena situs disajikan dari sub-path, tambahkan `base` di
`vite.config.js`:

```js
export default defineConfig({
  base: '/nama-repo/',
  // ...
})
```

lalu deploy isi folder `dist/` (mis. dengan `gh-pages`). Situs ini berupa satu
halaman dengan navigasi anchor, jadi tidak perlu aturan rewrite tambahan.

---

## Kredit

Dibangun dengan [React](https://react.dev), [Vite](https://vitejs.dev),
[Three.js](https://threejs.org), [React Three Fiber](https://r3f.docs.pmnd.rs),
[drei](https://github.com/pmndrs/drei), [Framer Motion](https://motion.dev), dan
[lucide-react](https://lucide.dev).

Seluruh isi, desain, dan preview proyek dibuat untuk keperluan portofolio pribadi
Rangga Ramadiyanto Nurhakim.
