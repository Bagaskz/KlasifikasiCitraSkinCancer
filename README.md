# Klasifikasi Pneumonia dari Citra X-Ray dengan Metode Random Forest

Proyek ini menggunakan algoritma **Random Forest** untuk mengklasifikasikan citra X-Ray dada ke dalam dua kelas: **Normal** dan **Pneumonia**, berdasarkan ekstraksi fitur tekstur dan statistik intensitas.

---

## Dataset

- **Nama Dataset:** Chest X-Ray Images (Pneumonia)
- **Jumlah Kelas:** 2 (Normal, Pneumonia)
- **Total Gambar:** 232 (116 per kelas setelah pembersihan)
- **Sumber Data:** Google Drive (Google Colab)
- **Format Gambar:** `.jpeg`, `.jpg`, `.png`
- **Struktur Folder:**  
  `Pneumonia/train/normal/` dan `Pneumonia/train/unnormal/`

**Link Dataset:**  
[Pneumonia Dataset](https://drive.google.com/drive/folders/1CZPcJ9KhIby2ibXNwAsfXFE5evBUwQLY?usp=sharing)

---

## Metode yang Digunakan

- **Ekstraksi Fitur (21 fitur per gambar):**
  - **Statistik Intensitas (4 fitur):**  
    Mean, Standar Deviasi, Skewness, Kurtosis
  - **GLCM (Gray-Level Co-occurrence Matrix) (5 fitur):**  
    Contrast, Dissimilarity, Homogeneity, Energy, Correlation  
    (jarak = 1 piksel, sudut 0°)
  - **LBP (Local Binary Pattern) (10 fitur):**  
    Histogram 10 bin dari pola uniform (P=8, R=1)
  - **HOG (Histogram of Oriented Gradients) (2 fitur):**  
    Mean dan standar deviasi dari deskriptor HOG  
    (orientations=9, pixels_per_cell=(32,32), cells_per_block=(1,1))

- **Preprocessing:**
  - Konversi ke grayscale
  - Resize ke 256×256 piksel
  - Normalisasi fitur dengan `StandardScaler` (mean=0, std=1) hanya pada data latih

- **Algoritma:**  
  **Random Forest Classifier** dengan 100 pohon keputusan (`n_estimators=100`, `random_state=42`, `n_jobs=-1`)

- **Evaluasi:**  
  Accuracy, Precision, Recall, F1-Score, Confusion Matrix, Feature Importance

---

## Pembagian Data

| Split      | Proporsi | Jumlah Gambar |
|------------|----------|----------------|
| Training   | 80%      | 185            |
| Testing    | 20%      | 47             |

Pembagian dilakukan secara **stratified** (berdasarkan label) untuk menjaga proporsi kelas tetap seimbang.

---

## Hasil

- **Test Accuracy:** **97.87%**
- **Classification Report:**

| Class       | Precision | Recall | F1-Score | Support |
|-------------|-----------|--------|----------|---------|
| Normal      | 0.96      | 1.00   | 0.98     | 24      |
| Pneumonia   | 1.00      | 0.96   | 0.98     | 23      |

- **Top 5 Fitur Terpenting (berdasarkan Gini Importance):**
  1. GLCM Dissimilarity (F7)
  2. GLCM Contrast (F6)
  3. GLCM Homogeneity (F5)
  4. HOG Mean (F10)
  5. GLCM Correlation (F9)

- **Confusion Matrix:**
  - True Normal: 24, False Pneumonia: 0
  - False Normal: 1, True Pneumonia: 22

Model menunjukkan kinerja yang sangat baik dalam membedakan kedua kelas, dengan tingkat kesalahan minimal (hanya 1 dari 47 data uji salah prediksi).

---

## Catatan Tambahan

- Fitur GLCM (tekstur) terbukti paling dominan dalam klasifikasi.
- Dataset yang seimbang (116:116) membantu model menghindari bias kelas.
- Hasil ekstraksi fitur disimpan dalam file CSV (`pneumonia_features.csv`) untuk digunakan kembali tanpa perlu ekstraksi ulang.