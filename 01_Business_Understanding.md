# 01 — Business Understanding

## Prediksi Kejadian Hujan Esok Hari Berdasarkan Parameter Meteorologi Menggunakan Naive Bayes dan Random Forest

> **Proyek Akhir Data Science | Informatika, UPN “Veteran” Yogyakarta | Tahun Akademik 2026/2027**  
> **Tahap CRISP-DM:** 1 — Business Understanding  
> **Dokumentasi:** Progress 1 (sudah dipresentasikan); dirangkum ulang untuk repository proyek  
> **Tahap implementasi:** perencanaan; hasil performa model belum tersedia

---

## 1. Ringkasan Proyek

Proyek ini bertujuan mengembangkan dan membandingkan dua model klasifikasi, **Gaussian Naive Bayes** dan **Random Forest**, untuk memperkirakan apakah **akan terjadi hujan pada hari berikutnya** berdasarkan parameter meteorologi pada hari pengamatan. Penelitian menggunakan dataset historis **Rain in Australia** (`weatherAUS.csv`). Variabel yang menjadi target adalah `RainTomorrow`, dengan kategori `Yes` (hujan) dan `No` (tidak hujan).

Hasil utama yang ditargetkan berupa model klasifikasi yang dapat dievaluasi secara kuantitatif, perbandingan performa kedua algoritma, serta dokumentasi proses yang dapat direproduksi. Prediksi “esok hari” merujuk pada **hari setelah suatu observasi dalam dataset historis**, bukan ramalan cuaca real-time untuk tanggal kalender saat aplikasi digunakan.

## 2. Latar Belakang dan Konteks Masalah

Informasi mengenai kemungkinan terjadinya hujan dapat membantu perencanaan aktivitas yang sensitif terhadap kondisi cuaca, seperti kegiatan luar ruang, mobilitas, dan aktivitas operasional tertentu. Parameter meteorologi—antara lain suhu, kelembapan udara, tekanan atmosfer, angin, dan curah hujan—dapat dianalisis untuk mengenali pola yang berkaitan dengan kondisi hujan pada hari berikutnya.

Dalam konteks pembelajaran Data Science, persoalan ini sesuai untuk **supervised learning** karena dataset menyediakan fitur meteorologi dan label hasil pengamatan `RainTomorrow`. Penelitian membandingkan pendekatan probabilistik melalui Gaussian Naive Bayes dengan pendekatan *ensemble* berbasis pohon keputusan melalui Random Forest. Perbandingan diharapkan membantu menjelaskan perbedaan kemampuan model dalam mengenali kelas hujan dan tidak hujan.

Penelitian ini merupakan **studi akademik menggunakan data historis Australia**. Keluaran penelitian belum ditujukan untuk menggantikan sistem peringatan dini atau prakiraan resmi lembaga meteorologi.

## 3. Pernyataan Masalah (*Problem Statement*)

**Masalah utama:** Bagaimana memperkirakan kategori `RainTomorrow` (`Yes`/`No`) dari parameter meteorologi historis dengan pendekatan klasifikasi yang dapat dievaluasi dan direproduksi?

Masalah turunan yang perlu dijawab:

1. Parameter meteorologi mana yang relevan sebagai kandidat fitur prediksi?
2. Bagaimana kualitas data (nilai hilang, duplikasi, outlier, dan ketidakseimbangan kelas) memengaruhi rancangan pemodelan?
3. Bagaimana performa Gaussian Naive Bayes dibandingkan Random Forest pada data uji yang sama?
4. Model mana yang lebih sesuai berdasarkan metrik evaluasi, terutama kemampuan mengenali kejadian hujan (`Yes`)?
5. Apa keterbatasan model ketika digunakan di luar cakupan lokasi dan periode historis dataset?

## 4. Tujuan Penelitian

### 4.1 Tujuan Umum

Membangun, mengevaluasi, dan membandingkan model klasifikasi Gaussian Naive Bayes dan Random Forest untuk prediksi kejadian hujan pada hari berikutnya berdasarkan data meteorologi historis.

### 4.2 Tujuan Khusus

