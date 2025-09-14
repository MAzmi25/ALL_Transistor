<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
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
        <h1>Pengetahuan Umum Dasar</h1>
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


  { "en": "Apa Itu Transistor?", "id": "Komponen Semikonduktor Untuk Saklar Penguat." },
  { "en": "Siapa Penemu Transistor?", "id": "Bardeen, Brattain, Dan Shockley." },
  { "en": "Di Mana Transistor Ditemukan?", "id": "Di Bell Labs." },
  { "en": "Apa Dua Fungsi Utama Transistor?", "id": "Sebagai Saklar Dan Penguat." },
  { "en": "Apa Bahan Dasar Transistor?", "id": "Silikon (Si) Atau Germanium (Ge)." },
  { "en": "Apa Dua Keluarga Utama Transistor?", "id": "BJT (Bipolar Junction Transistor) Dan FET (Field-Effect Transistor)." },
  { "en": "Apa Kepanjangan 'BJT'?", "id": "Bipolar Junction Transistor." },
  { "en": "Apa Kepanjangan 'FET'?", "id": "Field-Effect Transistor." },
  { "en": "Apa Prinsip Kontrol 'BJT (Bipolar Junction Transistor)'?", "id": "Dikontrol Oleh Arus." },
  { "en": "Apa Prinsip Kontrol 'FET (Field-Effect Transistor)'?", "id": "Dikontrol Oleh Tegangan." },
  { "en": "Apa Tiga Terminal 'BJT (Bipolar Junction Transistor)'?", "id": "Emitor, Basis, Dan Kolektor." },
  { "en": "Apa Tiga Terminal 'FET (Field-Effect Transistor)'?", "id": "Source, Gate, Dan Drain." },
  { "en": "Apa Itu 'Semikonduktor'?", "id": "Material Antara Konduktor Dan Isolator." },
  { "en": "Apa Itu 'Doping'?", "id": "Proses Menambahkan Impuritas Ke Semikonduktor." },
  { "en": "Apa Itu 'Semikonduktor Tipe-N'?", "id": "Semikonduktor Dengan Kelebihan Elektron." },
  { "en": "Apa Itu 'Semikonduktor Tipe-P'?", "id": "Semikonduktor Dengan Kelebihan Lubang (Hole)." },
  { "en": "Apa Itu 'Sambungan P-N'?", "id": "Batas Antara Material Tipe-P Tipe-N." },
  { "en": "Apa Dua Jenis 'BJT (Bipolar Junction Transistor)'?", "id": "NPN Dan PNP." },
  { "en": "Apa Arti 'NPN'?", "id": "Lapisan P Di Antara Dua Lapisan N." },
  { "en": "Apa Arti 'PNP'?", "id": "Lapisan N Di Antara Dua Lapisan P." },
  { "en": "Terminal Mana Yang Mengontrol 'BJT (Bipolar Junction Transistor)'?", "id": "Terminal Basis." },
  { "en": "Terminal Mana Yang Mengontrol 'FET (Field-Effect Transistor)'?", "id": "Terminal Gerbang (Gate)." },
  { "en": "Apa Itu 'Daerah Cut-off'?", "id": "Kondisi Transistor Mati Total." },
  { "en": "Apa Itu 'Daerah Saturasi'?", "id": "Kondisi Transistor Nyala Penuh." },
  { "en": "Apa Itu 'Daerah Aktif'?", "id": "Daerah Kerja Untuk Penguatan Sinyal." },
  { "en": "Apa Fungsi Transistor Sebagai 'Saklar'?", "id": "Beroperasi Di Daerah Cut-off Saturasi." },
  { "en": "Apa Fungsi Transistor Sebagai 'Penguat'?", "id": "Beroperasi Di Daerah Aktif." },
  { "en": "Apa Itu 'Gain Arus'?", "id": "Rasio Arus Output Terhadap Input." },
  { "en": "Apa Simbol Untuk 'Gain Arus BJT'?", "id": "Beta (β) Atau hFE." },
  { "en": "Apa Itu 'Bias Transistor'?", "id": "Memberi Tegangan DC (Direct Current) Untuk Operasi." },
  { "en": "Apa Kepanjangan 'MOSFET'?", "id": "Metal-Oxide-Semiconductor Field-Effect Transistor." },
  { "en": "Apa Ciri Khas 'MOSFET (Metal-Oxide-Semiconductor FET)'?", "id": "Memiliki Gerbang (Gate) Yang Terisolasi." },
  { "en": "Apa Bahan Isolator Gerbang 'MOSFET'?", "id": "Silikon Dioksida (SiO₂)." },
  { "en": "Apa Kepanjangan 'JFET'?", "id": "Junction Field-Effect Transistor." },
  { "en": "Apa Itu 'Kanal (Channel)'?", "id": "Daerah Tempat Arus Mengalir." },
  { "en": "Apa Dua Jenis Kanal 'FET (Field-Effect Transistor)'?", "id": "Kanal-N Dan Kanal-P." },
  { "en": "Apa Itu 'Impedansi Input'?", "id": "Resistansi Yang Dilihat Sinyal Input." },
  { "en": "Bagaimana 'Impedansi Input FET'?", "id": "Sangat Tinggi." },
  { "en": "Bagaimana 'Impedansi Input BJT'?", "id": "Relatif Rendah." },
  { "en": "Apa Itu 'Transistor Diskrit'?", "id": "Transistor Dalam Paket Individual." },
  { "en": "Apa Itu 'Transistor SMD (Surface-Mount Device)'?", "id": "Transistor Untuk Pemasangan Di Permukaan." },
  { "en": "Apa Itu 'Arus Bocor (Leakage)'?", "id": "Arus Kecil Saat Transistor Mati." },
  { "en": "Apa Itu 'Disipasi Daya'?", "id": "Energi Yang Diubah Transistor Menjadi Panas." },
  { "en": "Apa Itu 'Heatsink'?", "id": "Komponen Pendingin Untuk Transistor." },
  { "en": "Mengapa 'Heatsink' Dibutuhkan?", "id": "Untuk Mencegah Transistor Terlalu Panas." },
  { "en": "Apa Itu 'Transistor Sinyal Kecil'?", "id": "Transistor Untuk Aplikasi Daya Rendah." },
  { "en": "Apa Itu 'Transistor Daya'?", "id": "Transistor Untuk Aplikasi Daya Tinggi." },
  { "en": "Apa Itu 'Transkonduktansi'?", "id": "Ukuran Gain Pada FET." },
  { "en": "Apa Simbol 'Transkonduktansi'?", "id": "gₘ." },
  { "en": "Apa Itu 'Datasheet'?", "id": "Dokumen Spesifikasi Teknis Transistor." },
  { "en": "Apa Itu 'Arus Kolektor Maksimum (Ic max)'?", "id": "Arus Maksimum Yang Boleh Lewat Kolektor." },
  { "en": "Apa Itu 'Tegangan Tembus (Breakdown Voltage)'?", "id": "Tegangan Maksimum Antar Terminal." },
  { "en": "Apa Itu 'Keluarga Logika'?", "id": "Grup Sirkuit Digital Berbasis Teknologi Sama." },
  { "en": "Apa Kepanjangan 'TTL (Transistor-Transistor Logic)'?", "id": "Logika Transistor-Transistor." },
  { "en": "Apa Kepanjangan 'CMOS (Complementary MOS)'?", "id": "MOS (Metal-Oxide-Semiconductor) Komplementer." },
  { "en": "Apa Itu 'Logika CMOS'?", "id": "Logika Menggunakan Pasangan NMOS PMOS." },
  { "en": "Apa Keuntungan Logika 'CMOS'?", "id": "Konsumsi Daya Sangat Rendah." },
  { "en": "Apa Itu 'Efek Medan'?", "id": "Pengaruh Medan Listrik Pada Konduktivitas." },
  { "en": "Apa Itu 'Pembawa Muatan Mayoritas'?", "id": "Pembawa Muatan Paling Banyak." },
  { "en": "Apa Itu 'Pembawa Muatan Minoritas'?", "id": "Pembawa Muatan Paling Sedikit." },
  { "en": "Apakah 'BJT (Bipolar Junction Transistor)' Perangkat Bipolar?", "id": "Ya, Menggunakan Elektron Dan Lubang." },
  { "en": "Apakah 'FET (Field-Effect Transistor)' Perangkat Unipolar?", "id": "Ya, Gunakan Satu Jenis Pembawa." },
  { "en": "Apa Itu 'Daerah Deplesi'?", "id": "Daerah Tanpa Pembawa Muatan Bebas." },
  { "en": "Apa Itu 'Tegangan Threshold'?", "id": "Tegangan Minimum Menyalakan MOSFET." },
  { "en": "Apa Itu 'MOSFET Tipe Peningkatan'?", "id": "Normalnya Mati, Butuh Tegangan Untuk Nyala." },
  { "en": "Apa Itu 'MOSFET Tipe Deplesi'?", "id": "Normalnya Nyala, Butuh Tegangan Untuk Mati." },
  { "en": "Apa Itu 'Kapasitansi Parasit'?", "id": "Kapasitansi Tak Diinginkan Dalam Transistor." },
  { "en": "Apa Itu 'Waktu Tunda Propagasi'?", "id": "Waktu Sinyal Melewati Transistor." },
  { "en": "Apa Itu 'Rise Time'?", "id": "Waktu Sinyal Naik Dari 10% Ke 90%." },
  { "en": "Apa Itu 'Fall Time'?", "id": "Waktu Sinyal Turun Dari 90% Ke 10%." },
  { "en": "Apa Itu 'Switching Speed'?", "id": "Kecepatan Transistor Beralih On/Off." },
  { "en": "Apa Itu 'Safe Operating Area (SOA)'?", "id": "Batas Kondisi Operasi Aman." },
  { "en": "Apa Itu 'Thermal Runaway'?", "id": "Kondisi Panas Berlebih Tak Terkendali." },
  { "en": "Apa Itu 'Konfigurasi Common Emitter'?", "id": "Konfigurasi Penguat BJT (Bipolar Junction Transistor) Umum." },
  { "en": "Apa Itu 'Konfigurasi Common Collector'?", "id": "Konfigurasi BJT (Bipolar Junction Transistor) Sebagai Buffer." },
  { "en": "Apa Itu 'Konfigurasi Common Base'?", "id": "Konfigurasi BJT (Bipolar Junction Transistor) Frekuensi Tinggi." },
  { "en": "Apa Itu 'Konfigurasi Common Source'?", "id": "Konfigurasi Penguat FET (Field-Effect Transistor) Umum." },
  { "en": "Apa Itu 'Konfigurasi Common Drain'?", "id": "Konfigurasi FET (Field-Effect Transistor) Sebagai Buffer." },
  { "en": "Apa Itu 'Konfigurasi Common Gate'?", "id": "Konfigurasi FET (Field-Effect Transistor) Frekuensi Tinggi." },
  { "en": "Apa Itu 'Umpan Balik (Feedback)'?", "id": "Mengembalikan Sebagian Sinyal Output Ke Input." },
  { "en": "Apa Itu 'Osilator'?", "id": "Sirkuit Penguat Dengan Umpan Balik Positif." },
  { "en": "Apa Itu 'H-Bridge'?", "id": "Sirkuit Empat Transistor Pengontrol Motor." },
  { "en": "Apa Itu 'Push-Pull Amplifier'?", "id": "Penguat Menggunakan Pasangan Transistor." },
  { "en": "Apa Itu 'Kelas Penguat (Amplifier Class)'?", "id": "Klasifikasi Berdasarkan Cara Kerja Penguat." },
  { "en": "Contoh 'Kelas Penguat'?", "id": "Kelas A, B, AB, C, D." },
  { "en": "Apa Itu 'Penguat Diferensial'?", "id": "Menguatkan Selisih Dua Sinyal Input." },
  { "en": "Apa Itu 'Cermin Arus (Current Mirror)'?", "id": "Sirkuit Untuk Menyalin Arus." },
  { "en": "Apa Itu 'Beban Aktif (Active Load)'?", "id": "Menggunakan Transistor Sebagai Beban." },
  { "en": "Apa Itu 'Inverter CMOS'?", "id": "Gerbang NOT Dibuat Dari PMOS NMOS." },
  { "en": "Apa Itu 'Gerbang Transmisi (Transmission Gate)'?", "id": "Saklar Analog Dibuat Dari CMOS." },
  { "en": "Apa Itu 'Point-Contact Transistor'?", "id": "Jenis Transistor Fungsional Pertama." },
  { "en": "Apa Itu 'IGBT (Insulated-Gate Bipolar Transistor)'?", "id": "Perangkat Hibrida MOSFET Dan BJT." },
  { "en": "Apa Keuntungan 'IGBT (Insulated-Gate Bipolar Transistor)'?", "id": "Kontrol Tegangan, Kemampuan Arus Tinggi." },
  { "en": "Di Mana 'IGBT' Umum Digunakan?", "id": "Aplikasi Elektronika Daya Tinggi." },
  { "en": "Apa Itu 'Phototransistor'?", "id": "Transistor Yang Dikontrol Oleh Cahaya." },
  { "en": "Apa Itu 'Paket TO-92'?", "id": "Paket Plastik Umum Transistor Kecil." },
  { "en": "Apa Itu 'Paket TO-220'?", "id": "Paket Umum Untuk Transistor Daya." },
  { "en": "Apa Itu 'Arus Basis (Ib)'?", "id": "Arus Yang Mengalir Ke Terminal Basis." },
  { "en": "Apa Itu 'Arus Kolektor (Ic)'?", "id": "Arus Yang Mengalir Melalui Kolektor." },
  { "en": "Apa Itu 'Arus Emitor (Ie)'?", "id": "Arus Yang Keluar Dari Terminal Emitor." },
  { "en": "Apa Hubungan Arus 'BJT (Bipolar Junction Transistor)'?", "id": "Ie = Ib + Ic." },
  { "en": "Apa Itu 'Arus Gerbang (Ig)'?", "id": "Arus Yang Mengalir Ke Terminal Gerbang." },
  { "en": "Berapa 'Arus Gerbang (Ig) MOSFET'?", "id": "Hampir Nol Karena Terisolasi." },
  { "en": "Apa Itu 'Arus Drain (Id)'?", "id": "Arus Yang Mengalir Melalui Drain." },
  { "en": "Apa Itu 'Tegangan Vbe'?", "id": "Tegangan Antara Basis Dan Emitor." },
  { "en": "Berapa 'Tegangan Vbe' Tipikal Silikon?", "id": "Sekitar 0.7 Volt." },
  { "en": "Apa Itu 'Tegangan Vce'?", "id": "Tegangan Antara Kolektor Dan Emitor." },
  { "en": "Apa Itu 'Tegangan Vgs'?", "id": "Tegangan Antara Gerbang Dan Source." },
  { "en": "Apa Itu 'Tegangan Vds'?", "id": "Tegangan Antara Drain Dan Source." },
  { "en": "Apa Itu 'Darlington Pair'?", "id": "Dua BJT (Bipolar Junction Transistor) Untuk Gain Super." },
  { "en": "Siapa Penemu 'Darlington Pair'?", "id": "Sidney Darlington." },
  { "en": "Apa Kerugian 'Darlington Pair'?", "id": "Tegangan Saturasi Lebih Tinggi." },
  { "en": "Apa Itu 'Pasangan Sziklai'?", "id": "Pasangan Komplementer (NPN-PNP) Gain Tinggi." },
  { "en": "Apa Itu 'Efek Early'?", "id": "Variasi Lebar Basis Akibat Tegangan." },
  { "en": "Apa Itu 'HBT (Heterojunction Bipolar Transistor)'?", "id": "BJT (Bipolar Junction Transistor) Berkinerja Tinggi." },
  { "en": "Apa Bahan 'HBT (Heterojunction Bipolar Transistor)'?", "id": "Menggunakan Semikonduktor Berbeda (Misal SiGe)." },
  { "en": "Di Mana 'HBT' Digunakan?", "id": "Aplikasi RF (Radio Frequency) Sangat Cepat." },
  { "en": "Apa Kepanjangan 'HEMT (High-Electron-Mobility Transistor)'?", "id": "Transistor Mobilitas Elektron Tinggi." },
  { "en": "Apa Keuntungan 'HEMT'?", "id": "Derau (Noise) Sangat Rendah." },
  { "en": "Di Mana 'HEMT' Digunakan?", "id": "Penerima Satelit Dan Radar." },
  { "en": "Apa Itu 'FinFET'?", "id": "MOSFET (Metal-Oxide-Semiconductor FET) Dengan Struktur Sirip 3D." },
  { "en": "Mengapa 'FinFET' Diciptakan?", "id": "Untuk Kontrol Gerbang Yang Lebih Baik." },
  { "en": "Di Mana 'FinFET' Digunakan?", "id": "Prosesor Dan Chip Modern." },
  { "en": "Apa Itu 'Thyristor'?", "id": "Keluarga Perangkat Saklar Semikonduktor." },
  { "en": "Apa Kepanjangan 'SCR (Silicon Controlled Rectifier)'?", "id": "Penyearah Terkendali Silikon." },
  { "en": "Apa Fungsi 'SCR (Silicon Controlled Rectifier)'?", "id": "Saklar Daya Tinggi Untuk DC (Direct Current)." },
  { "en": "Apa Kepanjangan 'TRIAC'?", "id": "Triode for Alternating Current." },
  { "en": "Apa Fungsi 'TRIAC'?", "id": "Saklar Daya Tinggi Untuk AC (Alternating Current)." },
  { "en": "Apa Itu 'UJT (Unijunction Transistor)'?", "id": "Transistor Saklar Dengan Satu Sambungan." },
  { "en": "Di Mana 'UJT' Digunakan?", "id": "Sirkuit Osilator Dan Pemicu." },
  { "en": "Apa Itu 'Karakteristik Input'?", "id": "Grafik Arus Input Terhadap Tegangan Input." },
  { "en": "Apa Itu 'Karakteristik Output'?", "id": "Grafik Arus Output Terhadap Tegangan Output." },
  { "en": "Apa Itu 'Garis Beban (Load Line)'?", "id": "Garis Yang Merepresentasikan Beban." },
  { "en": "Apa Itu 'Titik Q (Quiescent Point)'?", "id": "Titik Operasi DC (Direct Current) Transistor." },
  { "en": "Apa Itu 'Kapasitor Bypass'?", "id": "Kapasitor Untuk Melewatkan Sinyal AC (Alternating Current)." },
  { "en": "Apa Itu 'Kapasitor Coupling'?", "id": "Kapasitor Untuk Menghubungkan Tahap Penguat." },
  { "en": "Apa Fungsi 'Kapasitor Coupling'?", "id": "Memblokir DC (Direct Current), Melewatkan AC (Alternating Current)." },
  { "en": "Apa Itu 'Distorsi Crossover'?", "id": "Cacat Sinyal Pada Penguat Push-Pull." },
  { "en": "Apa Itu 'Kelas A Amplifier'?", "id": "Transistor Aktif Selama 360 Derajat." },
  { "en": "Apa Itu 'Kelas B Amplifier'?", "id": "Transistor Aktif Selama 180 Derajat." },
  { "en": "Apa Itu 'Kelas AB Amplifier'?", "id": "Kompromi Antara Kelas A Dan B." },
  { "en": "Apa Itu 'Kelas D Amplifier'?", "id": "Penguat Switching Berbasis PWM." },
  { "en": "Apa Keuntungan 'Kelas D Amplifier'?", "id": "Efisiensi Sangat Tinggi." },
  { "en": "Apa Itu 'Matching Impedansi'?", "id": "Menyesuaikan Impedansi Untuk Transfer Daya." },
  { "en": "Apa Itu 'Aktivasi Gerbang'?", "id": "Proses Menyalakan Transistor." },
  { "en": "Apa Itu 'Miller Effect'?", "id": "Peningkatan Kapasitansi Akibat Gain." },
  { "en": "Apa Itu 'Frekuensi Transisi (fT)'?", "id": "Frekuensi Saat Gain Arus Menjadi Satu." },
  { "en": "Apa Itu 'S-parameters'?", "id": "Parameter Untuk Karakterisasi RF (Radio Frequency)." },
  { "en": "Apa Itu 'Noise Figure (NF)'?", "id": "Ukuran Derau (Noise) Yang Ditambahkan." },
  { "en": "Apa Itu 'Power Gain'?", "id": "Penguatan Daya Dari Input Ke Output." },
  { "en": "Apa Itu 'Linearitas'?", "id": "Kemampuan Penguat Bekerja Tanpa Distorsi." },
  { "en": "Apa Itu 'Titik Kompresi 1dB'?", "id": "Ukuran Batas Linearitas Penguat." },
  { "en": "Apa Itu 'Opto-isolator'?", "id": "Isolator Listrik Menggunakan Cahaya." },
  { "en": "Komponen Apa Dalam 'Opto-isolator'?", "id": "LED (Light Emitting Diode) Dan Phototransistor." },
  { "en": "Apa Itu 'Solid State Relay (SSR)'?", "id": "Relay Tanpa Bagian Bergerak." },
  { "en": "Komponen Apa Dalam 'SSR (Solid State Relay)'?", "id": "Opto-isolator Dan TRIAC Atau SCR." },
  { "en": "Apa Itu 'Arus Gelap (Dark Current)'?", "id": "Arus Bocor Pada Phototransistor." },
  { "en": "Apa Itu 'Resistansi On (On-Resistance)'?", "id": "Resistansi FET (Field-Effect Transistor) Saat Menyala." },
  { "en": "Apa Simbol 'Resistansi On'?", "id": "RDS(on)." },
  { "en": "Apa Itu 'Gerbang Mengambang (Floating Gate)'?", "id": "Gerbang Terisolasi Untuk Menyimpan Muatan." },
  { "en": "Di Mana 'Gerbang Mengambang' Digunakan?", "id": "Memori EPROM (Erasable PROM) Dan Flash." },
  { "en": "Apa Kepanjangan 'LDMOS (Laterally Diffused MOS)'?", "id": "MOS (Metal-Oxide-Semiconductor) Terdifusi Lateral." },
  { "en": "Di Mana 'LDMOS' Digunakan?", "id": "Penguat Daya RF (Radio Frequency)." },
  { "en": "Apa Itu 'Model Ebers-Moll'?", "id": "Model Matematika Untuk Transistor BJT." },
  { "en": "Apa Itu 'Model SPICE'?", "id": "Model Perilaku Komponen Untuk Simulasi." },
  { "en": "Apa Itu 'Dioda Bodi (Body Diode)'?", "id": "Dioda Parasit Di Dalam MOSFET." },
  { "en": "Apa Itu 'Muatan Gerbang (Gate Charge)'?", "id": "Muatan Yang Dibutuhkan Mengaktifkan Gerbang." },
  { "en": "Apa Satuan 'Muatan Gerbang'?", "id": "Coulomb (Biasanya nanoCoulomb)." },
  { "en": "Apa Itu 'Efek Avalanche'?", "id": "Perlipatgandaan Pembawa Muatan." },
  { "en": "Apa Itu 'Efek Zener'?", "id": "Mekanisme Tembus Pada Doping Tinggi." },
  { "en": "Apakah Transistor Bisa Simetris?", "id": "Beberapa Jenis JFET Bisa." },
  { "en": "Apa Itu 'Efek Hall'?", "id": "Pembangkitan Tegangan Akibat Medan Magnet." },
  { "en": "Apa Itu 'Sirkuit Terintegrasi (IC)'?", "id": "Banyak Transistor Dalam Satu Chip." },
  { "en": "Apa Itu 'Transistor Film Tipis (TFT)'?", "id": "Transistor Yang Digunakan Di Layar LCD." },
  { "en": "Apa Itu 'Elektronik Organik'?", "id": "Elektronik Berbasis Material Organik." },
  { "en": "Apa Kepanjangan 'OFET (Organic FET)'?", "id": "FET (Field-Effect Transistor) Organik." },
  { "en": "Apa Keuntungan 'OFET'?", "id": "Fleksibel Dan Dapat Dicetak." },
  { "en": "Apa Itu 'Papan Sirkuit Cetak (PCB)'?", "id": "Papan Untuk Merakit Komponen Elektronik." },
  { "en": "Apa Itu 'Proses Doping'?", "id": "Menambahkan Impuritas Ke Semikonduktor." },
  { "en": "Apa Itu 'Donor'?", "id": "Impuritas Yang Memberi Elektron (Tipe-N)." },
  { "en": "Apa Itu 'Akseptor'?", "id": "Impuritas Yang Menerima Elektron (Tipe-P)." },
  { "en": "Apa Itu 'Difusi'?", "id": "Proses Doping Pada Suhu Tinggi." },
  { "en": "Apa Itu 'Implantasi Ion'?", "id": "Proses Doping Dengan Menembakkan Ion." },
  { "en": "Apa Itu 'Rekombinasi'?", "id": "Saat Elektron Dan Lubang Bersatu." },
  { "en": "Apa Itu 'Arus Drift'?", "id": "Gerak Pembawa Muatan Akibat Medan Listrik." },
  { "en": "Apa Itu 'Arus Difusi'?", "id": "Gerak Pembawa Muatan Akibat Gradien." },
  { "en": "Apa Itu 'Mobilitas'?", "id": "Kemudahan Pembawa Muatan Bergerak." },
  { "en": "Apa Itu 'Masa Hidup (Lifetime)'?", "id": "Waktu Rata-rata Pembawa Muatan Eksis." },
  { "en": "Apa Itu 'Celah Pita (Band Gap)'?", "id": "Energi Yang Dibutuhkan Elektron." },
  { "en": "Apa Itu 'GAAFET (Gate-All-Around FET)'?", "id": "Evolusi Dari FinFET." },
  { "en": "Apa Itu 'MESFET (Metal-Semiconductor FET)'?", "id": "FET (Field-Effect Transistor) Dengan Gerbang Schottky." },
  { "en": "Apa Itu 'Sambungan Schottky'?", "id": "Sambungan Antara Logam Dan Semikonduktor." },
  { "en": "Apa Itu 'Efek Fotovoltaik'?", "id": "Pembangkitan Tegangan Akibat Cahaya." },
  { "en": "Bagaimana Cara Membuat 'Inverter CMOS'?", "id": "Menghubungkan PMOS Dan NMOS Secara Seri." },
  { "en": "Bagaimana Cara Membuat 'Gerbang NAND CMOS'?", "id": "Dua PMOS Paralel, Dua NMOS Seri." },
  { "en": "Bagaimana Cara Membuat 'Gerbang NOR CMOS'?", "id": "Dua PMOS Seri, Dua NMOS Paralel." },
  { "en": "Apa Beda 'hFE' Dan 'hfe'?", "id": "hFE Penguatan DC, hfe Penguatan AC." },
  { "en": "Apa Itu 'VCE(sat)'?", "id": "Tegangan Saturasi Kolektor-Emitor." },
  { "en": "Apa Arti 'VCE(sat)' Rendah?", "id": "Saklar Yang Baik, Kerugian Rendah." },
  { "en": "Apa Kepanjangan 'Ciss'?", "id": "Kapasitansi Input Transistor FET." },
  { "en": "Apa Kepanjangan 'Coss'?", "id": "Kapasitansi Output Transistor FET." },
  { "en": "Apa Kepanjangan 'Crss'?", "id": "Kapasitansi Transfer Balik." },
  { "en": "Apa Kepanjangan 'OFET (Organic FET)'?", "id": "FET (Field-Effect Transistor) Organik." },
  { "en": "Apa Bahan 'OFET (Organic FET)'?", "id": "Semikonduktor Berbasis Karbon." },
  { "en": "Apa Keuntungan 'OFET'?", "id": "Fleksibel Dan Dapat Dicetak." },
  { "en": "Apa Kepanjangan 'TFET (Tunnel FET)'?", "id": "FET (Field-Effect Transistor) Terowongan." },
  { "en": "Apa Keuntungan 'TFET'?", "id": "Konsumsi Daya Sangat Rendah." },
  { "en": "Apa Kepanjangan 'SET (Single-Electron Transistor)'?", "id": "Transistor Elektron Tunggal." },
  { "en": "Bagaimana 'SET' Bekerja?", "id": "Mengontrol Aliran Elektron Satu Persatu." },
  { "en": "Apa Itu 'Transistor Titik Kontak'?", "id": "Jenis Transistor Fungsional Pertama." },
  { "en": "Apa Itu 'Die'?", "id": "Satu Keping Inti Silikon Transistor." },
  { "en": "Apa Itu 'Wire Bonding'?", "id": "Menyambung Die Ke Kaki Paket." },
  { "en": "Bahan Apa Untuk 'Wire Bonding'?", "id": "Kawat Emas Atau Aluminium." },
  { "en": "Apa Itu 'Enkapsulasi'?", "id": "Proses Melindungi Die Dengan Plastik." },
  { "en": "Apa Itu 'Lead Frame'?", "id": "Kerangka Logam Untuk Kaki-kaki." },
  { "en": "Apa Itu 'Elektromigrasi'?", "id": "Pergerakan Atom Logam Akibat Arus." },
  { "en": "Apa Itu 'Injeksi Pembawa Panas'?", "id": "Elektron Panas Merusak Oksida Gerbang." },
  { "en": "Apa Itu 'Kerusakan Dielektrik'?", "id": "Kegagalan Lapisan Isolator." },
  { "en": "Mengapa 'Silikon (Si)' Banyak Digunakan?", "id": "Melimpah, Stabil, Dan Oksida Baik." },
  { "en": "Apa Itu 'Heterojunction'?", "id": "Sambungan Antara Dua Semikonduktor Berbeda." },
  { "en": "Bagaimana 'Phototransistor' Bekerja?", "id": "Cahaya Menghasilkan Arus Di Basis." },
  { "en": "Apa Beda 'Phototransistor' Dan 'Fotodioda'?", "id": "Phototransistor Punya Penguatan Internal." },
  { "en": "Apa Fungsi 'Gerbang Terapung (Floating Gate)'?", "id": "Menyimpan Muatan Listrik Secara Non-Volatil." },
  { "en": "Di Transistor Apa 'Gerbang Terapung' Ditemukan?", "id": "Transistor Memori Flash Dan EEPROM." },
  { "en": "Apa Itu 'Garis Beban DC (Direct Current)'?", "id": "Representasi Beban Untuk Analisis DC." },
  { "en": "Apa Itu 'Garis Beban AC (Alternating Current)'?", "id": "Representasi Beban Untuk Analisis AC." },
  { "en": "Apa Itu 'Titik Potong (Cutoff)'?", "id": "Titik Di Mana Transistor Mati." },
  { "en": "Apa Itu 'Titik Saturasi'?", "id": "Titik Di Mana Transistor Jenuh." },
  { "en": "Apa Itu 'Kurva Karakteristik'?", "id": "Grafik Yang Menjelaskan Perilaku Transistor." },
  { "en": "Apa Itu 'Beta Cutoff Frequency'?", "id": "Frekuensi Saat Beta (β) Turun." },
  { "en": "Apa Itu 'Gain-Bandwidth Product'?", "id": "Hasil Kali Gain Dan Bandwidth." },
  { "en": "Apa Itu 'Sziklai Pair'?", "id": "Pasangan Transistor Komplementer." },
  { "en": "Apa Nama Lain 'Sziklai Pair'?", "id": "Complementary Feedback Pair." },
  { "en": "Apa Itu 'Bootstrap'?", "id": "Teknik Umpan Balik Positif." },
  { "en": "Apa Itu 'Current Hogging'?", "id": "Arus Tidak Terbagi Rata." },
  { "en": "Apa Itu 'Ballast Resistor'?", "id": "Resistor Untuk Menyeimbangkan Arus." },
  { "en": "Apa Itu 'Efek Suhu' Pada Transistor?", "id": "Karakteristik Berubah Dengan Suhu." },
  { "en": "Bagaimana 'Beta (β)' Berubah Dengan Suhu?", "id": "Beta (β) Akan Meningkat." },
  { "en": "Apa Itu 'Kompensasi Suhu'?", "id": "Menjaga Kinerja Stabil Terhadap Suhu." },
  { "en": "Apa Itu 'Sirkuit Cascode'?", "id": "Konfigurasi Untuk Bandwidth Sangat Lebar." },
  { "en": "Apa Itu 'Current Limiting'?", "id": "Sirkuit Untuk Membatasi Arus Maksimum." },
  { "en": "Apa Itu 'Active Clamp'?", "id": "Sirkuit Pelindung Dari Lonjakan Tegangan." },
  { "en": "Apa Itu 'Flyback Diode'?", "id": "Dioda Pelindung Beban Induktif." },
  { "en": "Apa Itu 'GTO (Gate Turn-Off Thyristor)'?", "id": "Thyristor Yang Bisa Dimatikan Gerbang." },
  { "en": "Apa Itu 'LASCR (Light-Activated SCR)'?", "id": "SCR (Silicon Controlled Rectifier) Yang Diaktifkan Cahaya." },
  { "en": "Apa Itu 'DIAC (Diode for AC)'?", "id": "Dioda Pemicu Dua Arah Untuk TRIAC." },
  { "en": "Apa Itu 'PUT (Programmable UJT)'?", "id": "UJT (Unijunction Transistor) Yang Dapat Diprogram." },
  { "en": "Apa Itu 'LDMOS (Laterally Diffused MOS)'?", "id": "MOSFET (Metal-Oxide-Semiconductor FET) Daya Frekuensi Tinggi." },
  { "en": "Apa Itu 'VDMOS (Vertical Double-Diffused MOS)'?", "id": "Struktur Vertikal Untuk MOSFET Daya." },
  { "en": "Apa Itu 'UMOSFET (U-groove MOSFET)'?", "id": "MOSFET (Metal-Oxide-Semiconductor FET) Dengan Parit 'U'." },
  { "en": "Apa Itu 'SiGe (Silicon-Germanium)'?", "id": "Paduan Silikon-Germanium Untuk Kecepatan Tinggi." },
  { "en": "Apa Itu 'GNRFET (Graphene Nanoribbon FET)'?", "id": "FET (Field-Effect Transistor) Berbasis Pita Nano Graphene." },
  { "en": "Apa Itu 'FeFET (Ferroelectric FET)'?", "id": "FET (Field-Effect Transistor) Dengan Memori Feroelektrik." },
  { "en": "Apa Itu 'SONOS'?", "id": "Silicon-Oxide-Nitride-Oxide-Silicon." },
  { "en": "Apa Fungsi 'SONOS'?", "id": "Sebagai Transistor Memori Non-Volatil." },
  { "en": "Apa Itu 'Biasing Pembagi Tegangan'?", "id": "Metode Biasing BJT (Bipolar Junction Transistor) Yang Stabil." },
  { "en": "Apa Itu 'Biasing Umpan Balik Emitor'?", "id": "Metode Biasing Dengan Umpan Balik." },
  { "en": "Apa Itu 'Analisis Grafis'?", "id": "Menemukan Titik Q Dengan Garis Beban." },
  { "en": "Apa Itu 'Model Hibrida-Pi'?", "id": "Model Sinyal Kecil Untuk BJT." },
  { "en": "Apa Itu 'Parameter-re'?", "id": "Parameter Dioda Internal BJT." },
  { "en": "Apa Itu 'Tegangan Early'?", "id": "Parameter Yang Terkait Efek Early." },
  { "en": "Apa Itu 'Kapasitansi Deplesi'?", "id": "Kapasitansi Akibat Daerah Deplesi." },
  { "en": "Apa Itu 'Kapasitansi Difusi'?", "id": "Kapasitansi Akibat Injeksi Pembawa Minoritas." },
  { "en": "Apa Itu 'Kanal-N Enhancement'?", "id": "Tipe NMOS Paling Umum." },
  { "en": "Apa Itu 'Kanal-P Enhancement'?", "id": "Tipe PMOS Paling Umum." },
  { "en": "Apa Itu 'Substrat'?", "id": "Bahan Dasar Transistor." },
  { "en": "Apa Itu 'Body'?", "id": "Nama Lain Untuk Substrat." },
  { "en": "Apa Itu 'Gerbang (Gate) Oksida'?", "id": "Lapisan Isolator Tipis Di Bawah Gerbang." },
  { "en": "Apa Itu 'Transistor Unipolar'?", "id": "Transistor Yang Gunakan Satu Jenis Pembawa." },
  { "en": "Apa Itu 'Transistor Bipolar'?", "id": "Transistor Yang Gunakan Dua Jenis Pembawa." },
  { "en": "Apa Itu 'Arus Saturasi Mundur'?", "id": "Arus Bocor Pada Sambungan P-N." },
  { "en": "Apa Itu 'Tegangan Lutut (Knee Voltage)'?", "id": "Tegangan Saat Dioda Mulai Menghantar." },
  { "en": "Apa Itu 'Karakteristik Transfer'?", "id": "Grafik Output Terhadap Input." },
  { "en": "Apa Itu 'Penguat Operasional (Op-Amp)'?", "id": "Penguat Diferensial Dengan Gain Sangat Tinggi." },
  { "en": "Apa Itu 'Keluaran Push-Pull'?", "id": "Tahap Keluaran Dengan Transistor Atas-Bawah." },
  { "en": "Apa Itu 'Dioda Anti-Paralel'?", "id": "Dioda Terhubung Paralel Arah Berlawanan." },
  { "en": "Di Mana 'Dioda Anti-Paralel' Ditemukan?", "id": "Di Dalam IGBT (Insulated-Gate Bipolar Transistor)." },
  { "en": "Apa Itu 'Transistor Tunggal'?", "id": "Nama Lain Untuk Transistor Diskrit." },
  { "en": "Apa Itu 'Arus Maksimum Kontinu'?", "id": "Arus Maksimum Yang Boleh Mengalir Terus." },
  { "en": "Apa Itu 'Arus Puncak (Pulsed)'?", "id": "Arus Maksimum Sesaat." },
  { "en": "Apa Itu 'Transistor Array'?", "id": "Beberapa Transistor Dalam Satu Paket." },
  { "en": "Apa Itu 'Pasangan Terpadan (Matched Pair)'?", "id": "Dua Transistor Dengan Karakteristik Serupa." },
  { "en": "Mengapa 'Pasangan Terpadan' Penting?", "id": "Untuk Sirkuit Diferensial Dan Cermin Arus." },
  { "en": "Apa Itu 'Dioda Varicap'?", "id": "Dioda Yang Berfungsi Sebagai Kapasitor Variabel." },
  { "en": "Apa Itu 'Grown-Junction Transistor'?", "id": "Jenis Awal Transistor BJT." },
  { "en": "Apa Itu 'Alloy-Junction Transistor'?", "id": "Jenis Awal Lain Transistor BJT." },
  { "en": "Apa Itu 'Paket Hermetik'?", "id": "Paket Logam Atau Keramik Kedap Udara." },
  { "en": "Mengapa 'Paket Hermetik' Digunakan?", "id": "Untuk Keandalan Tinggi (Militer)." },
  { "en": "Apa Itu 'Junction Temperature (Tj)'?", "id": "Suhu Internal Sambungan Semikonduktor." },
  { "en": "Apa Batas 'Junction Temperature' Silikon?", "id": "Biasanya Sekitar 150-175 Derajat Celsius." },
  { "en": "Apa Fungsi 'Guard Ring'?", "id": "Mencegah Latch-up Dan Isolasi Derau." },
  { "en": "Apa Itu 'Body Effect'?", "id": "Perubahan Vth Akibat Tegangan Substrat." },
  { "en": "Apa Itu 'Pustaka Sel Standar'?", "id": "Kumpulan Blok Logika Siap Pakai." },
  { "en": "Apa Itu 'Sirkuit Cascode'?", "id": "Konfigurasi Untuk Gain-Bandwidth Tinggi." },
  { "en": "Apa Itu 'Rugi-rugi Konduksi'?", "id": "Daya Hilang Saat Transistor Menghantar." },
  { "en": "Apa Itu 'Rugi-rugi Pensaklaran'?", "id": "Daya Hilang Saat Transistor Beralih." },
  { "en": "Apa Kepanjangan 'RBSOA'?", "id": "Reverse-Biased Safe Operating Area." },
  { "en": "Apa Itu 'P1dB'?", "id": "Titik Kompresi Gain 1-desibel." },
  { "en": "Apa Itu 'IP3'?", "id": "Titik Intersep Orde Ketiga." },
  { "en": "Apa Yang Membatasi Kecepatan Transistor?", "id": "Kapasitansi Internal Dan Waktu Transit." },
  { "en": "Apa Itu 'Fotodarlington'?", "id": "Darlington Pair Yang Diaktifkan Cahaya." },
  { "en": "Apa Itu 'Curve Tracer'?", "id": "Alat Untuk Menggambar Kurva Karakteristik." },
  { "en": "Apa Itu 'Parameter Analyzer'?", "id": "Alat Canggih Untuk Karakterisasi Semikonduktor." },
  { "en": "Apa Itu 'Wafer Probing'?", "id": "Pengujian Transistor Saat Masih Di Wafer." },
  { "en": "Apa Itu 'Efek Miller'?", "id": "Peningkatan Kapasitansi Input Akibat Gain." },
  { "en": "Bagaimana Cara Mengurangi 'Efek Miller'?", "id": "Dengan Menggunakan Konfigurasi Cascode." },
  { "en": "Apa Itu 'Transistor Balistik'?", "id": "Elektron Bergerak Tanpa Hamburan." },
  { "en": "Apa Itu 'Kanal Terkubur (Buried Channel)'?", "id": "Kanal Di Bawah Permukaan." },
  { "en": "Apa Keuntungan 'Kanal Terkubur'?", "id": "Mengurangi Derau Frekuensi Rendah." },
  { "en": "Apa Itu 'Daerah Pinch-off'?", "id": "Kondisi Kanal JFET Mulai Menyempit." },
  { "en": "Apa Itu 'Tegangan Saturasi'?", "id": "Tegangan Vds Saat Arus Jenuh." },
  { "en": "Apa Itu 'Early Voltage'?", "id": "Parameter Yang Terkait Efek Early." },
  { "en": "Apa Itu 'Noise Margin'?", "id": "Toleransi Level Logika Terhadap Derau." },
  { "en": "Apa Itu 'Fan-Out'?", "id": "Jumlah Beban Yang Dapat Digerakkan." },
  { "en": "Apa Itu 'Fan-In'?", "id": "Jumlah Input Ke Sebuah Gerbang." },
  { "en": "Apa Itu 'Transistor Sebagai Resistor Variabel'?", "id": "FET (Field-Effect Transistor) Beroperasi Di Daerah Linier." },
  { "en": "Apa Itu 'Transistor Sebagai Kapasitor Variabel'?", "id": "Dioda Varactor Atau MOSFET." },
  { "en": "Apa Itu 'Bootstrap'?", "id": "Teknik Menaikkan Tegangan Menggunakan Kapasitor." },
  { "en": "Apa Itu 'Pompa Muatan (Charge Pump)'?", "id": "Sirkuit Penaik Tegangan Berbasis Kapasitor." },
  { "en": "Apa Itu 'Transistor Matched Pair'?", "id": "Dua Transistor Dengan Karakteristik Identik." },
  { "en": "Mengapa 'Matched Pair' Penting?", "id": "Untuk Penguat Diferensial Dan Cermin Arus." },
  { "en": "Apa Itu 'Layout Transistor'?", "id": "Gambar Geometris Fisik Transistor." },
  { "en": "Apa Itu 'Common-Centroid Layout'?", "id": "Teknik Layout Untuk Matching Yang Baik." },
  { "en": "Apa Itu 'Interdigitation'?", "id": "Teknik Layout Jari-jemari Bersisipan." },
  { "en": "Apa Itu 'Suhu Sambungan (Junction Temperature)'?", "id": "Suhu Internal Sambungan Transistor." },
  { "en": "Apa Itu 'Resistansi Termal'?", "id": "Hambatan Aliran Panas Dari Sambungan." },
  { "en": "Apa Satuan 'Resistansi Termal'?", "id": "Derajat Celsius Per Watt." },
  { "en": "Apa Itu 'Dioda Bodi (Body Diode)'?", "id": "Dioda Parasit Di Dalam MOSFET." },
  { "en": "Kapan 'Dioda Bodi' Menghantar?", "id": "Saat Tegangan Source-Drain Terbalik." },
  { "en": "Apa Itu 'Avalanche Breakdown'?", "id": "Tembusan Akibat Ionisasi Tumbukan." },
  { "en": "Apa Itu 'Second Breakdown'?", "id": "Kegagalan Merusak Akibat Arus Terkonsentrasi." },
  { "en": "Apa Itu 'Kompensasi Frekuensi'?", "id": "Menstabilkan Penguat Pada Frekuensi Tinggi." },
  { "en": "Apa Itu 'Penguat Umpan Balik'?", "id": "Penguat Yang Gunakan Umpan Balik." },
  { "en": "Apa Itu 'Umpan Balik Negatif'?", "id": "Umpan Balik Yang Menstabilkan Penguat." },
  { "en": "Apa Itu 'Umpan Balik Positif'?", "id": "Umpan Balik Yang Sebabkan Osilasi." },
  { "en": "Apa Itu 'Kriteria Stabilitas Barkhausen'?", "id": "Syarat Terjadinya Osilasi." },
  { "en": "Apa Itu 'Osilator Colpitts'?", "id": "Osilator Berbasis Pembagi Kapasitif." },
  { "en": "Apa Itu 'Osilator Hartley'?", "id": "Osilator Berbasis Pembagi Induktif." },
  { "en": "Apa Itu 'Osilator Pierce'?", "id": "Osilator Kristal Dengan Konfigurasi Umum." },
  { "en": "Apa Itu 'Model Sinyal Besar'?", "id": "Model Yang Valid Untuk Sinyal Besar." },
  { "en": "Apa Itu 'Model Sinyal Kecil'?", "id": "Model Linier Untuk Sinyal Kecil." },
  { "en": "Apa Itu 'Linierisasi'?", "id": "Aproksimasi Perilaku Non-linier Menjadi Linier." },
  { "en": "Apa Itu 'Biasing Diri (Self-Biasing)'?", "id": "Metode Biasing Yang Stabil." },
  { "en": "Apa Itu 'Titik Operasi'?", "id": "Titik Kerja DC (Direct Current) Tanpa Sinyal." },
  { "en": "Apa Itu 'Arus Kolektor Diam'?", "id": "Arus Kolektor Pada Titik Operasi." },
  { "en": "Apa Itu 'Saturation Current'?", "id": "Arus Maksimum Yang Bisa Mengalir." },
  { "en": "Apa Itu 'Tegangan Pinch-off'?", "id": "Tegangan Yang Menutup Kanal JFET." },
  { "en": "Apa Itu 'Modulasi Panjang Kanal'?", "id": "Variasi Arus Akibat Perubahan Vds." },
  { "en": "Apa Itu 'Resistansi Output'?", "id": "Resistansi Dilihat Dari Terminal Output." },
  { "en": "Apa Itu 'Parameter Alfa (α)'?", "id": "Rasio Arus Kolektor Terhadap Emitor." },
  { "en": "Apa Itu 'Transistor Fotoelektrik'?", "id": "Nama Lain Untuk Fototransistor." },
  { "en": "Apa Itu 'Solid-State Switch'?", "id": "Saklar Elektronik Tanpa Bagian Bergerak." },
  { "en": "Apakah Transistor 'Solid-State'?", "id": "Ya, Transistor Adalah Solid-State." },
  { "en": "Apa Itu 'Gate Drive'?", "id": "Sirkuit Untuk Menggerakkan Gerbang MOSFET." },
  { "en": "Mengapa 'Gate Drive' Dibutuhkan?", "id": "Untuk Mengisi-Kosongkan Kapasitansi Gerbang." },
  { "en": "Apa Itu 'Muatan Gerbang (Gate Charge)'?", "id": "Total Muatan Untuk Menyalakan Gerbang." },
  { "en": "Apa Itu 'Efek Suhu' Pada Transistor?", "id": "Perubahan Karakteristik Akibat Suhu." },
  { "en": "Apa Itu 'Kompensasi Dioda'?", "id": "Menggunakan Dioda Untuk Stabilisasi Suhu." },
  { "en": "Apa Itu 'Sumber Arus Konstan'?", "id": "Sirkuit Yang Memberikan Arus Stabil." },
  { "en": "Apa Itu 'Current Steering'?", "id": "Mengarahkan Arus Menggunakan Saklar Diferensial." },
  { "en": "Apa Itu 'Sel Gilbert'?", "id": "Inti Sirkuit Pencampur (Mixer) Seimbang." },
  { "en": "Apa Itu 'Konversi Frekuensi'?", "id": "Mengubah Sinyal Dari Satu Frekuensi." },
  { "en": "Apa Itu 'Bahan Semikonduktor'?", "id": "Material Dasar Pembuatan Transistor." },
  { "en": "Apa Itu 'Wafer Silikon'?", "id": "Irisan Tipis Kristal Silikon Murni." },
  { "en": "Apa Itu 'Fabrikasi'?", "id": "Proses Pembuatan Transistor Di Wafer." },
  { "en": "Apa Itu 'Sirkuit Terpadu (IC)'?", "id": "Banyak Transistor Dalam Satu Chip." },
  { "en": "Apa Itu 'Transistor Planar'?", "id": "Proses Fabrikasi Transistor Modern." },
  { "en": "Siapa Yang Mengembangkan 'Proses Planar'?", "id": "Jean Hoerni." },
  { "en": "Apa Itu 'Transistor Paduan (Alloy-Junction)'?", "id": "Jenis Awal Transistor BJT." },
  { "en": "Apa Itu 'Transistor Tumbuh (Grown-Junction)'?", "id": "Jenis Awal Lain Transistor BJT." },
  { "en": "Apa Itu 'Doping Kompensasi'?", "id": "Menetralkan Efek Doping Sebelumnya." },
  { "en": "Apa Itu 'Profil Doping'?", "id": "Distribusi Konsentrasi Dopan." },
  { "en": "Apa Itu 'Sambungan Bertingkat (Graded)'?", "id": "Sambungan Dengan Profil Doping Gradual." },
  { "en": "Apa Itu 'Sambungan Tajam (Abrupt)'?", "id": "Sambungan Dengan Profil Doping Tiba-tiba." },
  { "en": "Apa Itu 'Kanal Tipe-N'?", "id": "Arus Dibawa Oleh Elektron." },
  { "en": "Apa Itu 'Kanal Tipe-P'?", "id": "Arus Dibawa Oleh Lubang (Hole)." },
  { "en": "Apa Itu 'Substrat'?", "id": "Bahan Dasar Tempat Transistor Dibuat." },
  { "en": "Apa Itu 'Efek Kanal Pendek'?", "id": "Perilaku Non-ideal Akibat Penskalaan." },
  { "en": "Apa Itu 'DIBL (Drain-Induced Barrier Lowering)'?", "id": "Efek Kanal Pendek." },
  { "en": "Apa Itu 'Saturasi Kecepatan'?", "id": "Kecepatan Maksimum Pembawa Muatan." },
  { "en": "Apa Itu 'Oksida Tipis'?", "id": "Lapisan Isolator Gerbang Sangat Tipis." },
  { "en": "Apa Itu 'Transistor Sebagai Dioda'?", "id": "Menghubungkan Basis Dan Kolektor BJT." },
  { "en": "Apa Itu 'Beban Dioda'?", "id": "Menggunakan Transistor Terhubung Dioda." },
  { "en": "Apa Itu 'Konfigurasi Sziklai'?", "id": "Nama Lain Untuk Pasangan Sziklai." },
  { "en": "Apa Itu 'Bootstrap'?", "id": "Teknik Umpan Balik Positif." },
  { "en": "Apa Itu 'Masker Fase-Geser (PSM)'?", "id": "Masker Untuk Meningkatkan Resolusi Litografi." },
  { "en": "Apa Kepanjangan 'OPC (Optical Proximity Correction)'?", "id": "Koreksi Kedekatan Optik." },
  { "en": "Apa Fungsi 'OPC (Optical Proximity Correction)'?", "id": "Mengubah Bentuk Masker Untuk Kompensasi." },
  { "en": "Apa Itu 'Litografi Imersi'?", "id": "Litografi Menggunakan Cairan Antara Lensa." },
  { "en": "Apa Itu 'Wafer Flat'?", "id": "Sisi Datar Penanda Orientasi Kristal." },
  { "en": "Apa Itu 'Wafer Notch'?", "id": "Takik Kecil Penanda Orientasi Kristal." },
  { "en": "Apa Beda 'Wafer Flat' Dan 'Notch'?", "id": "Notch Menghemat Area Wafer." },
  { "en": "Apa Itu 'Cacat Titik (Point Defect)'?", "id": "Cacat Kristal Melibatkan Satu Atom." },
  { "en": "Apa Itu 'Kekosongan (Vacancy)'?", "id": "Posisi Atom Kosong Dalam Kisi." },
  { "en": "Apa Itu 'Interstisial'?", "id": "Atom Tambahan Di Antara Posisi Kisi." },
  { "en": "Apa Itu 'Dislokasi'?", "id": "Cacat Garis Dalam Struktur Kristal." },
  { "en": "Apa Itu 'Gettering'?", "id": "Proses Menjebak Kontaminan Jauh." },
  { "en": "Apa Itu 'Analisis Kegagalan (Failure Analysis)'?", "id": "Investigasi Penyebab Kerusakan IC." },
  { "en": "Apa Itu 'Delayering'?", "id": "Mengupas Lapisan IC Untuk Analisis." },
  { "en": "Apa Itu 'Mikroskopi Akustik'?", "id": "Pencitraan IC (Integrated Circuit) Menggunakan Suara." },
  { "en": "Apa Itu 'Lead Pitch'?", "id": "Jarak Antar Pusat Kaki-kaki IC." },
  { "en": "Apa Itu 'Coplanarity'?", "id": "Kesejajaran Ujung Kaki-kaki IC." },
  { "en": "Apa Itu 'Bahan Die Attach'?", "id": "Perekat Konduktif Untuk Melekatkan Die." },
  { "en": "Apa Itu 'Solder Bridging'?", "id": "Hubung Singkat Akibat Timah Solder." },
  { "en": "Apa Itu 'Cold Solder Joint'?", "id": "Sambungan Solder Yang Tidak Matang." },
  { "en": "Apa Itu 'Popcorning'?", "id": "Kerusakan Paket IC Akibat Kelembaban." },
  { "en": "Apa Kepanjangan 'MSL (Moisture Sensitivity Level)'?", "id": "Tingkat Sensitivitas Kelembaban." },
  { "en": "Apa Itu 'Dry Pack'?", "id": "Kemasan Vakum Untuk Komponen Sensitif." },
  { "en": "Apa Itu 'Sirkuit Terpadu Hibrida'?", "id": "Gabungan Teknologi Chip Dan Komponen Diskrit." },
  { "en": "Apa Itu 'Substrat Keramik'?", "id": "Papan Sirkuit Berbasis Keramik." },
  { "en": "Apa Kepanjangan 'LTCC (Low-Temperature Co-fired Ceramic)'?", "id": "Keramik Co-fired Suhu Rendah." },
  { "en": "Apa Itu 'IC Analog Switch'?", "id": "Saklar Elektronik Untuk Sinyal Analog." },
  { "en": "Apa Itu 'IC Multiplexer Analog'?", "id": "Pemilih Sinyal Analog." },
  { "en": "Apa Itu 'IC Power Driver'?", "id": "IC (Integrated Circuit) Penggerak Beban Daya." },
  { "en": "Apa Itu 'IC Motor Driver'?", "id": "IC (Integrated Circuit) Pengendali Motor." },
  { "en": "Apa Itu 'H-Bridge'?", "id": "Sirkuit Pengubah Arah Putaran Motor." },
  { "en": "Apa Itu 'IC Display Driver'?", "id": "IC (Integrated Circuit) Pengendali Layar." },
  { "en": "Apa Itu 'IC Real-Time Clock (RTC)'?", "id": "IC (Integrated Circuit) Penyimpan Waktu." },
  { "en": "Apa Itu 'Power Integrity'?", "id": "Menjaga Kualitas Jaringan Catu Daya." },
  { "en": "Apa Itu 'Signal Integrity'?", "id": "Menjaga Kualitas Sinyal Listrik." },
  { "en": "Apa Itu 'Impedansi' Jalur?", "id": "Karakteristik Listrik Jalur Transmisi." },
  { "en": "Apa Itu 'Terminasi'?", "id": "Mencegah Pantulan Sinyal Di Ujung Jalur." },
  { "en": "Apa Itu 'Overshoot'?", "id": "Sinyal Melebihi Level Tegangan Targetnya." },
  { "en": "Apa Itu 'Undershoot'?", "id": "Sinyal Jatuh Di Bawah Level Targetnya." },
  { "en": "Apa Itu 'Ringing'?", "id": "Osilasi Sesaat Pada Tepi Sinyal." },
  { "en": "Apa Itu 'Ground Bounce'?", "id": "Fluktuasi Tegangan Pada Jalur Ground." },
  { "en": "Apa Itu 'Power Supply Noise'?", "id": "Derau Pada Jalur Catu Daya." },
  { "en": "Apa Fungsi 'Kapasitor Decoupling'?", "id": "Menstabilkan Tegangan Suplai Lokal." },
  { "en": "Apa Fungsi 'Kapasitor Bypass'?", "id": "Menyaring Derau Frekuensi Tinggi." },
  { "en": "Apa Itu 'Ferrite Bead'?", "id": "Komponen Pasif Peredam Derau." },
  { "en": "Apa Itu 'Common-Mode Choke'?", "id": "Induktor Untuk Menekan Derau Common-Mode." },
  { "en": "Apa Itu 'Desain RTL (Register-Transfer Level)'?", "id": "Mendeskripsikan Aliran Data Antar Register." },
  { "en": "Apa Itu 'Verifikasi'?", "id": "Memastikan Desain Berfungsi Sesuai Rencana." },
  { "en": "Apa Itu 'Simulasi'?", "id": "Menjalankan Model Komputer Dari Desain." },
  { "en": "Apa Itu 'Testbench'?", "id": "Kode Yang Menguji Desain Sirkuit." },
  { "en": "Apa Itu 'Sintesis'?", "id": "Mengubah Kode Desain Menjadi Gerbang Logika." },
  { "en": "Apa Itu 'Netlist'?", "id": "Deskripsi Koneksi Antar Gerbang Logika." },
  { "en": "Apa Itu 'Timing Analysis'?", "id": "Analisis Waktu Tunda Sinyal Sirkuit." },
  { "en": "Apa Itu 'Clock Skew'?", "id": "Perbedaan Waktu Tiba Sinyal Clock." },
  { "en": "Apa Itu 'Clock Jitter'?", "id": "Variasi Periode Sinyal Clock." },
  { "en": "Apa Itu 'Gate Delay'?", "id": "Waktu Tunda Sinyal Melewati Gerbang." },
  { "en": "Apa Itu 'Wire Delay'?", "id": "Waktu Tunda Sinyal Di Jalur Logam." },
  { "en": "Apa Itu 'Jalur Kritis (Critical Path)'?", "id": "Jalur Dengan Penundaan Paling Lama." },
  { "en": "Apa Itu 'Setup Time Violation'?", "id": "Pelanggaran Waktu Setup." },
  { "en": "Apa Itu 'Hold Time Violation'?", "id": "Pelanggaran Waktu Hold." },
  { "en": "Apa Itu 'Metastabilitas'?", "id": "Keadaan Tidak Menentu Pada Flip-Flop." },
  { "en": "Apa Itu 'Sinkronizer'?", "id": "Sirkuit Untuk Menangani Sinyal Asinkron." },
  { "en": "Apa Kepanjangan 'CDC (Clock-Domain Crossing)'?", "id": "Lintas Domain Clock." },
  { "en": "Apa Itu 'Gray Code'?", "id": "Kode Biner Yang Hanya Ubah Satu Bit." },
  { "en": "Mengapa 'Gray Code' Dipakai Di CDC?", "id": "Untuk Mengurangi Kesalahan Transisi." },
  { "en": "Apa Itu 'FIFO (First-In, First-Out)' Asinkron?", "id": "Buffer Untuk Transfer Data Antar Clock." },
  { "en": "Apa Itu 'Manufaktur Aditif'?", "id": "Proses Membangun Objek Lapis Demi Lapis." },
  { "en": "Apa Itu 'Manufaktur Subtraktif'?", "id": "Proses Membuang Material Dari Blok." },
  { "en": "Apa Itu 'Printed Electronics'?", "id": "Mencetak Sirkuit Menggunakan Tinta Khusus." },
  { "en": "Apa Itu 'Tinta Konduktif'?", "id": "Tinta Mengandung Partikel Logam." },
  { "en": "Apa Itu 'Elektronik Fleksibel'?", "id": "Sirkuit Yang Dibuat Di Substrat Fleksibel." },
  { "en": "Apa Itu 'Elektronik Organik'?", "id": "Elektronik Berbasis Material Semikonduktor Organik." },
  { "en": "Apa Kepanjangan 'OLED (Organic LED)'?", "id": "Light Emitting Diode (LED) Organik." },
  { "en": "Apa Kepanjangan 'OPV (Organic Photovoltaic)'?", "id": "Sel Surya Organik." },
  { "en": "Apa Itu 'Fotodetektor'?", "id": "Komponen Yang Mendeteksi Cahaya." },
  { "en": "Apa Itu 'Sensor Gambar'?", "id": "IC (Integrated Circuit) Yang Menangkap Gambar." },
  { "en": "Apa Kepanjangan 'CCD (Charge-Coupled Device)'?", "id": "Perangkat Terkopel Muatan." },
  { "en": "Apa Kepanjangan 'CIS (CMOS Image Sensor)'?", "id": "Sensor Gambar CMOS." },
  { "en": "Apa Itu 'Piksel'?", "id": "Elemen Individual Dari Sensor Gambar." },
  { "en": "Apa Itu 'IC Driver'?", "id": "IC (Integrated Circuit) Untuk Menggerakkan Komponen Lain." },
  { "en": "Contoh 'IC Driver'?", "id": "Driver LED, Driver Motor." },
  { "en": "Apa Itu 'Logic Level'?", "id": "Level Tegangan Untuk Logika 0 Dan 1." },
  { "en": "Apa Itu 'Keluarga Logika TTL'?", "id": "Logika Berbasis BJT (Bipolar Junction Transistor)." },
  { "en": "Apa Itu 'Keluarga Logika CMOS'?", "id": "Logika Berbasis MOSFET (Metal-Oxide-Semiconductor FET)." },
  { "en": "Apa Itu 'Pull-up Resistor'?", "id": "Resistor Penarik Tegangan Ke Atas." },
  { "en": "Apa Itu 'Pull-down Resistor'?", "id": "Resistor Penarik Tegangan Ke Bawah." },
  { "en": "Apa Itu 'Keluaran Open-Drain'?", "id": "Output Yang Hanya Bisa Menarik Ke Ground." },
  { "en": "Apa Itu 'Buffer Tri-state'?", "id": "Buffer Dengan Keadaan Impedansi Tinggi." },
  { "en": "Apa Itu 'Bus Contention'?", "id": "Konflik Saat Dua Output Menggerakkan Bus." },
  { "en": "Apa Itu Format 'GDSII'?", "id": "Format File Standar Untuk Layout IC." },
  { "en": "Apa Itu 'OASIS'?", "id": "Format Pengganti GDSII Yang Lebih Efisien." },
  { "en": "Apa Kepanjangan 'LEF (Library Exchange Format)'?", "id": "Format Pertukaran Pustaka (Abstrak)." },
  { "en": "Apa Kepanjangan 'DEF (Design Exchange Format)'?", "id": "Format Pertukaran Desain (Penempatan)." },
  { "en": "Apa Itu 'Tahap Fetch' Pipeline?", "id": "Mengambil Instruksi Dari Memori." },
  { "en": "Apa Itu 'Tahap Decode' Pipeline?", "id": "Menerjemahkan Instruksi." },
  { "en": "Apa Itu 'Tahap Execute' Pipeline?", "id": "Menjalankan Operasi Aritmetika Atau Logika." },
  { "en": "Apa Itu 'Barrel Shifter'?", "id": "Sirkuit Penggeser Bit Berkecepatan Tinggi." },
  { "en": "Apa Itu 'Wallace Tree'?", "id": "Struktur Pengali Biner Yang Cepat." },
  { "en": "Apa Itu 'Carry-Save Adder'?", "id": "Penjumlah Yang Menunda Propagasi Carry." },
  { "en": "Apa Kepanjangan 'OTA (Operational Transconductance Amplifier)'?", "id": "Penguat Transkonduktansi Operasional." },
  { "en": "Apa Output Sebuah 'OTA'?", "id": "Sumber Arus Yang Dikontrol Tegangan." },
  { "en": "Apa Itu 'Sel Gilbert'?", "id": "Sirkuit Inti Untuk Pencampur (Mixer)." },
  { "en": "Apa Fungsi 'Referensi Celah Pita (Bandgap Reference)'?", "id": "Menghasilkan Tegangan Referensi Yang Sangat Stabil." },
  { "en": "Apa Itu 'Mobilitas Pembawa'?", "id": "Ukuran Seberapa Cepat Pembawa Bergerak." },
  { "en": "Apa Itu 'Masa Hidup Pembawa'?", "id": "Waktu Rata-rata Sebelum Terjadinya Rekombinasi." },
  { "en": "Apa Itu 'Tingkat Fermi'?", "id": "Tingkat Energi Probabilitas Pengisian Setengah." },
  { "en": "Apa Fungsi 'Gate Driver'?", "id": "Menguatkan Sinyal Untuk Menggerakkan Gerbang." },
  { "en": "Apa Itu 'Waktu Mati (Dead Time)'?", "id": "Jeda Mencegah Konduksi Bersamaan." },
  { "en": "Apa Itu 'Soft Start'?", "id": "Fitur Menaikkan Tegangan Output Perlahan." },
  { "en": "Apa Kepanjangan 'UVLO (Under-Voltage Lockout)'?", "id": "Penguncian Tegangan Kurang." },
  { "en": "Apa Fungsi 'UVLO (Under-Voltage Lockout)'?", "id": "Mematikan IC (Integrated Circuit) Saat Suplai Rendah." },
  { "en": "Apa Itu 'Lapisan Fisik (PHY)'?", "id": "Sirkuit Transceiver Untuk Komunikasi." },
  { "en": "Apa Itu 'Kontrol Akses Media (MAC)'?", "id": "Logika Pengontrol Akses Jaringan." },
  { "en": "Apa Itu 'Bahan Die Attach'?", "id": "Perekat Untuk Melekatkan Die." },
  { "en": "Apa Jenis 'Bahan Die Attach'?", "id": "Epoksi Konduktif Atau Non-Konduktif." },
  { "en": "Apa Bahan 'Enkapsulasi'?", "id": "Resin Epoksi." },
  { "en": "Apa Bahan 'Leadframe'?", "id": "Paduan Tembaga Atau Besi-Nikel." },
  { "en": "Apa Beda 'Theta-JA' Dan 'Theta-JC'?", "id": "JA Ke Udara, JC Ke Casing." },
  { "en": "Apa Itu 'Logic Synthesis'?", "id": "Nama Lain Untuk Sintesis Logika." },
  { "en": "Apa Itu 'Physical Synthesis'?", "id": "Sintesis Yang Memperhitungkan Tata Letak." },
  { "en": "Apa Itu 'Standard Cell'?", "id": "Blok Logika Dasar Dengan Tinggi Sama." },
  { "en": "Apa Itu 'Power Rail'?", "id": "Jalur Logam Distribusi Daya (VCC/GND)." },
  { "en": "Apa Itu 'Power Grid'?", "id": "Jaringan Power Rail Di Seluruh Chip." },
  { "en": "Apa Itu 'Clock Spine'?", "id": "Jalur Utama Distribusi Sinyal Clock." },
  { "en": "Apa Itu 'Buffer Clock'?", "id": "Buffer Untuk Menguatkan Sinyal Clock." },
  { "en": "Apa Itu 'IO Pad'?", "id": "Struktur Fisik Untuk Koneksi Pin." },
  { "en": "Apa Fungsi 'Sirkuit Proteksi ESD'?", "id": "Melindungi Inti IC Dari ESD (Electrostatic Discharge)." },
  { "en": "Apa Itu 'Antenna Diode'?", "id": "Dioda Untuk Melindungi Dari Efek Antena." },
  { "en": "Apa Itu 'Efek Antena'?", "id": "Penumpukan Muatan Saat Proses Etsa." },
  { "en": "Apa Itu 'Design Rule'?", "id": "Aturan Geometris Dari Proses Fabrikasi." },
  { "en": "Siapa Yang Menyediakan 'Design Rule'?", "id": "Pabrik Semikonduktor (Foundry)." },
  { "en": "Apa Itu 'Sel Memori 6T'?", "id": "Sel SRAM (Static RAM) Dengan Enam Transistor." },
  { "en": "Apa Itu 'Sel Memori 1T1C'?", "id": "Sel DRAM (Dynamic RAM) Satu Transistor-Kapasitor." },
  { "en": "Apa Itu 'DRAM Refresh'?", "id": "Proses Mengisi Ulang Muatan Kapasitor." },
  { "en": "Apa Itu 'Kapasitor Trench'?", "id": "Kapasitor DRAM (Dynamic RAM) Yang Dibuat Vertikal." },
  { "en": "Apa Itu 'Kapasitor Tumpuk (Stacked)'?", "id": "Kapasitor DRAM (Dynamic RAM) Di Atas Transistor." },
  { "en": "Apa Itu 'Tunnel Oxide'?", "id": "Oksida Sangat Tipis Untuk Penerowongan." },
  { "en": "Sel Mana Yang Paling Tahan Lama?", "id": "SLC (Single-Level Cell)." },
  { "en": "Sel Mana Yang Punya Kapasitas Terbesar?", "id": "QLC (Quad-Level Cell)." },
  { "en": "Apa Itu 'Logic-in-Memory'?", "id": "Konsep Melakukan Komputasi Di Dalam Memori." },
  { "en": "Apa Itu 'Sinapsis Buatan'?", "id": "Elemen Sirkuit Yang Meniru Sinapsis." },
  { "en": "Apa Itu 'Neuron Buatan'?", "id": "Sirkuit Yang Meniru Fungsi Neuron." },
  { "en": "Apa Itu 'Fotonik Silikon'?", "id": "Sirkuit Optik Terintegrasi Di Silikon." },
  { "en": "Apa Itu 'Modulator' Optik?", "id": "Perangkat Yang Memodulasi Sinar Cahaya." },
  { "en": "Apa Itu 'Detektor Foto'?", "id": "Perangkat Pengubah Cahaya Menjadi Listrik." },
  { "en": "Apa Itu 'Pandu Gelombang (Waveguide)'?", "id": "Struktur Pemandu Sinyal Cahaya." },
  { "en": "Apa Itu 'Grating Coupler'?", "id": "Struktur Untuk Memasukkan Cahaya Ke Chip." },
  { "en": "Apa Itu 'Ring Resonator'?", "id": "Resonator Optik Berbentuk Cincin." },
  { "en": "Apa Itu 'Kristal Fotonik'?", "id": "Struktur Periodik Yang Memanipulasi Foton." },
  { "en": "Apa Itu 'Phase-Change Material (PCM)'?", "id": "Material Pengubah Fasa Untuk Memori." },
  { "en": "Apa Itu 'Memristor'?", "id": "Resistor Dengan Memori." },
  { "en": "Apa Fase Awal 'Kurva Bak Mandi'?", "id": "Fase Kegagalan Dini (Infant Mortality)." },
  { "en": "Apa Fase Tengah 'Kurva Bak Mandi'?", "id": "Fase Umur Berguna (Useful Life)." },
  { "en": "Apa Fase Akhir 'Kurva Bak Mandi'?", "id": "Fase Aus (Wear-Out)." },
  { "en": "Apa Itu 'Derating Kurva'?", "id": "Grafik Batas Operasi Aman Komponen." },
  { "en": "Apa Itu 'Analisis Termal'?", "id": "Simulasi Distribusi Suhu Pada Chip." },
  { "en": "Apa Itu 'Hotspot'?", "id": "Area Dengan Suhu Paling Tinggi." },
  { "en": "Apa Itu 'Pemodelan Perangkat'?", "id": "Membuat Model Matematika Perilaku Transistor." },
  { "en": "Apa Kepanjangan 'SPICE (Simulation Program with Integrated Circuit Emphasis)'?", "id": "Program Simulasi Dengan Penekanan IC." },
  { "en": "Apa Itu 'Netlist SPICE'?", "id": "Deskripsi Sirkuit Untuk Simulator SPICE." },
  { "en": "Apa Itu 'Model BSIM'?", "id": "Model Standar Industri Untuk MOSFET." },
  { "en": "Apa Itu 'Verilog-A'?", "id": "Bahasa Pemodelan Perilaku Sirkuit Analog." },
  { "en": "Apa Itu 'Verifikasi Sinyal Campuran'?", "id": "Verifikasi Sirkuit Dengan Bagian Analog-Digital." },
  { "en": "Apa Itu 'Desain Hirarkis'?", "id": "Membagi Desain Menjadi Blok-blok Hirarkis." },
  { "en": "Apa Itu 'Top-Level Design'?", "id": "Level Paling Atas Dalam Hierarki Desain." },
  { "en": "Apa Itu 'Blok Bangunan (Building Block)'?", "id": "Modul Sirkuit Yang Dapat Digunakan Kembali." },
  { "en": "Apa Itu 'Desain Full-Custom'?", "id": "Setiap Transistor Digambar Secara Manual." },
  { "en": "Apa Itu 'Desain Semi-Custom'?", "id": "Menggunakan Sel Standar Dan Blok IP." },
  { "en": "Apa Itu 'Perpustakaan (Library)' Dalam Desain?", "id": "Kumpulan Sel Atau Komponen Standar." },
  { "en": "Apa Itu 'Karakterisasi Sel'?", "id": "Mengukur Kinerja Sel Pustaka." },
  { "en": "Apa Itu 'Tapeout'?", "id": "Proses Mengirim Desain Final Ke Pabrik." },
  { "en": "Apa Itu 'Manufaktur Masker'?", "id": "Proses Pembuatan Photomask." },
  { "en": "Apa Itu 'E-beam Writer'?", "id": "Alat Penulis Pola Masker Dengan Elektron." },
  { "en": "Apa Itu 'Pemeriksaan Masker'?", "id": "Memeriksa Cacat Pada Photomask." },
  { "en": "Apa Itu 'Reticle Enhancement Technology (RET)'?", "id": "Teknik Canggih Untuk Meningkatkan Kualitas Cetak." },
  { "en": "Apa Itu 'Optical Proximity Correction (OPC)'?", "id": "Contoh Teknik RET (Reticle Enhancement Technology)." },
  { "en": "Apa Itu 'Phase-Shift Mask (PSM)'?", "id": "Contoh Lain Teknik RET." },
  { "en": "Apa Itu 'Layout-Aware Design'?", "id": "Desain Yang Memperhitungkan Efek Tata Letak." },
  { "en": "Apa Itu 'Antenna Effect'?", "id": "Kerusakan Gerbang Akibat Pengumpulan Muatan." },
  { "en": "Apa Itu 'Antenna Rule Check (ARC)'?", "id": "Pemeriksaan Untuk Mencegah Efek Antena." },
  { "en": "Apa Itu 'Electromigration'?", "id": "Perpindahan Atom Logam Akibat Aliran Elektron." },
  { "en": "Apa Itu 'Stress Migration'?", "id": "Perpindahan Atom Akibat Stres Mekanis." },
  { "en": "Apa Itu 'Time-Dependent Dielectric Breakdown (TDDB)'?", "id": "Kegagalan Isolator Seiring Waktu." },
  { "en": "Apa Itu 'Hot Carrier Injection (HCI)'?", "id": "Degradasi Transistor Akibat Pembawa Panas." },
  { "en": "Apa Itu 'Delayering'?", "id": "Proses Mengupas Lapisan Chip Untuk Analisis." },
  { "en": "Mengapa 'Gettering' Penting?", "id": "Menyingkirkan Kontaminan Logam Perusak." },
  { "en": "Apa Beda Inspeksi 'Brightfield' Dan 'Darkfield'?", "id": "Brightfield Deteksi Kontras, Darkfield Deteksi Hamburan." },
  { "en": "Apa Fungsi 'FIB (Focused Ion Beam)'?", "id": "Memotong Dan Pencitraan Skala Nano." },
  { "en": "Bagaimana Cara Membuat 'Filter BAW (Bulk Acoustic Wave)'?", "id": "Membuat Resonator Akustik Di Dalam Chip." },
  { "en": "Apa Itu 'Gowning Procedure'?", "id": "Prosedur Mengenakan Pakaian Ruang Bersih." },
  { "en": "Apa Fungsi 'Air Shower'?", "id": "Menghilangkan Partikel Debu Dari Pakaian." },
  { "en": "Apa Itu 'Rheologi' Pasta Solder?", "id": "Studi Sifat Aliran Pasta Solder." },
  { "en": "Apa Itu 'Vapor Phase Soldering'?", "id": "Solder Menggunakan Uap Cairan Inert Panas." },
  { "en": "Apa Jenis 'Conformal Coating'?", "id": "Akrilik, Silikon, Uretan." },
  { "en": "Bagaimana 'Potensiometer' Dibuat?", "id": "Dengan Jalur Resistif Dan Kontak Geser." },
  { "en": "Apa Itu 'Substrate Engineering'?", "id": "Memodifikasi Substrat Untuk Kinerja Lebih Baik." },
  { "en": "Apa Itu 'Wafer Dicing' Menggunakan Laser?", "id": "Memotong Wafer Dengan Sinar Laser Presisi." },
  { "en": "Apa Itu 'Thermal Compression Bonding'?", "id": "Menyambung Chip Menggunakan Panas Dan Tekanan." },
  { "en": "Bagaimana 'Cermin MEMS' Dibuat?", "id": "Dengan Surface Micromachining." },
  { "en": "Apa Fungsi 'Cermin MEMS'?", "id": "Mengarahkan Sinar Laser Dengan Cepat." },
  { "en": "Apa Itu 'Dielectric Constant'?", "id": "Kemampuan Bahan Menyimpan Energi Listrik." },
  { "en": "Mengapa 'Dielektrik Low-k' Digunakan?", "id": "Mengurangi Tundaan Sinyal Antar Jalur." },
  { "en": "Apa Itu 'Copper Pillar Bump'?", "id": "Tonjolan Tembaga (Cu) Untuk Koneksi Flip-Chip." },
  { "en": "Apa Keuntungan 'Copper Pillar'?", "id": "Kinerja Listrik Dan Termal Lebih Baik." },
  { "en": "Apa Itu 'Reliability Testing'?", "id": "Pengujian Ketahanan Komponen Seiring Waktu." },
  { "en": "Apa Kepanjangan 'HTOL (High Temperature Operating Life)'?", "id": "Uji Umur Operasi Suhu Tinggi." },
  { "en": "Apa Itu 'Thermal Shock Test'?", "id": "Pengujian Dengan Perubahan Suhu Ekstrem." },
  { "en": "Apa Itu 'Vibration Test'?", "id": "Pengujian Ketahanan Terhadap Getaran Mekanis." },
  { "en": "Apa Itu 'Proses Manufaktur CMOS'?", "id": "Langkah-langkah Membuat Chip CMOS." },
  { "en": "Apa Itu 'Litho-Etch-Litho-Etch (LELE)'?", "id": "Teknik Double Patterning Untuk Skala Kecil." },
  { "en": "Apa Itu 'Self-Aligned Double Patterning (SADP)'?", "id": "Teknik Double Patterning Lainnya." },
  { "en": "Mengapa 'Double Patterning' Diperlukan?", "id": "Untuk Membuat Fitur Lebih Kecil." },
  { "en": "Apa Itu 'Optical Lithography'?", "id": "Nama Lain Untuk Fotolitografi." },
  { "en": "Apa Itu 'E-beam Lithography'?", "id": "Litografi Tanpa Masker Menggunakan Berkas Elektron." },
  { "en": "Apa Kelemahan 'E-beam Lithography'?", "id": "Sangat Lambat, Tidak Untuk Produksi Massal." },
  { "en": "Apa Itu 'Nanoimprint Lithography (NIL)'?", "id": "Mencetak Pola Nano Seperti Stempel." },
  { "en": "Apa Itu 'Molecular Imprinting'?", "id": "Membuat Cetakan Molekuler Di Polimer." },
  { "en": "Apa Itu 'Sol-Gel Process'?", "id": "Metode Kimia Membuat Oksida Logam." },
  { "en": "Bagaimana 'Kaca' Dibuat?", "id": "Mendinginkan Cairan Silika Dengan Cepat." },
  { "en": "Apa Itu 'Tempering' Kaca?", "id": "Proses Pemanasan-Pendinginan Untuk Perkuat Kaca." },
  { "en": "Bagaimana 'LED (Light Emitting Diode) Inframerah' Dibuat?", "id": "Menggunakan Bahan Seperti Galium Arsenida (GaAs)." },
  { "en": "Bagaimana 'Sensor UV (UltraViolet)' Dibuat?", "id": "Menggunakan Semikonduktor Celah Pita Lebar." },
  { "en": "Bahan Apa Untuk 'Sensor UV'?", "id": "Aluminium Galium Nitrida (AlGaN)." },
  { "en": "Apa Itu 'Photomultiplier Tube (PMT)'?", "id": "Detektor Cahaya Sangat Sensitif." },
  { "en": "Bagaimana 'PMT' Dibuat?", "id": "Merakit Fotokatoda Dan Dynode Dalam Tabung Vakum." },
  { "en": "Apa Itu 'Fotokatoda'?", "id": "Bahan Yang Melepas Elektron Saat Kena Cahaya." },
  { "en": "Apa Itu 'Dynode'?", "id": "Elektroda Yang Melipatgandakan Elektron." },
  { "en": "Apa Itu 'Scintillator'?", "id": "Bahan Yang Berpendar Saat Kena Radiasi." },
  { "en": "Bagaimana 'Detektor Scintillation' Dibuat?", "id": "Menggabungkan Scintillator Dengan Photodetector." },
  { "en": "Bagaimana 'Tabung Geiger-Müller' Dibuat?", "id": "Tabung Diisi Gas Dengan Elektroda." },
  { "en": "Apa Itu 'Semikonduktor Organik'?", "id": "Semikonduktor Berbasis Molekul Organik." },
  { "en": "Bagaimana 'Transistor Organik (OFET)' Dibuat?", "id": "Deposisi Lapisan Organik Pada Substrat." },
  { "en": "Apa Itu 'Deposisi Vakum Termal'?", "id": "Menguapkan Material Dalam Vakum Tinggi." },
  { "en": "Apa Itu 'Doctor Blading'?", "id": "Teknik Melapisi Cairan Dengan Ketebalan Tepat." },
  { "en": "Bagaimana 'Sel Surya Organik' Dibuat?", "id": "Dengan Melapisi Polimer Donor-Akseptor." },
  { "en": "Apa Itu 'Fullerene'?", "id": "Molekul Karbon Seperti Bola (C60)." },
  { "en": "Apa Peran 'Fullerene' Dalam Sel Surya?", "id": "Sebagai Material Akseptor Elektron." },
  { "en": "Apa Itu 'Perovskite'?", "id": "Material Kristal Dengan Struktur Tertentu." },
  { "en": "Bagaimana 'Sel Surya Perovskite' Dibuat?", "id": "Melalui Pelapisan Larutan (Solution Coating)." },
  { "en": "Apa Itu 'Slot-Die Coating'?", "id": "Teknik Pelapisan Presisi Skala Besar." },
  { "en": "Apa Itu 'Manufaktur Elektronik Cetak'?", "id": "Mencetak Sirkuit Menggunakan Printer Khusus." },
  { "en": "Apa Itu 'Gravure Printing'?", "id": "Teknik Cetak Untuk Volume Sangat Tinggi." },
  { "en": "Apa Itu 'Flexographic Printing'?", "id": "Teknik Cetak Fleksibel Seperti Stempel." },
  { "en": "Apa Itu 'Aerosol Jet Printing'?", "id": "Mencetak Garis Sangat Halus Dengan Aerosol." },
  { "en": "Apa Itu 'Sintering Fotonik'?", "id": "Sintering Tinta Logam Dengan Kilatan Cahaya." },
  { "en": "Bagaimana 'Transduser Piezoelektrik' Dibuat?", "id": "Sintering Keramik Dan Proses Poling." },
  { "en": "Apa Itu 'Poling'?", "id": "Menyelaraskan Domain Dipol Dengan Medan Listrik." },
  { "en": "Bagaimana 'Kapasitor Variabel (Varco)' Dibuat?", "id": "Merakit Set Pelat Stator Dan Rotor." },
  { "en": "Bagaimana 'Relay Elektromekanis' Dibuat?", "id": "Merakit Koil, Inti Besi, Dan Kontak." },
  { "en": "Apa Itu 'Armature' Relay?", "id": "Bagian Bergerak Yang Mengoperasikan Kontak." },
  { "en": "Bagaimana 'Motor DC' Dibuat?", "id": "Merakit Stator, Rotor, Komutator, Sikat." },
  { "en": "Apa Itu 'Komutator'?", "id": "Saklar Putar Pembalik Arah Arus." },
  { "en": "Apa Itu 'Sikat (Brush)' Motor?", "id": "Kontak Karbon Yang Menyuplai Arus." },
  { "en": "Bagaimana 'Transformer' Dibuat?", "id": "Melilitkan Kumparan Kawat Pada Inti Besi." },
  { "en": "Apa Itu 'Inti Laminasi'?", "id": "Inti Besi Dari Tumpukan Pelat Tipis." },
  { "en": "Mengapa Inti Trafo Perlu Dilaminasi?", "id": "Untuk Mengurangi Kerugian Arus Eddy." },
  { "en": "Bagaimana 'Kabel Listrik' Dibuat?", "id": "Menarik Konduktor Dan Melapisinya Isolasi." },
  { "en": "Apa Itu 'Stranding' Kabel?", "id": "Menggabungkan Banyak Kawat Kecil." },
  { "en": "Mengapa Kabel Dibuat 'Stranded'?", "id": "Agar Lebih Fleksibel." },
  { "en": "Apa Itu 'Die Drawing'?", "id": "Proses Mengecilkan Diameter Kawat." },
  { "en": "Apa Itu 'Annealing'?", "id": "Proses Pemanasan Untuk Melunakkan Logam." },
  { "en": "Apa Itu 'Extrusion' Isolasi?", "id": "Melapisi Kawat Dengan Plastik Meleleh." },
  { "en": "Bagaimana 'Steker Listrik' Dibuat?", "id": "Stamping Pin Logam Dan Cetak Plastik." },
  { "en": "Bagaimana 'Papan Breadboard' Dibuat?", "id": "Merakit Klip Logam Dalam Blok Plastik." },
  { "en": "Bagaimana 'Multimeter Digital' Dirakit?", "id": "Merakit PCB, Layar, Saklar, Probe." },
  { "en": "Bagaimana 'Osiloskop' Dirakit?", "id": "Merakit Sirkuit Kompleks Dan Tabung CRT/Layar." },
  { "en": "Apa Kepanjangan 'CRT (Cathode Ray Tube)'?", "id": "Tabung Sinar Katoda." },
  { "en": "Bagaimana 'Tabung CRT' Dibuat?", "id": "Merakit Senapan Elektron Dalam Tabung Kaca." },
  { "en": "Apa Itu 'Layar Fosfor'?", "id": "Lapisan Yang Berpendar Saat Ditembak Elektron." },
  { "en": "Bagaimana 'Magnetron' Dibuat?", "id": "Membuat Rongga Resonan Dan Katoda." },
  { "en": "Apa Fungsi 'Magnetron'?", "id": "Menghasilkan Gelombang Mikro Daya Tinggi." },
  { "en": "Bagaimana 'Klystron' Bekerja?", "id": "Menguatkan Sinyal RF Dengan Modulasi Kecepatan." },
  { "en": "Bagaimana 'Tabung Gelombang Berjalan (TWT)' Bekerja?", "id": "Interaksi Berkelanjutan Antara Berkas-Gelombang." },
  { "en": "Apa Kepanjangan 'TWT (Traveling-Wave Tube)'?", "id": "Tabung Gelombang Berjalan." },
  { "en": "Bagaimana 'Kabel Serat Optik' Dibuat?", "id": "Melapisi Serat Kaca Dengan Pelindung." },
  { "en": "Apa Itu 'Buffer' Kabel Optik?", "id": "Lapisan Plastik Pelindung Serat." },
  { "en": "Apa Itu 'Kevlar' Dalam Kabel?", "id": "Benang Kuat Untuk Kekuatan Tarik." },
  { "en": "Apa Itu 'Jaket' Kabel?", "id": "Lapisan Pelindung Terluar Kabel." },
  { "en": "Bagaimana 'Konektor Serat Optik' Dipasang?", "id": "Dengan Polishing Presisi Dan Penyambungan." },
  { "en": "Apa Itu 'Fusion Splicing'?", "id": "Menyambung Serat Optik Dengan Meleburnya." },
  { "en": "Apa Itu 'Mechanical Splicing'?", "id": "Menyambung Serat Optik Secara Mekanis." },
  { "en": "Apa Beda 'Verifikasi' Dan 'Validasi'?", "id": "Verifikasi Cek Desain, Validasi Cek Produk." },
  { "en": "Apa Itu 'Karakterisasi Proses'?", "id": "Mengukur Parameter Statistik Proses Fabrikasi." },
  { "en": "Apa Itu 'Pojok Proses (Process Corner)'?", "id": "Kondisi Ekstrem Parameter Proses." },
  { "en": "Contoh 'Pojok Proses'?", "id": "Cepat, Lambat, Tipikal." },
  { "en": "Apa Itu 'Variabilitas' Dalam Fabrikasi?", "id": "Perbedaan Karakteristik Antar Chip." },
  { "en": "Apa Itu 'Lot-to-Lot Variability'?", "id": "Variasi Antara Lot Produksi Berbeda." },
  { "en": "Apa Itu 'Wafer-to-Wafer Variability'?", "id": "Variasi Antara Wafer Dalam Satu Lot." },
  { "en": "Apa Itu 'Die-to-Die Variability'?", "id": "Variasi Antara Die Dalam Satu Wafer." },
  { "en": "Apa Itu 'Desain Toleran Variasi'?", "id": "Desain Yang Tahan Terhadap Variasi Proses." },
  { "en": "Apa Itu 'Desain Statistik'?", "id": "Desain Yang Mempertimbangkan Variabilitas Statistik." },
  { "en": "Apa Kepanjangan 'DFM (Design for Manufacturability)'?", "id": "Desain Untuk Kemudahan Manufaktur." },
  { "en": "Apa Tujuan 'DFM (Design for Manufacturability)'?", "id": "Meningkatkan Yield Dan Mengurangi Biaya." },
  { "en": "Apa Kepanjangan 'DFY (Design for Yield)'?", "id": "Desain Untuk Yield." },
  { "en": "Apa Kepanjangan 'DFT (Design for Testability)'?", "id": "Desain Untuk Kemudahan Pengujian." },
  { "en": "Apa Itu 'Scan Chain'?", "id": "Teknik DFT (Design for Testability) Mengubah Flip-flop." },
  { "en": "Apa Kepanjangan 'BIST (Built-In Self-Test)'?", "id": "Uji Mandiri Tertanam." },
  { "en": "Apa Fungsi 'BIST (Built-In Self-Test)'?", "id": "Memungkinkan Chip Menguji Dirinya Sendiri." },
  { "en": "Apa Itu 'Model Kesalahan (Fault Model)'?", "id": "Model Abstrak Kerusakan Fisik." },
  { "en": "Contoh 'Model Kesalahan'?", "id": "Stuck-at, Bridging, Open." },
  { "en": "Apa Itu 'Kesalahan Stuck-at'?", "id": "Asumsi Jalur Terjebak Di Logika 0/1." },
  { "en": "Apa Itu 'Cakupan Kesalahan (Fault Coverage)'?", "id": "Persentase Kesalahan Yang Dapat Dideteksi." },
  { "en": "Apa Kepanjangan 'ATPG (Automatic Test Pattern Generation)'?", "id": "Pembangkitan Pola Uji Otomatis." },
  { "en": "Apa Itu 'Manufaktur Tanpa Cacat (Zero Defect)'?", "id": "Filosofi Manufaktur Untuk Kualitas Sempurna." },
  { "en": "Apa Itu 'Six Sigma'?", "id": "Metodologi Peningkatan Kualitas Proses." },
  { "en": "Apa Itu 'Kontrol Proses Statistik (SPC)'?", "id": "Menggunakan Statistik Untuk Memantau Proses." },
  { "en": "Apa Kepanjangan 'SPC (Statistical Process Control)'?", "id": "Kontrol Proses Statistik." },
  { "en": "Apa Itu 'Peta Kontrol (Control Chart)'?", "id": "Grafik Untuk Memantau Variasi Proses." },
  { "en": "Apa Itu 'Batas Kontrol Atas (UCL)'?", "id": "Batas Atas Variasi Normal." },
  { "en": "Apa Itu 'Batas Kontrol Bawah (LCL)'?", "id": "Batas Bawah Variasi Normal." },
  { "en": "Apa Itu 'Kemampuan Proses (Process Capability)'?", "id": "Kemampuan Proses Memenuhi Spesifikasi." },
  { "en": "Apa Kepanjangan 'Cpk'?", "id": "Indeks Kemampuan Proses." },
  { "en": "Apa Itu 'Analisis Akar Masalah (RCA)'?", "id": "Metode Mencari Penyebab Utama Masalah." },
  { "en": "Apa Kepanjangan 'RCA (Root Cause Analysis)'?", "id": "Analisis Akar Masalah." },
  { "en": "Apa Itu 'Diagram Tulang Ikan'?", "id": "Alat RCA (Root Cause Analysis) (Ishikawa)." },
  { "en": "Apa Itu 'Analisis 5 Mengapa (5 Whys)'?", "id": "Teknik RCA (Root Cause Analysis) Bertanya 'Mengapa'." },
  { "en": "Apa Kepanjangan 'FMEA (Failure Mode and Effects Analysis)'?", "id": "Analisis Mode Kegagalan Dan Efeknya." },
  { "en": "Apa Tujuan 'FMEA'?", "id": "Mengidentifikasi Potensi Kegagalan Secara Proaktif." },
  { "en": "Apa Itu 'Wafer Bumping'?", "id": "Proses Menambahkan Tonjolan Solder Ke Wafer." },
  { "en": "Apa Itu 'Under Bump Metallurgy (UBM)'?", "id": "Lapisan Logam Di Bawah Tonjolan Solder." },
  { "en": "Apa Fungsi 'UBM (Under Bump Metallurgy)'?", "id": "Sebagai Lapisan Adhesi Dan Penghalang." },
  { "en": "Apa Itu 'Pengemasan Tingkat Wafer (WLP)'?", "id": "Proses Pengemasan Dilakukan Di Tingkat Wafer." },
  { "en": "Apa Kepanjangan 'WLP (Wafer-Level Packaging)'?", "id": "Pengemasan Tingkat Wafer." },
  { "en": "Apa Itu 'Fan-In WLP'?", "id": "Koneksi I/O (Input/Output) Berada Di Dalam Area Die." },
  { "en": "Apa Itu 'Fan-Out WLP (FOWLP)'?", "id": "Koneksi I/O (Input/Output) Di Luar Area Die." },
  { "en": "Apa Keuntungan 'FOWLP'?", "id": "Jumlah Pin Lebih Banyak, Kinerja Lebih Baik." },
  { "en": "Apa Itu 'Lapisan Redistribusi (RDL)'?", "id": "Jalur Logam Tambahan Untuk Merutekan Sinyal." },
  { "en": "Apa Kepanjangan 'RDL (Redistribution Layer)'?", "id": "Lapisan Redistribusi." },
  { "en": "Apa Itu 'Integrasi 2.5D'?", "id": "Menumpuk Chip Di Atas Interposer Silikon." },
  { "en": "Apa Itu 'Interposer'?", "id": "Lapisan Penghubung Antara Chip Dan Substrat." },
  { "en": "Apa Itu 'Integrasi 3D-IC'?", "id": "Menumpuk Beberapa Chip Secara Vertikal." },
  { "en": "Apa Itu 'Die Stacking'?", "id": "Proses Menumpuk Die Satu Sama Lain." },
  { "en": "Apa Itu 'Wafer Thinning'?", "id": "Proses Menipiskan Wafer." },
  { "en": "Mengapa 'Wafer Thinning' Diperlukan?", "id": "Untuk Tumpukan 3D-IC Dan Paket Tipis." },
  { "en": "Apa Itu 'Wafer Bonding'?", "id": "Proses Menyambungkan Dua Wafer." },
  { "en": "Apa Itu 'Anodic Bonding'?", "id": "Penyambungan Kaca Ke Silikon Dengan Listrik." },
  { "en": "Apa Itu 'Fusion Bonding'?", "id": "Penyambungan Langsung Antar Wafer Mulus." },
  { "en": "Apa Itu 'Sistem Tertanam (Embedded System)'?", "id": "Sistem Komputer Dengan Fungsi Khusus." },
  { "en": "Apa Itu 'Firmware'?", "id": "Perangkat Lunak Yang Tertanam Di Perangkat Keras." },
  { "en": "Apa Itu 'Bootloader'?", "id": "Program Pertama Yang Dijalankan Saat Startup." },
  { "en": "Apa Itu 'Sistem Operasi Waktu Nyata (RTOS)'?", "id": "OS (Operating System) Untuk Aplikasi Responsif Waktu." },
  { "en": "Apa Kepanjangan 'RTOS (Real-Time Operating System)'?", "id": "Sistem Operasi Waktu Nyata." },
  { "en": "Apa Itu 'Watchdog Timer'?", "id": "Pewaktu Untuk Mereset Sistem Jika Macet." },
  { "en": "Apa Itu 'Mode Tidur (Sleep Mode)'?", "id": "Mode Operasi Hemat Daya." },
  { "en": "Apa Itu 'Arus Tidur (Sleep Current)'?", "id": "Arus Yang Dikonsumsi Saat Mode Tidur." },
  { "en": "Apa Itu 'Waktu Bangun (Wake-up Time)'?", "id": "Waktu Untuk Kembali Dari Mode Tidur." },
  { "en": "Apa Itu 'Brown-Out Detector (BOD)'?", "id": "Mendeteksi Jika Tegangan Suplai Turun." },
  { "en": "Apa Kepanjangan 'BOD (Brown-Out Detector)'?", "id": "Detektor Brown-Out." },
  { "en": "Apa Itu 'Power Gating'?", "id": "Teknik Mematikan Daya Ke Blok Tidak Aktif." },
  { "en": "Apa Itu 'Clock Gating'?", "id": "Teknik Mematikan Clock Ke Blok Tidak Aktif." },
  { "en": "Apa Itu 'Penskalaan Tegangan Dan Frekuensi Dinamis'?", "id": "Menyesuaikan Tegangan-Frekuensi Sesuai Beban." },
  { "en": "Apa Kepanjangan 'DVFS'?", "id": "Dynamic Voltage and Frequency Scaling." },
  { "en": "Apa Itu 'Arus Inrush'?", "id": "Lonjakan Arus Awal Saat Perangkat Dinyalakan." },
  { "en": "Apa Itu 'Hot Swap'?", "id": "Memasang Atau Melepas Komponen Saat Sistem Nyala." },
  { "en": "Apa Itu 'Siklus Daya (Power Cycling)'?", "id": "Mematikan Dan Menyalakan Kembali Perangkat." },
  { "en": "Apa Itu 'Urutan Daya (Power Sequencing)'?", "id": "Urutan Menyalakan Berbagai Rel Daya." },
  { "en": "Mengapa 'Urutan Daya' Penting?", "id": "Untuk Mencegah Kerusakan Pada Chip Kompleks." },
  { "en": "Apa Itu 'Desain Referensi'?", "id": "Desain Sirkuit Lengkap Yang Sudah Teruji." },
  { "en": "Apa Itu 'Papan Evaluasi'?", "id": "Papan Sirkuit Untuk Mengevaluasi Chip Tertentu." },
  { "en": "Apa Itu 'Kit Pengembangan Perangkat Lunak (SDK)'?", "id": "Kumpulan Alat Untuk Pengembangan Perangkat Lunak." },
  { "en": "Apa Kepanjangan 'SDK (Software Development Kit)'?", "id": "Kit Pengembangan Perangkat Lunak." },
  { "en": "Apa Itu 'Lingkungan Pengembangan Terpadu (IDE)'?", "id": "Aplikasi Perangkat Lunak Untuk Pemrograman." },
  { "en": "Apa Kepanjangan 'IDE (Integrated Development Environment)'?", "id": "Lingkungan Pengembangan Terpadu." },
  { "en": "Apa Itu 'Debugger In-Circuit (ICD)'?", "id": "Alat Untuk Debugging Mikrokontroler." },
  { "en": "Apa Kepanjangan 'ICD (In-Circuit Debugger)'?", "id": "Debugger In-Circuit." },
  { "en": "Apa Itu 'Emulator In-Circuit (ICE)'?", "id": "Alat Debugging Yang Menggantikan Prosesor." },
  { "en": "Apa Kepanjangan 'ICE (In-Circuit Emulator)'?", "id": "Emulator In-Circuit." },
  { "en": "Apa Itu 'Analisis Logika'?", "id": "Merekam Dan Menganalisis Banyak Sinyal Digital." },
  { "en": "Apa Itu 'Pembangkit Sinyal Sewenang-wenang (AWG)'?", "id": "Alat Pembangkit Bentuk Gelombang Kustom." },
  { "en": "Apa Kepanjangan 'AWG (Arbitrary Waveform Generator)'?", "id": "Pembangkit Sinyal Sewenang-wenang." },
  { "en": "Apa Itu 'Bus Antarmuka Tujuan Umum (GPIB)'?", "id": "Standar Bus Untuk Kontrol Instrumen." },
  { "en": "Apa Kepanjangan 'GPIB (General Purpose Interface Bus)'?", "id": "Bus Antarmuka Tujuan Umum." },
  { "en": "Apa Itu 'LXI'?", "id": "Standar Kontrol Instrumen Berbasis LAN." },
  { "en": "Apa Itu 'PXI'?", "id": "Platform Instrumen Modular Berbasis PCI." },
  { "en": "Apa Itu 'SCPI'?", "id": "Bahasa Perintah Standar Untuk Instrumen." },
  { "en": "Apa Itu 'Kalibrasi'?", "id": "Menyesuaikan Instrumen Agar Sesuai Standar." },
  { "en": "Apa Itu 'Ketertelusuran (Traceability)'?", "id": "Menghubungkan Pengukuran Ke Standar Nasional." },
  { "en": "Apa Itu 'Akurasi'?", "id": "Kedekatan Pengukuran Dengan Nilai Sebenarnya." },
  { "en": "Apa Itu 'Presisi'?", "id": "Kedekatan Pengukuran Berulang Satu Sama Lain." },
  { "en": "Apa Itu 'Verifikasi Formal'?", "id": "Membuktikan Kebenaran Desain Secara Matematis." },
  { "en": "Apa Kepanjangan 'ABV (Assertion-Based Verification)'?", "id": "Verifikasi Berbasis Pernyataan." },
  { "en": "Apa Kepanjangan 'UVM (Universal Verification Methodology)'?", "id": "Metodologi Verifikasi Universal." },
  { "en": "Apa Itu 'Pustaka Sel Standar (Standard Cell Library)'?", "id": "Kumpulan Gerbang Logika Dasar." },
  { "en": "Apa Itu 'Pustaka I/O (I/O Library)'?", "id": "Kumpulan Sel Input/Output." },
  { "en": "Apa Itu 'Kompilator Memori (Memory Compiler)'?", "id": "Alat Penghasil Layout Memori Kustom." },
  { "en": "Apa Beda 'Sintesis Logika' Dan 'Fisik'?", "id": "Fisik Memperhitungkan Tata Letak." },
  { "en": "Apa Itu 'Datapath'?", "id": "Jalur Aliran Data Dalam Prosesor." },
  { "en": "Apa Itu 'Unit Kontrol (Control Unit)'?", "id": "Mengatur Operasi Datapath." },
  { "en": "Apa Itu 'Operasi Mikro (Micro-operation)'?", "id": "Operasi Elementer Yang Dilakukan CPU." },
  { "en": "Apa Itu 'Register Status'?", "id": "Register Yang Menyimpan Bendera (Flag)." },
  { "en": "Apa Itu 'Bendera Carry (Carry Flag)'?", "id": "Indikator Terjadinya Carry Out Aritmetika." },
  { "en": "Apa Itu 'Bendera Nol (Zero Flag)'?", "id": "Indikator Hasil Operasi Adalah Nol." },
  { "en": "Apa Itu 'Bendera Tanda (Sign Flag)'?", "id": "Indikator Hasil Operasi Adalah Negatif." },
  { "en": "Apa Itu 'Bendera Overflow (Overflow Flag)'?", "id": "Indikator Kesalahan Aritmetika Bertanda." },
  { "en": "Apa Itu 'Siklus Instruksi'?", "id": "Urutan Fetch-Decode-Execute." },
  { "en": "Apa Itu 'Kontroler Interrupt'?", "id": "Sirkuit Pengelola Permintaan Interrupt." },
  { "en": "Apa Itu 'Penyamaran Interrupt (Interrupt Masking)'?", "id": "Proses Menonaktifkan Interrupt Tertentu." },
  { "en": "Apa Kepanjangan 'NMI (Non-Maskable Interrupt)'?", "id": "Interrupt Yang Tidak Dapat Disamarkan." },
  { "en": "Apa Itu 'Arbitrasi Bus'?", "id": "Proses Menentukan Master Bus Selanjutnya." },
  { "en": "Apa Itu 'Bus Sinkron'?", "id": "Transfer Data Diatur Oleh Sinyal Clock." },
  { "en": "Apa Itu 'Bus Asinkron'?", "id": "Transfer Data Diatur Sinyal Handshake." },
  { "en": "Apa Itu 'Lebar Bus'?", "id": "Jumlah Bit Paralel Yang Ditransfer." },
  { "en": "Apa Itu 'I/O (Input/Output) Terisolasi'?", "id": "Ruang Alamat Terpisah Untuk I/O." },
  { "en": "Apa Itu 'I/O (Input/Output) Terpetakan Memori'?", "id": "Berbagi Ruang Alamat Dengan Memori." },
  { "en": "Apa Itu 'Kisi Kristal (Crystal Lattice)'?", "id": "Susunan Atom Periodik Dalam Kristal." },
  { "en": "Apa Itu 'Sel Satuan (Unit Cell)'?", "id": "Unit Berulang Terkecil Dari Kisi." },
  { "en": "Apa Itu 'Indeks Miller'?", "id": "Notasi Untuk Bidang Dalam Kristal." },
  { "en": "Apa Itu 'Generasi Pembawa'?", "id": "Penciptaan Pasangan Elektron-Lubang." },
  { "en": "Apa Itu 'Model Drift-Diffusion'?", "id": "Model Matematika Aliran Arus Semikonduktor." },
  { "en": "Apa Itu 'Kapasitansi Sambungan (Junction Capacitance)'?", "id": "Kapasitansi Daerah Deplesi." },
  { "en": "Apa Itu 'Kapasitansi Difusi'?", "id": "Kapasitansi Akibat Injeksi Pembawa Muatan." },
  { "en": "Apa Itu 'Polisilicon'?", "id": "Silikon Polikristalin Yang Digunakan Untuk Gerbang." },
  { "en": "Apa Itu 'Semikonduktor III-V'?", "id": "Senyawa Dari Grup III Dan V." },
  { "en": "Contoh 'Semikonduktor III-V'?", "id": "Galium Arsenida (GaAs)." },
  { "en": "Apa Kepanjangan 'CFA (Current Feedback Amplifier)'?", "id": "Penguat Umpan Balik Arus." },
  { "en": "Apa Itu 'Kesalahan Kuantisasi (Quantization Error)'?", "id": "Kesalahan Pembulatan Dalam Konversi Analog-Digital." },
  { "en": "Apa Itu 'Laju Sampling (Sampling Rate)'?", "id": "Seberapa Sering Sinyal Analog Diukur." },
  { "en": "Apa Itu 'Teorema Nyquist'?", "id": "Laju Sampling Harus Dua Kali Bandwidth." },
  { "en": "Apa Itu 'Modulasi Sigma-Delta'?", "id": "Teknik Untuk Konverter Data Resolusi Tinggi." },
  { "en": "Apa Itu 'Filter Pasif'?", "id": "Filter Yang Hanya Menggunakan R, L, C." },
  { "en": "Apa Itu 'Filter Aktif'?", "id": "Filter Yang Menggunakan Komponen Aktif." },
  { "en": "Contoh Komponen Filter Aktif?", "id": "Op-Amp (Operational Amplifier) Atau Transistor." },
  { "en": "Apa Itu 'Filter Butterworth'?", "id": "Filter Dengan Respons Frekuensi Paling Datar." },
  { "en": "Apa Itu 'Filter Chebyshev'?", "id": "Filter Dengan Transisi Lebih Tajam." },
  { "en": "Apa Itu 'Topologi Sallen-Key'?", "id": "Topologi Umum Untuk Filter Aktif." },
  { "en": "Apa Itu 'Tegangan Saturasi'?", "id": "Tegangan Vce Saat Transistor Jenuh." },
  { "en": "Apa Itu 'Arus Cutoff'?", "id": "Arus Bocor Saat Transistor Mati." },
  { "en": "Apa Itu 'Penskalaan Dennard'?", "id": "Prinsip Penskalaan Transistor MOSFET." },
  { "en": "Apa Itu 'Efek Kanal Pendek'?", "id": "Perilaku Non-ideal Akibat Penskalaan." },
  { "en": "Apa Kepanjangan 'DIBL (Drain-Induced Barrier Lowering)'?", "id": "Penurunan Penghalang Akibat Drain." },
  { "en": "Apa Itu 'Saturasi Kecepatan'?", "id": "Kecepatan Pembawa Muatan Mencapai Batasnya." },
  { "en": "Apa Itu 'Oksida Gerbang Tipis'?", "id": "Lapisan Isolator Tipis Di Bawah Gerbang." },
  { "en": "Apa Itu 'Arus Bocor Gerbang'?", "id": "Arus Yang Menerowong Melalui Oksida." },
  { "en": "Apa Itu 'Dielektrik High-k'?", "id": "Isolator Gerbang Pengganti Oksida." },
  { "en": "Tujuan 'Dielektrik High-k'?", "id": "Mengurangi Arus Bocor Gerbang." },
  { "en": "Apa Itu 'Gerbang Logam (Metal Gate)'?", "id": "Mengganti Gerbang Polisilicon Dengan Logam." },
  { "en": "Apa Itu 'Silikon Terasah (Strained Silicon)'?", "id": "Meningkatkan Mobilitas Dengan Meregangkan Kisi." },
  { "en": "Apa Kepanjangan 'SOI (Silicon-on-Insulator)'?", "id": "Silikon Di Atas Isolator." },
  { "en": "Apa Keuntungan 'SOI (Silicon-on-Insulator)'?", "id": "Mengurangi Kapasitansi Parasit Dan Latch-up." },
  { "en": "Apa Itu 'Fotodioda'?", "id": "Dioda Yang Berfungsi Sebagai Sensor Cahaya." },
  { "en": "Apa Itu 'Sel Surya'?", "id": "Dioda Area Besar Untuk Ubah Cahaya." },
  { "en": "Apa Itu 'Dioda Zener'?", "id": "Dioda Untuk Referensi Tegangan." },
  { "en": "Apa Itu 'Dioda Schottky'?", "id": "Dioda Cepat Dengan Jatuh Tegangan Rendah." },
  { "en": "Apa Itu 'Dioda Varactor'?", "id": "Dioda Yang Kapasitansinya Dapat Diubah." },
  { "en": "Apa Itu 'Dioda PIN'?", "id": "Dioda Dengan Lapisan Intrinsik." },
  { "en": "Apa Itu 'Sirkuit Tangki LC'?", "id": "Resonator Menggunakan Induktor Dan Kapasitor." },
  { "en": "Apa Itu 'Resonator Kristal'?", "id": "Komponen Piezoelektrik Untuk Frekuensi Stabil." },
  { "en": "Apa Itu 'Resonator Keramik'?", "id": "Resonator Dengan Stabilitas Lebih Rendah." },
  { "en": "Apa Itu 'Mode Bersama (Common Mode)'?", "id": "Sinyal Yang Sama Pada Dua Jalur." },
  { "en": "Apa Itu 'Mode Diferensial'?", "id": "Sinyal Yang Berlawanan Pada Dua Jalur." },
  { "en": "Apa Itu 'Kabel Twisted Pair'?", "id": "Sepasang Kabel Yang Dipilin." },
  { "en": "Mengapa Kabel 'Twisted Pair' Digunakan?", "id": "Untuk Mengurangi Gangguan Elektromagnetik." },
  { "en": "Apa Itu 'Tegangan Offset Input'?", "id": "Tegangan DC (Direct Current) Kecil Di Input Op-Amp." },
  { "en": "Apa Itu 'Arus Bias Input'?", "id": "Arus DC (Direct Current) Kecil Mengalir Ke Input." },
  { "en": "Apa Kepanjangan 'CMRR (Common-Mode Rejection Ratio)'?", "id": "Rasio Penolakan Mode Bersama." },
  { "en": "Apa Kepanjangan 'PSRR (Power Supply Rejection Ratio)'?", "id": "Rasio Penolakan Catu Daya." },
  { "en": "Apa Itu 'Produk Gain-Bandwidth'?", "id": "Karakteristik Penguat Operasional." },
  { "en": "Apa Itu 'Derau 1/f'?", "id": "Derau Frekuensi Rendah (Pink Noise)." },
  { "en": "Apa Itu 'Derau Termal'?", "id": "Derau Akibat Agitasi Termal Elektron." },
  { "en": "Apa Itu 'Derau Tembak (Shot Noise)'?", "id": "Derau Akibat Sifat Diskrit Muatan." },
  { "en": "Apa Itu 'Angka Derau (Noise Figure)'?", "id": "Ukuran Degradasi SNR (Signal-to-Noise Ratio)." },
  { "en": "Apa Itu 'Distorsi Harmonik Total (THD)'?", "id": "Ukuran Distorsi Sinyal." },
  { "en": "Apa Itu 'Intermodulasi'?", "id": "Pencampuran Sinyal Yang Menghasilkan Frekuensi Baru." },
  { "en": "Apa Itu 'Titik Potong Orde Ketiga (IP3)'?", "id": "Ukuran Linieritas." },
  { "en": "Apa Itu 'Rentang Dinamis Bebas Spurious (SFDR)'?", "id": "Rentang Antara Sinyal Dan Derau Terkuat." },
  { "en": "Apa Itu 'Konverter Buck-Boost'?", "id": "Bisa Menaikkan Atau Menurunkan Tegangan." },
  { "en": "Apa Itu 'Kontrol Mode Arus'?", "id": "Metode Kontrol Untuk Regulator Switching." },
  { "en": "Apa Itu 'Kontrol Mode Tegangan'?", "id": "Metode Kontrol Regulator Switching Lain." },
  { "en": "Apa Itu 'Pengemasan Hermetik'?", "id": "Paket Kedap Udara." },
  { "en": "Mengapa 'Pengemasan Hermetik' Digunakan?", "id": "Untuk Keandalan Tinggi (Militer, Luar Angkasa)." },
  { "en": "Bahan Apa Untuk 'Paket Hermetik'?", "id": "Keramik Atau Logam." },
  { "en": "Apa Itu 'FinFET'?", "id": "Transistor Dengan Struktur Sirip (Fin)." },
  { "en": "Apa Itu 'Gate-All-Around (GAA)'?", "id": "Struktur Transistor Dengan Gerbang Mengelilingi." },
  { "en": "Apa Itu 'Keluaran Totem-Pole'?", "id": "Struktur Output Khas Pada Logika TTL." },
  { "en": "Apa Itu 'Gerbang AND-OR-Invert (AOI)'?", "id": "Gerbang Fungsional Kompleks." },
  { "en": "Apa Itu 'Array Memori'?", "id": "Susunan Sel Memori Dalam Baris-Kolom." },
  { "en": "Apa Itu 'Sintesis Tingkat Tinggi (HLS)'?", "id": "Membuat Desain IC (Integrated Circuit) Dari Bahasa C." },
  { "en": "Apa Itu 'Verifikasi Formal'?", "id": "Membuktikan Kebenaran Desain Secara Matematis." },
  { "en": "Apa Itu 'Clock Tree Synthesis (CTS)'?", "id": "Proses Membangun Jaringan Distribusi Clock." },
  { "en": "Tujuan 'CTS (Clock Tree Synthesis)'?", "id": "Meminimalkan Skew Dan Jitter." },
  { "en": "Apa Itu 'Sinkronisasi'?", "id": "Proses Penyelarasan Dengan Sinyal Clock." },
  { "en": "Apa Itu 'Sistem Digital Multilevel'?", "id": "Sistem Dengan Lebih Dari Dua Level Logika." },
  { "en": "Apa Itu 'Logika Fuzzy'?", "id": "Logika Yang Menangani Nilai Antara 0-1." },
  { "en": "Apa Itu 'Sirkuit Digital Optik'?", "id": "Sirkuit Yang Memproses Sinyal Cahaya." },
  { "en": "Apa Itu 'Clock Recovery'?", "id": "Mengekstrak Sinyal Clock Dari Data." },
  { "en": "Apa Itu 'Data Encoding'?", "id": "Mengubah Data Menjadi Format Transmisi." },
  { "en": "Apa Itu 'Kode Manchester'?", "id": "Encoding Yang Menggabungkan Clock Dan Data." },
  { "en": "Apa Itu 'Transceiver'?", "id": "Perangkat Pengirim (Transmitter) Dan Penerima (Receiver)." },
  { "en": "Apa Itu 'Sistem Toleran Kesalahan'?", "id": "Sistem Yang Tetap Bekerja Meski Ada Gagal." },
  { "en": "Apa Itu 'Redundansi'?", "id": "Menyediakan Komponen Cadangan." },
  { "en": "Apa Itu 'Redundansi Perangkat Keras'?", "id": "Menggunakan Komponen Fisik Cadangan." },
  { "en": "Apa Itu 'Redundansi Informasi'?", "id": "Menambahkan Bit Ekstra (Seperti Paritas)." },
  { "en": "Apa Itu 'Voting Logic'?", "id": "Mengambil Keputusan Berdasarkan Mayoritas." },
  { "en": "Apa Itu 'Sistem Failsafe'?", "id": "Sistem Yang Gagal Dalam Keadaan Aman." },
  { "en": "Apa Itu 'Latch Transparan'?", "id": "Output Mengikuti Input Saat Enable." },
  { "en": "Apa Itu 'Pengalamatan Memori'?", "id": "Metode Untuk Memilih Lokasi Memori." },
  { "en": "Apa Itu 'Peta Memori'?", "id": "Alokasi Alamat Untuk Perangkat Berbeda." },
  { "en": "Apa Itu 'Waktu Siklus (Cycle Time)'?", "id": "Periode Sinyal Clock." },
  { "en": "Apa Itu 'Laju Transfer Data'?", "id": "Jumlah Data Yang Dipindahkan Per Detik." },
  { "en": "Apa Satuan 'Laju Transfer Data'?", "id": "Bit Per Detik (bps)." },
  { "en": "Apa Itu 'Bandwidth'?", "id": "Kapasitas Maksimum Laju Transfer Data." },
  { "en": "Apa Itu 'Latensi'?", "id": "Waktu Tunda Awal Transfer Data." },
  { "en": "Apa Itu 'Multiplexing'?", "id": "Berbagi Satu Saluran Untuk Banyak Sinyal." },
  { "en": "Apa Itu 'Demultiplexing'?", "id": "Memisahkan Sinyal Gabungan Kembali." },
  { "en": "Apa Kepanjangan 'FDM (Frequency Division Multiplexing)'?", "id": "Multiplexing Pembagian Frekuensi." },
  { "en": "Apa Kepanjangan 'TDM (Time Division Multiplexing)'?", "id": "Multiplexing Pembagian Waktu." },
  { "en": "Apa Itu 'Antarmuka Digital'?", "id": "Titik Sambungan Antara Perangkat Digital." },
  { "en": "Contoh 'Antarmuka Digital'?", "id": "USB (Universal Serial Bus), HDMI, SPI, I2C." },
  { "en": "Apa Kepanjangan 'USB (Universal Serial Bus)'?", "id": "Bus Serial Universal." },
  { "en": "Apa Itu 'Sirkuit Digital Kustom'?", "id": "Sirkuit Yang Didesain Untuk Tugas Tertentu." },
  { "en": "Apa Itu 'ASIC (Application-Specific Integrated Circuit)'?", "id": "Contoh Sirkuit Digital Kustom." },
  { "en": "Apa Itu 'Prosesor Sinyal Digital (DSP)'?", "id": "Prosesor Khusus Untuk Pemrosesan Sinyal." },
  { "en": "Apa Itu 'Siklus Tugas 50%'?", "id": "Waktu Sinyal Tinggi Dan Rendah Sama." },
  { "en": "Apa Itu 'Rangkaian Logika Resistor-Transistor (RTL)'?", "id": "Keluarga Logika Awal Menggunakan Resistor-Transistor." },
  { "en": "Apa Itu 'Rangkaian Logika Dioda-Transistor (DTL)'?", "id": "Keluarga Logika Sebelum TTL." },
  { "en": "Apa Itu 'Rangkaian Logika Emitter-Coupled (ECL)'?", "id": "Keluarga Logika Bipolar Berkecepatan Sangat Tinggi." },
  { "en": "Apa Itu 'Active Low'?", "id": "Sinyal Aktif Saat Logika Rendah (0)." },
  { "en": "Apa Itu 'Active High'?", "id": "Sinyal Aktif Saat Logika Tinggi (1)." },
  { "en": "Apa Itu 'Finite Automata'?", "id": "Model Matematika Untuk Mesin Keadaan." },
  { "en": "Apa Beda 'Finite Automata' Dan 'State Machine'?", "id": "Konsep Yang Sama, Beda Terminologi." },
  { "en": "Apa Itu 'System Call'?", "id": "Cara Program Aplikasi Meminta Layanan OS." },
  { "en": "Bagaimana 'Register' Berbeda Dari 'Cache'?", "id": "Register Di Dalam, Cache Di Luar CPU." },
  { "en": "Apa Peran 'Linker' Dalam Kompilasi?", "id": "Menggabungkan File Objek Menjadi Executable." },
  { "en": "Apa Itu 'Siklus Fetch'?", "id": "Tahap Pengambilan Instruksi Dari Memori." },
  { "en": "Apa Itu 'Siklus Execute'?", "id": "Tahap Menjalankan Instruksi Oleh CPU." },
  { "en": "Apa Itu 'Datapath'?", "id": "Jalur Data Dan Komponen Fungsional CPU." },
  { "en": "Apa Itu 'Control Path'?", "id": "Sirkuit Yang Mengontrol Aliran Datapath." },
  { "en": "Apa Itu 'Sinyal Kontrol'?", "id": "Sinyal Yang Dihasilkan Oleh Unit Kontrol." },
  { "en": "Apa Itu 'Micro-operation'?", "id": "Operasi Elementer Yang Dilakukan CPU." },
  { "en": "Apa Itu 'Register Transfer Language (RTL)'?", "id": "Bahasa Simbolik Mendeskripsikan Transfer Register." },
  { "en": "Apa Itu 'Carry Flag'?", "id": "Indikator Terjadinya Carry Out." },
  { "en": "Apa Itu 'Zero Flag'?", "id": "Indikator Hasil Operasi Adalah Nol." },
  { "en": "Apa Itu 'Sign Flag'?", "id": "Indikator Hasil Operasi Adalah Negatif." },
  { "en": "Apa Itu 'Overflow Flag'?", "id": "Indikator Terjadinya Aritmetika Overflow." },
  { "en": "Apa Itu 'Status Register'?", "id": "Register Yang Menyimpan Flag." },
  { "en": "Nama Lain 'Status Register'?", "id": "Flag Register Atau CCR (Condition Code Register)." },
  { "en": "Apa Itu 'Instruksi Percabangan (Branch)'?", "id": "Instruksi Yang Mengubah Alur Program." },
  { "en": "Apa Itu 'Percabangan Bersyarat (Conditional Branch)'?", "id": "Percabangan Yang Bergantung Pada Flag." },
  { "en": "Apa Itu 'Percabangan Tak Bersyarat (Unconditional)'?", "id": "Percabangan Yang Selalu Terjadi." },
  { "en": "Apa Itu 'Subroutine'?", "id": "Blok Kode Yang Bisa Dipanggil." },
  { "en": "Apa Instruksi Untuk Memanggil 'Subroutine'?", "id": "CALL." },
  { "en": "Apa Instruksi Untuk Kembali Dari 'Subroutine'?", "id": "RETURN." },
  { "en": "Bagaimana 'Stack' Digunakan Dalam Panggilan Subroutine?", "id": "Untuk Menyimpan Alamat Kembali." },
  { "en": "Apa Itu 'Interrupt Controller'?", "id": "Mengelola Prioritas Dan Sinyal Interrupt." },
  { "en": "Apa Beda 'Interrupt' Dan 'Subroutine' Call?", "id": "Interrupt Asinkron, Subroutine Sinkron." },
  { "en": "Apa Itu 'Interrupt Masking'?", "id": "Mengabaikan Atau Menonaktifkan Interrupt." },
  { "en": "Apa Itu 'Non-Maskable Interrupt (NMI)'?", "id": "Interrupt Prioritas Tinggi, Tidak Bisa Diabaikan." },
  { "en": "Apa Itu 'Siklus Stealing'?", "id": "DMA (Direct Memory Access) 'Mencuri' Siklus Bus." },
  { "en": "Apa Itu 'Lebar Bus'?", "id": "Jumlah Jalur Paralel Dalam Bus." },
  { "en": "Apa Itu 'Sinkronisasi Bus'?", "id": "Transfer Data Dikendalikan Oleh Clock." },
  { "en": "Apa Itu 'Bus Asinkron'?", "id": "Transfer Data Dikendalikan Sinyal Handshake." },
  { "en": "Apa Itu 'Arbiter Bus'?", "id": "Mengatur Perangkat Mana Yang Gunakan Bus." },
  { "en": "Apa Itu 'I/O Terisolasi'?", "id": "Ruang Alamat Terpisah Untuk I/O." },
  { "en": "Apa Itu 'I/O Dipetakan Memori'?", "id": "Berbagi Ruang Alamat Dengan Memori." },
  { "en": "Apa Itu 'Port Paralel'?", "id": "Antarmuka Untuk Transfer Data Paralel." },
  { "en": "Apa Itu 'Port Serial'?", "id": "Antarmuka Untuk Transfer Data Serial." },
  { "en": "Apa Itu 'Keluarga Logika'?", "id": "Sekelompok Gerbang Dengan Teknologi Sama." },
  { "en": "Apa Itu 'Noise Immunity'?", "id": "Kemampuan Sirkuit Menolak Gangguan Derau." },
  { "en": "Apa Itu 'Active Pull-up'?", "id": "Struktur Output Menggunakan Transistor Aktif." },
  { "en": "Apa Itu 'Passive Pull-up'?", "id": "Struktur Output Menggunakan Resistor." },
  { "en": "Apa Itu 'Gerbang AND-OR-Invert (AOI)'?", "id": "Gerbang Fungsional Kompleks." },
  { "en": "Apa Itu 'Kapasitansi Input'?", "id": "Kapasitansi Yang Dilihat Di Terminal Input." },
  { "en": "Apa Itu 'Kapasitansi Output'?", "id": "Kapasitansi Yang Dilihat Di Terminal Output." },
  { "en": "Apa Itu 'Characteristic Equation'?", "id": "Persamaan Yang Mendeskripsikan Perilaku Flip-Flop." },
  { "en": "Apa Itu 'Sequential Logic Design'?", "id": "Proses Merancang Rangkaian Sekuensial." },
  { "en": "Apa Itu 'Register Geser Universal'?", "id": "Bisa Geser Kiri, Kanan, Muat Paralel." },
  { "en": "Apa Itu 'Array Memori'?", "id": "Susunan Sel Memori Dalam Baris-Kolom." },
  { "en": "Apa Itu 'Sense Amplifier'?", "id": "Mendeteksi Sinyal Lemah Dari Sel Memori." },
  { "en": "Apa Itu 'Programmable Logic Array (PLA)'?", "id": "PLD (Programmable Logic Device) Dengan Plane AND-OR." },
  { "en": "Apa Itu 'Programmable Array Logic (PAL)'?", "id": "PLD (Programmable Logic Device) Dengan Plane AND." },
  { "en": "Apa Itu 'Complex Programmable Logic Device (CPLD)'?", "id": "Perangkat Logika Kompleks Berbasis Makrosel." },
  { "en": "Apa Itu 'Makrosel'?", "id": "Blok Fungsional Dalam CPLD." },
  { "en": "Apa Itu 'Hard Core Processor'?", "id": "CPU (Central Processing Unit) Fisik Di FPGA." },
  { "en": "Apa Itu 'Soft Core Processor'?", "id": "CPU (Central Processing Unit) Diimplementasi Di Logika." },
  { "en": "Apa Beda 'Verifikasi Formal' Dan 'Simulasi'?", "id": "Formal Buktikan, Simulasi Uji." },
  { "en": "Apa Itu 'Batasan Sintesis (Synthesis Constraint)'?", "id": "Aturan Untuk Proses Sintesis." },
  { "en": "Apa Kepanjangan 'CDC (Clock Domain Crossing)'?", "id": "Lintas Domain Clock." },
  { "en": "Apa Jenis 'Instruksi Transfer Data'?", "id": "LOAD, STORE, MOVE." },
  { "en": "Apa Jenis 'Instruksi Aritmetika'?", "id": "ADD, SUB, MUL, DIV." },
  { "en": "Apa Itu 'Mode Pengalamatan Immediate'?", "id": "Data Ada Di Dalam Instruksi." },
  { "en": "Apa Beda 'OTA' Dan 'Op-Amp'?", "id": "OTA (Operational Transconductance Amplifier) Output Arus." },
  { "en": "Apa Itu 'Penguat Umpan Balik Arus'?", "id": "Penguat Dengan Umpan Balik Arus." },
  { "en": "Apa Itu 'Sel Gilbert'?", "id": "Inti Sirkuit Pencampur (Mixer)." },
  { "en": "Bagaimana Cara Mengurangi 'Crosstalk'?", "id": "Jarak Jalur, Jalur Ground." },
  { "en": "Bagaimana 'Lebar Daerah Deplesi' Berubah?", "id": "Tergantung Pada Tegangan Bias." },
  { "en": "Apa Itu 'Simulasi Kesalahan (Fault Simulation)'?", "id": "Mensimulasikan Efek Kerusakan." },
  { "en": "Apa Itu 'Soket Burn-in'?", "id": "Soket Tahan Suhu Tinggi." },
  { "en": "Apa Kepanjangan 'SMC (System Management Controller)'?", "id": "Kontroler Manajemen Sistem." },
  { "en": "Apa Kepanjangan 'TPM (Trusted Platform Module)'?", "id": "Modul Platform Terpercaya." },
  { "en": "Siapa Yang Mencetuskan 'Hukum Moore'?", "id": "Gordon Moore." },
  { "en": "Apa Mikroprosesor Komersial Pertama?", "id": "Intel 4004." },
  { "en": "Apa Itu 'Wafer Sort'?", "id": "Pengujian Dan Pemilahan Die." },
  { "en": "Apa Itu 'Laser Marking'?", "id": "Menandai Paket IC Dengan Laser." },
  { "en": "Apa Itu 'Lead Pitch'?", "id": "Jarak Antar Pusat Pin IC." },
  { "en": "Apa Itu 'Coplanarity'?", "id": "Kesejajaran Ujung Kaki-kaki IC." },
  { "en": "Apa Itu 'Ball Attach'?", "id": "Proses Pemasangan Bola Solder BGA." },
  { "en": "Apa Itu 'Flux'?", "id": "Bahan Kimia Pembersih Untuk Solder." },
  { "en": "Apa Itu 'Solder Bridging'?", "id": "Hubung Singkat Akibat Timah Solder." },
  { "en": "Apa Itu 'Cold Solder Joint'?", "id": "Sambungan Solder Yang Tidak Matang." },
  { "en": "Apa Itu 'Rework Station'?", "id": "Alat Untuk Memperbaiki Kesalahan Solder." },
  { "en": "Apa Itu 'Bake-out'?", "id": "Memanaskan Komponen Untuk Hilangkan Lembab." },
  { "en": "Apa Kepanjangan 'MSL (Moisture Sensitivity Level)'?", "id": "Tingkat Sensitivitas Kelembaban." },
  { "en": "Apa Itu 'Dry Pack'?", "id": "Kemasan Vakum Dengan Desiccant." },
  { "en": "Apa Itu 'Desiccant'?", "id": "Bahan Penyerap Kelembaban." },
  { "en": "Apa Itu 'Humidity Indicator Card (HIC)'?", "id": "Kartu Indikator Tingkat Kelembaban." },
  { "en": "Apa Itu 'Electroplating'?", "id": "Proses Pelapisan Logam Dengan Listrik." },
  { "en": "Apa Itu 'Anodizing'?", "id": "Membentuk Lapisan Oksida Pelindung." },
  { "en": "Apa Itu 'Chemical Etching'?", "id": "Nama Lain Untuk Wet Etching." },
  { "en": "Apa Itu 'Ion Beam Etching'?", "id": "Mengetsa Dengan Menembakkan Berkas Ion." },
  { "en": "Apa Itu 'Passivation'?", "id": "Lapisan Pelindung Terakhir Pada Chip." },
  { "en": "Apa Itu 'Sintering'?", "id": "Memadatkan Serbuk Material Dengan Panas." },
  { "en": "Apa Itu 'Die Preparation'?", "id": "Menyiapkan Wafer Untuk Dipotong." },
  { "en": "Apa Itu 'Wafer Mounting'?", "id": "Melekatkan Wafer Ke Bingkai." },
  { "en": "Apa Itu 'Die Separation'?", "id": "Nama Lain Untuk Proses Dicing." },
  { "en": "Apa Itu 'Blade Dicing'?", "id": "Memotong Wafer Dengan Gergaji Presisi." },
  { "en": "Apa Itu 'Stealth Dicing'?", "id": "Membuat Retakan Internal Dengan Laser." },
  { "en": "Apa Itu 'Eutectic Bonding'?", "id": "Menyambung Menggunakan Paduan Titik Lebur Rendah." },
  { "en": "Apa Itu 'Epoxy Adhesive'?", "id": "Perekat Berbasis Resin Epoksi." },
  { "en": "Apa Itu 'Thermal Conductivity'?", "id": "Kemampuan Bahan Menghantarkan Panas." },
  { "en": "Apa Kepanjangan 'CTE (Coefficient of Thermal Expansion)'?", "id": "Koefisien Ekspansi Termal." },
  { "en": "Mengapa 'CTE (Coefficient of Thermal Expansion)' Penting?", "id": "Perbedaan CTE (Coefficient of Thermal Expansion) Sebabkan Stres." },
  { "en": "Apa Itu 'Stress Buffer'?", "id": "Lapisan Untuk Mengurangi Stres Mekanis." },
  { "en": "Apa Itu 'Wirebond Pull Test'?", "id": "Uji Kekuatan Tarik Kawat Bonding." },
  { "en": "Apa Itu 'Ball Shear Test'?", "id": "Uji Kekuatan Geser Sambungan Bola." },
  { "en": "Apa Itu 'Acoustic Microscopy'?", "id": "Pencitraan Menggunakan Gelombang Suara." },
  { "en": "Apa Fungsi 'Acoustic Microscopy'?", "id": "Mendeteksi Celah Atau Delaminasi." },
  { "en": "Apa Itu 'Delamination'?", "id": "Pemisahan Antar Lapisan Dalam Paket." },
  { "en": "Apa Itu 'Popcorning'?", "id": "Kerusakan Paket Akibat Uap Air." },
  { "en": "Apa Itu 'Tin Whiskers'?", "id": "Pertumbuhan Kristal Timah Mirip Kumis." },
  { "en": "Mengapa 'Tin Whiskers' Berbahaya?", "id": "Dapat Menyebabkan Hubung Singkat." },
  { "en": "Apa Kepanjangan 'AOI (Automated Optical Inspection)'?", "id": "Inspeksi Optik Otomatis." },
  { "en": "Apa Kepanjangan 'AXI (Automated X-ray Inspection)'?", "id": "Inspeksi Sinar-X Otomatis." },
  { "en": "Apa Kepanjangan 'ICT (In-Circuit Test)'?", "id": "Tes Dalam Sirkuit." },
  { "en": "Apa Itu 'Bed of Nails'?", "id": "Fixture Uji Dengan Banyak Pin." },
  { "en": "Apa Itu 'Flying Probe'?", "id": "Pengujian Tanpa Fixture, Probe Bergerak." },
  { "en": "Apa Itu 'Functional Test'?", "id": "Pengujian Fungsi Akhir Produk." },
  { "en": "Apa Itu 'System-Level Test'?", "id": "Pengujian Sistem Secara Keseluruhan." },
  { "en": "Apa Itu 'Test Fixture'?", "id": "Alat Kustom Untuk Memegang DUT." },
  { "en": "Apa Itu 'Pogo Pin'?", "id": "Pin Kontak Berpegas Untuk Pengujian." },
  { "en": "Apa Itu 'Yield Analysis'?", "id": "Menganalisis Penyebab Rendahnya Yield." },
  { "en": "Apa Itu 'Wafer Map'?", "id": "Peta Visual Die Gagal Di Wafer." },
  { "en": "Apa Itu 'Process Control Monitor (PCM)'?", "id": "Struktur Tes Untuk Memantau Proses." },
  { "en": "Apa Kepanjangan 'DFY (Design for Yield)'?", "id": "Desain Untuk Yield." },
  { "en": "Apa Kepanjangan 'DFR (Design for Reliability)'?", "id": "Desain Untuk Keandalan." },
  { "en": "Apa Itu 'Semiconductor Device Modeling'?", "id": "Membuat Model Matematika Perangkat." },
  { "en": "Apa Itu 'SPICE Model'?", "id": "Model Standar Untuk Simulasi Sirkuit." },
  { "en": "Apa Itu 'IBIS Model'?", "id": "Model Perilaku I/O (Input/Output) Untuk Sinyal." },
  { "en": "Apa Itu 'Verilog-A'?", "id": "Bahasa Pemodelan Perilaku Analog." },
  { "en": "Apa Itu 'Verilog-AMS'?", "id": "Verilog Untuk Sinyal Campuran Analog." },
  { "en": "Apa Itu 'SystemVerilog'?", "id": "Ekstensi Verilog Untuk Verifikasi." },
  { "en": "Apa Kepanjangan 'ABV (Assertion-Based Verification)'?", "id": "Verifikasi Berbasis Pernyataan." },
  { "en": "Apa Kepanjangan 'UVM (Universal Verification Methodology)'?", "id": "Metodologi Verifikasi Universal." },
  { "en": "Apa Itu 'Standard Cell Library'?", "id": "Kumpulan Gerbang Logika Dasar." },
  { "en": "Apa Itu 'I/O Library'?", "id": "Kumpulan Sel Input/Output." },
  { "en": "Apa Itu 'Memory Compiler'?", "id": "Alat Untuk Menghasilkan Layout Memori." },
  { "en": "Apa Itu 'Logic Synthesis'?", "id": "Mengubah RTL Menjadi Netlist Gerbang." },
  { "en": "Apa Itu 'Physical Synthesis'?", "id": "Optimisasi Netlist Dengan Info Fisik." },
  { "en": "Apa Itu 'Clock Tree Synthesis'?", "id": "Membangun Jaringan Distribusi Sinyal Clock." },
  { "en": "Apa Kepanjangan 'STA (Static Timing Analysis)'?", "id": "Analisis Waktu Statis." },
  { "en": "Apa Itu 'Formal Verification'?", "id": "Membuktikan Kebenaran Desain Secara Matematis." },
  { "en": "Apa Itu 'Equivalence Checking'?", "id": "Membandingkan Dua Versi Desain." },
  { "en": "Apa Itu 'Model Checking'?", "id": "Memverifikasi Apakah Model Memenuhi Spesifikasi." },
  { "en": "Apa Itu 'Tapeout'?", "id": "Tahap Akhir Mengirim Desain Ke Pabrik." },
  { "en": "Apa Itu 'GDSII'?", "id": "Format File Standar Untuk Layout IC." },
  { "en": "Apa Itu 'OASIS'?", "id": "Format File Pengganti GDSII." },
  { "en": "Apa Itu 'Arsitektur Set Instruksi (ISA)'?", "id": "Spesifikasi Instruksi Yang Dipahami Prosesor." },
  { "en": "Apa Itu 'Mikroarsitektur'?", "id": "Implementasi Internal Dari ISA." },
  { "en": "Apa Itu 'Datapath'?", "id": "Jalur Aliran Data Dalam Prosesor." },
  { "en": "Apa Itu 'Unit Kontrol'?", "id": "Bagian Yang Mengarahkan Operasi Datapath." },
  { "en": "Apa Itu 'Pipelining'?", "id": "Teknik Tumpang Tindih Eksekusi Instruksi." },
  { "en": "Apa Itu 'Pipeline Hazard'?", "id": "Konflik Yang Mengganggu Aliran Pipeline." },
  { "en": "Apa Itu 'Eksekusi Superscalar'?", "id": "Mengeksekusi Lebih Dari Satu Instruksi." },
  { "en": "Apa Itu 'Eksekusi Out-of-Order'?", "id": "Eksekusi Instruksi Tidak Sesuai Urutan." },
  { "en": "Apa Itu 'Branch Prediction'?", "id": "Menebak Arah Percabangan Kode." },
  { "en": "Apa Itu 'Memory Hierarchy'?", "id": "Tingkatan Memori Berdasarkan Kecepatan." },
  { "en": "Apa Itu 'Cache Coherence'?", "id": "Menjaga Konsistensi Data Antar Cache." },
  { "en": "Apa Kepanjangan 'MMU (Memory Management Unit)'?", "id": "Unit Manajemen Memori." },
  { "en": "Apa Itu 'Memori Virtual'?", "id": "Mekanisme Pengalamatan Memori Abstrak." },
  { "en": "Apa Kepanjangan 'TLB (Translation Lookaside Buffer)'?", "id": "Cache Untuk Terjemahan Alamat." },
  { "en": "Apa Itu 'Desain RTL (Register-Transfer Level)'?", "id": "Mendeskripsikan Aliran Data Antar Register." },
  { "en": "Apa Kepanjangan 'HDL (Hardware Description Language)'?", "id": "Bahasa Deskripsi Perangkat Keras." },
  { "en": "Apa Itu 'Verilog'?", "id": "Bahasa HDL (Hardware Description Language) Populer." },
  { "en": "Apa Itu 'VHDL'?", "id": "Bahasa HDL (Hardware Description Language) Populer Lainnya." },
  { "en": "Apa Itu 'Sintesis Logika'?", "id": "Mengubah Kode RTL Menjadi Gerbang." },
  { "en": "Apa Itu 'Netlist'?", "id": "Deskripsi Koneksi Antar Gerbang Logika." },
  { "en": "Apa Itu 'Desain Fisik'?", "id": "Proses Mengubah Netlist Menjadi Layout." },
  { "en": "Apa Itu 'Floorplanning'?", "id": "Perencanaan Penempatan Blok-blok Besar." },
  { "en": "Apa Itu 'Placement'?", "id": "Penempatan Sel-sel Logika Standar." },
  { "en": "Apa Itu 'Routing'?", "id": "Menghubungkan Sel-sel Dengan Jalur Logam." },
  { "en": "Apa Kepanjangan 'CTS (Clock Tree Synthesis)'?", "id": "Sintesis Pohon Clock." },
  { "en": "Apa Itu 'Verifikasi Fungsional'?", "id": "Memastikan Desain Bekerja Sesuai Fungsi." },
  { "en": "Apa Itu 'Testbench'?", "id": "Lingkungan Kode Untuk Memverifikasi Desain." },
  { "en": "Apa Itu 'Simulasi Event-Driven'?", "id": "Simulasi Berdasarkan Perubahan Sinyal." },
  { "en": "Apa Itu 'Assertion'?", "id": "Pernyataan Properti Yang Harus Dipenuhi." },
  { "en": "Apa Itu 'Functional Coverage'?", "id": "Ukuran Seberapa Baik Skenario Terverifikasi." },
  { "en": "Apa Itu 'Verifikasi Formal'?", "id": "Membuktikan Kebenaran Desain Secara Matematis." },
  { "en": "Apa Kepanjangan 'STA (Static Timing Analysis)'?", "id": "Analisis Waktu Statis." },
  { "en": "Apa Itu 'Jalur Kritis'?", "id": "Jalur Dengan Penundaan Terlama." },
  { "en": "Apa Itu 'Setup Time'?", "id": "Waktu Data Stabil Sebelum Clock." },
  { "en": "Apa Itu 'Hold Time'?", "id": "Waktu Data Stabil Setelah Clock." },
  { "en": "Apa Itu 'Clock Skew'?", "id": "Perbedaan Waktu Tiba Sinyal Clock." },
  { "en": "Apa Itu 'Clock Jitter'?", "id": "Variasi Periode Sinyal Clock." },
  { "en": "Apa Kepanjangan 'PDK (Process Design Kit)'?", "id": "Kit Desain Proses." },
  { "en": "Apa Isi 'PDK (Process Design Kit)'?", "id": "Aturan Desain Dan Model Dari Pabrik." },
  { "en": "Apa Itu 'Standard Cell'?", "id": "Blok Logika Dasar Siap Pakai." },
  { "en": "Apa Itu 'Pustaka Sel Standar'?", "id": "Kumpulan Sel Standar Untuk Desain." },
  { "en": "Apa Kepanjangan 'LEF (Library Exchange Format)'?", "id": "Format Pertukaran Pustaka (Abstrak)." },
  { "en": "Apa Kepanjangan 'DEF (Design Exchange Format)'?", "id": "Format Pertukaran Desain (Penempatan)." },
  { "en": "Apa Kepanjangan 'GDSII'?", "id": "Format File Standar Untuk Layout IC." },
  { "en": "Apa Itu 'Tape-out'?", "id": "Mengirim Desain Final Ke Pabrik." },
  { "en": "Apa Itu 'Power Integrity'?", "id": "Analisis Kualitas Jaringan Catu Daya." },
  { "en": "Apa Itu 'Signal Integrity'?", "id": "Analisis Kualitas Sinyal Digital." },
  { "en": "Apa Itu 'IR Drop'?", "id": "Jatuh Tegangan Pada Jaringan Daya." },
  { "en": "Apa Itu 'Ground Bounce'?", "id": "Fluktuasi Tegangan Pada Jalur Ground." },
  { "en": "Apa Itu 'Crosstalk'?", "id": "Gangguan Antara Jalur Sinyal Berdekatan." },
  { "en": "Apa Itu 'Design Rule Check (DRC)'?", "id": "Memeriksa Kepatuhan Layout Terhadap Aturan." },
  { "en": "Apa Itu 'Layout Versus Schematic (LVS)'?", "id": "Membandingkan Layout Dengan Skematik Asli." },
  { "en": "Apa Itu 'Antenna Effect'?", "id": "Kerusakan Gerbang Akibat Pengumpulan Muatan." },
  { "en": "Apa Itu 'Packaging'?", "id": "Proses Mengemas Chip." },
  { "en": "Apa Itu 'Substrat Paket'?", "id": "Papan Sirkuit Kecil Tempat Chip." },
  { "en": "Apa Itu 'Leadframe'?", "id": "Kerangka Logam Untuk Kaki-kaki Paket." },
  { "en": "Apa Itu 'Die Attach'?", "id": "Proses Melekatkan Chip Ke Substrat." },
  { "en": "Apa Itu 'Wire Bonding'?", "id": "Menyambung Chip Ke Kaki Paket." },
  { "en": "Apa Itu 'Encapsulation'?", "id": "Melindungi Chip Dengan Bahan Cetakan." },
  { "en": "Apa Itu 'Flip-Chip'?", "id": "Pemasangan Chip Terbalik." },
  { "en": "Apa Itu 'Solder Bump'?", "id": "Tonjolan Solder Untuk Koneksi Flip-Chip." },
  { "en": "Apa Itu 'Underfill'?", "id": "Bahan Pengisi Celah Bawah Chip." },
  { "en": "Apa Itu 'System-in-Package (SiP)'?", "id": "Banyak Chip Dalam Satu Paket." },
  { "en": "Apa Itu '3D IC (Integrated Circuit)'?", "id": "Penumpukan Chip Secara Vertikal." },
  { "en": "Apa Itu 'Through-Silicon Via (TSV)'?", "id": "Koneksi Vertikal Melalui Silikon." },
  { "en": "Apa Itu 'Interposer'?", "id": "Lapisan Perantara Untuk Chip 2.5D." },
  { "en": "Apa Itu 'Yield'?", "id": "Persentase Chip Fungsional Dari Wafer." },
  { "en": "Apa Itu 'Wafer Sort'?", "id": "Pengujian Chip Saat Masih Di Wafer." },
  { "en": "Apa Itu 'Final Test'?", "id": "Pengujian Chip Setelah Dikemas." },
  { "en": "Apa Itu 'Binning'?", "id": "Mengelompokkan Chip Berdasarkan Kinerja." },
  { "en": "Apa Itu 'Reliability'?", "id": "Keandalan IC (Integrated Circuit) Seiring Waktu." },
  { "en": "Apa Itu 'Electromigration'?", "id": "Perpindahan Atom Logam Akibat Arus." },
  { "en": "Apa Itu 'Stress Migration'?", "id": "Perpindahan Atom Akibat Stres Mekanis." },
  { "en": "Apa Itu 'Hot Carrier Injection (HCI)'?", "id": "Degradasi Transistor Akibat Pembawa Panas." },
  { "en": "Apa Itu 'Time-Dependent Dielectric Breakdown (TDDB)'?", "id": "Kegagalan Isolator Seiring Waktu." },
  { "en": "Apa Itu 'Latch-up'?", "id": "Kondisi Hubung Singkat Parasit CMOS." },
  { "en": "Apa Itu 'Soft Error'?", "id": "Kesalahan Transien Akibat Partikel Kosmik." },
  { "en": "Apa Itu 'Design for Test (DFT)'?", "id": "Desain Untuk Memudahkan Pengujian." },
  { "en": "Apa Itu 'Scan Chain'?", "id": "Rantai Flip-Flop Untuk Pengujian." },
  { "en": "Apa Itu 'Built-In Self-Test (BIST)'?", "id": "Sirkuit Yang Menguji Dirinya Sendiri." },
  { "en": "Apa Itu 'Fault Model'?", "id": "Model Kerusakan Fisik Untuk Pengujian." },
  { "en": "Apa Itu 'Stuck-at Fault'?", "id": "Asumsi Jalur Terjebak Di Logika 0/1." },
  { "en": "Apa Itu 'Test Pattern'?", "id": "Pola Input Untuk Mendeteksi Kesalahan." },
  { "en": "Apa Itu 'Automated Test Equipment (ATE)'?", "id": "Mesin Otomatis Untuk Pengujian IC." },
  { "en": "Apa Itu 'Probe Card'?", "id": "Kartu Jarum Untuk Pengujian Wafer." },
  { "en": "Apa Itu 'Device Under Test (DUT)'?", "id": "Perangkat Yang Sedang Diuji." },
  { "en": "Apa Itu 'Struktur Kristal Diamond'?", "id": "Struktur Atom Silikon Dan Germanium." },
  { "en": "Apa Itu 'Struktur Kristal Zincblende'?", "id": "Struktur Atom Galium Arsenida (GaAs)." },
  { "en": "Apa Itu 'Indeks Miller'?", "id": "Notasi Untuk Bidang Dan Arah Kristal." },
  { "en": "Mengapa Orientasi Wafer Penting?", "id": "Mempengaruhi Sifat Listrik Dan Mekanis." },
  { "en": "Apa Kepanjangan 'INL (Integral Nonlinearity)'?", "id": "Non-Linearitas Integral." },
  { "en": "Apa Kepanjangan 'DNL (Differential Nonlinearity)'?", "id": "Non-Linearitas Diferensial." },
  { "en": "Apa Itu 'INL' dan 'DNL'?", "id": "Ukuran Kesalahan Pada ADC/DAC." },
  { "en": "Apa Itu 'Konverter Buck'?", "id": "Topologi Penurun Tegangan DC-DC." },
  { "en": "Apa Itu 'Konverter Boost'?", "id": "Topologi Penaik Tegangan DC-DC." },
  { "en": "Apa Itu 'Konverter Buck-Boost'?", "id": "Topologi Penaik/Penurun Tegangan DC-DC." },
  { "en": "Apa Itu 'Pompa Muatan (Charge Pump)'?", "id": "Konverter DC-DC (Direct Current-DC) Berbasis Kapasitor." },
  { "en": "Apa Itu 'Arus Diam (Quiescent Current)'?", "id": "Arus Yang Dikonsumsi IC Saat Diam." },
  { "en": "Apa Beda Sel 'SRAM' Dan 'DRAM'?", "id": "SRAM (Static RAM) 6T, DRAM (Dynamic RAM) 1T1C." },
  { "en": "Arsitektur Flash Mana Yang Lebih Cepat?", "id": "Arsitektur NOR." },
  { "en": "Arsitektur Flash Mana Yang Lebih Padat?", "id": "Arsitektur NAND." },
  { "en": "Apa Itu 'Wear Leveling'?", "id": "Meratakan Penggunaan Blok Memori Flash." },
  { "en": "Apa Itu 'Garbage Collection'?", "id": "Proses Membersihkan Blok Data Flash." },
  { "en": "Apa Beda Emas Dan Tembaga Untuk 'Wirebond'?", "id": "Emas Lebih Tahan Korosi." },
  { "en": "Apa Itu 'Logic Gate Delay'?", "id": "Waktu Tunda Sinyal Melewati Gerbang." },
  { "en": "Apa Itu 'Clock Distribution Network'?", "id": "Jaringan Jalur Distribusi Sinyal Clock." },
  { "en": "Apa Itu 'Power Distribution Network'?", "id": "Jaringan Jalur Distribusi Daya." },
  { "en": "Apa Kepanjangan 'RTL (Resistor-Transistor Logic)'?", "id": "Logika Resistor-Transistor." },
  { "en": "Apa Kepanjangan 'DTL (Diode-Transistor Logic)'?", "id": "Logika Dioda-Transistor." },
  { "en": "Apa Kepanjangan 'ECL (Emitter-Coupled Logic)'?", "id": "Logika Terkopel Emitor." },
  { "en": "Apa Keuntungan 'ECL'?", "id": "Sangat Cepat." },
  { "en": "Apa Kerugian 'ECL'?", "id": "Konsumsi Daya Sangat Tinggi." },
  { "en": "Apa Itu 'Phase Noise'?", "id": "Derau Fasa Pada Osilator." },
  { "en": "Apa Itu 'Jitter'?", "id": "Variasi Waktu Pada Tepi Sinyal." },
  { "en": "Apa Itu 'Sel Gilbert'?", "id": "Sirkuit Inti Untuk Pencampur (Mixer)." },
  { "en": "Apa Itu 'Tegangan Saturasi'?", "id": "Tegangan Vce Saat Transistor Jenuh." },
  { "en": "Apa Itu 'Arus Cutoff'?", "id": "Arus Bocor Saat Transistor Mati." },
  { "en": "Apa Itu 'Penskalaan Dennard'?", "id": "Prinsip Penskalaan Transistor MOSFET." },
  { "en": "Apa Itu 'Efek Kanal Pendek'?", "id": "Perilaku Non-ideal Akibat Penskalaan." },
  { "en": "Apa Kepanjangan 'DIBL (Drain-Induced Barrier Lowering)'?", "id": "Penurunan Penghalang Akibat Drain." },
  { "en": "Apa Itu 'Saturasi Kecepatan'?", "id": "Kecepatan Pembawa Muatan Mencapai Batasnya." },
  { "en": "Apa Itu 'Arus Bocor Gerbang'?", "id": "Arus Yang Menerowong Melalui Oksida." },
  { "en": "Apa Itu 'Silikon Terasah (Strained Silicon)'?", "id": "Meningkatkan Mobilitas Dengan Meregangkan Kisi." },
  { "en": "Apa Itu 'Fotodioda'?", "id": "Dioda Yang Berfungsi Sebagai Sensor Cahaya." },
  { "en": "Apa Itu 'Dioda Gunn'?", "id": "Dioda Untuk Menghasilkan Sinyal Microwave." },
  { "en": "Apa Itu 'Dioda IMPATT'?", "id": "Dioda Lain Untuk Aplikasi Microwave." },
  { "en": "Apa Itu 'Resonator Kristal'?", "id": "Komponen Piezoelektrik Untuk Frekuensi Stabil." },
  { "en": "Apa Itu 'Resonator Keramik'?", "id": "Resonator Dengan Stabilitas Lebih Rendah." },
  { "en": "Apa Itu 'Mode Bersama (Common Mode)'?", "id": "Sinyal Yang Sama Pada Dua Jalur." },
  { "en": "Apa Itu 'Mode Diferensial'?", "id": "Sinyal Yang Berlawanan Pada Dua Jalur." },
  { "en": "Apa Itu 'Kabel Twisted Pair'?", "id": "Sepasang Kabel Yang Dipilin." },
  { "en": "Mengapa Kabel 'Twisted Pair' Digunakan?", "id": "Untuk Mengurangi Gangguan Elektromagnetik." },
  { "en": "Apa Itu 'Angka Derau (Noise Figure)'?", "id": "Ukuran Degradasi SNR (Signal-to-Noise Ratio)." },
  { "en": "Apa Itu 'Distorsi Harmonik Total (THD)'?", "id": "Ukuran Distorsi Sinyal." },
  { "en": "Apa Itu 'Intermodulasi'?", "id": "Pencampuran Sinyal Yang Menghasilkan Frekuensi Baru." },
  { "en": "Apa Itu 'Titik Potong Orde Ketiga (IP3)'?", "id": "Ukuran Linieritas." },
  { "en": "Apa Itu 'Rentang Dinamis Bebas Spurious (SFDR)'?", "id": "Rentang Antara Sinyal Dan Derau Terkuat." },
  { "en": "Apa Itu 'Pengemasan Hermetik'?", "id": "Paket Kedap Udara." },
  { "en": "Mengapa 'Pengemasan Hermetik' Digunakan?", "id": "Untuk Keandalan Tinggi (Militer, Luar Angkasa)." },
  { "en": "Bahan Apa Untuk 'Paket Hermetik'?", "id": "Keramik Atau Logam." },
  { "en": "Apa Itu 'Keluaran Totem-Pole'?", "id": "Struktur Output Khas Pada Logika TTL." },
  { "en": "Apa Itu 'Gerbang AND-OR-Invert (AOI)'?", "id": "Gerbang Fungsional Kompleks." },
  { "en": "Apa Itu 'Array Memori'?", "id": "Susunan Sel Memori Dalam Baris-Kolom." },
  { "en": "Apa Itu 'Pohon Clock'?", "id": "Jaringan Distribusi Sinyal Clock." },
  { "en": "Apa Itu 'Siklus Tunggu (Wait State)'?", "id": "Penundaan Siklus Clock Untuk Sinkronisasi." },
  { "en": "Apa Itu 'Burst Transfer'?", "id": "Transfer Blok Data Berurutan Cepat." },
  { "en": "Apa Itu 'Cache Controller'?", "id": "Sirkuit Logika Pengelola Operasi Cache." },
  { "en": "Apa Itu 'Cache Policy'?", "id": "Aturan Untuk Manajemen Cache." },
  { "en": "Apa Itu 'Cache Coherence Protocol'?", "id": "Protokol Penjaga Konsistensi Antar Cache." },
  { "en": "Apa Itu 'Snooping'?", "id": "Proses Cache Memantau Aktivitas Bus." },
  { "en": "Apa Itu 'Directory-Based Protocol'?", "id": "Protokol Koherensi Cache Tersentralisasi." },
  { "en": "Apa Itu 'False Sharing'?", "id": "Invalidasi Cache Akibat Data Tak Terkait." },
  { "en": "Apa Itu 'Halaman Memori (Page)'?", "id": "Blok Memori Virtual Ukuran Tetap." },
  { "en": "Apa Itu 'Swapping'?", "id": "Memindahkan Halaman Antara RAM Dan Disk." },
  { "en": "Apa Itu 'Segmentation' Memori?", "id": "Membagi Memori Menjadi Segmen Logis." },
  { "en": "Apa Itu 'Sistem Interkoneksi Crossbar'?", "id": "Setiap Input Bisa Terhubung Ke Output." },
  { "en": "Apa Itu 'Parity Ganjil'?", "id": "Memastikan Jumlah Bit 1 Ganjil." },
  { "en": "Apa Itu 'Parity Genap'?", "id": "Memastikan Jumlah Bit 1 Genap." },
  { "en": "Apa Itu 'Hamming Distance'?", "id": "Jumlah Bit Yang Berbeda." },
  { "en": "Apa Itu 'Stack Frame'?", "id": "Area Stack Untuk Satu Panggilan Fungsi." },
  { "en": "Apa Itu 'Microcode'?", "id": "Instruksi Level Rendah Pengontrol CPU." },
  { "en": "Apa Itu 'Horizontal Microcode'?", "id": "Microcode Dengan Kontrol Paralel Tinggi." },
  { "en": "Apa Itu 'Vertical Microcode'?", "id": "Microcode Dengan Pengkodean Lebih Padat." },
  { "en": "Apa Itu 'Prosesor Vektor'?", "id": "Prosesor Yang Beroperasi Pada Array Data." },
  { "en": "Apa Kepanjangan 'SIMD (Single Instruction, Multiple Data)'?", "id": "Satu Instruksi Untuk Banyak Data." },
  { "en": "Apa Kepanjangan 'MIMD (Multiple Instruction, Multiple Data)'?", "id": "Banyak Instruksi Untuk Banyak Data." },
  { "en": "Apa Itu 'Sistem Multiprocessor'?", "id": "Sistem Dengan Beberapa CPU." },
  { "en": "Apa Kepanjangan 'SMP (Symmetric Multiprocessing)'?", "id": "Multiprocessing Simetris." },
  { "en": "Apa Itu 'Hukum Amdahl'?", "id": "Hukum Tentang Batas Percepatan Paralel." },
  { "en": "Apa Itu 'Benchmark'?", "id": "Program Untuk Mengukur Kinerja." },
  { "en": "Apa Kepanjangan 'MIPS (Million Instructions Per Second)'?", "id": "Juta Instruksi Per Detik." },
  { "en": "Apa Kepanjangan 'FLOPS (Floating-Point Operations Per Second)'?", "id": "Operasi Floating-Point Per Detik." },
  { "en": "Apa Itu 'Port I/O'?", "id": "Antarmuka Fisik Untuk Perangkat I/O." },
  { "en": "Apa Itu 'Interrupt Latency'?", "id": "Waktu Tunda Respon Terhadap Interrupt." },
  { "en": "Apa Itu 'Interrupt Priority'?", "id": "Tingkat Kepentingan Sebuah Sinyal Interrupt." },
  { "en": "Apa Itu 'Daisy Chaining'?", "id": "Metode Sederhana Untuk Arbitrasi." },
  { "en": "Apa Itu 'Cache Line'?", "id": "Unit Transfer Data Antara Cache-RAM." },
  { "en": "Apa Itu 'Cache Tag'?", "id": "Menyimpan Informasi Alamat Cache Line." },
  { "en": "Apa Itu 'Valid Bit'?", "id": "Menandakan Apakah Data Cache Valid." },
  { "en": "Apa Itu 'Dirty Bit'?", "id": "Menandakan Data Cache Telah Dimodifikasi." },
  { "en": "Apa Itu 'Cache Mapping'?", "id": "Aturan Penempatan Data Dari RAM Ke Cache." },
  { "en": "Apa Itu 'Direct-Mapped Cache'?", "id": "Setiap Blok RAM Hanya Satu Lokasi Cache." },
  { "en": "Apa Itu 'Fully Associative Cache'?", "id": "Blok RAM Bisa Ditempatkan Di Lokasi Apapun." },
  { "en": "Apa Itu 'Set-Associative Cache'?", "id": "Kompromi Antara Direct Dan Fully Associative." },
  { "en": "Apa Itu 'Replacement Policy'?", "id": "Aturan Memilih Blok Cache Untuk Diganti." },
  { "en": "Apa Kepanjangan 'LRU (Least Recently Used)'?", "id": "Paling Jarang Digunakan." },
  { "en": "Apa Kepanjangan 'FIFO (First-In, First-Out)' Cache Replacement?", "id": "Pertama Masuk, Pertama Keluar." },
  { "en": "Apa Itu 'Random Replacement'?", "id": "Mengganti Blok Cache Secara Acak." },
  { "en": "Apa Itu 'Unified Cache'?", "id": "Cache Tunggal Untuk Instruksi Dan Data." },
  { "en": "Apa Itu 'Split Cache'?", "id": "Cache Terpisah Untuk Instruksi Dan Data." },
  { "en": "Apa Itu 'Multilevel Cache'?", "id": "Hirarki Cache (L1, L2, L3)." },
  { "en": "Apa Itu 'Write Allocate'?", "id": "Alokasikan Cache Line Saat Operasi Tulis." },
  { "en": "Apa Itu 'No-Write Allocate'?", "id": "Tidak Alokasikan Cache Line Saat Tulis." },
  { "en": "Apa Itu 'Prefetching'?", "id": "Mengambil Data Sebelum Dibutuhkan." },
  { "en": "Apa Itu 'Lockup-Free Cache'?", "id": "Cache Yang Bisa Tangani Banyak Miss." },
  { "en": "Apa Itu 'Virtual Cache'?", "id": "Cache Yang Diindeks Dengan Alamat Virtual." },
  { "en": "Apa Itu 'Physical Cache'?", "id": "Cache Yang Diindeks Dengan Alamat Fisik." },
  { "en": "Apa Itu 'Bus Clock'?", "id": "Clock Yang Mengatur Kecepatan Transfer Bus." },
  { "en": "Apa Itu 'Bus Protocol'?", "id": "Aturan Untuk Komunikasi Di Atas Bus." },
  { "en": "Apa Itu 'Bus Bridge'?", "id": "Menghubungkan Dua Bus Yang Berbeda." },
  { "en": "Apa Itu 'I/O Controller Hub (ICH)'?", "id": "Chipset Pengatur Perangkat I/O Lambat." },
  { "en": "Apa Itu 'Northbridge'?", "id": "Chipset Pengatur Komunikasi Cepat (CPU, RAM)." },
  { "en": "Apa Itu 'Southbridge'?", "id": "Chipset Pengatur Komunikasi Lambat (I/O)." },
  { "en": "Apa Itu 'Chipset'?", "id": "Sekumpulan Sirkuit Terpadu Papan Induk." },
  { "en": "Apa Itu 'Sistem Operasi'?", "id": "Perangkat Lunak Pengelola Sumber Daya Komputer." },
  { "en": "Apa Itu 'Kernel'?", "id": "Inti Dari Sebuah Sistem Operasi." },
  { "en": "Apa Itu 'Process Control Block (PCB)'?", "id": "Struktur Data Penyimpan Info Proses." },
  { "en": "Apa Itu 'Zombie Process'?", "id": "Proses Selesai Yang Belum Dibersihkan." },
  { "en": "Apa Itu 'Orphan Process'?", "id": "Proses Yang Induknya Telah Berakhir." },
  { "en": "Apa Itu 'System Call Interface'?", "id": "Antarmuka Antara Aplikasi Dan Kernel." },
  { "en": "Apa Itu 'Manajemen Memori'?", "id": "Tugas OS (Operating System) Mengatur Alokasi Memori." },
  { "en": "Apa Itu 'Alokasi Kontigu'?", "id": "Memberi Satu Blok Memori Berurutan." },
  { "en": "Apa Itu 'Alokasi Non-Kontigu'?", "id": "Memberi Blok Memori Yang Tersebar." },
  { "en": "Apa Itu 'File'?", "id": "Kumpulan Informasi Terkait Di Penyimpanan." },
  { "en": "Apa Itu 'Direktori'?", "id": "Struktur Untuk Mengorganisir File." },
  { "en": "Apa Itu 'Sistem File'?", "id": "Metode Penyimpanan Dan Organisasi File." },
  { "en": "Apa Itu 'Metadata'?", "id": "Data Tentang Data (Ukuran, Tanggal)." },
  { "en": "Apa Itu 'Hak Akses File'?", "id": "Izin Baca, Tulis, Eksekusi." },
  { "en": "Apa Itu 'Virtualisasi'?", "id": "Membuat Versi Virtual Dari Sumber Daya." },
  { "en": "Apa Itu 'Hypervisor'?", "id": "Perangkat Lunak Yang Membuat Mesin Virtual." },
  { "en": "Apa Itu 'Virtual Machine Monitor (VMM)'?", "id": "Nama Lain Untuk Hypervisor." },
  { "en": "Apa Itu 'Emulasi'?", "id": "Meniru Perilaku Sistem Lain." },
  { "en": "Apa Itu 'Simulasi'?", "id": "Memodelkan Perilaku Sistem." },
  { "en": "Apa Beda 'Emulasi' Dan 'Simulasi'?", "id": "Emulasi Tiru Fungsi, Simulasi Tiru Perilaku." },
  { "en": "Apa Itu 'Jaringan Komputer'?", "id": "Kumpulan Komputer Yang Saling Terhubung." },
  { "en": "Apa Itu 'Topologi Jaringan'?", "id": "Tata Letak Fisik Atau Logis Jaringan." },
  { "en": "Apa Itu 'Topologi Bus'?", "id": "Semua Perangkat Terhubung Ke Satu Kabel." },
  { "en": "Apa Itu 'Topologi Bintang'?", "id": "Semua Perangkat Terhubung Ke Pusat." },
  { "en": "Apa Itu 'Topologi Cincin'?", "id": "Perangkat Terhubung Dalam Lingkaran." },
  { "en": "Apa Itu 'Topologi Mesh'?", "id": "Banyak Perangkat Saling Terhubung." },
  { "en": "Apa Kepanjangan 'LAN (Local Area Network)'?", "id": "Jaringan Area Lokal." },
  { "en": "Apa Kepanjangan 'WAN (Wide Area Network)'?", "id": "Jaringan Area Luas." },
  { "en": "Apa Itu 'Internet'?", "id": "Jaringan Global Dari Jaringan." },
  { "en": "Apa Itu 'Model OSI (Open Systems Interconnection)'?", "id": "Model Konseptual Tujuh Lapis Jaringan." },
  { "en": "Apa Itu 'Model TCP/IP'?", "id": "Model Empat Lapis Yang Digunakan Internet." },
  { "en": "Apa Itu 'Encapsulation'?", "id": "Proses Membungkus Data Dengan Header." },
  { "en": "Apa Itu 'Decapsulation'?", "id": "Proses Membuka Bungkusan Header." },
  { "en": "Apa Kepanjangan 'MAC (Media Access Control)'?", "id": "Kontrol Akses Media." },
  { "en": "Apa Kepanjangan 'IP (Internet Protocol)'?", "id": "Protokol Internet." },
  { "en": "Apa Itu 'Port Number'?", "id": "Nomor Pengidentifikasi Aplikasi Di Host." },
  { "en": "Apa Itu 'Socket'?", "id": "Kombinasi Alamat IP Dan Nomor Port." },
  { "en": "Apa Kepanjangan 'DNS (Domain Name System)'?", "id": "Sistem Penamaan Domain." },
  { "en": "Apa Fungsi 'DNS'?", "id": "Menerjemahkan Nama Domain Menjadi Alamat IP." },
  { "en": "Apa Kepanjangan 'DHCP (Dynamic Host Configuration Protocol)'?", "id": "Protokol Konfigurasi Host Dinamis." },
  { "en": "Apa Fungsi 'DHCP'?", "id": "Memberi Alamat IP Secara Otomatis." },
  { "en": "Apa Kepanjangan 'NAT (Network Address Translation)'?", "id": "Translasi Alamat Jaringan." },
  { "en": "Apa Itu 'Sistem Failsafe'?", "id": "Sistem Yang Gagal Dalam Keadaan Aman." },
  { "en": "Apa Itu 'Latch Transparan'?", "id": "Output Mengikuti Input Saat Enable." },
  { "en": "Apa Itu 'Pengalamatan Memori'?", "id": "Metode Untuk Memilih Lokasi Memori." },
  { "en": "Apa Itu 'Peta Memori'?", "id": "Alokasi Alamat Untuk Perangkat Berbeda." },
  { "en": "Apa Itu 'Waktu Siklus (Cycle Time)'?", "id": "Periode Sinyal Clock." },
  { "en": "Apa Itu 'Laju Transfer Data'?", "id": "Jumlah Data Yang Dipindahkan Per Detik." },
  { "en": "Apa Satuan 'Laju Transfer Data'?", "id": "Bit Per Detik (bps)." },
  { "en": "Apa Itu 'Bandwidth'?", "id": "Kapasitas Maksimum Laju Transfer Data." },
  { "en": "Apa Itu 'Latensi'?", "id": "Waktu Tunda Awal Transfer Data." },
  { "en": "Apa Itu 'Multiplexing'?", "id": "Berbagi Satu Saluran Untuk Banyak Sinyal." },
  { "en": "Apa Itu 'Demultiplexing'?", "id": "Memisahkan Sinyal Gabungan Kembali." },
  { "en": "Apa Kepanjangan 'FDM (Frequency Division Multiplexing)'?", "id": "Multiplexing Pembagian Frekuensi." },
  { "en": "Apa Kepanjangan 'TDM (Time Division Multiplexing)'?", "id": "Multiplexing Pembagian Waktu." },
  { "en": "Apa Itu 'Antarmuka Digital'?", "id": "Titik Sambungan Antara Perangkat Digital." },
  { "en": "Contoh 'Antarmuka Digital'?", "id": "USB (Universal Serial Bus), HDMI, SPI, I2C." },
  { "en": "Apa Kepanjangan 'USB (Universal Serial Bus)'?", "id": "Bus Serial Universal." },
  { "en": "Apa Itu 'Sirkuit Digital Kustom'?", "id": "Sirkuit Yang Didesain Untuk Tugas Tertentu." },
  { "en": "Apa Itu 'ASIC (Application-Specific Integrated Circuit)'?", "id": "Contoh Sirkuit Digital Kustom." },
  { "en": "Apa Itu 'Prosesor Sinyal Digital (DSP)'?", "id": "Prosesor Khusus Untuk Pemrosesan Sinyal." },
  { "en": "Apa Itu 'Siklus Tugas 50%'?", "id": "Waktu Sinyal Tinggi Dan Rendah Sama." },
  { "en": "Apa Itu 'Rangkaian Logika Resistor-Transistor (RTL)'?", "id": "Keluarga Logika Awal Menggunakan Resistor-Transistor." },
  { "en": "Apa Itu 'Rangkaian Logika Dioda-Transistor (DTL)'?", "id": "Keluarga Logika Sebelum TTL." },
  { "en": "Apa Itu 'Rangkaian Logika Emitter-Coupled (ECL)'?", "id": "Keluarga Logika Bipolar Berkecepatan Sangat Tinggi." },
  { "en": "Apa Itu 'Active Low'?", "id": "Sinyal Aktif Saat Logika Rendah (0)." },
  { "en": "Apa Itu 'Active High'?", "id": "Sinyal Aktif Saat Logika Tinggi (1)." },
  { "en": "Apa Itu 'Finite Automata'?", "id": "Model Matematika Untuk Mesin Keadaan." },
  { "en": "Apa Beda 'Finite Automata' Dan 'State Machine'?", "id": "Konsep Yang Sama, Beda Terminologi." },
  { "en": "Apa Itu 'System Call'?", "id": "Cara Program Aplikasi Meminta Layanan OS." },
  { "en": "Bagaimana 'Register' Berbeda Dari 'Cache'?", "id": "Register Di Dalam, Cache Di Luar CPU." },
  { "en": "Apa Peran 'Linker' Dalam Kompilasi?", "id": "Menggabungkan File Objek Menjadi Executable." },
  { "en": "Apa Itu 'Siklus Fetch'?", "id": "Tahap Pengambilan Instruksi Dari Memori." },
  { "en": "Apa Itu 'Siklus Execute'?", "id": "Tahap Menjalankan Instruksi Oleh CPU." },
  { "en": "Apa Itu 'Datapath'?", "id": "Jalur Data Dan Komponen Fungsional CPU." },
  { "en": "Apa Itu 'Control Path'?", "id": "Sirkuit Yang Mengontrol Aliran Datapath." },
  { "en": "Apa Itu 'Sinyal Kontrol'?", "id": "Sinyal Yang Dihasilkan Oleh Unit Kontrol." },
  { "en": "Apa Itu 'Micro-operation'?", "id": "Operasi Elementer Yang Dilakukan CPU." },
  { "en": "Apa Itu 'Register Transfer Language (RTL)'?", "id": "Bahasa Simbolik Mendeskripsikan Transfer Register." },
  { "en": "Apa Itu 'Struktur Kristal Diamond'?", "id": "Struktur Atom Silikon Dan Germanium." },
  { "en": "Apa Itu 'Struktur Kristal Zincblende'?", "id": "Struktur Atom Galium Arsenida (GaAs)." },
  { "en": "Apa Itu 'Indeks Miller'?", "id": "Notasi Untuk Bidang Dan Arah Kristal." },
  { "en": "Mengapa Orientasi Wafer Penting?", "id": "Mempengaruhi Sifat Listrik Dan Mekanis." },
  { "en": "Apa Kepanjangan 'INL (Integral Nonlinearity)'?", "id": "Non-Linearitas Integral." },
  { "en": "Apa Kepanjangan 'DNL (Differential Nonlinearity)'?", "id": "Non-Linearitas Diferensial." },
  { "en": "Apa Itu 'INL' dan 'DNL'?", "id": "Ukuran Kesalahan Pada ADC/DAC." },
  { "en": "Apa Itu 'Konverter Buck'?", "id": "Topologi Penurun Tegangan DC-DC." },
  { "en": "Apa Itu 'Konverter Boost'?", "id": "Topologi Penaik Tegangan DC-DC." },
  { "en": "Apa Itu 'Konverter Buck-Boost'?", "id": "Topologi Penaik/Penurun Tegangan DC-DC." },
  { "en": "Apa Itu 'Pompa Muatan (Charge Pump)'?", "id": "Konverter DC-DC (Direct Current-DC) Berbasis Kapasitor." },
  { "en": "Apa Itu 'Arus Diam (Quiescent Current)'?", "id": "Arus Yang Dikonsumsi IC Saat Diam." },
  { "en": "Apa Beda Sel 'SRAM' Dan 'DRAM'?", "id": "SRAM (Static RAM) 6T, DRAM (Dynamic RAM) 1T1C." },
  { "en": "Arsitektur Flash Mana Yang Lebih Cepat?", "id": "Arsitektur NOR." },
  { "en": "Arsitektur Flash Mana Yang Lebih Padat?", "id": "Arsitektur NAND." },
  { "en": "Apa Itu 'Wear Leveling'?", "id": "Meratakan Penggunaan Blok Memori Flash." },
  { "en": "Apa Itu 'Garbage Collection'?", "id": "Proses Membersihkan Blok Data Flash." },
  { "en": "Apa Beda Emas Dan Tembaga Untuk 'Wirebond'?", "id": "Emas Lebih Tahan Korosi." },
  { "en": "Apa Itu 'Logic Gate Delay'?", "id": "Waktu Tunda Sinyal Melewati Gerbang." },
  { "en": "Apa Itu 'Clock Distribution Network'?", "id": "Jaringan Jalur Distribusi Sinyal Clock." },
  { "en": "Apa Itu 'Power Distribution Network'?", "id": "Jaringan Jalur Distribusi Daya." },
  { "en": "Apa Kepanjangan 'RTL (Resistor-Transistor Logic)'?", "id": "Logika Resistor-Transistor." },
  { "en": "Apa Kepanjangan 'DTL (Diode-Transistor Logic)'?", "id": "Logika Dioda-Transistor." },
  { "en": "Apa Kepanjangan 'ECL (Emitter-Coupled Logic)'?", "id": "Logika Terkopel Emitor." },
  { "en": "Apa Keuntungan 'ECL'?", "id": "Sangat Cepat." },
  { "en": "Apa Kerugian 'ECL'?", "id": "Konsumsi Daya Sangat Tinggi." },
  { "en": "Apa Itu 'Phase Noise'?", "id": "Derau Fasa Pada Osilator." },
  { "en": "Apa Itu 'Jitter'?", "id": "Variasi Waktu Pada Tepi Sinyal." },
  { "en": "Apa Itu 'Sel Gilbert'?", "id": "Sirkuit Inti Untuk Pencampur (Mixer)." },
  { "en": "Apa Itu 'Tegangan Saturasi'?", "id": "Tegangan Vce Saat Transistor Jenuh." },
  { "en": "Apa Itu 'Arus Cutoff'?", "id": "Arus Bocor Saat Transistor Mati." },
  { "en": "Apa Itu 'Penskalaan Dennard'?", "id": "Prinsip Penskalaan Transistor MOSFET." },
  { "en": "Apa Itu 'Efek Kanal Pendek'?", "id": "Perilaku Non-ideal Akibat Penskalaan." },
  { "en": "Apa Kepanjangan 'DIBL (Drain-Induced Barrier Lowering)'?", "id": "Penurunan Penghalang Akibat Drain." },
  { "en": "Apa Itu 'Saturasi Kecepatan'?", "id": "Kecepatan Pembawa Muatan Mencapai Batasnya." },
  { "en": "Apa Itu 'Arus Bocor Gerbang'?", "id": "Arus Yang Menerowong Melalui Oksida." },
  { "en": "Apa Itu 'Silikon Terasah (Strained Silicon)'?", "id": "Meningkatkan Mobilitas Dengan Meregangkan Kisi." },
  { "en": "Apa Itu 'Fotodioda'?", "id": "Dioda Yang Berfungsi Sebagai Sensor Cahaya." },
  { "en": "Apa Itu 'Dioda Gunn'?", "id": "Dioda Untuk Menghasilkan Sinyal Microwave." },
  { "en": "Apa Itu 'Dioda IMPATT'?", "id": "Dioda Lain Untuk Aplikasi Microwave." },
  { "en": "Apa Itu 'Resonator Kristal'?", "id": "Komponen Piezoelektrik Untuk Frekuensi Stabil." },
  { "en": "Apa Itu 'Resonator Keramik'?", "id": "Resonator Dengan Stabilitas Lebih Rendah." },
  { "en": "Apa Itu 'Mode Bersama (Common Mode)'?", "id": "Sinyal Yang Sama Pada Dua Jalur." },
  { "en": "Apa Itu 'Mode Diferensial'?", "id": "Sinyal Yang Berlawanan Pada Dua Jalur." },
  { "en": "Apa Itu 'Kabel Twisted Pair'?", "id": "Sepasang Kabel Yang Dipilin." },
  { "en": "Mengapa Kabel 'Twisted Pair' Digunakan?", "id": "Untuk Mengurangi Gangguan Elektromagnetik." },
  { "en": "Apa Itu 'Angka Derau (Noise Figure)'?", "id": "Ukuran Degradasi SNR (Signal-to-Noise Ratio)." },
  { "en": "Apa Itu 'Distorsi Harmonik Total (THD)'?", "id": "Ukuran Distorsi Sinyal." },
  { "en": "Apa Itu 'Intermodulasi'?", "id": "Pencampuran Sinyal Yang Menghasilkan Frekuensi Baru." },
  { "en": "Apa Itu 'Titik Potong Orde Ketiga (IP3)'?", "id": "Ukuran Linieritas." },
  { "en": "Apa Itu 'Rentang Dinamis Bebas Spurious (SFDR)'?", "id": "Rentang Antara Sinyal Dan Derau Terkuat." },
  { "en": "Apa Itu 'Pengemasan Hermetik'?", "id": "Paket Kedap Udara." },
  { "en": "Mengapa 'Pengemasan Hermetik' Digunakan?", "id": "Untuk Keandalan Tinggi (Militer, Luar Angkasa)." },
  { "en": "Bahan Apa Untuk 'Paket Hermetik'?", "id": "Keramik Atau Logam." },
  { "en": "Apa Itu 'Keluaran Totem-Pole'?", "id": "Struktur Output Khas Pada Logika TTL." },
  { "en": "Apa Itu 'Gerbang AND-OR-Invert (AOI)'?", "id": "Gerbang Fungsional Kompleks." },
  { "en": "Apa Itu 'Array Memori'?", "id": "Susunan Sel Memori Dalam Baris-Kolom." },
  { "en": "Apa Itu 'Pohon Clock'?", "id": "Jaringan Distribusi Sinyal Clock." },
  { "en": "Apa Itu 'Siklus Tunggu (Wait State)'?", "id": "Penundaan Siklus Clock Untuk Sinkronisasi." },
  { "en": "Apa Itu 'Burst Transfer'?", "id": "Transfer Blok Data Berurutan Cepat." },
  { "en": "Apa Itu 'Cache Controller'?", "id": "Sirkuit Logika Pengelola Operasi Cache." },
  { "en": "Apa Itu 'Cache Policy'?", "id": "Aturan Untuk Manajemen Cache." },
  { "en": "Apa Itu 'Cache Coherence Protocol'?", "id": "Protokol Penjaga Konsistensi Antar Cache." },
  { "en": "Apa Itu 'Snooping'?", "id": "Proses Cache Memantau Aktivitas Bus." },
  { "en": "Apa Itu 'Directory-Based Protocol'?", "id": "Protokol Koherensi Cache Tersentralisasi." },
  { "en": "Apa Itu 'False Sharing'?", "id": "Invalidasi Cache Akibat Data Tak Terkait." },
  { "en": "Apa Itu 'Halaman Memori (Page)'?", "id": "Blok Memori Virtual Ukuran Tetap." },
  { "en": "Apa Itu 'Swapping'?", "id": "Memindahkan Halaman Antara RAM Dan Disk." },
  { "en": "Apa Itu 'Segmentation' Memori?", "id": "Membagi Memori Menjadi Segmen Logis." },
  { "en": "Apa Itu 'Sistem Interkoneksi Crossbar'?", "id": "Setiap Input Bisa Terhubung Ke Output." },
  { "en": "Apa Itu 'Parity Ganjil'?", "id": "Memastikan Jumlah Bit 1 Ganjil." },
  { "en": "Apa Itu 'Parity Genap'?", "id": "Memastikan Jumlah Bit 1 Genap." },
  { "en": "Apa Itu 'Hamming Distance'?", "id": "Jumlah Bit Yang Berbeda." },
  { "en": "Apa Itu 'Stack Frame'?", "id": "Area Stack Untuk Satu Panggilan Fungsi." },
  { "en": "Apa Itu 'Microcode'?", "id": "Instruksi Level Rendah Pengontrol CPU." },
  { "en": "Apa Itu 'Horizontal Microcode'?", "id": "Microcode Dengan Kontrol Paralel Tinggi." },
  { "en": "Apa Itu 'Vertical Microcode'?", "id": "Microcode Dengan Pengkodean Lebih Padat." },
  { "en": "Apa Itu 'Prosesor Vektor'?", "id": "Prosesor Yang Beroperasi Pada Array Data." },
  { "en": "Apa Kepanjangan 'SIMD (Single Instruction, Multiple Data)'?", "id": "Satu Instruksi Untuk Banyak Data." },
  { "en": "Apa Kepanjangan 'MIMD (Multiple Instruction, Multiple Data)'?", "id": "Banyak Instruksi Untuk Banyak Data." },
  { "en": "Apa Itu 'Sistem Multiprocessor'?", "id": "Sistem Dengan Beberapa CPU." },
  { "en": "Apa Kepanjangan 'SMP (Symmetric Multiprocessing)'?", "id": "Multiprocessing Simetris." },
  { "en": "Apa Itu 'Hukum Amdahl'?", "id": "Hukum Tentang Batas Percepatan Paralel." },
  { "en": "Apa Itu 'Benchmark'?", "id": "Program Untuk Mengukur Kinerja." },
  { "en": "Apa Kepanjangan 'MIPS (Million Instructions Per Second)'?", "id": "Juta Instruksi Per Detik." },
  { "en": "Apa Kepanjangan 'FLOPS (Floating-Point Operations Per Second)'?", "id": "Operasi Floating-Point Per Detik." },
  { "en": "Apa Itu 'Port I/O'?", "id": "Antarmuka Fisik Untuk Perangkat I/O." },
  { "en": "Apa Itu 'Interrupt Latency'?", "id": "Waktu Tunda Respon Terhadap Interrupt." },
  { "en": "Apa Itu 'Interrupt Priority'?", "id": "Tingkat Kepentingan Sebuah Sinyal Interrupt." },
  { "en": "Apa Itu 'Daisy Chaining'?", "id": "Metode Sederhana Untuk Arbitrasi." },
  { "en": "Apa Itu 'Cache Line'?", "id": "Unit Transfer Data Antara Cache-RAM." },
  { "en": "Apa Itu 'Cache Tag'?", "id": "Menyimpan Informasi Alamat Cache Line." },
  { "en": "Apa Itu 'Valid Bit'?", "id": "Menandakan Apakah Data Cache Valid." },
  { "en": "Apa Itu 'Dirty Bit'?", "id": "Menandakan Data Cache Telah Dimodifikasi." },
  { "en": "Apa Itu 'Cache Mapping'?", "id": "Aturan Penempatan Data Dari RAM Ke Cache." },
  { "en": "Apa Itu 'Direct-Mapped Cache'?", "id": "Setiap Blok RAM Hanya Satu Lokasi Cache." },
  { "en": "Apa Itu 'Fully Associative Cache'?", "id": "Blok RAM Bisa Ditempatkan Di Lokasi Apapun." },
  { "en": "Apa Itu 'Set-Associative Cache'?", "id": "Kompromi Antara Direct Dan Fully Associative." },
  { "en": "Apa Itu 'Replacement Policy'?", "id": "Aturan Memilih Blok Cache Untuk Diganti." },
  { "en": "Apa Kepanjangan 'LRU (Least Recently Used)'?", "id": "Paling Jarang Digunakan." },
  { "en": "Apa Kepanjangan 'FIFO (First-In, First-Out)' Cache Replacement?", "id": "Pertama Masuk, Pertama Keluar." },
  { "en": "Apa Itu 'Random Replacement'?", "id": "Mengganti Blok Cache Secara Acak." },
  { "en": "Apa Itu 'Unified Cache'?", "id": "Cache Tunggal Untuk Instruksi Dan Data." },
  { "en": "Apa Itu 'Split Cache'?", "id": "Cache Terpisah Untuk Instruksi Dan Data." },
  { "en": "Apa Itu 'Multilevel Cache'?", "id": "Hirarki Cache (L1, L2, L3)." },
  { "en": "Apa Itu 'Write Allocate'?", "id": "Alokasikan Cache Line Saat Operasi Tulis." },
  { "en": "Apa Itu 'No-Write Allocate'?", "id": "Tidak Alokasikan Cache Line Saat Tulis." },
  { "en": "Apa Itu 'Prefetching'?", "id": "Mengambil Data Sebelum Dibutuhkan." },
  { "en": "Apa Itu 'Lockup-Free Cache'?", "id": "Cache Yang Bisa Tangani Banyak Miss." },
  { "en": "Apa Itu 'Virtual Cache'?", "id": "Cache Yang Diindeks Dengan Alamat Virtual." },
  { "en": "Apa Itu 'Physical Cache'?", "id": "Cache Yang Diindeks Dengan Alamat Fisik." },
  { "en": "Apa Itu 'Bus Clock'?", "id": "Clock Yang Mengatur Kecepatan Transfer Bus." },
  { "en": "Apa Itu 'Bus Protocol'?", "id": "Aturan Untuk Komunikasi Di Atas Bus." },
  { "en": "Apa Itu 'Bus Bridge'?", "id": "Menghubungkan Dua Bus Yang Berbeda." },
  { "en": "Apa Itu 'I/O Controller Hub (ICH)'?", "id": "Chipset Pengatur Perangkat I/O Lambat." },
  { "en": "Apa Itu 'Northbridge'?", "id": "Chipset Pengatur Komunikasi Cepat (CPU, RAM)." },
  { "en": "Apa Itu 'Southbridge'?", "id": "Chipset Pengatur Komunikasi Lambat (I/O)." },
  { "en": "Apa Itu 'Chipset'?", "id": "Sekumpulan Sirkuit Terpadu Papan Induk." },
  { "en": "Apa Itu 'Sistem Operasi'?", "id": "Perangkat Lunak Pengelola Sumber Daya Komputer." },
  { "en": "Apa Itu 'Kernel'?", "id": "Inti Dari Sebuah Sistem Operasi." },
  { "en": "Apa Itu 'Process Control Block (PCB)'?", "id": "Struktur Data Penyimpan Info Proses." },
  { "en": "Apa Itu 'Zombie Process'?", "id": "Proses Selesai Yang Belum Dibersihkan." },
  { "en": "Apa Itu 'Orphan Process'?", "id": "Proses Yang Induknya Telah Berakhir." },
  { "en": "Apa Itu 'System Call Interface'?", "id": "Antarmuka Antara Aplikasi Dan Kernel." },
  { "en": "Apa Itu 'Manajemen Memori'?", "id": "Tugas OS (Operating System) Mengatur Alokasi Memori." },
  { "en": "Apa Itu 'Alokasi Kontigu'?", "id": "Memberi Satu Blok Memori Berurutan." },
  { "en": "Apa Itu 'Alokasi Non-Kontigu'?", "id": "Memberi Blok Memori Yang Tersebar." },
  { "en": "Apa Itu 'File'?", "id": "Kumpulan Informasi Terkait Di Penyimpanan." },
  { "en": "Apa Itu 'Direktori'?", "id": "Struktur Untuk Mengorganisir File." },
  { "en": "Apa Itu 'Sistem File'?", "id": "Metode Penyimpanan Dan Organisasi File." },
  { "en": "Apa Itu 'Metadata'?", "id": "Data Tentang Data (Ukuran, Tanggal)." },
  { "en": "Apa Itu 'Hak Akses File'?", "id": "Izin Baca, Tulis, Eksekusi." },
  { "en": "Apa Itu 'Virtualisasi'?", "id": "Membuat Versi Virtual Dari Sumber Daya." },
  { "en": "Apa Itu 'Hypervisor'?", "id": "Perangkat Lunak Yang Membuat Mesin Virtual." },
  { "en": "Apa Itu 'Virtual Machine Monitor (VMM)'?", "id": "Nama Lain Untuk Hypervisor." },
  { "en": "Apa Itu 'Emulasi'?", "id": "Meniru Perilaku Sistem Lain." },
  { "en": "Apa Itu 'Simulasi'?", "id": "Memodelkan Perilaku Sistem." },
  { "en": "Apa Beda 'Emulasi' Dan 'Simulasi'?", "id": "Emulasi Tiru Fungsi, Simulasi Tiru Perilaku." },
  { "en": "Apa Itu 'Jaringan Komputer'?", "id": "Kumpulan Komputer Yang Saling Terhubung." },
  { "en": "Apa Itu 'Topologi Jaringan'?", "id": "Tata Letak Fisik Atau Logis Jaringan." },
  { "en": "Apa Itu 'Topologi Bus'?", "id": "Semua Perangkat Terhubung Ke Satu Kabel." },
  { "en": "Apa Itu 'Topologi Bintang'?", "id": "Semua Perangkat Terhubung Ke Pusat." },
  { "en": "Apa Itu 'Topologi Cincin'?", "id": "Perangkat Terhubung Dalam Lingkaran." },
  { "en": "Apa Itu 'Topologi Mesh'?", "id": "Banyak Perangkat Saling Terhubung." },
  { "en": "Apa Kepanjangan 'LAN (Local Area Network)'?", "id": "Jaringan Area Lokal." },
  { "en": "Apa Kepanjangan 'WAN (Wide Area Network)'?", "id": "Jaringan Area Luas." },
  { "en": "Apa Itu 'Internet'?", "id": "Jaringan Global Dari Jaringan." },
  { "en": "Apa Itu 'Model OSI (Open Systems Interconnection)'?", "id": "Model Konseptual Tujuh Lapis Jaringan." },
  { "en": "Apa Itu 'Model TCP/IP'?", "id": "Model Empat Lapis Yang Digunakan Internet." },
  { "en": "Apa Itu 'Encapsulation'?", "id": "Proses Membungkus Data Dengan Header." },
  { "en": "Apa Itu 'Decapsulation'?", "id": "Proses Membuka Bungkusan Header." },
  { "en": "Apa Kepanjangan 'MAC (Media Access Control)'?", "id": "Kontrol Akses Media." },
  { "en": "Apa Kepanjangan 'IP (Internet Protocol)'?", "id": "Protokol Internet." },
  { "en": "Apa Itu 'Port Number'?", "id": "Nomor Pengidentifikasi Aplikasi Di Host." },
  { "en": "Apa Itu 'Socket'?", "id": "Kombinasi Alamat IP Dan Nomor Port." },
  { "en": "Apa Kepanjangan 'DNS (Domain Name System)'?", "id": "Sistem Penamaan Domain." },
  { "en": "Apa Fungsi 'DNS'?", "id": "Menerjemahkan Nama Domain Menjadi Alamat IP." },
  { "en": "Apa Kepanjangan 'DHCP (Dynamic Host Configuration Protocol)'?", "id": "Protokol Konfigurasi Host Dinamis." },
  { "en": "Apa Fungsi 'DHCP'?", "id": "Memberi Alamat IP Secara Otomatis." },
  { "en": "Apa Kepanjangan 'NAT (Network Address Translation)'?", "id": "Translasi Alamat Jaringan." },
  { "en": "Apa Itu 'Sistem Failsafe'?", "id": "Sistem Yang Gagal Dalam Keadaan Aman." },
  { "en": "Apa Itu 'Latch Transparan'?", "id": "Output Mengikuti Input Saat Enable." },
  { "en": "Apa Itu 'Pengalamatan Memori'?", "id": "Metode Untuk Memilih Lokasi Memori." },
  { "en": "Apa Itu 'Peta Memori'?", "id": "Alokasi Alamat Untuk Perangkat Berbeda." },
  { "en": "Apa Itu 'Waktu Siklus (Cycle Time)'?", "id": "Periode Sinyal Clock." },
  { "en": "Apa Itu 'Laju Transfer Data'?", "id": "Jumlah Data Yang Dipindahkan Per Detik." },
  { "en": "Apa Satuan 'Laju Transfer Data'?", "id": "Bit Per Detik (bps)." },
  { "en": "Apa Itu 'Bandwidth'?", "id": "Kapasitas Maksimum Laju Transfer Data." },
  { "en": "Apa Itu 'Latensi'?", "id": "Waktu Tunda Awal Transfer Data." },
  { "en": "Apa Itu 'Multiplexing'?", "id": "Berbagi Satu Saluran Untuk Banyak Sinyal." },
  { "en": "Apa Itu 'Demultiplexing'?", "id": "Memisahkan Sinyal Gabungan Kembali." },
  { "en": "Apa Kepanjangan 'FDM (Frequency Division Multiplexing)'?", "id": "Multiplexing Pembagian Frekuensi." },
  { "en": "Apa Kepanjangan 'TDM (Time Division Multiplexing)'?", "id": "Multiplexing Pembagian Waktu." },
  { "en": "Apa Itu 'Antarmuka Digital'?", "id": "Titik Sambungan Antara Perangkat Digital." },
  { "en": "Contoh 'Antarmuka Digital'?", "id": "USB (Universal Serial Bus), HDMI, SPI, I2C." },
  { "en": "Apa Kepanjangan 'USB (Universal Serial Bus)'?", "id": "Bus Serial Universal." },
  { "en": "Apa Itu 'Sirkuit Digital Kustom'?", "id": "Sirkuit Yang Didesain Untuk Tugas Tertentu." },
  { "en": "Apa Itu 'ASIC (Application-Specific Integrated Circuit)'?", "id": "Contoh Sirkuit Digital Kustom." },
  { "en": "Apa Itu 'Prosesor Sinyal Digital (DSP)'?", "id": "Prosesor Khusus Untuk Pemrosesan Sinyal." },
  { "en": "Apa Itu 'Siklus Tugas 50%'?", "id": "Waktu Sinyal Tinggi Dan Rendah Sama." },
  { "en": "Apa Itu 'Rangkaian Logika Resistor-Transistor (RTL)'?", "id": "Keluarga Logika Awal Menggunakan Resistor-Transistor." },
  { "en": "Apa Itu 'Rangkaian Logika Dioda-Transistor (DTL)'?", "id": "Keluarga Logika Sebelum TTL." },
  { "en": "Apa Itu 'Rangkaian Logika Emitter-Coupled (ECL)'?", "id": "Keluarga Logika Bipolar Berkecepatan Sangat Tinggi." },
  { "en": "Apa Itu 'Active Low'?", "id": "Sinyal Aktif Saat Logika Rendah (0)." },
  { "en": "Apa Itu 'Active High'?", "id": "Sinyal Aktif Saat Logika Tinggi (1)." },
  { "en": "Apa Itu 'Finite Automata'?", "id": "Model Matematika Untuk Mesin Keadaan." },
  { "en": "Apa Beda 'Finite Automata' Dan 'State Machine'?", "id": "Konsep Yang Sama, Beda Terminologi." },
  { "en": "Apa Itu 'System Call'?", "id": "Cara Program Aplikasi Meminta Layanan OS." },
  { "en": "Bagaimana 'Register' Berbeda Dari 'Cache'?", "id": "Register Di Dalam, Cache Di Luar CPU." },
  { "en": "Apa Peran 'Linker' Dalam Kompilasi?", "id": "Menggabungkan File Objek Menjadi Executable." },
  { "en": "Apa Itu 'Siklus Fetch'?", "id": "Tahap Pengambilan Instruksi Dari Memori." },
  { "en": "Apa Itu 'Siklus Execute'?", "id": "Tahap Menjalankan Instruksi Oleh CPU." },
  { "en": "Apa Itu 'Datapath'?", "id": "Jalur Data Dan Komponen Fungsional CPU." },
  { "en": "Apa Itu 'Control Path'?", "id": "Sirkuit Yang Mengontrol Aliran Datapath." },
  { "en": "Apa Itu 'Sinyal Kontrol'?", "id": "Sinyal Yang Dihasilkan Oleh Unit Kontrol." },
  { "en": "Apa Itu 'Micro-operation'?", "id": "Operasi Elementer Yang Dilakukan CPU." },
  { "en": "Apa Itu 'Register Transfer Language (RTL)'?", "id": "Bahasa Simbolik Mendeskripsikan Transfer Register." },



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
            }, 7000);
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
