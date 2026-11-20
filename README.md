# Tugas 4 - Layout Manager

Proyek `p04-layout-manager` berisi tiga JFrame Form dengan tema FlatLaf 3.5.4:

| Form | Keterangan |
|------|------------|
| `GaleriLayout` | Galeri BorderLayout, FlowLayout, GridLayout, BoxLayout, dan layout bersarang (Praktikum 2) |
| `FormBiodata` | Form biodata dengan GridBagLayout (Praktikum 4) |
| `FormAnggotaPerpustakaan` | Form pendaftaran anggota perpustakaan (Studio Desain) |

Setiap form memiliki berkas `.java` dan `.form` di `src/main/java/id/ac/uniska/pbo2/p04/`.

## Cara Menjalankan

Buka proyek di Apache NetBeans (JDK 21), lalu klik kanan form dan pilih **Run File** (Shift+F6).
Dengan Maven di terminal:

```bash
mvn compile exec:java
```

## Studio Desain: Form Anggota Perpustakaan

### Tahap 1: Foto Sketsa

![Sketsa form](screenshots/sketsa.jpg)

### Tahap 2: Tabel Rancangan

| Bagian | Panel | Layout |
|--------|-------|--------|
| Jendela | JFrame | BorderLayout |
| Header (North) | `headerPanel` | BoxLayout (Y Axis) |
| Isi form (Center) | `isiPanel` berisi `formPanel` di North | `isiPanel`: BorderLayout; `formPanel`: GridBagLayout |
| Pilihan jenis anggota | `jenisPanel` (di dalam `formPanel`) | GridLayout (1 baris, 3 kolom) |
| Tombol (South) | `tombolPanel` | FlowLayout (Alignment Right) |

### Tahap 3: Tangkapan Layar

Ukuran awal:

![Ukuran awal](screenshots/anggota-awal.png)

Saat dilebarkan:

![Saat dilebarkan](screenshots/anggota-lebar.png)

Tab Design dengan jendela Navigator:

![Tab Design](screenshots/anggota-design-navigator.png)

### Tahap 4: Saran dari Teman

Diperiksa oleh: _(nama teman)_

| No | Pemeriksaan | Ya atau Belum |
|----|-------------|---------------|
| 1 | Label singkat, jelas, dan sejajar | |
| 2 | Isian dikelompokkan dengan judul bagian | |
| 3 | Placeholder dipakai untuk isian yang punya format | |
| 4 | Komponen sesuai jenis data | |
| 5 | Tombol utama menonjol dan berada di kanan bawah | |
| 6 | Jarak antar komponen konsisten | |
| 7 | Form tetap rapi saat jendela dilebarkan dan ditinggikan | |
| 8 | Tampilan dibuat dengan JFrame Form dan layout manager, bukan Null Layout atau Absolute Layout | |

Saran perbaikan: _(isi saran dari teman)_

## Jawaban Eksperimen Praktikum 2 (GaleriLayout)

1. **`flowPanel`: Alignment dari Left menjadi Center, lalu jendela dikecilkan.**
   Saat tombol tidak muat dalam satu baris, FlowLayout memindahkan tombol ke baris berikutnya.
   Dengan Left, setiap baris dimulai dari tepi kiri. Dengan Center, setiap baris diletakkan di tengah
   panel (pada lebar 300 px, baris pertama bergeser dari x=20 menjadi x=38). Baris yang berubah di
   `initComponents()` adalah `flowPanel.setLayout(new java.awt.FlowLayout(java.awt.FlowLayout.CENTER, 8, 8));`
   (sebelumnya `FlowLayout.LEFT`).

2. **`gridPanel`: Rows 0 dan Columns 2.**
   Terbentuk **5 baris**. Nilai Rows 0 berarti jumlah baris dihitung otomatis dari jumlah komponen dan
   jumlah kolom: 9 tombol dibagi 2 kolom = 4,5, dibulatkan ke atas menjadi 5. Baris terakhir hanya berisi
   tombol 9 di kolom kiri, sedangkan sel kanannya kosong. Barisnya menjadi
   `new java.awt.GridLayout(0, 2, 8, 8)`.

3. **`boxPanel`: `maximumSize` lebar 32767 dan Vertical Strut.**
   Sebelumnya setiap tombol hanya selebar ukuran aslinya (sekitar 82 sampai 132 px) dan rata kiri.
   Setelah `maximumSize` diisi, semua tombol melebar sama lebar memenuhi panel. BoxLayout hanya
   melebarkan komponen sampai batas ukuran maksimumnya. Vertical Strut menambah jarak tetap antar tombol
   (misalnya 12 px) tanpa mengubah lebar tombol.

## Jawaban Eksperimen Praktikum 4 (FormBiodata)

