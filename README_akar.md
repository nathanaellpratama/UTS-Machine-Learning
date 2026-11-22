# UTS Machine Learning: Regresi, Klasifikasi, dan Clustering

Kelompok 7, Telkom University. Repo ini berisi pipeline machine learning untuk tiga tugas pada tiga dataset tabular sintetis: estimasi durasi pengiriman (regresi), deteksi fraud (klasifikasi), dan segmentasi pelanggan (clustering).

## Anggota

| Nama | NIM | Akun GitHub | Peran |
|---|---|---|---|
| Prasta Arya Seta | 101032330051 | [kwa23](https://github.com/kwa23) | Menyusun `laporan.pdf` dan `kontribusi.md`, serta menggabungkan (merge) pull request ke `main` |
| Nathanael Liongky Pratama | 101032300014 | [nathanaellpratama](https://github.com/nathanaellpratama) | Mengerjakan seluruh kode dan keluaran prediksi: `kode/` (tiga notebook, `jalankan_semua.sh`, `requirements.txt`, `README.md`), `prediksi_regresi.csv`, `prediksi_klasifikasi.csv`, `cluster.csv` |

Rincian peran, link pull request, dan pernyataan penggunaan AI ada di [`kontribusi.md`](kontribusi.md).

## Isi repo

| Berkas | Isi |
|---|---|
| [`laporan.pdf`](laporan.pdf) | Laporan yang menjawab Tugas 1 sampai 3 |
| [`kode/`](kode/) | Seluruh kode (tiga notebook) dan [`kode/README.md`](kode/README.md) berisi satu perintah untuk mereproduksi semua angka |
| [`prediksi_regresi.csv`](prediksi_regresi.csv) | Tugas 1: `id_paket, p50, p90` (60.000 baris, `p90 >= p50`) |
| [`prediksi_klasifikasi.csv`](prediksi_klasifikasi.csv) | Tugas 2: `id_transaksi, prob, tandai` (106.674 baris) |
| [`cluster.csv`](cluster.csv) | Tugas 3: `id_pelanggan, cluster` (120.000 baris, `-1` untuk anomali) |
| [`kontribusi.md`](kontribusi.md) | Peran anggota, link PR, dan penggunaan AI |

## Ringkasan pendekatan

**Tugas 1, Regresi (P50 dan P90 durasi pengiriman).** Audit data menemukan fitur bocor (`jumlah_scan`), fitur proksi waktu (`f30`), kode sentinel pada jarak, dan nilai berat yang hilang tidak acak; dampaknya diukur lewat ablasi. Validasi memakai tiga lipatan berurutan waktu. Model akhir menggabungkan LightGBM kuantil dan LightGBM L2 pada log target; P90 dikalibrasi dengan metode konformal. Dibandingkan juga dengan baseline kuantil grup dan Ridge.

**Tugas 2, Klasifikasi (deteksi fraud dengan biaya).** Label belum lengkap pada sekitar tiga minggu terakhir, sehingga validasi memakai jeda 21 hari dan hanya label matang. Fitur `flag_review_manual` bocor dan dikeluarkan. Dibangun fitur riwayat dari masa lalu, empat pendekatan imbalance dibandingkan, dan pergeseran pola fraud sejak Agustus dianalisis. Probabilitas dikalibrasi dan keputusan menandai memakai aturan biaya: tandai bila probabilitas kali nominal melebihi Rp 25.000.

**Tugas 3, Clustering (segmentasi pelanggan).** Transformasi sesuai jenis variabel (log, logit, *centered log-ratio* untuk komposisi), seleksi fitur dengan bukti, perbandingan K-Means, GMM, dan Ward, pemilihan jumlah cluster dengan beberapa kriteria, stabilitas bootstrap, aturan anomali, serta profil dan aksi pemasaran per segmen (6 segmen).

## Cara mereproduksi

Letakkan data di folder `data/` (nama berkas ada di [`kode/README.md`](kode/README.md)), lalu dari akar repo:

```bash
pip install -r kode/requirements.txt
bash kode/jalankan_semua.sh
```

Perintah ini menjalankan ketiga notebook (sekitar 70 menit pada 2 inti CPU) dan menulis ulang tiga berkas prediksi di akar repo. Data tidak disertakan di repo karena ukurannya besar.
