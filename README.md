# ETH ARIMA Forecasting Analysis

Analisis time series harga Ethereum (ETH) menggunakan model ARIMA. Project ini berfokus pada workflow forecasting yang rapi: validasi data, uji stasioneritas, pemilihan model tanpa data leakage, diagnostic residual, dan evaluasi forecast out-of-sample.

[Open in Google Colab](https://colab.research.google.com/drive/1E5_PT1LhZxsKd_gwqBorksiFGKhd8y-V?usp=sharing)

## Dataset

Dataset berada pada file `ETH.xlsx` dan berisi 119 observasi harga ETH.

- Kolom waktu: `t`
- Kolom target: `harga_ETH`
- Periode data: `2025-10-21 01:22:38` sampai `2025-10-21 02:11:57`
- Split data: 80% train dan 20% test secara kronologis

## Methodology

Tahapan analisis:

1. Load dan validasi data dari `ETH.xlsx`
2. Konversi kolom waktu menjadi datetime index
3. Eksplorasi pola harga ETH
4. Uji stasioneritas menggunakan Augmented Dickey-Fuller (ADF)
5. Transformasi log-difference untuk mengecek stasioneritas setelah differencing
6. Analisis ACF dan PACF
7. Grid search ARIMA pada data training saja
8. Pemilihan model berdasarkan BIC dan validation error
9. Diagnostic residual menggunakan Ljung-Box, Shapiro-Wilk, dan ARCH test
10. Evaluasi forecast out-of-sample pada data test

## Key Results

Hasil uji stasioneritas:

| Series | ADF Statistic | p-value | Kesimpulan |
|---|---:|---:|---|
| Harga ETH full data | -2.2335 | 0.1943 | Tidak stasioner |
| Harga ETH train | -1.8788 | 0.3421 | Tidak stasioner |
| Log-difference train | -3.5122 | 0.0077 | Stasioner |

Model terbaik yang dipilih:

```text
ARIMA(2, 1, 2)
```

Evaluasi test out-of-sample:

| Metric | Value |
|---|---:|
| MAE | 3.67 |
| RMSE | 4.36 |
| MAPE | 0.092% |

Diagnostic residual model terbaik:

| Test | p-value | Kesimpulan |
|---|---:|---|
| Ljung-Box | 0.2216 | Tidak ada autokorelasi residual yang kuat |
| Shapiro-Wilk | 0.1061 | Residual cukup mendekati normal |
| ARCH LM | 0.9395 | Tidak ada indikasi ARCH yang kuat |
| ARCH F | 0.9511 | Tidak ada indikasi ARCH yang kuat |

## Interpretation

Harga ETH asli tidak stasioner berdasarkan uji ADF. Setelah transformasi log-difference, seri menjadi stasioner sehingga penggunaan ARIMA dengan differencing orde pertama (`d=1`) dapat digunakan.

Model ARIMA(2,1,2) menghasilkan error rata-rata sekitar 3.67 USD pada data test. Nilai MAPE terlihat kecil karena harga ETH berada di sekitar 3970 USD, sehingga error beberapa dolar hanya menjadi persentase yang kecil.

## Limitations

Data yang digunakan hanya mencakup sekitar 50 menit observasi, sehingga hasil forecast perlu dibaca sebagai eksperimen time series jangka pendek. Model ini belum cocok digunakan sebagai sistem prediksi harga real-time atau dasar keputusan trading.

Selain itu, performa ARIMA hanya sedikit lebih baik dibanding baseline naive forecast, sehingga generalisasi model masih terbatas.

## Files

```text
.
+-- ETH.xlsx
+-- ETH_ARIMA_Forecasting_Analysis.ipynb
+-- README.md
```

## How to Run

Jalankan notebook `ETH_ARIMA_Forecasting_Analysis.ipynb` menggunakan Jupyter Notebook, JupyterLab, VS Code, atau Google Colab.

Jika menjalankan secara lokal, pastikan dependencies berikut tersedia:

```bash
pip install pandas numpy matplotlib scipy scikit-learn statsmodels openpyxl
```

Pastikan `ETH.xlsx` berada di folder yang sama dengan notebook.