1. **Weight X `namaField` menjadi 0.0, lalu jendela dilebarkan.**
   Hasilnya tidak ada perubahan: `namaField` tetap ikut melebar. Bobot sebuah kolom di GridBagLayout
   ditentukan oleh nilai terbesar di kolom itu, dan isian lain di kolom yang sama (NPM, Email, dan lainnya)
   masih ber-Weight X 1.0, sedangkan Fill Horizontal membuat `namaField` memenuhi selnya.
   Isian baru berhenti melebar jika **semua** isian di kolom itu 0.0. Saat itu tidak ada sisa ruang
   yang dibagikan, sehingga kolom label dan isian tetap pada ukuran aslinya dan seluruh susunan
   berada di tengah panel, tidak lagi menempel di kiri.

2. **Direction `formPanel` dari North menjadi Center, lalu jendela ditinggikan.**
   Dengan North, `formPanel` setinggi kebutuhannya dan menempel di atas; ruang kosong ada di bawah.
   Dengan Center, `formPanel` direntangkan memenuhi seluruh tinggi `isiPanel`. Tinggi baris tidak ikut
   membesar (Weight Y 0.0), sehingga GridBagLayout menaruh seluruh blok isian di **tengah vertikal**:
   judul "Data Diri" turun dari y=8 menjadi y=157 pada tinggi jendela 700 px.
   Karena itu `formPanel` dibungkus `isiPanel` dan diletakkan di North, supaya baris form tetap di atas.

3. **Baris `setMinimumSize(getSize())` dihapus, lalu jendela dikecilkan sekecil mungkin.**
   Pada hitungan tata letak, kolom label tidak mau menyempit dari ukuran aslinya (84 px) sehingga
   yang menyusut lebih dulu adalah kolom isian: lebar `namaField` turun dari 223 px menjadi 22 px
   pada lebar jendela 150 px dan menjadi 0 px pada 100 px. Teks keterangan di header juga terpotong,
   dan tombol Batal dan Simpan yang berada di FlowLayout pindah ke dua baris lalu terpotong.
   Dengan `setMinimumSize`, jendela tidak dapat dikecilkan melebihi ukuran awalnya sehingga kerusakan
   ini tidak terjadi.
   _Catatan: hasil nomor 3 ini dihitung dari perilaku layout, bukan diamati di jendela asli.
   Coba sendiri di NetBeans dan sesuaikan dengan yang kamu lihat._

## Jawaban Pertanyaan Refleksi

**1. Mengapa Null Layout dan Absolute Layout sebaiknya dihindari?**
Keduanya memakai posisi dan ukuran piksel yang tetap. Akibatnya tampilan tidak menyesuaikan ukuran
jendela dan teks dapat terpotong pada skala layar atau huruf yang berbeda. Absolute Layout juga
membutuhkan library tambahan NetBeans sehingga aplikasi sulit dijalankan di komputer lain.

**2. Perbedaan perilaku FlowLayout dan GridLayout saat jendela dilebarkan.**
FlowLayout mempertahankan ukuran asli tiap komponen dan hanya menyusunnya berderet, pindah baris jika
tidak muat; komponen tidak ikut melebar. GridLayout membagi ruang menjadi sel yang sama besar, sehingga
semua komponen ikut melebar dan meninggi mengikuti ukuran jendela.

**3. Fungsi Weight X dan Fill pada GridBagLayout.**
Weight X menentukan kolom mana yang mendapat sisa lebar saat jendela dilebarkan; kolom ber-bobot 0
tetap pada lebar aslinya. Fill menentukan apakah komponen memenuhi sel yang disediakan (Horizontal,
Vertical, Both) atau tetap pada ukuran aslinya. Isian form memakai Weight X 1.0 dan Fill Horizontal
agar melebar, sedangkan label dibiarkan 0.0 dan None.

**4. Mengapa `formPanel` dibungkus `isiPanel` dan diletakkan di North?**
Di Center, `formPanel` direntangkan sampai tinggi jendela, dan GridBagLayout menaruh barisnya di tengah
vertikal sehingga muncul ruang kosong di atas dan di bawah. Dengan pembungkus BorderLayout dan
`formPanel` di North, panel hanya setinggi kebutuhannya dan barisnya tetap di atas saat jendela
ditinggikan.

**5. Dua prinsip desain pada Form Anggota Perpustakaan dan buktinya.**
- *Kedekatan*: isian yang berkaitan dikelompokkan dengan judul bagian "Data Anggota", "Kontak", dan
  "Persetujuan", dengan jarak atas lebih besar di antara bagian.
- *Keselarasan*: semua label berada di kolom 0 (rata kiri) dan semua isian di kolom 1 yang sama lebar,
  sehingga membentuk garis lurus dari atas ke bawah. Ini dihasilkan oleh GridBagLayout dengan Weight X
  1.0 dan Fill Horizontal pada kolom isian.

## Deklarasi Penggunaan AI

_(Edit bagian ini agar sesuai dengan keadaan sebenarnya.)_

Saya menggunakan Claude (Anthropic) untuk membantu menyusun kerangka proyek `p04-layout-manager`,
termasuk berkas `.java`, `.form`, `pom.xml`, serta draf isi README ini. Saya sudah membuka dan memeriksa
seluruh form di Apache NetBeans, mengambil sendiri semua tangkapan layar, membuat sketsa, dan memahami
kode yang dihasilkan.
