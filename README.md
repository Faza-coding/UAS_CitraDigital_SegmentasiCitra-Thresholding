## Segmentasi Citra Digital Menggunakan Thresholding

**Anggota Kelompok:**

* Faza Alega Yahya
* Ferdi Zandio
* Syandhika Ahmad Farroja
* Taufiq Hidayatur R
* Vicky Irmansyah

---

## 1. Deskripsi Proyek

Project ini bertujuan untuk mengimplementasikan **segmentasi citra digital menggunakan metode thresholding**.
Segmentasi dilakukan untuk **memisahkan objek (foreground) dari latar belakang (background)** dengan mengubah citra grayscale menjadi citra biner (hitam–putih).

Metode ini banyak digunakan pada:

* OCR (pengenalan teks)
* Citra medis (segmentasi organ/tumor)
* Inspeksi kualitas produk industri

---

## 2. Konsep Dasar Thresholding

Thresholding membandingkan setiap piksel citra grayscale terhadap nilai ambang batas **T**.

Jika:

* f(x, y) > T → g(x, y) = 255 (objek)
* f(x, y) ≤ T → g(x, y) = 0 (background)

Tujuan utamanya adalah **menyederhanakan citra agar objek mudah dianalisis**.

---

## 3. Jenis Thresholding yang Digunakan

### Global Thresholding

Menggunakan satu nilai T untuk seluruh citra. Cepat, tetapi sensitif terhadap perubahan pencahayaan.

### Otsu’s Thresholding

Menentukan nilai threshold secara otomatis berdasarkan histogram citra dengan meminimalkan varian dalam kelas.

### Adaptive Thresholding

Menentukan threshold untuk setiap area kecil citra sehingga mampu mengatasi pencahayaan tidak merata.
Terdiri dari Adaptive Mean dan Adaptive Gaussian.

---

## 4. Alur Program

1. Membaca citra
2. Konversi ke grayscale
3. Reduksi noise
4. Global thresholding
5. Otsu thresholding
6. Adaptive thresholding
7. Morphological cleaning
8. Visualisasi hasil

---

## 5. Penjelasan Kode

### 5.1 Import Library

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

cv2 untuk pengolahan citra, numpy untuk operasi numerik, dan matplotlib untuk menampilkan hasil.

---

### 5.2 Membaca dan Preprocessing Citra

```python
image = cv2.imread('IHI.jpeg')
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
blurred = cv2.GaussianBlur(gray, (5,5), 0)
```

Citra diubah ke grayscale agar thresholding dapat diterapkan.
Gaussian Blur mengurangi noise agar hasil segmentasi lebih halus.

---

### 5.3 Global Thresholding

```python
T = 127
_, thresh_global = cv2.threshold(gray, T, 255, cv2.THRESH_BINARY)
```

Semua piksel dibandingkan dengan nilai T = 127.
Jika lebih besar → putih, jika lebih kecil → hitam.

---

### 5.4 Otsu’s Thresholding

```python
T_otsu, thresh_otsu = cv2.threshold(gray, 0, 255,
                                   cv2.THRESH_BINARY + cv2.THRESH_OTSU)
```

OpenCV menghitung nilai threshold optimal secara otomatis berdasarkan histogram citra.

---

### 5.5 Adaptive Thresholding

```python
thresh_mean = cv2.adaptiveThreshold(gray, 255,
                                    cv2.ADAPTIVE_THRESH_MEAN_C,
                                    cv2.THRESH_BINARY, 11, 2)
```

Threshold ditentukan dari rata-rata piksel lokal.

```python
thresh_gauss = cv2.adaptiveThreshold(gray, 255,
                                     cv2.ADAPTIVE_THRESH_GAUSSIAN_C,
                                     cv2.THRESH_BINARY, 11, 2)
```

Threshold ditentukan dari rata-rata berbobot Gaussian sehingga lebih halus.

---

### 5.6 Morphological Cleaning

```python
kernel = np.ones((3,3), np.uint8)
cleaned = cv2.morphologyEx(thresh_gauss, cv2.MORPH_CLOSE, kernel)
```

Operasi closing digunakan untuk menghilangkan noise kecil dan menutup lubang pada objek hasil segmentasi.

---

### 5.7 Visualisasi

```python
fig, axs = plt.subplots(2, 3, figsize=(15,10))
```

Semua hasil ditampilkan dalam satu halaman agar mudah membandingkan performa setiap metode thresholding.

---

## 6. Kesimpulan

Thresholding adalah metode segmentasi citra yang sederhana, cepat, dan efektif untuk memisahkan objek dari background.
Untuk citra dengan pencahayaan merata, metode Global dan Otsu sudah cukup baik.
Namun, pada citra dunia nyata dengan pencahayaan tidak merata, **Adaptive Thresholding** memberikan hasil yang paling akurat dan stabil.
