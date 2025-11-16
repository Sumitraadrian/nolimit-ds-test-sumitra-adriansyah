
# Sentiment Classification using Hugging Face Sentence Transformers & FAISS

Repositori ini berisi implementasi lengkap untuk tugas text classification sesuai instruksi **NoLimit Indonesia — Data Scientist Hiring Test**.
Proyek ini menggunakan model dari Hugging Face untuk menghasilkan embedding, memanfaatkan FAISS sebagai metode ANN (Approximate Nearest Neighbor) untuk pencarian vektor, serta Logistic Regression sebagai model klasifikasi sentimen.

Tujuan utama proyek ini adalah membangun sistem yang mampu mengklasifikasikan sentimen teks ke dalam tiga kategori:

* **0 — Positive**
* **1 — Neutral**
* **2 — Negative**

Seluruh tahapan pipeline diimplementasikan dalam notebook dan digambarkan dalam flowchart yang tersedia pada repositori.

---

## **1. Project Overview**

Tugas yang dipilih untuk penyelesaian technical test ini adalah:

### **A. Classification — Sentiment Analysis**

Rangkaian proses yang digunakan:

1. Load Libraries & Instalation Dependences
2. Pemuatan dataset berlabel
3. Exploratory Data Analysis (EDA)
2. Preprocessing ringan
3. Pembuatan embedding menggunakan sentence-transformers
4. Pembuatan indeks FAISS
5. Pelatihan model Logistic Regression
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

### **1. LOad Libraries & Installation Dependences
Membaca dan menginstal libraries dan dependensi yang akan digunakan dalam proyek.

### **2. Load Dataset**

Membaca dataset berlabel untuk persiapan embedding dan pelatihan.

## **2. Exploratory Data Analysis (EDA)

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

### **A. Menjalankan via Notebook**

Buka file berikut:

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

Semua sel siap dijalankan tanpa perubahan tambahan.

---

### **B. Menjalankan via Script (Opsional)**

Jika ingin melakukan prediksi cepat dari terminal:

```bash
python src/predict.py --text "PPKM membantu menekan COVID"
```

Output yang ditampilkan:

* Sentiment prediction
* Confidence score
* Nearest example dari FAISS

---

## **6. Example Predictions**

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
flowchart/pipeline_flowchart.png
```

Bagan memuat alur:

Input → Preprocessing → Embedding
→ Train Classifier & Build FAISS
→ Evaluation
→ Prediction Output

Alur dibuat linear dan ringkas sesuai instruksi teknis.

---

## **8. Requirements**

Daftar pustaka yang digunakan:

```
pandas
numpy
scikit-learn
sentence-transformers
faiss-cpu
matplotlib
seaborn
```

Semua paket dapat dipasang melalui `requirements.txt`.

---

## **9. Contact**

Jika dibutuhkan informasi tambahan:

**Author:** Sumitra Adriansyah
**Email:** <isi-email>
**Repository:** nolimit-ds-test-sumitra

---

# **Struktur Folder Final**

Berikut struktur folder yang bisa Anda gunakan di GitHub:

```
nolimit-ds-test-sumitra/
│
├── data/
│   ├── INA_TweetsPPKM_Labeled_Pure.csv
│   ├── sample_data.csv
│   └── README.md
│
├── notebooks/
│   └── sentiment_classification.ipynb
│
├── src/
│   ├── predict.py
│   └── utils.py
│
├── flowchart/
│   └── pipeline_flowchart.png
│
├── requirements.txt
├── README.md
└── LICENSE  (opsional)
```

---
