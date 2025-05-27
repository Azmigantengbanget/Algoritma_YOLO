<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Arduino Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Arduino Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [
            // Kosakata 1-308 (dari PDF pertama)disini yahhhhh
        // Set 1: 100 Pertanyaan Elektronika Dasar



  { "en": "Apa kepanjangan dari YOLO?", "id": "You Only Look Once." },
  { "en": "Apa fungsi utama algoritma YOLO?", "id": "Deteksi objek secara real-time." },
  { "en": "Bagaimana YOLO memproses sebuah gambar?", "id": "Melihat seluruh gambar sekaligus." },
  { "en": "Termasuk jenis detektor apakah YOLO?", "id": "Detektor satu tahap (single-stage)." },
  { "en": "Apa yang diprediksi YOLO untuk objek?", "id": "Kotak batas dan label kelas." },
  { "en": "Siapa yang pertama mengembangkan YOLO?", "id": "Joseph Redmon dan tim." },
  { "en": "Pada tahun berapa YOLO diperkenalkan?", "id": "Tahun 2016." },
  { "en": "Apa keunggulan utama YOLO?", "id": "Kecepatan deteksi sangat tinggi." },
  { "en": "Bagaimana YOLO membagi gambar untuk deteksi?", "id": "Menjadi beberapa sel grid." },
  { "en": "Apa tugas setiap sel grid YOLO?", "id": "Mendeteksi objek di area sel." },
  { "en": "Apa itu skor kepercayaan YOLO?", "id": "Keyakinan adanya objek dan akurasi kotak." },
  { "en": "Apakah YOLO menggunakan region proposal?", "id": "Tidak, YOLO tidak menggunakannya." },
  { "en": "Apa perbedaan YOLO dengan R-CNN?", "id": "YOLO lebih cepat dari R-CNN." },
  { "en": "Apakah YOLO bisa mendeteksi banyak objek?", "id": "Ya, mampu deteksi banyak objek." },
  { "en": "Bagaimana YOLO menangani objek tumpang tindih?", "id": "Menggunakan Non-Maximum Suppression (NMS)." },
  { "en": "Apakah YOLO belajar fitur objek umum?", "id": "Ya, belajar representasi umum objek." },
  { "en": "Bagaimana performa YOLO pada objek kecil?", "id": "Versi awal kurang baik." },
  { "en": "Apakah kode YOLO bersifat open source?", "id": "Ya, banyak versi open source." },
  { "en": "Framework apa yang awalnya digunakan YOLO?", "id": "Darknet." },
  { "en": "Apa itu bounding box dalam YOLO?", "id": "Kotak penanda lokasi objek terdeteksi." },
  { "en": "Parameter apa saja pada bounding box?", "id": "x, y, lebar, tinggi, kepercayaan." },
  { "en": "Apakah YOLO cocok untuk aplikasi video?", "id": "Ya, sangat cocok untuk video." },
  { "en": "Mengapa YOLO disebut 'unified detection'?", "id": "Memandang deteksi sebagai masalah regresi." },
  { "en": "Apa output dari jaringan YOLO?", "id": "Tensor berisi prediksi bounding box." },
  { "en": "Apa itu probabilitas kelas bersyarat?", "id": "Probabilitas objek kelas tertentu ada." },
  { "en": "Versi YOLO mana yang paling awal?", "id": "YOLOv1." },
  { "en": "Apa peningkatan utama di YOLOv2?", "id": "Lebih baik, cepat, dan kuat." },
  { "en": "Nama lain dari YOLOv2 adalah?", "id": "YOLO9000." },
  { "en": "Fitur apa yang diperkenalkan di YOLOv2?", "id": "Anchor boxes dan batch normalization." },
  { "en": "Berapa kelas bisa dideteksi YOLO9000?", "id": "Lebih dari 9000 kelas objek." },
  { "en": "Fokus utama pengembangan YOLOv3?", "id": "Akurasi lebih baik, deteksi objek kecil." },
  { "en": "Backbone network pada YOLOv3?", "id": "Darknet-53." },
  { "en": "Bagaimana YOLOv3 meningkatkan deteksi objek kecil?", "id": "Prediksi pada tiga skala berbeda." },
  { "en": "Siapa yang mengembangkan YOLOv4?", "id": "Alexey Bochkovskiy dan lainnya." },
  { "en": "Apa konsep 'Bag of Freebies' YOLOv4?", "id": "Teknik training tingkatkan akurasi gratis." },
  { "en": "Apa konsep 'Bag of Specials' YOLOv4?", "id": "Modul tingkatkan akurasi sedikit biaya." },
  { "en": "Siapa yang merilis YOLOv5?", "id": "Ultralytics." },
  { "en": "Framework apa yang digunakan YOLOv5?", "id": "PyTorch." },
  { "en": "Apa keunggulan utama YOLOv5?", "id": "Mudah digunakan dan performa bagus." },
  { "en": "Siapa pengembang utama YOLOv6?", "id": "Meituan." },
  { "en": "Apa fokus utama YOLOv6?", "id": "Efisiensi untuk aplikasi industri." },
  { "en": "Siapa yang mengembangkan YOLOv7?", "id": "WongKinYiu dan AlexeyAB." },
  { "en": "Inovasi apa pada YOLOv7?", "id": "Extended ELAN dan model scaling." },
  { "en": "Siapa merilis YOLOv8?", "id": "Ultralytics." },
  { "en": "Kemampuan apa saja yang dimiliki YOLOv8?", "id": "Deteksi, segmentasi, klasifikasi, pose." },
  { "en": "Apakah YOLOv8 menggunakan anchor boxes?", "id": "Tidak, YOLOv8 bersifat anchor-free." },
  { "en": "Apa itu YOLO-NAS?", "id": "YOLO dengan Neural Architecture Search." },
  { "en": "Apa keunggulan dari YOLO-NAS?", "id": "Arsitektur optimal hasil pencarian otomatis." },
  { "en": "Apa itu YOLO World?", "id": "Deteksi objek open-vocabulary canggih." },
  { "en": "Bagaimana YOLO World deteksi objek baru?", "id": "Menggunakan deskripsi teks sebagai input." },
  { "en": "Apa tiga bagian utama arsitektur YOLO?", "id": "Backbone, Neck, dan Head." },
  { "en": "Apa fungsi dari Backbone pada YOLO?", "id": "Ekstraksi fitur dari gambar input." },
  { "en": "Sebutkan contoh arsitektur Backbone YOLO?", "id": "Darknet, ResNet, CSPNet." },
  { "en": "Apa fungsi dari Neck pada YOLO?", "id": "Menggabungkan fitur dari berbagai level." },
  { "en": "Sebutkan contoh arsitektur Neck YOLO?", "id": "FPN (Feature Pyramid Network), PANet." },
  { "en": "Apa fungsi dari Head pada YOLO?", "id": "Melakukan prediksi bounding box dan kelas." },
  { "en": "Apa itu anchor boxes pada YOLO?", "id": "Kotak pra-definisi berbagai rasio aspek." },
  { "en": "Kapan anchor boxes diperkenalkan di YOLO?", "id": "Pada versi YOLOv2." },
  { "en": "Apa fungsi utama dari anchor boxes?", "id": "Membantu prediksi kotak lebih akurat." },
  { "en": "Apakah semua versi YOLO memakai anchor?", "id": "Tidak, beberapa versi bersifat anchor-free." },
  { "en": "Apa itu grid cell dalam YOLO?", "id": "Bagian dari gambar yang diproses." },
  { "en": "Apa itu convolutional layer (lapisan konvolusi)?", "id": "Lapisan utama untuk ekstraksi fitur." },
  { "en": "Apa fungsi batch normalization di YOLO?", "id": "Menstabilkan dan mempercepat proses training." },
  { "en": "Sebutkan contoh fungsi aktivasi di YOLO?", "id": "Leaky ReLU, Mish, SiLU." },
  { "en": "Apa itu Darknet dalam konteks YOLO?", "id": "Framework dan arsitektur backbone." },
  { "en": "Apa itu CSPNet (Cross Stage Partial Network)?", "id": "Arsitektur backbone efisien." },
  { "en": "Apa fungsi FPN (Feature Pyramid Network)?", "id": "Menggabungkan fitur multi-skala efektif." },
  { "en": "Apa fungsi PANet (Path Aggregation Network)?", "id": "Meningkatkan alur informasi antar fitur." },
  { "en": "Apa itu decoupled head dalam YOLO?", "id": "Memisahkan prediksi kotak dan kelas." },
  { "en": "Apa fungsi Spatial Pyramid Pooling (SPP)?", "id": "Menangani berbagai ukuran input gambar." },
  { "en": "Apa yang dibutuhkan untuk melatih YOLO?", "id": "Dataset gambar dengan anotasi objek." },
  { "en": "Apa itu anotasi objek dalam dataset?", "id": "Label kelas dan koordinat kotak." },
  { "en": "Format anotasi umum untuk YOLO?", "id": "File teks berisi (kelas x y w h)." },
  { "en": "Apa itu loss function dalam training?", "id": "Mengukur error prediksi model." },
  { "en": "Komponen loss function YOLO apa saja?", "id": "Lokalisasi, kepercayaan, dan klasifikasi." },
  { "en": "Apa itu localization loss?", "id": "Error prediksi lokasi dan ukuran kotak." },
  { "en": "Apa itu confidence loss (objectness)?", "id": "Error prediksi keberadaan suatu objek." },
  { "en": "Apa itu classification loss?", "id": "Error prediksi label kelas objek." },
  { "en": "Optimizer apa yang sering digunakan YOLO?", "id": "Adam atau SGD (Stochastic Gradient Descent)." },
  { "en": "Apa itu learning rate dalam training?", "id": "Ukuran langkah pembaruan bobot model." },
  { "en": "Apa yang dimaksud dengan epoch?", "id": "Satu siklus penuh training dataset." },
  { "en": "Apa itu batch size saat training?", "id": "Jumlah sampel per iterasi training." },
  { "en": "Apa itu data augmentation?", "id": "Memperbanyak data training secara artifisial." },
  { "en": "Sebutkan contoh teknik data augmentation?", "id": "Rotasi, flip, scaling, mosaic, mixup." },
  { "en": "Apa itu mosaic data augmentation?", "id": "Menggabungkan empat gambar jadi satu." },
  { "en": "Manfaat mosaic augmentation untuk apa?", "id": "Meningkatkan deteksi objek berukuran kecil." },
  { "en": "Apa itu transfer learning?", "id": "Menggunakan bobot model pra-terlatih." },
  { "en": "Mengapa transfer learning penting?", "id": "Hemat waktu, butuh data sedikit." },
  { "en": "Dataset apa sering dipakai pra-latih?", "id": "COCO, ImageNet, Pascal VOC." },
  { "en": "Apa itu fine-tuning model?", "id": "Melatih ulang model pra-latih sedikit." },
  { "en": "Kapan fine-tuning model diperlukan?", "id": "Saat dataset target sangat berbeda." },
  { "en": "Perangkat keras apa ideal untuk training?", "id": "GPU dengan memori besar." },
  { "en": "Apa itu overfitting dalam machine learning?", "id": "Model bagus di train, buruk di tes." },
  { "en": "Bagaimana cara mencegah overfitting?", "id": "Augmentasi, regularisasi, dropout, early stopping." },
  { "en": "Apa itu early stopping?", "id": "Hentikan training jika validasi memburuk." },
  { "en": "Apa itu multi-scale training?", "id": "Training dengan berbagai resolusi gambar." },
  { "en": "Metrik evaluasi utama deteksi objek?", "id": "mAP (mean Average Precision)." },
  { "en": "Apa itu IoU (Intersection over Union)?", "id": "Ukuran tumpang tindih dua kotak." },
  { "en": "Bagaimana IoU dihitung secara sederhana?", "id": "Area irisan dibagi area gabungan." },
  { "en": "Apa fungsi IoU threshold?", "id": "Menentukan apakah deteksi benar (TP)." },
  { "en": "Apa itu True Positive (TP)?", "id": "Deteksi benar objek yang ada." },
  { "en": "Apa itu False Positive (FP)?", "id": "Deteksi salah, objek tidak ada." },
  { "en": "Apa itu False Negative (FN)?", "id": "Gagal deteksi objek yang ada." },
  { "en": "Apa itu Precision dalam evaluasi?", "id": "Proporsi deteksi benar dari total deteksi." },
  { "en": "Rumus sederhana untuk Precision?", "id": "TP dibagi (TP + FP)." },
  { "en": "Apa itu Recall (Sensitivity)?", "id": "Proporsi objek terdeteksi dari total objek." },
  { "en": "Rumus sederhana untuk Recall?", "id": "TP dibagi (TP + FN)." },
  { "en": "Apa itu kurva Precision-Recall?", "id": "Grafik hubungan presisi dan recall." },
  { "en": "Apa itu Average Precision (AP)?", "id": "Rata-rata presisi di berbagai recall." },
  { "en": "Bagaimana mAP (mean AP) dihitung?", "id": "Rata-rata AP untuk semua kelas." },
  { "en": "Nilai mAP yang baik bagaimana?", "id": "Semakin tinggi nilainya semakin baik." },
  { "en": "Selain mAP, metrik apa lagi penting?", "id": "FPS (Frames Per Second) untuk kecepatan." },
  { "en": "Apa arti mAP@0.5?", "id": "mAP dengan IoU threshold 0.5." },
  { "en": "Apa arti mAP@[.5:.95] (COCO)?", "id": "Rata-rata mAP pada berbagai IoU." },
  { "en": "Apa itu confidence threshold saat inferensi?", "id": "Ambang batas skor kepercayaan minimum." },
  { "en": "Apa itu F1-Score?", "id": "Rata-rata harmonik presisi dan recall." },
  { "en": "Apa itu GFLOPS dalam model?", "id": "Ukuran kompleksitas komputasi model." },
  { "en": "Aplikasi YOLO pada mobil otonom?", "id": "Deteksi kendaraan, pejalan kaki, rambu." },
  { "en": "Aplikasi YOLO dalam bidang keamanan?", "id": "Pengawasan, deteksi penyusup, keamanan publik." },
  { "en": "Aplikasi YOLO di sektor ritel?", "id": "Analisis pelanggan, manajemen stok otomatis." },
  { "en": "Aplikasi YOLO di bidang pertanian?", "id": "Deteksi hama, pemantauan kondisi tanaman." },
  { "en": "Aplikasi YOLO di dunia medis?", "id": "Analisis citra medis, deteksi sel." },
  { "en": "Aplikasi YOLO untuk konservasi satwa liar?", "id": "Pelacakan hewan, anti-perburuan liar." },
  { "en": "Aplikasi YOLO dalam analisis olahraga?", "id": "Pelacakan pemain dan bola otomatis." },
  { "en": "Aplikasi YOLO di sektor manufaktur?", "id": "Kontrol kualitas, deteksi cacat produk." },
  { "en": "Aplikasi YOLO untuk drone (UAV)?", "id": "Pemetaan udara, pengawasan area luas." },
  { "en": "Bisakah YOLO untuk deteksi wajah?", "id": "Ya, bisa dilatih untuk itu." },
  { "en": "Bisakah YOLO untuk pembacaan plat nomor?", "id": "Ya, sebagai bagian sistem ANPR." },
  { "en": "YOLO untuk navigasi robot cerdas?", "id": "Membantu robot mengenali lingkungan sekitar." },
  { "en": "YOLO untuk aplikasi augmented reality (AR)?", "id": "Penempatan objek virtual dunia nyata." },
  { "en": "YOLO dalam sistem parkir pintar?", "id": "Deteksi ketersediaan slot parkir." },
  { "en": "YOLO untuk manajemen lalu lintas?", "id": "Pemantauan kepadatan, deteksi pelanggaran." },
  { "en": "YOLO pada perangkat edge computing?", "id": "Ya, versi ringan bisa dijalankan." },
  { "en": "Manfaat YOLO pada perangkat edge?", "id": "Proses lokal, latensi sangat rendah." },
  { "en": "Tantangan YOLO pada perangkat edge?", "id": "Keterbatasan sumber daya komputasi." },
  { "en": "YOLO untuk analisis video keamanan?", "id": "Keunggulan utama YOLO adalah itu." },
  { "en": "YOLO untuk deteksi objek di citra satelit?", "id": "Ya, dengan dataset yang sesuai." },
  { "en": "Industri apa yang banyak gunakan YOLO?", "id": "Otomotif, keamanan, ritel, manufaktur." },
  { "en": "YOLO untuk sistem presensi otomatis?", "id": "Deteksi wajah untuk pencatatan kehadiran." },
  { "en": "YOLO dalam pemantauan lingkungan hidup?", "id": "Deteksi polusi atau deforestasi." },
  { "en": "YOLO untuk interaksi manusia-komputer (HCI)?", "id": "Pengenalan gestur tangan pengguna." },
  { "en": "YOLO untuk sistem sortir otomatis?", "id": "Memilah objek di lini produksi." },
  { "en": "YOLO dalam inspeksi infrastruktur publik?", "id": "Deteksi retakan jembatan atau jalan." },
  { "en": "YOLO untuk pemantauan kerumunan massa?", "id": "Estimasi jumlah, deteksi perilaku abnormal." },
  { "en": "YOLO untuk deteksi emosi wajah?", "id": "Bisa, jika dilatih data relevan." },
  { "en": "YOLO untuk deteksi teks dalam gambar?", "id": "Bukan utama, tapi bisa dikombinasikan." },
  { "en": "Kelebihan utama algoritma YOLO?", "id": "Sangat cepat dalam melakukan deteksi." },
  { "en": "Kelebihan lain dari YOLO?", "id": "Memproses gambar secara global, konteks luas." },
  { "en": "Apakah YOLO termasuk algoritma akurat?", "id": "Versi baru memiliki akurasi tinggi." },
  { "en": "Apakah YOLO mudah untuk diimplementasikan?", "id": "Beberapa versi (YOLOv5/v8) sangat mudah." },
  { "en": "Kekurangan awal YOLO (misalnya YOLOv1)?", "id": "Kurang akurat pada objek kecil." },
  { "en": "Kekurangan lain dari YOLOv1?", "id": "Kesulitan dengan objek berkelompok rapat." },
  { "en": "Apakah YOLO boros sumber daya komputasi?", "id": "Relatif efisien dibanding detektor dua-tahap." },
  { "en": "Tantangan YOLO pada objek sangat kecil?", "id": "Masih menjadi area riset aktif." },
  { "en": "Tantangan objek aspek rasio ekstrem?", "id": "Perlu anchor atau desain khusus." },
  { "en": "Apakah YOLO solusi terbaik selalu?", "id": "Tergantung kasus penggunaan spesifiknya." },
  { "en": "Kapan YOLO menjadi pilihan unggul?", "id": "Saat kecepatan adalah prioritas utama." },
  { "en": "Kapan metode lain mungkin lebih baik?", "id": "Jika akurasi tertinggi mutlak diperlukan." },
  { "en": "Bagaimana trade-off kecepatan dan akurasi?", "id": "Umumnya, model cepat kurang akurat." },
  { "en": "Apakah YOLO butuh banyak data training?", "id": "Ya, seperti model deep learning." },
  { "en": "Kesulitan YOLO dengan objek baru (unseen)?", "id": "Performa menurun tanpa fine-tuning." },
  { "en": "Masalah localization error pada YOLO?", "id": "Bisa terjadi, terutama versi awal." },
  { "en": "Apakah YOLO bisa salah klasifikasi?", "id": "Ya, tidak ada model sempurna." },
  { "en": "Ketergantungan pada kualitas data training?", "id": "Sangat bergantung pada kualitas data." },
  { "en": "Apakah YOLO sensitif terhadap variasi pencahayaan?", "id": "Augmentasi data bisa membantu." },
  { "en": "Apakah YOLO sulit untuk di-debug?", "id": "Bisa jadi, seperti model DL." },
  { "en": "Kompleksitas implementasi custom YOLO sendiri?", "id": "Membutuhkan pemahaman mendalam." },
  { "en": "Keterbatasan YOLO pada dataset kecil?", "Rentan overfitting, transfer learning membantu." },
  { "en": "Apakah YOLO menghasilkan banyak false positive?", "Tergantung threshold dan kualitas training." },
  { "en": "Bagaimana mengatasi kekurangan-kekurangan YOLO?", "Versi baru, teknik training, post-processing." },
  { "en": "Apakah biaya komputasi training tinggi?", "Ya, GPU bertenaga biasanya diperlukan." },
  { "en": "Apakah hasil YOLO selalu konsisten?", "Dipengaruhi banyak faktor, termasuk inisialisasi." },
  { "en": "Tantangan etika penggunaan teknologi YOLO?", "Privasi, bias dalam data training." },
  { "en": "Apakah YOLO bisa deteksi objek teroklusi?", "Kemampuan terbatas, tergantung tingkat oklusi." },
  { "en": "Performa YOLO di kondisi cuaca buruk?", "Bisa menurun, perlu data beragam." },
  { "en": "Apa itu Non-Maximum Suppression (NMS)?", "id": "Menghilangkan kotak batas prediksi redundan." },
  { "en": "Bagaimana cara kerja NMS secara umum?", "Pilih skor tertinggi, hapus tumpang tindih." },
  { "en": "Parameter penting dalam NMS?", "IoU threshold dan score threshold." },
  { "en": "Apa itu soft-NMS?", "Mengurangi skor kotak tumpang tindih." },
  { "en": "Apa itu Bag of Freebies (BoF)?", "id": "Teknik training tanpa biaya inferensi." },
  { "en": "Sebutkan contoh teknik BoF?", "Augmentasi data (CutMix, Mosaic), DropBlock." },
  { "en": "Apa itu Bag of Specials (BoS)?", "id": "Modul tingkatkan akurasi, sedikit biaya." },
  { "en": "Sebutkan contoh modul BoS?", "Mish activation, SPP, SAM, PAN." },
  { "en": "Apa itu label assignment dalam training?", "Menentukan grid/anchor mana bertanggung jawab." },
  { "en": "Sebutkan teknik label assignment canggih?", "SimOTA, TAL (TaskAlignedAssigner)." },
  { "en": "Apa itu Generalized IoU (GIoU) Loss?", "Peningkatan IoU loss, atasi non-overlap." },
  { "en": "Apa itu Distance IoU (DIoU) Loss?", "Mempertimbangkan jarak pusat antar kotak." },
  { "en": "Apa itu Complete IoU (CIoU) Loss?", "Mempertimbangkan aspek rasio kotak juga." },
  { "en": "Apa itu Focus layer pada YOLOv5?", "Mengurangi komputasi, mempertahankan informasi penting." },
  { "en": "Apa itu SiLU (Swish) activation function?", "Fungsi aktivasi halus, non-monotonik." },
  { "en": "Apa itu Mish activation function?", "Fungsi aktivasi canggih mirip SiLU." },
  { "en": "Apa itu RepVGG block?", "Konversi arsitektur training ke inferensi." },
  { "en": "Apa itu quantization model?", "Mengurangi presisi bobot model AI." },
  { "en": "Manfaat utama dari quantization model?", "Model lebih kecil, inferensi lebih cepat." },
  { "en": "Apa itu pruning model?", "Menghilangkan bobot/koneksi tidak penting." },
  { "en": "Manfaat utama dari pruning model?", "Model lebih ringan dan lebih efisien." },
  { "en": "Apa itu knowledge distillation?", "Melatih model kecil dari model besar." },
  { "en": "Apa itu anchor-free detection?", "Deteksi tanpa anchor boxes pra-definisi." },
  { "en": "Keuntungan detektor anchor-free?", "Lebih sederhana, generalisasi sering lebih baik." },
  { "en": "Contoh model YOLO anchor-free?", "YOLOX, YOLOv8, beberapa varian lain." },
  { "en": "Apakah YOLO menggunakan arsitektur Transformer?", "Beberapa varian baru mulai mengadopsi." },
  { "en": "Apa itu Efficient Layer Aggregation Network (ELAN)?", "Blok arsitektur efisien di YOLOv7." },
  { "en": "Apa itu model scaling pada YOLO?", "Menyesuaikan kedalaman/lebar model sistematis." },
  { "en": "Apa itu reparameterization dalam model?", "Mengubah struktur blok saat inferensi." },
  { "en": "Apa itu NVIDIA TensorRT?", "Library optimasi inferensi deep learning." },
  { "en": "Bagaimana TensorRT mempercepat inferensi YOLO?", "Optimasi layer, presisi rendah, kernel fusion." },
  { "en": "Apa itu format ONNX?", "Format pertukaran model antar framework." },
  { "en": "Bisakah model YOLO diekspor ke ONNX?", "Ya, banyak versi mendukung ekspor ONNX." },
  { "en": "Apa itu self-supervised learning untuk YOLO?", "Training tanpa label manusia secara ekstensif." },
  { "en": "Apa itu zero-shot detection?", "Deteksi kelas tak terlihat saat training." },
  { "en": "Apakah YOLO World mendukung zero-shot detection?", "Ya, melalui deskripsi teks input." },
  { "en": "Dataset populer untuk deteksi objek?", "COCO, Pascal VOC, OpenImages, ImageNet." },
  { "en": "Apa itu dataset COCO?", "Common Objects in Context dataset." },
  { "en": "Berapa jumlah kelas di dataset COCO?", "80 kategori objek umum." },
  { "en": "Apa itu dataset Pascal VOC?", "Visual Object Classes challenge dataset." },
  { "en": "Berapa jumlah kelas di Pascal VOC?", "20 kategori objek." },
  { "en": "Format label anotasi COCO?", "JSON (JavaScript Object Notation)." },
  { "en": "Format label anotasi Pascal VOC?", "XML (Extensible Markup Language)." },
  { "en": "Sebutkan alat anotasi objek populer?", "LabelImg, CVAT, Roboflow, VGG VIA." },
  { "en": "Platform untuk mengelola dataset AI?", "Roboflow, V7 Labs, Dataloop." },
  { "en": "Framework Darknet ditulis dalam bahasa apa?", "C dan CUDA." },
  { "en": "Kelebihan utama dari framework Darknet?", "Cepat, fleksibel untuk riset YOLO." },
  { "en": "Kekurangan dari framework Darknet?", "Kurang user-friendly dibanding PyTorch/TensorFlow." },
  { "en": "Implementasi YOLO di PyTorch populer?", "Ultralytics YOLOv5, YOLOv8, dan lainnya." },
  { "en": "Kelebihan implementasi YOLO di PyTorch?", "Komunitas besar, mudah digunakan, fleksibel." },
  { "en": "Implementasi YOLO di TensorFlow/Keras populer?", "Beberapa repositori GitHub tersedia." },
  { "en": "Bisakah YOLO dijalankan hanya dengan CPU?", "Ya, tapi inferensi akan lambat." },
  { "en": "Apakah GPU diperlukan untuk inferensi YOLO?", "Sangat dianjurkan untuk performa real-time." },
  { "en": "Jenis GPU apa cocok untuk YOLO?", "NVIDIA GPU dengan dukungan CUDA." },
  { "en": "Apa itu Google Colab?", "Platform Jupyter notebook gratis dengan GPU." },
  { "en": "Bisakah YOLO dilatih di Google Colab?", "Ya, cocok untuk eksperimen awal." },
  { "en": "Apa itu Docker untuk deployment YOLO?", "Memudahkan deployment lingkungan konsisten." },
  { "en": "Perintah instalasi YOLOv5 (Ultralytics)?", "pip install yolov5." },
  { "en": "Perintah instalasi YOLOv8 (Ultralytics)?", "pip install ultralytics." },
  { "en": "Contoh perintah inferensi YOLOv8 CLI?", "yolo predict model=yolov8n.pt source=img.jpg" },
  { "en": "Bisakah YOLO digunakan dengan library OpenCV?", "Ya, OpenCV bisa memuat model YOLO." },
  { "en": "Format bobot model YOLO (Darknet)?", ".weights." },
  { "en": "Format bobot model YOLO (PyTorch)?", ".pt atau .pth." },
  { "en": "Apa itu file konfigurasi .cfg (Darknet)?", "Mendefinisikan arsitektur jaringan model." },
  { "en": "Apa itu file .yaml di YOLOv5/v8?", "Konfigurasi dataset, model, dan training." },
  { "en": "Bagaimana memvisualisasikan hasil deteksi YOLO?", "Gambar kotak batas pada gambar asli." },
  { "en": "Library Python untuk visualisasi deteksi?", "OpenCV, Matplotlib, Pillow." },
  { "en": "Apakah ada GUI untuk YOLO?", "Beberapa proyek komunitas menyediakan GUI." },
  { "en": "Bisakah YOLO diintegrasikan dengan webcam?", "Ya, sangat umum dilakukan pengguna." },
  { "en": "Format video input untuk YOLO?", "Format umum seperti MP4, AVI, MOV." },
  { "en": "Bagaimana menyimpan hasil deteksi video?", "Simpan sebagai video baru beranotasi." },
  { "en": "Tantangan implementasi YOLO di perangkat mobile?", "Ukuran model, konsumsi daya baterai." },
  { "en": "Framework untuk YOLO di mobile?", "TensorFlow Lite, PyTorch Mobile, CoreML." },
  { "en": "Apakah ada model YOLO pra-latih mobile?", "Ya, versi ringan sering tersedia." },
  { "en": "Pentingnya memilih ukuran input gambar?", "Mempengaruhi akurasi dan kecepatan inferensi." },
  { "en": "Ukuran input gambar umum YOLO?", "416x416, 640x640, 1280x1280 piksel." },
  { "en": "Bagaimana menangani gambar ukuran berbeda?", "Resize atau padding ke ukuran tetap." },
  { "en": "YOLO vs Faster R-CNN: Mana lebih cepat?", "YOLO jauh lebih cepat." },
  { "en": "YOLO vs Faster R-CNN: Mana lebih akurat (awal)?", "Faster R-CNN (awalnya lebih akurat)." },
  { "en": "YOLO vs SSD: Perbedaan utama apa?", "Cara prediksi kotak dan skala fitur." },
  { "en": "Apakah SSD juga detektor satu-tahap?", "Ya, SSD detektor satu-tahap." },
  { "en": "YOLO vs RetinaNet: Keunggulan RetinaNet?", "Mengatasi class imbalance dengan Focal Loss." },
  { "en": "YOLO vs EfficientDet: Fokus EfficientDet?", "Skalabilitas model yang sangat efisien." },
  { "en": "YOLO vs DETR (Transformer): Perbedaan utama?", "DETR gunakan transformer, tanpa NMS." },
  { "en": "Apakah YOLO masih relevan saat ini?", "Ya, sangat relevan terus berkembang." },
  { "en": "Bagaimana YOLO dibandingkan model segmentasi?", "YOLO deteksi, segmentasi lebih detail." },
  { "en": "Bisakah YOLO dimodifikasi untuk segmentasi?", "Ya, ada varian seperti YOLACT." },
  { "en": "Apa itu instance segmentation?", "Deteksi dan segmentasi setiap instans." },
  { "en": "YOLO untuk estimasi pose manusia?", "Ya, YOLOv8 mendukung estimasi pose." },
  { "en": "Apa itu estimasi pose (pose estimation)?", "Mendeteksi keypoints tubuh manusia/objek." },
  { "en": "Performa YOLO pada dataset benchmark?", "Versi baru sering capai state-of-the-art." },
  { "en": "Apa itu kompetisi COCO Detection?", "Ajang bergengsi uji model deteksi." },
  { "en": "Apakah YOLO sering menang kompetisi?", "Varian YOLO sering berkinerja sangat baik." },
  { "en": "Seberapa besar komunitas pengguna YOLO?", "Sangat besar, aktif, dan suportif." },
  { "en": "Di mana mencari bantuan terkait YOLO?", "GitHub issues, forum, Stack Overflow." },
  { "en": "Kontribusi utama Joseph Redmon pada AI?", "Pencipta YOLO dan framework Darknet." },
  { "en": "Mengapa Redmon berhenti riset computer vision?", "Alasan etika terkait penggunaan militer." },
  { "en": "Siapa yang melanjutkan pengembangan YOLO?", "Banyak peneliti dan berbagai organisasi." },
  { "en": "Peran Ultralytics dalam ekosistem YOLO?", "Mengembangkan dan mempopulerkan YOLOv5, YOLOv8." },
  { "en": "Peran Meituan dalam ekosistem YOLO?", "Mengembangkan seri YOLOv6 untuk industri." },
  { "en": "Dampak YOLO pada bidang computer vision?", "Revolusi deteksi objek secara real-time." },
  { "en": "Apakah YOLO digunakan di industri besar?", "Ya, diadopsi luas berbagai industri." },
  { "en": "Bagaimana lisensi software untuk YOLO?", "Bervariasi, banyak open source (GPL, Apache)." },
  { "en": "Apakah ada kursus online belajar YOLO?", "Ya, banyak tersedia di berbagai platform." },
  { "en": "Apakah YOLO sulit dipelajari pemula?", "Konsep dasar mudah, implementasi butuh usaha." },
  { "en": "Bagaimana masa depan algoritma YOLO?", "Lebih akurat, cepat, efisien, fitur baru." },
  { "en": "Apa pendekatan YOLOv1 untuk deteksi objek?", "Regresi tunggal untuk kotak dan kelas." },
  { "en": "Berapa grid cell digunakan YOLOv1?", "7x7 grid cells." },
  { "en": "Berapa bounding box per sel YOLOv1?", "Dua bounding boxes." },
  { "en": "Backbone YOLOv1 terinspirasi dari arsitektur apa?", "GoogLeNet." },
  { "en": "Apakah YOLOv1 menggunakan anchor boxes?", "Tidak, YOLOv1 tidak pakai anchor." },
  { "en": "Apa itu Fast YOLO (varian YOLOv1)?", "Versi lebih kecil, lebih cepat." },
  { "en": "Teknik apa saja baru di YOLOv2?", "Batch Norm, High Res, Anchor Boxes." },
  { "en": "Apa itu Dimension Clusters di YOLOv2?", "K-means pada anchor box priors." },
  { "en": "Apa itu Direct location prediction YOLOv2?", "Prediksi offset relatif ke grid cell." },
  { "en": "Backbone YOLOv2 disebut apa?", "Darknet-19." },
  { "en": "Bagaimana YOLO9000 deteksi ribuan kelas?", "Gabung dataset deteksi dan klasifikasi." },
  { "en": "Apa itu WordTree di YOLO9000?", "Struktur hirarki untuk label kelas." },
  { "en": "Backbone YOLOv3 menggunakan berapa lapisan?", "53 lapisan konvolusi (Darknet-53)." },
  { "en": "YOLOv3 prediksi pada berapa skala?", "Tiga skala berbeda (multi-scale)." },
  { "en": "Loss function klasifikasi di YOLOv3?", "Binary cross-entropy per kelas independen." },
  { "en": "Varian kecil YOLOv3 yang populer?", "YOLOv3-tiny." },
  { "en": "Filosofi utama di balik YOLOv4?", "Kecepatan dan akurasi optimal (real-time)." },
  { "en": "Backbone utama yang digunakan YOLOv4?", "CSPDarknet53." },
  { "en": "Neck yang digunakan pada YOLOv4?", "SPP (Spatial Pyramid Pooling), PAN (Path Aggregation)." },
  { "en": "Contoh Bag of Freebies di YOLOv4?", "CutMix, Mosaic, DropBlock, Label Smoothing." },
  { "en": "Contoh Bag of Specials di YOLOv4?", "Mish, CSP, SAM, DiCAN, FPN+PAN." },
  { "en": "Keunggulan utama framework YOLOv5?", "Mudah digunakan, training cepat, dokumentasi." },
  { "en": "Arsitektur backbone YOLOv5 berbasis apa?", "Modifikasi CSPNet dan Focus Layer." },
  { "en": "Neck yang digunakan pada YOLOv5?", "PANet (Path Aggregation Network)." },
  { "en": "Sebutkan varian model YOLOv5?", "n (nano), s (small), m (medium), l (large), x (xlarge)." },
  { "en": "Lisensi software yang digunakan YOLOv5?", "GNU General Public License v3.0." },
  { "en": "Fokus utama pengembangan YOLOv6?", "Efisiensi untuk aplikasi industri (deployment)." },
  { "en": "Desain backbone YOLOv6 menggunakan blok apa?", "EfficientRep (berbasis blok RepVGG)." },
  { "en": "Desain neck YOLOv6 disebut apa?", "RepBiFPAN (Reparameterized Bi-directional FPN)." },
  { "en": "Apakah YOLOv6 menggunakan decoupled head?", "Ya, versi awal menggunakannya." },
  { "en": "Lisensi software yang digunakan YOLOv6?", "Lisensi permisif (misalnya Apache 2.0)." },
  { "en": "Inovasi arsitektur utama pada YOLOv7?", "E-ELAN (Extended Efficient Layer Aggregation Network)." },
  { "en": "Bagaimana YOLOv7 melakukan model scaling?", "Menyesuaikan model untuk berbagai ukuran." },
  { "en": "Apa itu reparameterized convolution di YOLOv7?", "Teknik optimasi untuk efisiensi inferensi." },
  { "en": "Apakah YOLOv7 menggunakan auxiliary head?", "Ya, untuk membantu proses training." },
  { "en": "Fokus YOLOv7 selain akurasi/kecepatan?", "Efisiensi proses training model." },
  { "en": "Apakah YOLOv8 bersifat anchor-free?", "Ya, YOLOv8 adalah anchor-free." },
  { "en": "Backbone YOLOv8 menggunakan modul apa?", "Modifikasi CSPDarknet, dengan modul C2f." },
  { "en": "Head yang digunakan pada YOLOv8?", "Decoupled head dan anchor-free design." },
  { "en": "Tugas apa saja didukung YOLOv8?", "Deteksi, segmentasi, klasifikasi, estimasi pose." },
  { "en": "Loss function YOLOv8 melibatkan apa?", "TaskAlignedAssigner, CIoU loss, DFL." },
  { "en": "Apa kepanjangan YOLO-NAS?", "YOLO Neural Architecture Search." },
  { "en": "Siapa yang mengembangkan model YOLO-NAS?", "Deci AI." },
  { "en": "Apakah YOLO-NAS quantization-aware?", "Ya, dirancang untuk proses quantization." },
  { "en": "Mesin NAS apa digunakan YOLO-NAS?", "AutoNAC (milik Deci AI)." },
  { "en": "Blok baru pada arsitektur YOLO-NAS?", "Quantization-friendly basic block inovatif." },
  { "en": "Kemampuan utama dari model YOLO-World?", "Deteksi objek open-vocabulary (kosakata terbuka)." },
  { "en": "Siapa pengembang model YOLO-World?", "Tencent AI Lab." },
  { "en": "Bagaimana cara kerja YOLO-World deteksi?", "Belajar asosiasi visual dan linguistik." },
  { "en": "Text encoder apa digunakan YOLO-World?", "Pre-trained CLIP text encoder." },
  { "en": "Vision backbone YOLO-World berbasis apa?", "RepVL-PAN (berbasis arsitektur YOLOv8)." },
  { "en": "Apa itu grid sensitivity problem?", "Masalah objek di batas grid cell." },
  { "en": "Apa itu focal loss function?", "Mengatasi class imbalance pada objek sulit." },
  { "en": "Apa itu hard negative mining?", "Fokus training pada sampel negatif sulit." },
  { "en": "Masalah umum: CUDA out of memory?", "Kurangi batch size atau resolusi gambar." },
  { "en": "Masalah umum: mAP sangat rendah?", "Periksa data, augmentasi, atau hyperparameter." },
  { "en": "Pentingnya normalisasi data input YOLO?", "Ya, membantu konvergensi proses training." },
  { "en": "Apa itu vanishing gradient problem?", "Gradien kecil, training lambat/stagnan." },
  { "en": "Apa itu exploding gradient problem?", "Gradien besar, training tidak stabil." },
  { "en": "Apa itu dropout regularization?", "Teknik regularisasi, nonaktifkan neuron acak." },
  { "en": "Apa itu label smoothing regularization?", "Mencegah model terlalu percaya diri (overconfident)." },
  { "en": "Pentingnya membersihkan dataset sebelum training?", "Sangat penting untuk hasil model baik." },
  { "en": "Apa itu model checkpointing saat training?", "Menyimpan bobot model secara berkala." },
  { "en": "Alat visualisasi metrik training YOLO?", "TensorBoard atau Weights & Biases (W&B)." },
  { "en": "Kenapa validasi mAP bisa nol?", "Masalah anotasi, konfigurasi, atau training." },
  { "en": "Apa itu reproducibility dalam deep learning?", "Kemampuan menghasilkan ulang hasil eksperimen." },
  { "en": "Pengaruh random seed dalam training?", "Mengontrol inisialisasi acak untuk reproducibility." },
  { "en": "Apa itu data leakage?", "Informasi data tes bocor ke training." },
  { "en": "Bagaimana memilih IoU threshold NMS?", "Eksperimen, tergantung kepadatan objek target." },
  { "en": "Bagaimana memilih confidence threshold inferensi?", "Trade-off antara presisi dan recall." },
  { "en": "Apa itu few-shot object detection?", "Deteksi dengan sedikit sampel data." },
  { "en": "Etika: Bias dalam dataset YOLO?", "Bisa menyebabkan performa tidak adil." },
  { "en": "Etika: Penggunaan YOLO untuk pengawasan?", "Menimbulkan kekhawatiran terkait privasi individu." },
  { "en": "Bagaimana mengurangi bias dalam model YOLO?", "Dataset beragam, teknik fairness-aware AI." },
  { "en": "Tren YOLO masa depan bagaimana?", "Lebih efisien, multi-task, self-supervised." },
  { "en": "YOLO dan Neural Architecture Search (NAS)?", "Tren untuk arsitektur lebih optimal." },
  { "en": "YOLO dan arsitektur Transformer?", "Integrasi fitur Transformer mungkin meningkat." },
  { "en": "YOLO dan self-supervised learning (SSL)?", "Mengurangi ketergantungan pada label manual." },
  { "en": "YOLO untuk video dengan konsistensi temporal?", "Area riset aktif dan berkembang." },
  { "en": "YOLO pada perangkat keras khusus (ASIC)?", "Potensi untuk efisiensi daya tinggi." },
  { "en": "YOLO dengan Explainable AI (XAI)?", "Penting untuk memahami keputusan model." },
  { "en": "YOLO untuk deteksi objek 3D?", "Varian YOLO untuk data 3D (LIDAR)." },
  { "en": "YOLO dan continual learning (CL)?", "Model belajar terus menerus tanpa lupa." },
  { "en": "YOLO dengan on-device learning?", "Model belajar langsung di perangkat pengguna." },
  { "en": "YOLO yang lebih hemat energi (green AI)?", "Penting untuk perangkat mobile dan edge." },
  { "en": "Kombinasi YOLO dengan sensor lain?", "Sensor fusion (LIDAR, radar) lebih robust." },
  { "en": "YOLO untuk domain adaptation?", "Model bekerja baik di domain berbeda." },
  { "en": "Tantangan terbesar YOLO ke depan?", "Generalisasi, robustnes, efisiensi, etika." },
  { "en": "YOLO dan open-world detection?", "Deteksi objek tak dikenal (seperti YOLO-World)." },
  { "en": "Apakah akan ada versi YOLOvNext?", "Pengembangan akan terus berlanjut pesat." },
  { "en": "Peran komunitas dalam evolusi YOLO?", "Sangat besar, mendorong inovasi berkelanjutan." },
  { "en": "YOLO adalah algoritma untuk tugas apa?", "Deteksi objek dalam gambar/video." },
  { "en": "Arti dari 'You Only Look Once'?", "Proses gambar dalam satu kali jalan." },
  { "en": "YOLO membagi gambar input menjadi?", "Kumpulan sel grid (grid cells)." },
  { "en": "Output YOLO untuk setiap objek terdeteksi?", "Kotak batas (bounding box) dan kelas." },
  { "en": "Metrik evaluasi performa utama YOLO?", "mAP (mean Average Precision) dan FPS." },
  { "en": "Fungsi dari Non-Maximum Suppression (NMS)?", "Hapus prediksi kotak yang tumpang tindih." },
  { "en": "Bagian Backbone pada arsitektur YOLO berfungsi?", "Ekstraksi fitur-fitur penting dari gambar." },
  { "en": "Bagian Neck pada arsitektur YOLO berfungsi?", "Agregasi fitur dari berbagai level." },
  { "en": "Bagian Head pada arsitektur YOLO berfungsi?", "Melakukan prediksi akhir (kotak, kelas)." },
  { "en": "Anchor boxes membantu prediksi objek apa?", "Objek dengan berbagai ukuran/rasio aspek." },
  { "en": "Tujuan utama dari data augmentation?", "Tingkatkan variasi data set training." },
  { "en": "Manfaat utama dari transfer learning?", "Percepat training, butuh data lebih sedikit." },
  { "en": "COCO adalah contoh dari apa?", "Dataset standar untuk deteksi objek." },
  { "en": "Darknet awalnya adalah apa untuk YOLO?", "Framework neural network asli YOLO." },
  { "en": "PyTorch adalah apa dalam konteks YOLO?", "Framework deep learning populer untuk YOLO." },
  { "en": "FPS (Frames Per Second) mengukur apa?", "Kecepatan inferensi atau deteksi model." },
  { "en": "IoU adalah singkatan dari apa?", "Intersection over Union (area tumpang tindih)." },
  { "en": "Loss function dalam training mengukur apa?", "Seberapa besar kesalahan prediksi model." },
  { "en": "Siapa pengembang utama seri YOLOv8?", "Perusahaan Ultralytics." },
  { "en": "YOLO-World dirancang untuk jenis deteksi apa?", "Deteksi objek open-vocabulary (kosakata terbuka)." },
  { "en": "Apakah YOLO bisa untuk image segmentation?", "Ya, varian tertentu mendukung segmentasi." },
  { "en": "Apakah YOLO bisa untuk pose estimation?", "Ya, varian tertentu mendukung estimasi pose." },
  { "en": "Arti dari model 'anchor-free'?", "Tidak menggunakan anchor boxes pra-definisi." },
  { "en": "Apa itu deteksi objek real-time?", "Deteksi sangat cepat, seolah instan." },
  { "en": "YOLO pertama kali dikenalkan tahun berapa?", "Tahun 2016 oleh Joseph Redmon." },
  { "en": "Apakah YOLO selalu membutuhkan hardware GPU?", "Tidak selalu, tapi GPU sangat percepat." },
  { "en": "Apa itu bounding box regression?", "Prediksi koordinat (x,y,w,h) kotak batas." },
  { "en": "Apa itu class prediction dalam YOLO?", "Prediksi label kategori objek terdeteksi." },
  { "en": "Apakah pengembangan YOLO masih aktif?", "Ya, sangat aktif dengan versi baru." },
  { "en": "Apa fungsi activation Leaky ReLU?", "Memasukkan non-linearitas, hindari dying ReLU." },
  { "en": "Apa itu 'bottleneck' dalam arsitektur CNN?", "Lapisan yang mengurangi jumlah fitur." },
  { "en": "Apa itu 'upsampling' dalam YOLO neck?", "Meningkatkan resolusi peta fitur." },
  { "en": "Apa itu 'strides' dalam convolutional layer?", "Langkah pergeseran filter konvolusi." },
  { "en": "Apa itu 'padding' dalam convolutional layer?", "Menambahkan piksel di sekitar batas gambar." },
  { "en": "Manfaat 'padding=same' pada konvolusi?", "Menjaga ukuran output sama dengan input." },
  { "en": "Apa itu 'feature map' dalam CNN?", "Representasi fitur hasil dari konvolusi." },
  { "en": "Bagaimana YOLOv1 prediksi multiple objects per cell?", "Prediksi B boxes, pilih IoU tertinggi." },
  { "en": "Apa batasan 'spatial constraint' YOLOv1?", "Satu objek per grid cell maksimal." },
  { "en": "Apa itu 'passthrough layer' di YOLOv2?", "Membawa fitur resolusi tinggi ke akhir." },
  { "en": "Teknik 'joint training' YOLO9000 untuk apa?", "Deteksi dan klasifikasi ribuan kelas." },
  { "en": "Apa itu 'logistic regression' untuk objectness?", "Prediksi skor keberadaan objek per kotak." },
  { "en": "YOLOv3 menggunakan berapa anchor per skala?", "Tiga anchor box per skala deteksi." },
  { "en": "Apa itu 'residual block' di Darknet-53?", "Memungkinkan training jaringan sangat dalam." },
  { "en": "Bagaimana YOLOv4 optimasi training?", "Gunakan banyak teknik BoF dan BoS." },
  { "en": "Apa itu 'CSP' (Cross Stage Partial) connection?", "Mengurangi komputasi, menjaga akurasi model." },
  { "en": "Fitur auto-anchor di YOLOv5 untuk apa?", "Secara otomatis optimasi anchor boxes." },
  { "en": "Apa itu 'quantization-aware training' (QAT)?", "Training model untuk optimasi kuantisasi." },
  { "en": "YOLOv6 fokus pada 'hardware-friendly design' artinya?", "Desain efisien untuk berbagai perangkat keras." },
  { "en": "Teknik 'model re-parameterization' untuk apa?", "Ubah arsitektur saat inferensi jadi cepat." },
  { "en": "Apa itu 'label assignment' dinamis?", "Penentuan tanggung jawab prediksi secara adaptif." },
  { "en": "Apa itu 'auxiliary losses' dalam training?", "Loss tambahan bantu optimasi fitur utama." },
  { "en": "YOLOv8 menggunakan 'C2f module', apa itu?", "Modul CSP dengan dua alur fitur." },
  { "en": "Apa itu 'Distribution Focal Loss' (DFL)?", "Loss untuk regresi distribusi kotak halus." },
  { "en": "Apa itu 'prompt engineering' di YOLO-World?", "Membuat deskripsi teks input efektif." },
  { "en": "Bagaimana YOLO tangani variasi skala objek?", "Multi-scale features, FPN, anchor (jika ada)." },
  { "en": "Apakah YOLO bisa deteksi objek transparan?", "Sangat sulit, tergantung kualitas data." },
  { "en": "Apakah YOLO bisa deteksi objek bergerak cepat?", "Tergantung FPS kamera dan model." },
  { "en": "Bagaimana meningkatkan FPS YOLO pada CPU?", "Gunakan model ringan, kuantisasi, optimasi." },
  { "en": "Apa itu 'model zoo' untuk YOLO?", "Koleksi model pra-latih siap pakai." },
  { "en": "Bisakah YOLO dilatih untuk satu kelas saja?", "Ya, sangat bisa dan umum." },
  { "en": "Apa itu 'validation set' dalam training?", "Dataset untuk evaluasi model selama training." },
  { "en": "Apa itu 'test set' dalam machine learning?", "Dataset final untuk evaluasi performa." },
  { "en": "Bisakah YOLO salah mendeteksi objek (halusinasi)?", "Ya, bisa menghasilkan false positive." },
  { "en": "Pentingnya resolusi gambar input untuk YOLO?", "Resolusi tinggi bisa deteksi objek kecil." },
  { "en": "Apakah YOLO bisa dijalankan di Raspberry Pi?", "Ya, model ringan dengan optimasi." },
  { "en": "Apa itu 'edge TPU' untuk akselerasi AI?", "ASIC Google untuk inferensi edge AI." },
  { "en": "Bisakah YOLO digabung dengan pelacakan objek?", "Ya, sering dikombinasi dengan DeepSORT." },
  { "en": "Apa itu 'Kalman Filter' dalam pelacakan?", "Memprediksi posisi objek masa depan." },
  { "en": "Apa itu 'Hungarian algorithm' dalam pelacakan?", "Mencocokkan deteksi dengan track yang ada." },
  { "en": "Apakah YOLO bisa untuk estimasi kedalaman?", "Tidak secara langsung, butuh input stereo." },
  { "en": "Apa itu 'data-centric AI'?", "Fokus pada kualitas dan kuantitas data." },
  { "en": "Apa itu 'model-centric AI'?", "Fokus pada pengembangan arsitektur model." },
  { "en": "Apakah YOLO termasuk pendekatan model-centric?", "Awalnya ya, kini data juga penting." },
  { "en": "Apakah augmentasi offline lebih baik dari online?", "Tergantung kasus, online lebih fleksibel." },
  { "en": "Apa itu 'synthetic data' untuk training?", "Data buatan komputer untuk augmentasi." },
  { "en": "Bisakah YOLO deteksi objek dari sketsa?", "Jika dilatih dengan dataset sketsa." },
  { "en": "Apa itu 'active learning' untuk anotasi?", "Model bantu pilih data paling informatif." },
  { "en": "Apakah ada batasan jumlah kelas YOLO?", "Praktis tidak, tapi performa turun." },
  { "en": "Apa itu 'attention mechanism' di CNN?", "Fokus pada bagian penting dari fitur." },
  { "en": "Apakah YOLO menggunakan attention mechanism?", "Beberapa varian baru mulai menggunakan." },
  { "en": "Bagaimana YOLO mengurangi false negatives?", "Threshold kepercayaan, augmentasi, model lebih baik." },
  { "en": "Apa itu 'model compression'?", "Teknik mengurangi ukuran model AI." },
  { "en": "Contoh teknik model compression?", "Pruning, quantization, knowledge distillation." },
  { "en": "Apakah YOLO bisa untuk deteksi anomali?", "Bisa, jika anomali visualnya jelas." },
  { "en": "Apa itu 'adversarial attack' pada YOLO?", "Input dirancang menipu model YOLO." },
  { "en": "Bagaimana bertahan dari adversarial attack?", "Adversarial training, input sanitization." },
  { "en": "Apa itu 'federated learning'?", "Training model terdistribusi tanpa bagi data." },
  { "en": "Bisakah YOLO dilatih dengan federated learning?", "Secara teoritis bisa, implementasi kompleks." },
  { "en": "Apa itu 'differential privacy' dalam AI?", "Menjaga privasi data individu saat training." },
  { "en": "Apakah YOLO versi terbaru selalu lebih baik?", "Umumnya ya, tapi ada trade-off." },
  { "en": "Bagaimana memilih versi YOLO yang tepat?", "Sesuaikan kebutuhan kecepatan, akurasi, kemudahan." },
  { "en": "YOLO untuk 'human-in-the-loop' anotasi?", "Bisa percepat proses anotasi data." },
  { "en": "Apa itu 'global context' yang dilihat YOLO?", "Informasi dari seluruh gambar, bukan patch." },
  { "en": "Apakah YOLO bisa untuk deteksi suara?", "Tidak, YOLO untuk data visual." },
  { "en": "Bagaimana pengaruh blur pada deteksi YOLO?", "Bisa menurunkan akurasi deteksi objek." },
  { "en": "Bagaimana pengaruh noise pada deteksi YOLO?", "Juga bisa menurunkan akurasi deteksi." },
  { "en": "Pentingnya dataset yang seimbang (balanced)?", "Mencegah bias model ke kelas mayoritas." },
  { "en": "Teknik mengatasi dataset tidak seimbang?", "Oversampling, undersampling, focal loss, bobot kelas." },
  { "en": "Apa itu 'heatmap' prediksi YOLO?", "Visualisasi area fokus deteksi model." },
  { "en": "Bisakah YOLO dijalankan di browser (webassembly)?", "Ya, dengan TensorFlow.js atau ONNX.js." },
  { "en": "Apa itu 'pipeline' deteksi objek?", "Serangkaian langkah dari input ke output." },
  { "en": "YOLO sebagai bagian dari pipeline apa?", "Sistem cerdas, robotika, analisis video." },
  { "en": "Apa itu 'inference server' untuk YOLO?", "Server untuk melayani permintaan deteksi." },
  { "en": "Contoh inference server untuk model AI?", "NVIDIA Triton, TensorFlow Serving, TorchServe." },
  { "en": "Apakah hasil prediksi YOLO deterministik?", "Dengan seed tetap, bisa deterministik." },
  { "en": "Apa itu 'benchmark' untuk model AI?", "Dataset dan metrik standar untuk perbandingan." },
  { "en": "Seberapa sering versi YOLO baru rilis?", "Cukup sering, bidang ini berkembang pesat." }





        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 5000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
