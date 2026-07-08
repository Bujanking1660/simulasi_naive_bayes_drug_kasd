# Naive Bayes Manual — Prediksi Obat (Dataset Drug200)

Notebook ini membangun model **Naive Bayes untuk data kategorik dari nol**
(Prior & Likelihood dihitung manual, **tanpa scikit-learn**), dibantu **pandas**
& **numpy** untuk pengolahan data, dan **seaborn** / **matplotlib** untuk visualisasi.

---

## 📁 Struktur Folder

```
.
├── README.md
├── main.ipynb          ← notebook utama
└── data/
    └── drug200(NAIVE BAYES).xlsx
```

> Notebook membaca dataset dari `./data/drug200(NAIVE BAYES).xlsx`.
> Pastikan folder `data/` berada satu level dengan `main.ipynb`.

---

## 📊 Dataset

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

## 🧠 Daftar Isi Notebook

| No. | Bagian | Keterangan |
|---|---|---|
| 1 | **Setup Library** | Import pandas, numpy, matplotlib, seaborn |
| 2 | **Load Data** | Baca file Excel, tampilkan 5 data teratas & info jumlah data |
| 3 | **Sebaran Data** | Visualisasi distribusi tiap fitur terhadap jenis obat |
| 4 | **Prior P(Drug)** | Hitung probabilitas prior tiap kelas tanpa fitur |
| 5 | **Likelihood Age** | Prior × P(Age \| Drug) — satu fitur saja |
| 6 | **Likelihood BP** | Prior × P(BP \| Drug) — satu fitur saja |
| 7 | **Likelihood Cholesterol** | Prior × P(Cholesterol \| Drug) — satu fitur saja |
| 8 | **Likelihood Na_to_K** | Prior × P(Na_to_K \| Drug) — satu fitur saja |
| 9 | **Input Data Uji** | Isi `pasien_baru` sesuai kasus yang ingin diuji |
| 10 | **Hasil Prediksi** | Posterior gabungan semua fitur → rekomendasi obat |
| 11 | **Kesimpulan** | Perbandingan kekuatan tiap fitur, fitur paling berpengaruh |

### Fungsi Utama

| Fungsi | Keterangan |
|---|---|
| `hitung_likelihood_fitur(df, kolom_fitur, kolom_label)` | Menghitung tabel likelihood P(fitur \| kelas) |
| `uji_satu_fitur(df, kolom_fitur, kolom_label, prior, likelihood_fitur)` | Akurasi model jika hanya memakai satu fitur |
| `prediksi(pasien, prior, tabel_likelihood)` | Prediksi kelas dari data pasien baru |

---

## ⚙️ Cara Menjalankan

### 1. Buat virtual environment & install dependency

```bash
pip install openpyxl pandas numpy seaborn matplotlib jupyter
```

### 2. Jalankan notebook

```bash
jupyter notebook main.ipynb
```

atau jalankan seluruh sel sekaligus:

```bash
jupyter nbconvert --to notebook --execute --inplace main.ipynb
```

### 3. Jalankan sel dari atas ke bawah (Run All)

Semua sel sudah diuji berjalan berurutan tanpa error. Hindari menjalankan sel secara acak karena beberapa sel bergantung pada variabel dari sel sebelumnya (`df`, `prior`, `likelihood_*`, dst).

---

## 🔬 Mencoba Kasus Pasien Baru

Ubah dictionary `pasien_baru` di **bagian 9** sesuai nilai fitur kasus yang ingin diuji:

```python
pasien_baru = {
    "Age": "OLD",           # pilihan: YOUNG / ADULT / OLD
    "BP": "HIGH",           # pilihan: HIGH / LOW / NORMAL
    "Cholesterol": "NORMAL",  # pilihan: HIGH / NORMAL
    "Na_to_K": "HIGH",      # pilihan: HIGH / LOW / NORMAL
}
```

Seluruh langkah perhitungan posterior dan visualisasi bar chart di bagian 10 akan otomatis mengikuti nilai yang diubah.

---

## 📝 Catatan untuk Laporan

- **Tidak ada scikit-learn** — semua rumus Prior, Likelihood, dan Posterior ditulis manual menggunakan `dict` / `for-loop` / operasi pandas.
- Library `pandas` & `numpy` digunakan hanya untuk **membaca data, pengolahan tabel, dan menyusun array** — bukan sebagai engine machine learning.
- Library `seaborn` & `matplotlib` digunakan untuk **visualisasi** (heatmap likelihood, bar chart posterior, bar chart perbandingan fitur).
- Lampirkan **screenshot setiap sel beserta output-nya** (terutama bagian 4–11) pada laporan Word/PPT.
- Bagian referensi / daftar pustaka tersedia di folder `data/Referensi_Naive_Bayes/`.

---

## 🔧 Requirements

| Library | Fungsi |
|---|---|
| `openpyxl` | membaca file `.xlsx` |
| `pandas` | pengolahan data & pembentukan tabel |
| `numpy` | operasi numerik array |
| `seaborn` | heatmap & bar chart |
| `matplotlib` | rendering grafik |
| `random` *(bawaan Python)* | tidak dipakai di versi ini |
| `collections` *(bawaan Python)* | tidak dipakai di versi ini |
