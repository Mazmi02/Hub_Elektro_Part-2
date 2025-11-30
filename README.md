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


    { "en": "Apa Itu Resistor Metal Film?", "id": "Resistor Presisi Toleransi Rendah 1%." },
    { "en": "Apa Itu Resistor Carbon Film?", "id": "Resistor Umum Toleransi Standar 5%." },
    { "en": "Apa Warna Body Resistor Metal Film?", "id": "Biasanya Berwarna Biru." },
    { "en": "Apa Warna Body Resistor Carbon Film?", "id": "Biasanya Berwarna Krem Atau Cokelat." },
    { "en": "Apa Itu Resistor Kapur (Cement)?", "id": "Resistor Daya Besar Kotak Putih." },
    { "en": "Apa Kegunaan Resistor Kapur?", "id": "Beban Daya Audio Atau Power." },
    { "en": "Apa Itu Potensiometer Logaritmik (Tipe A)?", "id": "Perubahan Resistansi Melengkung Untuk Volume." },
    { "en": "Apa Itu Potensiometer Linear (Tipe B)?", "id": "Perubahan Resistansi Lurus Proporsional." },
    { "en": "Apa Fungsi Trimpot Multiturn?", "id": "Kalibrasi Nilai Sangat Presisi." },
    { "en": "Apa Itu Rheostat?", "id": "Resistor Variabel Daya Tinggi 2 Kaki." },
    { "en": "Apa Fungsi Kapasitor MKM?", "id": "Kapasitor Non-polar Stabil Ukuran Kecil." },
    { "en": "Apa Warna Khas Kapasitor MKM?", "id": "Merah Hati Atau Kuning Kotak." },
    { "en": "Apa Itu Kapasitor Mika (Silver Mica)?", "id": "Kapasitor Presisi Frekuensi Tinggi RF." },
    { "en": "Apa Itu Varibel Capacitor (Varco)?", "id": "Kapasitor Putar Penala Radio Analog." },
    { "en": "Apa Fungsi Coil IF (Trafo IF)?", "id": "Filter Frekuensi Menengah Radio." },
    { "en": "Apa Warna Inti Ferit Oscilator Radio?", "id": "Merah." },
    { "en": "Apa Warna Inti Ferit IF 455kHz?", "id": "Kuning, Putih, Dan Hitam." },
    { "en": "Apa Fungsi Speaker Woofer?", "id": "Hasilkan Suara Nada Rendah Bass." },
    { "en": "Apa Fungsi Speaker Full Range?", "id": "Hasilkan Semua Rentang Frekuensi Suara." },
    { "en": "Apa Itu Speaker Piezo Tweeter?", "id": "Tweeter Murah Tanpa Magnet." },
    { "en": "Apa Fungsi Pasta Solder (Solder Paste)?", "id": "Butiran Timah Dan Flux Untuk SMD." },
    { "en": "Apa Itu Stencil PCB?", "id": "Cetakan Plat Lubang Pasta Solder." },
    { "en": "Apa Fungsi Solder Uap (Blower)?", "id": "Panaskan Timah Pasta Secara Merata." },
    { "en": "Apa Suhu Udara Solder Uap Umum?", "id": "350 Hingga 400 Derajat Celcius." },
    { "en": "Apa Itu Kapton Tape?", "id": "Selotip Tahan Panas Warna Kuning." },
    { "en": "Apa Fungsi Kapton Tape Saat Soldering?", "id": "Lindungi Komponen Sekitar Dari Panas." },
    { "en": "Apa Itu Pinset Bengkok (Curved Tweezers)?", "id": "Mudahkan Ambil Komponen Di Sudut." },
    { "en": "Apa Fungsi Kaca Pembesar (Magnifier)?", "id": "Inspeksi Solderan Kaki Kecil." },
    { "en": "Apa Itu Lampu Service Kaca Pembesar?", "id": "Lampu Meja Dengan Lensa Besar." },
    { "en": "Apa Fungsi Bor PCB Mini?", "id": "Buat Lubang Kaki Komponen THT." },
    { "en": "Apa Ukuran Mata Bor Kaki IC?", "id": "0.8 Milimeter." },
    { "en": "Apa Ukuran Mata Bor Kaki Resistor?", "id": "0.8 Hingga 1.0 Milimeter." },
    { "en": "Apa Ukuran Mata Bor Terminal Blok?", "id": "1.2 Hingga 1.5 Milimeter." },
    { "en": "Apa Fungsi Kikir Jarum (Needle File)?", "id": "Haluskan Tepian PCB Potongan Kasar." },
    { "en": "Apa Itu Spacer PCB Kuningan?", "id": "Baut Penyangga PCB Bahan Logam." },
    { "en": "Apa Itu Spacer PCB Plastik?", "id": "Penyangga PCB Isolator Listrik." },
    { "en": "Apa Fungsi Ring Baut (Washer)?", "id": "Cegah Baut Longgar Dan Sebar Tekanan." },
    { "en": "Apa Itu Ring Per (Spring Washer)?", "id": "Ring Belah Pengunci Anti Getar." },
    { "en": "Apa Itu Mur Nilon (Nylon Lock Nut)?", "id": "Mur Dengan Karet Pengunci Dalam." },
    { "en": "Apa Fungsi Kabel Ties (Zip Tie)?", "id": "Ikat Rapikan Kabel." },
    { "en": "Apa Itu Kabel Ties Mount?", "id": "Dudukan Kabel Ties Tempel Dinding." },
    { "en": "Apa Fungsi IC ULN2003?", "id": "Driver Relay Atau Stepper Motor." },
    { "en": "Apa Isi Dalam IC ULN2003?", "id": "Tujuh Pasangan Transistor Darlington." },
    { "en": "Apa Fungsi IC MAX7219?", "id": "Driver Display Matriks LED Serial." },
    { "en": "Apa Fungsi IC Shift Register 74HC165?", "id": "Ubah Input Paralel Ke Serial." },
    { "en": "Apa Beda 74HC595 Dan 74HC165?", "id": "595 Output, 165 Input." },
    { "en": "Apa Fungsi IC Optocoupler 4N35?", "id": "Isolator Sinyal Fototransistor Standar." },
    { "en": "Apa Fungsi IC Decoder 7447?", "id": "Driver 7-Segment Dari BCD." },
    { "en": "Apa Itu BCD (Binary Coded Decimal)?", "id": "Representasi Biner Angka Desimal." },
    { "en": "Apa Fungsi Sensor BME280?", "id": "Ukur Suhu, Lembap, Tekanan Udara." },
    { "en": "Apa Beda BMP280 Dan BME280?", "id": "BMP280 Tidak Ukur Kelembapan." },
    { "en": "Apa Antarmuka Sensor BME280?", "id": "I2C Atau SPI." },
    { "en": "Apa Fungsi Sensor Hujan (Raindrop)?", "id": "Deteksi Tetesan Air Hujan Konduktif." },
    { "en": "Apa Kelemahan Sensor Hujan PCB?", "id": "Jalur Tembaga Mudah Terkorosi." },
    { "en": "Apa Fungsi Sensor Suara KY-037?", "id": "Deteksi Kebisingan Suara Sekitar." },
    { "en": "Apa Fungsi Sensor Getar Piezo?", "id": "Deteksi Ketukan Pada Permukaan Keras." },
    { "en": "Apa Itu Sensor Flex (Flex Sensor)?", "id": "Ubah Resistansi Saat Ditekuk." },
    { "en": "Apa Aplikasi Sensor Flex?", "id": "Sarung Tangan Robotik (Power Glove)." },
    { "en": "Apa Itu Sensor Force Sensitive Resistor?", "id": "Ubah Resistansi Saat Ditekan (FSR)." },
    { "en": "Apa Fungsi Solenoid Door Lock 12V?", "id": "Kunci Pintu Elektrik Otomatis." },
    { "en": "Apa Bentuk Lidah Solenoid Lock?", "id": "Miring Agar Bisa Menutup Sendiri." },
    { "en": "Apa Fungsi Water Pump Mini 5V?", "id": "Pompa Air Submersible Proyek Kecil." },
    { "en": "Apa Itu Peristaltic Pump?", "id": "Pompa Cairan Dosis Presisi (Medis)." },
    { "en": "Apa Kelebihan Pompa Peristaltic?", "id": "Cairan Tidak Sentuh Mesin Pompa." },
    { "en": "Apa Fungsi Peltier TEC1-12706?", "id": "Elemen Pendingin Dan Pemanas Elektrik." },
    { "en": "Apa Syarat Utama Pakai Peltier?", "id": "Wajib Pasang Heatsink Sisi Panas." },
    { "en": "Apa Akibat Peltier Tanpa Heatsink?", "id": "Rusak Terbakar Dalam Hitungan Detik." },
    { "en": "Apa Itu Joule Thief Circuit?", "id": "Rangkaian Peras Sisa Energi Baterai." },
    { "en": "Apa Komponen Utama Joule Thief?", "id": "Transistor, Resistor, Toroid, LED." },
    { "en": "Apa Fungsi Toroid Pada Joule Thief?", "id": "Naikkan Tegangan Secara Induksi." },
    { "en": "Apa Itu Breadboard Mini 170 Point?", "id": "Papan Rangkaian Prototipe Sangat Kecil." },
    { "en": "Apa Itu Prototype PCB (Perfboard)?", "id": "PCB Lubang Siap Solder." },
    { "en": "Apa Itu Stripboard (Veroboard)?", "id": "PCB Lubang Dengan Jalur Tembaga." },
    { "en": "Apa Cara Memutus Jalur Stripboard?", "id": "Bor Atau Kerik Tembaga." },
    { "en": "Apa Itu Kabel AWG 30 (Wrapping)?", "id": "Kabel Tunggal Sangat Tipis." },
    { "en": "Apa Kegunaan Kabel AWG 30?", "id": "Jumper PCB Jalur Rapat." },
    { "en": "Apa Itu Kabel Rainbow (Pita)?", "id": "Kabel Banyak Warna Berjejer." },
    { "en": "Apa Fungsi Kabel Patch Cord LAN?", "id": "Kabel Jaringan Pendek Siap Pakai." },
    { "en": "Apa Itu Keystone Jack RJ45?", "id": "Soket LAN Modular Dinding." },
    { "en": "Apa Fungsi LAN Tester LED?", "id": "Cek Urutan Koneksi 8 Kabel." },
    { "en": "Apa Indikasi Kabel LAN Bagus?", "id": "Lampu 1-8 Nyala Berurutan." },
    { "en": "Apa Itu Tang Kupas Kabel Otomatis?", "id": "Kupas Isolasi Tanpa Putus Kawat." },
    { "en": "Apa Fungsi Lem Tembak (Hot Glue)?", "id": "Rekatkan Komponen Dan Isolasi." },
    { "en": "Apa Sifat Lem Tembak?", "id": "Isolator Listrik Yang Baik." },
    { "en": "Apa Itu Double Tape Foam?", "id": "Perekat Dua Sisi Busa Tebal." },
    { "en": "Apa Fungsi Rubber Feet (Kaki Karet)?", "id": "Alas Box Agar Tidak Licin." },
    { "en": "Apa Itu Box Hitam X1?", "id": "Kotak Plastik Proyek Elektronika Kecil." },
    { "en": "Apa Itu V-Belt Pada Motor?", "id": "Sabuk Karet Pemindah Putaran." },
    { "en": "Apa Fungsi Pulley Motor?", "id": "Roda Alur Dudukan Belt." },
    { "en": "Apa Itu Bearing (Laher) Motor?", "id": "Bantalan Poros Minim Gesekan." },
    { "en": "Apa Kode Bearing 608?", "id": "Ukuran Standar Skateboard/Spinner." },
    { "en": "Apa Itu Pillow Block Bearing?", "id": "Rumah Bearing Siap Pasang." },
    { "en": "Apa Fungsi As (Shaft) Motor?", "id": "Batang Besi Pusat Putaran." },
    { "en": "Apa Itu Shaft Coupler Flexible?", "id": "Penyambung As Motor Ke Ulir." },
    { "en": "Apa Fungsi Lead Screw T8?", "id": "Batang Ulir Penggerak Printer 3D." },
    { "en": "Apa Itu Timing Belt GT2?", "id": "Sabuk Bergerigi Presisi CNC." },
    { "en": "Apa Itu Endstop Switch Mekanik?", "id": "Saklar Batas Gerak CNC." },
    { "en": "Apa Fungsi Nozzle Printer 3D?", "id": "Ujung Keluar Filamen Panas." },
    { "en": "Apa Itu Filamen PLA?", "id": "Plastik Printer 3D Ramah Lingkungan." },
    { "en": "Apa Itu Filamen ABS?", "id": "Plastik Kuat Tahan Panas." },
    { "en": "Apa Fungsi Relay ANSI Kode 50?", "id": "Proteksi Arus Lebih Instan." },
    { "en": "Apa Fungsi Relay ANSI Kode 51?", "id": "Proteksi Arus Lebih Waktu Tunda." },
    { "en": "Apa Fungsi Relay ANSI Kode 87?", "id": "Proteksi Diferensial Trafo/Generator." },
    { "en": "Apa Fungsi Relay ANSI Kode 27?", "id": "Proteksi Tegangan Kurang (Undervoltage)." },
    { "en": "Apa Fungsi Relay ANSI Kode 59?", "id": "Proteksi Tegangan Lebih (Overvoltage)." },
    { "en": "Apa Fungsi Relay ANSI Kode 81?", "id": "Proteksi Frekuensi (Kurang/Lebih)." },
    { "en": "Apa Fungsi Relay ANSI Kode 32?", "id": "Proteksi Daya Balik (Reverse Power)." },
    { "en": "Apa Fungsi Relay ANSI Kode 49?", "id": "Proteksi Thermal Mesin/Trafo." },
    { "en": "Apa Fungsi Relay ANSI Kode 63?", "id": "Deteksi Tekanan Gas (Buchholz)." },
    { "en": "Apa Fungsi Relay ANSI Kode 64?", "id": "Deteksi Gangguan Tanah (Ground Fault)." },
    { "en": "Apa Itu Single Line Diagram (SLD)?", "id": "Gambar Representasi Tunggal Sistem Daya." },
    { "en": "Apa Itu Schematic Diagram?", "id": "Gambar Detail Logika Kontrol." },
    { "en": "Apa Itu Wiring Diagram?", "id": "Gambar Fisik Koneksi Kabel." },
    { "en": "Apa Itu As-built Drawing?", "id": "Gambar Revisi Sesuai Pemasangan Akhir." },
    { "en": "Apa Itu Legend Dalam Gambar Teknik?", "id": "Keterangan Simbol Dan Singkatan." },
    { "en": "Apa Itu Load Schedule?", "id": "Daftar Beban Listrik Gedung." },
    { "en": "Apa Itu Panel Schedule?", "id": "Daftar Breaker Dalam Panel." },
    { "en": "Apa Itu Cable Schedule?", "id": "Daftar Spesifikasi Dan Rute Kabel." },
    { "en": "Apa Itu Combiner Box Surya?", "id": "Kotak Gabungan String Panel Surya." },
    { "en": "Apa Fungsi DC Disconnect Switch?", "id": "Isolasi Panel Surya Dari Inverter." },
    { "en": "Apa Fungsi AC Disconnect Switch?", "id": "Isolasi Inverter Dari Grid PLN." },
    { "en": "Apa Itu Solar Array?", "id": "Kumpulan Modul Surya Terhubung." },
    { "en": "Apa Itu Solar Insolation?", "id": "Total Energi Surya Per Waktu." },
    { "en": "Apa Sudut Azimuth Panel Surya?", "id": "Arah Hadap Panel (Utara/Selatan)." },
    { "en": "Apa Efek Shading Pada Panel Surya?", "id": "Produksi Daya Turun Drastis." },
    { "en": "Apa Fungsi Baterai Deep Cycle?", "id": "Tahan Discharge Hingga Hampir Kosong." },
    { "en": "Apa Itu Baterai Flooded Lead Acid?", "id": "Aki Basah Cairan Elektrolit." },
    { "en": "Apa Itu Baterai VRLA?", "id": "Valve Regulated Lead Acid (Kering)." },
    { "en": "Apa Itu Baterai AGM?", "id": "Absorbed Glass Mat (Spons Kaca)." },
    { "en": "Apa Itu Baterai Gel?", "id": "Elektrolit Gel Silika Kental." },
    { "en": "Apa Itu Sulfasi Pada Baterai?", "id": "Kristal Sulfat Menutupi Pelat." },
    { "en": "Apa Penyebab Utama Sulfasi Baterai?", "id": "Disimpan Dalam Keadaan Kosong." },
    { "en": "Apa Fungsi Equalization Charge?", "id": "Cas Tegangan Tinggi Ratakan Sel." },
    { "en": "Apa Itu Specific Gravity Elektrolit?", "id": "Kepadatan Cairan Aki (Berat Jenis)." },
    { "en": "Apa Alat Ukur Berat Jenis Aki?", "id": "Hydrometer." },
    { "en": "Apa Itu Siklus Hidup Baterai (Cycle Life)?", "id": "Jumlah Cas Pakai Hingga Rusak." },
    { "en": "Apa Itu Depth Of Discharge (DoD)?", "id": "Persentase Energi Yang Terpakai." },
    { "en": "Apa Itu Autonomy Days?", "id": "Hari Cadangan Tanpa Matahari." },
    { "en": "Apa Itu C-rate Pada Baterai?", "id": "Kecepatan Cas Atau Discharge." },
    { "en": "Apa Arti Baterai 1C Discharge?", "id": "Habis Dalam Satu Jam." },
    { "en": "Apa Arti Baterai 0.5C Charge?", "id": "Penuh Dalam Dua Jam." },
    { "en": "Apa Itu Baterai LiFePO4?", "id": "Lithium Iron Phosphate (Aman/Awet)." },
    { "en": "Apa Keunggulan LiFePO4 Dibanding Li-ion?", "id": "Siklus Hidup Lebih Panjang." },
    { "en": "Apa Itu Soft Starter Motor?", "id": "Start Motor Tegangan Bertahap." },
    { "en": "Apa Itu Ramp Up Time?", "id": "Waktu Mencapai Tegangan Penuh." },
    { "en": "Apa Itu Ramp Down Time?", "id": "Waktu Berhenti Secara Perlahan." },
    { "en": "Apa Itu Kick Start Function?", "id": "Pulsa Torsi Awal Tinggi." },
    { "en": "Apa Fungsi Bypass Contactor Soft Starter?", "id": "Pindah Ke Langsung Setelah Start." },
    { "en": "Apa Itu Jogging Motor?", "id": "Gerakan Motor Sesaat (Inching)." },
    { "en": "Apa Itu Plugging Motor?", "id": "Pengereman Dengan Balik Fasa." },
    { "en": "Apa Itu Dynamic Braking?", "id": "Buang Energi Ke Resistor." },
    { "en": "Apa Fungsi MPCB?", "id": "Motor Protection Circuit Breaker." },
    { "en": "Apa Beda MPCB Dan MCB?", "id": "MPCB Bisa Stel Overload Thermal." },
    { "en": "Apa Itu Shunt Trip Coil?", "id": "Coil Trip Jarak Jauh." },
    { "en": "Apa Itu Undervoltage Release Coil?", "id": "Trip Saat Tegangan Hilang." },
    { "en": "Apa Itu Interlock Contact?", "id": "Cegah Dua Kontaktor Nyala Bersamaan." },
    { "en": "Apa Itu Latching Circuit (Self-holding)?", "id": "Kunci Kontaktor Tetap Nyala." },
    { "en": "Apa Itu Contactor Chatter?", "id": "Kontak Getar Buka Tutup Cepat." },
    { "en": "Apa Penyebab Contactor Chatter?", "id": "Tegangan Coil Drop Atau Kotor." },
    { "en": "Apa Itu Arc Flash Hazard?", "id": "Ledakan Energi Panas Busur Listrik." },
    { "en": "Apa Satuan Energi Arc Flash?", "id": "Kalori Per Sentimeter Persegi." },
    { "en": "Apa Itu APD Kategori 4?", "id": "Pakaian Tahan Arc Flash Tinggi." },
    { "en": "Apa Fungsi Hot Stick?", "id": "Tongkat Isolasi Tegangan Tinggi." },
    { "en": "Apa Fungsi Discharge Stick?", "id": "Buang Muatan Kapasitor Tegangan Tinggi." },
    { "en": "Apa Fungsi Grounding Cluster?", "id": "Kabel Grounding Portable Saat Maintenance." },
    { "en": "Apa Itu Permit To Work?", "id": "Izin Kerja Area Berbahaya." },
    { "en": "Apa Itu Confined Space Listrik?", "id": "Ruang Sempit (Manhole/Trafo Vault)." },
    { "en": "Apa Itu LOTO Electrical?", "id": "Gembok Dan Label Sumber Daya." },
    { "en": "Apa Fungsi Voltage Detector Proximity?", "id": "Cek Tegangan Tanpa Kontak Fisik." },
    { "en": "Apa Itu Busbar Tembaga?", "id": "Batang Konduktor Distribusi Daya." },
    { "en": "Apa Itu Busbar Aluminium?", "id": "Alternatif Ringan Tapi Resistansi Tinggi." },
    { "en": "Apa Fungsi Grease Pada Busbar?", "id": "Cegah Oksidasi Sambungan." },
    { "en": "Apa Itu Bimetal Lug?", "id": "Konektor Kabel Aluminium Ke Tembaga." },
    { "en": "Apa Fungsi Tanda Torsi (Torque Mark)?", "id": "Cek Visual Baut Kendor." },
    { "en": "Apa Itu Isolator Busbar?", "id": "Dudukan Busbar Tahan Tegangan." },
    { "en": "Apa Itu Knockout Punch?", "id": "Alat Lubangi Plat Panel." },
    { "en": "Apa Itu Step Drill Bit?", "id": "Mata Bor Bertingkat." },
    { "en": "Apa Itu Blind Flange?", "id": "Penutup Ujung Pipa/Panel." },
    { "en": "Apa Fungsi Kunci Panel (Panel Key)?", "id": "Buka Pintu Panel Listrik." },
    { "en": "Apa Itu Kabel Ladder Rung?", "id": "Palang Anak Tangga Kabel." },
    { "en": "Apa Itu Kabel Tray Cover?", "id": "Tutup Pelindung Kabel Tray." },
    { "en": "Apa Itu Grounding High Impedance?", "id": "Grounding Lewat Resistor Besar." },
    { "en": "Apa Itu Grounding Solid?", "id": "Grounding Langsung Ke Tanah." },
    { "en": "Apa Itu Ungrounded System?", "id": "Sistem Tanpa Referensi Ground." },
    { "en": "Apa Keuntungan Ungrounded System?", "id": "Tetap Operasi Saat Satu Fault." },
    { "en": "Apa Kerugian Ungrounded System?", "id": "Sulit Lacak Lokasi Gangguan." },
    { "en": "Apa Fungsi Zig-zag Trafo?", "id": "Buat Titik Netral Buatan." },
    { "en": "Apa Itu Ground Fault Monitor?", "id": "Deteksi Kebocoran Arus Tanah." },
    { "en": "Apa Itu CT Core Balance?", "id": "CT Nol Deteksi Arus Bocor." },
    { "en": "Apa Itu Zero Sequence Current?", "id": "Arus Gangguan Ke Tanah." },
    { "en": "Apa Itu Positive Sequence?", "id": "Urutan Fasa Normal (R-S-T)." },
    { "en": "Apa Itu Negative Sequence?", "id": "Urutan Fasa Terbalik (R-T-S)." },
    { "en": "Apa Penyebab Negative Sequence?", "id": "Beban Tidak Seimbang Atau Fault." },
    { "en": "Apa Itu Load Bank Testing?", "id": "Uji Beban Buatan Genset." },
    { "en": "Apa Fungsi Dummy Load RF?", "id": "Beban Buatan Pemancar Radio." },
    { "en": "Apa Itu SWR Meter?", "id": "Ukur Efisiensi Antena Radio." },
    { "en": "Berapa Standar Lux Pencahayaan Ruang Kantor?", "id": "Minimal 350 Hingga 500 Lux." },
    { "en": "Berapa Standar Lux Pencahayaan Gudang Arsip?", "id": "Minimal 200 Lux." },
    { "en": "Berapa Standar Lux Pencahayaan Jalan Raya Umum?", "id": "Sekitar 15 Hingga 30 Lux." },
    { "en": "Berapa Standar Lux Meja Operasi Rumah Sakit?", "id": "10.000 Hingga 20.000 Lux." },
    { "en": "Berapa Standar Lux Ruang Kelas Sekolah?", "id": "Minimal 250 Hingga 300 Lux." },
    { "en": "Berapa Standar Lux Area Parkir Indoor?", "id": "Minimal 50 Hingga 100 Lux." },
    { "en": "Apa Itu Metode Rolling Sphere Proteksi Petir?", "id": "Metode Bola Gelinding Tentukan Area Aman." },
    { "en": "Apa Sudut Proteksi Batang Franklin Standar?", "id": "Sekitar 45 Derajat Kerucut." },
    { "en": "Apa Fungsi Down Conductor Penangkal Petir?", "id": "Salurkan Arus Petir Ke Tanah." },
    { "en": "Apa Jarak Klem Kabel Down Conductor?", "id": "Maksimal 1 Hingga 1.5 Meter." },
    { "en": "Apa Itu Bak Kontrol Grounding?", "id": "Tempat Cek Sambungan Dan Nilai Tanah." },
    { "en": "Apa Fungsi Counterpoise Grounding?", "id": "Kawat Tanah Horizontal Area Berbatu." },
    { "en": "Apa Kepanjangan EVCS (Stasiun Cas Mobil)?", "id": "Electric Vehicle Charging Station." },
    { "en": "Apa Itu Slow Charging AC Mobil Listrik?", "id": "Cas Lambat Pakai Listrik Rumah." },
    { "en": "Apa Itu Fast Charging DC Mobil Listrik?", "id": "Cas Cepat Langsung Ke Baterai." },
    { "en": "Apa Fungsi OBC (On Board Charger) Mobil?", "id": "Ubah Listrik AC Grid Jadi DC." },
    { "en": "Apa Tipe Konektor Charger CCS2?", "id": "Standar Eropa Untuk AC Dan DC." },
    { "en": "Apa Tipe Konektor Charger CHAdeMO?", "id": "Standar Jepang Khusus DC Charging." },
    { "en": "Apa Tipe Konektor Charger GB/T?", "id": "Standar China." },
    { "en": "Apa Itu Preventive Maintenance (Perawatan Pencegahan)?", "id": "Perawatan Rutin Mencegah Kerusakan." },
    { "en": "Apa Itu Corrective Maintenance (Perawatan Perbaikan)?", "id": "Perbaikan Setelah Alat Rusak." },
    { "en": "Apa Itu Predictive Maintenance (Perawatan Prediksi)?", "id": "Prediksi Rusak Berdasar Data Sensor." },
    { "en": "Apa Itu Breakdown Maintenance?", "id": "Perbaikan Saat Mesin Mati Total." },
    { "en": "Apa Fungsi Vibration Analysis Motor?", "id": "Deteksi Kerusakan Bearing Atau Unbalance." },
    { "en": "Apa Fungsi Oil Analysis Trafo?", "id": "Cek Kandungan Gas Dan Kualitas Minyak." },
    { "en": "Apa Itu MTBF (Mean Time Between Failures)?", "id": "Rata-rata Waktu Antar Kerusakan." },
    { "en": "Apa Itu MTTR (Mean Time To Repair)?", "id": "Rata-rata Waktu Untuk Perbaikan." },
    { "en": "Apa Fungsi OEE (Overall Equipment Effectiveness)?", "id": "Ukuran Efektivitas Produksi Mesin." },
    { "en": "Apa Kepanjangan HVAC?", "id": "Heating Ventilation And Air Conditioning." },
    { "en": "Apa Fungsi Chiller Pada Gedung?", "id": "Pendingin Air Sentral AC Gedung." },
    { "en": "Apa Fungsi Cooling Tower?", "id": "Buang Panas Air Kondensor Ke Udara." },
    { "en": "Apa Itu AHU (Air Handling Unit)?", "id": "Unit Penanganan Udara Sentral." },
    { "en": "Apa Itu FCU (Fan Coil Unit)?", "id": "Unit Pendingin Ruangan Kecil." },
    { "en": "Apa Fungsi Damper Pada Ducting AC?", "id": "Atur Debit Aliran Udara." },
    { "en": "Apa Itu HEPA Filter?", "id": "Filter Udara Partikel Sangat Halus." },
    { "en": "Apa Kegunaan Kabel Fiber Optik Core 9/125?", "id": "Single Mode Jarak Jauh." },
    { "en": "Apa Kegunaan Kabel Fiber Optik Core 50/125?", "id": "Multi Mode OM2/OM3 Jarak Dekat." },
    { "en": "Apa Itu Splicing Loss Fiber Optik?", "id": "Rugi Daya Pada Sambungan Las." },
    { "en": "Berapa Batas Splicing Loss Yang Baik?", "id": "Kurang Dari 0.03 Desibel." },
    { "en": "Apa Itu Bending Loss Fiber Optik?", "id": "Rugi Daya Akibat Tekukan Kabel." },
    { "en": "Apa Fungsi Patch Panel Fiber Optik?", "id": "Terminal Sambungan Kabel Fiber Rak." },
    { "en": "Apa Itu Pigtail Fiber Optik?", "id": "Kabel Pendek Satu Konektor Untuk Splicing." },
    { "en": "Apa Itu Drop Wire Fiber Optik?", "id": "Kabel Masuk Ke Rumah Pelanggan." },
    { "en": "Apa Fungsi ONT (Optical Network Terminal)?", "id": "Modem Fiber Di Sisi Pelanggan." },
    { "en": "Apa Fungsi OLT (Optical Line Terminal)?", "id": "Perangkat Pusat Jaringan Fiber." },
    { "en": "Apa Perbedaan Kabel UTP Cat5e Dan Cat6?", "id": "Cat6 Frekuensi Lebih Tinggi Dan Separator." },
    { "en": "Apa Kecepatan Maksimum Kabel Cat5e?", "id": "1 Gigabit Per Detik." },
    { "en": "Apa Kecepatan Maksimum Kabel Cat6a?", "id": "10 Gigabit Per Detik." },
    { "en": "Apa Fungsi Shielding Pada Kabel STP?", "id": "Tahan Interferensi Elektromagnetik Eksternal." },
    { "en": "Apa Itu Crosstalk Pada Kabel LAN?", "id": "Bocoran Sinyal Antar Pasangan Kabel." },
    { "en": "Apa Fungsi V/f Control Pada VFD?", "id": "Jaga Rasio Tegangan Frekuensi Konstan." },
    { "en": "Apa Fungsi Slip Compensation VFD?", "id": "Stabilkan Kecepatan Saat Beban Naik." },
    { "en": "Apa Fungsi Braking Chopper VFD?", "id": "Buang Energi Balik Ke Resistor." },
    { "en": "Apa Itu Carrier Frequency VFD?", "id": "Frekuensi Switching Transistor Inverter." },
    { "en": "Apa Efek Carrier Frequency Rendah?", "id": "Motor Berisik (Dengung)." },
    { "en": "Apa Efek Carrier Frequency Tinggi?", "id": "Motor Halus Tapi Inverter Panas." },
    { "en": "Apa Fungsi DC Injection Braking?", "id": "Suntik Arus DC Untuk Pengereman." },
    { "en": "Apa Itu Proximity Sensor Induktif?", "id": "Deteksi Logam Tanpa Sentuh." },
    { "en": "Apa Itu Proximity Sensor Kapasitif?", "id": "Deteksi Benda Non-logam." },
    { "en": "Apa Itu Photoelectric Sensor Through Beam?", "id": "Pemancar Penerima Terpisah Jarak Jauh." },
    { "en": "Apa Itu Photoelectric Sensor Retro-reflective?", "id": "Pemancar Penerima Satu Unit Pakai Cermin." },
    { "en": "Apa Itu Photoelectric Sensor Diffuse?", "id": "Deteksi Pantulan Langsung Dari Objek." },
    { "en": "Apa Fungsi Limit Switch Roller?", "id": "Deteksi Posisi Mekanik Dengan Roda." },
    { "en": "Apa Fungsi Float Switch Mekanik?", "id": "Saklar Pelampung Level Air." },
    { "en": "Apa Itu Pressure Switch?", "id": "Saklar Berbasis Tekanan Fluida." },
    { "en": "Apa Satuan Tekanan Bar Ke PSI?", "id": "1 Bar Sekitar 14.5 PSI." },
    { "en": "Apa Itu Thermowell Sensor Suhu?", "id": "Selongsong Pelindung Sensor Di Pipa." },
    { "en": "Apa Fungsi Transmitter 4-20mA?", "id": "Kirim Data Analog Jarak Jauh." },
    { "en": "Kenapa Menggunakan Arus 4-20mA?", "id": "Tahan Noise Dan Deteksi Kabel Putus." },
    { "en": "Apa Itu HART Protocol?", "id": "Komunikasi Digital Tumpangan Analog." },
    { "en": "Apa Fungsi Multimeter Kategori CAT III?", "id": "Aman Untuk Instalasi Listrik Gedung." },
    { "en": "Apa Fungsi Multimeter Kategori CAT IV?", "id": "Aman Untuk Sumber Listrik Luar." },
    { "en": "Apa Bahaya Mengukur Resistansi Saat Bertegangan?", "id": "Multimeter Bisa Rusak Meledak." },
    { "en": "Apa Fungsi Low Pass Filter Audio?", "id": "Teruskan Bass Potong Treble." },
    { "en": "Apa Fungsi High Pass Filter Audio?", "id": "Teruskan Treble Potong Bass." },
    { "en": "Apa Itu Notch Filter Harmonisa?", "id": "Buang Satu Frekuensi Spesifik." },
    { "en": "Apa Itu Total Demand Distortion (TDD)?", "id": "Distorsi Harmonisa Arus Beban Puncak." },
    { "en": "Apa Itu Point Of Common Coupling (PCC)?", "id": "Titik Sambung Pelanggan Dan PLN." },
    { "en": "Apa Penyebab Voltage Unbalance?", "id": "Beban Satu Fasa Tidak Merata." },
    { "en": "Apa Efek Voltage Unbalance Pada Motor?", "id": "Motor Cepat Panas Dan Torsi Turun." },
    { "en": "Apa Itu Ferroresonance?", "id": "Osilasi Tegangan Lebih Trafo Kapasitor." },
    { "en": "Apa Fungsi Kapasitor Snubber?", "id": "Redam Lonjakan Tegangan Switching." },
    { "en": "Apa Fungsi Varistor (MOV) Pada PCB?", "id": "Korban Diri Saat Tegangan Lonjak." },
    { "en": "Apa Fungsi Thermistor NTC Inrush?", "id": "Hambat Arus Kejut Saat Start." },
    { "en": "Apa Fungsi Fuse Kaca (Glass Fuse)?", "id": "Proteksi Arus Lebih Perangkat Kecil." },
    { "en": "Apa Itu Ceramic Fuse (Sekering Keramik)?", "id": "Tahan Ledakan Arus Hubung Singkat." },
    { "en": "Apa Itu Resettable Fuse (Polyfuse)?", "id": "Nyambung Kembali Setelah Dingin." },
    { "en": "Apa Kode Warna Resistor 100k Ohm?", "id": "Cokelat Hitam Kuning Emas." },
    { "en": "Apa Kode Warna Resistor 2k2 Ohm?", "id": "Merah Merah Merah Emas." },
    { "en": "Apa Kode Warna Resistor 470 Ohm?", "id": "Kuning Ungu Cokelat Emas." },
    { "en": "Apa Arti Kode Kapasitor 473?", "id": "47 Nano Farad." },
    { "en": "Apa Arti Kode Kapasitor 105?", "id": "1 Mikro Farad." },
    { "en": "Apa Itu Induktor Toroid?", "id": "Lilitan Kawat Pada Inti Cincin." },
    { "en": "Apa Fungsi Induktor Common Mode Choke?", "id": "Filter Noise EMI Frekuensi Tinggi." },
    { "en": "Apa Itu Ferit Bead Kabel?", "id": "Cincin Magnet Redam Interferensi RF." },
    { "en": "Apa Fungsi Crystal Oscillator?", "id": "Pembangkit Frekuensi Stabil Digital." },
    { "en": "Apa Itu Piezo Buzzer Pasif?", "id": "Perlu Sinyal Nada Untuk Bunyi." },
    { "en": "Apa Itu Piezo Buzzer Aktif?", "id": "Langsung Bunyi Diberi Tegangan DC." },
    { "en": "Apa Fungsi Relay 5 Kaki (SPDT)?", "id": "Satu Induk Dua Cabang (NC/NO)." },
    { "en": "Apa Fungsi Transistor PNP Sebagai Saklar?", "id": "Aktif Saat Basis Diberi Low." },
    { "en": "Apa Itu PoE Type 1?", "id": "Daya Maksimal 15.4 Watt." },
    { "en": "Apa Itu PoE Type 2 (PoE+)?", "id": "Daya Maksimal 30 Watt." },
    { "en": "Apa Itu PoE Type 3 (PoE++)?", "id": "Daya Maksimal 60 Watt." },
    { "en": "Apa Itu PoE Type 4?", "id": "Daya Maksimal 100 Watt." },
    { "en": "Apa Fungsi Optocoupler 4N25?", "id": "Isolasi Sinyal Fototransistor Basis Keluar." },
    { "en": "Apa Beda PC817 Dan 4N25?", "id": "4N25 Punya Pin Basis Eksternal." },
    { "en": "Apa Fungsi IC TL431?", "id": "Referensi Tegangan Shunt Presisi." },
    { "en": "Apa Tegangan Referensi TL431?", "id": "2.5 Volt." },
    { "en": "Apa Fungsi IC LM317T?", "id": "Regulator Positif Adjustable 1.5 Ampere." },
    { "en": "Apa Rumus Vout LM317?", "id": "1.25 X (1 + R2/R1)." },
    { "en": "Apa Fungsi Heatsink TO-220?", "id": "Buang Panas Komponen Paket TO-220." },
    { "en": "Apa Itu Thermal Resistance Heatsink?", "id": "Kemampuan Hambat Aliran Panas." },
    { "en": "Apa Satuan Thermal Resistance?", "id": "Derajat Celcius Per Watt." },
    { "en": "Apa Fungsi Fan DC Brushless?", "id": "Pendingin Elektronik Minim Gesekan." },
    { "en": "Apa Kabel Merah Pada Fan PC?", "id": "Tegangan Positif 12 Volt." },
    { "en": "Apa Kabel Hitam Pada Fan PC?", "id": "Ground Atau Negatif." },
    { "en": "Apa Kabel Kuning Pada Fan PC?", "id": "Sinyal Tachometer (RPM)." },
    { "en": "Apa Kabel Biru Pada Fan PC?", "id": "Sinyal PWM Control." },
    { "en": "Apa Itu Solder Bar (Batang)?", "id": "Timah Solder Untuk Solder Pot." },
    { "en": "Apa Fungsi Solder Pot?", "id": "Celup Kabel Atau Komponen Besar." },
    { "en": "Apa Itu Tinning Kabel?", "id": "Melapisi Ujung Kabel Dengan Timah." },
    { "en": "Apa Tanda IC Rusak Terbakar?", "id": "Lubang Kecil Atau Gembung." },
    { "en": "Apa Tanda Elco Rusak Kering?", "id": "Kapasitas Turun ESR Naik." },
    { "en": "Apa Fungsi Kapasitor Snubber Triac?", "id": "Lindungi Triac Dari dv/dt." },
    { "en": "Apa Itu dv/dt Pada Thyristor?", "id": "Laju Perubahan Tegangan Per Waktu." },
    { "en": "Apa Itu di/dt Pada Thyristor?", "id": "Laju Perubahan Arus Per Waktu." },
    { "en": "Apa Fungsi Fuse Thermal Rice Cooker?", "id": "Putus Saat Suhu 150-170 Celcius." },
    { "en": "Apa Posisi Pasang Thermal Fuse?", "id": "Menempel Erat Pada Panci/Elemen." },
    { "en": "Apa Itu Limit Switch NC?", "id": "Normally Closed (Putus Saat Ditekan)." },
    { "en": "Apa Itu Limit Switch NO?", "id": "Normally Open (Nyambung Saat Ditekan)." },
    { "en": "Apa Fungsi Microswitch Mouse?", "id": "Tombol Klik Kecil Presisi." },
    { "en": "Apa Itu Rotary Switch?", "id": "Saklar Putar Banyak Posisi." },
    { "en": "Apa Fungsi Dip Switch?", "id": "Saklar Setting Konfigurasi Manual." },
    { "en": "Apa Itu Toggle Switch SPST?", "id": "Saklar Tuas Satu Jalur." },
    { "en": "Apa Itu Toggle Switch DPDT?", "id": "Saklar Tuas Dua Jalur Ganda." },
    { "en": "Apa Fungsi Keypad Matriks 4x4?", "id": "Input Angka 16 Tombol." },
    { "en": "Apa Jumlah Pin Keypad 4x4?", "id": "8 Pin (4 Baris 4 Kolom)." },
    { "en": "Apa Fungsi Shift Register 74HC164?", "id": "Serial In Parallel Out Sederhana." },
    { "en": "Apa Fungsi IC Driver Motor L293D?", "id": "H-Bridge Ganda Arus Kecil." },
    { "en": "Apa Beda L293D Dan L298N?", "id": "L293D Ada Dioda Internal." },
    { "en": "Apa Fungsi Sensor Jarak VL53L0X?", "id": "Laser Time Of Flight (ToF)." },
    { "en": "Apa Keunggulan Sensor ToF?", "id": "Akurasi Tinggi Tidak Terpengaruh Warna." },
    { "en": "Apa Fungsi Sensor Warna TCS34725?", "id": "Deteksi Warna RGB Dan Cahaya." },
    { "en": "Apa Fungsi Sensor Gestur APDS9960?", "id": "Deteksi Gerakan Tangan Jarak Dekat." },
    { "en": "Apa Protokol Komunikasi APDS9960?", "id": "I2C." },
    { "en": "Apa Fungsi Module NRF24L01?", "id": "Komunikasi Radio 2.4GHz Murah." },
    { "en": "Apa Jarak Maksimal NRF24L01 PA/LNA?", "id": "Bisa Mencapai 1 Kilometer." },
    { "en": "Apa Tegangan Suplai NRF24L01?", "id": "Wajib 3.3 Volt." },
    { "en": "Apa Pin CE Pada NRF24L01?", "id": "Chip Enable (Aktifkan Mode RX/TX)." },
    { "en": "Apa Pin CSN Pada NRF24L01?", "id": "Chip Select Not (SPI)." },
    { "en": "Apa Fungsi Kapasitor Di NRF24L01?", "id": "Stabilkan Tegangan Cegah Putus Sinyal." },
    { "en": "Apa Itu LoRaWAN?", "id": "Protokol Jaringan Area Luas LoRa." },
    { "en": "Apa Itu Gateway LoRaWAN?", "id": "Penghubung Node Ke Internet Server." },
    { "en": "Apa Fungsi Node Sensor LoRa?", "id": "Kirim Data Sensor Jarak Jauh." },
    { "en": "Apa Itu Spreading Factor LoRa?", "id": "Pengaturan Jarak Vs Kecepatan Data." },
    { "en": "Apa Efek Spreading Factor Tinggi?", "id": "Jarak Jauh Tapi Data Lambat." },
    { "en": "Apa Efek Spreading Factor Rendah?", "id": "Jarak Dekat Tapi Data Cepat." },
    { "en": "Apa Frekuensi LoRa Asia (AS923)?", "id": "923 Mega Hertz." },
    { "en": "Apa Fungsi Antena Helical?", "id": "Antena Kawat Melingkar Seperti Per." },
    { "en": "Apa Fungsi Antena Rubber Duck?", "id": "Antena Karet Standar HT/Router." },
    { "en": "Apa Itu Gain Antena 3dBi?", "id": "Penguatan Sinyal Standar Antena Kecil." },
    { "en": "Apa Itu Gain Antena 9dBi?", "id": "Penguatan Tinggi Jangkauan Lebih Jauh." },
    { "en": "Apa Konektor Antena SMA Male?", "id": "Jarum Di Tengah, Ulir Dalam." },
    { "en": "Apa Konektor Antena SMA Female?", "id": "Lubang Di Tengah, Ulir Luar." },
    { "en": "Apa Konektor Antena RP-SMA Male?", "id": "Lubang Di Tengah (Reverse Polarity)." },
    { "en": "Apa Konektor Antena RP-SMA Female?", "id": "Jarum Di Tengah (Reverse Polarity)." },
    { "en": "Apa Kabel Coaxial RG-174?", "id": "Kabel RF Tipis Fleksibel." },
    { "en": "Apa Kabel Coaxial RG-213?", "id": "Kabel RF Tebal Rugi Rendah." },
    { "en": "Apa Fungsi Dummy Load 50 Ohm?", "id": "Tes Pemancar Tanpa Pancar Sinyal." },
    { "en": "Apa Fungsi Attenuator RF?", "id": "Turunkan Level Sinyal RF." },
    { "en": "Apa Itu LNA (Low Noise Amplifier)?", "id": "Perkuat Sinyal Lemah Di Penerima." },
    { "en": "Apa Itu PA (Power Amplifier) RF?", "id": "Perkuat Sinyal Sebelum Dipancar." },
    { "en": "Apa Fungsi Filter SAW?", "id": "Filter Gelombang Akustik Permukaan Presisi." },
    { "en": "Apa Itu Software Defined Radio (SDR)?", "id": "Radio Berbasis Perangkat Lunak Komputer." },
    { "en": "Apa Chip SDR Murah Populer?", "id": "RTL2832U." },
    { "en": "Apa Rentang Frekuensi RTL-SDR?", "id": "24 MHz Hingga 1.7 GHz." },
    { "en": "Apa Itu Modulasi AM-DSB-SC?", "id": "Amplitudo Ganda Tanpa Carrier." },
    { "en": "Apa Itu Modulasi SSB-USB?", "id": "Single Side Band Upper Side." },
    { "en": "Apa Itu Modulasi SSB-LSB?", "id": "Single Side Band Lower Side." },
    { "en": "Apa Fungsi Tuner Kaleng Radio?", "id": "Modul Penerima RF Analog/Digital." },
    { "en": "Apa Tegangan Tuning Varco Digital?", "id": "Biasanya Hingga 30 Volt." },
    { "en": "Apa Fungsi Pin AGC Tuner?", "id": "Kontrol Gain Otomatis Dari IC." },
    { "en": "Apa Itu Gelombang Mikro (Microwave)?", "id": "Frekuensi Di Atas 1 GHz." },
    { "en": "Apa Fungsi Magnetron Oven?", "id": "Pembangkit Gelombang Mikro Pemanas." },
    { "en": "Apa Tegangan Kerja Magnetron?", "id": "Sekitar 4000 Volt DC." },
    { "en": "Apa Fungsi Kapasitor High Voltage Oven?", "id": "Pengganda Tegangan Untuk Magnetron." },
    { "en": "Apa Fungsi Dioda High Voltage Oven?", "id": "Penyearah Tegangan Tinggi Ke Ground." },
    { "en": "Apa Bahaya Membuka Microwave Oven?", "id": "Kapasitor HV Simpan Muatan Mematikan." },
    { "en": "Apa Fungsi Mica Sheet Microwave?", "id": "Lindungi Lubang Waveguide Dari Cipratan." },
    { "en": "Apa Itu Turntable Motor Microwave?", "id": "Motor Pemutar Piringan Makanan." },
    { "en": "Apa Tegangan Motor Turntable?", "id": "Biasanya 21 Volt Atau 220V AC." },
    { "en": "Apa Fungsi Thermostat KST?", "id": "Saklar Suhu Setrika Listrik." },
    { "en": "Apa Prinsip Kerja Setrika Listrik?", "id": "Bimetal Melengkung Saat Panas." },
    { "en": "Apa Fungsi Fuse Thermal Setrika?", "id": "Pengaman Terakhir Jika Thermostat Rusak." },
    { "en": "Apa Elemen Pemanas Setrika?", "id": "Kawat Nikelin Dalam Pipa Besi." },
    { "en": "Apa Penyebab Setrika Tidak Panas?", "id": "Kabel Putus Atau Fuse Putus." },
    { "en": "Apa Fungsi Lampu Indikator Setrika?", "id": "Tanda Elemen Sedang Bekerja." },
    { "en": "Apa Resistor Seri Lampu Neon Indikator?", "id": "Sekitar 100 Kilo Ohm Hingga 220k." },
    { "en": "Apa Fungsi Solenoid Valve Mesin Cuci?", "id": "Kran Air Masuk Elektrik." },
    { "en": "Apa Fungsi Water Level Sensor Mesin Cuci?", "id": "Deteksi Tinggi Air Lewat Tekanan." },
    { "en": "Apa Fungsi T-Con Board Pada TV LED?", "id": "Olah Sinyal Gambar Ke Panel Layar." },
    { "en": "Apa Gejala Kerusakan T-Con Board?", "id": "Gambar Klise, Bergaris, Atau Blank." },
    { "en": "Apa Itu COF (Chip On Film)?", "id": "Chip Driver Pada Kabel Fleksibel Panel." },
    { "en": "Apa Fungsi ACF Tape (Anisotropic Conductive Film)?", "id": "Lem Perekat Konduktif Jalur LCD." },
    { "en": "Apa Itu Bonding Machine LCD?", "id": "Mesin Pres Kabel Fleksibel COF." },
    { "en": "Apa Fungsi Lapisan Polarizer LCD?", "id": "Saring Arah Cahaya Agar Gambar Muncul." },
    { "en": "Apa Tanda Lapisan Polarizer Rusak?", "id": "Gambar Terlihat Berkerut Atau Putih." },
    { "en": "Apa Fungsi Backlight Inverter TV LCD Lama?", "id": "Pembangkit Tegangan Tinggi Lampu CCFL." },
    { "en": "Apa Tegangan Output Inverter CCFL?", "id": "Ribuan Volt AC Frekuensi Tinggi." },
    { "en": "Apa Fungsi Diffuser Sheet Pada Layar?", "id": "Ratakan Cahaya Backlight Ke Seluruh Layar." },
    { "en": "Apa Itu Dead Pixel Pada Monitor?", "id": "Titik Piksel Mati Tidak Berubah Warna." },
    { "en": "Apa Itu Stuck Pixel?", "id": "Titik Piksel Macet Di Satu Warna." },
    { "en": "Apa Warna Kabel Fasa L1 PUIL 2011?", "id": "Cokelat." },
    { "en": "Apa Warna Kabel Fasa L2 PUIL 2011?", "id": "Hitam." },
    { "en": "Apa Warna Kabel Fasa L3 PUIL 2011?", "id": "Abu-abu." },
    { "en": "Apa Warna Kabel Netral PUIL 2011?", "id": "Biru." },
    { "en": "Apa Warna Kabel Grounding PUIL 2011?", "id": "Loreng Hijau Kuning." },
    { "en": "Apa Warna Kabel Fasa R Standar Lama?", "id": "Merah." },
    { "en": "Apa Warna Kabel Fasa S Standar Lama?", "id": "Kuning." },
    { "en": "Apa Warna Kabel Fasa T Standar Lama?", "id": "Hitam." },
    { "en": "Apa Itu ODP (Optical Distribution Point)?", "id": "Kotak Bagi Fiber Optik Di Tiang." },
    { "en": "Apa Itu ODC (Optical Distribution Cabinet)?", "id": "Lemari Pembagi Fiber Optik Utama." },
    { "en": "Apa Itu Splitter Fiber Optik?", "id": "Pecah Satu Core Jadi Banyak Core." },
    { "en": "Apa Rasio Splitter Umum Rumahan?", "id": "1 Banding 8 Atau 1 Banding 16." },
    { "en": "Apa Itu Kabel Drop Core?", "id": "Kabel Fiber Pipih Masuk Rumah." },
    { "en": "Apa Itu Fast Connector Fiber Optik?", "id": "Konektor Pasang Manual Tanpa Splicing." },
    { "en": "Apa Itu Stripper Drop Core?", "id": "Tang Kupas Kulit Kabel Fiber Pipih." },
    { "en": "Apa Itu Stripper Cladding (Miller)?", "id": "Tang Kupas Lapisan Kaca Fiber." },
    { "en": "Apa Itu Fiber Cleaver?", "id": "Alat Potong Kaca Fiber Presisi." },
    { "en": "Apa Cairan Pembersih Kaca Fiber?", "id": "Alkohol 99 Persen." },
    { "en": "Apa Itu Tisu Kimwipes?", "id": "Tisu Khusus Serat Optik Tanpa Debu." },
    { "en": "Apa Fungsi Sleeve Protector Fiber?", "id": "Lindungi Sambungan Hasil Splicing." },
    { "en": "Apa Itu Transceiver SFP BiDi?", "id": "Komunikasi Dua Arah Satu Kabel Fiber." },
    { "en": "Apa Panjang Gelombang Fiber Single Mode Umum?", "id": "1310 nm Dan 1550 nm." },
    { "en": "Apa Fungsi Binding Post Speaker?", "id": "Terminal Jepit Kabel Di Box Speaker." },
    { "en": "Apa Itu Banana Plug?", "id": "Konektor Kabel Speaker Praktis." },
    { "en": "Apa Itu Konektor Spades (Garpu)?", "id": "Konektor Kabel Speaker Bentuk U." },
    { "en": "Apa Fungsi Bi-wiring Speaker?", "id": "Kabel Terpisah Untuk Bass Dan Treble." },
    { "en": "Apa Itu Speaker Impedance Curve?", "id": "Grafik Hambatan Berubah Sesuai Frekuensi." },
    { "en": "Apa Itu Resonansi Box Speaker?", "id": "Frekuensi Getar Alami Kotak." },
    { "en": "Apa Fungsi Glasswool/Dacron Di Speaker?", "id": "Peredam Gema Dalam Box." },
    { "en": "Apa Itu Bass Reflex Port?", "id": "Lubang Angin Penambah Respon Bass." },
    { "en": "Apa Fungsi Heat Shrink Busbar?", "id": "Isolasi Warna Warni Batang Tembaga." },
    { "en": "Apa Itu Skun Ring (O-Ring)?", "id": "Terminal Kabel Bentuk Cincin Baut." },
    { "en": "Apa Itu Skun Garpu (Y-Terminal)?", "id": "Terminal Kabel Bentuk Huruf Y." },
    { "en": "Apa Itu Skun Pin (Jarum)?", "id": "Terminal Kabel Masuk Lubang MCB." },
    { "en": "Apa Fungsi Magnetic Contactor 3 Pole?", "id": "Saklar Utama Motor 3 Fasa." },
    { "en": "Apa Fungsi Auxiliary Contact (Kontak Bantu)?", "id": "Saklar Tambahan Untuk Indikator/Kontrol." },
    { "en": "Apa Itu Thermal Overload Class 10?", "id": "Trip Cepat (10 Detik) Saat Overload." },
    { "en": "Apa Itu Thermal Overload Class 20?", "id": "Trip Lambat (20 Detik) Beban Berat." },
    { "en": "Apa Fungsi Phase Failure Relay?", "id": "Proteksi Jika Satu Fasa Hilang." },
    { "en": "Apa Fungsi Phase Reversal Protection?", "id": "Proteksi Jika Urutan Fasa Terbalik." },
    { "en": "Apa Itu Hour Meter Panel?", "id": "Penghitung Jam Operasi Mesin." },
    { "en": "Apa Fungsi Ampere Selector Switch?", "id": "Pilih Fasa Arus Yang Ditampilkan." },
    { "en": "Apa Fungsi Volt Selector Switch?", "id": "Pilih Tegangan Antar Fasa/Netral." },
    { "en": "Apa Itu Current Transformer (CT) 100/5A?", "id": "Ubah 100A Jadi 5A Untuk Metering." },
    { "en": "Apa Kelas Akurasi CT Metering?", "id": "Kelas 0.5 Atau 0.2 (Presisi)." },
    { "en": "Apa Kelas Akurasi CT Proteksi?", "id": "Kelas 5P10 Atau 5P20." },
    { "en": "Apa Arti Kode 5P20 Pada CT?", "id": "Error 5% Pada 20 Kali Arus." },
    { "en": "Apa Fungsi Kwh Meter 3 Fasa?", "id": "Ukur Energi Listrik Sistem 3 Fasa." },
    { "en": "Apa Itu Kwh Meter CT Connected?", "id": "Meter Menggunakan CT Tidak Langsung." },
    { "en": "Apa Itu MCB DC (Direct Current)?", "id": "Pemutus Arus Khusus Listrik Searah." },
    { "en": "Apa Bahaya Pakai MCB AC Untuk DC?", "id": "Busur Api Sulit Padam Saat Trip." },
    { "en": "Apa Fungsi Fuse Holder Din Rail?", "id": "Rumah Sekering Pasang Di Rel Panel." },
    { "en": "Apa Itu Kabel NYAF 0.75mm?", "id": "Kabel Serabut Kecil Kontrol Panel." },
    { "en": "Apa Fungsi Penanda Kabel (Cable Marker)?", "id": "Label Nomor Identifikasi Ujung Kabel." },
    { "en": "Apa Itu Sensor PNP (Sourcing)?", "id": "Output Keluarkan Tegangan Positif." },
    { "en": "Apa Itu Sensor NPN (Sinking)?", "id": "Output Sambung Ke Negatif (Ground)." },
    { "en": "Apa Cara Cek Sensor NPN Dengan Voltmeter?", "id": "Ukur Antara Kabel Output Dan Positif." },
    { "en": "Apa Cara Cek Sensor PNP Dengan Voltmeter?", "id": "Ukur Antara Kabel Output Dan Negatif." },
    { "en": "Apa Itu Sensor Namur?", "id": "Sensor Arus Rendah Area Berbahaya." },
    { "en": "Apa Fungsi Isolator Barrier?", "id": "Pemisah Sinyal Area Aman Dan Bahaya." },
    { "en": "Apa Itu PLC Scan Time?", "id": "Waktu Baca Input Proses Tulis Output." },
    { "en": "Apa Satuan PLC Scan Time?", "id": "Milidetik." },
    { "en": "Apa Fungsi Watchdog Timer PLC?", "id": "Deteksi Error Jika Scan Time Lama." },
    { "en": "Apa Itu Forcing I/O PLC?", "id": "Paksa Nilai Input/Output Lewat Software." },
    { "en": "Apa Fungsi Memory Retentive PLC?", "id": "Simpan Nilai Saat Listrik Mati." },
    { "en": "Apa Itu Cold Start PLC?", "id": "Reset Semua Data Ke Awal." },
    { "en": "Apa Itu Warm Start PLC?", "id": "Lanjut Proses Simpan Data Terakhir." },
    { "en": "Apa Kabel Komunikasi PLC Ke HMI?", "id": "Ethernet, RS232, Atau RS485." },
    { "en": "Apa Itu HMI Macro Script?", "id": "Program Logika Sederhana Di Layar." },
    { "en": "Apa Fungsi Alarm History HMI?", "id": "Rekam Jejak Gangguan Mesin." },
    { "en": "Apa Itu Trend Graph HMI?", "id": "Grafik Data Sensor Berjalan Waktu." },
    { "en": "Apa Fungsi Resep (Recipe) HMI?", "id": "Simpan Parameter Produksi Berbeda." },
    { "en": "Apa Itu Password Level HMI?", "id": "Batasi Akses Operator Dan Engineer." },
    { "en": "Apa Fungsi VNC Server HMI?", "id": "Pantau Layar HMI Dari Jarak Jauh." },
    { "en": "Apa Itu IP67 Pada Sensor?", "id": "Tahan Debu Dan Celup Air Sesaat." },
    { "en": "Apa Itu IP69K Pada Sensor?", "id": "Tahan Semprotan Air Panas Tekanan Tinggi." },
    { "en": "Apa Fungsi Load Cell Amplifier?", "id": "Perkuat Sinyal mV Jembatan Timbang." },
    { "en": "Apa Output Standar Load Cell Amplifier?", "id": "0-10 Volt Atau 4-20 mA." },
    { "en": "Apa Itu Junction Box Load Cell?", "id": "Gabung Sinyal 4 Sensor Timbangan." },
    { "en": "Apa Fungsi Trimpot Di Junction Box?", "id": "Ratakan Nilai Tiap Sudut Timbangan." },
    { "en": "Apa Itu Tare Function Timbangan?", "id": "Nol-kan Berat Wadah." },
    { "en": "Apa Itu Kalibrasi Span Timbangan?", "id": "Setting Nilai Berat Maksimum." },
    { "en": "Apa Itu Kalibrasi Zero Timbangan?", "id": "Setting Titik Nol Tanpa Beban." },
    { "en": "Apa Penyebab Timbangan Tidak Stabil?", "id": "Grounding Buruk Atau Kabel Lembap." },
    { "en": "Apa Itu Checkweigher?", "id": "Timbangan Cek Berat Produk Berjalan." },
    { "en": "Apa Fungsi Metal Detector Conveyor?", "id": "Deteksi Logam Dalam Produk Makanan." },
    { "en": "Apa Prinsip Kerja Metal Detector?", "id": "Gangguan Medan Elektromagnetik Koil." },
    { "en": "Apa Itu Topologi Flyback Converter?", "id": "SMPS Terisolasi Daya Rendah." },
    { "en": "Apa Komponen Utama Flyback SMPS?", "id": "Trafo Ferit Dan MOSFET Switching." },
    { "en": "Apa Fungsi Optocoupler Pada SMPS?", "id": "Umpan Balik Tegangan Output Stabil." },
    { "en": "Apa Itu PWM Controller SMPS?", "id": "Chip Pengatur Lebar Pulsa Switching." },
    { "en": "Apa Bahaya Kapasitor Primer SMPS?", "id": "Simpan Tegangan Tinggi 300 Volt." },
    { "en": "Apa Fungsi NTC Pada Input SMPS?", "id": "Redam Arus Kejut Saat Dinyalakan." },
    { "en": "Apa Fungsi EMI Filter Input SMPS?", "id": "Cegah Noise Masuk Jala-jala PLN." },
    { "en": "Apa Itu Topologi Buck Converter?", "id": "Penurun Tegangan DC Non-isolasi." },
    { "en": "Apa Itu Topologi Boost Converter?", "id": "Penaik Tegangan DC Non-isolasi." },
    { "en": "Apa Fungsi Induktor Pada Buck Converter?", "id": "Simpan Energi Magnetik Saat Switching." },
    { "en": "Apa Fungsi Instruksi MOV Pada PLC?", "id": "Pindahkan Data Antar Register Memori." },
    { "en": "Apa Fungsi Instruksi CMP Pada PLC?", "id": "Bandingkan Dua Nilai Data." },
    { "en": "Apa Itu Bit Memory (M) PLC?", "id": "Relay Internal Semu Dalam PLC." },
    { "en": "Apa Itu Data Register (D) PLC?", "id": "Memori Penyimpan Angka 16 Bit." },
    { "en": "Apa Fungsi High Speed Counter (HSC)?", "id": "Hitung Pulsa Encoder Kecepatan Tinggi." },
    { "en": "Apa Itu Pulse Output PLC?", "id": "Kirim Pulsa Ke Driver Stepper/Servo." },
    { "en": "Apa Fungsi Modul Analog Input PLC?", "id": "Baca Sinyal Sensor 4-20mA/0-10V." },
    { "en": "Apa Fungsi Modul Analog Output PLC?", "id": "Kendalikan Inverter Atau Control Valve." },
    { "en": "Apa Itu Sink Input PLC?", "id": "Input Aktif Saat Diberi Ground." },
    { "en": "Apa Itu Source Input PLC?", "id": "Input Aktif Saat Diberi Positif." },
    { "en": "Apa Itu Thermocouple Transmitter?", "id": "Ubah mV Jadi 4-20 mA." },
    { "en": "Apa Kelebihan Sinyal Arus 4-20mA?", "id": "Tidak Drop Tegangan Jarak Jauh." },
    { "en": "Apa Itu Dead Band Pressure Switch?", "id": "Selisih Tekanan Cut-in Dan Cut-out." },
    { "en": "Apa Fungsi Siphon Tube Pressure Gauge?", "id": "Lindungi Gauge Dari Uap Panas." },
    { "en": "Apa Fungsi Snubber Pressure Gauge?", "id": "Redam Getaran Jarum Akibat Pulsasi." },
    { "en": "Apa Itu Manifold Valve 3 Way?", "id": "Katup Isolasi Dan Kalibrasi Transmitter." },
    { "en": "Apa Itu Bourdon Tube?", "id": "Elemen Mekanis Pengukur Tekanan." },
    { "en": "Apa Itu Diaphragm Seal?", "id": "Membran Pelindung Sensor Dari Cairan." },
    { "en": "Apa Fungsi Rotameter (Flow Meter)?", "id": "Ukur Debit Air Tabung Kaca." },
    { "en": "Apa Itu Orifice Plate?", "id": "Plat Lubang Pengukur Beda Tekanan." },
    { "en": "Apa Itu Braking Resistor VFD?", "id": "Buang Energi Regeneratif Jadi Panas." },
    { "en": "Apa Itu DC Bus VFD?", "id": "Tegangan DC Setelah Penyearah Input." },
    { "en": "Berapa Tegangan DC Bus Input 380V?", "id": "Sekitar 540 Volt DC." },
    { "en": "Apa Fungsi IGBT Module VFD?", "id": "Switching Output Ke Motor AC." },
    { "en": "Apa Tanda IGBT VFD Rusak?", "id": "Motor Tidak Jalan Atau Short." },
    { "en": "Apa Itu Parameter Accel Time?", "id": "Waktu Kecepatan Nol Ke Maksimum." },
    { "en": "Apa Itu Parameter Decel Time?", "id": "Waktu Kecepatan Maksimum Ke Nol." },
    { "en": "Apa Fungsi Jog Frequency?", "id": "Frekuensi Jalan Pelan Untuk Setup." },
    { "en": "Apa Itu Multi-step Speed VFD?", "id": "Kecepatan Bertingkat Sesuai Kombinasi Input." },
    { "en": "Apa Fungsi Analog Output VFD?", "id": "Kirim Data RPM/Arus Ke Meter." },
    { "en": "Apa Fungsi Internal Pull-up Arduino?", "id": "Resistor Internal Ke Positif 5V." },
    { "en": "Bagaimana Mengaktifkan Pull-up Internal?", "id": "Mode INPUT_PULLUP Di Void Setup." },
    { "en": "Apa Fungsi Pin Reset ESP8266?", "id": "Restart Modul Secara Hardware." },
    { "en": "Apa Itu Baud Rate 115200?", "id": "Kecepatan Komunikasi Serial Cepat." },
    { "en": "Apa Fungsi Library PubSubClient?", "id": "Pustaka MQTT Untuk Arduino ESP." },
    { "en": "Apa Itu Deep Sleep Wake Stub?", "id": "Kode Jalan Segera Setelah Bangun." },
    { "en": "Apa Fungsi EEPROM.write()?", "id": "Tulis Satu Byte Ke Memori." },
    { "en": "Apa Fungsi EEPROM.commit() ESP?", "id": "Simpan Perubahan Ke Flash Memori." },
    { "en": "Apa Itu SPIFFS/LittleFS?", "id": "Sistem File Di Flash ESP." },
    { "en": "Apa Fungsi Web Server ESP32?", "id": "Halaman Web Kendali Dari Chip." },
    { "en": "Apa Itu Kabel Coaxial RG-58?", "id": "Kabel Antena Radio 50 Ohm." },
    { "en": "Apa Itu Kabel Coaxial RG-8?", "id": "Kabel Antena Besar Rugi Rendah." },
    { "en": "Apa Konektor PL-259?", "id": "Konektor UHF Male Kabel Coax." },
    { "en": "Apa Konektor SO-239?", "id": "Soket UHF Female Di Radio." },
    { "en": "Apa Itu Kabel Twin Lead?", "id": "Kabel Antena TV Pita Pipih." },
    { "en": "Apa Konektor N-Type?", "id": "Konektor RF Tahan Cuaca Frekuensi Tinggi." },
    { "en": "Apa Konektor BNC Male?", "id": "Konektor Putar Kunci Osiloskop/CCTV." },
    { "en": "Apa Fungsi SWR Meter Jarum?", "id": "Ukur Matching Antena Dan Radio." },
    { "en": "Apa Impedansi Kabel CCTV RG-6?", "id": "75 Ohm." },
    { "en": "Apa Impedansi Kabel Transceiver Radio?", "id": "50 Ohm." },
    { "en": "Apa Itu GFI (Ground Fault Interrupter)?", "id": "Soket Amerika Dengan Proteksi Bocor." },
    { "en": "Apa Fungsi Safety Padlock?", "id": "Gembok Isolasi Energi LOTO." },
    { "en": "Apa Fungsi Tagout Label?", "id": "Tanda Peringatan Sedang Ada Perbaikan." },
    { "en": "Apa Fungsi Scaffolding Tag?", "id": "Status Keamanan Perancah (Hijau/Merah)." },
    { "en": "Apa Itu Permit To Work (PTW)?", "id": "Dokumen Izin Kerja Risiko Tinggi." },
    { "en": "Apa Itu Job Safety Analysis (JSA)?", "id": "Analisis Bahaya Sebelum Kerja Dimulai." },
    { "en": "Apa Itu Toolbox Meeting?", "id": "Briefing Keselamatan Sebelum Mulai Kerja." },
    { "en": "Apa Bahaya Bekerja Di Ketinggian?", "id": "Jatuh Dan Kejatuhan Benda." },
    { "en": "Apa Fungsi Safety Belt Lineman?", "id": "Sabuk Pengaman Panjat Tiang Listrik." },
    { "en": "Apa Itu Ping Latency?", "id": "Waktu Bolak-balik Paket Data." },
    { "en": "Apa Itu Jitter Jaringan?", "id": "Variasi Waktu Kedatangan Paket Data." },
    { "en": "Apa Itu Packet Loss?", "id": "Data Yang Hilang Di Jaringan." },
    { "en": "Apa Fungsi VLAN (Virtual LAN)?", "id": "Pecah Jaringan Fisik Jadi Logis." },
    { "en": "Apa Itu Trunk Port Switch?", "id": "Port Pembawa Banyak VLAN." },
    { "en": "Apa Itu Access Port Switch?", "id": "Port Untuk Satu Perangkat Akhir." },
    { "en": "Apa Fungsi Spanning Tree Protocol (STP)?", "id": "Cegah Looping Pada Switch Jaringan." },
    { "en": "Apa Itu Broadcast Storm?", "id": "Banjir Data Lumpuhkan Jaringan." },
    { "en": "Apa Fungsi MikroTik RouterOS?", "id": "Sistem Operasi Router Populer." },
    { "en": "Apa Itu WinBox?", "id": "Aplikasi Konfigurasi Router MikroTik." },
    { "en": "Apa Itu Transistor Mosfet N-Channel?", "id": "Aktif Saat Gate Positif Source." },
    { "en": "Apa Itu Transistor Mosfet P-Channel?", "id": "Aktif Saat Gate Negatif Source." },
    { "en": "Apa Itu RDS(on) Mosfet?", "id": "Hambatan Drain-Source Saat On." },
    { "en": "Apa Efek RDS(on) Rendah?", "id": "Panas Sedikit Efisiensi Tinggi." },
    { "en": "Apa Itu Vgs(th) Mosfet?", "id": "Tegangan Gate Minimal Untuk On." },
    { "en": "Apa Itu Body Diode Mosfet?", "id": "Dioda Parasitik Internal Mosfet." },
    { "en": "Apa Fungsi Dioda Fast Recovery?", "id": "Penyearah Frekuensi Tinggi SMPS." },
    { "en": "Apa Kode Dioda Fast Recovery Umum?", "id": "FR107 Atau UF4007." },
    { "en": "Apa Beda 1N4007 Dan UF4007?", "id": "UF4007 Jauh Lebih Cepat Switching." },
    { "en": "Apa Fungsi Transistor IGBT?", "id": "Gabungan Input Mosfet Output BJT." },
    { "en": "Apa Itu Thermal Paste Abu-abu?", "id": "Pasta Perak Konduktivitas Panas Tinggi." },
    { "en": "Apa Itu Thermal Paste Putih?", "id": "Pasta Silikon Standar Elektronik." },
    { "en": "Apa Itu Thermal Pad Silikon?", "id": "Lembaran Karet Penghantar Panas." },
    { "en": "Apa Fungsi Mica Insulator Transistor?", "id": "Isolasi Listrik Antara Body Heatsink." },
    { "en": "Apa Fungsi Bushing Plastik Transistor?", "id": "Isolasi Baut Dari Body Transistor." },
    { "en": "Apa Itu Fan Grill (Jaring Kipas)?", "id": "Pelindung Jari Dari Baling-baling." },
    { "en": "Apa Itu Dust Filter Fan?", "id": "Saring Debu Masuk Panel." },
    { "en": "Apa Ukuran Fan Panel Standar?", "id": "12cm Atau 120mm." },
    { "en": "Apa Tegangan Fan Panel Umum?", "id": "220V AC Atau 24V DC." },
    { "en": "Apa Arah Angin Fan Exhaust?", "id": "Buang Udara Panas Keluar." },
    { "en": "Apa Fungsi Pin VIN Pada ESP8266?", "id": "Input Tegangan Eksternal 5V." },
    { "en": "Apa Fungsi Pin RST Pada ESP8266?", "id": "Reset Modul (Active Low)." },
    { "en": "Apa Tegangan Operasi Pin GPIO ESP8266?", "id": "Maksimal 3.3 Volt." },
    { "en": "Apa Itu NodeMCU Amica?", "id": "Versi NodeMCU Ukuran Standar." },
    { "en": "Apa Itu NodeMCU Lolin?", "id": "Versi NodeMCU Ukuran Besar." },
    { "en": "Apa Fungsi Pin D0 (GPIO16) NodeMCU?", "id": "Bangunkan Dari Deep Sleep." },
    { "en": "Apa Fungsi Pin ADC (A0) ESP8266?", "id": "Baca Tegangan Analog 0-1V." },
    { "en": "Apa Fungsi Library ESP8266WiFi.h?", "id": "Hubungkan ESP8266 Ke WiFi." },
    { "en": "Apa Fungsi Library BlynkSimpleEsp8266.h?", "id": "Hubungkan ESP8266 Ke Blynk." },
    { "en": "Apa Itu SSID Hidden?", "id": "Nama WiFi Tidak Terlihat." },
    { "en": "Apa Itu DHCP Client?", "id": "Minta IP Otomatis Ke Router." },
    { "en": "Apa Itu Static IP Address?", "id": "Alamat IP Diatur Manual." },
    { "en": "Apa Fungsi Command Ping?", "id": "Cek Koneksi Ke Server." },
    { "en": "Apa Fungsi Trimpot 10k?", "id": "Atur Kontras LCD 16x2." },
    { "en": "Apa Fungsi Backlight Jumper LCD?", "id": "Nyalakan Matikan Lampu Latar." },
    { "en": "Apa Itu I2C Backpack LCD?", "id": "Modul Konverter Paralel Ke I2C." },
    { "en": "Apa Chip Utama I2C Backpack?", "id": "PCF8574." },
    { "en": "Apa Fungsi Resistor Shunt?", "id": "Ukur Arus Lewat Tegangan." },
    { "en": "Apa Rumus Hukum Ohm?", "id": "V = I X R." },
    { "en": "Apa Rumus Daya DC?", "id": "P = V X I." },
    { "en": "Apa Itu Short Circuit (Korsleting)?", "id": "Hubungan Langsung Positif Negatif." },
    { "en": "Apa Akibat Short Circuit Baterai?", "id": "Panas Tinggi Dan Meledak." },
    { "en": "Apa Fungsi Sekering (Fuse)?", "id": "Putus Saat Arus Berlebih." },
    { "en": "Apa Cara Cek Fuse Putus?", "id": "Ukur Kontinuitas (Bunyi Beep)." },
    { "en": "Apa Fungsi Dioda Penyearah?", "id": "Ubah AC Menjadi DC." },
    { "en": "Apa Kode Dioda 1 Ampere Umum?", "id": "1N4007." },
    { "en": "Apa Kode Dioda 3 Ampere Umum?", "id": "1N5408." },
    { "en": "Apa Itu Dioda Bridge Sisir?", "id": "Empat Dioda Dalam Satu Paket." },
    { "en": "Apa Tanda Kaki Positif Elco?", "id": "Kaki Yang Lebih Panjang." },
    { "en": "Apa Tanda Kaki Negatif Elco?", "id": "Garis Putih Di Body." },
    { "en": "Apa Fungsi Elco Pada Power Supply?", "id": "Ratakan Gelombang DC (Filter)." },
    { "en": "Apa Itu Kapasitor Keramik 104?", "id": "100 Nano Farad (Filter Noise)." },
    { "en": "Apa Satuan Induktansi?", "id": "Henry (H)." },
    { "en": "Apa Fungsi Induktor Pada Buck Converter?", "id": "Simpan Energi Sementara." },
    { "en": "Apa Itu Transistor NPN?", "id": "Aktif Saat Basis Positif." },
    { "en": "Apa Itu Transistor PNP?", "id": "Aktif Saat Basis Negatif." },
    { "en": "Apa Kaki Transistor BJT?", "id": "Basis, Kolektor, Emitor." },
    { "en": "Apa Kaki Transistor MOSFET?", "id": "Gate, Drain, Source." },
    { "en": "Apa Fungsi Heatsink Aluminium?", "id": "Buang Panas Komponen." },
    { "en": "Apa Fungsi Kipas DC 12V?", "id": "Pendingin Aktif Sirkuit." },
    { "en": "Apa Itu Relay 5V?", "id": "Saklar Elektromagnetik Koil 5V." },
    { "en": "Apa Itu Relay 12V?", "id": "Saklar Elektromagnetik Koil 12V." },
    { "en": "Apa Bunyi Klik Pada Relay?", "id": "Suara Kontak Mekanis Berpindah." },
    { "en": "Apa Itu COM Pada Relay?", "id": "Common (Terminal Pusat)." },
    { "en": "Apa Itu NO Pada Relay?", "id": "Normally Open (Buka Awal)." },
    { "en": "Apa Itu NC Pada Relay?", "id": "Normally Closed (Tutup Awal)." },
    { "en": "Apa Fungsi Sensor Suhu Thermistor?", "id": "Resistansi Berubah Sesuai Suhu." },
    { "en": "Apa Itu Sensor NTC 10k?", "id": "Resistansi 10k Pada 25C." },
    { "en": "Apa Fungsi Sensor Gas MQ-135?", "id": "Deteksi Kualitas Udara (Amonia/CO2)." },
    { "en": "Apa Fungsi Sensor Gas MQ-7?", "id": "Deteksi Gas Karbon Monoksida." },
    { "en": "Apa Pemanasan Awal Sensor MQ?", "id": "Pre-heat 24 Jam Agar Akurat." },
    { "en": "Apa Fungsi Buzzer 5V?", "id": "Hasilkan Suara Alarm." },
    { "en": "Apa Beda Buzzer Aktif Dan Pasif?", "id": "Aktif Langsung Bunyi, Pasif Nada." },
    { "en": "Apa Itu Tactile Switch?", "id": "Tombol Tekan Kecil (Push Button)." },
    { "en": "Apa Kaki Tactile Switch Yang Terhubung?", "id": "Kaki Seberang Selalu Terhubung." },
    { "en": "Apa Fungsi Breadboard MB-102?", "id": "Papan Rangkaian Tanpa Solder Besar." },
    { "en": "Apa Jalur Merah Breadboard?", "id": "Jalur Positif (+)." },
    { "en": "Apa Jalur Biru Breadboard?", "id": "Jalur Negatif/Ground (-)." },
    { "en": "Apa Fungsi Kabel Jumper Rainbow?", "id": "Koneksi Sementara Rangkaian." },
    { "en": "Apa Itu Multimeter Digital?", "id": "Alat Ukur Listrik Layar Angka." },
    { "en": "Apa Fungsi Mode Buzzer Multimeter?", "id": "Cek Kabel Nyambung (Short)." },
    { "en": "Apa Bahaya Ukur Tegangan Mode Ohm?", "id": "Multimeter Bisa Rusak/Sekering Putus." },
    { "en": "Apa Itu Solder 40 Watt?", "id": "Pemanas Timah Daya Sedang." },
    { "en": "Apa Itu Solder 60 Watt?", "id": "Pemanas Timah Lebih Cepat." },
    { "en": "Apa Fungsi Dudukan Solder?", "id": "Tempat Meletakkan Solder Panas." },
    { "en": "Apa Fungsi Spons Solder?", "id": "Bersihkan Mata Solder Panas." },
    { "en": "Apa Cairan Untuk Spons Solder?", "id": "Air Biasa." },
    { "en": "Apa Fungsi Tang Potong?", "id": "Potong Kaki Komponen." },
    { "en": "Apa Fungsi Tang Lancip?", "id": "Tekuk Kaki Komponen." },
    { "en": "Apa Itu Obeng Plus (+)?", "id": "Untuk Baut Kepala Kembang." },
    { "en": "Apa Itu Obeng Minus (-)?", "id": "Untuk Baut Kepala Pipih." },
    { "en": "Apa Fungsi Box Hitam X2?", "id": "Wadah Rangkaian Elektronik." },
    { "en": "Apa Itu Baut M3?", "id": "Baut Diameter 3 Milimeter." },
    { "en": "Apa Itu Mur M3?", "id": "Pasangan Baut M3." },
    { "en": "Apa Fungsi Spacer Kuningan?", "id": "Penyangga PCB Agar Tidak Konslet." },
    { "en": "Apa Itu PCB Polos?", "id": "Papan Tembaga Belum Di-etching." },
    { "en": "Apa Itu Ferric Chloride?", "id": "Larutan Pelarut Tembaga PCB." },
    { "en": "Apa Itu Spidol Permanen PCB?", "id": "Gambar Jalur PCB Manual." },
    { "en": "Apa Itu Bor PCB Mini?", "id": "Lubangi PCB Untuk Kaki Komponen." },
    { "en": "Apa Ukuran Mata Bor Standar?", "id": "0.8mm Hingga 1.0mm." },
    { "en": "Apa Fungsi Amplas Halus?", "id": "Bersihkan PCB Sebelum Solder." },
    { "en": "Apa Itu Flux Solder?", "id": "Memudahkan Timah Menempel." },
    { "en": "Apa Itu Timah Gulung?", "id": "Kawat Solder (Biasanya 0.8mm)." },
    { "en": "Apa Kandungan Timah 60/40?", "id": "60% Timah 40% Timbal." },
    { "en": "Apa Itu Timah Lead Free?", "id": "Timah Tanpa Timbal (Ramah Lingkungan)." },
    { "en": "Apa Suhu Leleh Timah Lead Free?", "id": "Lebih Tinggi Dari Timah Biasa." },
    { "en": "Apa Itu Desoldering Pump (Atraktor)?", "id": "Sedot Timah Cair." },
    { "en": "Apa Fungsi Pinset Lurus?", "id": "Pegang Komponen Kecil." },
    { "en": "Apa Fungsi Pinset Bengkok?", "id": "Pegang Komponen Di Sudut." },
    { "en": "Apa Fungsi Kaca Pembesar?", "id": "Lihat Jalur/Solderan Kecil." },
    { "en": "Apa Itu Lampu Service?", "id": "Penerangan Meja Kerja." },
    { "en": "Apa Itu Power Supply Variabel?", "id": "Sumber Tegangan Bisa Diatur." },
    { "en": "Apa Fungsi Knop Volts?", "id": "Atur Tegangan Output." },
    { "en": "Apa Fungsi Knop Amps?", "id": "Batasi Arus Maksimal." },
    { "en": "Apa Itu Capit Buaya?", "id": "Jepit Kabel Ke Terminal." },
    { "en": "Apa Fungsi Kabel USB Printer?", "id": "Upload Program Arduino Uno." },
    { "en": "Apa Fungsi Kabel Micro USB?", "id": "Upload Program NodeMCU/ESP32." },
    { "en": "Apa Kepanjangan IC Kemasan DIP?", "id": "Dual Inline Package." },
    { "en": "Apa Kepanjangan IC Kemasan SMD?", "id": "Surface Mount Device." },
    { "en": "Apa Itu Kemasan SOIC?", "id": "Small Outline Integrated Circuit." },
    { "en": "Apa Itu Kemasan QFP?", "id": "Quad Flat Package." },
    { "en": "Apa Itu Kemasan BGA?", "id": "Ball Grid Array." },
    { "en": "Apa Kesulitan Utama Solder BGA?", "id": "Kaki Tersembunyi Di Bawah Chip." },
    { "en": "Apa Itu Reballing Chip BGA?", "id": "Pasang Ulang Bola Timah Chip." },
    { "en": "Apa Itu Silkscreen Pada PCB?", "id": "Tinta Teks Label Komponen." },
    { "en": "Apa Warna Umum Silkscreen PCB?", "id": "Putih." },
    { "en": "Apa Fungsi Soldermask PCB?", "id": "Lindungi Tembaga Dan Cegah Short." },
    { "en": "Apa Warna Umum Soldermask PCB?", "id": "Hijau." },
    { "en": "Apa Itu Pad Pada PCB?", "id": "Area Tembaga Untuk Solder Kaki." },
    { "en": "Apa Itu Trace Pada PCB?", "id": "Jalur Konduktor Penghubung Komponen." },
    { "en": "Apa Itu Via Through-hole?", "id": "Lubang Tembus Penghubung Layer." },
    { "en": "Apa Itu Blind Via?", "id": "Lubang Layer Luar Ke Dalam." },
    { "en": "Apa Itu Buried Via?", "id": "Lubang Antar Layer Dalam Saja." },
    { "en": "Apa Bahan Dasar PCB Fleksibel?", "id": "Plastik Polimida (Kapton)." },
    { "en": "Apa Tegangan Nominal Baterai NiCd?", "id": "1.2 Volt." },
    { "en": "Apa Masalah Utama Baterai NiCd?", "id": "Memory Effect (Efek Memori)." },
    { "en": "Apa Tegangan Nominal Baterai NiMH?", "id": "1.2 Volt." },
    { "en": "Apa Kelebihan NiMH Dibanding NiCd?", "id": "Kapasitas Lebih Besar Ramah Lingkungan." },
    { "en": "Apa Tegangan Baterai Li-Po Fully Charged?", "id": "4.2 Volt." },
    { "en": "Apa Tegangan Baterai LiFePO4 Fully Charged?", "id": "3.6 Volt." },
    { "en": "Apa Itu Self-discharge Baterai?", "id": "Daya Berkurang Saat Tidak Dipakai." },
    { "en": "Baterai Mana Yang Low Self-discharge?", "id": "Lithium-Ion Dan LSD NiMH." },
    { "en": "Apa Fungsi Konektor XLR 3-Pin?", "id": "Kabel Mikrofon Balanced Professional." },
    { "en": "Pin 1 Konektor XLR Adalah?", "id": "Ground (Shield)." },
    { "en": "Pin 2 Konektor XLR Adalah?", "id": "Positif (Hot)." },
    { "en": "Pin 3 Konektor XLR Adalah?", "id": "Negatif (Cold)." },
    { "en": "Apa Itu Konektor TRS 6.5mm?", "id": "Jack Stereo Besar (Akai Stereo)." },
    { "en": "Apa Itu Konektor TS 6.5mm?", "id": "Jack Mono Besar (Akai Mono)." },
    { "en": "Apa Fungsi Konektor Speakon?", "id": "Konektor Speaker Daya Tinggi Mengunci." },
    { "en": "Apa Itu Konektor RCA Kuning?", "id": "Video Komposit Analog." },
    { "en": "Apa Itu Konektor RCA Putih?", "id": "Audio Analog Kanal Kiri." },
    { "en": "Apa Itu Konektor RCA Merah?", "id": "Audio Analog Kanal Kanan." },
    { "en": "Apa Fungsi Kabel MIDI?", "id": "Komunikasi Data Alat Musik Digital." },
    { "en": "Apa Jumlah Pin Konektor MIDI Standar?", "id": "5 Pin DIN." },
    { "en": "Apa Itu Konektor VGA (D-Sub)?", "id": "Video Analog Komputer (Biru)." },
    { "en": "Apa Jumlah Pin Konektor VGA?", "id": "15 Pin (3 Baris)." },
    { "en": "Apa Itu Konektor DVI-D?", "id": "Video Digital Saja." },
    { "en": "Apa Itu Konektor DVI-I?", "id": "Video Digital Dan Analog." },
    { "en": "Apa Kelebihan HDMI Dibanding VGA?", "id": "Digital, Kualitas Tinggi, Ada Suara." },
    { "en": "Apa Versi HDMI Support 4K 60Hz?", "id": "HDMI 2.0." },
    { "en": "Apa Versi HDMI Support 8K?", "id": "HDMI 2.1." },
    { "en": "Apa Itu DisplayPort (DP)?", "id": "Konektor Video Digital Monitor PC." },
    { "en": "Apa Fungsi Kabel Optik Toslink?", "id": "Kirim Audio Digital Lewat Cahaya." },
    { "en": "Apa Impedansi Kabel Antena TV (Coax)?", "id": "75 Ohm." },
    { "en": "Apa Impedansi Kabel Transceiver (HT)?", "id": "50 Ohm." },
    { "en": "Apa Itu Kabel UTP Cat6?", "id": "Kabel LAN Gigabit Ada Separator." },
    { "en": "Apa Warna Pasangan Kabel UTP?", "id": "Oranye, Hijau, Biru, Cokelat." },
    { "en": "Apa Fungsi Kabel Cross-over?", "id": "Hubungkan PC Ke PC Langsung." },
    { "en": "Apa Fungsi Kabel Straight-through?", "id": "Hubungkan PC Ke Switch/Hub." },
    { "en": "Apa Itu Konektor RJ45?", "id": "Konektor Kabel Jaringan LAN." },
    { "en": "Apa Itu Konektor RJ11?", "id": "Konektor Kabel Telepon." },
    { "en": "Apa Fungsi Crimping Tool RJ45?", "id": "Jepit Konektor Ke Kabel UTP." },
    { "en": "Apa Fungsi LAN Tester?", "id": "Cek Konektivitas Urutan Kabel LAN." },
    { "en": "Apa Itu PoE Injector?", "id": "Alat Suntik Daya Ke Kabel LAN." },
    { "en": "Apa Itu PoE Splitter?", "id": "Pisahkan Daya Dan Data Di Ujung." },
    { "en": "Apa Tegangan PoE Standar IEEE 802.3af?", "id": "48 Volt DC." },
    { "en": "Apa Itu Multimeter True RMS?", "id": "Akurat Ukur Gelombang Non-sinus." },
    { "en": "Apa Itu Auto Ranging Multimeter?", "id": "Otomatis Pilih Rentang Ukur." },
    { "en": "Apa Fungsi Tombol Hold Multimeter?", "id": "Bekukan Angka Di Layar." },
    { "en": "Apa Fungsi Backlight Multimeter?", "id": "Lampu Latar Untuk Gelap." },
    { "en": "Apa Itu Continuity Test (Buzzer)?", "id": "Cek Kabel Nyambung Bunyi Beep." },
    { "en": "Apa Itu HFE Test Multimeter?", "id": "Ukur Penguatan Arus Transistor." },
    { "en": "Apa Fungsi Tang Kombinasi?", "id": "Jepit Dan Potong Kabel Umum." },
    { "en": "Apa Fungsi Tang Potong (Diagonal)?", "id": "Potong Kawat Atau Kaki Komponen." },
    { "en": "Apa Fungsi Tang Lancip (Long Nose)?", "id": "Jepit Di Ruang Sempit." },
    { "en": "Apa Fungsi Tang Kupas Kabel?", "id": "Kelupas Isolasi Tanpa Putus Kawat." },
    { "en": "Apa Fungsi Obeng Presisi?", "id": "Buka Baut Kecil HP/Laptop." },
    { "en": "Apa Itu Mata Obeng Torx (Bintang)?", "id": "Kepala Baut Segi Enam Bintang." },
    { "en": "Apa Itu Mata Obeng Pentalobe?", "id": "Baut Bintang 5 Sudut (iPhone)." },
    { "en": "Apa Fungsi Gelang Antistatis (ESD)?", "id": "Buang Listrik Statis Tubuh." },
    { "en": "Apa Itu Meja Kerja Antistatis?", "id": "Meja Lapisan Karet Konduktif Ground." },
    { "en": "Apa Fungsi Exhaust Fan Solder?", "id": "Sedot Asap Timah Beracun." },
    { "en": "Apa Itu Flux Pen?", "id": "Flux Cair Bentuk Spidol." },
    { "en": "Apa Itu Timah Solder 0.8mm?", "id": "Diameter Standar Elektronika Umum." },
    { "en": "Apa Itu Timah Solder 0.3mm?", "id": "Diameter Kecil Untuk SMD Halus." },
    { "en": "Apa Komposisi Timah 60/40?", "id": "60% Timah, 40% Timbal." },
    { "en": "Apa Titik Leleh Timah 60/40?", "id": "Sekitar 183-190 Derajat Celcius." },
    { "en": "Apa Itu Timah Lead-free (RoHS)?", "id": "Tanpa Timbal, Leleh Lebih Tinggi." },
    { "en": "Apa Fungsi Wick (Solder Braid)?", "id": "Pita Tembaga Penyerap Timah." },
    { "en": "Apa Fungsi Atraktor (Pompa Sedot)?", "id": "Sedot Timah Cair Manual." },
    { "en": "Apa Itu Solder Station?", "id": "Solder Dengan Pengatur Suhu Stabil." },
    { "en": "Apa Itu Hot Air Station?", "id": "Solder Uap Untuk Komponen SMD." },
    { "en": "Apa Fungsi Pinset ESD Safe?", "id": "Pinset Anti Statis Warna Hitam." },
    { "en": "Apa Fungsi Cairan IPA (Isopropyl Alcohol)?", "id": "Bersihkan PCB Dari Sisa Flux." },
    { "en": "Apa Itu Kaca Pembesar Lampu?", "id": "Lampu Meja Dengan Lensa Besar." },
    { "en": "Apa Fungsi Third Hand Tool?", "id": "Penjepit PCB Bantuan Menyolder." },
    { "en": "Apa Itu Power Supply Variabel?", "id": "Sumber Tegangan DC Bisa Diatur." },
    { "en": "Apa Fungsi Mode CC Power Supply?", "id": "Constant Current (Arus Tetap)." },
    { "en": "Apa Fungsi Mode CV Power Supply?", "id": "Constant Voltage (Tegangan Tetap)." },
    { "en": "Apa Itu Short Circuit Protection?", "id": "Proteksi Alat Saat Korsleting." },
    { "en": "Apa Fungsi Oscilloscope Probe 10x?", "id": "Redam Sinyal 10 Kali Lipat." },
    { "en": "Apa Itu Bandwidth Osiloskop 100MHz?", "id": "Bisa Ukur Sinyal Hingga 100MHz." },
    { "en": "Apa Itu Sampling Rate 1GSa/s?", "id": "Satu Miliar Sampel Per Detik." },
    { "en": "Apa Fungsi IC NE555 Mode Astabil?", "id": "Hasilkan Gelombang Kotak Terus Menerus." },
    { "en": "Apa Fungsi IC NE555 Mode Monostabil?", "id": "Hasilkan Satu Pulsa Saat Dipicu." },
    { "en": "Apa Fungsi IC NE555 Mode Bistabil?", "id": "Berfungsi Sebagai Flip-flop Sederhana." },
    { "en": "Apa Pin 1 Pada IC NE555?", "id": "Ground (GND)." },
    { "en": "Apa Pin 3 Pada IC NE555?", "id": "Output Sinyal." },
    { "en": "Apa Pin 8 Pada IC NE555?", "id": "VCC (Positif)." },
    { "en": "Apa Fungsi Op-Amp LM741?", "id": "Penguat Operasional Tunggal Serbaguna." },
    { "en": "Apa Fungsi Op-Amp LM358?", "id": "Dua Penguat Operasional Satu Paket." },
    { "en": "Apa Fungsi Op-Amp TL072?", "id": "Dua Penguat JFET Low Noise." },
    { "en": "Apa Itu Konfigurasi Inverting Op-Amp?", "id": "Sinyal Output Berlawanan Fasa Input." },
    { "en": "Apa Itu Konfigurasi Non-inverting Op-Amp?", "id": "Sinyal Output Se-fasa Dengan Input." },
    { "en": "Apa Fungsi Komparator Op-Amp?", "id": "Bandingkan Dua Tegangan Input." },
    { "en": "Apa Itu Schmitt Trigger Op-Amp?", "id": "Komparator Dengan Histeresis Anti Noise." },
    { "en": "Apa Fungsi IC 7805?", "id": "Regulator Tegangan Tetap 5 Volt." },
    { "en": "Apa Fungsi IC 7812?", "id": "Regulator Tegangan Tetap 12 Volt." },
    { "en": "Apa Fungsi IC 7905?", "id": "Regulator Tegangan Negatif 5 Volt." },
    { "en": "Apa Kaki Input IC 7805?", "id": "Kaki Paling Kiri (Pin 1)." },
    { "en": "Apa Kaki Ground IC 7805?", "id": "Kaki Tengah (Pin 2)." },
    { "en": "Apa Kaki Output IC 7805?", "id": "Kaki Paling Kanan (Pin 3)." },
    { "en": "Apa Fungsi Heatsink Pada IC Regulator?", "id": "Buang Panas Akibat Penurunan Tegangan." },
    { "en": "Apa Itu 7-Segment Common Anode?", "id": "Semua Kutub Positif Digabung." },
    { "en": "Apa Itu 7-Segment Common Cathode?", "id": "Semua Kutub Negatif Digabung." },
    { "en": "Apa Fungsi IC 7447?", "id": "Decoder BCD Ke 7-Segment Anode." },
    { "en": "Apa Fungsi IC 74HC595?", "id": "Shift Register Serial Ke Paralel." },
    { "en": "Apa Kegunaan Shift Register?", "id": "Hemat Pin Mikrokontroler Untuk Output." },
    { "en": "Apa Itu LED Dot Matrix 8x8?", "id": "Layar Matriks 64 Titik LED." },
    { "en": "Apa Fungsi IC MAX7219?", "id": "Driver LED Matrix Dan 7-Segment." },
    { "en": "Apa Protokol Komunikasi MAX7219?", "id": "SPI (Serial Peripheral Interface)." },
    { "en": "Apa Itu LCD 16x2?", "id": "Layar Karakter 16 Kolom 2 Baris." },
    { "en": "Apa Fungsi Pin V0 Pada LCD?", "id": "Pengatur Kontras Tampilan." },
    { "en": "Apa Fungsi Pin A Dan K LCD?", "id": "Power Untuk Lampu Latar (Backlight)." },
    { "en": "Apa Itu Layar OLED 0.96 Inci?", "id": "Layar Grafis Kecil Kontras Tinggi." },
    { "en": "Apa Chip Driver OLED Umum?", "id": "SSD1306." },
    { "en": "Apa Kelebihan OLED Dibanding LCD?", "id": "Tidak Butuh Backlight, Hitam Pekat." },
    { "en": "Apa Itu Layar TFT?", "id": "Thin Film Transistor (Layar Berwarna)." },
    { "en": "Apa Itu Touchscreen Resistif?", "id": "Deteksi Sentuhan Berdasarkan Tekanan." },
    { "en": "Apa Itu Touchscreen Kapasitif?", "id": "Deteksi Sentuhan Berdasarkan Muatan Jari." },
    { "en": "Apa Fungsi IC 4017?", "id": "Penghitung Dekade (Decade Counter)." },
    { "en": "Apa Aplikasi Umum IC 4017?", "id": "Lampu Berjalan (Running LED)." },
    { "en": "Apa Fungsi IC 4026?", "id": "Counter Dan Driver 7-Segment." },
    { "en": "Apa Fungsi Gerbang Logika NOT?", "id": "Membalikkan Logika Input (Inverter)." },
    { "en": "Apa Fungsi Gerbang Logika BUFFER?", "id": "Perkuat Sinyal Tanpa Ubah Logika." },
    { "en": "Apa Itu Pull-up Resistor?", "id": "Jaga Input Tetap High Saat Mengambang." },
    { "en": "Apa Itu Pull-down Resistor?", "id": "Jaga Input Tetap Low Saat Mengambang." },
    { "en": "Apa Itu Floating Pin?", "id": "Pin Input Tanpa Referensi Tegangan." },
    { "en": "Apa Efek Floating Pin?", "id": "Pembacaan Nilai Tidak Stabil (Acak)." },
    { "en": "Apa Itu Debounce Tombol?", "id": "Hilangkan Sinyal Getar Mekanis Tombol." },
    { "en": "Apa Cara Debounce Hardware?", "id": "Gunakan Kapasitor Paralel Dengan Tombol." },
    { "en": "Apa Cara Debounce Software?", "id": "Beri Jeda Waktu Pembacaan Kode." },
    { "en": "Apa Fungsi Dioda Zener 5V1?", "id": "Stabilkan Tegangan Di 5.1 Volt." },
    { "en": "Apa Simbol Dioda Zener?", "id": "Panah Dioda Dengan Garis Bengkok." },
    { "en": "Apa Fungsi Transistor NPN Sebagai Saklar?", "id": "Sambung Ke Ground Saat Basis High." },
    { "en": "Apa Fungsi Transistor PNP Sebagai Saklar?", "id": "Sambung Ke VCC Saat Basis Low." },
    { "en": "Apa Itu H-Bridge L298N?", "id": "Driver Motor DC Dua Kanal." },
    { "en": "Apa Fungsi Heatsink L298N?", "id": "Buang Panas Karena Efisiensi Rendah." },
    { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Modulasi Lebar Pulsa Digital." },
    { "en": "Apa Fungsi Duty Cycle PWM?", "id": "Tentukan Rata-rata Tegangan Output." },
    { "en": "Apa Itu Analog Write Arduino?", "id": "Perintah Keluarkan Sinyal PWM." },
    { "en": "Apa Itu Analog Read Arduino?", "id": "Baca Tegangan Pin Analog (ADC)." },
    { "en": "Berapa Resolusi ADC Arduino Uno?", "id": "10 Bit (Nilai 0-1023)." },
    { "en": "Berapa Resolusi ADC ESP32?", "id": "12 Bit (Nilai 0-4095)." },
    { "en": "Apa Itu I2C Bus?", "id": "Komunikasi Serial Dua Kabel (SDA, SCL)." },
    { "en": "Apa Itu SPI Bus?", "id": "Komunikasi Serial Empat Kabel Cepat." },
    { "en": "Apa Itu UART Serial?", "id": "Komunikasi Asinkron TX Dan RX." },
    { "en": "Apa Fungsi Pin TX?", "id": "Transmit (Kirim Data)." },
    { "en": "Apa Fungsi Pin RX?", "id": "Receive (Terima Data)." },
    { "en": "Bagaimana Menghubungkan TX Dan RX?", "id": "TX Ketemu RX, RX Ketemu TX." },
    { "en": "Apa Itu Baud Rate?", "id": "Kecepatan Pengiriman Data Per Detik." },
    { "en": "Apa Itu Parity Bit?", "id": "Bit Cek Kesalahan Sederhana." },
    { "en": "Apa Itu Start Bit?", "id": "Tanda Awal Pengiriman Data Serial." },
    { "en": "Apa Itu Stop Bit?", "id": "Tanda Akhir Pengiriman Data Serial." },
    { "en": "Apa Fungsi Optocoupler PC817?", "id": "Isolasi Sinyal Antara Dua Rangkaian." },
    { "en": "Apa Tegangan Isolasi PC817?", "id": "Hingga 5000 Volt RMS." },
    { "en": "Apa Itu Zero Crossing Detector?", "id": "Deteksi Saat Gelombang AC Nol." },
    { "en": "Apa Fungsi Dimmer Lampu AC?", "id": "Atur Kecerahan Lampu Pijar AC." },
    { "en": "Apa Komponen Utama Dimmer AC?", "id": "Triac Dan Diac." },
    { "en": "Apa Itu Snubber Circuit?", "id": "Pelindung Komponen Dari Lonjakan Tegangan." },
    { "en": "Apa Isi Rangkaian Snubber RC?", "id": "Resistor Dan Kapasitor Seri." },
    { "en": "Apa Fungsi Varistor (MOV)?", "id": "Lindungi Dari Lonjakan Tegangan Tinggi." },
    { "en": "Apa Itu NTC Thermistor?", "id": "Hambatan Turun Saat Suhu Naik." },
    { "en": "Apa Itu PTC Thermistor?", "id": "Hambatan Naik Saat Suhu Naik." },
    { "en": "Apa Fungsi NTC Pada Power Supply?", "id": "Cegah Arus Lonjakan Saat Start." },
    { "en": "Apa Fungsi PTC Pada Motor?", "id": "Proteksi Overheat (Fuse Reset Otomatis)." },
    { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Hambatan Berubah Sesuai Cahaya." },
    { "en": "Apa Sifat LDR Saat Gelap?", "id": "Resistansi Sangat Tinggi (Mega Ohm)." },
    { "en": "Apa Sifat LDR Saat Terang?", "id": "Resistansi Sangat Rendah (Ratusan Ohm)." },
    { "en": "Apa Fungsi Photodiode?", "id": "Ubah Cahaya Menjadi Arus Listrik." },
    { "en": "Apa Beda LDR Dan Photodiode?", "id": "Photodiode Lebih Cepat Responnya." },
    { "en": "Apa Itu IR Receiver (TL1838)?", "id": "Penerima Sinyal Remote Inframerah." },
    { "en": "Apa Frekuensi Carrier Remote TV?", "id": "Umumnya 38 Kilo Hertz." },
    { "en": "Apa Itu Laser Diode 5V?", "id": "Modul Laser Merah Penunjuk." },
    { "en": "Apa Bahaya Laser Diode?", "id": "Dapat Merusak Retina Mata." },
    { "en": "Apa Fungsi Hall Effect Sensor?", "id": "Deteksi Medan Magnet." },
    { "en": "Apa Tipe Sensor Hall Linear?", "id": "Output Tegangan Sesuai Kuat Magnet." },
    { "en": "Apa Tipe Sensor Hall Switch?", "id": "Output Digital (On/Off) Ada Magnet." },
    { "en": "Apa Output Gerbang AND Input 1 Dan 1?", "id": "Logika 1 (High)." },
    { "en": "Apa Output Gerbang AND Input 1 Dan 0?", "id": "Logika 0 (Low)." },
    { "en": "Apa Output Gerbang OR Input 0 Dan 0?", "id": "Logika 0 (Low)." },
    { "en": "Apa Output Gerbang OR Input 1 Dan 0?", "id": "Logika 1 (High)." },
    { "en": "Apa Output Gerbang XOR Input 1 Dan 1?", "id": "Logika 0 (Low)." },
    { "en": "Apa Output Gerbang XOR Input 0 Dan 1?", "id": "Logika 1 (High)." },
    { "en": "Apa Output Gerbang NAND Input 1 Dan 1?", "id": "Logika 0 (Low)." },
    { "en": "Apa Output Gerbang NOR Input 0 Dan 0?", "id": "Logika 1 (High)." },
    { "en": "Apa Output Gerbang NOT Input 1?", "id": "Logika 0 (Low)." },
    { "en": "Apa Output Gerbang NOT Input 0?", "id": "Logika 1 (High)." },
    { "en": "Apa Tegangan Forward LED Merah Standar?", "id": "1.8 Hingga 2.0 Volt." },
    { "en": "Apa Tegangan Forward LED Biru Standar?", "id": "3.0 Hingga 3.3 Volt." },
    { "en": "Apa Tegangan Forward LED Putih Standar?", "id": "3.0 Hingga 3.3 Volt." },
    { "en": "Apa Tegangan Forward LED Kuning Standar?", "id": "2.0 Hingga 2.2 Volt." },
    { "en": "Apa Fungsi Kaki Panjang Pada LED?", "id": "Anoda (Positif)." },
    { "en": "Apa Fungsi Kaki Pendek Pada LED?", "id": "Katoda (Negatif)." },
    { "en": "Apa Fungsi Sisi Datar Body LED?", "id": "Penanda Kaki Katoda (Negatif)." },
    { "en": "Apa Arus Maksimal LED 5mm Standar?", "id": "20 Miliampere." },
    { "en": "Apa Rumus Resistor Pembatas Arus LED?", "id": "(Vcc - Vled) / I." },
    { "en": "Apa Fungsi Pin 5V Pada Arduino?", "id": "Output Tegangan 5 Volt Tergulasi." },
    { "en": "Apa Fungsi Pin 3.3V Pada Arduino?", "id": "Output Tegangan 3.3 Volt Tergulasi." },
    { "en": "Apa Fungsi Pin GND Pada Arduino?", "id": "Ground (Negatif) Sistem." },
    { "en": "Apa Fungsi Pin Vin Arduino?", "id": "Input Tegangan Eksternal (7-12V)." },
    { "en": "Apa Fungsi Pin A0 Sampai A5?", "id": "Input Sinyal Analog (ADC)." },
    { "en": "Apa Fungsi Pin D0 (RX) Arduino?", "id": "Penerima Data Serial." },
    { "en": "Apa Fungsi Pin D1 (TX) Arduino?", "id": "Pengirim Data Serial." },
    { "en": "Apa Tanda Pin PWM Pada Arduino?", "id": "Simbol Tilde (~)." },
    { "en": "Apa Fungsi Pin 13 Pada Arduino?", "id": "Terhubung Ke LED Built-in." },
    { "en": "Apa Fungsi Tombol Reset Di Papan?", "id": "Restart Program Mikrokontroler." },
    { "en": "Apa Chip USB To Serial Arduino Uno?", "id": "ATmega16U2." },
    { "en": "Apa Chip USB To Serial Arduino Clone?", "id": "CH340G." },
    { "en": "Apa Fungsi Regulator AMS1117-5.0?", "id": "Turunkan Tegangan Ke 5V Stabil." },
    { "en": "Apa Fungsi Regulator AMS1117-3.3?", "id": "Turunkan Tegangan Ke 3.3V Stabil." },
    { "en": "Apa Fungsi Sekering Polyfuse Arduino?", "id": "Lindungi USB Dari Arus Lebih." },
    { "en": "Apa Batas Arus Polyfuse Arduino Uno?", "id": "500 Miliampere." },
    { "en": "Apa Fungsi Pin SDA Pada I2C?", "id": "Serial Data Line." },
    { "en": "Apa Fungsi Pin SCL Pada I2C?", "id": "Serial Clock Line." },
    { "en": "Apa Pin I2C Pada Arduino Uno?", "id": "A4 (SDA) Dan A5 (SCL)." },
    { "en": "Apa Pin I2C Pada Arduino Mega?", "id": "Pin 20 (SDA) Dan 21 (SCL)." },
    { "en": "Apa Pin SPI MOSI Arduino Uno?", "id": "Pin 11." },
    { "en": "Apa Pin SPI MISO Arduino Uno?", "id": "Pin 12." },
    { "en": "Apa Pin SPI SCK Arduino Uno?", "id": "Pin 13." },
    { "en": "Apa Pin SPI SS Arduino Uno?", "id": "Pin 10." },
    { "en": "Apa Fungsi Pin Trigger Sensor Ultrasonik?", "id": "Kirim Pulsa Suara." },
    { "en": "Apa Fungsi Pin Echo Sensor Ultrasonik?", "id": "Terima Pantulan Suara." },
    { "en": "Apa Durasi Pulsa Trigger HC-SR04?", "id": "Minimal 10 Mikrotik." },
    { "en": "Apa Rumus Jarak Sensor Ultrasonik?", "id": "Durasi X 0.034 / 2." },
    { "en": "Apa Fungsi VCC Pada Modul Sensor?", "id": "Suplai Daya Positif." },
    { "en": "Apa Fungsi DO Pada Sensor Garis?", "id": "Digital Output (0 Atau 1)." },
    { "en": "Apa Fungsi AO Pada Sensor Garis?", "id": "Analog Output (Nilai Variabel)." },
    { "en": "Apa Fungsi Trimpot Pada Modul Sensor?", "id": "Atur Sensitivitas Deteksi." },
    { "en": "Apa Fungsi Relay NC (Normally Closed)?", "id": "Tersambung Saat Relay Mati." },
    { "en": "Apa Fungsi Relay NO (Normally Open)?", "id": "Terputus Saat Relay Mati." },
    { "en": "Apa Fungsi Pin IN Pada Relay Module?", "id": "Sinyal Pemicu (Trigger) Relay." },
    { "en": "Apa Itu Active Low Trigger Relay?", "id": "Nyala Saat Diberi Ground (0V)." },
    { "en": "Apa Itu Active High Trigger Relay?", "id": "Nyala Saat Diberi 5V." },
    { "en": "Apa Fungsi Transistor Pada Modul Relay?", "id": "Driver Penguat Arus Koil." },
    { "en": "Apa Fungsi Dioda Flyback Modul Relay?", "id": "Cegah Arus Balik Koil." },
    { "en": "Apa Fungsi Optocoupler Modul Relay?", "id": "Isolasi Mikrokontroler Dari Relay." },
    { "en": "Apa Tipe Baterai Remote TV Umum?", "id": "AAA (A3) 1.5 Volt." },
    { "en": "Apa Tipe Baterai Jam Dinding Umum?", "id": "AA (A2) 1.5 Volt." },
    { "en": "Apa Tipe Baterai Kotak Multimeter?", "id": "9 Volt (6F22)." },
    { "en": "Apa Tipe Baterai BIOS Komputer?", "id": "CR2032 (3 Volt)." },
    { "en": "Apa Kapasitas Baterai 18650 Standar?", "id": "2000 Hingga 3400 mAh." },
    { "en": "Apa Bahaya Baterai Li-Ion Bocor?", "id": "Cairan Elektrolit Mudah Terbakar." },
    { "en": "Apa Fungsi Konektor JST-XH 3 Pin?", "id": "Balance Charger Baterai 2S." },
    { "en": "Apa Fungsi Konektor JST-XH 4 Pin?", "id": "Balance Charger Baterai 3S." },
    { "en": "Apa Fungsi Kabel Merah Kabel USB?", "id": "VBUS (+5 Volt)." },
    { "en": "Apa Fungsi Kabel Hitam Kabel USB?", "id": "Ground (GND)." },
    { "en": "Apa Fungsi Kabel Putih Kabel USB?", "id": "Data Minus (D-)." },
    { "en": "Apa Fungsi Kabel Hijau Kabel USB?", "id": "Data Plus (D+)." },
    { "en": "Apa Fungsi Shielding Kabel USB?", "id": "Lindungi Dari Noise Luar." },
    { "en": "Apa Beda Kabel Charging Dan Data?", "id": "Kabel Charging Tidak Ada Jalur Data." },
    { "en": "Apa Itu Breadboard 400 Point?", "id": "Papan Prototipe Ukuran Setengah." },
    { "en": "Apa Itu Breadboard 830 Point?", "id": "Papan Prototipe Ukuran Penuh." },
    { "en": "Apa Jarak Antar Lubang Breadboard?", "id": "2.54 mm (0.1 Inci)." },
    { "en": "Apa Fungsi Header Pin Male?", "id": "Kaki Tusuk Ke Breadboard." },
    { "en": "Apa Fungsi Header Pin Female?", "id": "Lubang Terima Kabel Jumper." },
    { "en": "Apa Itu Terminal Block Screw?", "id": "Konektor Kabel Jepit Baut." },
    { "en": "Apa Itu DC Jack Female PCB?", "id": "Soket Adaptor Di Papan." },
    { "en": "Apa Itu Push Button 2 Pin?", "id": "Tombol Saklar Sederhana." },
    { "en": "Apa Itu Push Button 4 Pin?", "id": "Dua Pasang Kaki Terhubung." },
    { "en": "Apa Fungsi Kapasitor 100nF Di Tombol?", "id": "Debouncing (Hilangkan Noise)." },
    { "en": "Apa Fungsi Buzzer Active Low?", "id": "Bunyi Saat Diberi 0 Volt." },
    { "en": "Apa Frekuensi Nada A4 Standar?", "id": "440 Hertz." },
    { "en": "Apa Itu LDR 5mm?", "id": "Sensor Cahaya Diameter 5mm." },
    { "en": "Apa Resistansi LDR Saat Gelap Total?", "id": "Sangat Tinggi (Mega Ohm)." },
    { "en": "Apa Resistansi LDR Saat Terang?", "id": "Sangat Rendah (Ratusan Ohm)." },
    { "en": "Apa Fungsi Pembagi Tegangan LDR?", "id": "Ubah Resistansi Jadi Tegangan." },
    { "en": "Apa Fungsi LED RGB Common Anode?", "id": "Kaki Panjang Ke Positif." },
    { "en": "Apa Fungsi LED RGB Common Cathode?", "id": "Kaki Panjang Ke Ground." },
    { "en": "Apa Warna Dasar LED RGB?", "id": "Red, Green, Blue." },
    { "en": "Apa Campuran Warna Merah Dan Hijau?", "id": "Kuning." },
    { "en": "Apa Campuran Warna Merah Dan Biru?", "id": "Magenta (Ungu)." },
    { "en": "Apa Campuran Warna Hijau Dan Biru?", "id": "Cyan (Muda)." },
    { "en": "Apa Campuran Ketiga Warna RGB?", "id": "Putih." },
    { "en": "Apa Teorema Thevenin?", "id": "Sumber Tegangan Dan Resistor Seri." },
    { "en": "Apa Teorema Norton?", "id": "Sumber Arus Dan Resistor Paralel." },
    { "en": "Apa Teorema Millman?", "id": "Gabungan Sumber Paralel Jadi Satu." },
    { "en": "Apa Teorema Transfer Daya Maksimum?", "id": "Impedansi Beban Sama Dengan Sumber." },
    { "en": "Apa Itu Rangkaian Clipper?", "id": "Pemotong Puncak Sinyal Gelombang." },
    { "en": "Apa Itu Rangkaian Clamper?", "id": "Geser Level Tegangan DC Sinyal." },
    { "en": "Apa Itu Rangkaian Multiplier Tegangan?", "id": "Naikkan Tegangan AC Jadi DC Tinggi." },
    { "en": "Apa Komponen Utama Voltage Doubler?", "id": "Dioda Dan Kapasitor." },
    { "en": "Apa Itu Efek Hall?", "id": "Medan Magnet Hasilkan Beda Potensial." },
    { "en": "Apa Itu Efek Seebeck?", "id": "Panas Hasilkan Listrik." },
    { "en": "Apa Itu Efek Peltier?", "id": "Listrik Hasilkan Panas Dingin." },
    { "en": "Apa Itu Efek Piezoelektrik?", "id": "Tekanan Mekanis Hasilkan Listrik." },
    { "en": "Apa Itu Efek Photovoltaic?", "id": "Cahaya Hasilkan Energi Listrik." },
    { "en": "Apa Itu Efek Photoconductive?", "id": "Cahaya Ubah Nilai Resistansi." },
    { "en": "Apa Itu Motor Universal?", "id": "Bisa Pakai AC Atau DC." },
    { "en": "Apa Ciri Fisik Motor Universal?", "id": "Punya Sikat Karbon Dan Komutator." },
    { "en": "Apa Itu Motor Shaded Pole?", "id": "Motor Induksi Satu Fasa Kecil." },
    { "en": "Apa Ciri Motor Shaded Pole?", "id": "Ada Cincin Tembaga Di Kutub." },
    { "en": "Apa Itu Motor Histeresis?", "id": "Motor Sinkron Rotor Tanpa Gigi." },
    { "en": "Apa Keunggulan Motor Histeresis?", "id": "Sangat Hening Dan Halus." },
    { "en": "Apa Itu Motor Reluktansi?", "id": "Rotor Besi Tanpa Magnet/Lilitan." },
    { "en": "Apa Itu Motor Repulsi?", "id": "Start Dengan Tolakan Kutub Magnet." },
    { "en": "Apa Itu Motor Linear?", "id": "Gerak Lurus Bukan Berputar." },
    { "en": "Apa Aplikasi Motor Linear?", "id": "Kereta Maglev Dan Pintu Otomatis." },
    { "en": "Apa Itu PAM (Pulse Amplitude Modulation)?", "id": "Amplitudo Pulsa Ikuti Sinyal." },
    { "en": "Apa Itu PPM (Pulse Position Modulation)?", "id": "Posisi Pulsa Ikuti Sinyal." },
    { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Lebar Pulsa Ikuti Sinyal." },
    { "en": "Apa Itu PCM (Pulse Code Modulation)?", "id": "Sinyal Analog Diubah Ke Digital." },
    { "en": "Apa Itu ASK (Amplitude Shift Keying)?", "id": "Modulasi Digital Ubah Amplitudo." },
    { "en": "Apa Itu FSK (Frequency Shift Keying)?", "id": "Modulasi Digital Ubah Frekuensi." },
    { "en": "Apa Itu PSK (Phase Shift Keying)?", "id": "Modulasi Digital Ubah Fasa." },
    { "en": "Apa Itu QPSK?", "id": "Quadrature Phase Shift Keying." },
    { "en": "Apa Itu Baud Rate?", "id": "Jumlah Simbol Per Detik." },
    { "en": "Apa Itu Bit Rate?", "id": "Jumlah Bit Per Detik." },
    { "en": "Apa Hubungan Baud Rate Dan Bit Rate?", "id": "Bisa Sama Atau Berbeda." },
    { "en": "Apa Itu Half Duplex?", "id": "Komunikasi Dua Arah Bergantian." },
    { "en": "Apa Itu Full Duplex?", "id": "Komunikasi Dua Arah Bersamaan." },
    { "en": "Apa Itu Simplex?", "id": "Komunikasi Satu Arah Saja." },
    { "en": "Apa Contoh Komunikasi Simplex?", "id": "Siaran Radio Atau TV." },
    { "en": "Apa Contoh Komunikasi Half Duplex?", "id": "Walkie Talkie." },
    { "en": "Apa Contoh Komunikasi Full Duplex?", "id": "Telepon Seluler." },
    { "en": "Apa Fungsi MOV (Varistor)?", "id": "Lindungi Dari Lonjakan Tegangan." },
    { "en": "Apa Sifat MOV Saat Normal?", "id": "Resistansi Sangat Tinggi." },
    { "en": "Apa Sifat MOV Saat Overvoltage?", "id": "Resistansi Sangat Rendah (Short)." },
    { "en": "Apa Fungsi NTC Inrush Limiter?", "id": "Batasi Arus Kejut Awal." },
    { "en": "Apa Sifat NTC Saat Panas?", "id": "Resistansi Menjadi Rendah." },
    { "en": "Apa Fungsi PTC Fuse?", "id": "Sekering Otomatis Reset Sendiri." },
    { "en": "Apa Sifat PTC Saat Panas?", "id": "Resistansi Menjadi Tinggi." },
    { "en": "Apa Itu GDT (Gas Discharge Tube)?", "id": "Pelindung Petir Berbasis Gas." },
    { "en": "Apa Itu TVS Diode?", "id": "Dioda Pelindung Tegangan Transien." },
    { "en": "Apa Beda TVS Dan Zener?", "id": "TVS Lebih Cepat Responnya." },
    { "en": "Apa Itu Common Mode Noise?", "id": "Gangguan Pada Kedua Jalur Kabel." },
    { "en": "Apa Itu Differential Mode Noise?", "id": "Gangguan Antar Dua Kabel." },
    { "en": "Apa Fungsi Common Mode Choke?", "id": "Filter Noise Common Mode." },
    { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Pasif Redam Frekuensi Tinggi." },
    { "en": "Apa Itu Phototransistor?", "id": "Transistor Peka Cahaya." },
    { "en": "Apa Itu Darlington Pair?", "id": "Dua Transistor Gain Ganda." },
    { "en": "Apa Itu Sziklai Pair?", "id": "Pasangan Komplementer NPN PNP." },
    { "en": "Apa Itu IGBT?", "id": "Gabungan MOSFET Dan BJT." },
    { "en": "Apa Kelebihan IGBT?", "id": "Switching Cepat Tegangan Tinggi." },
    { "en": "Apa Itu TRIAC?", "id": "Saklar AC Dua Arah." },
    { "en": "Apa Itu SCR?", "id": "Saklar DC Satu Arah." },
    { "en": "Apa Itu DIAC?", "id": "Pemicu Gate TRIAC." },
    { "en": "Apa Itu PUT (Programmable Unijunction Transistor)?", "id": "Thyristor Pemicu Osilator." },
    { "en": "Apa Itu UJT (Unijunction Transistor)?", "id": "Transistor Satu Sambungan PN." },
    { "en": "Apa Aplikasi Utama UJT?", "id": "Osilator Relaksasi." },
    { "en": "Apa Itu VCO (Voltage Controlled Oscillator)?", "id": "Frekuensi Output Diatur Tegangan." },
    { "en": "Apa Itu PLL (Phase Locked Loop)?", "id": "Sistem Kendali Frekuensi Dan Fasa." },
    { "en": "Apa Komponen Utama PLL?", "id": "Detektor Fasa, Filter, VCO." },
    { "en": "Apa Itu Mixer RF?", "id": "Pencampur Dua Frekuensi Sinyal." },
    { "en": "Apa Itu IF (Intermediate Frequency)?", "id": "Frekuensi Menengah Radio." },
    { "en": "Apa Nilai IF Radio AM Standar?", "id": "455 Kilo Hertz." },
    { "en": "Apa Nilai IF Radio FM Standar?", "id": "10.7 Mega Hertz." },
    { "en": "Apa Itu AGC (Automatic Gain Control)?", "id": "Stabilkan Volume Sinyal Radio." },
    { "en": "Apa Itu AFC (Automatic Frequency Control)?", "id": "Kunci Frekuensi Agar Tidak Geser." },
    { "en": "Apa Itu Squelch Pada Radio?", "id": "Bisukan Suara Desis Kosong." },
    { "en": "Apa Itu CMRR Op-Amp?", "id": "Rasio Penolakan Sinyal Bersama." },
    { "en": "Apa Itu Slew Rate Op-Amp?", "id": "Kecepatan Perubahan Tegangan Output." },
    { "en": "Apa Itu Gain Bandwidth Product?", "id": "Perkalian Gain Dan Bandwidth." },
    { "en": "Apa Itu Virtual Ground Op-Amp?", "id": "Titik Nol Volt Semu." },
    { "en": "Apa Itu Instrumentation Amplifier?", "id": "Penguat Presisi Impedansi Tinggi." },
    { "en": "Apa Komponen Instrumentation Amplifier?", "id": "Tiga Buah Op-Amp." },
    { "en": "Apa Itu Isolation Amplifier?", "id": "Penguat Terpisah Secara Galvanis." },
    { "en": "Apa Itu Charge Amplifier?", "id": "Penguat Sinyal Sensor Piezo." },
    { "en": "Apa Itu Logarithmic Amplifier?", "id": "Output Logaritma Dari Input." },
    { "en": "Apa Itu Antilog Amplifier?", "id": "Kebalikan Dari Log Amplifier." },
    { "en": "Apa Itu Precision Rectifier?", "id": "Penyearah Dioda Dan Op-Amp." },
    { "en": "Apa Kelebihan Precision Rectifier?", "id": "Tidak Ada Drop Tegangan Dioda." },
    { "en": "Apa Itu Active Filter?", "id": "Filter Menggunakan Op-Amp." },
    { "en": "Apa Itu Passive Filter?", "id": "Filter Hanya R, L, C." },
    { "en": "Apa Itu Butterworth Filter?", "id": "Respon Frekuensi Datar (Flat)." },
    { "en": "Apa Itu Chebyshev Filter?", "id": "Respon Curam Ada Ripple." },
    { "en": "Apa Itu Bessel Filter?", "id": "Respon Fasa Linear." },
    { "en": "Apa Itu Notch Filter?", "id": "Buang Satu Frekuensi Sempit." },
    { "en": "Apa Aplikasi Notch Filter?", "id": "Hilangkan Dengung 50 Hertz." },
    { "en": "Apa Itu All Pass Filter?", "id": "Ubah Fasa Tanpa Ubah Amplitudo." },
    { "en": "Apa Itu Sample And Hold Circuit?", "id": "Ambil Dan Tahan Nilai Tegangan." },
    { "en": "Apa Fungsi Slip Ring Generator AC?", "id": "Transfer Arus Ke Rotor Berputar." },
    { "en": "Apa Beda Slip Ring Dan Split Ring?", "id": "Slip Ring AC, Split Ring DC." },
    { "en": "Apa Itu Komutasi Pada Motor DC?", "id": "Pembalik Arah Arus Pada Jangkar." },
    { "en": "Apa Efek Reaksi Jangkar (Armature Reaction)?", "id": "Distorsi Fluks Magnet Utama." },
    { "en": "Cara Mengurangi Efek Reaksi Jangkar?", "id": "Gunakan Belitan Kompensasi Atau Interpole." },
    { "en": "Apa Itu Hunting Pada Motor Sinkron?", "id": "Osilasi Rotor Sekitar Kecepatan Sinkron." },
    { "en": "Apa Penyebab Hunting Motor Sinkron?", "id": "Perubahan Beban Tiba-tiba." },
    { "en": "Apa Fungsi Damper Winding?", "id": "Cegah Hunting Dan Start Motor." },
    { "en": "Apa Itu Cogging Pada Motor Induksi?", "id": "Rotor Terkunci Magnetik Dengan Stator." },
    { "en": "Apa Penyebab Cogging Motor Induksi?", "id": "Jumlah Slot Stator Rotor Sama." },
    { "en": "Apa Itu Crawling Pada Motor Induksi?", "id": "Putar Lambat Seper-tujuh Kecepatan." },
    { "en": "Apa Penyebab Crawling Motor Induksi?", "id": "Harmonisa Ganjil Ke-7 Pada Fluks." },
    { "en": "Apa Karakteristik Motor DC Seri?", "id": "Torsi Awal Tinggi Kecepatan Jatuh." },
    { "en": "Mengapa Motor DC Seri Jangan Tanpa Beban?", "id": "Kecepatan Naik Berbahaya (Runaway)." },
    { "en": "Apa Aplikasi Motor DC Seri?", "id": "Traksi, Derek, Dan Crane." },
    { "en": "Apa Aplikasi Motor DC Shunt?", "id": "Mesin Bubut, Kipas, Pompa." },
    { "en": "Apa Aplikasi Motor DC Compound?", "id": "Mesin Pres, Rolling Mill." },
    { "en": "Apa Fungsi Minyak Trafo?", "id": "Isolasi Listrik Dan Pendingin." },
    { "en": "Apa Fungsi Breather Pada Trafo?", "id": "Saring Udara Masuk Konservator." },
    { "en": "Apa Fungsi Tangki Konservator Trafo?", "id": "Ruang Pemuaian Minyak Trafo." },
    { "en": "Apa Warna Silica Gel Kering?", "id": "Biru Atau Oranye." },
    { "en": "Apa Warna Silica Gel Basah?", "id": "Merah Muda Atau Putih." },
    { "en": "Apa Itu Buchholz Relay?", "id": "Relay Proteksi Deteksi Gas." },
    { "en": "Dimana Letak Buchholz Relay?", "id": "Antara Tangki Utama Dan Konservator." },
    { "en": "Apa Bahan Inti Besi Trafo?", "id": "Baja Silikon (CRGO)." },
    { "en": "Mengapa Inti Trafo Dilaminasi?", "id": "Kurangi Rugi Arus Eddy." },
    { "en": "Tes Open Circuit Trafo Menentukan Apa?", "id": "Rugi Inti (Rugi Besi)." },
    { "en": "Tes Short Circuit Trafo Menentukan Apa?", "id": "Rugi Tembaga (Ohmic Loss)." },
    { "en": "Kapan Efisiensi Trafo Maksimum?", "id": "Rugi Tembaga Sama Dengan Inti." },
    { "en": "Apa Itu All Day Efficiency?", "id": "Efisiensi Energi Selama 24 Jam." },
    { "en": "Apa Keuntungan Autotrafo?", "id": "Hemat Tembaga Ukuran Kecil." },
    { "en": "Apa Kerugian Autotrafo?", "id": "Tidak Ada Isolasi Galvanis." },
    { "en": "Berapa Rating Sekunder CT Standar?", "id": "1 Ampere Atau 5 Ampere." },
    { "en": "Berapa Rating Sekunder PT Standar?", "id": "100 Volt Atau 110 Volt." },
    { "en": "Mengapa Sekunder CT Tidak Boleh Terbuka?", "id": "Tegangan Induksi Sangat Tinggi." },
    { "en": "Apa Bahan Kabel Transmisi ACSR?", "id": "Aluminium Penguat Baja." },
    { "en": "Apa Fungsi Baja Pada ACSR?", "id": "Kuatkan Tarikan Mekanis." },
    { "en": "Mengapa Pakai Bundle Conductor?", "id": "Kurangi Corona Dan Induktansi." },
    { "en": "Apa Itu Skin Effect?", "id": "Arus Mengalir Di Kulit Konduktor." },
    { "en": "Apa Itu Proximity Effect?", "id": "Distribusi Arus Dipengaruhi Konduktor Lain." },
    { "en": "Apa Itu Ferranti Effect?", "id": "Tegangan Terima Lebih Besar Kirim." },
    { "en": "Kapan Terjadi Ferranti Effect?", "id": "Saluran Panjang Beban Ringan." },
    { "en": "Apa Itu Surge Impedance Loading?", "id": "Daya Pada Impedansi Surja." },
    { "en": "Apa Fungsi Transposisi Kabel?", "id": "Seimbangkan Impedansi Dan Tegangan Fasa." },
    { "en": "Apa Rumus Efisiensi String Isolator?", "id": "V_total Dibagi (n Kali V_max)." },
    { "en": "Apa Arti Efisiensi String 100%?", "id": "Tegangan Tiap Isolator Sama." },
    { "en": "Apa Faktor Yang Mempengaruhi Corona?", "id": "Cuaca, Diameter, Jarak Kabel." },
    { "en": "Apa Efek Visual Corona?", "id": "Cahaya Ungu Di Kabel." },
    { "en": "Apa Efek Suara Corona?", "id": "Suara Desis (Hissing Noise)." },
    { "en": "Apa Rumus Dasar Sag (Andongan)?", "id": "WL Kuadrat Dibagi 8T." },
    { "en": "Apa Efek Suhu Terhadap Sag?", "id": "Suhu Naik Sag Bertambah." },
    { "en": "Apa Efek Angin Terhadap Sag?", "id": "Angin Tambah Tegangan Mekanis." },
    { "en": "Apa Bahan Isolasi Kabel Tanah?", "id": "XLPE, PVC, Atau Kertas." },
    { "en": "Apa Fungsi Sheath Kabel Tanah?", "id": "Lindungi Dari Lembap Dan Kimia." },
    { "en": "Apa Fungsi Armouring Kabel Tanah?", "id": "Pelindung Mekanis Benturan." },
    { "en": "Apa Fungsi Serving Kabel Tanah?", "id": "Lindungi Armour Dari Karat." },
    { "en": "Apa Itu Capacitor Grading Kabel?", "id": "Ratakan Stres Dielektrik Isolasi." },
    { "en": "Apa Rumus Load Factor?", "id": "Beban Rata-rata Bagi Puncak." },
    { "en": "Apa Rumus Diversity Factor?", "id": "Jumlah Puncak Bagi Puncak Sistem." },
    { "en": "Apa Rumus Demand Factor?", "id": "Demand Maksimum Bagi Beban Terpasang." },
    { "en": "Apa Itu Base Load Plant?", "id": "Pembangkit Operasi Terus Menerus." },
    { "en": "Apa Itu Peak Load Plant?", "id": "Pembangkit Operasi Saat Beban Puncak." },
    { "en": "Apa Itu Tarif Flat Rate?", "id": "Harga Tetap Per KWh." },
    { "en": "Apa Itu Tarif Block Rate?", "id": "Harga Berubah Sesuai Blok Pakai." },
    { "en": "Apa Alat Perbaiki Faktor Daya?", "id": "Kapasitor Bank Atau Kondensor Sinkron." },
    { "en": "Apa Itu Symmetrical Fault?", "id": "Gangguan Tiga Fasa Seimbang." },
    { "en": "Apa Itu Unsymmetrical Fault?", "id": "Gangguan Satu Atau Dua Fasa." },
    { "en": "Apa Gangguan Paling Sering Terjadi?", "id": "Satu Fasa Ke Tanah (LG)." },
    { "en": "Apa Gangguan Paling Parah?", "id": "Hubung Singkat Tiga Fasa." },
    { "en": "Apa Fungsi Current Limiting Reactor?", "id": "Batasi Arus Hubung Singkat." },
    { "en": "Beda Isolator Dan Circuit Breaker?", "id": "Isolator Tanpa Beban, CB Berbeban." },
    { "en": "Apa Itu Making Capacity CB?", "id": "Arus Puncak Saat Menutup." },
    { "en": "Apa Itu Breaking Capacity CB?", "id": "Arus RMS Saat Memutus." },
    { "en": "Apa Bahan Kontak Arcing CB?", "id": "Tungsten Atau Tembaga Tungsten." },
    { "en": "Apa Sifat Gas SF6?", "id": "Elektronegatif Serap Elektron Bebas." },
    { "en": "Aplikasi Vacuum Circuit Breaker?", "id": "Tegangan Menengah 11kV Hingga 33kV." },
    { "en": "Apa Fungsi Relay Mho?", "id": "Proteksi Saluran Transmisi Panjang." },
    { "en": "Apa Fungsi Relay Impedance?", "id": "Proteksi Saluran Transmisi Menengah." },
    { "en": "Apa Fungsi Relay Reactance?", "id": "Proteksi Saluran Transmisi Pendek." },
    { "en": "Apa Prinsip Proteksi Diferensial?", "id": "Bandingkan Vektor Arus Masuk Keluar." },
    { "en": "Apa Itu Merz-Price Protection?", "id": "Proteksi Diferensial Alternator Trafo." },
    { "en": "Apa Fungsi Negative Sequence Relay?", "id": "Proteksi Beban Tidak Seimbang." },
    { "en": "Apa Itu IDMT Relay?", "id": "Inverse Definite Minimum Time." },
    { "en": "Apa Rumus PSM (Plug Setting Multiplier)?", "id": "Arus Gangguan Bagi Arus Setting." },
    { "en": "Apa Rumus TSM (Time Setting Multiplier)?", "id": "Pengali Waktu Operasi Relay." },
    { "en": "Apa Rumus Fusing Factor?", "id": "Arus Leleh Bagi Arus Nominal." },
    { "en": "Berapa Nilai Fusing Factor?", "id": "Selalu Lebih Besar Dari Satu." },
    { "en": "Apa Kepanjangan HRC Fuse?", "id": "High Rupturing Capacity." },
    { "en": "Dimana Lokasi Lightning Arrester?", "id": "Dekat Trafo Atau Gardu Induk." },
    { "en": "Apa Itu Rod Gap Arrester?", "id": "Sela Udara Paling Sederhana." },
    { "en": "Apa Sudut Lindung Kawat Tanah?", "id": "Sekitar 30 Hingga 45 Derajat." },
    { "en": "Apa Itu Semikonduktor Intrinsik?", "id": "Semikonduktor Murni Tanpa Doping." },
    { "en": "Apa Itu Semikonduktor Ekstrinsik?", "id": "Semikonduktor Dengan Campuran Doping." },
    { "en": "Apa Itu Energi Celah Pita (Band Gap)?", "id": "Energi Minimal Elektron Loncat Jalur." },
    { "en": "Apa Band Gap Silikon Pada Suhu Ruang?", "id": "1.1 Elektron Volt." },
    { "en": "Apa Band Gap Germanium Pada Suhu Ruang?", "id": "0.67 Elektron Volt." },
    { "en": "Apa Keunggulan Gallium Nitride (GaN)?", "id": "Efisiensi Tinggi Suhu Tinggi." },
    { "en": "Apa Fungsi Doping Trivalen?", "id": "Hasilkan Semikonduktor Tipe-P (Hole)." },
    { "en": "Apa Fungsi Doping Pentavalen?", "id": "Hasilkan Semikonduktor Tipe-N (Elektron)." },
    { "en": "Apa Itu Arus Difusi?", "id": "Gerak Muatan Akibat Beda Konsentrasi." },
    { "en": "Apa Itu Arus Drift?", "id": "Gerak Muatan Akibat Medan Listrik." },
    { "en": "Apa Itu Tegangan Breakdown Zener?", "id": "Tegangan Dadal Arah Mundur Stabil." },
    { "en": "Apa Itu Efek Early Pada Transistor?", "id": "Lebar Basis Berubah Akibat Tegangan." },
    { "en": "Apa Itu CMOS (Complementary Metal Oxide Semiconductor)?", "id": "Gabungan NMOS Dan PMOS Hemat Daya." },
    { "en": "Apa Keunggulan Logika TTL?", "id": "Tahan Terhadap Listrik Statis." },
    { "en": "Apa Kelemahan Logika TTL?", "id": "Konsumsi Daya Lebih Besar." },
    { "en": "Apa Itu Fan-out?", "id": "Jumlah Gerbang Yang Bisa Didrive." },
    { "en": "Apa Itu Noise Margin High?", "id": "Batas Bawah Tegangan Logika 1." },
    { "en": "Apa Itu Noise Margin Low?", "id": "Batas Atas Tegangan Logika 0." },
    { "en": "Apa Itu RAM Statis (SRAM)?", "id": "Memori Cepat Pakai Flip-flop." },
    { "en": "Apa Itu RAM Dinamis (DRAM)?", "id": "Memori Padat Pakai Kapasitor." },
    { "en": "Mengapa DRAM Perlu Refresh?", "id": "Muatan Kapasitor Bocor Seiring Waktu." },
    { "en": "Apa Itu Flash Memory?", "id": "Memori Non-volatile Bisa Hapus Elektrik." },
    { "en": "Apa Beda NAND Flash Dan NOR Flash?", "id": "NAND Padat, NOR Akses Cepat." },
    { "en": "Apa Itu Traksi Listrik DC?", "id": "Kereta Pakai Motor Seri DC." },
    { "en": "Apa Tegangan Listrik KRL Jabodetabek?", "id": "1500 Volt DC." },
    { "en": "Apa Itu Pantograf Kereta?", "id": "Alat Ambil Listrik Dari Kabel Atas." },
    { "en": "Apa Itu Listrik Aliran Atas (LAA)?", "id": "Kabel Suplai Daya Kereta Listrik." },
    { "en": "Apa Itu Rel Ketiga (Third Rail)?", "id": "Rel Tambahan Penyuplai Listrik Bawah." },
    { "en": "Apa Keuntungan Pengereman Regeneratif Kereta?", "id": "Kembalikan Energi Ke Jaringan Listrik." },
    { "en": "Apa Itu Deadman Switch Kereta?", "id": "Rem Otomatis Jika Masinis Pingsan." },
    { "en": "Apa Itu Sistem Sinyal Block?", "id": "Cegah Dua Kereta Satu Jalur." },
    { "en": "Apa Fungsi Motor Traksi AC Induksi?", "id": "Penggerak Kereta Modern Bebas Perawatan." },
    { "en": "Apa Fungsi Inverter VVVF Pada Kereta?", "id": "Atur Tegangan Frekuensi Motor AC." },
    { "en": "Apa Kepanjangan VVVF?", "id": "Variable Voltage Variable Frequency." },
    { "en": "Apa Itu HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Transfer Data Web." },
    { "en": "Apa Itu HTTPS?", "id": "HTTP Aman Dengan Enkripsi SSL." },
    { "en": "Apa Itu CoAP (Constrained Application Protocol)?", "id": "Protokol IoT Perangkat Daya Rendah." },
    { "en": "Apa Beda TCP Dan UDP?", "id": "TCP Terjamin, UDP Cepat." },
    { "en": "Apa Itu MAC Address Filtering?", "id": "Batasi Akses Berdasar ID Hardware." },
    { "en": "Apa Itu Port 80?", "id": "Port Standar Web Server HTTP." },
    { "en": "Apa Itu Port 443?", "id": "Port Standar Web Server HTTPS." },
    { "en": "Apa Itu Port 1883?", "id": "Port Standar Broker MQTT." },
    { "en": "Apa Itu JSON (JavaScript Object Notation)?", "id": "Format Data Teks Ringan Terbaca." },
    { "en": "Apa Fungsi API (Application Programming Interface)?", "id": "Jembatan Komunikasi Antar Software." },
    { "en": "Apa Itu Firmware OTA?", "id": "Update Software Lewat Udara/WiFi." },
    { "en": "Apa Fungsi Watchdog Timer Hardware?", "id": "Reset Chip Jika Software Macet." },
    { "en": "Apa Itu Brownout Reset?", "id": "Reset Saat Tegangan Tidak Stabil." },
    { "en": "Apa Itu Heat Shrink Ratio 2:1?", "id": "Menyusut Setengah Diameter Awal." },
    { "en": "Apa Itu Kabel Shielded Twisted Pair?", "id": "Kabel Pelintir Dengan Pelindung Noise." },
    { "en": "Apa Itu Konektor RJ45 Shielded?", "id": "Konektor LAN Dengan Besi Ground." },
    { "en": "Apa Fungsi Bootlace Ferrule?", "id": "Lindungi Ujung Kabel Serabut." },
    { "en": "Apa Warna Kabel Positif Aki Mobil?", "id": "Merah." },
    { "en": "Apa Warna Kabel Negatif Aki Mobil?", "id": "Hitam." },
    { "en": "Apa Itu Sekering Blade (Tancap)?", "id": "Sekering Pipih Standar Otomotif." },
    { "en": "Apa Warna Sekering Blade 10A?", "id": "Merah." },
    { "en": "Apa Warna Sekering Blade 15A?", "id": "Biru." },
    { "en": "Apa Warna Sekering Blade 20A?", "id": "Kuning." },
    { "en": "Apa Warna Sekering Blade 30A?", "id": "Hijau." },
    { "en": "Apa Fungsi Relay 4 Kaki Otomotif?", "id": "Saklar Beban Arus Besar." },
    { "en": "Apa Fungsi Relay 5 Kaki Otomotif?", "id": "Saklar Tukar Arus Besar." },
    { "en": "Apa Kaki 30 Pada Relay?", "id": "Sumber Arus Utama (Baterai)." },
    { "en": "Apa Kaki 87 Pada Relay?", "id": "Output Normally Open (NO)." },
    { "en": "Apa Kaki 87a Pada Relay?", "id": "Output Normally Closed (NC)." },
    { "en": "Apa Kaki 85 Dan 86 Relay?", "id": "Koil Pemicu Magnet." },
    { "en": "Apa Fungsi Flasher Sein?", "id": "Buat Lampu Sein Berkedip." },
    { "en": "Apa Itu CDI Motor?", "id": "Capacitor Discharge Ignition." },
    { "en": "Apa Fungsi Koil Motor?", "id": "Naikkan 12V Jadi Ribuan Volt." },
    { "en": "Apa Itu Pulser Motor?", "id": "Sensor Posisi Kruk As (Timing)." },
    { "en": "Apa Fungsi Kiprok (Rectifier Regulator)?", "id": "Penyearah Dan Pembatas Tegangan Pengisian." },
    { "en": "Apa Tanda Kiprok Rusak?", "id": "Lampu Sering Putus Atau Aki Tekor." },
    { "en": "Apa Itu ECU (Engine Control Unit)?", "id": "Komputer Pengatur Mesin Injeksi." },
    { "en": "Apa Fungsi Sensor O2 (Lambda)?", "id": "Ukur Sisa Oksigen Gas Buang." },
    { "en": "Apa Fungsi Sensor TPS (Throttle Position)?", "id": "Deteksi Bukaan Gas." },
    { "en": "Apa Fungsi Injektor Bahan Bakar?", "id": "Semprot Bensin Kabut Ke Mesin." },
    { "en": "Apa Prinsip Kerja Injektor?", "id": "Solenoid Buka Katup Jarum." },
    { "en": "Apa Itu Relay Stater (Bendik)?", "id": "Saklar Daya Motor Starter." },
    { "en": "Apa Penyebab Bunyi Cetek-cetek Starter?", "id": "Aki Lemah Atau Bendik Rusak." },
    { "en": "Apa Fungsi Alternator Mobil?", "id": "Pembangkit Listrik Saat Mesin Hidup." },
    { "en": "Apa Komponen Utama Alternator?", "id": "Rotor, Stator, Diode, IC Regulator." },
    { "en": "Apa Fungsi Carbon Brush Alternator?", "id": "Suplai Listrik Ke Rotor Berputar." },
    { "en": "Apa Tegangan Pengisian Aki Normal?", "id": "13.5 Hingga 14.5 Volt." },
    { "en": "Apa Itu Baterai Kering (Maintenance Free)?", "id": "Aki Tanpa Lubang Isi Air." },
    { "en": "Apa Itu CCA (Cold Cranking Amps)?", "id": "Arus Start Maksimal Suhu Dingin." },
    { "en": "Apa Fungsi Inverter DC Ke AC?", "id": "Ubah Aki Jadi Listrik Rumah." },
    { "en": "Apa Gelombang Inverter Murah?", "id": "Modified Sine Wave (Kotak)." },
    { "en": "Apa Gelombang Inverter Bagus?", "id": "Pure Sine Wave (Sinus Murni)." },
    { "en": "Apa Bahaya Inverter Kotak?", "id": "Peralatan Induktif Panas Dan Dengung." },
    { "en": "Apa Efisiensi Inverter Umumnya?", "id": "Sekitar 85 Hingga 90 Persen." },
    { "en": "Apa Itu MPPT Solar Charger?", "id": "Maximum Power Point Tracking." },
    { "en": "Apa Kelebihan MPPT Dibanding PWM?", "id": "Efisiensi Daya Lebih Tinggi 30%." },
    { "en": "Apa Itu Panel Surya Monokristalin?", "id": "Terbuat Dari Kristal Silikon Tunggal." },
    { "en": "Apa Itu Panel Surya Polikristalin?", "id": "Terbuat Dari Pecahan Kristal Silikon." },
    { "en": "Apa Itu Panel Surya Thin Film?", "id": "Lapisan Tipis Fleksibel Efisiensi Rendah." },
    { "en": "Apa Fungsi Dioda Bypass Panel?", "id": "Cegah Hotspot Saat Bayangan." },
    { "en": "Apa Fungsi Dioda Bloking Panel?", "id": "Cegah Arus Balik Malam Hari." },
    { "en": "Apa Itu PLTS Off-Grid?", "id": "Sistem Surya Tanpa Koneksi PLN." },
    { "en": "Apa Itu PLTS On-Grid?", "id": "Sistem Surya Terkoneksi PLN." },
    { "en": "Apa Itu Net Metering?", "id": "Meteran Ekspor Impor Listrik." },
    { "en": "Apa Itu GIS (Gas Insulated Switchgear)?", "id": "Gardu Induk Berisolasi Gas SF6." },
    { "en": "Apa Itu AIS (Air Insulated Switchgear)?", "id": "Gardu Induk Berisolasi Udara Terbuka." },
    { "en": "Apa Fungsi Disconnecting Switch (DS)?", "id": "Pemisah Rangkaian Tanpa Beban." },
    { "en": "Apa Fungsi Earthing Switch (ES)?", "id": "Hubungkan Konduktor Ke Tanah." },
    { "en": "Apa Syarat Mengoperasikan Disconnecting Switch?", "id": "Circuit Breaker Harus Posisi Terbuka." },
    { "en": "Apa Fungsi Wave Trap Gardu Induk?", "id": "Jebak Sinyal Komunikasi PLC." },
    { "en": "Apa Fungsi CVT (Capacitive Voltage Transformer)?", "id": "Trafo Tegangan Sekaligus Coupling PLC." },
    { "en": "Apa Itu Busbar Sistem Double Bus?", "id": "Dua Rel Daya Fleksibilitas Tinggi." },
    { "en": "Apa Fungsi Bus Coupler?", "id": "Hubungkan Dua Busbar Berbeda." },
    { "en": "Apa Itu Bay Gardu Induk?", "id": "Satu Set Peralatan Lengkap." },
    { "en": "Apa Itu Line Bay?", "id": "Bay Penghubung Saluran Transmisi." },
    { "en": "Apa Itu Trafo Bay?", "id": "Bay Penghubung Ke Transformator Daya." },
    { "en": "Apa Itu OSI Layer 1?", "id": "Physical Layer (Fisik Kabel)." },
    { "en": "Apa Itu OSI Layer 2?", "id": "Data Link Layer (MAC Address)." },
    { "en": "Apa Itu OSI Layer 3?", "id": "Network Layer (IP Address)." },
    { "en": "Apa Itu OSI Layer 4?", "id": "Transport Layer (TCP/UDP)." },
    { "en": "Apa Itu OSI Layer 7?", "id": "Application Layer (HTTP/FTP)." },
    { "en": "Apa Fungsi Switch Manageable?", "id": "Switch Bisa Dikonfigurasi VLAN." },
    { "en": "Apa Itu DHCP Lease Time?", "id": "Masa Berlaku Peminjaman IP." },
    { "en": "Apa Itu Gateway Jaringan?", "id": "Pintu Keluar Ke Internet." },
    { "en": "Apa Itu DNS Resolver?", "id": "Server Pencari Alamat IP Domain." },
    { "en": "Apa Fungsi Firewall Jaringan?", "id": "Saring Lalu Lintas Data Masuk." },
    { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Terowongan Koneksi Aman Terenkripsi." },
    { "en": "Apa Fungsi Dark Current Photodiode?", "id": "Arus Bocor Saat Gelap Total." },
    { "en": "Apa Itu Saturation Current Dioda?", "id": "Arus Bocor Saat Bias Mundur." },
    { "en": "Apa Fungsi Bootstrap Capacitor?", "id": "Suplai Gate Driver MOSFET Atas." },
    { "en": "Apa Itu Miller Plateau MOSFET?", "id": "Tegangan Gate Datar Saat Switching." },
    { "en": "Apa Fungsi Gate Driver IC?", "id": "Perkuat Sinyal Kontrol MOSFET." },
    { "en": "Apa Itu Dead Time PWM?", "id": "Jeda Cegah Hubung Singkat H-Bridge." },
    { "en": "Apa Fungsi Flyback Transformer?", "id": "Trafo Inti Ferit Celah Udara." },
    { "en": "Apa Itu Duty Cycle 50%?", "id": "Waktu On Sama Dengan Off." },
    { "en": "Apa Itu Duty Cycle 100%?", "id": "Sinyal Terus Menerus On (DC)." },
    { "en": "Apa Fungsi Pilot Signal EV Charger?", "id": "Komunikasi Status Mobil Dan Charger." },
    { "en": "Apa Itu Proximity Pilot (PP)?", "id": "Deteksi Konektor Mobil Terpasang." },
    { "en": "Apa Itu Control Pilot (CP)?", "id": "Atur Besar Arus Pengisian." },
    { "en": "Apa Protokol OCPP?", "id": "Komunikasi Charger Ke Server Pusat." },
    { "en": "Apa Itu V2G (Vehicle To Grid)?", "id": "Mobil Kirim Listrik Ke Jaringan." },
    { "en": "Apa Itu Flicker Pst?", "id": "Indeks Kedipan Jangka Pendek." },
    { "en": "Apa Itu Flicker Plt?", "id": "Indeks Kedipan Jangka Panjang." },
    { "en": "Apa Batas Harmonisa Tegangan IEEE 519?", "id": "Maksimal 5 Persen THD." },
    { "en": "Apa Itu Sag Tegangan (Voltage Dip)?", "id": "Tegangan Turun Sesaat (Kedip)." },
    { "en": "Apa Itu Interupsi Sesaat?", "id": "Padam Sangat Singkat (< 1 Menit)." },
    { "en": "Apa Penyebab Utama Voltage Sag?", "id": "Hubung Singkat Jarak Jauh." },
    { "en": "Apa Itu K-Factor Trafo?", "id": "Rating Penanganan Arus Harmonisa." },
    { "en": "Apa Fungsi Active Harmonic Filter?", "id": "Suntik Arus Lawan Harmonisa." },
    { "en": "Apa Itu Passive Harmonic Filter?", "id": "Filter LC Tala Frekuensi Tertentu." },
    { "en": "Apa Fungsi Kapasitor Detuning?", "id": "Cegah Resonansi Dengan Trafo." },
    { "en": "Apa Itu Displacement Power Factor?", "id": "Faktor Daya Gelombang Dasar Saja." },
    { "en": "Apa Itu Distortion Power Factor?", "id": "Faktor Daya Akibat Harmonisa." },
    { "en": "Apa True Power Factor?", "id": "Displacement PF Kali Distortion PF." },
    { "en": "Apa Fungsi Tang Press Hidrolik?", "id": "Krimping Skun Kabel Besar." },
    { "en": "Apa Fungsi Puller Bearing?", "id": "Alat Lepas Laher Dari As." },
    { "en": "Apa Fungsi Tracker Kabel Tanah?", "id": "Lacak Lokasi Kabel Tertanam." },
    { "en": "Apa Fungsi Thermal Imager?", "id": "Lihat Titik Panas Sambungan." },
    { "en": "Apa Fungsi Vibration Meter?", "id": "Ukur Getaran Motor/Mesin." },
    { "en": "Apa Fungsi Tachometer Laser?", "id": "Ukur RPM Tanpa Sentuh." },
    { "en": "Apa Fungsi Lux Meter Digital?", "id": "Ukur Intensitas Cahaya Ruangan." },
    { "en": "Apa Fungsi Sound Level Meter?", "id": "Ukur Kebisingan Suara (dB)." },
    { "en": "Apa Fungsi Anemometer Digital?", "id": "Ukur Kecepatan Aliran Udara." },
    { "en": "Apa Fungsi Hygrometer?", "id": "Ukur Kelembapan Udara Relatif." },
    { "en": "Apa Itu Thermocouple Calibrator?", "id": "Alat Simulasi Sinyal Suhu." },
    { "en": "Apa Fungsi Loop Calibrator 4-20mA?", "id": "Simulasi Arus Sensor Industri." },
    { "en": "Apa Itu ESD Safe Mat?", "id": "Alas Meja Anti Listrik Statis." },
    { "en": "Apa Fungsi Ionizer Fan?", "id": "Netralkan Muatan Statis Udara." },
    { "en": "Apa Resistansi Gelang ESD?", "id": "1 Mega Ohm (Aman)." },
    { "en": "Apa Fungsi Safety Padlock LOTO?", "id": "Gembok Isolasi Energi Berbahaya." },
    { "en": "Apa Warna Gembok LOTO Listrik?", "id": "Biasanya Merah." },
    { "en": "Apa Itu Hasp LOTO?", "id": "Pengait Gembok Jamak (Multi-lock)." },
    { "en": "Apa Itu Breaker Lockout?", "id": "Pengunci Tuas MCB/MCCB." },
    { "en": "Apa Itu Valve Lockout?", "id": "Pengunci Kran Pipa." },
    { "en": "Apa Fungsi Tag LOTO?", "id": "Label Peringatan Jangan Dioperasikan." },
    { "en": "Apa Fungsi Grounding Cluster?", "id": "Kabel Tanah Portable Tegangan Tinggi." },
    { "en": "Apa Itu Voltage Detector 20kV?", "id": "Tongkat Deteksi Tegangan Menengah." },
    { "en": "Apa Fungsi Hot Stick Teleskopik?", "id": "Tongkat Isolasi Kerja Berjarak." },
    { "en": "Apa Itu Sepatu Tahan Listrik 20kV?", "id": "Sepatu Isolasi Tegangan Menengah." },
    { "en": "Apa Fungsi Helm Proyek V-Guard?", "id": "Helm Tahan Benturan Dan Listrik." },
    { "en": "Apa Itu APD (Alat Pelindung Diri)?", "id": "Perlengkapan Keselamatan Perorangan." },
    { "en": "Apa Fungsi Vest Rompi Safety?", "id": "Reflektor Cahaya Agar Terlihat." },
    { "en": "Apa Fungsi Masker Respirator?", "id": "Saring Uap Kimia Atau Debu." },
    { "en": "Apa Fungsi Ear Muff?", "id": "Peredam Suara Bising Industri." },
    { "en": "Apa Fungsi Body Harness?", "id": "Sabuk Pengaman Kerja Ketinggian." },
    { "en": "Apa Itu Shock Absorber Lanyard?", "id": "Peredam Kejut Saat Jatuh." },
    { "en": "Apa Fungsi Carabiner?", "id": "Cincin Kait Logam Pengaman." },
    { "en": "Apa Itu Confined Space?", "id": "Ruang Terbatas Minim Oksigen." },
    { "en": "Apa Fungsi Gas Detector Portable?", "id": "Deteksi Gas Beracun Di Lapangan." },
    { "en": "Apa Itu LEL (Lower Explosive Limit)?", "id": "Batas Bawah Konsentrasi Ledakan." },
    { "en": "Apa Itu Sensor H2S?", "id": "Deteksi Gas Hidrogen Sulfida." },
    { "en": "Apa Itu Sensor CO?", "id": "Deteksi Gas Karbon Monoksida." },
    { "en": "Apa Fungsi Fire Blanket?", "id": "Selimut Pemadam Api Kecil." },
    { "en": "Apa Itu APAR Dry Chemical Powder?", "id": "Serbuk Kimia Pemadam Serbaguna." },
    { "en": "Apa Itu APAR CO2?", "id": "Gas Karbon Dioksida Pemadam Listrik." },
    { "en": "Apa Itu Hydrant Pillar?", "id": "Titik Ambil Air Pemadam Luar." },
    { "en": "Apa Itu Smoke Detector Photoelectric?", "id": "Deteksi Asap Menggunakan Cahaya." },
    { "en": "Apa Itu Heat Detector Rate of Rise?", "id": "Deteksi Kenaikan Suhu Cepat." },
    { "en": "Apa Itu Manual Call Point?", "id": "Tombol Alarm Kebakaran Manual." },
    { "en": "Apa Fungsi Fire Alarm Bell?", "id": "Bunyi Peringatan Evakuasi Kebakaran." },
    { "en": "Apa Fungsi Lampu Exit Emergency?", "id": "Petunjuk Arah Keluar Darurat." },
    { "en": "Apa Baterai Lampu Emergency?", "id": "Ni-Cd Atau Sealed Lead Acid." },
    { "en": "Apa Kepanjangan CAN Bus?", "id": "Controller Area Network Bus." },
    { "en": "Apa Itu CAN High?", "id": "Kabel Data Positif Jaringan CAN." },
    { "en": "Apa Itu CAN Low?", "id": "Kabel Data Negatif Jaringan CAN." },
    { "en": "Berapa Impedansi Kabel CAN Bus?", "id": "120 Ohm." },
    { "en": "Apa Fungsi Resistor Terminasi CAN?", "id": "Cegah Pantulan Sinyal Data." },
    { "en": "Dimana Letak Resistor Terminasi CAN?", "id": "Di Ujung Awal Dan Akhir Bus." },
    { "en": "Apa Itu Protokol LIN Bus?", "id": "Jaringan Komunikasi Otomotif Kecepatan Rendah." },
    { "en": "Apa Itu Protokol FlexRay?", "id": "Jaringan Otomotif Kecepatan Tinggi Real-time." },
    { "en": "Apa Itu OBD-II Port?", "id": "Soket Diagnostik Standar Mobil." },
    { "en": "Apa Fungsi Scanner OBD-II?", "id": "Baca Kode Error Mesin Mobil." },
    { "en": "Apa Itu Pola Radiasi Omnidirectional?", "id": "Memancar Segala Arah 360 Derajat." },
    { "en": "Apa Itu Pola Radiasi Directional?", "id": "Memancar Kuat Ke Satu Arah." },
    { "en": "Apa Itu Front To Back Ratio?", "id": "Perbandingan Sinyal Depan Dan Belakang." },
    { "en": "Apa Fungsi Antena Yagi?", "id": "Antena Pengarah Gain Tinggi." },
    { "en": "Apa Fungsi Reflektor Antena Parabola?", "id": "Fokuskan Sinyal Ke Titik Tengah." },
    { "en": "Apa Itu LNB (Low Noise Block)?", "id": "Penerima Sinyal Satelit Di Dish." },
    { "en": "Apa Fungsi Feedhorn Antena?", "id": "Kumpulkan Sinyal Pantulan Reflektor." },
    { "en": "Apa Itu Polarisasi Vertikal?", "id": "Gelombang Listrik Tegak Lurus Tanah." },
    { "en": "Apa Itu Polarisasi Horizontal?", "id": "Gelombang Listrik Sejajar Tanah." },
    { "en": "Apa Itu Polarisasi Circular?", "id": "Gelombang Berputar (Kanan Atau Kiri)." },
    { "en": "Apa Itu Mode Field Diameter?", "id": "Lebar Efektif Cahaya Dalam Fiber." },
    { "en": "Apa Itu Numerical Aperture Fiber?", "id": "Sudut Tangkap Cahaya Serat Optik." },
    { "en": "Apa Fungsi OTDR Dead Zone?", "id": "Area Buta Awal Pengukuran Kabel." },
    { "en": "Apa Penyebab Macrobending Loss?", "id": "Tekukan Kabel Radius Besar." },
    { "en": "Apa Penyebab Microbending Loss?", "id": "Tekanan Fisik Pada Inti Fiber." },
    { "en": "Apa Itu Dark Fiber?", "id": "Kabel Optik Terpasang Belum Aktif." },
    { "en": "Apa Itu Fusion Splicer?", "id": "Alat Sambung Fiber Dengan Peleburan." },
    { "en": "Apa Itu Mechanical Splicing?", "id": "Sambung Fiber Tanpa Peleburan Panas." },
    { "en": "Apa Fungsi Dust Cap Fiber?", "id": "Tutup Pelindung Konektor Dari Debu." },
    { "en": "Apa Bahaya Melihat Ujung Fiber?", "id": "Laser Merusak Retina Mata Permanen." },
    { "en": "Apa Itu Feedforward Control?", "id": "Kontrol Antisipasi Gangguan Sebelum Terjadi." },
    { "en": "Apa Itu Feedback Control?", "id": "Koreksi Berdasarkan Output Aktual." },
    { "en": "Apa Fungsi Setpoint Dalam Kontrol?", "id": "Nilai Target Yang Diinginkan." },
    { "en": "Apa Itu Manipulated Variable (MV)?", "id": "Sinyal Output Kontroler Ke Actuator." },
    { "en": "Apa Itu Disturbance (Gangguan)?", "id": "Faktor Luar Pengubah Nilai Proses." },
    { "en": "Apa Itu Lag Time Sistem?", "id": "Waktu Tunda Respon Sistem." },
    { "en": "Apa Itu Dead Time Sistem?", "id": "Waktu Sebelum Output Mulai Berubah." },
    { "en": "Apa Fungsi Cascade Control?", "id": "Dua Loop Kontrol Bertingkat." },
    { "en": "Apa Itu Split Range Control?", "id": "Satu Output Kendalikan Dua Actuator." },
    { "en": "Apa Itu Ratio Control?", "id": "Jaga Perbandingan Dua Aliran Proses." },
    { "en": "Apa Itu Polarization Index (PI)?", "id": "Rasio Megger 10 Menit Semenit." },
    { "en": "Apa Itu DAR (Dielectric Absorption Ratio)?", "id": "Rasio Megger 60 Detik 30 Detik." },
    { "en": "Apa Arti Nilai PI Di Atas 2?", "id": "Kondisi Isolasi Motor Sangat Baik." },
    { "en": "Apa Arti Nilai PI Di Bawah 1?", "id": "Isolasi Buruk Dan Berbahaya." },
    { "en": "Apa Penyebab Motor Panas Berlebih?", "id": "Beban Lebih Atau Ventilasi Buruk." },
    { "en": "Apa Penyebab Motor Mendengung?", "id": "Hilang Satu Fasa Atau Macet." },
    { "en": "Apa Itu Air Gap Motor?", "id": "Celah Udara Stator Dan Rotor." },
    { "en": "Apa Efek Air Gap Tidak Rata?", "id": "Getaran Dan Suara Berisik." },
    { "en": "Apa Fungsi Varnish Motor?", "id": "Rekatkan Dan Isolasi Kawat Lilitan." },
    { "en": "Apa Itu Rewinding Motor?", "id": "Gulung Ulang Kawat Tembaga Rusak." },
    { "en": "Apa Itu Pick And Place Machine?", "id": "Robot Pasang Komponen SMD Otomatis." },
    { "en": "Apa Itu Reflow Oven?", "id": "Pemanas Solder Pasta Komponen SMD." },
    { "en": "Apa Itu Wave Soldering Machine?", "id": "Solder Komponen THT Secara Massal." },
    { "en": "Apa Fungsi Solder Paste Printer?", "id": "Cetak Pasta Solder Ke PCB." },
    { "en": "Apa Itu AOI Machine?", "id": "Inspeksi Visual PCB Otomatis Kamera." },
    { "en": "Apa Itu Panelization PCB?", "id": "Gabung Banyak PCB Satu Papan." },
    { "en": "Apa Itu V-Cut PCB?", "id": "Garis Potong Untuk Memisahkan Panel." },
    { "en": "Apa Itu Breakaway Rail PCB?", "id": "Pinggiran PCB Untuk Pegangan Mesin." },
    { "en": "Apa Itu Conformal Coating?", "id": "Pelapis PCB Tahan Lembap Korosi." },
    { "en": "Apa Itu Potting Compound?", "id": "Cor Blok Elektronik Tahan Air." },
    { "en": "Apa Itu Fuse Bit AVR?", "id": "Konfigurasi Hardware Chip Mikrokontroler." },
    { "en": "Apa Fungsi Lock Bit AVR?", "id": "Proteksi Kode Agar Tidak Dibaca." },
    { "en": "Apa Itu Bootloader Arduino?", "id": "Program Awal Upload Kode Sketch." },
    { "en": "Apa Fungsi Pin RESET Mikro?", "id": "Mulai Ulang Eksekusi Program." },
    { "en": "Apa Itu Brown-out Detection (BOD)?", "id": "Reset Chip Saat Tegangan Drop." },
    { "en": "Apa Itu Watchdog Timer (WDT)?", "id": "Reset Chip Jika Program Macet." },
    { "en": "Apa Fungsi Crystal 16MHz?", "id": "Sumber Detak Sistem Arduino Uno." },
    { "en": "Apa Fungsi Kapasitor Load Crystal?", "id": "Stabilkan Osilasi Kristal Kuarsa." },
    { "en": "Apa Itu ISP (In-System Programming)?", "id": "Program Chip Tanpa Lepas Papan." },
    { "en": "Apa Fungsi Pin MISO?", "id": "Master In Slave Out (SPI)." },
    { "en": "Apa Fungsi Pin MOSI?", "id": "Master Out Slave In (SPI)." },
    { "en": "Apa Fungsi Pin SCK?", "id": "Serial Clock Sinyal SPI." },
    { "en": "Apa Fungsi Pin SS (Slave Select)?", "id": "Pilih Perangkat Target SPI." },
    { "en": "Apa Itu Resistor Array?", "id": "Beberapa Resistor Dalam Satu Paket." },
    { "en": "Apa Itu Kapasitor Tantalum?", "id": "Kapasitor Polar Stabil Ukuran Kecil." },
    { "en": "Apa Tanda Positif Kapasitor Tantalum?", "id": "Garis Di Body Adalah Positif." },
    { "en": "Apa Bahaya Kapasitor Tantalum Terbalik?", "id": "Meledak Dan Terbakar Hebat." },
    { "en": "Apa Itu Polymer Capacitor?", "id": "Elco Solid Tahan Lama." },
    { "en": "Apa Itu Supercapacitor (Ultracapacitor)?", "id": "Kapasitor Kapasitas Sangat Besar (Farad)." },
    { "en": "Apa Aplikasi Supercapacitor?", "id": "Backup Daya Memori Atau Starter." },
    { "en": "Apa Itu Induktor SMD?", "id": "Kumparan Chip Untuk Rangkaian Padat." },
    { "en": "Apa Itu Shielded Inductor?", "id": "Induktor Tertutup Tahan Noise Magnetik." },
    { "en": "Apa Fungsi TVS Diode Array?", "id": "Lindungi Jalur Data Dari ESD." },
    { "en": "Apa Itu Thermistor PTC Reset?", "id": "Sekering Otomatis Polimer (Polyfuse)." },
    { "en": "Apa Fungsi Gas Sensor Heater?", "id": "Panaskan Elemen Agar Reaktif." },
    { "en": "Apa Tegangan Heater Sensor MQ?", "id": "5 Volt AC Atau DC." },
    { "en": "Apa Itu Load Cell Beam?", "id": "Sensor Berat Batang Logam." },
    { "en": "Apa Itu Wheatstone Bridge Sensor?", "id": "Rangkaian 4 Resistor Pengukur Perubahan." },
    { "en": "Apa Itu Gauge Factor?", "id": "Sensitivitas Strain Gauge Terhadap Regangan." },
    { "en": "Apa Fungsi Cold Junction Thermocouple?", "id": "Referensi Suhu Sambungan Kabel." },
    { "en": "Apa Itu RTD 3 Wire?", "id": "Kompensasi Hambatan Kabel Satu Jalur." },
    { "en": "Apa Itu RTD 4 Wire?", "id": "Kompensasi Hambatan Kabel Total Presisi." },
    { "en": "Apa Itu Hall Effect Latch?", "id": "Sensor Magnet Mengunci Status (Memory)." },
    { "en": "Apa Itu Hall Effect Ratiometric?", "id": "Output Proporsional Kekuatan Magnet." },
    { "en": "Apa Fungsi Reed Switch?", "id": "Saklar Kaca Aktif Oleh Magnet." },
    { "en": "Apa Kelebihan Reed Switch?", "id": "Kontak Tertutup Kedap Udara." },
    { "en": "Apa Kekurangan Reed Switch?", "id": "Kaca Mudah Pecah Dan Getas." },
    { "en": "Apa Itu Mercury Switch (Tilt)?", "id": "Saklar Miring Berisi Air Raksa." },
    { "en": "Apa Itu Protokol Modbus RTU?", "id": "Komunikasi Serial Biner Master Slave." },
    { "en": "Apa Itu Protokol Modbus TCP?", "id": "Modbus Lewat Jaringan Ethernet." },
    { "en": "Apa Fungsi Address Modbus Slave?", "id": "Identitas Unik Perangkat Slave." },
    { "en": "Apa Itu Register Holding Modbus?", "id": "Alamat Data Baca Tulis (4xxxx)." },
    { "en": "Apa Itu Register Input Modbus?", "id": "Alamat Data Baca Saja (3xxxx)." },
    { "en": "Apa Fungsi CRC Pada Modbus?", "id": "Cek Error Data Transmisi." },
    { "en": "Apa Itu Profibus DP?", "id": "Protokol Fieldbus Kecepatan Tinggi." },
    { "en": "Apa Itu Profibus PA?", "id": "Protokol Fieldbus Proses Automation." },
    { "en": "Apa Warna Kabel Profibus DP?", "id": "Ungu (Violet)." },
    { "en": "Apa Warna Kabel Profibus PA?", "id": "Biru (Area Aman) Hitam." },
    { "en": "Apa Itu HART Communicator?", "id": "Alat Setting Transmitter 4-20mA." },
    { "en": "Apa Sinyal Digital HART?", "id": "Modulasi FSK Di Atas Analog." },
    { "en": "Apa Itu OPC Server?", "id": "Software Penghubung Hardware Beda Merek." },
    { "en": "Apa Kepanjangan OPC?", "id": "Open Platform Communications." },
    { "en": "Apa Itu SCADA Historian?", "id": "Database Penyimpan Data Jangka Panjang." },
    { "en": "Apa Fungsi RTU Dalam SCADA?", "id": "Remote Terminal Unit Lapangan." },
    { "en": "Apa Beda RTU Dan PLC?", "id": "RTU Tahan Lingkungan Ekstrem." },
    { "en": "Apa Itu DNP3 Protocol?", "id": "Protokol Telemetri Utilitas Listrik." },
    { "en": "Apa Itu IEC 61850?", "id": "Standar Komunikasi Gardu Induk Digital." },
    { "en": "Apa Fungsi GOOSE Message?", "id": "Pesan Cepat Antar Relay Proteksi." },
    { "en": "Apa Itu IED (Intelligent Electronic Device)?", "id": "Perangkat Kontrol Proteksi Cerdas." },
    { "en": "Apa Fungsi Kabel Fiber Optik OPGW?", "id": "Ground Wire Berisi Fiber Optik." },
    { "en": "Apa Itu ADSS Fiber Cable?", "id": "Kabel Fiber Gantung Tanpa Logam." },
    { "en": "Apa Fungsi Splice Closure Fiber?", "id": "Pelindung Sambungan Kabel Tanam/Tiang." },
    { "en": "Apa Itu OTDR Trace?", "id": "Grafik Jarak Vs Redaman Fiber." },
    { "en": "Apa Itu Event Dead Zone OTDR?", "id": "Jarak Buta Setelah Konektor." },
    { "en": "Apa Fungsi Visual Fault Locator?", "id": "Laser Merah Cek Fiber Putus." },
    { "en": "Apa Konektor Fiber SC-UPC?", "id": "Konektor Biru Ujung Rata." },
    { "en": "Apa Konektor Fiber SC-APC?", "id": "Konektor Hijau Ujung Miring." },
    { "en": "Mengapa Konektor APC Miring?", "id": "Kurangi Pantulan Balik (Return Loss)." },
    { "en": "Apa Itu SFP Transceiver LX?", "id": "Modul Fiber Jarak Menengah (10km)." },
    { "en": "Apa Itu SFP Transceiver SX?", "id": "Modul Fiber Jarak Pendek (550m)." },
    { "en": "Apa Itu SFP Transceiver ZX?", "id": "Modul Fiber Jarak Jauh (80km)." },
    { "en": "Apa Fungsi Media Converter LAN?", "id": "Ubah Sinyal Tembaga Ke Optik." },
    { "en": "Apa Itu PoE Switch?", "id": "Switch LAN Suplai Daya Perangkat." },
    { "en": "Apa Standar PoE 802.3af?", "id": "Daya Maksimal 15.4 Watt." },
    { "en": "Apa Standar PoE+ 802.3at?", "id": "Daya Maksimal 30 Watt." },
    { "en": "Apa Standar PoE++ 802.3bt?", "id": "Daya Maksimal 60 Hingga 100W." },
    { "en": "Apa Itu Passive PoE 24V?", "id": "Daya Langsung Tanpa Negosiasi." },
    { "en": "Apa Bahaya Passive PoE?", "id": "Bisa Merusak Perangkat Non-PoE." },
    { "en": "Apa Pinout Kabel Cross Over?", "id": "1-3 Dan 2-6 Ditukar." },
    { "en": "Apa Fungsi Kabel Console Cisco?", "id": "Konfigurasi Router Lewat Serial." },
    { "en": "Apa Warna Kabel Console Standar?", "id": "Biru Pipih (Rollover Cable)." },
    { "en": "Apa Itu IP Address Public?", "id": "Alamat IP Bisa Diakses Internet." },
    { "en": "Apa Itu IP Address Private?", "id": "Alamat IP Lokal Jaringan LAN." },
    { "en": "Apa Kelas IP 192.168.x.x?", "id": "Kelas C (Private)." },
    { "en": "Apa Kelas IP 10.x.x.x?", "id": "Kelas A (Private)." },
    { "en": "Apa Fungsi Subnet Mask /24?", "id": "254 Host Dalam Satu Jaringan." },
    { "en": "Apa Itu Default Gateway?", "id": "Alamat Router Menuju Internet." },
    { "en": "Apa Fungsi DNS Server 8.8.8.8?", "id": "Google Public DNS Resolver." },
    { "en": "Apa Itu DHCP Reservation?", "id": "IP Tetap Berdasarkan MAC Address." },
    { "en": "Apa Fungsi VLAN Tagging (802.1Q)?", "id": "Tandai Paket Data Beda Network." },
    { "en": "Apa Itu Trunk Link Switch?", "id": "Kabel Pembawa Banyak VLAN." },
    { "en": "Apa Itu Access Link Switch?", "id": "Kabel Menuju Satu Perangkat Akhir." },
    { "en": "Apa Fungsi Spanning Tree Protocol?", "id": "Cegah Loop Pada Switch Jaringan." },
    { "en": "Apa Itu Broadcast Storm?", "id": "Banjir Paket Lumpuhkan Jaringan." },
    { "en": "Apa Fungsi UPS Double Conversion?", "id": "Output Selalu Dari Inverter Stabil." },
    { "en": "Apa Fungsi Static Bypass UPS?", "id": "Pindah Ke PLN Jika UPS Rusak." },
    { "en": "Apa Fungsi Maintenance Bypass UPS?", "id": "Saklar Manual Untuk Servis UPS." },
    { "en": "Apa Tipe Baterai UPS Standar?", "id": "VRLA 12 Volt." },
    { "en": "Apa Tegangan Float Charge 12V?", "id": "13.5 Hingga 13.8 Volt." },
    { "en": "Apa Tegangan Cut-off Discharge 12V?", "id": "Sekitar 10.5 Hingga 11 Volt." },
    { "en": "Apa Satuan Kapasitas Genset?", "id": "kVA (Kilo Volt Ampere)." },
    { "en": "Apa Faktor Daya Genset Standar?", "id": "0.8 Lagging." },
    { "en": "Apa Itu Prime Power Genset?", "id": "Daya Untuk Beban Terus Menerus." },
    { "en": "Apa Itu Standby Power Genset?", "id": "Daya Cadangan Darurat Sesaat." },
    { "en": "Apa Fungsi AVR Genset?", "id": "Stabilkan Tegangan Output Generator." },
    { "en": "Apa Fungsi Governor Genset?", "id": "Stabilkan Putaran (Frekuensi) Mesin." },
    { "en": "Apa Itu Droop Control Genset?", "id": "Pengaturan Paralel Berbagi Beban." },
    { "en": "Apa Fungsi ATS Genset?", "id": "Pindah Sumber Otomatis Saat Padam." },
    { "en": "Apa Fungsi AMF Genset?", "id": "Start Stop Otomatis Ikut PLN." },
    { "en": "Apa Itu Load Bank Test?", "id": "Uji Pembebanan Buatan Genset." },
    { "en": "Apa Itu Wet Stacking Genset?", "id": "Sisa Bahan Bakar Akibat Beban Ringan." },
    { "en": "Apa Fungsi Kabel NYMHY?", "id": "Kabel Serabut Putih Fleksibel." },
    { "en": "Apa Fungsi Kabel NYYHY?", "id": "Kabel Serabut Hitam Portable." },
    { "en": "Apa Beda Kabel NYA Dan NYAF?", "id": "NYA Tunggal, NYAF Serabut." },
    { "en": "Apa Kapasitas Arus Kabel 1.5mm?", "id": "Sekitar 10 Hingga 16 Ampere." },
    { "en": "Apa Kapasitas Arus Kabel 2.5mm?", "id": "Sekitar 16 Hingga 25 Ampere." },
    { "en": "Apa Warna Fasa R Standar Baru?", "id": "Cokelat (L1)." },
    { "en": "Apa Warna Fasa S Standar Baru?", "id": "Hitam (L2)." },
    { "en": "Apa Warna Fasa T Standar Baru?", "id": "Abu-abu (L3)." },
    { "en": "Apa Warna Netral Standar Baru?", "id": "Biru." },
    { "en": "Apa Warna Grounding Standar Baru?", "id": "Kuning Strip Hijau." },
    { "en": "Apa Itu ELCB 30mA?", "id": "Proteksi Manusia Dari Sengatan." },
    { "en": "Apa Itu ELCB 300mA?", "id": "Proteksi Gedung Dari Kebakaran." },
    { "en": "Apa Fungsi RCBO?", "id": "Gabungan MCB Dan ELCB." },
    { "en": "Apa Fungsi Surge Arrester Panel?", "id": "Buang Tegangan Surja Petir." },
    { "en": "Apa Indikator Arrester Rusak?", "id": "Warna Berubah Menjadi Merah." },
    { "en": "Apa Itu Busbar Sisir (Comb)?", "id": "Jumper MCB Rapi Dan Aman." },
    { "en": "Apa Fungsi Terminal Block Ground?", "id": "Titik Kumpul Kabel Grounding." },
    { "en": "Apa Fungsi Pilot Lamp LED?", "id": "Indikator Fasa Bertegangan." },
    { "en": "Apa Fungsi Volt Meter Selector?", "id": "Pilih Fasa Yang Diukur." },
    { "en": "Apa Fungsi CT (Current Transformer)?", "id": "Turunkan Arus Untuk Pengukuran." },
    { "en": "Apa Rasio CT 100/5A?", "id": "100 Ampere Jadi 5 Ampere." },
    { "en": "Apa Bahaya Sekunder CT Terbuka?", "id": "Tegangan Tinggi Bisa Meledak." },
    { "en": "Apa Fungsi Selector Switch MOA?", "id": "Manual, Off, Auto." },
    { "en": "Apa Fungsi Push Button Emergency?", "id": "Tombol Stop Darurat Mengunci." },
    { "en": "Apa Fungsi Relay Timer TDR?", "id": "Tunda Waktu On Atau Off." },
    { "en": "Apa Fungsi Floatless Level Switch?", "id": "Kontrol Pompa Air Elektronik." },
    { "en": "Apa Fungsi Thermal Overload Relay?", "id": "Proteksi Motor Dari Beban Lebih." },
    { "en": "Apa Fungsi Resistor Bleeder Kapasitor?", "id": "Buang Muatan Sisa Saat Matikan." },
    { "en": "Apa Itu Kabel NYFGbY?", "id": "Kabel Tanah Perisai Baja Pipih." },
    { "en": "Apa Itu Kabel NYRGbY?", "id": "Kabel Tanah Perisai Kawat Baja." },
    { "en": "Apa Fungsi Perisai (Armour) Kabel?", "id": "Lindungi Kabel Dari Benturan Fisik." },
    { "en": "Apa Warna Kabel Grounding Standar?", "id": "Hijau Garis Kuning." },
    { "en": "Apa Fungsi Sepatu Kabel (Cable Lug)?", "id": "Koneksi Ujung Kabel Ke Terminal." },
    { "en": "Apa Itu Busbar Ampacity?", "id": "Kapasitas Hantar Arus Batang Tembaga." },
    { "en": "Apa Standar Kepadatan Arus Busbar?", "id": "Sekitar 2 Ampere Per Milimeter." },
    { "en": "Apa Fungsi Isolator Tumpu (Post Insulator)?", "id": "Penyangga Busbar Tegangan Tinggi." },
    { "en": "Apa Itu Panel LVMDP?", "id": "Low Voltage Main Distribution Panel." },
    { "en": "Apa Itu Panel SDP (Sub Distribution)?", "id": "Panel Pembagi Dari Panel Utama." },
    { "en": "Apa Fungsi Kapasitor Bank Panel?", "id": "Perbaiki Faktor Daya (Cos Phi)." },
    { "en": "Apa Itu Detuned Reactor Kapasitor?", "id": "Cegah Resonansi Harmonisa Kapasitor." },
    { "en": "Apa Fungsi Exhaust Fan Panel?", "id": "Buang Panas Dari Dalam Panel." },
    { "en": "Apa Itu Thermostat Panel?", "id": "Kontrol Otomatis Kipas Pendingin Panel." },
    { "en": "Apa Fungsi Heater Anti Kondensasi?", "id": "Cegah Embun Di Dalam Panel." },
    { "en": "Apa Itu Ingress Protection IP54?", "id": "Tahan Debu Dan Cipratan Air." },
    { "en": "Apa Itu Ingress Protection IP65?", "id": "Tahan Debu Dan Semprotan Air." },
    { "en": "Apa Fungsi Rubber Seal Panel?", "id": "Karet Pelindung Celah Pintu Panel." },
    { "en": "Apa Itu Wiring Duct (Trunking)?", "id": "Jalur Kabel Rapi Dalam Panel." },
    { "en": "Apa Fungsi DIN Rail Omega?", "id": "Rel Standar Pemasangan Komponen MCB." },
    { "en": "Apa Fungsi End Stopper Terminal?", "id": "Kunci Posisi Terminal Block." },
    { "en": "Apa Itu Current Transformer Split Core?", "id": "CT Pasang Tanpa Lepas Kabel." },
    { "en": "Apa Fungsi Ammeter Selector Switch?", "id": "Pilih Fasa Arus Yang Ditampilkan." },
    { "en": "Apa Fungsi Voltmeter Selector Switch?", "id": "Pilih Fasa Tegangan Yang Ditampilkan." },
    { "en": "Apa Itu Pilot Lamp LED?", "id": "Lampu Indikator Status Panel." },
    { "en": "Apa Warna Lampu Indikator R-S-T?", "id": "Merah, Kuning, Hijau." },
    { "en": "Apa Itu Kontak NO (Normally Open)?", "id": "Terbuka Saat Tidak Ditekan." },
    { "en": "Apa Itu Kontak NC (Normally Closed)?", "id": "Tertutup Saat Tidak Ditekan." },
    { "en": "Apa Fungsi Relay Timer (TDR)?", "id": "Saklar Tunda Waktu." },
    { "en": "Apa Itu Timer On-Delay?", "id": "Tunda Waktu Sebelum Hidup." },
    { "en": "Apa Itu Timer Off-Delay?", "id": "Tunda Waktu Sebelum Mati." },
    { "en": "Apa Fungsi Floatless Level Switch?", "id": "Kontrol Pompa Berbasis Elektroda Air." },
    { "en": "Apa Itu Elektroda E1 Level Switch?", "id": "Sensor Batas Atas (Stop)." },
    { "en": "Apa Itu Elektroda E2 Level Switch?", "id": "Sensor Batas Bawah (Start)." },
    { "en": "Apa Itu Elektroda E3 Level Switch?", "id": "Sensor Referensi Paling Bawah." },
    { "en": "Apa Tombol Reset Thermal Overload?", "id": "Kembalikan Kontak Setelah Trip." },
    { "en": "Apa Fungsi Magnetic Contactor?", "id": "Saklar Daya Utama Motor Listrik." },
    { "en": "Apa Itu Kontak Utama Kontaktor?", "id": "L1, L2, L3 Untuk Beban." },
    { "en": "Apa Itu Kontak Bantu Kontaktor?", "id": "NO/NC Untuk Rangkaian Kontrol." },
    { "en": "Apa Tegangan Coil Kontaktor Umum?", "id": "220V AC Atau 24V DC." },
    { "en": "Apa Fungsi Interlock Mekanis Kontaktor?", "id": "Cegah Dua Kontaktor Masuk Bersamaan." },
    { "en": "Apa Rangkaian Star-Delta Motor?", "id": "Start Bintang, Jalan Segitiga." },
    { "en": "Apa Tujuan Start Star-Delta?", "id": "Kurangi Arus Lonjakan Awal." },
    { "en": "Berapa Penurunan Arus Star-Delta?", "id": "Menjadi Sepertiga Arus DOL." },
    { "en": "Apa Rangkaian Direct On Line (DOL)?", "id": "Motor Langsung Sambung Sumber." },
    { "en": "Apa Fungsi Soft Starter?", "id": "Naikkan Tegangan Motor Perlahan." },
    { "en": "Apa Fungsi Variable Speed Drive?", "id": "Atur Kecepatan Putar Motor AC." },
    { "en": "Apa Itu Braking Resistor VSD?", "id": "Buang Energi Saat Pengereman Motor." },
    { "en": "Apa Itu Kabel Shielded VSD?", "id": "Kabel Motor Tahan Interferensi EMI." },
    { "en": "Apa Jarak Maksimal Kabel VSD?", "id": "Tergantung Filter Output VSD." },
    { "en": "Apa Fungsi Motor Circuit Breaker?", "id": "Proteksi Hubung Singkat Dan Overload." },
    { "en": "Apa Itu Nameplate Motor?", "id": "Plat Data Spesifikasi Motor." },
    { "en": "Apa Arti Frame Size Motor?", "id": "Ukuran Dimensi Fisik Standar Motor." },
    { "en": "Apa Arti Insulation Class F?", "id": "Tahan Suhu Hingga 155 Celcius." },
    { "en": "Apa Arti IP55 Pada Motor?", "id": "Tahan Debu Dan Semprotan Air." },
    { "en": "Apa Itu Service Factor Motor?", "id": "Kemampuan Operasi Di Atas Nominal." },
    { "en": "Apa Fungsi Bearing (Laher) Motor?", "id": "Bantalan Poros Rotor Berputar." },
    { "en": "Apa Penyebab Bearing Motor Berisik?", "id": "Pelumas Kering Atau Bola Rusak." },
    { "en": "Apa Fungsi Grease Bearing?", "id": "Pelumas Kurangi Gesekan Dan Panas." },
    { "en": "Apa Itu Alignment Poros Motor?", "id": "Luruskan Poros Motor Dan Beban." },
    { "en": "Apa Akibat Misalignment Poros?", "id": "Getaran Tinggi Dan Bearing Rusak." },
    { "en": "Apa Itu Coupling Motor?", "id": "Penyambung Poros Motor Ke Beban." },
    { "en": "Apa Fungsi Vibrasi Meter?", "id": "Ukur Getaran Untuk Diagnosa Kerusakan." },
    { "en": "Apa Satuan Vibrasi Motor?", "id": "Milimeter Per Detik (Velocity)." },
    { "en": "Apa Itu Tachometer Laser?", "id": "Alat Ukur RPM Tanpa Sentuh." },
    { "en": "Apa Itu Megger Test Motor?", "id": "Ukur Tahanan Isolasi Gulungan." },
    { "en": "Berapa Nilai Megger Motor Bagus?", "id": "Minimal 1 Mega Ohm Per kV." },
    { "en": "Apa Itu Surge Arrester Panel?", "id": "Proteksi Tegangan Surja Petir." },
    { "en": "Apa Kabel UTP Cat6?", "id": "Kabel LAN Gigabit 250 MHz." },
    { "en": "Apa Kabel UTP Cat5e?", "id": "Kabel LAN Gigabit 100 MHz." },
    { "en": "Apa Fungsi Cross Bar Pada Cat6?", "id": "Pemisah Antar Pasangan Kabel." },
    { "en": "Apa Jarak Maksimal Kabel UTP?", "id": "100 Meter Tanpa Repeater." },
    { "en": "Apa Itu Patch Panel LAN?", "id": "Terminal Kumpulan Kabel Jaringan." },
    { "en": "Apa Fungsi Modular Jack RJ45?", "id": "Lubang Konektor Dinding Atau Panel." },
    { "en": "Apa Alat Tes Kabel LAN?", "id": "LAN Tester (Cable Tester)." },
    { "en": "Apa Itu Fiber Optik Single Mode?", "id": "Inti Kecil Jarak Jauh." },
    { "en": "Apa Itu Fiber Optik Multi Mode?", "id": "Inti Besar Jarak Pendek." },
    { "en": "Apa Warna Kabel Fiber Single Mode?", "id": "Kuning." },
    { "en": "Apa Warna Kabel Fiber Multi Mode?", "id": "Oranye Atau Aqua." },
    { "en": "Apa Konektor Fiber SC?", "id": "Konektor Kotak Biru (Subscriber)." },
    { "en": "Apa Konektor Fiber LC?", "id": "Konektor Kotak Kecil (Lucent)." },
    { "en": "Apa Konektor Fiber FC?", "id": "Konektor Bulat Ulir (Ferrule)." },
    { "en": "Apa Konektor Fiber ST?", "id": "Konektor Bulat Bayonet (Straight Tip)." },
    { "en": "Apa Alat Sambung Fiber Optik?", "id": "Fusion Splicer." },
    { "en": "Apa Alat Potong Fiber Optik?", "id": "Fiber Cleaver." },
    { "en": "Apa Alat Kupas Fiber Optik?", "id": "Fiber Stripper." },
    { "en": "Apa Fungsi OPM (Optical Power Meter)?", "id": "Ukur Kekuatan Cahaya Sinyal." },
    { "en": "Apa Fungsi VFL (Visual Fault Locator)?", "id": "Senter Laser Cek Kabel Putus." },
    { "en": "Apa Itu SFP Transceiver?", "id": "Modul Konverter Switch Ke Fiber." },
    { "en": "Apa Fungsi Media Converter?", "id": "Ubah Sinyal LAN Ke Fiber." },
    { "en": "Apa Itu OTB (Optical Termination Box)?", "id": "Kotak Terminasi Ujung Kabel Fiber." },
    { "en": "Apa Itu Kabel Drop Core?", "id": "Kabel Fiber Masuk Rumah Pelanggan." },
    { "en": "Apa Fungsi Fast Connector Fiber?", "id": "Konektor Fiber Pasang Manual." },
    { "en": "Apa Bahaya Serat Kaca Fiber?", "id": "Tusuk Kulit Dan Sulit Diambil." },
    { "en": "Apa Itu Jarak Rambat (Creepage Distance)?", "id": "Jarak Permukaan Isolator Antar Konduktor." },
    { "en": "Apa Itu Jarak Bebas (Clearance Distance)?", "id": "Jarak Udara Terpendek Antar Konduktor." },
    { "en": "Apa Itu Tegangan Tembus (Breakdown Voltage)?", "id": "Tegangan Minimal Isolator Gagal Isolasi." },
    { "en": "Apa Itu Tegangan Flashover?", "id": "Loncatan Api Lewat Permukaan Isolator." },
    { "en": "Apa Itu BIL (Basic Insulation Level)?", "id": "Standar Ketahanan Tegangan Impuls Petir." },
    { "en": "Apa Itu SIL (Switching Impulse Level)?", "id": "Ketahanan Terhadap Lonjakan Switching." },
    { "en": "Apa Fungsi Gardu Pasang Luar?", "id": "Gardu Induk Di Area Terbuka." },
    { "en": "Apa Fungsi Gardu Pasang Dalam?", "id": "Gardu Induk Dalam Gedung Tertutup." },
    { "en": "Apa Itu Stockbridge Damper?", "id": "Peredam Getaran Kabel Transmisi." },
    { "en": "Apa Itu Spacer Damper?", "id": "Jaga Jarak Dan Redam Getaran." },
    { "en": "Apa Itu Galloping Conductor?", "id": "Ayunan Kabel Besar Akibat Angin." },
    { "en": "Apa Itu Aeolian Vibration?", "id": "Getaran Frekuensi Tinggi Akibat Angin." },
    { "en": "Apa Itu Armour Rod?", "id": "Pelindung Kabel Di Titik Jepit." },
    { "en": "Apa Fungsi Arcing Horn?", "id": "Lindungi Isolator Dari Busur Api." },
    { "en": "Apa Itu PUE (Power Usage Effectiveness)?", "id": "Rasio Total Energi Bagi IT." },
    { "en": "Apa Nilai PUE Ideal?", "id": "Mendekati Angka Satu Koma Nol." },
    { "en": "Apa Itu DCiE (Data Center Efficiency)?", "id": "Kebalikan Dari Rumus PUE." },
    { "en": "Apa Itu CRAC (Computer Room Air Conditioner)?", "id": "Pendingin Ruang Server Lantai." },
    { "en": "Apa Itu CRAH (Computer Room Air Handler)?", "id": "Pendingin Menggunakan Air Dingin (Chilled)." },
    { "en": "Apa Itu Hot Aisle Containment?", "id": "Kurung Udara Panas Agar Efisien." },
    { "en": "Apa Itu Cold Aisle Containment?", "id": "Kurung Udara Dingin Masuk Server." },
    { "en": "Apa Itu Tier III Data Center?", "id": "Dapat Diperbaiki Tanpa Matikan Sistem." },
    { "en": "Apa Itu Uptime Tier IV?", "id": "Jaminan Hidup 99.995 Persen." },
    { "en": "Apa Itu PCB Single Sided?", "id": "Jalur Tembaga Hanya Satu Sisi." },
    { "en": "Apa Itu PCB Double Sided?", "id": "Jalur Tembaga Di Kedua Sisi." },
    { "en": "Apa Itu PCB Multi Layer?", "id": "Lebih Dari Dua Lapis Tembaga." },
    { "en": "Apa Itu Core PCB?", "id": "Lapisan Dasar Keras Di Tengah." },
    { "en": "Apa Itu Prepreg PCB?", "id": "Lembaran Perekat Antar Layer PCB." },
    { "en": "Apa Itu Surface Finish OSP?", "id": "Pelapis Tembaga Organik Anti Karat." },
    { "en": "Apa Itu Surface Finish HASL?", "id": "Pelapis Timah Panas (Hot Air)." },
    { "en": "Apa Itu Surface Finish ENIG?", "id": "Pelapis Emas Nikel Tahan Lama." },
    { "en": "Apa Itu Thermal Relief Pad?", "id": "Pad Solder Dengan Celah Panas." },
    { "en": "Apa Fungsi Thermal Relief?", "id": "Memudahkan Solder Kaki Ground." },
    { "en": "Apa Itu PCB Stencil?", "id": "Cetakan Pasta Solder Stainless Steel." },
    { "en": "Apa Itu Fiducial Mark PCB?", "id": "Titik Referensi Mesin Pick Place." },
    { "en": "Apa Itu Protokol EtherNet/IP?", "id": "Protokol Industri Berbasis Ethernet Standar." },
    { "en": "Apa Itu Protokol Profinet?", "id": "Standar Ethernet Industri Dari Siemens." },
    { "en": "Apa Itu Protokol EtherCAT?", "id": "Ethernet Cepat Untuk Motion Control." },
    { "en": "Apa Itu Protokol Modbus ASCII?", "id": "Modbus Format Teks Terbaca Manusia." },
    { "en": "Apa Itu Protokol DeviceNet?", "id": "Jaringan Sensor Aktuator Berbasis CAN." },
    { "en": "Apa Itu Protokol CC-Link?", "id": "Jaringan Industri Populer Di Asia." },
    { "en": "Apa Itu Protokol BACnet?", "id": "Komunikasi Otomasi Gedung (Building Automation)." },
    { "en": "Apa Itu Protokol KNX?", "id": "Standar Dunia Otomasi Rumah Gedung." },
    { "en": "Apa Itu Protokol DALI?", "id": "Digital Addressable Lighting Interface." },
    { "en": "Apa Fungsi Gateway Protokol?", "id": "Penerjemah Bahasa Antar Protokol Berbeda." },
    { "en": "Apa Itu IO-Link?", "id": "Standar Komunikasi Sensor Cerdas." },
    { "en": "Apa Itu Relay ANSI Kode 50BF?", "id": "Breaker Failure (Gagal Trip)." },
    { "en": "Apa Itu Relay ANSI Kode 79?", "id": "Reclosing Relay (Tutup Balik Otomatis)." },
    { "en": "Apa Itu Relay ANSI Kode 25?", "id": "Synchronizing Check Relay." },
    { "en": "Apa Itu Relay ANSI Kode 52?", "id": "AC Circuit Breaker." },
    { "en": "Apa Itu Relay ANSI Kode 86?", "id": "Lockout Relay (Kunci Permanen)." },
    { "en": "Apa Itu Relay ANSI Kode 51N?", "id": "Overcurrent Netral Time Delay." },
    { "en": "Apa Itu Relay ANSI Kode 51G?", "id": "Overcurrent Ground Time Delay." },
    { "en": "Apa Itu Current Transformer Saturation?", "id": "Inti Jenuh Arus Output Cacat." },
    { "en": "Apa Itu Knee Point Voltage CT?", "id": "Titik Jenuh Kurva Eksitasi CT." },
    { "en": "Apa Itu Burden CT?", "id": "Beban Impedansi Rangkaian Sekunder CT." },
    { "en": "Apa Satuan Burden CT?", "id": "Volt Ampere (VA) Atau Ohm." },
    { "en": "Apa Itu Instrument Transformer?", "id": "Istilah Gabungan CT Dan PT." },
    { "en": "Apa Itu Motor Shaded Pole?", "id": "Motor Induksi Kecil Tanpa Kapasitor." },
    { "en": "Apa Aplikasi Motor Shaded Pole?", "id": "Kipas Angin Kecil Dan Pompa." },
    { "en": "Apa Itu Motor Histeresis?", "id": "Motor Sinkron Torsi Halus." },
    { "en": "Apa Aplikasi Motor Histeresis?", "id": "Pemutar Piringan Hitam Dan Jam." },
    { "en": "Apa Itu Motor Reluktansi Variabel?", "id": "Stepper Motor Torsi Reluktansi." },
    { "en": "Apa Itu Motor Linear?", "id": "Stator Rotor Bentang Lurus." },
    { "en": "Apa Aplikasi Motor Linear?", "id": "Kereta Maglev Dan Pintu Geser." },
    { "en": "Apa Itu Actuator Pneumatik?", "id": "Penggerak Menggunakan Tekanan Udara." },
    { "en": "Apa Itu Actuator Hidrolik?", "id": "Penggerak Menggunakan Tekanan Minyak." },
    { "en": "Apa Kelebihan Pneumatik?", "id": "Gerakan Cepat Dan Bersih." },
    { "en": "Apa Kelebihan Hidrolik?", "id": "Tenaga Angkat Sangat Besar." },
    { "en": "Apa Itu Solenoid Valve 5/2?", "id": "Lima Lubang Dua Posisi." },
    { "en": "Apa Itu Solenoid Valve 3/2?", "id": "Tiga Lubang Dua Posisi." },
    { "en": "Apa Fungsi Speed Controller Pneumatik?", "id": "Atur Debit Udara Masuk Silinder." },
    { "en": "Apa Itu Air Filter Regulator?", "id": "Saring Udara Dan Atur Tekanan." },
    { "en": "Apa Itu Lubricator Pneumatik?", "id": "Beri Pelumas Kabut Ke Sistem." },
    { "en": "Apa Itu Kabel Coaxial RG-11?", "id": "Kabel Coax Tebal Jarak Jauh." },
    { "en": "Apa Itu Kabel Coaxial RG-59?", "id": "Kabel CCTV Analog Standar." },
    { "en": "Apa Itu Kabel Coaxial RG-6?", "id": "Kabel Antena TV Dan Satelit." },
    { "en": "Apa Itu Konektor F-Type?", "id": "Konektor Ulir Antena TV/Parabola." },
    { "en": "Apa Itu Konektor BNC Compression?", "id": "Konektor CCTV Press Tahan Air." },
    { "en": "Apa Itu Balun Video CCTV?", "id": "Kirim Video Lewat Kabel UTP." },
    { "en": "Apa Itu Splitter HDMI?", "id": "Satu Input Ke Banyak Layar." },
    { "en": "Apa Itu Switch HDMI?", "id": "Banyak Input Ke Satu Layar." },
    { "en": "Apa Itu Kabel HDMI Active?", "id": "Kabel Panjang Dengan Penguat Sinyal." },
    { "en": "Apa Itu ARC (Audio Return Channel)?", "id": "Audio TV Balik Ke Speaker." },
    { "en": "Apa Itu CEC (Consumer Electronics Control)?", "id": "Satu Remote Kontrol Banyak Alat." },
    { "en": "Apa Itu HDCP?", "id": "Proteksi Konten Digital HDMI." },
    { "en": "Apa Itu Resolusi 1080p?", "id": "1920 Kali 1080 Piksel Progressive." },
    { "en": "Apa Itu Resolusi 1080i?", "id": "1920 Kali 1080 Piksel Interlaced." },
    { "en": "Apa Beda Progressive Dan Interlaced?", "id": "Progressive Utuh, Interlaced Ganjil Genap." },
    { "en": "Apa Itu Refresh Rate 60Hz?", "id": "Layar Update 60 Kali Sedetik." },
    { "en": "Apa Itu Resistor NTC 10D-20?", "id": "Diameter 20mm Resistansi 10 Ohm." },
    { "en": "Apa Itu Kapasitor X2?", "id": "Kapasitor Safety Filter AC Line." },
    { "en": "Apa Itu Kapasitor Y1?", "id": "Kapasitor Safety Line Ke Ground." },
    { "en": "Apa Itu Thermistor PTC Starter?", "id": "Komponen Start Kompresor Kulkas." },
    { "en": "Apa Itu Bimetal Defrost?", "id": "Sensor Suhu Pencair Bunga Es." },
    { "en": "Apa Fungsi Timer Kulkas?", "id": "Atur Siklus Dingin Dan Defrost." },
    { "en": "Apa Itu Heater Kaca Kulkas?", "id": "Cegah Embun Di Pintu Kaca." },
    { "en": "Apa Kepanjangan DMX512?", "id": "Digital Multiplex 512 Channel." },
    { "en": "Apa Fungsi Protokol DMX512?", "id": "Komunikasi Kontrol Lampu Panggung." },
    { "en": "Berapa Impedansi Kabel DMX?", "id": "120 Ohm (Standar RS-485)." },
    { "en": "Apa Itu DMX Terminator?", "id": "Resistor 120 Ohm Ujung Jalur." },
    { "en": "Apa Konektor Standar DMX Profesional?", "id": "XLR 5 Pin." },
    { "en": "Apa Itu DMX Universe?", "id": "Satu Jalur Data 512 Channel." },
    { "en": "Apa Itu RDM (Remote Device Management)?", "id": "Komunikasi Dua Arah Protokol DMX." },
    { "en": "Apa Itu Art-Net?", "id": "Protokol DMX Melalui Jaringan Ethernet." },
    { "en": "Apa Fungsi Dimmer Pack DMX?", "id": "Atur Tegangan Lampu Halogen Panggung." },
    { "en": "Apa Fungsi Moving Head Light?", "id": "Lampu Sorot Dengan Gerakan Robotik." },
    { "en": "Apa Itu Runner Turbin Air?", "id": "Bagian Berputar Yang Ditekan Air." },
    { "en": "Apa Fungsi Guide Vane PLTA?", "id": "Pengarah Aliran Air Masuk Runner." },
    { "en": "Apa Itu Draft Tube PLTA?", "id": "Pipa Buang Air Keluar Turbin." },
    { "en": "Apa Itu Water Hammer Effect?", "id": "Lonjakan Tekanan Air Pipa Pesat." },
    { "en": "Apa Fungsi Surge Tank PLTA?", "id": "Redam Tekanan Water Hammer." },
    { "en": "Apa Itu Turbin Pelton?", "id": "Turbin Impuls Untuk Head Tinggi." },
    { "en": "Apa Itu Turbin Francis?", "id": "Turbin Reaksi Head Menengah." },
    { "en": "Apa Itu Turbin Kaplan?", "id": "Turbin Reaksi Head Rendah Debit Besar." },
    { "en": "Apa Fungsi Governor PLTA?", "id": "Atur Debit Air Dan Putaran." },
    { "en": "Apa Itu Penstock PLTA?", "id": "Pipa Pesat Penyalur Air Tekanan." },
    { "en": "Apa Itu Trash Rack PLTA?", "id": "Saringan Sampah Di Intake Air." },
    { "en": "Apa Itu Spillway Bendungan?", "id": "Saluran Limpahan Air Berlebih." },
    { "en": "Apa Fungsi Exciter Generator?", "id": "Suplai Arus DC Ke Rotor." },
    { "en": "Apa Itu Black Start Generator?", "id": "Kemampuan Start Tanpa Listrik Luar." },
    { "en": "Apa Fungsi Coriolis Flowmeter?", "id": "Ukur Aliran Massa Secara Langsung." },
    { "en": "Apa Fungsi Magnetic Flowmeter (Magflow)?", "id": "Ukur Cairan Konduktif Hukum Faraday." },
    { "en": "Apa Fungsi Vortex Flowmeter?", "id": "Ukur Aliran Berdasarkan Pusaran Fluida." },
    { "en": "Apa Fungsi Ultrasonic Flowmeter?", "id": "Ukur Aliran Dengan Gelombang Suara." },
    { "en": "Apa Kelebihan Ultrasonic Flowmeter Clamp-on?", "id": "Pasang Tanpa Potong Pipa." },
    { "en": "Apa Itu Turbin Flowmeter?", "id": "Ukur Aliran Dengan Baling-baling." },
    { "en": "Apa Itu Rotameter?", "id": "Indikator Aliran Tabung Kaca Vertikal." },
    { "en": "Apa Itu Laminar Flow?", "id": "Aliran Fluida Halus Dan Teratur." },
    { "en": "Apa Itu Turbulent Flow?", "id": "Aliran Fluida Acak Dan Berputar." },
    { "en": "Apa Itu Bilangan Reynolds?", "id": "Indikator Jenis Aliran Fluida." },
    { "en": "Apa Itu Pick And Place Machine?", "id": "Robot Pasang Komponen SMD." },
    { "en": "Apa Itu Reflow Oven?", "id": "Pemanas Pasta Solder SMD." },
    { "en": "Apa Itu Solder Paste Printing?", "id": "Cetak Pasta Timah Lewat Stencil." },
    { "en": "Apa Itu Fiducial Mark?", "id": "Titik Referensi Optik Mesin SMT." },
    { "en": "Apa Itu Tombstoning Effect?", "id": "Komponen SMD Berdiri Saat Reflow." },
    { "en": "Apa Penyebab Tombstoning?", "id": "Pemanasan Tidak Rata Antar Pad." },
    { "en": "Apa Itu Solder Balling?", "id": "Butiran Timah Terpisah Di PCB." },
    { "en": "Apa Itu Bridging Solder?", "id": "Dua Kaki Terhubung Timah Berlebih." },
    { "en": "Apa Itu Cold Solder Joint?", "id": "Solderan Kusam Dan Tidak Kuat." },
    { "en": "Apa Itu Flux No-Clean?", "id": "Flux Sisa Tidak Perlu Dicuci." },
    { "en": "Apa Itu Wave Soldering?", "id": "Metode Solder Celup Gelombang Timah." },
    { "en": "Apa Itu Conformal Coating?", "id": "Lapisan Pelindung PCB Dari Lembap." },
    { "en": "Apa Itu ICT (In-Circuit Test)?", "id": "Tes Komponen PCB Dengan Jarum." },
    { "en": "Apa Itu AOI (Automated Optical Inspection)?", "id": "Inspeksi Visual PCB Dengan Kamera." },
    { "en": "Apa Itu X-Ray Inspection PCB?", "id": "Lihat Solderan Bawah Chip BGA." },
    { "en": "Apa Itu ESD Safe Area?", "id": "Area Bebas Listrik Statis." },
    { "en": "Apa Itu Moisture Sensitive Level (MSL)?", "id": "Ketahanan Komponen Terhadap Kelembapan Udara." },
    { "en": "Apa Itu Dry Cabinet?", "id": "Lemari Penyimpan Komponen Anti Lembap." },
    { "en": "Apa Itu Baking Komponen?", "id": "Panaskan Chip Hilangkan Uap Air." },
    { "en": "Apa Itu Thermal Relief Pad?", "id": "Pad Ground Dengan Celah Panas." },
    { "en": "Apa Fungsi Thermal Via?", "id": "Lubang Tembus Buang Panas." },
    { "en": "Apa Itu Blind Via?", "id": "Lubang Antar Layer Tidak Tembus." },
    { "en": "Apa Itu Buried Via?", "id": "Lubang Terkubur Di Layer Dalam." },
    { "en": "Apa Itu Impedance Control PCB?", "id": "Jaga Impedansi Jalur Sinyal Tepat." },
    { "en": "Apa Itu Rogers PCB Material?", "id": "Bahan PCB Frekuensi Tinggi (RF)." },
    { "en": "Apa Itu Aluminium PCB?", "id": "PCB Inti Logam Untuk LED." },
    { "en": "Apa Itu Flexible PCB (FPC)?", "id": "PCB Lentur Bahan Polimida." },
    { "en": "Apa Itu Rigid-Flex PCB?", "id": "Gabungan PCB Kaku Dan Lentur." },
    { "en": "Apa Fungsi V-Cut PCB?", "id": "Garis Potong Pemisah Panel PCB." },
    { "en": "Apa Itu Mouse Bites PCB?", "id": "Lubang Kecil Putus Panel." },
    { "en": "Apa Itu Castellated Hole?", "id": "Lubang Solder Di Pinggir Modul." },
    { "en": "Apa Itu Test Point PCB?", "id": "Pad Tembaga Untuk Probe Alat." },
    { "en": "Apa Itu Jumper Wire (Jamper)?", "id": "Kabel Penghubung Titik Rangkaian." },
    { "en": "Apa Fungsi Heat Shrink Tubing?", "id": "Isolasi Sambungan Kabel Bakar." },
    { "en": "Apa Itu Wire Wrapping?", "id": "Teknik Sambung Kabel Lilit." },
    { "en": "Apa Itu Breadboard Prototyping?", "id": "Rakit Rangkaian Tanpa Solder." },
    { "en": "Apa Itu Dead Bug Style?", "id": "Rakit Komponen IC Terbalik." },
    { "en": "Apa Itu Manhattan Style Soldering?", "id": "Teknik Solder RF Di Tembaga." },
    { "en": "Apa Fungsi Desoldering Wick?", "id": "Pita Tembaga Penyerap Timah." },
    { "en": "Apa Fungsi Solder Sucker?", "id": "Pompa Sedot Timah Cair." },
    { "en": "Apa Itu Tip Tinner?", "id": "Pembersih Dan Pelapis Mata Solder." },
    { "en": "Apa Itu Flux Pen?", "id": "Alat Oles Flux Praktis." },
    { "en": "Apa Itu Kabel AWG 30?", "id": "Kabel Tunggal Sangat Tipis (Wrapping)." },
    { "en": "Apa Itu Kabel AWG 24?", "id": "Ukuran Standar Kabel LAN/Jumper." },
    { "en": "Apa Itu Kabel Coaxial RG-316?", "id": "Kabel RF Tipis Tahan Panas." },
    { "en": "Apa Itu Kabel Coaxial LMR-400?", "id": "Kabel RF Besar Rugi Rendah." },
    { "en": "Apa Itu Konektor SMA Male?", "id": "Konektor RF Jarum Tengah." },
    { "en": "Apa Itu Konektor SMA Female?", "id": "Konektor RF Lubang Tengah." },
    { "en": "Apa Itu Konektor U.FL (IPEX)?", "id": "Konektor Antena WiFi Sangat Kecil." },
    { "en": "Apa Itu Konektor Banana 4mm?", "id": "Konektor Standar Kabel Multimeter." },
    { "en": "Apa Itu Konektor Aligator?", "id": "Jepit Buaya Untuk Koneksi Sementara." },
    { "en": "Apa Itu Konektor XT60?", "id": "Konektor Baterai Arus Besar (60A)." },
    { "en": "Apa Itu Konektor Dean (T-Plug)?", "id": "Konektor Baterai RC Jadul." },
    { "en": "Apa Itu Konektor JST-PH 2.0?", "id": "Konektor Baterai LiPo Kecil." },
    { "en": "Apa Itu Konektor Molex KK?", "id": "Konektor Putih Pin Header PCB." },
    { "en": "Apa Itu Pin Header Male?", "id": "Deretan Jarum Konek PCB." },
    { "en": "Apa Itu Pin Header Female?", "id": "Deretan Lubang Konek PCB." },
    { "en": "Apa Itu Terminal Screw Block?", "id": "Konektor Kabel Jepit Baut." },
    { "en": "Apa Itu Jack DC 5.5x2.1mm?", "id": "Ukuran Standar Adaptor Umum." },
    { "en": "Apa Itu Jack Audio 3.5mm?", "id": "Konektor Headphone Standar." },
    { "en": "Apa Itu Jack Audio 6.5mm?", "id": "Konektor Instrumen Musik/Mic." },
    { "en": "Apa Itu MIDI Connector?", "id": "DIN 5 Pin Alat Musik." },
    { "en": "Apa Itu DB9 Connector?", "id": "Port Serial RS-232 Komputer." },
    { "en": "Apa Itu DB25 Connector?", "id": "Port Paralel Printer Jadul." },
    { "en": "Apa Itu MCB Kurva D?", "id": "Trip 10 Sampai 20 Kali Nominal." },
    { "en": "Apa Kegunaan MCB Kurva D?", "id": "Beban Induktif Berat Seperti Trafo." },
    { "en": "Apa Itu MCB Kurva Z?", "id": "Trip 2 Sampai 3 Kali Nominal." },
    { "en": "Apa Kegunaan MCB Kurva Z?", "id": "Proteksi Semikonduktor Sangat Sensitif." },
    { "en": "Apa Itu AFDD (Arc Fault Detection)?", "id": "Deteksi Percikan Api Kabel Rusak." },
    { "en": "Apa Fungsi Magnetic Blowout Coil?", "id": "Padamkan Busur Api Breaker DC." },
    { "en": "Apa Itu Shunt Trip Breaker?", "id": "Trip Breaker Dari Sinyal Luar." },
    { "en": "Apa Itu UVR (Undervoltage Release)?", "id": "Trip Breaker Saat Tegangan Hilang." },
    { "en": "Apa Standar Tegangan PoE 802.3af?", "id": "44 Hingga 57 Volt DC." },
    { "en": "Apa Standar Tegangan PoE 802.3at?", "id": "50 Hingga 57 Volt DC." },
    { "en": "Apa Itu PoE Mode A?", "id": "Data Dan Daya Satu Kabel." },
    { "en": "Apa Itu PoE Mode B?", "id": "Daya Lewat Kabel Cadangan." },
    { "en": "Apa Warna Konektor USB 3.0?", "id": "Biru (Pantone 300C)." },
    { "en": "Apa Warna Konektor USB 2.0?", "id": "Hitam Atau Putih." },
    { "en": "Apa Warna Konektor USB Charging?", "id": "Kuning Atau Oranye." },
    { "en": "Apa Itu USB Type-C Alt Mode?", "id": "Kirim Video DisplayPort Lewat USB." },
    { "en": "Apa Impedansi Diferensial USB?", "id": "90 Ohm." },
    { "en": "Apa Impedansi Kabel Ethernet CAT5?", "id": "100 Ohm." },
    { "en": "Apa Impedansi Kabel Audio AES/EBU?", "id": "110 Ohm." },
    { "en": "Apa Impedansi Kabel DMX512?", "id": "120 Ohm." },
    { "en": "Apa Impedansi Kabel Antena TV?", "id": "75 Ohm." },
    { "en": "Apa Itu Skin Depth 50Hz?", "id": "Sekitar 9.3 Milimeter Tembaga." },
    { "en": "Apa Itu Skin Depth 100MHz?", "id": "Sekitar 6.6 Mikrometer Tembaga." },
    { "en": "Apa Fungsi Litz Wire?", "id": "Kurangi Skin Effect Frekuensi Tinggi." },
    { "en": "Apa Diameter Kawat AWG 18?", "id": "Sekitar 1.02 Milimeter." },
    { "en": "Apa Diameter Kawat AWG 24?", "id": "Sekitar 0.51 Milimeter." },
    { "en": "Apa Itu Voltage Drop?", "id": "Penurunan Voltase Akibat Panjang Kabel." },
    { "en": "Apa Rumus Voltage Drop DC?", "id": "(2 L I Rho) Dibagi A." },
    { "en": "Apa Itu Derating Factor Kabel?", "id": "Penurunan Kapasitas Akibat Suhu." },
    { "en": "Apa Itu NEMA 1 Enclosure?", "id": "Penggunaan Indoor Umum Tidak Kedap." },
    { "en": "Apa Itu NEMA 12 Enclosure?", "id": "Indoor Tahan Debu Dan Tetesan." },
    { "en": "Apa Itu NEMA 3R Enclosure?", "id": "Outdoor Tahan Hujan Dan Es." },
    { "en": "Apa Itu NEMA 4 Enclosure?", "id": "Watertight Tahan Semprotan Air Outdoor." },
    { "en": "Apa Itu NEMA 4X Enclosure?", "id": "Tahan Karat Dan Semprotan Air." },
    { "en": "Apa Persamaan NEMA 4X Dengan IP?", "id": "Setara Dengan IP66." },
    { "en": "Apa Persamaan NEMA 12 Dengan IP?", "id": "Setara Dengan IP54." },
    { "en": "Apa Itu Explosion Proof Enclosure?", "id": "Tahan Ledakan Internal Tanpa Bocor." },
    { "en": "Apa Klasifikasi Hazardous Zone 0?", "id": "Gas Meledak Selalu Ada." },
    { "en": "Apa Klasifikasi Hazardous Zone 1?", "id": "Gas Meledak Mungkin Ada Normal." },
    { "en": "Apa Klasifikasi Hazardous Zone 2?", "id": "Gas Meledak Jarang Ada." },
    { "en": "Apa Itu Intrinsic Safety (IS)?", "id": "Energi Dibatasi Bawah Titik Nyala." },
    { "en": "Apa Itu Purged Enclosure?", "id": "Tekanan Udara Positif Cegah Gas." },
    { "en": "Apa Material Thermocouple Tipe K?", "id": "Nikel-Kromium Dan Nikel-Aluminium." },
    { "en": "Apa Warna Konektor Tipe K ANSI?", "id": "Kuning." },
    { "en": "Apa Rentang Suhu Tipe K?", "id": "Minus 200 Hingga 1250 Celcius." },
    { "en": "Apa Material Thermocouple Tipe J?", "id": "Besi Dan Konstantan." },
    { "en": "Apa Warna Konektor Tipe J ANSI?", "id": "Hitam." },
    { "en": "Apa Material Thermocouple Tipe T?", "id": "Tembaga Dan Konstantan." },
    { "en": "Apa Warna Konektor Tipe T ANSI?", "id": "Biru." },
    { "en": "Apa Keunggulan Thermocouple Tipe T?", "id": "Akurat Untuk Suhu Sangat Rendah." },
    { "en": "Apa Itu RTD Pt100?", "id": "Resistansi 100 Ohm Pada 0C." },
    { "en": "Apa Itu RTD Pt1000?", "id": "Resistansi 1000 Ohm Pada 0C." },
    { "en": "Apa Koefisien Suhu Pt100?", "id": "0.00385 Ohm Per Ohm Derajat." },
    { "en": "Apa Itu Class A RTD?", "id": "Toleransi Error Sangat Kecil." },
    { "en": "Apa Itu Class B RTD?", "id": "Toleransi Error Standar Industri." },
    { "en": "Apa Fungsi Thermowell?", "id": "Lindungi Sensor Suhu Dari Tekanan." },
    { "en": "Apa Itu Cold Junction Compensation?", "id": "Koreksi Suhu Sambungan Terminal Alat." },
    { "en": "Apa Itu Efek Peltier Seebeck?", "id": "Konversi Panas Ke Listrik." },
    { "en": "Apa Itu Piezoelectric Buzzer?", "id": "Kristal Getar Hasilkan Suara." },
    { "en": "Apa Itu Magnetic Buzzer?", "id": "Koil Magnet Getarkan Membran Logam." },
    { "en": "Apa Keunggulan Magnetic Buzzer?", "id": "Suara Keras Di Tegangan Rendah." },
    { "en": "Apa Keunggulan Piezo Buzzer?", "id": "Konsumsi Arus Sangat Kecil." },
    { "en": "Apa Ukuran SMD Package 0805?", "id": "2.0mm Kali 1.25mm." },
    { "en": "Apa Ukuran SMD Package 0603?", "id": "1.6mm Kali 0.8mm." },
    { "en": "Apa Ukuran SMD Package 0402?", "id": "1.0mm Kali 0.5mm." },
    { "en": "Apa Ukuran SMD Package 1206?", "id": "3.2mm Kali 1.6mm." },
    { "en": "Apa Daya Resistor SMD 1206?", "id": "Biasanya 1/4 Watt (0.25W)." },
    { "en": "Apa Daya Resistor SMD 0805?", "id": "Biasanya 1/8 Watt (0.125W)." },
    { "en": "Apa Pitch Kaki IC SOIC?", "id": "1.27 Milimeter (50 mil)." },
    { "en": "Apa Pitch Kaki IC SSOP?", "id": "0.65 Milimeter." },
    { "en": "Apa Pitch Kaki IC TQFP?", "id": "Bervariasi 0.8mm Hingga 0.4mm." },
    { "en": "Apa Titik Leleh Solder 63/37?", "id": "183 Derajat Celcius." },
    { "en": "Apa Titik Leleh Solder SAC305?", "id": "217 Derajat Celcius (Lead Free)." },
    { "en": "Apa Itu Flux Type R (Rosin)?", "id": "Tidak Aktif, Sisa Tidak Korosif." },
    { "en": "Apa Itu Flux Type RMA?", "id": "Rosin Mildly Activated (Sedikit Aktif)." },
    { "en": "Apa Itu Flux Type RA?", "id": "Rosin Activated (Sangat Aktif)." },
    { "en": "Apa Itu Water Soluble Flux?", "id": "Sisa Flux Bisa Dicuci Air." },
    { "en": "Apa Itu No-Clean Flux?", "id": "Sisa Flux Sedikit Dan Aman." },
    { "en": "Apa Lebar Pita Desoldering Braid?", "id": "Lebar Tembaga Penyerap Timah." },
    { "en": "Apa Fungsi Coating Acrylic PCB?", "id": "Mudah Diperbaiki Tahan Lembap." },
    { "en": "Apa Fungsi Coating Silicone PCB?", "id": "Tahan Panas Tinggi Dan Fleksibel." },
    { "en": "Apa Fungsi Coating Urethane PCB?", "id": "Keras Dan Tahan Kimia." },
    { "en": "Apa Itu Potting Epoxy?", "id": "Cor Keras Tahan Benturan Kimia." },
    { "en": "Apa Itu Standar UL 94 V-0?", "id": "Plastik Padam Sendiri (Api)." },
    { "en": "Apa Bahan PCB FR-1?", "id": "Kertas Fenolik (Warna Cokelat)." },
    { "en": "Apa Bahan PCB FR-4?", "id": "Fiberglass Epoxy (Warna Hijau)." },
    { "en": "Apa Itu Tg (Glass Transition)?", "id": "Suhu PCB Mulai Melunak." },
    { "en": "Apa Arti 1 Oz Copper?", "id": "Berat Tembaga Per Kaki Persegi." },
    { "en": "Berapa Tebal Tembaga 1 Oz?", "id": "Sekitar 35 Mikrometer." },
    { "en": "Berapa Tebal Tembaga 2 Oz?", "id": "Sekitar 70 Mikrometer." },
    { "en": "Apa Itu Via Tenting?", "id": "Tutup Lubang Via Dengan Soldermask." },
    { "en": "Apa Itu Via Plugging?", "id": "Isi Lubang Via Dengan Epoxy." },
    { "en": "Apa Itu Gold Finger PCB?", "id": "Kontak Emas Keras Di Pinggir." },
    { "en": "Apa Fungsi V-Score PCB?", "id": "Potongan Parit Untuk Patahkan Panel." },
    { "en": "Apa Fungsi Mouse Bites PCB?", "id": "Lubang Berjajar Untuk Patahkan Panel." },
    { "en": "Apa Tebal Stencil Standar?", "id": "Biasanya 0.1mm Hingga 0.15mm." },
    { "en": "Apa Itu Soak Zone Reflow?", "id": "Fase Ratakan Suhu Seluruh PCB." },
    { "en": "Apa Itu Reflow Zone?", "id": "Fase Timah Meleleh Sempurna." },
    { "en": "Apa Itu Cooling Zone Reflow?", "id": "Fase Pendinginan Membekukan Timah." }



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