1. Mengidentifikasi karakteristik dataset beserta definisi target `RainTomorrow`.
2. Melakukan audit kualitas data dan eksplorasi hubungan fitur meteorologi dengan target.
3. Menyusun pipeline persiapan data yang mencegah *data leakage*.
4. Melatih kedua model menggunakan pembagian data latih dan uji yang konsisten.
5. Mengevaluasi hasil prediksi menggunakan *confusion matrix*, *precision*, *recall*, *F1-score*, dan *accuracy*.
6. Menyimpulkan kelebihan, keterbatasan, dan rekomendasi pemilihan model berdasarkan bukti hasil pengujian.

### 4.3 Sasaran SMART yang Diusulkan

Sasaran berikut merupakan **target perencanaan internal**, bukan hasil yang sudah dicapai dan bukan ambang resmi dari dosen:

| Unsur | Sasaran |
|---|---|
| **Specific** | Mengklasifikasikan `RainTomorrow` menggunakan Gaussian Naive Bayes dan Random Forest. |
| **Measurable** | Menyajikan *confusion matrix*, *precision*, *recall*, *F1-score*, dan *accuracy* untuk kedua model pada data uji yang sama. |
| **Achievable** | Menerapkan kedua algoritma melalui pustaka Python sesuai materi praktikum dengan dataset historis yang tersedia. |
| **Relevant** | Mendukung analisis kemampuan parameter meteorologi dalam membedakan kejadian hujan dan tidak hujan. |
| **Time-bound** | Menyelesaikan implementasi dan evaluasi sebelum tenggat Proyek Akhir sesuai jadwal mata kuliah. |

**Target kualitas awal yang akan diuji:** *F1-score* kelas `Yes` ≥ **0,60** dan *recall* kelas `Yes` ≥ **0,65**. Keduanya merupakan **sasaran usulan** yang perlu dikaji bersama dosen/kelompok, bukan jaminan performa. Seluruh hasil aktual dan kesenjangan terhadap sasaran akan dicatat pada tahap Evaluation.

## 5. Pemangku Kepentingan dan Kebutuhannya

| Pemangku kepentingan | Peran/kebutuhan dalam proyek |
|---|---|
| **Tim mahasiswa** | Melaksanakan analisis, membuat pipeline, mengevaluasi model, dan mendokumentasikan hasil. |
| **Dosen/asisten praktikum** | Menilai ketepatan metodologi, kedalaman analisis, bukti eksekusi, dan reprodusibilitas. |
| **Pengguna potensial (hipotetis)** | Membutuhkan informasi kemungkinan hujan untuk membantu perencanaan aktivitas. Belum ada pengguna operasional yang terlibat langsung. |
| **Penyedia data** | Sumber provenance dan ketentuan pemanfaatan dataset yang perlu dihormati. |

Keberhasilan akademik mencakup **proses yang benar, analisis yang dapat dijelaskan, dan hasil yang dapat direproduksi**, bukan hanya memperoleh angka akurasi tertinggi.

## 6. Ruang Lingkup Penelitian

### 6.1 Termasuk dalam Lingkup

- Data historis `weatherAUS.csv` dari **Rain in Australia**.
- Prediksi biner dengan target `RainTomorrow`: `Yes` atau `No`.
- Pemeriksaan struktur dan kualitas data, EDA, cleaning, preprocessing, dan feature selection.
- Perbandingan **Gaussian Naive Bayes** dan **Random Forest**.
- Evaluasi pada data uji dengan skema pemisahan yang ditetapkan sebelum pelatihan.
- Dokumentasi kode dan hasil melalui Jupyter Notebook serta GitHub.

### 6.2 Di Luar Lingkup

- Prakiraan cuaca real-time atau pengambilan data cuaca terkini melalui API.
- Prediksi jumlah curah hujan dalam satuan milimeter (regresi).
- Peringatan dini cuaca ekstrem dan pengambilan keputusan keselamatan.
- Generalisasi otomatis untuk wilayah Indonesia atau semua wilayah di luar Australia.
- Pembangunan aplikasi web/mobile produksi; implementasi atau *deployment* dapat dibahas sebagai rencana pengembangan jika diminta mata kuliah.

## 7. Gambaran Data dan Target

