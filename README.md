# Neural-Style
🎨 Tugas Generative Deep Learning: Neural Style Transfer (NST)

Proyek ini adalah implementasi dari teknik Neural Style Transfer (NST) menggunakan TensorFlow 2 dan Keras pada Google Colab. NST memungkinkan penggabungan konten struktural dari satu gambar dengan gaya artistik (tekstur, warna, goresan kuas) dari gambar lain.

File Utama: Neural_Style_Transfer_Colab.ipynb

🖼️ Demonstrasi Hasil

Berikut adalah contoh visual dari proses Style Transfer, menggunakan gambar input yang Anda sediakan:

Gambar Konten (Content)

Gambar Gaya (Style)

Gambar Hasil (Output)

Peacemaker.jpg

neural style.jfif

Peacemaker_Styled.png





🧠 Penjelasan Kode Inti (.ipynb)

Kode ini bekerja dengan prinsip optimasi untuk meminimalkan fungsi kerugian (Loss Function) komposit.

1. Model VGG19 sebagai Feature Extractor

Kami menggunakan arsitektur VGG19 yang telah dilatih (pre-trained) pada dataset ImageNet. Model ini berfungsi sebagai "mata" yang mampu membedakan:

Fitur Konten: Apa yang ada di gambar (objek, bentuk). Diekstrak dari lapisan yang lebih dalam (CONTENT_LAYERS = ['block5_conv2']).

Fitur Gaya: Bagaimana gambar itu dilukis (tekstur, warna). Diekstrak dari lapisan-lapisan yang berbeda kedalaman (STYLE_LAYERS = ['block1_conv1', ... 'block5_conv1']).

2. Fungsi Kerugian (Loss Function)

Inti dari NST adalah Total Loss, yang merupakan kombinasi berbobot dari dua komponen kerugian:

$$\text{Loss}_{\text{Total}} = \alpha \times \text{Loss}_{\text{Content}} + \beta \times \text{Loss}_{\text{Style}}$$

a. Content Loss

Tujuan: Memastikan struktur objek di gambar hasil tetap mirip dengan gambar konten.

Perhitungan: Menggunakan Mean Squared Error (MSE) antara fitur lapisan dalam gambar hasil dan gambar konten target. Bobotnya diatur oleh $\alpha$ (ALPHA = 1e3).

b. Style Loss

Tujuan: Memastikan tekstur, palet warna, dan pola gambar hasil mengikuti gambar gaya.

Perhitungan: Menggunakan Mean Squared Error (MSE) antara Gram Matrix dari gambar hasil dan Gram Matrix dari gambar gaya target.

Gram Matrix: Matriks ini dihitung dari aktivasi fitur dan secara efektif menangkap korelasi statistik antara berbagai fitur, yang merepresentasikan "gaya" atau tekstur.

Bobotnya diatur oleh $\beta$ (BETA = 1e6), yang jauh lebih tinggi untuk menekankan transfer gaya secara dominan.

3. Proses Optimasi

Gambar Output: Proses dimulai dengan menginisialisasi gambar output sebagai salinan dari gambar konten.

Optimizer: Menggunakan Adam Optimizer (walaupun paper asli menggunakan L-BFGS, Adam lebih mudah diimplementasikan dan efisien di TensorFlow) untuk secara iteratif menyesuaikan nilai piksel pada gambar output.

train_step: Setiap langkah optimasi menghitung gradien dari Total Loss terhadap piksel gambar output dan kemudian menerapkan gradien tersebut untuk memodifikasi gambar, sehingga Total Loss terus berkurang hingga gaya dan konten tercapai.

⚙️ Petunjuk Eksekusi dan Konfigurasi

1. Prasyarat

Akselerator GPU: Wajib diaktifkan di Colab.

Jalur Gambar: Pastikan gambar Anda berada di jalur berikut di Google Drive:

/content/drive/MyDrive/Colab Notebooks/Deep Learning/Peacemaker.jpg
/content/drive/MyDrive/Colab Notebooks/Deep Learning/neural style.jfif


2. Hyperparameters

Anda dapat menyesuaikan nilai ini di sel 2.1 Variabel Konfigurasi dalam Notebook:

Variabel

Deskripsi

Nilai Default

ALPHA

Bobot Content Loss

1e3

BETA

Bobot Style Loss

1e6

EPOCHS

Jumlah Epoch Optimasi

10

STEPS_PER_EPOCH

Jumlah langkah per Epoch

100

3. Simpan Hasil

Setelah proses selesai, gambar hasil akan disimpan secara otomatis ke jalur berikut di Drive Anda:
/content/drive/MyDrive/Colab Notebooks/Deep Learning/Peacemaker_Styled.png
