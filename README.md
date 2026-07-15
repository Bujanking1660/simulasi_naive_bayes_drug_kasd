# Naive Bayes Manual — Prediksi Obat (Dataset Drug200)

Notebook ini membangun model **Naive Bayes untuk data kategorik dari nol**
(Prior & Likelihood dihitung manual, **tanpa scikit-learn**), dibantu **pandas**
& **numpy** untuk pengolahan data, dan **seaborn** / **matplotlib** untuk visualisasi.

---

## Struktur Folder

```
.
├── README.md
├── drug200_naive_bayes.xlsx    ← simulasi Excel (lengkap dengan CW)
├── main_v2.ipynb               ← implementasi tanpa class weight
├── main_v3.ipynb               ← implementasi dengan class weight
└── data/
    └── drug200(NAIVE BAYES).xlsx
```

> Notebook membaca dataset dari `./data/drug200(NAIVE BAYES).xlsx`.

---

## Dataset

`drug200` berisi **200 data pasien** dengan 4 fitur **kategorikal** dan 1 label kelas:

| Kolom | Tipe | Nilai |
|---|---|---|
| `Age` | kategorikal | YOUNG, ADULT, OLD |
| `BP` | kategorikal | HIGH, NORMAL, LOW |
| `Cholesterol` | kategorikal | HIGH, NORMAL |
| `Na_to_K` | kategorikal | HIGH, NORMAL, LOW |
| `Drug` (target) | kategorikal | DrugY, drugX, drugA, drugB, drugC |

> **Catatan:** Kolom `Na_to_K` pada file Excel sudah dikategorikan menjadi HIGH / LOW / NORMAL
> sehingga diperlakukan sama seperti fitur kategorik lainnya.

---

## Notebook

### main_v2.ipynb (tanpa class weight)
| No. | Bagian | Keterangan |
|---|---|---|
| 1 | **Setup Library** | Import pandas, numpy, matplotlib, seaborn |
| 2 | **Load Data** | Baca file Excel, tampilkan 5 data teratas |
| 3 | **EDA** | Visualisasi distribusi fitur terhadap jenis obat |
| 4 | **Stratified Split 80:20** | Split manual tanpa sklearn (160 train, 40 test) |
| 5 | **Prior P(Drug)** | Hitung probabilitas prior dari data train |
| 6 | **Likelihood P(Fitur\|Drug)** | Tabel likelihood untuk setiap fitur |
| 7 | **Prediksi 40 Data Test** | Naive Bayes manual + evaluasi |
| 8 | **Evaluasi Akurasi** | Akurasi, confusion matrix, metric per kelas |
| 9 | **Prediksi Pasien Baru** | Input manual + visualisasi posterior |

### main_v3.ipynb (dengan class weight)
Sama seperti v2, ditambah:
| No. | Bagian |
|---|---|
| 10 | **Class Weight** — perhitungan bobot balanced + prediksi ulang |
| 11 | **Prediksi Pasien Baru** — dengan dan tanpa class weight |

---

## Perbandingan Hasil

### Excel (`drug200_naive_bayes.xlsx`)

| Metrik | Tanpa CW | Dengan CW |
|---|---|---|
| Akurasi | **70.0%** (28/40) | **67.5%** (27/40) |
| Recall drugB | 100% (3/3) | 100% (3/3) |
| Recall drugC | 33.3% (1/3) | 100% (3/3) |
| Recall DrugY | 77.8% (14/18) | 44.4% (8/18) |

Class weight di Excel **menurunkan** akurasi (-2.5%) karena drugB sudah 100% recall tanpa CW, tetapi CW mengorbankan banyak prediksi DrugY.

### Python Notebook (main_v2 / main_v3)

| Metrik | Tanpa CW (v2) | Dengan CW (v3) |
|---|---|---|
| Akurasi | **67.5%** (27/40) | **80.0%** (32/40) |
| Recall drugB | 0% (0/3) | 66.7% (2/3) |
| Recall drugC | 33.3% (1/3) | 100% (3/3) |
| Recall DrugY | 83.3% (15/18) | 72.2% (13/18) |

Class weight di notebook **meningkatkan** akurasi (+12.5%) karena drugB awalnya 0% — CW memperbaiki prediksi kelas minoritas secara signifikan.

### Mengapa Hasil Berbeda?

Meskipun sama-sama menggunakan seed 42, **split data train/test berbeda** karena random generator:
- **Excel** menggunakan random VBA internal
- **Python** menggunakan `np.random.permutation`

Akibatnya, distribusi kelas di train/test tidak identik, yang mengubah prior, likelihood, dan pada akhirnya efek class weight.

---

## Kesimpulan Akhir

1. **Naive Bayes tanpa library ML** berhasil diimplementasikan dan menghasilkan akurasi 67.5%-70% hanya dari 4 fitur kategorik.
2. **Class weight bersifat sensitif terhadap split data** — bisa sangat membantu (notebook: +12.5%) atau justru merugikan (Excel: -2.5%) tergantung apakah kelas minoritas banyak salah prediksi pada split tersebut.
3. **Fitur BP dan Na_to_K** adalah yang paling diskriminatif, dengan likelihood 0.0 atau 1.0 untuk beberapa kelas.
4. **Split stratified** penting untuk menjaga proporsi kelas, tetapi hasil akhir tetap dipengaruhi oleh random generator yang digunakan.
5. Untuk laporan: sertakan **kedua hasil** (Excel dan Python) sebagai validasi silang bahwa implementasi sudah benar, dan jelaskan perbedaan split sebagai penyebab variasi angka.

---

## Cara Menjalankan

### 1. Buat virtual environment & install dependency

```bash
pip install openpyxl pandas numpy seaborn matplotlib jupyter
```

### 2. Jalankan notebook

```bash
jupyter notebook main_v2.ipynb   # tanpa class weight
jupyter notebook main_v3.ipynb   # dengan class weight
```

atau jalankan seluruh sel sekaligus:

```bash
jupyter nbconvert --to notebook --execute --inplace main_v2.ipynb
jupyter nbconvert --to notebook --execute --inplace main_v3.ipynb
```

### 3. Jalankan sel dari atas ke bawah (Run All)

Semua sel sudah diuji berjalan berurutan tanpa error.

---

## Mencoba Kasus Pasien Baru

Ubah dictionary `pasien_baru` di bagian prediksi pasien baru:

```python
pasien_baru = {
    "Age": "OLD",           # YOUNG / ADULT / OLD
    "BP": "HIGH",           # HIGH / LOW / NORMAL
    "Cholesterol": "NORMAL",  # HIGH / NORMAL
    "Na_to_K": "HIGH",      # HIGH / LOW / NORMAL
}
```

---

## Catatan untuk Laporan

- **Tidak ada scikit-learn** — semua rumus Prior, Likelihood, dan Posterior ditulis manual.
- Library `pandas` & `numpy` digunakan hanya untuk **membaca data, pengolahan tabel, dan menyusun array** — bukan sebagai engine machine learning.
- Library `seaborn` & `matplotlib` digunakan untuk **visualisasi** (heatmap likelihood, bar chart posterior, confusion matrix).
- File `drug200_naive_bayes.xlsx` berisi simulasi lengkap Excel sebagai validasi.
- Lampirkan **screenshot setiap sel beserta output-nya** pada laporan Word/PPT.

---

## Requirements

| Library | Fungsi |
|---|---|
| `openpyxl` | membaca file `.xlsx` |
| `pandas` | pengolahan data & pembentukan tabel |
| `numpy` | operasi numerik array |
| `seaborn` | heatmap & bar chart |
| `matplotlib` | rendering grafik |