| Komponen | Keterangan |
|---|---|
| **Dataset** | Rain in Australia |
| **Nama berkas** | `weatherAUS.csv` |
| **Sumber distribusi** | [Kaggle — Rain in Australia](https://www.kaggle.com/datasets/jsphyg/weather-dataset-rattle-package) |
| **Asal pengamatan** | Australian Bureau of Meteorology; dataset dikenal melalui ekosistem Rattle |
| **Jenis masalah** | *Supervised binary classification* |
| **Variabel target** | `RainTomorrow` |
| **Label target** | `Yes` dan `No` |
| **Contoh kelompok fitur** | Suhu, kelembapan, tekanan, curah hujan, angin, dan tutupan awan |
| **Unit observasi** | Catatan cuaca pada lokasi dan tanggal tertentu |

Jumlah baris, tipe data, missing value, distribusi kelas, dan statistik lainnya **didokumentasikan terpisah pada tahap 02 — Data Understanding**, agar setiap tahap CRISP-DM memiliki fokus yang jelas.

## 8. Kriteria Keberhasilan

### 8.1 Keberhasilan dari Sisi Proyek (*Business/Project Success*)

- Pertanyaan penelitian terjawab melalui bukti analisis dan perbandingan kedua model.
- Prosedur penelitian jelas sehingga tim atau dosen dapat menelusuri sumber data, transformasi, pelatihan, dan evaluasi.
- Kelebihan serta keterbatasan metode dijelaskan tanpa klaim kegunaan operasional yang berlebihan.
- Seluruh artefak yang diwajibkan mata kuliah dikumpulkan sesuai jadwal.

### 8.2 Keberhasilan dari Sisi Data Science

- Pembagian data latih dan uji dilakukan secara benar; transformasi yang belajar dari data hanya di-*fit* pada data latih.
- **Baseline** sederhana disiapkan agar hasil kedua algoritma dapat dibandingkan secara masuk akal.
- Kedua model dievaluasi pada **data uji yang sama** dengan metrik yang sama.
- *Precision*, *recall*, dan *F1-score* kelas `Yes` diprioritaskan untuk menilai kemampuan mengenali kejadian hujan; *accuracy* tetap dilaporkan sebagai pelengkap.
- Perbedaan hasil dan *error* dijelaskan memakai *confusion matrix* serta keterbatasan data.

> **Status:** Semua metrik di atas adalah **rencana evaluasi**. Belum ada angka hasil pelatihan model pada tahap Business Understanding.

## 9. Pendekatan Analisis yang Direncanakan

Proyek mengikuti tahapan **CRISP-DM**:

1. **Business Understanding:** menetapkan masalah, tujuan, pemangku kepentingan, ruang lingkup, dan kriteria keberhasilan.
2. **Data Understanding:** menelaah provenance, struktur, kualitas, statistik deskriptif, distribusi, dan hubungan antarvariabel.
3. **Data Preparation:** menetapkan pemisahan data, menangani target hilang, imputasi fitur, encoding, pemeriksaan outlier, dan seleksi fitur.
4. **Modeling:** melatih Gaussian Naive Bayes dan Random Forest serta melakukan penyesuaian parameter secara terkendali.
5. **Evaluation:** membandingkan hasil pada data uji, membahas kesalahan prediksi dan keterbatasan.
6. **Deployment:** mendokumentasikan kemungkinan implementasi dan kebutuhan data jika pengembangan lanjutan diperlukan; **bukan kewajiban membuat aplikasi produksi dalam ruang lingkup saat ini**.

**Rancangan validasi:** mengutamakan pemisahan data berbasis urutan waktu untuk mengurangi penggunaan informasi masa depan saat menguji model pada observasi yang lebih baru. Tanggal batas pemisahan dan detail preprocessing akan dipastikan dalam notebook tahap Data Preparation.

## 10. Asumsi dan Keterbatasan

- Observasi berlabel merepresentasikan kategori hujan esok hari berdasarkan definisi dataset; label harus diperiksa kembali pada tahap Data Understanding.
- Fitur prediksi yang digunakan tidak boleh memanfaatkan informasi yang baru diketahui setelah waktu prediksi.
- Pola cuaca pada periode historis belum tentu menggambarkan kondisi masa kini.
- Ketersediaan sensor, kelengkapan pencatatan, dan jumlah observasi dapat berbeda antarlokasi.
- Ketidakseimbangan kelas dan nilai hilang berpotensi memengaruhi estimasi performa.
- Satuan pengukuran, definisi atribut, dan syarat pemanfaatan data harus merujuk pada dokumentasi sumber.

## 11. Risiko dan Rencana Mitigasi

| Risiko | Dampak potensial | Mitigasi yang direncanakan |
|---|---|---|
| Nilai hilang pada fitur/target | Data latih berkurang atau prediksi bias | Audit missing value; tangani label kosong; imputasi fitur berdasarkan data latih. |
| Kelas `Yes` lebih sedikit daripada `No` | Model cenderung memprediksi `No` | Gunakan metrik kelas `Yes`, stratifikasi atau strategi ketidakseimbangan yang sesuai tanpa kebocoran data. |
| *Data leakage* | Evaluasi terlihat terlalu baik tetapi tidak realistis | Tetapkan batas waktu train/test; *fit* preprocessing hanya pada training; audit fitur yang mengandung informasi masa depan. |
| Distribusi berubah menurut waktu/lokasi | Performa tidak stabil pada data baru | Gunakan evaluasi holdout dan bahas keterbatasan cakupan. |
| Outlier meteorologi sebenarnya valid | Informasi kejadian ekstrem hilang | Validasi konteks sebelum menghapus atau mengoreksi outlier. |
| Lisensi/izin redistribusi tidak jelas | Masalah kepatuhan publikasi | Cantumkan sumber dan tautan unduhan resmi; hindari mengunggah ulang CSV ke repository publik sebelum izin dipastikan. |
| Ketergantungan lingkungan komputasi | Notebook sulit dijalankan ulang | Dokumentasikan versi perangkat lunak, sumber dataset, dan urutan eksekusi. |

## 12. Luaran dan Bukti Penyelesaian

| Tahap | Luaran yang direncanakan | Status |
|---|---|---|
| Business Understanding | `01_Business_Understanding.md`, materi/laporan Progress 1 | Dokumentasi disusun; Progress 1 telah dipresentasikan |
| Data Understanding | Notebook EDA, data dictionary, audit kualitas, insight | Notebook sudah dibuat; verifikasi hasil dan kelengkapan mengikuti rubrik |
| Data Preparation | Notebook cleaning, preprocessing, splitting, dan feature selection | Belum selesai |
| Modeling | Notebook eksperimen Gaussian Naive Bayes dan Random Forest | Belum dimulai dalam dokumentasi ini |
| Evaluation | Tabel metrik, confusion matrix, analisis perbandingan, kesimpulan | Belum dimulai dalam dokumentasi ini |

## 13. Keputusan Proyek (*Decision Log*)

| Keputusan | Alasan |
|---|---|
| Menggunakan `RainTomorrow` sebagai target | Sesuai tujuan klasifikasi hujan pada hari setelah observasi. |
| Membandingkan Gaussian Naive Bayes dan Random Forest | Memberikan dua pendekatan berbeda: probabilistik dan *ensemble tree*. |
| Memisahkan dokumen setiap tahap CRISP-DM | Memudahkan penelusuran progres, tugas kelompok, dan pemeriksaan dosen. |
| Menggunakan GitHub untuk dokumentasi | Memudahkan akses ke kode, riwayat perubahan, dan bukti reprodusibilitas. |
| Tidak menyertakan dataset mentah di GitHub publik untuk sementara | Izin distribusi ulang belum dipastikan. |

## 14. Referensi Awal

- Young, J. (n.d.). *Rain in Australia* [Data set]. Kaggle. <https://www.kaggle.com/datasets/jsphyg/weather-dataset-rattle-package>
- Breiman, L. (2001). Random forests. *Machine Learning, 45*, 5–32. <https://doi.org/10.1023/A:1010933404324>
- IBM. (n.d.). *CRISP-DM help overview*. IBM Documentation. <https://www.ibm.com/docs/en/spss-modeler/saas?topic=dm-crisp-help-overview>
- Scikit-learn developers. (n.d.). *Naive Bayes*. Scikit-learn documentation. <https://scikit-learn.org/stable/modules/naive_bayes.html>

---

**Catatan dokumentasi:** Dokumen ini menyatukan keputusan dan rancangan awal penelitian yang telah dibahas dalam pekerjaan Progress 1. Jika terdapat perbedaan redaksi atau target dengan laporan yang disetujui dosen, gunakan laporan final sebagai acuan utama dan perbarui dokumen ini sebelum commit.
