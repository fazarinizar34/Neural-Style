# Neural-Style
 Tugas Generative Deep Learning: Neural Style Transfer (NST)

Proyek ini adalah implementasi dari teknik Neural Style Transfer (NST) menggunakan TensorFlow 2 dan Keras pada Google Colab.

Tujuan utama dari proyek ini adalah menggabungkan konten struktural dari gambar utama (Peacemaker.jpg) dengan gaya artistik (tekstur, warna, goresan kuas) dari gambar lain (neural style.jfif), menghasilkan karya seni baru yang unik.

 Cara Menjalankan Proyek di Google Colab

Proyek ini dirancang untuk berjalan optimal di Google Colab, memanfaatkan GPU untuk komputasi cepat.

1. Prasyarat

Akselerator GPU: Pastikan Anda mengaktifkan GPU pada Google Colab (Runtime > Change runtime type > Hardware accelerator: GPU).

Gambar Input: Pastikan dua file gambar berikut sudah ada di Google Drive Anda pada lokasi yang ditentukan:

Gambar Konten (Content Image): /content/drive/MyDrive/Colab Notebooks/Deep Learning/Peacemaker.jpg

Gambar Gaya (Style Image): /content/drive/MyDrive/Colab Notebooks/Deep Learning/neural style.jfif

2. Langkah-Langkah Eksekusi

Buka Notebook: Buka file Neural_Style_Transfer_Colab.ipynb di Google Colab.

Mount Google Drive: Jalankan sel kode pertama untuk me-mount Google Drive. Ini memungkinkan Notebook mengakses gambar input Anda.

from google.colab import drive
drive.mount('/content/drive')


Jalankan Sel Berurutan: Jalankan semua sel kode secara berurutan. Proses ini akan membutuhkan waktu beberapa menit tergantung pada GPU yang dialokasikan.

Cek Hasil: Gambar hasil kombinasi akan ditampilkan di bagian akhir Notebook setelah optimasi selesai.

 Detail Teknis Model

Arsitektur

Kami menggunakan model VGG19 yang sudah dilatih (pre-trained) pada ImageNet. Model ini berfungsi sebagai Feature Extractor yang memisahkan informasi konten (apa yang ada di gambar) dan informasi gaya (bagaimana gambar itu dilukis).

Fungsi Kerugian (Loss Function)

Inti dari NST adalah meminimalkan fungsi kerugian total yang terdiri dari dua komponen utama:

1. Content Loss ($\text{Loss}_{\text{Content}}$)

Tujuan: Memastikan gambar hasil mempertahankan struktur dan objek yang sama dengan gambar konten.

Implementasi: Mean Squared Error (MSE) antara fitur gambar hasil dan gambar konten, yang diekstrak dari lapisan-lapisan dalam model (misalnya, block5_conv2 VGG19).

2. Style Loss ($\text{Loss}_{\text{Style}}$)

Tujuan: Memastikan gambar hasil memiliki tekstur, warna, dan pola yang sama dengan gambar gaya.

Implementasi: MSE antara Gram Matrix dari gambar hasil dan Gram Matrix dari gambar gaya. Gram Matrix menghitung kovarians fitur di berbagai saluran dan digunakan untuk menangkap korelasi statistik fitur visual (gaya) di seluruh gambar. Lapisan yang digunakan tersebar di seluruh model (misalnya, block1_conv1 hingga block5_conv1).

Total Loss

Total Loss yang dioptimalkan adalah kombinasi berbobot dari kedua kerugian tersebut:

$$\text{Loss}_{\text{Total}} = \alpha \times \text{Loss}_{\text{Content}} + \beta \times \text{Loss}_{\text{Style}}$$

 Konfigurasi (Hyperparameters)

Variabel

Deskripsi

Nilai Default dalam Kode

ALPHA

Bobot untuk Content Loss

1e3 (Ditingkatkan untuk detail konten)

BETA

Bobot untuk Style Loss

1e6 (Ditingkatkan untuk efek artistik yang kuat)

IMG_HEIGHT, IMG_WIDTH

Ukuran gambar input/output

512

EPOCHS

Jumlah perulangan optimasi

10

STEPS_PER_EPOCH

Jumlah langkah gradient descent per Epoch

100

 Hasil Output

Gambar hasil akhir akan otomatis disimpan ke Google Drive Anda pada jalur:
/content/drive/MyDrive/Colab Notebooks/Deep Learning/Peacemaker_Styled.png
