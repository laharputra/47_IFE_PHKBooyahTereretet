# Proyek Akhir Praktikum Data Science 2026/2027 — Kelompok 47

**Judul:** Prediksi Kejadian Hujan Esok Hari Berdasarkan Parameter Meteorologi Menggunakan Naive Bayes dan Random Forest  
**Kelas:** IF-E, Informatika UPN "Veteran" Yogyakarta
**Anggota Kelompok:** 47
**Anggota Kelompok:**
1.	Lahar Nuansa Putra		(123240053)
2.	Fairuz Alif Maghribi		(123240176)
3.	Arya Kresna Diandra Putra	(123240238)
4.	Naufal Shidqi R’A.A		(123240260)
**Tipe masalah:** Supervised learning — klasifikasi biner (`RainTomorrow`: `Yes` atau `No`)

## Gambaran proyek

Proyek ini membangun model klasifikasi untuk memperkirakan apakah hari setelah suatu pengamatan meteorologi akan terjadi hujan. Istilah *esok hari* berarti hari berikutnya **di dalam data historis**, bukan prakiraan realtime untuk tanggal besok saat pengguna membuka notebook.

Dataset yang digunakan adalah **Rain in Australia**, dengan 145.460 observasi awal, 23 kolom, dan 49 lokasi. Sumber: [Kaggle — Rain in Australia](https://www.kaggle.com/datasets/jsphyg/weather-dataset-rattle-package) dan [dokumentasi Rattle](https://rdrr.io/cran/rattle.data/man/weather.html). Periksa persyaratan lisensi dataset apabila CSV hendak dibagikan ulang secara publik.

## Struktur repository

```text
47_IFE_PHKBooyahTereretet/
├── Project_Akhir_Data_Science_Kelompok_47.ipynb
├── weatherAUS.csv
└── README.md
```

Seluruh langkah proyek berada dalam **satu notebook**:

1. Business Understanding
2. Data Gathering
3. Data Understanding dan EDA
4. Data Cleaning dan Preprocessing
5. Feature Selection
6. Modeling — Gaussian Naive Bayes dan Random Forest
7. Model Evaluation
8. Results and Discussion
9. Conclusion

File JSON hasil notebook tahap lama tidak diperlukan untuk menjalankan notebook utama. Daftar fitur dan konfigurasi model disimpan sebagai variabel di dalam notebook.

## Cara menjalankan

1. Unduh/salin seluruh file pada struktur di atas.
2. Buka `Project_Akhir_Data_Science_Kelompok_47.ipynb` di **VS Code dengan ekstensi Jupyter** atau Jupyter Notebook.
3. Gunakan Python dengan paket `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn` versi 1.2 atau lebih baru, dan `IPython`.
4. Pastikan `weatherAUS.csv` berada di samping notebook (atau di subfolder `data/`).
5. Jalankan **Restart Kernel → Run All**. Training sejumlah kombinasi Random Forest dapat membutuhkan beberapa menit, tergantung komputer.
6. Simpan notebook setelah selesai agar output grafik, tabel, dan metrik pengujian turut tersimpan.

## Metodologi

- Data dibersihkan dari tanggal/target tidak valid dan duplikasi, lalu atribut yang berisiko membocorkan target seperti `RISK_MM` dikecualikan.
- Pemisahan dilakukan **berdasarkan tanggal**, dengan jeda satu hari menjelang periode validasi/testing untuk mencegah label `RainTomorrow` melintasi batas waktu.
- Feature Selection mencakup audit missing value, Mutual Information, dan korelasi. Baseline historis 19 fitur tetap dibandingkan dengan alternatif fitur yang telah diuji pada notebook sebelumnya.
- Preprocessing untuk model berada dalam `Pipeline`, sehingga imputasi, scaling, serta encoding dipelajari pada training dan diterapkan ke validation/test.
- Lima alternatif fitur diuji dengan **dua algoritma** pada validation (10 eksperimen). Pemilihan model menggunakan F1-score dan recall kelas hujan (`Yes`), bukan data testing.
- Kandidat terpilih dilatih ulang pada 109.254 baris training, kemudian dinilai pada 32.891 baris testing yang disisihkan.

## Hasil pengujian

Berikut hasil pengujian ketika notebook gabungan dijalankan pada dataset asli di lingkungan penyusun. Nilai dapat sedikit berbeda jika versi pustaka atau sistem berubah. **Ini hasil testing, bukan hasil validation.**

| Model dan varian fitur | Accuracy | Precision `Yes` | Recall `Yes` | F1-score `Yes` |
|---|---:|---:|---:|---:|
| Random Forest + Sunshine (20 fitur) | 80,34% | 53,91% | 71,10% | 61,32% |
| Gaussian Naive Bayes + Sunshine (20 fitur) | 64,72% | 34,52% | 67,94% | 45,78% |

Random Forest + Sunshine dipilih berdasarkan validation. Hasil testing ditampilkan sebagai **evaluasi final**, bukan digunakan untuk memilih ulang model.

## Keterbatasan

- Dataset tidak realtime dan berasal dari Australia, sehingga performa di lokasi atau periode cuaca lain perlu diuji ulang.
- Kelas hujan (`Yes`) lebih sedikit daripada kelas `No`. Karena itu, accuracy perlu dibaca bersama recall/F1 kelas hujan.
- Beberapa atribut memiliki banyak nilai kosong; imputasi tidak dapat menggantikan pengukuran aktual yang tidak tersedia.

Untuk penjelasan lengkap, grafik, confusion matrix, audit fitur, dan contoh prediksi, buka notebook utama.
