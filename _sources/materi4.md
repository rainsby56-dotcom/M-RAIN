# Face Recognition Eigenface + SVD
**Google Colab | Penjelasan Lengkap Kode & Output**

> 🔗 **Link Notebook Colab:** [Buka di Google Colab](https://colab.research.google.com/drive/1KRwEGOYUpLwzKxJlV1UcU4HWrxiEc63V?usp=sharing)

---

## 📁 Struktur Dataset

Dataset yang digunakan berbentuk file ZIP dengan struktur folder sebagai berikut:

```
dataset.zip
└── dataset/
    ├── bj habibie/
    │   ├── 1.jpg
    │   ├── 2.jpg
    │   └── 3.jpg
    └── bungtomo/
        ├── 1.jpg
        ├── 2.jpg
        └── 3.jpg
```

Terdapat **2 orang (kelas)** yaitu `bj habibie` dan `bungtomo`, masing-masing dengan **3 foto wajah** sebagai data training. Total = **6 gambar**.

---

## ⚙️ Install & Import Library

```python
!pip install opencv-python matplotlib scikit-learn -q
```

```python
import os
import cv2
import zipfile
import numpy as np
import matplotlib.pyplot as plt
from google.colab import files
```

| Library | Fungsi |
|---|---|
| `cv2` | Membaca dan memproses gambar (grayscale, resize) |
| `numpy` | Operasi matriks, aritmatika array, dan SVD |
| `matplotlib` | Visualisasi eigenface dan plot gambar |
| `zipfile` | Ekstrak dataset dari file ZIP |
| `os` | Navigasi direktori dan listing file |

---

## 📊 Tahap 1 — Load & Preprocessing Dataset

```python
# Pseudocode alur load dataset
for setiap_folder_orang in dataset/:
    for setiap_gambar in folder:
        img = cv2.imread(gambar, grayscale)
        img = cv2.resize(img, (100, 100))     # seragamkan ukuran
        vektor = img.flatten()                # 100x100 → 10000 elemen
        data_matrix.append(vektor)
        label.append(nama_orang)
```

Setiap gambar melewati 3 langkah preprocessing:

1. **Grayscale** — warna dihilangkan, hanya intensitas cahaya (0–255) yang dipakai. Ini menyederhanakan data dan mengurangi pengaruh warna kulit/pencahayaan.
2. **Resize 100×100** — semua gambar diseragamkan ukurannya agar bisa dibandingkan.
3. **Flatten** — matriks 2D (100×100) diubah menjadi vektor 1D sepanjang **10.000 elemen**. Setiap piksel menjadi satu fitur.

**Output:**
```
Jumlah data training : 6
Ukuran matrix data   : (6, 10000)
```

Hasilnya adalah **Matrix Data** berukuran **6 × 10.000** — 6 baris (gambar) dan 10.000 kolom (piksel).

```
Matrix Data =
┌─ gambar 1 (bj habibie_1)   → [p1, p2, p3, ..., p10000] ─┐
│  gambar 2 (bj habibie_2)   → [p1, p2, p3, ..., p10000]   │
│  gambar 3 (bj habibie_3)   → [p1, p2, p3, ..., p10000]   │
│  gambar 4 (bungtomo_1) → [p1, p2, p3, ..., p10000]   │
│  gambar 5 (bungtomo_2) → [p1, p2, p3, ..., p10000]   │
└─ gambar 6 (bungtomo_3) → [p1, p2, p3, ..., p10000] ─┘
```

---

## 📊 Tahap 2 — Hitung Mean Face (Wajah Rata-rata)

```python
mean_face = np.mean(data_matrix, axis=0)
```

**`axis=0`** artinya rata-rata dihitung **per kolom** (per piksel), bukan per baris. Jadi untuk setiap dari 10.000 piksel, diambil rata-rata nilainya dari ke-6 gambar.

**Secara matematis:**

```
mean_face[j] = (1/n) × Σ data_matrix[i][j]
             = rata-rata piksel ke-j dari semua n gambar
```

**Output (sebagian):**
```
[121.5   124.166664   119.166664 ... 109.5   119.333336   126.166664]
```

Contoh: piksel pertama bernilai 121.5, artinya rata-rata kecerahan piksel pojok kiri atas dari semua 6 gambar adalah 121.5.

> 💡 **Mengapa perlu Mean Face?** Karena SVD/PCA bekerja pada **variasi data**, bukan nilai absolutnya. Dengan mengurangi mean, kita memindahkan pusat data ke titik nol sehingga SVD bisa menangkap pola perbedaan antar wajah secara akurat.

---

## 📊 Tahap 3 — Hitung Matrix A (Data Terpusat / Centered)

```python
A = data_matrix - mean_face
```

Setiap gambar dikurangi mean face:

```
A[i] = data_matrix[i] - mean_face

Contoh baris pertama (amba_1):
data_matrix[0][0] = 152.0   →   A[0][0] = 152.0 - 121.5 = 30.5
data_matrix[0][1] = 154.0   →   A[0][1] = 154.0 - 124.17 = 29.83
...
```

**Output Matrix A (6 × 10.000):**
```
[[  30.5      29.833336   34.833336 ...  -46.5    -54.333336  -37.166664]
 [ 131.5     128.83334   133.83334  ...  -13.5    -27.333336  -30.166664]
 [ -59.5     -62.166664  -58.166664 ...   73.5     88.666664   88.833336]
 [  20.5      15.833336   20.833336 ... -102.5    -94.333336  -96.166664]
 [ -52.5     -51.166664  -40.166664 ...   53.5     60.666664   54.833336]
 [ -70.5     -61.166664  -91.166664 ...   35.5     26.666664   19.833336]]
```

Nilai **positif** berarti piksel itu lebih terang dari rata-rata, nilai **negatif** berarti lebih gelap. Ini merepresentasikan "seberapa beda" tiap wajah dari wajah rata-rata.

---

## 🔢 Tahap 4 — SVD (Singular Value Decomposition)

```python
U, S, VT = np.linalg.svd(A, full_matrices=False)
```

### Apa itu SVD?

SVD adalah teknik faktorisasi matriks yang memecah sebuah matriks **A** menjadi tiga matriks:

```
A  =  U  ×  Σ  ×  Vᵀ
```

Analoginya seperti memfaktorkan angka: `12 = 4 × 3`, tapi untuk matriks.

### Bagaimana SVD Dihitung?

Proses SVD secara konseptual dilakukan melalui dua langkah utama:

**Langkah 1 — Hitung AᵀA dan AAᵀ**

```
AᵀA  →  matriks berukuran (10000 × 10000)
AAᵀ  →  matriks berukuran (6 × 6)   ← ini yang lebih efisien dihitung
```

Karena A berukuran 6×10000, numpy otomatis memilih strategi yang efisien. Parameter `full_matrices=False` memastikan ukuran output diperkecil (ekonomis/thin SVD).

**Langkah 2 — Cari Eigenvalue & Eigenvector**

- **Eigenvalue** dari AAᵀ → akar kuadratnya = **nilai singular (S)**
- **Eigenvector** dari AAᵀ → kolom-kolom **U**
- **Eigenvector** dari AᵀA → baris-baris **Vᵀ**

```
eigenvalue(AAᵀ) = λ₁, λ₂, ..., λ₆
nilai singular  = σᵢ = √λᵢ
```

### Penjelasan Setiap Output Matrix:

### 🔵 Matrix U (6 × 6)

```
Ukuran U : (6, 6)

[[-0.13280219   0.00657479   0.17732333   0.39236313  -0.79389     0.40824828]
 [-0.8464758    0.12745075  -0.01840555  -0.18440233   0.25734302  0.40824828]
 [ 0.13314712  -0.7004411   -0.5435097   -0.17035691  -0.02373067  0.40824828]
 [ 0.20740968  -0.01993572   0.14427534   0.68600833   0.54634637  0.4082483 ]
 [ 0.3429516    0.6936716   -0.43801886  -0.2027295   -0.03971816  0.40824828]
 [ 0.29576957  -0.10732028   0.6783354   -0.5208827    0.05364941  0.4082483 ]]
```

**U adalah matriks ortonormal** berukuran 6×6. Setiap baris mewakili satu gambar training, dan setiap kolom adalah satu **komponen/dimensi** di ruang eigenface.

- Setiap **baris** = koordinat satu wajah di ruang eigenface baru
- Setiap **kolom** = arah (basis vektor) dari satu komponen utama
- Nilai-nilainya adalah **bobot** seberapa besar kontribusi tiap eigenface terhadap tiap gambar
- Kolom terakhir semua bernilai ≈ 0.408 = 1/√6, ini karena komponen ke-6 punya sigma ≈ 0 (semua gambar "sama" di dimensi itu)

> 💡 U bersifat **ortonormal**: setiap kolom saling tegak lurus (dot product = 0) dan panjangnya = 1. Ini menjamin transformasi tidak mendistorsi jarak antar wajah.

---

### 🟡 Matrix Sigma / Σ (6 × 6)

```
Ukuran SIGMA : (6, 6)

[[1.2002574e+04  0             0             0             0             0          ]
 [0              8.2377520e+03 0             0             0             0          ]
 [0              0             5.9307148e+03 0             0             0          ]
 [0              0             0             4.5860278e+03 0             0          ]
 [0              0             0             0             4.2169961e+03 0          ]
 [0              0             0             0             0             8.1560726e-04]]
```

**Sigma adalah matriks diagonal** — hanya diagonal utamanya yang berisi nilai, sisanya nol.

Nilai-nilai di diagonal disebut **Nilai Singular** dan selalu diurutkan **dari terbesar ke terkecil**:

```
σ₁ = 12002.574  →  "energi" komponen 1 = σ₁² = 144,061,788
σ₂ =  8237.752  →  "energi" komponen 2 = σ₂² =  67,860,668
σ₃ =  5930.714  →  "energi" komponen 3 = σ₃² =  35,173,364
σ₄ =  4586.028  →  "energi" komponen 4 = σ₄² =  21,031,653
σ₅ =  4216.997  →  "energi" komponen 5 = σ₅² =  17,783,044
σ₆ =     0.001  →  "energi" komponen 6 ≈ 0    (noise)
```

**Cara membacanya:**
- σ₁ = 12002 artinya komponen pertama menyimpan variasi terbesar antar wajah
- σ₆ ≈ 0.0008 artinya komponen ke-6 hampir tidak menyimpan informasi apapun
- Persentase informasi komponen 1: `σ₁² / Σσᵢ²` ≈ **49.6%** dari total variasi

> 💡 **Mengapa Sigma ke-6 ≈ 0?** Karena kita punya 6 gambar dan 10.000 dimensi. Secara matematis, rank matriks A maksimal = min(6, 10000) = 6. Tapi karena 6 gambar dari 2 orang yang relatif mirip strukturnya, komponen ke-6 hampir linier dengan yang lain, sehingga nilainya mendekati nol.

---

### 🟢 Matrix VT (6 × 10.000)

```
Ukuran VT : (6, 10000)

[[-1.3154604e-02  -1.2801295e-02  -1.3503426e-02 ...   2.9141479e-03   4.2728814e-03   3.9178636e-03]
 [ 3.5660444e-03   3.7529722e-03   4.7992137e-03 ...  -2.2049418e-03  -3.0160157e-03  -3.4580657e-03]
 [ 2.2691775e-03   3.3574183e-03  -9.9725812e-04 ...  -1.0468611e-02  -1.3390772e-02  -1.3279330e-02]
 ...
```

Setiap **baris** dari VT adalah satu **Eigenface** — vektor sepanjang 10.000 elemen yang bisa dibentuk ulang menjadi gambar 100×100.

- Baris 1 VT = Eigenface 1 (pola yang paling banyak menjelaskan variasi)
- Baris 2 VT = Eigenface 2 (pola kedua terpenting)
- ...dst

---

## 🖼️ Tahap 5 — Eigenface (Visualisasi)

**Jumlah Eigenface: 6** (sama dengan jumlah data training karena rank matrix = 6)

Setiap baris VT di-*reshape* kembali ke 100×100 piksel dan divisualisasikan:

```python
for i in range(num_eigenfaces):
    eigenface_img = VT[i].reshape(100, 100)
    plt.imshow(eigenface_img, cmap='gray')
```

```
Eigenface 1 → menangkap ~49.6% variasi  (kontras gelap-terang wajah)
Eigenface 2 → menangkap pola berikutnya (bayangan samping, dll)
Eigenface 3 → pola lebih halus
Eigenface 4 → pola lebih halus lagi
Eigenface 5 → pola minor
Eigenface 6 → NOISE (sigma ≈ 0, gambar tampak seperti static TV)
```

> 🔍 Eigenface 1–5 terlihat seperti "wajah hantu" — samar tapi ada struktur wajah. Eigenface 6 tampak acak/noise karena komponen ini tidak membawa informasi bermakna.

---

## 📐 Tahap 6 — Proyeksi Training ke Eigenspace

```python
projected_training = U @ np.diag(S)
```

### Mengapa U × Σ dan bukan sesuatu yang lain?

Ingat bahwa: `A = U × Σ × Vᵀ`

Jika kita ingin mengekspresikan setiap wajah dalam **koordinat eigenface**, caranya adalah memproyeksikan A ke basis Vᵀ:

```
A × V  =  U × Σ × Vᵀ × V
        =  U × Σ × I       (karena VᵀV = I, ortonormal)
        =  U × Σ
```

Jadi `projected_training = U @ np.diag(S)` adalah cara singkat mendapatkan koordinat setiap wajah di ruang eigenface **tanpa perlu mengalikan A dengan VT secara eksplisit**.

**Output:**
```
Ukuran projected training : (6, 6)

[[-1593.97    54.16   1051.65   1799.39  -3347.83   3.15e-04]
 [-10159.89  1049.90  -1091.58   -845.67   1085.21   3.82e-04]
 [ 1598.11  -5770.60  -3223.40   -781.26  -100.07   3.26e-04]
 [ 2489.45   -164.23    855.56   3146.05   2303.94   3.49e-04]
 [ 4116.30   5714.29  -2597.76   -929.72   -167.49   3.18e-04]
 [ 3549.99   -884.08   4023.01  -2388.78    226.24   3.28e-04]]
```

Dari **10.000 dimensi** (piksel) → dikompres menjadi **6 dimensi** (koordinat eigenface). Inilah representasi fitur yang akan digunakan untuk pengenalan wajah.

Setiap baris = satu wajah training, dinyatakan sebagai 6 angka yang menjadi "sidik jarinya" di ruang eigenface.

> 💡 Kolom ke-6 semua bernilai sangat kecil (~3e-04) karena σ₆ ≈ 0, artinya dimensi ke-6 tidak membedakan wajah satu sama lain dan aman untuk diabaikan.

---

## 🧪 Tahap 7 — Uji Gambar Baru (Recognition)

```
Upload gambar wajah test: bungtomo (1).jpg
```

### Alur pengenalan gambar baru:

```python
# 1. Preprocessing gambar baru (sama seperti training)
test_img = cv2.imread('bungtomo (1).jpg', cv2.IMREAD_GRAYSCALE)
test_img = cv2.resize(test_img, (100, 100))
test_vec = test_img.flatten().astype(float)        # shape: (10000,)

# 2. Kurangi mean face
test_centered = test_vec - mean_face               # shape: (10000,)

# 3. Proyeksi ke eigenspace menggunakan VT
test_projected = test_centered @ VT.T              # shape: (6,)

# 4. Hitung jarak ke setiap wajah training
for i in range(len(projected_training)):
    jarak = np.linalg.norm(test_projected - projected_training[i])

# 5. Ambil label dari jarak terkecil (Nearest Neighbor)
hasil = label[argmin(jarak)]
```

### Mengapa menggunakan VT.T untuk proyeksi?

Karena VT (baris = eigenface) adalah basis ruang eigenface. Proyeksi vektor `x` ke basis ini:

```
koordinat = x · Vᵀᵀ = x · V
```

Ini setara dengan "seberapa mirip gambar baru dengan tiap eigenface?"

### Nearest Neighbor (1-NN):

Setelah proyeksi, gambar baru memiliki koordinat 6D. Kemudian dihitung **jarak Euclidean** ke semua 6 gambar training:

```
jarak = √Σ(test_projected[i] - training_projected[i])²
```

Gambar training dengan jarak terkecil = identitas yang dikenali.

---

## 📌 Ringkasan Alur Kerja Lengkap

```
┌────────────────────────────────────────────────────┐
│  TRAINING PHASE                                    │
│                                                    │
│  Gambar (6 foto)                                   │
│       ↓ grayscale + resize(100×100) + flatten      │
│  Matrix Data  →  shape: (6, 10000)                 │
│       ↓ np.mean(axis=0)                            │
│  Mean Face    →  shape: (10000,)                   │
│       ↓ data_matrix - mean_face                    │
│  Matrix A     →  shape: (6, 10000)                 │
│       ↓ np.linalg.svd(A, full_matrices=False)      │
│  U (6×6)  +  Σ (6×6)  +  VT (6×10000)             │
│       ↓                                            │
│  Eigenface = baris VT  →  visualisasi              │
│       ↓ U @ diag(S)                                │
│  Projected Training   →  shape: (6, 6)             │
└────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────┐
│  TESTING PHASE                                     │
│                                                    │
│  Gambar Baru                                       │
│       ↓ grayscale + resize + flatten               │
│  test_vec  →  shape: (10000,)                      │
│       ↓ - mean_face                                │
│  test_centered  →  shape: (10000,)                 │
│       ↓ @ VT.T                                     │
│  test_projected →  shape: (6,)                     │
│       ↓ Euclidean distance ke projected_training   │
│  Hasil = label dengan jarak terkecil               │
└────────────────────────────────────────────────────┘
```

---

## 🧮 Hubungan SVD dengan PCA (Eigenface Method)

Metode eigenface pada dasarnya adalah **PCA (Principal Component Analysis)** yang diimplementasikan lewat SVD. Hubungannya:

| Konsep PCA | Padanannya di SVD |
|---|---|
| Covariance matrix | AᵀA (atau AAᵀ untuk efisiensi) |
| Eigenvector dari covariance | Baris-baris VT |
| Eigenvalue | σᵢ² (kuadrat nilai singular) |
| Principal Components | Eigenface (baris VT) |
| Skor PCA | Projected training = U × Σ |

SVD lebih disukai daripada menghitung covariance matrix secara eksplisit karena:
- Lebih stabil secara numerik
- Lebih efisien (tidak perlu menghitung matriks 10000×10000)

---

## 📝 Catatan Penting & Keterbatasan

- **Jumlah eigenface = min(n_gambar, n_piksel)** → dengan 6 gambar hanya ada 6 eigenface (bukan 10.000).
- **Eigenface ke-6 adalah noise** karena σ₆ ≈ 0 — dalam praktik sebaiknya dibuang untuk menghindari overfitting.
- **Dataset sangat kecil (6 gambar)** → hasil recognition bisa tidak akurat untuk wajah yang kondisinya sangat berbeda dari training.
- **Metode ini sensitif** terhadap pencahayaan, pose, dan ekspresi wajah karena berbasis piksel mentah.
- Ini adalah implementasi klasik **Turk & Pentland (1991)** — metode modern (deep learning) jauh lebih robust tapi prinsip matematikanya tetap relevan untuk dipahami.