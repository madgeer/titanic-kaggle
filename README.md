# Titanic Survival Prediction

Repositori ini berisi proyek analisis data dan pemodelan prediktif untuk memprediksi probabilitas keselamatan (survival) penumpang kapal Titanic berdasarkan data sosio-demografis dan detail perjalanan mereka. Proyek ini menggunakan dataset dari kompetisi populer **Titanic: Machine Learning from Disaster** di Kaggle.

## Struktur Proyek
- [notebook.ipynb](file:///D:/Project/data-science/titanic/notebook.ipynb): Jupyter Notebook yang berisi alur lengkap EDA (Exploratory Data Analysis), preprocessing data, visualisasi korelasi, pelatihan model, dan evaluasi hasil.
- [train.csv](file:///D:/Project/data-science/titanic/train.csv): Dataset training yang digunakan untuk melatih model ML (berisi kolom target `Survived`).
- [test.csv](file:///D:/Project/data-science/titanic/test.csv): Dataset testing untuk menguji performa model prediksi.
- [README.md](file:///D:/Project/data-science/titanic/README.md): Dokumentasi proyek ini.

---

## Alur Analisis & Pemodelan

Proyek ini terstruktur ke dalam beberapa tahapan utama di dalam notebook:

### 1. Data Cleaning & Preprocessing
- **Handling Missing Values**:
  - Kolom `Age` diisi menggunakan nilai median dari data training.
  - Kolom `Embarked` diisi dengan nilai modus (stasiun keberangkatan paling sering).
- **Feature Selection**:
  - Menghapus kolom yang tidak relevan seperti `Cabin`, `PassengerId`, dan `Ticket`.
- **Feature Engineering & Encoding**:
  - Mengekstrak gelar kehormatan (`Title`) dari kolom `Name` menggunakan pola regular expression (contoh: *Mr, Mrs, Miss, Master, Dr, Rev* dll.) lalu menghapus kolom asli `Name`.
  - Mengubah kolom `Sex` (kategorikal) menjadi format biner (`0` untuk laki-laki, `1` untuk perempuan).
  - Membuat fitur `FamilySize` (jumlah anggota keluarga) dengan menjumlahkan `SibSp` + `Parch` + 1.
  - Membuat fitur biner `IsAlone` (apakah penumpang bepergian sendirian) bernilai 1 jika `FamilySize` sama dengan 1, dan 0 jika lainnya.
  - Menggunakan One-Hot Encoding (`pd.get_dummies`) pada kolom `Embarked` dan `Title` dengan opsi `drop_first=True` guna menghindari trap.

### 2. Standardisasi Fitur
- Melakukan normalisasi skala fitur numerik menggunakan kelas `StandardScaler` dari scikit-learn pada data latih dan data uji sebelum masuk ke model klasifikasi.

### 3. Pembagian Data (Data Splitting)
- Data dibagi menjadi subset latih (*train*) dan subset uji (*test*) dengan proporsi **80% : 20%** menggunakan `train_test_split` dan seed acak (`RANDSEED = 42`) untuk hasil reproduksibel.

### 4. Pelatihan Model & Evaluasi
Sebanyak 6 algoritma klasifikasi diuji dan dibandingkan kinerjanya:
1. **Logistic Regression** (Regresi Logistik)
2. **Decision Tree Classifier** (Pohon Keputusan)
3. **Random Forest Classifier** (Ansambel Decision Tree)
4. **K-Nearest Neighbors (KNN)**
5. **Support Vector Classifier (SVM)**
6. **XGBoost Classifier** (eXtreme Gradient Boosting)

---

## Hasil Evaluasi Model

Berikut adalah ringkasan akurasi dari keenam model klasifikasi yang dievaluasi pada data uji (20% data split):

| Peringkat | Algoritma Klasifikasi | Akurasi (%) |
| :---: | :--- | :---: |
| 1 | **XGBoost Classifier** | **84.36%** |
| 2 | **Random Forest Classifier** | **83.24%** |
| 3 | Logistic Regression | 81.56% |
| 4 | Support Vector Machine (SVM) | 81.01% |
| 4 | K-Nearest Neighbors (KNN) | 81.01% |
| 6 | Decision Tree Classifier | 77.10% |

> [!NOTE]  
> Model **XGBoost Classifier** menghasilkan performa terbaik dengan tingkat akurasi mencapai **84.36%** setelah penambahan fitur `FamilySize` dan `IsAlone`, disusul oleh **Random Forest** dengan akurasi **83.24%**.

---

## Cara Menjalankan Proyek

### Prasyarat
Pastikan Anda telah menginstal beberapa library Python berikut:
```bash
pip install pandas numpy scikit-learn matplotlib xgboost jupyter
```