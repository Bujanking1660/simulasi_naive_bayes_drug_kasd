# Tugas Besar — Naive Bayes (Data Kategorikal) pada Dataset `drug200`

Notebook ini adalah implementasi tugas besar mata kuliah Kecerdasan Buatan dengan algoritma
**Naive Bayes untuk data kategorikal**, dibangun **dari nol (from scratch)** tanpa menggunakan
library machine learning (`sklearn`, dsb).

## 📁 Struktur Folder

```
.
├── README.md
├── Tugas_Besar_Naive_Bayes_drug200.ipynb   # notebook utama
├── drug200_NAIVE_BAYES_.xlsx               # dataset asli
└── data/
    └── drug200(NAIVE BAYES).xlsx           # salinan dataset (path yang dipakai notebook)
```

> Notebook membaca dataset dari `./data/drug200(NAIVE BAYES).xlsx`. Jika kamu memindahkan notebook,
> pastikan folder `data/` ikut dipindahkan pada lokasi yang sama, atau ubah path pada sel pembacaan
> file di awal notebook.

## 📊 Dataset

`drug200` berisi 200 data pasien dengan 4 fitur **kategorikal** dan 1 label kelas:

| Kolom | Tipe | Nilai |
|---|---|---|
| `Age` | kategorikal | YOUNG, ADULT, OLD |
| `BP` | kategorikal | HIGH, NORMAL, LOW |
| `Cholesterol` | kategorikal | HIGH, NORMAL |
| `Na_to_K` | kategorikal | HIGH, NORMAL, LOW |
| `ClassDrug` (target) | kategorikal | DrugY, drugX, drugA, drugB, drugC |

## 🧠 Isi Notebook

1. **Pendahuluan** — teori Bayes, asumsi *naive*, rumus Naive Bayes kategorikal + Laplace smoothing,
   langkah algoritma secara umum (tanpa kasus).
2. **Dataset & Visualisasi (EDA)** — pembacaan data + grafik seaborn (distribusi kelas, sebaran fitur
   per kelas) agar pola data mudah dipahami sebelum masuk ke perhitungan.
3. **Simulasi Kasus (manual)** — perhitungan langkah-demi-langkah menggunakan **seluruh 200 data & 4
   fitur**: prior → tabel kontingensi → likelihood (Laplace smoothing) → skor posterior → normalisasi →
   keputusan kelas, lengkap dengan visualisasi bar chart prior & posterior.
4. **Implementasi (kode program)** — fungsi Python murni: `naive_bayes_train`, `naive_bayes_predict`,
   `evaluasi_akurasi`, `confusion_matrix_manual`, ditambah split data latih/uji manual.
5. **Evaluasi** — akurasi model pada data uji + confusion matrix dalam bentuk heatmap seaborn.
6. **Daftar Pustaka** — 10 referensi jurnal terkait Naive Bayes.

## ⚙️ Cara Menjalankan

### 1. Buat environment & install dependency
```bash
pip install openpyxl pandas numpy seaborn matplotlib jupyter
```

### 2. Jalankan notebook
```bash
jupyter notebook Tugas_Besar_Naive_Bayes_drug200.ipynb
```
atau jalankan seluruh sel sekaligus dari terminal:
```bash
jupyter nbconvert --to notebook --execute --inplace Tugas_Besar_Naive_Bayes_drug200.ipynb
```

### 3. Jalankan sel dari atas ke bawah (Run All)
Semua sel sudah diuji berjalan berurutan tanpa error. Hindari menjalankan sel secara acak karena
beberapa sel bergantung pada variabel dari sel sebelumnya (`data`, `model`, `data_train`, dst).

## 🧩 Menyesuaikan dengan Kasus Kelompok

Bagian **Simulasi Kasus** memakai satu baris data asli sebagai contoh query karena tidak ada file
kasus terpisah yang disertakan. Jika dosen/kelompokmu memberi kasus khusus melalui link Google Drive,
ubah dictionary `query` pada bagian Simulasi Kasus (Langkah 3) sesuai nilai fitur kasus tersebut —
seluruh langkah perhitungan setelahnya akan otomatis mengikuti.

## 📝 Catatan untuk Laporan (Word/PPT)

- Sesuai instruksi tugas, **screenshot setiap sel kode beserta output-nya** pada bagian Implementasi
  untuk dilampirkan di laporan Word.
- Library yang dipakai (`openpyxl`, `random`, `pandas`, `seaborn`, `matplotlib`, `numpy`) hanya untuk
  membaca file, split data, dan **visualisasi** — bukan untuk menghitung/menjalankan algoritma Naive
  Bayes itu sendiri (perhitungan probabilitas ditulis manual dengan `dict`/`for-loop`).
- Bagian Daftar Pustaka berisi 10 jurnal (bukan buku/tautan web) sesuai ketentuan.

## 🔧 Requirements

| Library | Fungsi |
|---|---|
| `openpyxl` | membaca file `.xlsx` |
| `random` | split data latih/uji (bawaan Python) |
| `collections.OrderedDict` | struktur data penghitung (bawaan Python) |
| `pandas` | hanya untuk membentuk tabel saat visualisasi EDA |
| `seaborn`, `matplotlib` | visualisasi grafik |
| `numpy` | menyusun array confusion matrix untuk heatmap |
