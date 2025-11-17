
# Sentiment Classification using Hugging Face Sentence Transformers & FAISS

Repositori ini berisi implementasi lengkap untuk tugas text classification sesuai instruksi **NoLimit Indonesia — Data Scientist Hiring Test** dengan memilih opsi A: Text Classification. 

Proyek ini menggunakan model dari Hugging Face untuk menghasilkan embedding, serta memanfaatkan FAISS sebagai metode ANN (Approximate Nearest Neighbor) untuk pencarian vektor, dan Logistic Regression sebagai model klasifikasi sentimen.

## Latar Belakang
PPKM (Pemberlakukan Pembatasan Kegiatan Masyarakat) merupakan kebijakan pemerintah Indonesia untuk mengendalikan COVID-19. Reaksi masyarakat beragam yakni posistiff, netral hingga negatif. Sehingga analiis sentimen dapat membantu melihat persepsi publik terhadap kebijakan tersebut.

Tujuan utama proyek ini adalah membangun sistem yang mampu mengklasifikasikan sentimen teks ke dalam tiga kategori:

* **0 — Positive**
* **1 — Neutral**
* **2 — Negative**

---

## **1. Project Overview**

Tugas yang dipilih untuk penyelesaian technical test ini adalah:

### **Arsitektur & Alur Kerja**

Rangkaian proses yang digunakan:

1. Load Libraries & Instalation Dependences
2. Pemuatan dataset
3. Exploratory Data Analysis (EDA)
2. Preprocessing ringan
3. Pembuatan embedding menggunakan sentence-transformers
4. Pelatihan model Logistic Regression
5. Pembuatan FAISS index
6. Evaluasi performa model
7. Penyusunan pipeline prediksi
8. Dokumentasi lengkap beserta flowchart

Model bekerja dengan cara memproyeksikan teks ke ruang embedding berdimensi tinggi, kemudian melakukan prediksi menggunakan classifier dan menampilkan contoh paling dekat dari FAISS sebagai referensi.

---

## **2. Dataset**

Dataset yang digunakan diperoleh dari dataset terbuka berasal dari platform Kaggle:

