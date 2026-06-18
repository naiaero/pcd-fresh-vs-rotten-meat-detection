# Analisis Pengaruh Kombinasi Metode Preprocessing Citra Digital terhadap Akurasi Klasifikasi Kesegaran Daging Sapi Menggunakan Ekstraksi Fitur GLCM

## Anggota Kelompok
1. SALSABILA NAILAFAHDI - F1D02410135
2. BAIQ ERWINA YOLANDA - F1D02410039
3. ANDREA CORINA RAHMADI - F1D02410104
4. HAIDAR WAHYU YASARI - F1D02410114

## Latar Belakang
Daging merupakan salah satu sumber protein hewani yang paling banyak dikonsumsi oleh masyarakat. Namun, daging memiliki sifat yang mudah rusak (*perishable food*) akibat aktivitas mikroorganisme dan kerusakan enzimatis seiring berjalannya waktu. Mengonsumsi daging yang telah busuk dapat membahayakan kesehatan manusia karena risiko keracunan makanan. Secara konvensional, penentuan kesegaran daging masih mengandalkan indra manusia (visual dan aroma), yang bersifat subjektif dan rentan terhadap kesalahan. Oleh karena itu, diperlukan sebuah sistem komputasi cerdas berbasis Pemrosesan Citra Digital (PCD) yang mampu mengenali pola tekstur dan mengklasifikasikan kondisi daging secara objektif, cepat, dan non-destruktif.

## Deskripsi Program
Program ini dirancang untuk mendeteksi dan mengklasifikasikan citra daging ke dalam dua kategori, yaitu **Daging Segar (Fresh Meat)** dan **Daging Busuk (Rotten Meat)**. 

Alur utama program ini meliputi:
1. **Preprocessing Citra**: Mengondisikan citra mentah agar siap diekstrak fiturnya.
2. **Ekstraksi Fitur Tekstur (GLCM)**: Mengambil 6 fitur utama GLCM (*Contrast, Dissimilarity, Homogeneity, Entropy, ASM, Energy, Correlation*) pada 4 sudut arah ($0^\circ, 45^\circ, 90^\circ, 135^\circ$).
3. **Seleksi Fitur berbasis Korelasi**: Menyaring fitur yang memiliki korelasi redundan di atas *threshold* 0.95.
4. **Klasifikasi**: Melatih dan menguji data menggunakan tiga algoritma *Machine Learning*, yaitu **Random Forest**, **Support Vector Machine (SVM)**, dan **K-Nearest Neighbors (KNN)**.

## Penjelasan Dataset
Dataset yang digunakan dalam proyek ini terdiri dari citra foto digital permukaan daging yang dibagi menjadi dua kelas:
* **FreshMeat (Daging Segar)**: Karakteristik visual cenderung berwarna merah cerah alami dengan pola lemak (*marbling*) putih yang bersih.
* **RottenMeat (Daging Busuk)**: Karakteristik visual cenderung mengalami perubahan warna menjadi pucat, keabu-abuan, atau kecokelatan akibat proses pembusukan.

Setiap citra diubah ukurannya (*resize*) menjadi dimensi $128 \times 128$ piksel sebelum masuk ke tahap preprocessing berikutnya demi efisiensi komputasi.

## Tahapan Preprocessing
Proyek ini melakukan skenario pengujian melalui 4 eksperimen preprocessing yang berbeda untuk melihat pengaruhnya terhadap akurasi model:

### Eksperimen 0: Grayscale + Resize
Citra diubah dari format BGR menjadi citra keabuan (*grayscale*) standar untuk menyederhanakan saluran warna menjadi intensitas kecerahan tunggal, lalu langsung diekstrak fiturnya menggunakan GLCM.

### Eksperimen 1: Grayscale + Resize + Histogram Equalization (HE) + Gaussian Blur
Citra *grayscale* ditingkatkan kontrasnya secara global menggunakan HE, kemudian dihaluskan menggunakan Gaussian Blur untuk membuang detail-detail mikro dan *noise* frekuensi tinggi.

