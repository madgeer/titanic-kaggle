# Titanic Survival Prediction

Repositori ini berisi proyek analisis data dan pemodelan prediktif untuk memprediksi probabilitas keselamatan (survival) penumpang kapal Titanic berdasarkan data sosio-demografis dan detail perjalanan mereka. Proyek ini menggunakan dataset dari kompetisi populer **Titanic: Machine Learning from Disaster** di Kaggle.

## Struktur Proyek
- [notebook.ipynb]: Jupyter Notebook yang berisi alur lengkap EDA (Exploratory Data Analysis), preprocessing data cerdas, visualisasi korelasi, pencarian parameter otomatis menggunakan **GridSearchCV**, serta model-model yang di-tuning dengan regularisasi untuk hasil generalisasi terbaik.
- [train.csv]: Dataset training yang digunakan untuk melatih model ML (berisi kolom target `Survived`).
- [test.csv]: Dataset testing untuk menguji performa model prediksi.
- [README.md]: Dokumentasi proyek ini.


## Alur Analisis & Pemodelan

Pada berkas [notebook.ipynb], tahapan preprocessing dan pemodelan dilakukan secara menyeluruh:

### 1. Data Preprocessing & Imputation
- **Age Imputation**: Alih-alih mengisi nilai kosong pada `Age` dengan median global, nilai diisi menggunakan nilai median dari masing-masing kelompok gelar (`Title`). Hal ini jauh lebih akurat karena gelar (seperti *Master* atau *Mr*) merepresentasikan kelompok umur penumpang secara spesifik.
- **Title Mapping**: Gelar kehormatan atau profesi langka (seperti *Lady, Countess, Capt, Col, Don, Dr, Major, Rev, Sir, Jonkheer, Dona*) disederhanakan dan dipetakan menjadi kelompok gelar `"Rare"` untuk mencegah overfitting pada kategori minor.
- **Handling Missing Values**: Kolom `Embarked` dan `Fare` diisi menggunakan modus dan median lokal.

### 2. Feature Engineering & Scaling
- **FamilySize & IsAlone**: Menggabungkan `SibSp` (saudara/pasangan) dan `Parch` (orang tua/anak) untuk mendeteksi penumpang solo atau dengan rombongan keluarga.
- **HasCabin**: Membuat fitur biner baru `HasCabin` (apakah penumpang memiliki nomor kabin atau tidak). Penumpang berkabin memiliki kecenderungan selamat yang lebih tinggi.
- **Log Transform Fare**: Mengaplikasikan logaritma skala (`np.log1p`) pada kolom biaya tiket (`Fare`) untuk menekan *skewness* ekstrem.
- **Standard Scaling**: Normalisasi skala numerik dilakukan khusus pada model yang sensitif jarak/gradien (Logistic Regression, KNN, dan SVM). Model berbasis pohon (Decision Tree, Random Forest, dan XGBoost) tetap dilatih menggunakan data asli (unscaled) agar pencabangannya optimal.

### 3. Hyperparameter Tuning Otomatis (GridSearchCV) & Regularisasi
Teknik pencarian kombinasi parameter optimal otomatis diterapkan langsung di dalam notebook menggunakan **GridSearchCV** dari scikit-learn:
- **Random Forest**: Mencari kombinasi terbaik dari `n_estimators`, `max_depth`, dan `min_samples_leaf`. Kombinasi terbaik berhasil memilih `{'max_depth': None, 'min_samples_leaf': 4, 'n_estimators': 50}`.
- **XGBoost**: Mencari kombinasi terbaik dari `n_estimators`, `max_depth`, `learning_rate`, dan `reg_lambda` (L2 regularization). Kombinasi terbaik berhasil memilih `{'learning_rate': 0.03, 'max_depth': 3, 'n_estimators': 150, 'reg_lambda': 1.0}`.
- **Decision Tree**: Dibatasi dengan parameter kedalaman pohon `max_depth=4` dan minimum sampel daun `min_samples_leaf=5`.
- **SVM**: Mengatur kekuatan penalti regularisasi melalui parameter `C=0.8`.


## Hasil Perbandingan Akurasi

Berikut adalah ringkasan perbandingan akurasi model yang dievaluasi pada data uji (20% data split) secara langsung dari output eksekusi notebook:

| Peringkat | Algoritma Klasifikasi | Akurasi (%) | Keterangan |
| :---: | :--- | :---: | :---: |
| 1 | **Logistic Regression** | **82.68%** | [Optimized via Grid Search] - Well-Fit |
| 1 | **Random Forest Classifier** | **82.68%** | [Optimized via Grid Search] - Well-Fit |
| 3 | K-Nearest Neighbors (KNN) | 82.12% | [Optimized via Grid Search] - Well-Fit |
| 4 | Decision Tree Classifier | 81.56% | [Optimized via Grid Search] - Well-Fit |
| 4 | Support Vector Classifier (SVM) | 81.56% | [Optimized via Grid Search] - Well-Fit |
| 4 | **XGBoost Classifier** | **81.56%** | [Optimized via Grid Search] - Well-Fit |


## Cara Menjalankan Proyek

### Prasyarat
Pastikan library berikut sudah terinstal di Python Anda:
```bash
pip install pandas numpy scikit-learn matplotlib seaborn xgboost jupyter
```

### Langkah Menjalankan
1. Buka terminal pada folder proyek.
2. Jalankan server Jupyter:
   ```bash
   jupyter notebook
   ```
3. Buka berkas [notebook.ipynb] untuk melihat model optimasi dan alurnya secara lengkap.