**Kaggle — Indonesian Twitter Sentiment Analysis Dataset-PPKM**
Link: *[dataset](https://www.kaggle.com/datasets/anggapurnama/twitter-dataset-ppkm)*

Dataset tersebut berisi kumpulan tweet berbahasa Indonesia terkait Pemberlakuan Pembatasan Kegiatan Masyarakat (PPKM) yang telah diberi label sentimen. Dataset ini terdiri dari sekitar 20.000 tweet yang dikumpulkan dalam rentang waktu dari 1 April 2020 hingga 1 April 2022. Rentang waktu yang dipilih untuk pengumpulan data didasarkan pada saat Indonesia mulai menerapkan PPKM secara luas dan saat pemerintah mencabut kebijakan tersebut. Dalam dataset ini, dapat ditemukan berbagai pendapat, komentar, dan reaksi masyarakat terkait kebijakan PPKM selama periode tersebut. 

Dataset yang tersedia pada platform tersebut terdiri dari dua jenis, dataset mentah dan sudah bersih dan berlabel. Untuk keperluan proyek ini, file yang digunakan adalah:

```
INA_TweetsPPKM_Labeled_Pure.csv
```
Tujuan memilih dataset yang sudah bersih dan berlabel karena task yang dikerjakan adalah supervised sentiment classification. Dataset berlabel memungkinkan pelatihan model secara langsung, evaluasi objektif (accuracy, F1-score), dan menghasilkan workflow yang lebih stabil serta mudah direproduksi.

### **Lisensi Dataset**

Dataset pada Kaggle tercantum berlisensi:
**CC0: Public Domain**
Artinya dataset bebas digunakan, dimodifikasi, dan didistribusikan untuk tujuan apa pun.
Informasi ini juga dicantumkan pada direktori `data/README.md`.

### **Isi Folder data/**

* `INA_TweetsPPKM_Labeled_Pure.csv` — dataset utama
* `sample_data.csv` — sampel dataset kecil (±20 baris) untuk verifikasi lokal
* `README.md` — berisi sumber dataset, lisensi, dan format kolom

---

## **3. Workflow Overview**

Pipeline sistem dibangun dengan alur sebagai berikut:

### **1. Load Libraries & Installation Dependences**
Membaca dan menginstal libraries dan dependensi yang akan digunakan dalam proyek.

### **2. Load Dataset**

Membaca dataset berlabel untuk persiapan embedding dan pelatihan.

## **2. Exploratory Data Analysis (EDA)**

## **3. Preprocessing Ringan**

Digunakan agar tetap sesuai konteks data Twitter dan tidak menghilangkan makna:

* Lowercase
* Pembersihan URL
* Pembersihan emoji
* Punctuation cleaning

(Tidak dilakukan stemming/stopword removal untuk menjaga konteks semantik embedding.)

### **3. Generate Embeddings**

Model yang digunakan:
**`sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`**

Model ini dipilih karena:

* Mendukung banyak bahasa
* Cepat dan ringan
* Akurasi tinggi untuk semantic similarity

### **4. Split Train–Test**

Pembagian dataset menggunakan secara acak.

### **5. Train Classifier**

Model: **Logistic Regression**
Parameter penting:

```
class_weight="balanced"
max_iter=300
```

Penggunaan `class_weight="balanced"` bertujuan untuk mengatasi ketidakseimbangan label tanpa melakukan oversampling.

### **6. Build FAISS Index**

FAISS digunakan untuk:

* Mencari embedding paling dekat
* Memberikan konteks tambahan saat prediksi
* Mempercepat pencarian vektor

### **7. Evaluation**

Output evaluasi meliputi:

* Precision
* Recall
* F1-score
* Confusion matrix

### **8. Prediction Pipeline**

Sistem prediksi menghasilkan:

* Predicted sentiment
* Confidence score
* Nearest training example via FAISS

---

## **4. Installation & Setup**
### **Download Dataset**
Cara untuk mendownload dataset berikut:
* Akses link dataset berikut: *[dataset](https://www.kaggle.com/datasets/anggapurnama/twitter-dataset-ppkm)*
* Download dataset pada halaman website.
* Extract Dataset.
   
### **Clone Repository**

```bash
git clone https://github.com/<username>/nolimit-ds-test-sumitra-adriansyah.git
cd nolimit-ds-test-sumitra
```

### **Install Dependencies**

```bash
pip install -r requirements.txt
```

Jika menjalankan melalui Google Colab, seluruh dependensi akan terpasang otomatis melalui notebook.

---

## **5. How to Run**

### **Menjalankan via Notebook**

1. Buka file berikut:

```
notebooks/sentiment_classification.ipynb
```

Notebook berisi alur lengkap mulai dari:

* Pemrosesan data
* Pembuatan embedding
* Pelatihan model
* FAISS indexing
* Evaluasi performa
* Contoh prediksi

2. Upload dataset INA_TweetsPPKM_Labeled_Pure.csv
3. Jalankan Seluruh cell dari atas hingga akhir
4. Model akan:
 * Memuat dataset
 * Melatih model
 * Menampilkan hasil evaluasi
 * Menyediakan fungsi prediksi 

---

## **6. Hasil Klasifikasi

Model menghasilkan Classification Report (Precision, Recall, F1-Score) pada proses pelatihan dengan hasil berikut:

Serta hasil Confussion Matrix sebagai berikut:


## **7. Example Predictions**

Berikut beberapa hasil prediksi dari model:

---

**Input:**

> "PPKM bikin masyarakat makin susah"

**Prediction:** Negative
**Confidence:** 0.89
**Nearest example:**

> "… masyarakat kesulitan akibat ppkm …"

---

**Input:**

> "Menurut saya PPKM ini baik"

**Prediction:** Positive
**Confidence:** 0.81
**Nearest example:**

> "… menekan penyebaran covid …"

---

**Input:**

> "Saya merasa PPKM biasa saja"

**Prediction:** Neutral
**Confidence:** 0.72

Contoh lain dapat dilihat pada notebook.

---

## **7. Flowchart**

Flowchart pipeline terdapat pada:

```
flowchart/flowchart-ds-test.png
```

---

## **8. Requirements**

Daftar pustaka yang digunakan:

```
transformers>=4.40.0
datasets>=2.19.0
accelerate>=0.30.0
sentencepiece>=0.2.0
scikit-learn>=1.4.0
pandas>=2.1.0
matplotlib>=3.8.0
seaborn>=0.13.0
faiss-cpu>=1.7.4
tqdm>=4.66.0
sentence-transformers>=2.6.0
huggingface_hub>=0.24.0
joblib>=1.4.0

```

Semua paket dapat dipasang melalui `requirements.txt`.

---
## ** Kesimpulan