### Eksperimen 2: Grayscale + Resize + CLAHE + Median Blur
Citra kontrasnya ditingkatkan secara lokal menggunakan CLAHE (Contrast Limited Adaptive Histogram Equalization) agar tidak terjadi *over-saturation*, kemudian direduksi bintik *noise*-nya menggunakan Median Blur dengan tetap menjaga ketajaman garis tepi serat daging (*edge-preserving*).

### Eksperimen 3: Grayscale + Resize + Otsu Thresholding + Opening + Bitwise AND (Segmentasi Latar Belakang)
Citra dipisahkan antara objek daging dengan latar belakang menggunakan *Otsu Thresholding*. Masker biner tersebut kemudian dibersihkan dari noda kecil menggunakan operasi morfologi *Opening* (Erosi diikuti Dilasi). Terakhir, operasi *Bitwise AND* diterapkan pada citra asli sehingga latar belakang nampan/talenan berubah menjadi hitam pekat ($0$) sempurna, sementara tekstur asli di dalam daging tetap murni $100\%$.

## Analisis Hasil Eksperimen

### Perbandingan Akurasi Setiap Eksperimen
Berikut adalah rangkuman persentase akurasi data uji (*Testing Set*) dari seluruh skenario eksperimen:

| Model | Eksperimen 0 (Polos) | Eksperimen 1 (HE + Gaussian) | Eksperimen 2 (CLAHE + Median) | Eksperimen 3 (Otsu + Morph + AND) |
| :--- | :---: | :---: | :---: | :---: |
| **Random Forest** | 91.75% | 86.50% | 92.25% | **93.00%** |
| **SVM (RBF)** | 92.50% | 83.00% | 92.25% | **94.75%** |
| **KNN (K=5)** | **96.00%** | 90.25% | 92.25% | 94.25% |

### Analisis Hasil
1. **Kegagalan Eksperimen 1**: Penggunaan Gaussian Blur merusak detail spasial ketetanggaan piksel yang sangat dibutuhkan oleh GLCM (*feature smearing*), mengakibatkan penurunan akurasi yang tajam pada semua model.
2. **Kestabilan Eksperimen 2**: CLAHE dan Median Blur berhasil memperbaiki kekurangan Eksperimen 1. Hasil akurasi yang konvergen sama rata (92.25%) menunjukkan fitur yang dihasilkan sangat solid dan mencapai batas optimal informasi spasial lokal citra.
3. **Keunggulan Eksperimen 3 untuk Model Parametrik**: Menghilangkan latar belakang nampan melalui segmentasi murni menyisakan fitur daging tanpa distorsi tekstur. Hal ini membuat performa SVM melonjak hingga **94.75%** dan Random Forest hingga **93.00%**.
4. **Keunikan KNN pada Eksperimen 0**: KNN mencapai hasil tertinggi (**96.00%**) pada citra polos. Hal ini dikarenakan KNN bekerja berbasis jarak mentah objek, di mana variasi latar belakang asli yang organik justru secara tidak sengaja membantu KNN mengelompokkan sampel uji ke tetangga terdekatnya yang tepat.

## Kesimpulan
Berdasarkan seluruh pengujian skenario preprocessing yang telah dilakukan, dapat disimpulkan bahwa **tahap preprocessing memegang peranan krusial dalam keberhasilan metode ekstraksi fitur tekstur spasial seperti GLCM**. Metode pelembutan citra secara global seperti Gaussian Blur tidak disarankan untuk klasifikasi tekstur serat karena mengaburkan detail mikro piksel. 

Akan tetapi, teknik **segmentasi objek (Otsu Thresholding + Opening + Bitwise AND)** merupakan preprocessing terbaik untuk mempertegas batas kelas bagi model **SVM** dan **Random Forest**. Proyek klasifikasi ini berhasil membuktikan bahwa dengan preprocessing yang tepat, algoritma *Machine Learning* konvensional mampu menghasilkan performa deteksi kesegaran daging yang sangat akurat, objektif, dan bersaing tinggi.