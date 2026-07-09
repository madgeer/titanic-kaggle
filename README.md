<img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/5c68dc65-cf60-41cc-9274-02cf75989426" />

# Titanic Survival Prediction 

Repositori ini berisi proyek analisis data dan pemodelan prediktif untuk memprediksi probabilitas keselamatan (survival) penumpang kapal Titanic berdasarkan data sosio-demografis dan detail perjalanan mereka. Proyek ini menggunakan dataset dari kompetisi populer **Titanic: Machine Learning from Disaster** di Kaggle.

## Struktur Proyek
- **notebook.ipynb**: Notebook yang berisi alur lengkap EDA (Exploratory Data Analysis), preprocessing data, visualisasi korelasi, pelatihan model, dan evaluasi hasil.
- **train.csv**: Dataset training yang digunakan untuk melatih model ML (berisi kolom target `Survived`).
- **test.csv**: Dataset testing untuk menguji performa model prediksi.
- **README.md**: Dokumentasi proyek ini.


## Alur Analisis & Pemodelan

Proyek ini terstruktur ke dalam beberapa tahapan utama di dalam notebook:

### 1. Data Cleaning & Preprocessing
- **Handling Missing Values**:
  - Kolom `Age` diisi menggunakan nilai median dari data training.
  - Kolom `Embarked` diisi dengan nilai modus (stasiun keberangkatan paling sering).
- **Feature Selection**:
  - Menghapus kolom yang tidak relevan atau memiliki terlalu banyak kategori unik seperti `Cabin`, `PassengerId`, `Name`, dan `Ticket`.
- **Feature Encoding**:
  - Mengubah kolom `Sex` (kategorikal) menjadi format biner (`0` untuk laki-laki, `1` untuk perempuan).
  - Menggunakan One-Hot Encoding (`pd.get_dummies`) pada kolom `Embarked` dengan opsi `drop_first=True` guna menghindari *dummy variable trap*.

### 2. Standardisasi Fitur
- Melakukan normalisasi skala fitur numerik menggunakan kelas `StandardScaler` dari scikit-learn pada data latih dan data uji sebelum masuk ke model klasifikasi.

### 3. Pembagian Data (Data Splitting)
- Data dibagi menjadi subset latih (*train*) dan subset uji (*test*) dengan proporsi **80% : 20%** menggunakan `train_test_split` dan seed acak (`RANDSEED = 42`) untuk hasil reproduksibel.

### 4. Pelatihan Model & Evaluasi
Sebanyak 5 algoritma klasifikasi diuji dan dibandingkan kinerjanya:
1. **Logistic Regression** 
2. **Decision Tree Classifier** 
3. **Random Forest Classifier** 
4. **K-Nearest Neighbors (KNN)**
5. **Support Vector Classifier (SVM)**


## Hasil Evaluasi Model

Berikut adalah ringkasan akurasi dari kelima model klasifikasi yang dievaluasi pada data uji (20% data split):

| Peringkat | Algoritma Klasifikasi | Akurasi (%) |
| :---: | :--- | :---: |
| 1 | **Support Vector Machine (SVM)** | **82.12%** |
| 2 | Logistic Regression | 81.01% |
| 3 | K-Nearest Neighbors (KNN) | 80.45% |
| 4 | Random Forest | 79.89% |
| 5 | Decision Tree | 78.77% |

> [!NOTE]  
> Model **Support Vector Machine (SVM / SVC)** menghasilkan performa terbaik dengan tingkat akurasi mencapai **82.12%**.

## Cara Menjalankan Proyek

### Prasyarat
Pastikan Anda telah menginstal beberapa library Python berikut:
```bash
pip install pandas numpy scikit-learn matplotlib jupyter
```

### Langkah Menjalankan
1. Buka terminal/command prompt pada direktori proyek ini.
2. Jalankan server Jupyter:
   ```bash
   jupyter notebook
   ```
3. Buka berkas notebook.ipynb dan jalankan sel kode secara berurutan.
