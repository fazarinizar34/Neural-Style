# Neural-Style
🎨 Neural Style Transfer (NST) dengan VGG19 dan TensorFlow

Proyek ini adalah implementasi dari Neural Style Transfer (NST) menggunakan arsitektur Convolutional Neural Network (CNN) VGG19 yang dimuat melalui TensorFlow dan Keras. NST bertujuan untuk menggabungkan konten dari satu gambar dengan gaya artistik dari gambar lain.

🚀 Demonstrasi Hasil

Berikut adalah contoh visual dari proses Style Transfer yang dihasilkan oleh model ini, menggabungkan konten dari Peacemaker.jpg dengan gaya dari neural style.jfif.

Gambar Konten (Content)

Gambar Gaya (Style)

Gambar Hasil (Output)





🧠 Analisis dan Penjelasan Kode

Kode utama proyek ini, yang terdapat dalam Neural_Style_Transfer_Colab.ipynb, berfokus pada teknik optimasi untuk meminimalkan fungsi kerugian komposit, yang merupakan kunci keberhasilan proses Style Transfer.

1. Model Feature Extractor (VGG19)

Tujuan: Menggunakan VGG19 (pre-trained di ImageNet) untuk mengekstrak representasi hirarkis dari gambar. Lapisan yang berbeda menangkap informasi yang berbeda:

Lapisan Dalam (Deep Layers): Menangkap Konten (bentuk objek, struktur).

Lapisan Dangkal (Shallow Layers): Menangkap Gaya (tekstur, warna, pola).

Implementasi: Model StyleContentModel dibuat dari VGG19, mengembalikan output dari lapisan yang telah ditentukan untuk Style dan Content.

2. Fungsi Kerugian (Loss Function)

Proses optimasi bertujuan meminimalkan Total Loss ($\text{Loss}_{\text{Total}}$):

$$\text{Loss}_{\text{Total}} = \alpha \times \text{Loss}_{\text{Content}} + \beta \times \text{Loss}_{\text{Style}}$$

a. Content Loss ($\text{Loss}_{\text{Content}}$)

Lapisan yang Dipilih: CONTENT_LAYERS = ['block5_conv2']

Fungsi: Menghitung Mean Squared Error (MSE) antara aktivasi fitur dari gambar hasil dan gambar konten target. Ini memastikan bahwa struktur objek utama gambar output dipertahankan.

b. Style Loss ($\text{Loss}_{\text{Style}}$)

Lapisan yang Dipilih: STYLE_LAYERS dari block1_conv1 hingga block5_conv1.

Perhitungan: Menggunakan MSE antara Gram Matrix dari gambar hasil dan Gram Matrix dari gambar gaya target.

Gram Matrix: Matriks Gram ($G$) dihitung sebagai perkalian matriks dari fitur vektor ($F$) dengan transposenya ($F^T$). Ini secara efektif menangkap korelasi spasial antar saluran fitur, yang merepresentasikan tekstur dan gaya secara abstrak, terlepas dari lokasi spasialnya.

$G_{ij} = \sum_k F_{ik} F_{jk}$

Normalisasi: Loss gaya dibagi dengan jumlah lapisan gaya untuk normalisasi.

3. Optimasi (Gradient Descent)

Inisialisasi: Gambar yang akan dioptimasi (image_to_optimize) diinisialisasi sebagai salinan gambar konten.

Optimizer: Menggunakan tf.optimizers.Adam dengan learning rate 0.02.

@tf.function(): Fungsi train_step di-dekorasi dengan @tf.function untuk kompilasi graph TensorFlow, yang meningkatkan kecepatan eksekusi.

Langkah Pelatihan: Pada setiap langkah, Gradient Tape menghitung gradien dari Total Loss terhadap piksel gambar output. Gradien ini kemudian diterapkan oleh Adam Optimizer untuk memodifikasi piksel gambar output.

⚙️ Petunjuk Eksekusi

Proyek ini paling mudah dijalankan menggunakan Google Colab.

Prasyarat

Akselerator GPU: Aktifkan GPU pada Notebook Colab Anda.

File Gambar: Pastikan gambar input Anda tersimpan di Google Drive dengan jalur yang sesuai:

Konten: /content/drive/MyDrive/Colab Notebooks/Deep Learning/Peacemaker.jpg

Gaya: /content/drive/MyDrive/Colab Notebooks/Deep Learning/neural style.jfif

Langkah Eksekusi

Buka Neural_Style_Transfer_Colab.ipynb di Google Colab.

Jalankan sel Mount Google Drive untuk menghubungkan Drive Anda.

Jalankan semua sel secara berurutan.

Gambar hasil akhir akan disimpan di: /content/drive/MyDrive/Colab Notebooks/Deep Learning/Peacemaker_Styled.png.

Konfigurasi Hyperparameters

Nilai-nilai ini menentukan hasil akhir. Anda dapat bereksperimen dengan mengubah rasio $\alpha/\beta$.

Variabel

Deskripsi

Nilai Default

ALPHA

Bobot untuk Content Loss ($\alpha$)

1e3

BETA

Bobot untuk Style Loss ($\beta$)

1e6

EPOCHS

Jumlah Epoch Optimasi

10

STEPS_PER_EPOCH

Jumlah langkah gradient descent per Epoch

100
