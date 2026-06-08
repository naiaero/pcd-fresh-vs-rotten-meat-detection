# Analisis Pengaruh Kombinasi Metode Preprocessing Citra Digital terhadap Akurasi Klasifikasi Kesegaran Daging Sapi Menggunakan Ekstraksi Fitur GLCM

## Penjelasan Singkat

Fokus utama dari project ini bukan sekadar membuat model klasifikasi yang akurat, melainkan melakukan analisis kritis terhadap tahap preprocessing. Mengingat kualitas foto daging dari kamera bisa bervariasi (tergantung pencahayaan, adanya noise, atau latar belakang yang mengganggu), tahap preprocessing menjadi kunci krusial sebelum fitur tekstur daging diekstrak menggunakan metode GLCM.

---

## Tujuan Penelitian

Menganalisis pengaruh berbagai kombinasi metode preprocessing citra digital terhadap kualitas fitur tekstur yang dihasilkan oleh metode Gray Level Co-occurrence Matrix (GLCM), serta dampaknya terhadap akurasi klasifikasi kesegaran daging sapi.

---

## Alur Eksperimen

Penelitian ini terdiri dari tiga skenario preprocessing yang akan dibandingkan performanya.

### Eksperimen 1

**Pipeline:**

```text
Citra Asli
    ↓
Grayscale
    ↓
Histogram Equalisasi
    ↓
Ekstraksi Fitur GLCM
```

**Deskripsi:**

Pada eksperimen ini citra asli terlebih dahulu dikonversi ke grayscale, kemudian dilakukan histogram equalisasi untuk meningkatkan kontras citra sebelum proses ekstraksi fitur tekstur menggunakan GLCM.

---

### Eksperimen 2

**Pipeline:**

```text
Citra Asli
    ↓
Grayscale
    ↓
Gaussian Blur
    ↓
Contrast Stretching
    ↓
Ekstraksi Fitur GLCM
```

**Deskripsi:**

Pada eksperimen ini citra grayscale terlebih dahulu diproses menggunakan Gaussian Blur untuk mengurangi noise, kemudian dilakukan contrast stretching guna memperlebar rentang intensitas piksel sebelum ekstraksi fitur GLCM.

---

### Eksperimen 3

**Pipeline:**

```text
Citra Asli
    ↓
Grayscale
    ↓
Masking Background
    ↓
Histogram Equalisasi
    (Area Daging Saja)
    ↓
Ekstraksi Fitur GLCM
```

**Deskripsi:**

Pada eksperimen ini dilakukan pemisahan area daging dari latar belakang menggunakan masking. Histogram equalisasi hanya diterapkan pada area daging sehingga informasi tekstur yang diekstraksi oleh GLCM lebih terfokus pada objek utama.

---

## Perbandingan Eksperimen

| Eksperimen | Tahap Preprocessing |
|------------|---------------------|
| 1 | Grayscale → Histogram Equalisasi |
| 2 | Grayscale → Gaussian Blur → Contrast Stretching |
| 3 | Grayscale → Masking Background → Histogram Equalisasi (Area Daging Saja) |

---

## Fokus Analisis

Analisis pada penelitian ini difokuskan pada:

- Pengaruh masing-masing metode preprocessing terhadap kualitas citra.
- Pengaruh preprocessing terhadap nilai fitur tekstur yang dihasilkan GLCM.
- Perbandingan akurasi klasifikasi pada setiap skenario preprocessing.
- Identifikasi kombinasi preprocessing yang paling efektif untuk klasifikasi kesegaran daging sapi.

---

## Metode Ekstraksi Fitur

Fitur tekstur citra diekstraksi menggunakan metode **Gray Level Co-occurrence Matrix (GLCM)**.

---

## Hasil yang Diharapkan

Diperoleh pemahaman mengenai kombinasi preprocessing yang paling optimal dalam meningkatkan kualitas fitur tekstur dan akurasi klasifikasi kesegaran daging sapi.