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


  { "en": "Apa itu komunikasi serat optik?", "id": "Transmisi informasi menggunakan cahaya." },
  { "en": "Medium transmisi serat optik?", "id": "Kabel serat optik." },
  { "en": "Sinyal apa yang dibawa serat optik?", "id": "Sinyal cahaya (optik)." },
  { "en": "Keuntungan utama serat optik?", "id": "Bandwidth sangat lebar, kecepatan tinggi." },
  { "en": "Bahan dasar serat optik?", "id": "Kaca silika murni atau plastik." },
  { "en": "Prinsip dasar transmisi cahaya?", "id": "Total Internal Reflection (TIR)." },
  { "en": "Singkatan TIR (Total Internal Reflection)?", "id": "Total Internal Reflection." },
  { "en": "TIR (Total Internal Reflection) adalah?", "id": "Pemantulan internal total." },
  { "en": "Syarat terjadinya TIR (Total Internal Reflection)?", "id": "Cahaya dari medium rapat ke renggang." },
  { "en": "Syarat sudut pada TIR (Total Internal Reflection)?", "id": "Sudut datang lebih besar dari sudut kritis." },
  { "en": "Apa itu sudut kritis?", "id": "Sudut datang hasilkan sudut bias 90°." },
  { "en": "Tiga bagian utama serat optik?", "id": "Core, cladding, dan buffer coating." },
  { "en": "Apa itu 'core' (inti)?", "id": "Bagian tengah serat tempat cahaya merambat." },
  { "en": "Apa itu 'cladding' (selubung)?", "id": "Lapisan yang mengelilingi inti." },
  { "en": "Indeks bias 'core' dibanding 'cladding'?", "id": "Indeks bias core lebih tinggi." },
  { "en": "Mengapa indeks bias 'core' harus lebih tinggi?", "id": "Agar terjadi Total Internal Reflection (TIR)." },
  { "en": "Apa fungsi 'buffer coating'?", "id": "Melindungi serat dari kerusakan fisik." },
  { "en": "Ukuran diameter 'core' single-mode?", "id": "Sangat kecil (sekitar 9 mikrometer)." },
  { "en": "Ukuran diameter 'core' multi-mode?", "id": "Lebih besar (50 atau 62.5 mikrometer)." },
  { "en": "Apa itu indeks bias?", "id": "Ukuran kecepatan cahaya dalam material." },
  { "en": "Cahaya lebih lambat di medium?", "id": "Dengan indeks bias lebih tinggi." },
  { "en": "Tiga komponen sistem komunikasi optik?", "id": "Transmitter, serat optik, receiver." },
  { "en": "Fungsi transmitter (pemancar) optik?", "id": "Mengubah sinyal listrik menjadi sinyal cahaya." },
  { "en": "Komponen utama transmitter optik?", "id": "Sumber cahaya (laser atau LED)." },
  { "en": "Singkatan LED (Light Emitting Diode)?", "id": "Light Emitting Diode." },
  { "en": "Fungsi receiver (penerima) optik?", "id": "Mengubah sinyal cahaya kembali ke listrik." },
  { "en": "Komponen utama receiver optik?", "id": "Fotodetektor." },
  { "en": "Contoh fotodetektor?", "id": "Fotodioda PIN atau APD." },
  { "en": "Singkatan PIN (P-type, Intrinsic, N-type)?", "id": "P-type, Intrinsic, N-type." },
  { "en": "Singkatan APD (Avalanche Photodiode)?", "id": "Avalanche Photodiode." },
  { "en": "Dua jenis utama serat optik?", "id": "Single-mode dan multi-mode." },
  { "en": "Apa itu serat 'single-mode' (SMF)?", "id": "Serat yang hanya melewatkan satu mode." },
  { "en": "Singkatan SMF (Single-Mode Fiber)?", "id": "Single-Mode Fiber." },
  { "en": "Apa itu serat 'multi-mode' (MMF)?", "id": "Serat yang melewatkan banyak mode." },
  { "en": "Singkatan MMF (Multi-Mode Fiber)?", "id": "Multi-Mode Fiber." },
  { "en": "Apa itu 'mode' dalam serat optik?", "id": "Jalur yang bisa ditempuh cahaya." },
  { "en": "Serat mana untuk jarak jauh?", "id": "Single-mode fiber (SMF)." },
  { "en": "Serat mana untuk jarak pendek?", "id": "Multi-mode fiber (MMF)." },
  { "en": "Sumber cahaya untuk SMF (Single-Mode Fiber)?", "id": "Laser." },
  { "en": "Sumber cahaya untuk MMF (Multi-Mode Fiber)?", "id": "LED atau laser." },
  { "en": "Apa itu atenuasi?", "id": "Pelemahan sinyal seiring jarak." },
  { "en": "Satuan atenuasi?", "id": "Desibel per kilometer (dB/km)." },
  { "en": "Penyebab utama atenuasi?", "id": "Penyerapan dan hamburan." },
  { "en": "Apa itu 'absorption' (penyerapan)?", "id": "Energi cahaya diserap oleh material." },
  { "en": "Apa itu 'scattering' (hamburan)?", "id": "Cahaya dibelokkan ke arah acak." },
  { "en": "Jenis hamburan utama dalam serat?", "id": "Hamburan Rayleigh." },
  { "en": "Hamburan Rayleigh disebabkan oleh?", "id": "Fluktuasi densitas mikroskopis." },
  { "en": "Atenuasi lebih rendah pada?", "id": "Panjang gelombang yang lebih panjang." },
  { "en": "Apa itu 'jendela transmisi'?", "id": "Rentang panjang gelombang atenuasi rendah." },
  { "en": "Jendela transmisi pertama?", "id": "Sekitar 850 nm." },
  { "en": "Jendela transmisi kedua?", "id": "Sekitar 1310 nm." },
  { "en": "Jendela transmisi ketiga?", "id": "Sekitar 1550 nm." },
  { "en": "Jendela mana atenuasinya paling rendah?", "id": "Jendela ketiga (1550 nm)." },
  { "en": "Apa itu dispersi?", "id": "Pelebaran pulsa sinyal seiring jarak." },
  { "en": "Akibat dari dispersi?", "id": "Membatasi bandwidth dan laju data." },
  { "en": "Dua jenis dispersi utama?", "id": "Dispersi modal dan dispersi kromatik." },
  { "en": "Apa itu dispersi modal?", "id": "Pelebaran pulsa akibat perbedaan waktu tempuh." },
  { "en": "Dispersi modal hanya terjadi pada?", "id": "Serat multi-mode." },
  { "en": "Apa itu dispersi kromatik?", "id": "Pelebaran pulsa akibat perbedaan kecepatan." },
  { "en": "Penyebab dispersi kromatik?", "id": "Indeks bias bergantung panjang gelombang." },
  { "en": "Dispersi kromatik terdiri dari?", "id": "Dispersi material dan dispersi waveguide." },
  { "en": "Apa itu dispersi material?", "id": "Akibat sifat material kaca itu sendiri." },
  { "en": "Apa itu dispersi waveguide?", "id": "Akibat struktur pandu gelombang serat." },
  { "en": "Dispersi nol terjadi pada panjang gelombang?", "id": "Sekitar 1310 nm (serat standar)." },
  { "en": "Apa itu 'numerical aperture' (NA)?", "id": "Ukuran kemampuan serat menerima cahaya." },
  { "en": "Singkatan NA (Numerical Aperture)?", "id": "Numerical Aperture." },
  { "en": "NA (Numerical Aperture) yang lebih besar berarti?", "id": "Lebih banyak cahaya bisa masuk." },
  { "en": "Apa itu 'acceptance angle'?", "id": "Sudut penerimaan cahaya maksimum." },
  { "en": "Apa itu 'refraction' (pembiasan)?", "id": "Pembelokan cahaya saat melintasi medium." },
  { "en": "Hukum yang menjelaskan pembiasan?", "id": "Hukum Snellius." },
  { "en": "Apa itu 'reflection' (pemantulan)?", "id": "Cahaya memantul dari permukaan." },
  { "en": "Apa itu serat 'step-index'?", "id": "Serat dengan indeks bias inti uniform." },
  { "en": "Apa itu serat 'graded-index'?", "id": "Serat dengan indeks bias inti menurun." },
  { "en": "Tujuan profil 'graded-index'?", "id": "Mengurangi dispersi modal." },
  { "en": "Cahaya di 'graded-index' MMF merambat?", "id": "Dalam lintasan sinusoidal." },
  { "en": "Apa itu 'optical amplifier'?", "id": "Penguat sinyal optik tanpa konversi." },
  { "en": "Contoh 'optical amplifier'?", "id": "EDFA, Raman amplifier." },
  { "en": "Singkatan EDFA (Erbium-Doped Fiber Amplifier)?", "id": "Erbium-Doped Fiber Amplifier." },
  { "en": "EDFA (Erbium-Doped Fiber Amplifier) bekerja pada jendela?", "id": "Jendela ketiga (sekitar 1550 nm)." },
  { "en": "Apa itu 'regenerator'?", "id": "Memperbaiki bentuk sinyal (reshaping, retiming)." },
  { "en": "Perbedaan 'amplifier' dan 'regenerator'?", "id": "Regenerator membersihkan sinyal dari noise." },
  { "en": "Apa itu 'splicing'?", "id": "Proses menyambung dua serat optik." },
  { "en": "Jenis 'splicing'?", "id": "Fusion splicing dan mechanical splicing." },
  { "en": "Apa itu 'fusion splicing'?", "id": "Menyambung serat dengan melelehkannya." },
  { "en": "Apa itu 'mechanical splicing'?", "id": "Menyambung serat dengan penjepit mekanis." },
  { "en": "Mana sambungan yang lebih baik?", "id": "Fusion splicing." },
  { "en": "Apa itu konektor serat optik?", "id": "Menghubungkan serat ke perangkat." },
  { "en": "Contoh tipe konektor?", "id": "SC, ST, LC, FC." },
  { "en": "Kepanjangan SC (Subscriber Connector)?", "id": "Subscriber Connector." },
  { "en": "Kepanjangan LC (Lucent Connector)?", "id": "Lucent Connector." },
  { "en": "Apa itu 'pigtail'?", "id": "Serat optik dengan satu konektor." },
  { "en": "Apa itu 'patch cord'?", "id": "Serat optik dengan konektor di kedua ujung." },
  { "en": "Warna jaket SMF (Single-Mode Fiber) outdoor?", "id": "Biasanya hitam." },
  { "en": "Warna jaket SMF (Single-Mode Fiber) indoor?", "id": "Biasanya kuning." },
  { "en": "Warna jaket MMF (Multi-Mode Fiber)?", "id": "Biasanya oranye atau aqua." },
  { "en": "Apa itu dioda laser?", "id": "Sumber cahaya koheren semikonduktor." },
  { "en": "Prinsip kerja laser?", "id": "Emisi terstimulasi (stimulated emission)." },
  { "en": "Prinsip kerja LED (Light Emitting Diode)?", "id": "Emisi spontan (spontaneous emission)." },
  { "en": "Cahaya laser bersifat?", "id": "Monokromatik dan koheren." },
  { "en": "Apa itu 'spectral width'?", "id": "Lebar spektrum panjang gelombang sumber." },
  { "en": "Sumber mana 'spectral width' lebih sempit?", "id": "Laser." },
  { "en": "Jenis modulasi pada sumber cahaya?", "id": "Langsung dan eksternal." },
  { "en": "Apa itu modulasi langsung?", "id": "Memvariasikan arus penggerak sumber cahaya." },
  { "en": "Apa itu modulasi eksternal?", "id": "Menggunakan modulator terpisah setelah sumber." },
  { "en": "Apa itu 'chirp'?", "id": "Pergeseran frekuensi akibat modulasi." },
  { "en": "Jenis dioda laser yang umum?", "id": "Fabry-Perot, DFB, VCSEL." },
  { "en": "Singkatan DFB (Distributed Feedback)?", "id": "Distributed Feedback." },
  { "en": "Singkatan VCSEL (Vertical-Cavity Surface-Emitting Laser)?", "id": "Vertical-Cavity Surface-Emitting Laser." },
  { "en": "Apa itu 'responsivity' fotodetektor?", "id": "Rasio arus output terhadap daya optik." },
  { "en": "Satuan 'responsivity'?", "id": "Ampere per Watt (A/W)." },
  { "en": "Apa itu 'quantum efficiency'?", "id": "Efisiensi konversi foton ke elektron." },
  { "en": "Apa itu 'dark current'?", "id": "Arus bocor detektor tanpa cahaya." },
  { "en": "APD (Avalanche Photodiode) memiliki gain?", "id": "Ya, gain internal (multiplikasi avalanche)." },
  { "en": "Mana yang lebih sensitif, PIN atau APD?", "id": "APD (Avalanche Photodiode)." },
  { "en": "Kapan efek non-linear signifikan?", "id": "Pada daya optik tinggi dan jarak jauh." },
  { "en": "Singkatan SPM (Self-Phase Modulation)?", "id": "Self-Phase Modulation." },
  { "en": "Efek SPM (Self-Phase Modulation)?", "id": "Pelebaran spektral akibat daya sinyal." },
  { "en": "Singkatan XPM (Cross-Phase Modulation)?", "id": "Cross-Phase Modulation." },
  { "en": "Efek XPM (Cross-Phase Modulation)?", "id": "Fasa satu sinyal dipengaruhi sinyal lain." },
  { "en": "Singkatan FWM (Four-Wave Mixing)?", "id": "Four-Wave Mixing." },
  { "en": "Efek FWM (Four-Wave Mixing)?", "id": "Menghasilkan frekuensi baru yang tidak diinginkan." },
  { "en": "Singkatan SRS (Stimulated Raman Scattering)?", "id": "Stimulated Raman Scattering." },
  { "en": "Singkatan SBS (Stimulated Brillouin Scattering)?", "id": "Stimulated Brillouin Scattering." },
  { "en": "Apa itu 'soliton'?", "id": "Pulsa yang bentuknya tidak berubah." },
  { "en": "Singkatan WDM (Wavelength Division Multiplexing)?", "id": "Wavelength Division Multiplexing." },
  { "en": "Tujuan WDM (Wavelength Division Multiplexing)?", "id": "Meningkatkan kapasitas transmisi serat." },
  { "en": "Prinsip WDM (Wavelength Division Multiplexing)?", "id": "Mengirim sinyal di panjang gelombang berbeda." },
  { "en": "Komponen untuk menggabungkan sinyal WDM?", "id": "Multiplexer (MUX)." },
  { "en": "Komponen untuk memisahkan sinyal WDM?", "id": "Demultiplexer (DEMUX)." },
  { "en": "Singkatan DWDM (Dense Wavelength Division Multiplexing)?", "id": "Dense Wavelength Division Multiplexing." },
  { "en": "Singkatan CWDM (Coarse Wavelength Division Multiplexing)?", "id": "Coarse Wavelength Division Multiplexing." },
  { "en": "Mana yang jarak antar kanalnya lebih rapat?", "id": "DWDM." },
  { "en": "Mana yang kapasitasnya lebih besar?", "id": "DWDM." },
  { "en": "Apa itu 'ITU grid'?", "id": "Standar frekuensi untuk kanal DWDM." },
  { "en": "Singkatan ITU (International Telecommunication Union)?", "id": "International Telecommunication Union." },
  { "en": "Apa itu 'optical add-drop multiplexer' (OADM)?", "id": "Menambah/mengambil kanal tanpa demultiplexing." },
  { "en": "Singkatan OADM (Optical Add-Drop Multiplexer)?", "id": "Optical Add-Drop Multiplexer." },
  { "en": "Apa itu 'dispersion shifted fiber' (DSF)?", "id": "Serat dengan dispersi nol di 1550 nm." },
  { "en": "Singkatan DSF (Dispersion Shifted Fiber)?", "id": "Dispersion Shifted Fiber." },
  { "en": "Apa itu 'non-zero dispersion shifted fiber' (NZDSF)?", "id": "Varian DSF untuk mengurangi FWM." },
  { "en": "Singkatan NZDSF (Non-Zero Dispersion Shifted Fiber)?", "id": "Non-Zero Dispersion Shifted Fiber." },
  { "en": "Apa itu 'dispersion compensating fiber' (DCF)?", "id": "Serat untuk mengkompensasi dispersi." },
  { "en": "Singkatan DCF (Dispersion Compensating Fiber)?", "id": "Dispersion Compensating Fiber." },
  { "en": "DCF (Dispersion Compensating Fiber) memiliki dispersi?", "id": "Negatif besar." },
  { "en": "Apa itu 'polarization mode dispersion' (PMD)?", "id": "Dispersi akibat bentuk core tidak bulat." },
  { "en": "Singkatan PMD (Polarization Mode Dispersion)?", "id": "Polarization Mode Dispersion." },
  { "en": "Singkatan OTDR (Optical Time-Domain Reflectometer)?", "id": "Optical Time-Domain Reflectometer." },
  { "en": "Fungsi OTDR (Optical Time-Domain Reflectometer)?", "id": "Mengkarakterisasi serat dari satu ujung." },
  { "en": "Apa yang bisa dideteksi OTDR (Optical Time-Domain Reflectometer)?", "id": "Lokasi putus, loss, refleksi." },
  { "en": "Prinsip kerja OTDR (Optical Time-Domain Reflectometer)?", "id": "Mengukur sinyal hamburan-balik (backscatter)." },
  { "en": "Apa itu 'dead zone' pada OTDR?", "id": "Area di mana refleksi menutupi pengukuran." },
  { "en": "Apa itu 'optical power meter'?", "id": "Mengukur daya sinyal optik." },
  { "en": "Satuan daya optik?", "id": "dBm atau Watt." },
  { "en": "Apa itu 'optical light source' (OLS)?", "id": "Sumber cahaya stabil untuk pengetesan." },
  { "en": "Singkatan OLS (Optical Light Source)?", "id": "Optical Light Source." },
  { "en": "Alat untuk mengukur 'insertion loss'?", "id": "OLS dan optical power meter." },
  { "en": "Apa itu 'visual fault locator' (VFL)?", "id": "Laser merah untuk mencari patahan serat." },
  { "en": "Singkatan VFL (Visual Fault Locator)?", "id": "Visual Fault Locator." },
  { "en": "Apa itu 'optical spectrum analyzer' (OSA)?", "id": "Mengukur spektrum sinyal optik." },
  { "en": "Singkatan OSA (Optical Spectrum Analyzer)?", "id": "Optical Spectrum Analyzer." },
  { "en": "Apa itu 'bit error rate' (BER)?", "id": "Rasio bit error terhadap total bit." },
  { "en": "Singkatan BER (Bit Error Rate)?", "id": "Bit Error Rate." },
  { "en": "Alat untuk mengukur BER (Bit Error Rate)?", "id": "BERT (Bit Error Rate Tester)." },
  { "en": "Singkatan BERT (Bit Error Rate Tester)?", "id": "Bit Error Rate Tester." },
  { "en": "Apa itu diagram mata (eye diagram)?", "id": "Metode visualisasi kualitas sinyal digital." },
  { "en": "Diagram mata yang terbuka lebar berarti?", "id": "Kualitas sinyal baik." },
  { "en": "Apa itu 'loose tube' cable?", "id": "Jenis konstruksi kabel serat optik." },
  { "en": "Apa itu 'tight buffer' cable?", "id": "Jenis konstruksi kabel lain." },
  { "en": "Mana yang lebih cocok untuk outdoor?", "id": "Kabel loose tube." },
  { "en": "Apa itu 'tensile strength'?", "id": "Kekuatan tarik maksimum kabel." },
  { "en": "Material penguat pada kabel optik?", "id": "Aramid yarn atau fiberglass." },
  { "en": "Apa itu 'aramid yarn'?", "id": "Material penguat seperti Kevlar." },
  { "en": "Apa itu 'ribbon fiber'?", "id": "Serat optik yang disusun seperti pita." },
  { "en": "Apa itu 'optical switch'?", "id": "Mengalihkan sinyal cahaya antar serat." },
  { "en": "Apa itu 'optical circulator'?", "id": "Mengarahkan sinyal antar port secara berurutan." },
  { "en": "Apa itu 'optical isolator'?", "id": "Hanya melewatkan cahaya ke satu arah." },
  { "en": "Apa itu 'Faraday rotator'?", "id": "Komponen kunci dalam isolator optik." },
  { "en": "Apa itu 'return loss'?", "id": "Ukuran sinyal yang dipantulkan kembali." },
  { "en": "Return loss yang baik bernilai?", "id": "Sangat tinggi." },
  { "en": "Apa itu 'insertion loss'?", "id": "Pelemahan sinyal akibat komponen." },
  { "en": "Insertion loss yang baik bernilai?", "id": "Sangat rendah." },
  { "en": "Apa itu 'ferrule'?", "id": "Bagian presisi pada konektor serat." },
  { "en": "Bahan 'ferrule'?", "id": "Keramik (zirkonia)." },
  { "en": "Apa itu 'index matching gel'?", "id": "Gel untuk mengurangi refleksi." },
  { "en": "Apa itu 'mode field diameter' (MFD)?", "id": "Ukuran efektif dari core SMF." },
  { "en": "Singkatan MFD (Mode Field Diameter)?", "id": "Mode Field Diameter." },
  { "en": "Apa itu 'cut-off wavelength'?", "id": "Panjang gelombang batas operasi single-mode." },
  { "en": "Apa itu 'macrobending loss'?", "id": "Loss akibat lekukan besar pada serat." },
  { "en": "Apa itu 'microbending loss'?", "id": "Loss akibat lekukan kecil pada serat." },
  { "en": "Apa itu 'refractive index profile'?", "id": "Grafik indeks bias terhadap radius core." },
  { "en": "Serat step-index memiliki profil?", "id": "Berbentuk kotak." },
  { "en": "Serat graded-index memiliki profil?", "id": "Berbentuk parabola." },
  { "en": "Apa itu 'coherent communication'?", "id": "Komunikasi optik yang menggunakan fasa." },
  { "en": "Keuntungan 'coherent communication'?", "id": "Kapasitas dan sensitivitas sangat tinggi." },
  { "en": "Modulasi yang digunakan dalam sistem koheren?", "id": "PSK, QAM." },
  { "en": "Singkatan PSK (Phase-Shift Keying)?", "id": "Phase-Shift Keying." },
  { "en": "Singkatan QAM (Quadrature Amplitude Modulation)?", "id": "Quadrature Amplitude Modulation." },
  { "en": "Apa itu 'line coding'?", "id": "Metode merepresentasikan bit digital." },
  { "en": "Contoh 'line coding'?", "id": "NRZ, RZ, Manchester." },
  { "en": "Singkatan NRZ (Non-Return-to-Zero)?", "id": "Non-Return-to-Zero." },
  { "en": "Singkatan RZ (Return-to-Zero)?", "id": "Return-to-Zero." },
  { "en": "Apa itu 'forward error correction' (FEC)?", "id": "Teknik untuk mengkoreksi eror transmisi." },
  { "en": "Singkatan FEC (Forward Error Correction)?", "id": "Forward Error Correction." },
  { "en": "Cara kerja FEC (Forward Error Correction)?", "id": "Menambahkan bit redundan (paritas)." },
  { "en": "Apa itu 'optical signal-to-noise ratio' (OSNR)?", "id": "Rasio sinyal terhadap noise optik." },
  { "en": "Singkatan OSNR (Optical Signal-to-Noise Ratio)?", "id": "Optical Signal-to-Noise Ratio." },
  { "en": "OSNR (Optical Signal-to-Noise Ratio) yang baik bernilai?", "id": "Tinggi." },
  { "en": "Noise utama pada sistem ter-amplifikasi?", "id": "ASE (Amplified Spontaneous Emission)." },
  { "en": "Singkatan ASE (Amplified Spontaneous Emission)?", "id": "Amplified Spontaneous Emission." },
  { "en": "ASE (Amplified Spontaneous Emission) berasal dari?", "id": "Penguat optik (EDFA)." },
  { "en": "Singkatan PON (Passive Optical Network)?", "id": "Passive Optical Network." },
  { "en": "Arsitektur PON (Passive Optical Network) digunakan untuk?", "id": "Jaringan akses fiber (FTTH)." },
  { "en": "Singkatan FTTH (Fiber to the Home)?", "id": "Fiber to the Home." },
  { "en": "Mengapa disebut 'Passive Optical Network'?", "id": "Tidak ada komponen aktif di jaringan distribusi." },
  { "en": "Komponen pasif utama pada PON?", "id": "Optical splitter." },
  { "en": "Fungsi 'optical splitter'?", "id": "Membagi sinyal optik ke banyak pengguna." },
  { "en": "Perangkat di sisi sentral pada PON?", "id": "OLT (Optical Line Terminal)." },
  { "en": "Singkatan OLT (Optical Line Terminal)?", "id": "Optical Line Terminal." },
  { "en": "Perangkat di sisi pelanggan pada PON?", "id": "ONT atau ONU." },
  { "en": "Singkatan ONT (Optical Network Terminal)?", "id": "Optical Network Terminal." },
  { "en": "Singkatan ONU (Optical Network Unit)?", "id": "Optical Network Unit." },
  { "en": "Topologi jaringan PON (Passive Optical Network)?", "id": "Point-to-multipoint." },
  { "en": "Standar PON (Passive Optical Network) yang umum?", "id": "GPON, EPON." },
  { "en": "Singkatan GPON (Gigabit Passive Optical Network)?", "id": "Gigabit Passive Optical Network." },
  { "en": "Singkatan EPON (Ethernet Passive Optical Network)?", "id": "Ethernet Passive Optical Network." },
  { "en": "Transmisi 'downstream' (OLT ke ONT) menggunakan?", "id": "Broadcast." },
  { "en": "Transmisi 'upstream' (ONT ke OLT) menggunakan?", "id": "TDMA (Time Division Multiple Access)." },
  { "en": "Singkatan TDMA (Time Division Multiple Access)?", "id": "Time Division Multiple Access." },
  { "en": "Singkatan SONET (Synchronous Optical Networking)?", "id": "Synchronous Optical Networking." },
  { "en": "Standar internasional yang setara SONET?", "id": "SDH (Synchronous Digital Hierarchy)." },
  { "en": "Singkatan SDH (Synchronous Digital Hierarchy)?", "id": "Synchronous Digital Hierarchy." },
  { "en": "SONET/SDH adalah standar untuk?", "id": "Jaringan transport optik digital." },
  { "en": "Topologi jaringan SONET/SDH yang umum?", "id": "Cincin (ring) dua arah." },
  { "en": "Keuntungan topologi cincin?", "id": "Memiliki proteksi (jalur cadangan)." },
  { "en": "Apa itu 'optical fiber sensor'?", "id": "Sensor yang menggunakan serat optik." },
  { "en": "Keuntungan utama 'optical fiber sensor'?", "id": "Kebal EMI, aman, sensitif." },
  { "en": "Apa itu sensor FBG (Fiber Bragg Grating)?", "id": "Sensor berbasis kisi dalam serat." },
  { "en": "Singkatan FBG (Fiber Bragg Grating)?", "id": "Fiber Bragg Grating." },
  { "en": "Sensor FBG (Fiber Bragg Grating) mendeteksi?", "id": "Regangan dan suhu." },
  { "en": "Prinsip sensor FBG (Fiber Bragg Grating)?", "id": "Perubahan panjang gelombang pantulan." },
  { "en": "Apa itu giroskop serat optik (FOG)?", "id": "Sensor rotasi presisi tinggi." },
  { "en": "Singkatan FOG (Fiber Optic Gyroscope)?", "id": "Fiber Optic Gyroscope." },
  { "en": "Prinsip kerja FOG (Fiber Optic Gyroscope)?", "id": "Efek Sagnac." },
  { "en": "Apa itu 'distributed temperature sensing' (DTS)?", "id": "Mengukur suhu di sepanjang serat." },
  { "en": "Singkatan DTS (Distributed Temperature Sensing)?", "id": "Distributed Temperature Sensing." },
  { "en": "Mengapa silika menjadi bahan utama serat?", "id": "Transparansi tinggi dan kuat." },
  { "en": "Apa itu 'preform'?", "id": "Batang kaca besar cikal bakal serat." },
  { "en": "Proses pembuatan serat dari 'preform'?", "id": "Fiber drawing (penarikan serat)." },
  { "en": "Apa itu 'drawing tower'?", "id": "Menara tinggi untuk proses penarikan serat." },
  { "en": "Proses pembuatan 'preform'?", "id": "MCVD, OVD, VAD." },
  { "en": "Singkatan MCVD (Modified Chemical Vapor Deposition)?", "id": "Modified Chemical Vapor Deposition." },
  { "en": "Apa itu 'cleaving'?", "id": "Proses memotong serat dengan rata." },
  { "en": "Alat untuk 'cleaving'?", "id": "Fiber cleaver." },
  { "en": "Apa itu 'fiber stripper'?", "id": "Alat untuk mengupas lapisan coating." },
  { "en": "Mengapa 'cleaving' penting sebelum 'splicing'?", "id": "Agar kedua ujung rata sempurna." },
  { "en": "Apa itu 'optical attenuator'?", "id": "Komponen untuk melemahkan sinyal optik." },
  { "en": "Apa itu 'optical coupler' (splitter)?", "id": "Membagi atau menggabungkan sinyal optik." },
  { "en": "Apa itu 'coupler' 2x2?", "id": "Memiliki dua input dan dua output." },
  { "en": "Apa itu 'mode stripper'?", "id": "Menghilangkan mode yang tidak diinginkan." },
  { "en": "Apa itu 'cladding mode'?", "id": "Cahaya yang merambat di dalam cladding." },
  { "en": "Apa itu 'launch cable'?", "id": "Serat yang digunakan di awal pengukuran OTDR." },
  { "en": "Nama lain 'launch cable'?", "id": "Pulse suppressor." },
  { "en": "Apa itu 'tail cable'?", "id": "Serat yang digunakan di akhir pengukuran OTDR." },
  { "en": "Apa itu 'optical link budget'?", "id": "Perhitungan total loss dalam link." },
  { "en": "Tujuan 'link budget'?", "id": "Memastikan daya cukup di penerima." },
  { "en": "Apa itu 'power penalty'?", "id": "Peningkatan daya akibat efek non-ideal." },
  { "en": "Apa itu 'modal noise'?", "id": "Noise pada serat MMF akibat interferensi." },
  { "en": "Apa itu 'relative intensity noise' (RIN)?", "id": "Fluktuasi intensitas pada output laser." },
  { "en": "Singkatan RIN (Relative Intensity Noise)?", "id": "Relative Intensity Noise." },
  { "en": "Apa itu 'shot noise'?", "id": "Noise akibat sifat diskrit foton/elektron." },
  { "en": "Apa itu 'thermal noise'?", "id": "Noise akibat agitasi termal di resistor." },
  { "en": "Noise mana yang dominan pada fotodetektor?", "id": "Shot noise dan thermal noise." },
  { "en": "Apa itu 'extinction ratio'?", "id": "Rasio daya saat bit 1 dan 0." },
  { "en": "Extinction ratio yang baik bernilai?", "id": "Tinggi." },
  { "en": "Apa itu 'optical communication'?", "id": "Sinonim komunikasi serat optik." },
  { "en": "Apa itu 'free-space optical' (FSO) communication?", "id": "Komunikasi optik lewat udara." },
  { "en": "Singkatan FSO (Free-Space Optical)?", "id": "Free-Space Optical." },
  { "en": "Kelemahan FSO (Free-Space Optical)?", "id": "Rentan terhadap cuaca (kabut, hujan)." },
  { "en": "Apa itu 'optical fiber'?", "id": "Sinonim untuk serat optik." },
  { "en": "Apa itu 'photonic crystal fiber' (PCF)?", "id": "Jenis serat optik mikrostruktur." },
  { "en": "Singkatan PCF (Photonic Crystal Fiber)?", "id": "Photonic Crystal Fiber." },
  { "en": "Apa itu 'hollow-core fiber'?", "id": "Serat yang intinya berisi udara/vakum." },
  { "en": "Keuntungan 'hollow-core fiber'?", "id": "Latensi lebih rendah, non-linearitas rendah." },
  { "en": "Apa itu 'polarization maintaining' (PM) fiber?", "id": "Serat yang menjaga polarisasi cahaya." },
  { "en": "Singkatan PM (Polarization Maintaining)?", "id": "Polarization Maintaining." },
  { "en": "Apa itu 'laser safety class'?", "id": "Klasifikasi keamanan produk laser." },
  { "en": "Kelas laser yang aman untuk mata?", "id": "Kelas 1." },
  { "en": "Apa itu 'eye safety'?", "id": "Keamanan mata terhadap radiasi optik." },
  { "en": "Panjang gelombang 1550 nm lebih aman karena?", "id": "Diserap oleh kornea dan lensa." },
  { "en": "Apa itu 'chromatic dispersion coefficient'?", "id": "Ukuran dispersi kromatik per kilometer." },
  { "en": "Satuan 'chromatic dispersion coefficient'?", "id": "ps/(nm·km)." },
  { "en": "Apa itu 'rise time budget'?", "id": "Analisis untuk menentukan batas bandwidth." },
  { "en": "Tujuan 'rise time budget'?", "id": "Memastikan sistem cukup cepat." },
  { "en": "Apa itu 'quantum limit'?", "id": "Batas sensitivitas teoritis penerima optik." },
  { "en": "Apa itu 'Fused Biconical Taper' (FBT) coupler?", "id": "Jenis coupler serat optik." },
  { "en": "Singkatan FBT (Fused Biconical Taper)?", "id": "Fused Biconical Taper." },
  { "en": "Apa itu 'planar lightwave circuit' (PLC)?", "id": "Sirkuit optik yang dibuat di chip." },
  { "en": "Singkatan PLC (Planar Lightwave Circuit)?", "id": "Planar Lightwave Circuit." },
  { "en": "Contoh perangkat PLC (Planar Lightwave Circuit)?", "id": "Splitter PLC." },
  { "en": "Apa itu 'arrayed waveguide grating' (AWG)?", "id": "Komponen MUX/DEMUX untuk DWDM." },
  { "en": "Singkatan AWG (Arrayed Waveguide Grating)?", "id": "Arrayed Waveguide Grating." },
  { "en": "Apa itu 'thin-film filter' (TFF)?", "id": "Filter optik berbasis lapisan tipis." },
  { "en": "Singkatan TFF (Thin-Film Filter)?", "id": "Thin-Film Filter." },
  { "en": "Apa itu 'circulator'?", "id": "Sinonim untuk 'optical circulator'." },
  { "en": "Apa itu 'isolator'?", "id": "Sinonim untuk 'optical isolator'." },
  { "en": "Apa itu 'Optical Transport Network' (OTN)?", "id": "Standar jaringan transport optik modern." },
  { "en": "Singkatan OTN (Optical Transport Network)?", "id": "Optical Transport Network." },
  { "en": "Kelebihan OTN (Optical Transport Network) dibanding SONET/SDH?", "id": "Manajemen FEC dan transparansi." },
  { "en": "Apa itu Ethernet?", "id": "Standar dominan untuk jaringan area lokal (LAN)." },
  { "en": "Standar Ethernet melalui serat optik?", "id": "Misalnya 1000BASE-SX, 10GBASE-LR." },
  { "en": "Kecepatan 'Gigabit Ethernet'?", "id": "1 Gbps." },
  { "en": "Kecepatan 10G Ethernet?", "id": "10 Gbps." },
  { "en": "Apa itu 'Fibre Channel'?", "id": "Protokol jaringan untuk 'storage area network' (SAN)." },
  { "en": "Singkatan SAN (Storage Area Network)?", "id": "Storage Area Network." },
  { "en": "Apa itu 'conduit' atau 'duct'?", "id": "Pipa pelindung untuk kabel serat optik." },
  { "en": "Apa itu 'aerial cable'?", "id": "Kabel udara yang digantung di tiang." },
  { "en": "Apa itu 'direct-buried cable'?", "id": "Kabel yang ditanam langsung di tanah." },
  { "en": "Apa itu 'submarine cable'?", "id": "Kabel komunikasi yang dipasang di bawah laut." },
  { "en": "Tantangan utama kabel bawah laut?", "id": "Tekanan, korosi, dan perbaikan." },
  { "en": "Apa itu 'optical distribution frame' (ODF)?", "id": "Panel terminasi dan distribusi serat." },
  { "en": "Singkatan ODF (Optical Distribution Frame)?", "id": "Optical Distribution Frame." },
  { "en": "Apa itu 'fiber optic splice closure'?", "id": "Wadah pelindung untuk sambungan serat." },
  { "en": "Apa itu 'handhole' atau 'manhole'?", "id": "Akses bawah tanah ke jaringan kabel." },
  { "en": "Apa itu 'bending radius'?", "id": "Radius lekukan minimum yang diizinkan." },
  { "en": "Mengapa 'bending radius' penting?", "id": "Mencegah 'macrobending loss' dan kerusakan." },
  { "en": "Apa itu 'bend-insensitive fiber'?", "id": "Serat yang tahan terhadap lekukan." },
  { "en": "Apa itu 'polarization' (polarisasi)?", "id": "Orientasi dari medan listrik gelombang cahaya." },
  { "en": "Jenis polarisasi?", "id": "Linear, sirkular, eliptis." },
  { "en": "Apa itu 'birefringence'?", "id": "Indeks bias bergantung pada polarisasi." },
  { "en": "Efek 'birefringence' pada serat?", "id": "Menyebabkan 'polarization mode dispersion' (PMD)." },
  { "en": "Apa itu 'polarization controller'?", "id": "Mengubah keadaan polarisasi cahaya." },
  { "en": "Apa itu 'polarizer'?", "id": "Melewatkan hanya satu jenis polarisasi." },
  { "en": "Apa itu 'Faraday effect'?", "id": "Rotasi polarisasi akibat medan magnet." },
  { "en": "Aplikasi 'Faraday effect'?", "id": "Isolator dan sirkulator optik." },
  { "en": "Apa itu 'photonic integrated circuit' (PIC)?", "id": "Sirkuit optik terintegrasi dalam satu chip." },
  { "en": "Singkatan PIC (Photonic Integrated Circuit)?", "id": "Photonic Integrated Circuit." },
  { "en": "Apa itu 'silicon photonics'?", "id": "Teknologi PIC berbasis silikon." },
  { "en": "Keuntungan 'silicon photonics'?", "id": "Menggunakan fabrikasi CMOS yang matang." },
  { "en": "Apa itu 'optical modulator'?", "id": "Mengubah properti cahaya sesuai sinyal." },
  { "en": "Contoh 'optical modulator'?", "id": "Mach-Zehnder modulator, electro-absorption modulator." },
  { "en": "Apa itu 'electro-absorption modulator' (EAM)?", "id": "Modulator berbasis penyerapan terkontrol tegangan." },
  { "en": "Singkatan EAM (Electro-Absorption Modulator)?", "id": "Electro-Absorption Modulator." },
  { "en": "Apa itu 'laser chirp'?", "id": "Sinonim untuk chirp." },
  { "en": "Apa itu 'laser linewidth'?", "id": "Sinonim untuk 'spectral width'." },
  { "en": "Apa itu 'dark fiber'?", "id": "Serat optik yang terpasang tapi belum digunakan." },
  { "en": "Apa itu 'lit fiber'?", "id": "Serat optik yang sudah aktif digunakan." },
  { "en": "Apa itu 'optical power budget'?", "id": "Sinonim untuk 'optical link budget'." },
  { "en": "Apa itu 'system margin'?", "id": "Cadangan daya dalam 'link budget'." },
  { "en": "Tujuan 'system margin'?", "id": "Mengantisipasi degradasi dan perbaikan." },
  { "en": "Apa itu 'optical backplane'?", "id": "Papan sirkuit dengan koneksi optik." },
  { "en": "Apa itu 'optical transceiver'?", "id": "Perangkat yang berfungsi sebagai transmitter dan receiver." },
  { "en": "Contoh 'form factor' transceiver?", "id": "SFP, QSFP, CFP." },
  { "en": "Singkatan SFP (Small Form-factor Pluggable)?", "id": "Small Form-factor Pluggable." },
  { "en": "Singkatan QSFP (Quad Small Form-factor Pluggable)?", "id": "Quad Small Form-factor Pluggable." },
  { "en": "Apa itu 'optical ground wire' (OPGW)?", "id": "Kabel ground pada transmisi listrik." },
  { "en": "Singkatan OPGW (Optical Ground Wire)?", "id": "Optical Ground Wire." },
  { "en": "Fungsi OPGW (Optical Ground Wire)?", "id": "Sebagai kabel ground dan komunikasi." },
  { "en": "Apa itu 'modal bandwidth'?", "id": "Ukuran kapasitas serat multi-mode." },
  { "en": "Satuan 'modal bandwidth'?", "id": "MHz·km." },
  { "en": "Serat OM1, OM2, OM3, OM4, OM5 adalah jenis?", "id": "Serat multi-mode." },
  { "en": "Serat G.652, G.655, G.657 adalah jenis?", "id": "Serat single-mode (standar ITU)." },
  { "en": "Standar G.657 adalah untuk?", "id": "Serat 'bend-insensitive'." },
  { "en": "Apa itu 'mode scrambling'?", "id": "Proses untuk meratakan distribusi daya mode." },
  { "en": "Apa itu 'mode filtering'?", "id": "Proses untuk menghilangkan mode orde tinggi." },
  { "en": "Apa itu 'encircled flux' (EF)?", "id": "Standar untuk kondisi peluncuran cahaya." },
  { "en": "Singkatan EF (Encircled Flux)?", "id": "Encircled Flux." },
  { "en": "Apa itu 'launch condition'?", "id": "Cara cahaya dimasukkan ke dalam serat." },
  { "en": "Mengapa 'launch condition' penting?", "id": "Mempengaruhi hasil pengukuran atenuasi." },
  { "en": "Apa itu 'optical return loss' (ORL)?", "id": "Ukuran total refleksi dari suatu link." },
  { "en": "Singkatan ORL (Optical Return Loss)?", "id": "Optical Return Loss." },
  { "en": "Apa itu 'fusion splicer'?", "id": "Alat untuk melakukan 'fusion splicing'." },
  { "en": "Apa itu 'core alignment'?", "id": "Menyelaraskan inti serat sebelum splicing." },
  { "en": "Apa itu 'cladding alignment'?", "id": "Menyelaraskan cladding serat sebelum splicing." },
  { "en": "Mana yang lebih presisi, 'core' atau 'cladding alignment'?", "id": "Core alignment." },
  { "en": "Apa itu 'splice loss'?", "id": "Atenuasi yang disebabkan oleh sambungan." },
  { "en": "Nilai 'splice loss' yang baik?", "id": "Di bawah 0.1 dB." },
  { "en": "Apa itu 'connector loss'?", "id": "Atenuasi yang disebabkan oleh konektor." },
  { "en": "Nilai 'connector loss' yang baik?", "id": "Di bawah 0.5 dB." },
  { "en": "Apa itu 'Fresnel reflection'?", "id": "Refleksi akibat perubahan indeks bias." },
  { "en": "Di mana 'Fresnel reflection' terjadi?", "id": "Di celah udara antar konektor." },
  { "en": "Jenis polesan konektor?", "id": "PC, UPC, APC." },
  { "en": "Singkatan UPC (Ultra Physical Contact)?", "id": "Ultra Physical Contact." },
  { "en": "Singkatan APC (Angled Physical Contact)?", "id": "Angled Physical Contact." },
  { "en": "Konektor mana yang reflektansinya paling rendah?", "id": "Konektor APC." },
  { "en": "Warna konektor APC (Angled Physical Contact)?", "id": "Hijau." },
  { "en": "Atenuasi terendah serat silika?", "id": "Sekitar 0.2 dB/km di 1550 nm." },
  { "en": "Dispersi kromatik SMF (Single-Mode Fiber) di 1550 nm?", "id": "Sekitar +17 ps/(nm·km)." },
  { "en": "Hubungan atenuasi Rayleigh dan panjang gelombang?", "id": "Berbanding terbalik pangkat empat." },
  { "en": "Mengapa langit berwarna biru?", "id": "Karena hamburan Rayleigh cahaya biru." },
  { "en": "Penggunaan serat optik dalam endoskopi?", "id": "Mengirimkan iluminasi dan gambar." },
  { "en": "Apa itu 'laser surgery'?", "id": "Pembedahan presisi menggunakan energi laser." },
  { "en": "Aplikasi serat optik di industri migas?", "id": "Sensor suhu dan tekanan terdistribusi." },
  { "en": "Singkatan OOK (On-Off Keying)?", "id": "On-Off Keying." },
  { "en": "Apa itu OOK (On-Off Keying)?", "id": "Modulasi digital paling sederhana." },
  { "en": "OOK (On-Off Keying) merepresentasikan bit '1' dengan?", "id": "Adanya pulsa cahaya." },
  { "en": "OOK (On-Off Keying) merepresentasikan bit '0' dengan?", "id": "Tidak adanya pulsa cahaya." },
  { "en": "Singkatan DPSK (Differential Phase-Shift Keying)?", "id": "Differential Phase-Shift Keying." },
  { "en": "Apa itu 'constellation diagram'?", "id": "Visualisasi skema modulasi digital kompleks." },
  { "en": "Apa itu 'live fiber detector'?", "id": "Mendeteksi sinyal tanpa memutus serat." },
  { "en": "Apa itu 'fiber identifier'?", "id": "Mendeteksi sinyal dan arahnya." },
  { "en": "Apa itu 'optical loss test set' (OLTS)?", "id": "Gabungan OLS dan OPM." },
  { "en": "Singkatan OLTS (Optical Loss Test Set)?", "id": "Optical Loss Test Set." },
  { "en": "Singkatan OPM (Optical Power Meter)?", "id": "Optical Power Meter." },
  { "en": "Apa itu 'bare fiber adapter'?", "id": "Menghubungkan serat tanpa konektor ke alat." },
  { "en": "Pembersih konektor menggunakan?", "id": "Alkohol isopropil atau pembersih khusus." },
  { "en": "Mengapa kebersihan konektor penting?", "id": "Kotoran menyebabkan loss dan refleksi." },
  { "en": "Apa itu 'scattering'?", "id": "Sinonim untuk hamburan." },
  { "en": "Apa itu 'absorption'?", "id": "Sinonim untuk penyerapan." },
  { "en": "Puncak penyerapan air (OH-) di serat?", "id": "Sekitar 1383 nm." },
  { "en": "Apa itu 'low-water-peak fiber'?", "id": "Serat dengan penyerapan air rendah." },
  { "en": "Apa itu 'intrinsic loss'?", "id": "Loss yang inheren dari material." },
  { "en": "Contoh 'intrinsic loss'?", "id": "Hamburan Rayleigh dan penyerapan." },
  { "en": "Apa itu 'extrinsic loss'?", "id": "Loss akibat faktor eksternal." },
  { "en": "Contoh 'extrinsic loss'?", "id": "Loss lekukan dan sambungan." },
  { "en": "Apa itu 'opto-isolator'?", "id": "Komponen isolasi sinyal menggunakan cahaya." },
  { "en": "Nama lain 'opto-isolator'?", "id": "Optocoupler." },
  { "en": "Apa itu 'transceiver'?", "id": "Sinonim untuk optical transceiver." },
  { "en": "Fungsi 'transceiver'?", "id": "Transmitter dan receiver dalam satu modul." },
  { "en": "Apa itu 'hot-pluggable'?", "id": "Bisa dipasang/dilepas saat sistem menyala." },
  { "en": "Transceiver SFP (Small Form-factor Pluggable) bersifat?", "id": "Hot-pluggable." },
  { "en": "Apa itu 'digital diagnostics monitoring' (DDM)?", "id": "Fitur pemantauan pada transceiver." },
  { "en": "Singkatan DDM (Digital Diagnostics Monitoring)?", "id": "Digital Diagnostics Monitoring." },
  { "en": "Parameter yang dipantau DDM?", "id": "Daya, suhu, tegangan." },
  { "en": "Apa itu 'avalanche multiplication'?", "id": "Proses gain internal pada APD." },
  { "en": "Apa itu 'bandwidth-distance product'?", "id": "Ukuran performa serat multi-mode." },
  { "en": "Apa itu 'effective area' (Aeff)?", "id": "Area efektif mode pada serat." },
  { "en": "Singkatan Aeff (Effective Area)?", "id": "Effective Area." },
  { "en": "Aeff (Effective Area) besar mengurangi?", "id": "Efek non-linear." },
  { "en": "Apa itu 'four-wave mixing' (FWM)?", "id": "Sinonim untuk FWM." },
  { "en": "Apa itu 'self-phase modulation' (SPM)?", "id": "Sinonim untuk SPM." },
  { "en": "Apa itu 'cross-phase modulation' (XPM)?", "id": "Sinonim untuk XPM." },
  { "en": "Efek non-linear terkuat?", "id": "FWM, SPM, dan XPM." },
  { "en": "Apa itu 'birefringence'?", "id": "Sinonim untuk birefringence." },
  { "en": "Apa itu 'polarization dependent loss' (PDL)?", "id": "Loss yang bergantung pada polarisasi." },
  { "en": "Singkatan PDL (Polarization Dependent Loss)?", "id": "Polarization Dependent Loss." },
  { "en": "Apa itu 'state of polarization' (SOP)?", "id": "Orientasi polarisasi dari gelombang cahaya." },
  { "en": "Singkatan SOP (State of Polarization)?", "id": "State of Polarization." },
  { "en": "Apa itu 'degree of polarization' (DOP)?", "id": "Ukuran seberapa terpolarisasi suatu cahaya." },
  { "en": "Singkatan DOP (Degree of Polarization)?", "id": "Degree of Polarization." },
  { "en": "Apa itu 'Poincaré sphere'?", "id": "Representasi geometris dari SOP." },
  { "en": "Apa itu 'optical repeater'?", "id": "Sinonim untuk regenerator." },
  { "en": "Fungsi 3R pada regenerator?", "id": "Re-amplification, reshaping, retiming." },
  { "en": "Apa itu 'clock recovery'?", "id": "Mengekstrak sinyal clock dari data." },
  { "en": "Apa itu 'decision circuit'?", "id": "Memutuskan apakah bit 0 atau 1." },
  { "en": "Apa itu 'optical time division multiplexing' (OTDM)?", "id": "Multiplexing di domain waktu optik." },
  { "en": "Singkatan OTDM (Optical Time Division Multiplexing)?", "id": "Optical Time Division Multiplexing." },
  { "en": "Apa itu 'optical code division multiple access' (OCDMA)?", "id": "Teknik akses ganda optik." },
  { "en": "Singkatan OCDMA (Optical Code Division Multiple Access)?", "id": "Optical Code Division Multiple Access." },
  { "en": "Apa itu 'soliton'?", "id": "Pulsa optik yang menjaga bentuknya." },
  { "en": "Soliton terbentuk dari keseimbangan antara?", "id": "Dispersi dan efek non-linear SPM." },
  { "en": "Apa itu 'erbium'?", "id": "Elemen tanah jarang untuk doping serat." },
  { "en": "Apa itu 'pump laser'?", "id": "Laser untuk memberi energi pada EDFA." },
  { "en": "Panjang gelombang 'pump laser' EDFA?", "id": "980 nm atau 1480 nm." },
  { "en": "Apa itu 'Raman amplification'?", "id": "Penguatan optik berbasis hamburan Raman." },
  { "en": "Keuntungan 'Raman amplification'?", "id": "Bisa menguatkan di panjang gelombang apapun." },
  { "en": "Apa itu 'semiconductor optical amplifier' (SOA)?", "id": "Penguat optik berbasis semikonduktor." },
  { "en": "Singkatan SOA (Semiconductor Optical Amplifier)?", "id": "Semiconductor Optical Amplifier." },
  { "en": "Apa itu 'fiber optic gyroscope' (FOG)?", "id": "Sinonim untuk FOG." },
  { "en": "Apa itu 'ring laser gyroscope' (RLG)?", "id": "Jenis giroskop optik lain." },
  { "en": "Singkatan RLG (Ring Laser Gyroscope)?", "id": "Ring Laser Gyroscope." },
  { "en": "Apa itu efek Sagnac?", "id": "Prinsip dasar giroskop optik." },
  { "en": "Apa itu 'optical Kerr effect'?", "id": "Perubahan indeks bias akibat intensitas cahaya." },
  { "en": "SPM dan XPM adalah manifestasi dari?", "id": "Optical Kerr effect." },
  { "en": "Apa itu 'optical bistability'?", "id": "Perangkat dengan dua state transmisi stabil." },
  { "en": "Apa itu 'photonic switching'?", "id": "Switching sinyal dalam bentuk optik." },
  { "en": "Apa itu 'all-optical network'?", "id": "Jaringan yang sinyalnya tetap optik." },
  { "en": "Apa itu 'optoelectronics'?", "id": "Bidang studi perangkat elektronik-optik." },
  { "en": "Laser adalah singkatan dari?", "id": "Light Amplification by Stimulated Emission." },
  { "en": "Dioda pemancar cahaya?", "id": "LED (Light Emitting Diode)." },
  { "en": "Dioda pendeteksi cahaya?", "id": "Fotodioda." },
  { "en": "Lapisan penyangga pada kabel?", "id": "Buffer." },
  { "en": "Jaket terluar kabel disebut?", "id": "Jacket atau sheath." },
  { "en": "Apa itu 'breakout cable'?", "id": "Kabel dengan beberapa serat individual." },
  { "en": "Apa itu 'distribution cable'?", "id": "Kabel indoor dengan banyak serat." },
  { "en": "Apa itu 'plenum cable'?", "id": "Kabel dengan rating api untuk ruang plenum." },
  { "en": "Apa itu 'riser cable'?", "id": "Kabel untuk instalasi vertikal antar lantai." },
  { "en": "Apa itu 'armored cable'?", "id": "Kabel dengan lapisan pelindung logam." },
  { "en": "Tujuan 'armored cable'?", "id": "Melindungi dari gigitan hewan atau benturan." },
  { "en": "Konektor 'push-pull' yang umum?", "id": "Konektor SC." },
  { "en": "Konektor 'bayonet' yang umum?", "id": "Konektor ST." },
  { "en": "Konektor 'small form factor'?", "id": "Konektor LC." },
  { "en": "Apa itu 'phase velocity'?", "id": "Kecepatan perambatan fasa gelombang." },
  { "en": "Apa itu 'group velocity'?", "id": "Kecepatan perambatan amplop/pulsa gelombang." },
  { "en": "Dalam transmisi data, mana yang relevan?", "id": "Group velocity." },
  { "en": "Apa itu 'group velocity dispersion' (GVD)?", "id": "Penyebab utama pelebaran pulsa." },
  { "en": "Singkatan GVD (Group Velocity Dispersion)?", "id": "Group Velocity Dispersion." },
  { "en": "GVD (Group Velocity Dispersion) positif berarti?", "id": "Frekuensi tinggi lebih lambat." },
  { "en": "GVD (Group Velocity Dispersion) negatif berarti?", "id": "Frekuensi rendah lebih lambat." },
  { "en": "Apa itu 'quantum communication'?", "id": "Komunikasi menggunakan prinsip mekanika kuantum." },
  { "en": "Apa itu 'quantum key distribution' (QKD)?", "id": "Metode aman berbagi kunci enkripsi." },
  { "en": "Singkatan QKD (Quantum Key Distribution)?", "id": "Quantum Key Distribution." },
  { "en": "Prinsip dasar QKD (Quantum Key Distribution)?", "id": "Pengamatan mengubah keadaan kuantum." },
  { "en": "Apa itu 'single-photon source'?", "id": "Sumber yang memancarkan satu foton." },
  { "en": "Apa itu 'single-photon detector'?", "id": "Detektor yang bisa mendeteksi satu foton." },
  { "en": "Apa itu 'quantum entanglement'?", "id": "Keterikatan antara dua partikel kuantum." },
  { "en": "Apa itu 'network topology'?", "id": "Peta logis atau fisik dari jaringan." },
  { "en": "Contoh 'network topology'?", "id": "Star, bus, ring, mesh." },
  { "en": "Topologi paling andal?", "id": "Mesh." },
  { "en": "Apa itu 'latency'?", "id": "Waktu tunda transmisi sinyal." },
  { "en": "Penyebab utama 'latency'?", "id": "Kecepatan cahaya dan jarak." },
  { "en": "Apa itu 'jitter'?", "id": "Variasi pada waktu tunda sinyal." },
  { "en": "Apa itu 'last mile' telekomunikasi?", "id": "Segmen akhir jaringan ke pelanggan." },
  { "en": "FTTH (Fiber to the Home) adalah solusi untuk?", "id": "Masalah 'last mile'." },
  { "en": "Apa itu 'backbone network'?", "id": "Jaringan inti berkecepatan sangat tinggi." },
  { "en": "Kabel 'backbone network' biasanya?", "id": "Serat optik bawah laut atau darat." },
  { "en": "Siapa yang mengatur standar internet?", "id": "IETF (Internet Engineering Task Force)." },
  { "en": "Singkatan IETF (Internet Engineering Task Force)?", "id": "Internet Engineering Task Force." },
  { "en": "Apa itu 'Request for Comments' (RFC)?", "id": "Publikasi dari IETF." },
  { "en": "Singkatan RFC (Request for Comments)?", "id": "Request for Comments." },
  { "en": "Apa itu 'optical fiber connector'?", "id": "Sinonim konektor serat optik." },
  { "en": "Apa itu 'optical fiber splice'?", "id": "Sinonim sambungan serat optik." },
  { "en": "Apa itu 'optical fiber cable'?", "id": "Sinonim kabel serat optik." },
  { "en": "Apa itu 'optical amplifier'?", "id": "Sinonim penguat optik." },
  { "en": "Apa itu 'optical receiver'?", "id": "Sinonim penerima optik." },
  { "en": "Apa itu 'optical transmitter'?", "id": "Sinonim pemancar optik." },
  { "en": "Apa itu 'photodetector'?", "id": "Sinonim fotodetektor." },
  { "en": "Apa itu 'light source'?", "id": "Sinonim sumber cahaya." },
  { "en": "Apa itu 'laser diode'?", "id": "Sinonim dioda laser." },
  { "en": "Material serat optik plastik?", "id": "PMMA." },
  { "en": "Singkatan PMMA (Poly(methyl methacrylate))?", "id": "Poly(methyl methacrylate)." },
  { "en": "Kelebihan serat plastik?", "id": "Fleksibel dan murah." },
  { "en": "Kelemahan serat plastik?", "id": "Atenuasi sangat tinggi." },
  { "en": "Aplikasi serat plastik?", "id": "Jarak sangat pendek (audio mobil)." },
  { "en": "Apa itu 'mode stripper'?", "id": "Menghilangkan 'cladding mode'." },
  { "en": "Apa itu 'cladding mode'?", "id": "Cahaya yang merambat di dalam 'cladding'." },
  { "en": "Apa itu 'acceptance cone'?", "id": "Kerucut imajiner sudut penerimaan." },
  { "en": "Apa itu 'numerical aperture'?", "id": "Sinonim untuk 'numerical aperture' (NA)." },
  { "en": "NA (Numerical Aperture) serat 'single-mode'?", "id": "Sangat kecil (sekitar 0.14)." },
  { "en": "NA (Numerical Aperture) serat 'multi-mode'?", "id": "Lebih besar (di atas 0.20)." },
  { "en": "Apa itu 'bandwidth-limited' link?", "id": "Link yang dibatasi oleh dispersi." },
  { "en": "Apa itu 'power-limited' link?", "id": "Link yang dibatasi oleh atenuasi." },
  { "en": "Apa itu 'end face' konektor?", "id": "Permukaan ujung dari ferrule." },
  { "en": "Kualitas 'end face' penting untuk?", "id": "Mengurangi loss dan refleksi." },
  { "en": "Inspeksi 'end face' menggunakan?", "id": "Fiber microscope." },
  { "en": "Apa itu 'connector key'?", "id": "Tonjolan untuk memastikan orientasi benar." },
  { "en": "Apa itu 'adapter' atau 'coupler'?", "id": "Menghubungkan dua konektor." },
  { "en": "Apa itu 'hybrid adapter'?", "id": "Menghubungkan dua tipe konektor berbeda." },
  { "en": "Apa itu 'attenuator'?", "id": "Sinonim untuk 'optical attenuator'." },
  { "en": "Jenis 'attenuator'?", "id": "Tetap dan variabel." },
  { "en": "Tujuan menggunakan 'attenuator'?", "id": "Mencegah 'overload' pada penerima." },
  { "en": "Apa itu 'optical overload'?", "id": "Daya optik terlalu tinggi untuk penerima." },
  { "en": "Apa itu 'dynamic range' penerima?", "id": "Rentang daya antara sensitivitas dan overload." },
  { "en": "Apa itu 'receiver sensitivity'?", "id": "Daya optik minimum untuk BER tertentu." },
  { "en": "Apa itu 'transimpedance amplifier' (TIA)?", "id": "Penguat depan pada penerima optik." },
  { "en": "Singkatan TIA (Transimpedance Amplifier)?", "id": "Transimpedance Amplifier." },
  { "en": "Fungsi TIA (Transimpedance Amplifier)?", "id": "Mengubah arus fotodioda menjadi tegangan." },
  { "en": "Apa itu 'limiting amplifier'?", "id": "Penguat yang menghasilkan level logika tetap." },
  { "en": "Apa itu 'clock and data recovery' (CDR)?", "id": "Sirkuit untuk mengekstrak data dan clock." },
  { "en": "Singkatan CDR (Clock and Data Recovery)?", "id": "Clock and Data Recovery." },
  { "en": "Apa itu 'fiber optic cable plant'?", "id": "Infrastruktur fisik kabel optik." },
  { "en": "Apa itu 'optical link'?", "id": "Jalur transmisi optik dari Tx ke Rx." },
  { "en": "Singkatan Tx (Transmitter)?", "id": "Transmitter." },
  { "en": "Singkatan Rx (Receiver)?", "id": "Receiver." },
  { "en": "Apa itu 'simplex'?", "id": "Komunikasi satu arah." },
  { "en": "Apa itu 'duplex'?", "id": "Komunikasi dua arah." },
  { "en": "Apa itu 'half-duplex'?", "id": "Dua arah secara bergantian." },
  { "en": "Apa itu 'full-duplex'?", "id": "Dua arah secara bersamaan." },
  { "en": "Komunikasi serat optik biasanya?", "id": "Full-duplex (menggunakan dua serat)." },
  { "en": "Apa itu 'bidirectional' (BiDi) transceiver?", "id": "Transceiver yang menggunakan satu serat." },
  { "en": "Bagaimana 'BiDi' transceiver bekerja?", "id": "Menggunakan dua panjang gelombang berbeda." },
  { "en": "Apa itu 'aerial drop cable'?", "id": "Kabel terakhir dari tiang ke rumah." },
  { "en": "Apa itu 'splice tray'?", "id": "Wadah untuk menata sambungan serat." },
  { "en": "Apa itu 'fiber distribution hub' (FDH)?", "id": "Titik distribusi di jaringan akses." },
  { "en": "Singkatan FDH (Fiber Distribution Hub)?", "id": "Fiber Distribution Hub." },
  { "en": "Apa itu 'optical network'?", "id": "Jaringan yang menggunakan serat optik." },
  { "en": "Apa itu 'photonic'?", "id": "Berkaitan dengan foton atau cahaya." },
  { "en": "Apa itu 'electro-optic'?", "id": "Berkaitan dengan interaksi listrik-cahaya." },
  { "en": "Apa itu 'acousto-optic'?", "id": "Berkaitan dengan interaksi suara-cahaya." },
  { "en": "Apa itu 'magneto-optic'?", "id": "Berkaitan dengan interaksi magnet-cahaya." },
  { "en": "Apa itu 'nonlinear optics'?", "id": "Studi tentang interaksi cahaya intens." },
  { "en": "Apa itu 'laser noise'?", "id": "Fluktuasi pada output laser." },
  { "en": "Apa itu 'mode hopping'?", "id": "Laser melompat antar mode longitudinal." },
  { "en": "Apa itu 'mode partition noise'?", "id": "Noise pada laser Fabry-Perot." },
  { "en": "Apa itu 'detector noise'?", "id": "Noise yang dihasilkan oleh fotodetektor." },
  { "en": "Apa itu 'amplifier noise'?", "id": "Noise yang ditambahkan oleh penguat listrik." },
  { "en": "Apa itu 'Rayleigh scattering'?", "id": "Sinonim untuk hamburan Rayleigh." },
  { "en": "Apa itu 'Mie scattering'?", "id": "Hamburan oleh partikel seukuran panjang gelombang." },
  { "en": "Apa itu 'optical channel'?", "id": "Satu jalur panjang gelombang dalam WDM." },
  { "en": "Apa itu 'channel spacing'?", "id": "Jarak antar kanal dalam sistem WDM." },
  { "en": "Satuan 'channel spacing'?", "id": "Gigahertz (GHz) atau nanometer (nm)." },
  { "en": "Channel spacing pada DWDM (Dense Wavelength Division Multiplexing)?", "id": "Sangat rapat (misal: 50 atau 100 GHz)." },
  { "en": "Channel spacing pada CWDM (Coarse Wavelength Division Multiplexing)?", "id": "Lebar (misal: 20 nm)." },
  { "en": "Apa itu 'crosstalk' dalam WDM?", "id": "Interferensi antara kanal yang berdekatan." },
  { "en": "Apa itu 'optical filter'?", "id": "Melewatkan panjang gelombang tertentu." },
  { "en": "Aplikasi 'optical filter'?", "id": "Demultiplexer, OADM, penguat optik." },
  { "en": "Apa itu 'bandpass filter'?", "id": "Filter yang melewatkan satu pita frekuensi." },
  { "en": "Apa itu 'edge filter'?", "id": "Melewatkan panjang gelombang di atas/bawah batas." },
  { "en": "Apa itu 'gain flattening filter' (GFF)?", "id": "Filter untuk meratakan gain EDFA." },
  { "en": "Singkatan GFF (Gain Flattening Filter)?", "id": "Gain Flattening Filter." },
  { "en": "Mengapa gain EDFA (Erbium-Doped Fiber Amplifier) perlu diratakan?", "id": "Agar semua kanal WDM dikuatkan merata." },
  { "en": "Apa itu 'optical performance monitoring' (OPM)?", "id": "Pemantauan kualitas sinyal optik." },
  { "en": "Singkatan OPM (Optical Performance Monitoring)?", "id": "Optical Performance Monitoring." },
  { "en": "Parameter yang dipantau OPM?", "id": "OSNR, daya kanal, dispersi." },
  { "en": "Apa itu 'digital signal processing' (DSP)?", "id": "Pemrosesan sinyal menggunakan algoritma digital." },
  { "en": "Singkatan DSP (Digital Signal Processing)?", "id": "Digital Signal Processing." },
  { "en": "Peran DSP (Digital Signal Processing) dalam komunikasi optik?", "id": "Kompensasi dispersi, modulasi koheren." },
  { "en": "Apa itu 'electronic dispersion compensation' (EDC)?", "id": "Kompensasi dispersi menggunakan sirkuit elektronik." },
  { "en": "Singkatan EDC (Electronic Dispersion Compensation)?", "id": "Electronic Dispersion Compensation." },
  { "en": "Apa itu 'submarine line terminal equipment' (SLTE)?", "id": "Perangkat di ujung kabel bawah laut." },
  { "en": "Singkatan SLTE (Submarine Line Terminal Equipment)?", "id": "Submarine Line Terminal Equipment." },
  { "en": "Apa itu 'branching unit'?", "id": "Titik percabangan pada kabel bawah laut." },
  { "en": "Daya untuk 'repeater' bawah laut disuplai dari?", "id": "Daratan melalui kabel itu sendiri." },
  { "en": "Tantangan utama desain 'repeater' bawah laut?", "id": "Keandalan sangat tinggi, konsumsi daya rendah." },
  { "en": "Apa itu 'lightwave'?", "id": "Sinonim untuk gelombang cahaya." },
  { "en": "Apa itu 'photonics'?", "id": "Ilmu dan teknologi foton." },
  { "en": "Apa itu foton?", "id": "Partikel kuantum dari cahaya." },
  { "en": "Energi foton sebanding dengan?", "id": "Frekuensinya." },
  { "en": "Apa itu konstanta Planck (h)?", "id": "Konstanta fundamental dalam mekanika kuantum." },
  { "en": "Apa itu 'optical cavity' atau 'resonator'?", "id": "Struktur yang mengurung cahaya." },
  { "en": "Komponen utama laser?", "id": "Medium gain, sumber energi, optical cavity." },
  { "en": "Apa itu 'gain medium'?", "id": "Material yang bisa menguatkan cahaya." },
  { "en": "Apa itu 'optical pumping'?", "id": "Proses memberi energi ke medium gain." },
  { "en": "Apa itu 'laser threshold'?", "id": "Energi pompa minimum untuk lasing." },
  { "en": "Apa itu 'laser beam divergence'?", "id": "Ukuran penyebaran berkas laser." },
  { "en": "Apa itu 'laser mode'?", "id": "Pola medan elektromagnetik dalam laser." },
  { "en": "Jenis 'laser mode'?", "id": "Transversal dan longitudinal." },
  { "en": "Mode transversal (TEM) berhubungan dengan?", "id": "Profil intensitas penampang berkas." },
  { "en": "Singkatan TEM (Transverse Electromagnetic Mode)?", "id": "Transverse Electromagnetic Mode." },
  { "en": "Mode longitudinal berhubungan dengan?", "id": "Frekuensi resonansi di dalam cavity." },
  { "en": "Laser 'single-mode' berarti?", "id": "Hanya berosilasi pada satu mode transversal." },
  { "en": "Laser 'single-frequency' berarti?", "id": "Hanya berosilasi pada satu mode longitudinal." },
  { "en": "Laser DFB (Distributed Feedback) adalah laser?", "id": "Single-frequency." },
  { "en": "Apa itu 'mode-locking'?", "id": "Teknik untuk menghasilkan pulsa sangat pendek." },
  { "en": "Apa itu 'Q-switching'?", "id": "Teknik untuk menghasilkan pulsa energi tinggi." },
  { "en": "Apa itu 'optical fiber connector cleaner'?", "id": "Alat pembersih ujung konektor." },
  { "en": "Jenis pembersih konektor?", "id": "One-click cleaner, tisu, stik." },
  { "en": "Apa itu 'fusion splicer'?", "id": "Sinonim untuk fusion splicer." },
  { "en": "Apa itu 'fiber cleaver'?", "id": "Sinonim untuk fiber cleaver." },
  { "en": "Apa itu 'fiber stripper'?", "id": "Sinonim untuk fiber stripper." },
  { "en": "Apa itu 'link restoration'?", "id": "Proses memulihkan link yang putus." },
  { "en": "Apa itu 'protection switching'?", "id": "Beralih ke jalur cadangan secara otomatis." },
  { "en": "Waktu 'protection switching' yang umum?", "id": "Kurang dari 50 milidetik." },
  { "en": "Apa itu 'fault detection'?", "id": "Proses mendeteksi adanya gangguan." },
  { "en": "Apa itu 'fault localization'?", "id": "Proses menemukan lokasi gangguan." },
  { "en": "Instrumen untuk 'fault localization'?", "id": "OTDR." },
  { "en": "Apa itu 'optical cross-connect' (OXC)?", "id": "Switch matriks untuk sinyal optik." },
  { "en": "Singkatan OXC (Optical Cross-Connect)?", "id": "Optical Cross-Connect." },
  { "en": "Apa itu 'wavelength-selective switch' (WSS)?", "id": "Switch optik yang bisa dialihkan per panjang gelombang." },
  { "en": "Singkatan WSS (Wavelength-Selective Switch)?", "id": "Wavelength-Selective Switch." },
  { "en": "Apa itu 'reconfigurable OADM' (ROADM)?", "id": "OADM yang bisa dikonfigurasi dari jarak jauh." },
  { "en": "Singkatan ROADM (Reconfigurable Optical Add-Drop Multiplexer)?", "id": "Reconfigurable Optical Add-Drop Multiplexer." },
  { "en": "Apa itu 'attenuation coefficient'?", "id": "Parameter yang mendeskripsikan atenuasi." },
  { "en": "Simbol 'attenuation coefficient'?", "id": "Alpha (α)." },
  { "en": "Apa itu 'absorption coefficient'?", "id": "Parameter untuk penyerapan." },
  { "en": "Apa itu 'scattering coefficient'?", "id": "Parameter untuk hamburan." },
  { "en": "Apa itu 'connectorization'?", "id": "Proses memasang konektor pada serat." },
  { "en": "Apa itu 'cable pulling'?", "id": "Proses menarik kabel optik." },
  { "en": "Apa itu 'air-blowing'?", "id": "Metode instalasi kabel menggunakan udara." },
  { "en": "Apa itu 'figure-8' cable?", "id": "Kabel udara dengan kawat penyangga." },
  { "en": "Apa itu 'all-dielectric self-supporting' (ADSS) cable?", "id": "Kabel udara tanpa elemen logam." },
  { "en": "Singkatan ADSS (All-Dielectric Self-Supporting)?", "id": "All-Dielectric Self-Supporting." },
  { "en": "Apa itu 'optical fiber preform'?", "id": "Sinonim untuk preform." },
  { "en": "Apa itu 'sintering'?", "id": "Proses pemadatan material bubuk." },
  { "en": "Apa itu 'vitreous silica'?", "id": "Kaca silika amorf (non-kristal)." },
  { "en": "Serat optik terbuat dari?", "id": "Vitreous silica." },
  { "en": "Apa itu 'dopant'?", "id": "Atom pengotor untuk mengubah indeks bias." },
  { "en": "Dopan untuk menaikkan indeks bias?", "id": "Germanium (Ge)." },
  { "en": "Dopan untuk menurunkan indeks bias?", "id": "Fluorine (F) atau Boron (B)." },
  { "en": "Apa itu 'cladding diameter'?", "id": "Diameter cladding." },
  { "en": "Ukuran standar 'cladding diameter'?", "id": "125 mikrometer." },
  { "en": "Apa itu 'coating diameter'?", "id": "Diameter serat setelah dilapisi coating." },
  { "en": "Ukuran standar 'coating diameter'?", "id": "250 atau 900 mikrometer." },
  { "en": "Apa itu 'proof testing'?", "id": "Tes kekuatan tarik pada serat." },
  { "en": "Apa itu 'optical fiber amplifier'?", "id": "Sinonim untuk penguat optik." },
  { "en": "Apa itu 'rare-earth-doped fiber'?", "id": "Serat yang didoping elemen tanah jarang." },
  { "en": "Contoh elemen 'rare-earth'?", "id": "Erbium (Er), Ytterbium (Yb)." },
  { "en": "EDFA (Erbium-Doped Fiber Amplifier) menggunakan?", "id": "Serat yang didoping Erbium." },
  { "en": "Apa itu 'gain spectrum'?", "id": "Rentang panjang gelombang penguatan." },
  { "en": "Apa itu 'noise figure' (NF)?", "id": "Ukuran noise yang ditambahkan penguat." },
  { "en": "Singkatan NF (Noise Figure)?", "id": "Noise Figure." },
  { "en": "NF (Noise Figure) penguat ideal?", "id": "3 dB (quantum limit)." },
  { "en": "Apa itu 'saturation power'?", "id": "Daya output di mana gain turun." },
  { "en": "Apa itu 'phase velocity'?", "id": "Kecepatan perambatan fasa gelombang." },
  { "en": "Apa itu 'group velocity'?", "id": "Kecepatan perambatan amplop/pulsa gelombang." },
  { "en": "Dalam transmisi data, mana yang relevan?", "id": "Group velocity." },
  { "en": "Apa itu 'group velocity dispersion' (GVD)?", "id": "Penyebab utama pelebaran pulsa." },
  { "en": "Singkatan GVD (Group Velocity Dispersion)?", "id": "Group Velocity Dispersion." },
  { "en": "GVD (Group Velocity Dispersion) positif berarti?", "id": "Frekuensi tinggi lebih lambat." },
  { "en": "GVD (Group Velocity Dispersion) negatif berarti?", "id": "Frekuensi rendah lebih lambat." },
  { "en": "Apa itu 'quantum communication'?", "id": "Komunikasi menggunakan prinsip mekanika kuantum." },
  { "en": "Apa itu 'quantum key distribution' (QKD)?", "id": "Metode aman berbagi kunci enkripsi." },
  { "en": "Singkatan QKD (Quantum Key Distribution)?", "id": "Quantum Key Distribution." },
  { "en": "Prinsip dasar QKD (Quantum Key Distribution)?", "id": "Pengamatan mengubah keadaan kuantum." },
  { "en": "Apa itu 'single-photon source'?", "id": "Sumber yang memancarkan satu foton." },
  { "en": "Apa itu 'single-photon detector'?", "id": "Detektor yang bisa mendeteksi satu foton." },
  { "en": "Apa itu 'quantum entanglement'?", "id": "Keterikatan antara dua partikel kuantum." },
  { "en": "Apa itu 'network topology'?", "id": "Peta logis atau fisik dari jaringan." },
  { "en": "Contoh 'network topology'?", "id": "Star, bus, ring, mesh." },
  { "en": "Topologi paling andal?", "id": "Mesh." },
  { "en": "Apa itu 'latency'?", "id": "Waktu tunda transmisi sinyal." },
  { "en": "Penyebab utama 'latency'?", "id": "Kecepatan cahaya dan jarak." },
  { "en": "Apa itu 'jitter'?", "id": "Variasi pada waktu tunda sinyal." },
  { "en": "Apa itu 'last mile' telekomunikasi?", "id": "Segmen akhir jaringan ke pelanggan." },
  { "en": "FTTH (Fiber to the Home) adalah solusi untuk?", "id": "Masalah 'last mile'." },
  { "en": "Apa itu 'backbone network'?", "id": "Jaringan inti berkecepatan sangat tinggi." },
  { "en": "Kabel 'backbone network' biasanya?", "id": "Serat optik bawah laut atau darat." },
  { "en": "Siapa yang mengatur standar internet?", "id": "IETF (Internet Engineering Task Force)." },
  { "en": "Singkatan IETF (Internet Engineering Task Force)?", "id": "Internet Engineering Task Force." },
  { "en": "Apa itu 'Request for Comments' (RFC)?", "id": "Publikasi dari IETF." },
  { "en": "Singkatan RFC (Request for Comments)?", "id": "Request for Comments." },
  { "en": "Apa itu 'optical fiber connector'?", "id": "Sinonim konektor serat optik." },
  { "en": "Apa itu 'optical fiber splice'?", "id": "Sinonim sambungan serat optik." },
  { "en": "Apa itu 'optical fiber cable'?", "id": "Sinonim kabel serat optik." },
  { "en": "Apa itu 'optical amplifier'?", "id": "Sinonim penguat optik." },
  { "en": "Apa itu 'optical receiver'?", "id": "Sinonim penerima optik." },
  { "en": "Apa itu 'optical transmitter'?", "id": "Sinonim pemancar optik." },
  { "en": "Apa itu 'photodetector'?", "id": "Sinonim fotodetektor." },
  { "en": "Apa itu 'light source'?", "id": "Sinonim sumber cahaya." },
  { "en": "Apa itu 'laser diode'?", "id": "Sinonim dioda laser." },
  { "en": "Material serat optik plastik?", "id": "PMMA." },
  { "en": "Singkatan PMMA (Poly(methyl methacrylate))?", "id": "Poly(methyl methacrylate)." },
  { "en": "Kelebihan serat plastik?", "id": "Fleksibel dan murah." },
  { "en": "Kelemahan serat plastik?", "id": "Atenuasi sangat tinggi." },
  { "en": "Aplikasi serat plastik?", "id": "Jarak sangat pendek (audio mobil)." },
  { "en": "Apa itu 'mode stripper'?", "id": "Menghilangkan 'cladding mode'." },
  { "en": "Apa itu 'cladding mode'?", "id": "Cahaya yang merambat di dalam 'cladding'." },
  { "en": "Apa itu 'acceptance cone'?", "id": "Kerucut imajiner sudut penerimaan." },
  { "en": "Apa itu 'numerical aperture'?", "id": "Sinonim untuk 'numerical aperture' (NA)." },
  { "en": "NA (Numerical Aperture) serat 'single-mode'?", "id": "Sangat kecil (sekitar 0.14)." },
  { "en": "NA (Numerical Aperture) serat 'multi-mode'?", "id": "Lebih besar (di atas 0.20)." },
  { "en": "Apa itu 'bandwidth-limited' link?", "id": "Link yang dibatasi oleh dispersi." },
  { "en": "Apa itu 'power-limited' link?", "id": "Link yang dibatasi oleh atenuasi." },
  { "en": "Apa itu 'end face' konektor?", "id": "Permukaan ujung dari ferrule." },
  { "en": "Kualitas 'end face' penting untuk?", "id": "Mengurangi loss dan refleksi." },
  { "en": "Inspeksi 'end face' menggunakan?", "id": "Fiber microscope." },
  { "en": "Apa itu 'connector key'?", "id": "Tonjolan untuk memastikan orientasi benar." },
  { "en": "Apa itu 'adapter' atau 'coupler'?", "id": "Menghubungkan dua konektor." },
  { "en": "Apa itu 'hybrid adapter'?", "id": "Menghubungkan dua tipe konektor berbeda." },
  { "en": "Apa itu 'attenuator'?", "id": "Sinonim untuk 'optical attenuator'." },
  { "en": "Jenis 'attenuator'?", "id": "Tetap dan variabel." },
  { "en": "Tujuan menggunakan 'attenuator'?", "id": "Mencegah 'overload' pada penerima." },
  { "en": "Apa itu 'optical overload'?", "id": "Daya optik terlalu tinggi untuk penerima." },
  { "en": "Apa itu 'dynamic range' penerima?", "id": "Rentang daya antara sensitivitas dan overload." },
  { "en": "Apa itu 'receiver sensitivity'?", "id": "Daya optik minimum untuk BER tertentu." },
  { "en": "Apa itu 'transimpedance amplifier' (TIA)?", "id": "Penguat depan pada penerima optik." },
  { "en": "Singkatan TIA (Transimpedance Amplifier)?", "id": "Transimpedance Amplifier." },
  { "en": "Fungsi TIA (Transimpedance Amplifier)?", "id": "Mengubah arus fotodioda menjadi tegangan." },
  { "en": "Apa itu 'limiting amplifier'?", "id": "Penguat yang menghasilkan level logika tetap." },
  { "en": "Apa itu 'clock and data recovery' (CDR)?", "id": "Sirkuit untuk mengekstrak data dan clock." },
  { "en": "Singkatan CDR (Clock and Data Recovery)?", "id": "Clock and Data Recovery." },
  { "en": "Apa itu 'fiber optic cable plant'?", "id": "Infrastruktur fisik kabel optik." },
  { "en": "Apa itu 'optical link'?", "id": "Jalur transmisi optik dari Tx ke Rx." },
  { "en": "Singkatan Tx (Transmitter)?", "id": "Transmitter." },
  { "en": "Singkatan Rx (Receiver)?", "id": "Receiver." },
  { "en": "Apa itu 'simplex'?", "id": "Komunikasi satu arah." },
  { "en": "Apa itu 'duplex'?", "id": "Komunikasi dua arah." },
  { "en": "Apa itu 'half-duplex'?", "id": "Dua arah secara bergantian." },
  { "en": "Apa itu 'full-duplex'?", "id": "Dua arah secara bersamaan." },
  { "en": "Komunikasi serat optik biasanya?", "id": "Full-duplex (menggunakan dua serat)." },
  { "en": "Apa itu 'bidirectional' (BiDi) transceiver?", "id": "Transceiver yang menggunakan satu serat." },
  { "en": "Bagaimana 'BiDi' transceiver bekerja?", "id": "Menggunakan dua panjang gelombang berbeda." },
  { "en": "Apa itu 'aerial drop cable'?", "id": "Kabel terakhir dari tiang ke rumah." },
  { "en": "Apa itu 'splice tray'?", "id": "Wadah untuk menata sambungan serat." },
  { "en": "Apa itu 'fiber distribution hub' (FDH)?", "id": "Titik distribusi di jaringan akses." },
  { "en": "Singkatan FDH (Fiber Distribution Hub)?", "id": "Fiber Distribution Hub." },
  { "en": "Apa itu 'optical network'?", "id": "Jaringan yang menggunakan serat optik." },
  { "en": "Apa itu 'photonic'?", "id": "Berkaitan dengan foton atau cahaya." },
  { "en": "Apa itu 'electro-optic'?", "id": "Berkaitan dengan interaksi listrik-cahaya." },
  { "en": "Apa itu 'acousto-optic'?", "id": "Berkaitan dengan interaksi suara-cahaya." },
  { "en": "Apa itu 'magneto-optic'?", "id": "Berkaitan dengan interaksi magnet-cahaya." },
  { "en": "Apa itu 'nonlinear optics'?", "id": "Studi tentang interaksi cahaya intens." },
  { "en": "Apa itu 'laser noise'?", "id": "Fluktuasi pada output laser." },
  { "en": "Apa itu 'mode hopping'?", "id": "Laser melompat antar mode longitudinal." },
  { "en": "Apa itu 'mode partition noise'?", "id": "Noise pada laser Fabry-Perot." },
  { "en": "Apa itu 'detector noise'?", "id": "Noise yang dihasilkan oleh fotodetektor." },
  { "en": "Apa itu 'amplifier noise'?", "id": "Noise yang ditambahkan oleh penguat listrik." },
  { "en": "Apa itu 'Rayleigh scattering'?", "id": "Sinonim untuk hamburan Rayleigh." },
  { "en": "Apa itu 'Mie scattering'?", "id": "Hamburan oleh partikel seukuran panjang gelombang." },
  { "en": "Apa lawan dari atenuasi?", "id": "Penguatan (gain)." },
  { "en": "Apa lawan dari dispersi?", "id": "Kompensasi dispersi." },
  { "en": "Mengapa konektor APC (Angled Physical Contact) berwarna hijau?", "id": "Membedakannya dari konektor biru UPC." },
  { "en": "Trade-off utama laser vs. LED?", "id": "Performa dan kecepatan vs. biaya." },
  { "en": "BER (Bit Error Rate) yang umum untuk telekomunikasi?", "id": "10 pangkat -9 atau lebih baik." },
  { "en": "Jarak kanal standar ITU (International Telecommunication Union) DWDM?", "id": "100 GHz atau 50 GHz." },
  { "en": "Apa itu 'mode-locked laser'?", "id": "Laser penghasil pulsa sangat pendek." },
  { "en": "Apa itu 'tunable laser'?", "id": "Laser yang panjang gelombangnya bisa diubah." },
  { "en": "Apa itu 'optical power equalizer'?", "id": "Meratakan daya antar kanal WDM." },
  { "en": "Mengapa OTDR (Optical Time-Domain Reflectometer) tak bisa melihat ujung serat?", "id": "Karena 'end zone' atau 'dead zone' akhir." },
  { "en": "Apa itu 'ghost' pada OTDR (Optical Time-Domain Reflectometer)?", "id": "Gema atau refleksi palsu." },
  { "en": "Penyebab umum loss tinggi pada konektor?", "id": "Kotoran, celah udara, atau misalignment." },
  { "en": "Bahaya utama bekerja dengan serat optik?", "id": "Potongan serat kecil dan tajam." },
  { "en": "Bahaya laser Kelas 3B atau 4?", "id": "Bisa merusak mata secara permanen." },
  { "en": "Apa itu 'laser safety glasses'?", "id": "Kacamata pelindung dari radiasi laser." },
  { "en": "Apa itu 'optical power'?", "id": "Daya yang dibawa oleh sinyal cahaya." },
  { "en": "Apa itu 'average power'?", "id": "Daya rata-rata dari sinyal optik." },
  { "en": "Apa itu 'peak power'?", "id": "Daya maksimum dari pulsa optik." },
  { "en": "Apa itu 'duty cycle'?", "id": "Rasio antara lebar pulsa dan periode." },
  { "en": "Apa itu 'optical frequency'?", "id": "Frekuensi dari gelombang cahaya." },
  { "en": "Satuan 'optical frequency'?", "id": "Terahertz (THz)." },
  { "en": "Hubungan panjang gelombang dan frekuensi?", "id": "Berbanding terbalik (c = λf)." },
  { "en": "Apa itu 'linewidth' laser?", "id": "Sinonim untuk 'spectral width'." },
  { "en": "Laser 'single longitudinal mode' (SLM)?", "id": "Laser dengan linewidth sangat sempit." },
  { "en": "Singkatan SLM (Single Longitudinal Mode)?", "id": "Single Longitudinal Mode." },
  { "en": "Apa itu 'chirp'?", "id": "Perubahan frekuensi sesaat selama modulasi." },
  { "en": "Apa itu 'adiabatic chirp'?", "id": "Chirp yang berhubungan dengan daya." },
  { "en": "Apa itu 'transient chirp'?", "id": "Chirp yang terjadi saat transisi bit." },
  { "en": "Apa itu 'Mach-Zehnder modulator' (MZM)?", "id": "Modulator eksternal berbasis interferometer." },
  { "en": "Singkatan MZM (Mach-Zehnder Modulator)?", "id": "Mach-Zehnder Modulator." },
  { "en": "Apa itu 'electro-optic effect'?", "id": "Perubahan indeks bias akibat medan listrik." },
  { "en": "Prinsip kerja MZM (Mach-Zehnder Modulator)?", "id": "Electro-optic effect." },
  { "en": "Apa itu 'VOA (Variable Optical Attenuator)'?", "id": "Attenuator yang bisa diatur." },
  { "en": "Singkatan VOA (Variable Optical Attenuator)?", "id": "Variable Optical Attenuator." },
  { "en": "Apa itu 'backreflection'?", "id": "Sinonim untuk refleksi balik." },
  { "en": "Apa itu 'backscattering'?", "id": "Sinonim untuk hamburan balik." },
  { "en": "OTDR (Optical Time-Domain Reflectometer) bekerja dengan mengukur?", "id": "Sinyal backscattering." },
  { "en": "Perbedaan 'reflection' dan 'scattering'?", "id": "Terarah vs. ke segala arah." },
  { "en": "Apa itu 'optical isolator'?", "id": "Sinonim untuk isolator optik." },
  { "en": "Apa itu 'optical circulator'?", "id": "Sinonim untuk sirkulator optik." },
  { "en": "Apa itu 'optical switch'?", "id": "Sinonim untuk switch optik." },
  { "en": "Apa itu 'MEMS (Micro-Electro-Mechanical Systems)' switch?", "id": "Switch optik menggunakan cermin mikro." },
  { "en": "Singkatan MEMS (Micro-Electro-Mechanical Systems)?", "id": "Micro-Electro-Mechanical Systems." },
  { "en": "Apa itu 'thermo-optic switch'?", "id": "Switch yang menggunakan efek panas." },
  { "en": "Apa itu 'multiplexing'?", "id": "Menggabungkan banyak sinyal jadi satu." },
  { "en": "Apa itu 'demultiplexing'?", "id": "Memisahkan sinyal yang digabung." },
  { "en": "Apa itu 'point-to-point' link?", "id": "Koneksi langsung antara dua titik." },
  { "en": "Apa itu 'point-to-multipoint' link?", "id": "Satu titik terhubung ke banyak titik." },
  { "en": "Jaringan PON (Passive Optical Network) adalah contoh dari?", "id": "Point-to-multipoint." },
  { "en": "Apa itu 'protocol'?", "id": "Aturan komunikasi." },
  { "en": "Apa itu 'bandwidth'?", "id": "Kapasitas transmisi data." },
  { "en": "Satuan 'bandwidth'?", "id": "Bit per detik (bps)." },
  { "en": "Apa itu 'throughput'?", "id": "Laju data bersih yang terkirim." },
  { "en": "Apa itu 'goodput'?", "id": "Throughput di level aplikasi." },
  { "en": "Apa itu 'packet loss'?", "id": "Paket data yang hilang saat transmisi." },
  { "en": "Apa itu 'forward pumping'?", "id": "Arah pump EDFA sama dengan sinyal." },
  { "en": "Apa itu 'backward pumping'?", "id": "Arah pump EDFA berlawanan sinyal." },
  { "en": "Apa itu 'gain tilt'?", "id": "Kemiringan pada spektrum gain EDFA." },
  { "en": "Apa itu 'gain saturation'?", "id": "Penurunan gain saat sinyal input kuat." },
  { "en": "Apa itu 'noise figure'?", "id": "Ukuran noise yang ditambahkan penguat." },
  { "en": "Apa itu 'optical fiber amplifier' (OFA)?", "id": "Penguat yang mediumnya serat optik." },
  { "en": "Singkatan OFA (Optical Fiber Amplifier)?", "id": "Optical Fiber Amplifier." },
  { "en": "EDFA (Erbium-Doped Fiber Amplifier) adalah jenis?", "id": "OFA (Optical Fiber Amplifier)." },
  { "en": "Apa itu 'Raman amplifier'?", "id": "Jenis OFA lain berbasis efek Raman." },
  { "en": "Apa itu 'distributed amplification'?", "id": "Serat transmisi berfungsi sebagai penguat." },
  { "en": "Penguat Raman bisa digunakan sebagai?", "id": "Penguat terdistribusi." },
  { "en": "Apa itu 'zero-dispersion wavelength'?", "id": "Panjang gelombang dengan dispersi kromatik nol." },
  { "en": "Apa itu 'dispersion slope'?", "id": "Laju perubahan dispersi terhadap panjang gelombang." },
  { "en": "Satuan 'dispersion slope'?", "id": "ps/(nm²·km)." },
  { "en": "Apa itu 'cutback method'?", "id": "Metode pengukuran atenuasi serat." },
  { "en": "Apa itu 'microscope'?", "id": "Sinonim untuk mikroskop." },
  { "en": "Apa itu 'interferometer'?", "id": "Sinonim untuk interferometer." },
  { "en": "Apa itu 'laser'?", "id": "Sinonim untuk laser." },
  { "en": "Apa itu 'photodiode'?", "id": "Sinonim untuk fotodioda." },
  { "en": "Apa itu 'fiber optic cable'?", "id": "Sinonim untuk kabel serat optik." },
  { "en": "Apa itu 'single-mode'?", "id": "Sinonim untuk single-mode." },
  { "en": "Apa itu 'multi-mode'?", "id": "Sinonim untuk multi-mode." },
  { "en": "Apa itu 'core'?", "id": "Sinonim untuk core." },
  { "en": "Apa itu 'cladding'?", "id": "Sinonim untuk cladding." },
  { "en": "Apa itu 'coating'?", "id": "Sinonim untuk buffer coating." },
  { "en": "Apa itu 'jacket'?", "id": "Lapisan terluar kabel." },
  { "en": "Apa itu 'strength member'?", "id": "Elemen penguat pada kabel." },
  { "en": "Apa itu 'loose tube'?", "id": "Sinonim untuk loose tube." },
  { "en": "Apa itu 'tight buffer'?", "id": "Sinonim untuk tight buffer." },
  { "en": "Apa itu 'fusion splice'?", "id": "Sinonim untuk fusion splice." },
  { "en": "Apa itu 'mechanical splice'?", "id": "Sinonim untuk mechanical splice." },
  { "en": "Apa itu 'pigtail'?", "id": "Sinonim untuk pigtail." },
  { "en": "Apa itu 'patch cord'?", "id": "Sinonim untuk patch cord." },
  { "en": "Apa itu 'attenuation'?", "id": "Sinonim untuk atenuasi." },
  { "en": "Apa itu 'dispersion'?", "id": "Sinonim untuk dispersi." },
  { "en": "Apa itu 'chromatic dispersion'?", "id": "Sinonim untuk dispersi kromatik." },
  { "en": "Apa itu 'modal dispersion'?", "id": "Sinonim untuk dispersi modal." },
  { "en": "Apa itu 'polarization mode dispersion'?", "id": "Sinonim untuk PMD." },
  { "en": "Apa itu 'numerical aperture'?", "id": "Sinonim untuk NA." },
  { "en": "Apa itu 'total internal reflection'?", "id": "Sinonim untuk TIR." },
  { "en": "Apa itu 'critical angle'?", "id": "Sinonim untuk sudut kritis." },
  { "en": "Apa itu 'refractive index'?", "id": "Sinonim untuk indeks bias." },
  { "en": "Apa itu 'total internal reflection'?", "id": "Sinonim untuk TIR." },
  { "en": "Syarat utama TIR (Total Internal Reflection)?", "id": "Indeks bias core > cladding." },
  { "en": "Apa itu 'critical angle'?", "id": "Sinonim untuk sudut kritis." },
  { "en": "Hukum fisika di balik pembiasan?", "id": "Hukum Snellius." },
  { "en": "Dua jenis serat optik utama?", "id": "Single-mode dan multi-mode." },
  { "en": "Diameter core serat single-mode?", "id": "Sangat kecil (kurang lebih 9 µm)." },
  { "en": "Diameter core serat multi-mode?", "id": "Lebih besar (50 atau 62.5 µm)." },
  { "en": "Kelemahan utama serat multi-mode?", "id": "Dispersi modal." },
  { "en": "Apa yang membatasi jarak SMF (Single-Mode Fiber)?", "id": "Atenuasi dan dispersi kromatik." },
  { "en": "Apa yang membatasi jarak MMF (Multi-Mode Fiber)?", "id": "Dispersi modal." },
  { "en": "Sumber cahaya yang memancarkan cahaya koheren?", "id": "Laser." },
  { "en": "Sumber cahaya yang memancarkan cahaya inkoheren?", "id": "LED (Light Emitting Diode)." },
  { "en": "Perangkat yang mengubah cahaya ke listrik?", "id": "Fotodetektor." },
  { "en": "APD (Avalanche Photodiode) memiliki gain internal?", "id": "Ya." },
  { "en": "Penyebab utama atenuasi dalam serat?", "id": "Hamburan Rayleigh dan penyerapan." },
  { "en": "Penyebab utama dispersi dalam SMF (Single-Mode Fiber)?", "id": "Dispersi kromatik." },
  { "en": "Alat untuk menyambung serat permanen?", "id": "Fusion splicer." },
  { "en": "Alat untuk menyambung serat sementara?", "id": "Konektor." },
  { "en": "Apa itu 'wavelength'?", "id": "Sinonim untuk panjang gelombang." },
  { "en": "Warna jaket serat single-mode indoor?", "id": "Kuning." },
  { "en": "Warna jaket serat multi-mode OM3/OM4?", "id": "Aqua." },
  { "en": "Apa itu 'optical communication system'?", "id": "Sistem komunikasi berbasis cahaya." },
  { "en": "Apa itu 'optical network terminal' (ONT)?", "id": "Perangkat PON di sisi pelanggan." },
  { "en": "Apa itu 'optical line terminal' (OLT)?", "id": "Perangkat sentral di jaringan PON." },
  { "en": "Singkatan OLT (Optical Line Terminal)?", "id": "Optical Line Terminal." },
  { "en": "Singkatan ONT (Optical Network Terminal)?", "id": "Optical Network Terminal." },
  { "en": "Topologi fisik jaringan PON (Passive Optical Network)?", "id": "Pohon (tree)." },
  { "en": "Apa itu 'service level agreement' (SLA)?", "id": "Kontrak layanan antara provider dan pelanggan." },
  { "en": "Singkatan SLA (Service Level Agreement)?", "id": "Service Level Agreement." },
  { "en": "Parameter dalam SLA (Service Level Agreement)?", "id": "Uptime, bandwidth, latency." },
  { "en": "Apa itu 'network management system' (NMS)?", "id": "Sistem untuk memantau dan mengelola jaringan." },
  { "en": "Singkatan NMS (Network Management System)?", "id": "Network Management System." },
  { "en": "Protokol manajemen jaringan yang umum?", "id": "SNMP." },
  { "en": "Singkatan SNMP (Simple Network Management Protocol)?", "id": "Simple Network Management Protocol." },
  { "en": "Apa itu 'optical node'?", "id": "Titik dalam jaringan optik." },
  { "en": "Apa itu 'optical path'?", "id": "Jalur yang ditempuh sinyal optik." },
  { "en": "Apa itu 'wavelength routing'?", "id": "Mengarahkan sinyal berdasarkan panjang gelombangnya." },
  { "en": "Apa itu 'optical burst switching' (OBS)?", "id": "Teknik switching optik." },
  { "en": "Singkatan OBS (Optical Burst Switching)?", "id": "Optical Burst Switching." },
  { "en": "Apa itu 'optical packet switching' (OPS)?", "id": "Teknik switching optik lain." },
  { "en": "Singkatan OPS (Optical Packet Switching)?", "id": "Optical Packet Switching." },
  { "en": "Apa itu 'protocol stack'?", "id": "Lapisan-lapisan protokol komunikasi." },
  { "en": "Model referensi jaringan yang terkenal?", "id": "Model OSI dan TCP/IP." },
  { "en": "Singkatan OSI (Open Systems Interconnection)?", "id": "Open Systems Interconnection." },
  { "en": "Serat optik beroperasi di lapisan mana?", "id": "Lapisan fisik (Physical Layer)." },
  { "en": "Apa itu 'internet exchange point' (IXP)?", "id": "Titik interkoneksi antar ISP." },
  { "en": "Singkatan IXP (Internet Exchange Point)?", "id": "Internet Exchange Point." },
  { "en": "Apa itu 'latency'?", "id": "Waktu tunda total." },
  { "en": "Apa itu 'round-trip time' (RTT)?", "id": "Waktu sinyal pergi dan kembali." },
  { "en": "Singkatan RTT (Round-Trip Time)?", "id": "Round-Trip Time." },
  { "en": "Perintah untuk mengukur RTT (Round-Trip Time)?", "id": "Ping." },
  { "en": "Apa itu 'bufferbloat'?", "id": "Latency tinggi akibat buffer berlebihan." },
  { "en": "Apa itu 'quality of service' (QoS)?", "id": "Manajemen prioritas trafik jaringan." },
  { "en": "Singkatan QoS (Quality of Service)?", "id": "Quality of Service." },
  { "en": "Apa itu 'packet sniffing'?", "id": "Mencegat dan menganalisis trafik jaringan." },
  { "en": "Apa itu 'encryption'?", "id": "Mengamankan data dengan enkripsi." },
  { "en": "Apa itu 'decryption'?", "id": "Mengembalikan data terenkripsi ke bentuk asli." },
  { "en": "Apa itu 'firewall'?", "id": "Sistem keamanan jaringan." },
  { "en": "Apa itu 'intrusion detection system' (IDS)?", "id": "Sistem pendeteksi intrusi." },
  { "en": "Singkatan IDS (Intrusion Detection System)?", "id": "Intrusion Detection System." },
  { "en": "Apa itu 'virtual private network' (VPN)?", "id": "Jaringan pribadi di atas jaringan publik." },
  { "en": "Singkatan VPN (Virtual Private Network)?", "id": "Virtual Private Network." },
  { "en": "Apa itu 'mean time between failures' (MTBF)?", "id": "Waktu rata-rata antar kegagalan." },
  { "en": "Singkatan MTBF (Mean Time Between Failures)?", "id": "Mean Time Between Failures." },
  { "en": "MTBF (Mean Time Between Failures) tinggi berarti?", "id": "Perangkat lebih andal." },
  { "en": "Apa itu 'mean time to repair' (MTTR)?", "id": "Waktu rata-rata untuk perbaikan." },
  { "en": "Singkatan MTTR (Mean Time To Repair)?", "id": "Mean Time To Repair." },
  { "en": "Apa itu 'availability' (ketersediaan)?", "id": "Persentase waktu sistem beroperasi." },
  { "en": "Rumus 'availability'?", "id": "MTBF / (MTBF + MTTR)." },
  { "en": "Standar 'five nines' availability?", "id": "Ketersediaan 99.999%." },
  { "en": "Apa itu 'redundancy'?", "id": "Memiliki komponen cadangan." },
  { "en": "Tujuan 'redundancy'?", "id": "Meningkatkan 'availability'." },
  { "en": "Redundansi 1+1 berarti?", "id": "Satu aktif, satu cadangan." },
  { "en": "Apa itu 'hot standby'?", "id": "Cadangan yang siap mengambil alih." },
  { "en": "Apa itu 'cold standby'?", "id": "Cadangan yang perlu diaktifkan manual." },
  { "en": "Apa itu 'load balancing'?", "id": "Mendistribusikan beban ke beberapa link." },
  { "en": "Apa itu 'link aggregation'?", "id": "Menggabungkan beberapa link menjadi satu." },
  { "en": "Apa itu 'eye diagram'?", "id": "Sinonim untuk diagram mata." },
  { "en": "Apa itu 'optical power'?", "id": "Sinonim untuk daya optik." },
  { "en": "Apa itu 'noise figure'?", "id": "Sinonim untuk 'noise figure'." },
  { "en": "Apa itu 'quantum efficiency'?", "id": "Sinonim untuk efisiensi kuantum." },
  { "en": "Apa itu 'responsivity'?", "id": "Sinonim untuk responsivitas." },
  { "en": "Apa itu 'bandwidth'?", "id": "Sinonim untuk 'bandwidth'." },
  { "en": "Apa itu 'chromatic dispersion'?", "id": "Sinonim untuk dispersi kromatik." },
  { "en": "Apa itu 'modal dispersion'?", "id": "Sinonim untuk dispersi modal." },
  { "en": "Apa itu 'polarization'?", "id": "Sinonim untuk polarisasi." },
  { "en": "Apa itu 'attenuation'?", "id": "Sinonim untuk atenuasi." },
  { "en": "Apa itu 'splicing'?", "id": "Sinonim untuk splicing." },
  { "en": "Apa itu 'connector'?", "id": "Sinonim untuk konektor." },
  { "en": "Apa itu 'laser'?", "id": "Sinonim untuk laser." },
  { "en": "Apa itu 'LED'?", "id": "Sinonim untuk LED." },
  { "en": "Apa itu 'photodiode'?", "id": "Sinonim untuk fotodioda." },
  { "en": "Apa itu 'amplifier'?", "id": "Sinonim untuk penguat." },
  { "en": "Pelebaran pulsa akibat kecepatan cahaya berbeda?", "id": "Dispersi kromatik." },
  { "en": "Pelebaran pulsa akibat banyak jalur cahaya?", "id": "Dispersi modal." },
  { "en": "Teknik meningkatkan kapasitas dengan panjang gelombang?", "id": "WDM (Wavelength Division Multiplexing)." },
  { "en": "Alat untuk melihat lokasi putus serat?", "id": "OTDR (Optical Time-Domain Reflectometer)." },
  { "en": "Proses menyambung permanen dua serat?", "id": "Fusion splicing." },
  { "en": "Apa perbedaan 'laser' dan 'LED (Light Emitting Diode)'?", "id": "Cahaya koheren vs. inkoheren." },
  { "en": "Perbedaan 'fusion' dan 'mechanical splice'?", "id": "Permanen (dilebur) vs. mekanis." },
  { "en": "Perbedaan 'attenuation' dan 'dispersion'?", "id": "Pelemahan daya vs. pelebaran pulsa." },
  { "en": "Apa itu 'effective refractive index'?", "id": "Indeks bias rata-rata yang dilihat mode." },
  { "en": "Apa itu 'zero-dispersion slope'?", "id": "Kemiringan kurva dispersi di titik nol." },
  { "en": "Apa itu 'Brillouin scattering' dalam sensor?", "id": "Untuk sensor regangan dan suhu." },
  { "en": "Apa itu 'Raman scattering' dalam sensor?", "id": "Juga untuk sensor suhu terdistribusi." },
  { "en": "Apa itu 'fiber to the curb' (FTTC)?", "id": "Serat optik sampai ke kabinet jalan." },
  { "en": "Singkatan FTTC (Fiber to the Curb)?", "id": "Fiber to the Curb." },
  { "en": "Apa itu 'fiber to the building' (FTTB)?", "id": "Serat optik sampai ke gedung." },
  { "en": "Singkatan FTTB (Fiber to the Building)?", "id": "Fiber to the Building." },
  { "en": "Apa itu 'fiber to the node' (FTTN)?", "id": "Serat optik sampai ke node." },
  { "en": "Singkatan FTTN (Fiber to the Node)?", "id": "Fiber to the Node." },
  { "en": "Apa itu 'optical line protection' (OLP)?", "id": "Switch proteksi untuk jalur optik." },
  { "en": "Singkatan OLP (Optical Line Protection)?", "id": "Optical Line Protection." },
  { "en": "Apa itu 'forward pumping'?", "id": "Arah pompa sama dengan sinyal." },
  { "en": "Apa itu 'backward pumping'?", "id": "Arah pompa berlawanan dengan sinyal." },
  { "en": "Apa itu 'bidirectional pumping'?", "id": "Pompa dari kedua arah." },
  { "en": "Apa itu 'gain efficiency'?", "id": "Ukuran gain per miliwatt daya pompa." },
  { "en": "Satuan 'gain efficiency'?", "id": "dB/mW." },
  { "en": "Apa itu 'pump depletion'?", "id": "Daya pompa habis oleh sinyal kuat." },
  { "en": "Apa itu 'spontaneous emission noise'?", "id": "Noise dasar pada penguat optik." },
  { "en": "Angka 3 dB pada 'noise figure' disebut?", "id": "Batas kuantum (quantum limit)." },
  { "en": "Apa itu 'polarization dependent gain' (PDG)?", "id": "Gain yang bergantung pada polarisasi." },
  { "en": "Singkatan PDG (Polarization Dependent Gain)?", "id": "Polarization Dependent Gain." },
  { "en": "Apa itu 'gain transient'?", "id": "Fluktuasi gain saat kanal berubah." },
  { "en": "Apa itu 'optical filter'?", "id": "Sinonim untuk filter optik." },
  { "en": "Apa itu 'wavelength blocker'?", "id": "Filter untuk memblokir kanal tertentu." },
  { "en": "Apa itu 'interleaver'?", "id": "Menggabungkan/memisahkan kanal dengan spasi rapat." },
  { "en": "Apa itu 'intraday light'?", "id": "Cahaya dari sumber eksternal." },
  { "en": "Apa itu 'stray light'?", "id": "Cahaya liar yang tidak diinginkan." },
  { "en": "Apa itu 'Rayleigh backscattering'?", "id": "Hamburan Rayleigh ke arah pemancar." },
  { "en": "Prinsip dasar kerja OTDR?", "id": "Rayleigh backscattering." },
  { "en": "Apa itu 'Fresnel reflection'?", "id": "Pantulan akibat perubahan indeks bias." },
  { "en": "Di mana terjadi 'Fresnel reflection'?", "id": "Ujung konektor, patahan serat." },
  { "en": "Peristiwa reflektif pada OTDR ditandai?", "id": "Puncak (spike) tajam." },
  { "en": "Peristiwa non-reflektif pada OTDR ditandai?", "id": "Penurunan daya (drop)." },
  { "en": "Contoh peristiwa non-reflektif?", "id": "Sambungan fusion atau lekukan." },
  { "en": "Apa itu 'slope' pada jejak OTDR?", "id": "Merepresentasikan atenuasi serat (dB/km)." },
  { "en": "Apa itu 'gainer' pada OTDR?", "id": "Sambungan yang tampak memiliki gain." },
  { "en": "Penyebab 'gainer'?", "id": "Menyambung serat dengan koefisien beda." },
  { "en": "Apa itu 'optical power meter'?", "id": "Sinonim untuk OPM." },
  { "en": "Apa itu 'light source'?", "id": "Sinonim untuk OLS." },
  { "en": "Apa itu 'visual fault locator'?", "id": "Sinonim untuk VFL." },
  { "en": "Apa itu 'optical spectrum analyzer'?", "id": "Sinonim untuk OSA." },
  { "en": "Apa itu 'bit error rate tester'?", "id": "Sinonim untuk BERT." },
  { "en": "Pola data untuk pengujian BERT?", "id": "PRBS (Pseudo-Random Binary Sequence)." },
  { "en": "Singkatan PRBS (Pseudo-Random Binary Sequence)?", "id": "Pseudo-Random Binary Sequence." },
  { "en": "Apa itu 'out-of-band' signal?", "id": "Sinyal di luar pita frekuensi operasi." },
  { "en": "Apa itu 'in-band' signal?", "id": "Sinyal di dalam pita frekuensi operasi." },
  { "en": "Apa itu 'single-ended' measurement?", "id": "Pengukuran dari satu ujung saja." },
  { "en": "Contoh instrumen 'single-ended'?", "id": "OTDR." },
  { "en": "Apa itu 'dual-ended' measurement?", "id": "Pengukuran yang membutuhkan dua ujung." },
  { "en": "Contoh pengukuran 'dual-ended'?", "id": "Pengukuran insertion loss (OLS+OPM)." },
  { "en": "Apa itu 'optical link'?", "id": "Sinonim untuk link optik." },
  { "en": "Apa itu 'optical fiber'?", "id": "Sinonim untuk serat optik." },
  { "en": "Apa itu 'optical cable'?", "id": "Sinonim untuk kabel optik." },
  { "en": "Apa itu 'optical network'?", "id": "Sinonim untuk jaringan optik." },
  { "en": "Apa itu 'multiplexing'?", "id": "Sinonim untuk multiplexing." },
  { "en": "Apa itu 'demultiplexing'?", "id": "Sinonim untuk demultiplexing." },
  { "en": "Apa itu 'transmitter'?", "id": "Sinonim untuk transmitter." },
  { "en": "Apa itu 'receiver'?", "id": "Sinonim untuk receiver." },
  { "en": "Apa itu 'transceiver'?", "id": "Sinonim untuk transceiver." },
  { "en": "Apa itu 'optoelectronics'?", "id": "Sinonim untuk optoelektronik." },
  { "en": "Apa itu 'photonics'?", "id": "Sinonim untuk fotonika." },
  { "en": "Apa itu 'lightwave'?", "id": "Sinonim untuk gelombang cahaya." },
  { "en": "Apa itu 'fiber optics'?", "id": "Sinonim untuk serat optik." },
  { "en": "Apa itu 'optical communication'?", "id": "Sinonim untuk komunikasi optik." },
  { "en": "Apa itu 'last mile'?", "id": "Sinonim untuk last mile." },
  { "en": "Apa itu 'backbone'?", "id": "Sinonim untuk backbone network." },
  { "en": "Apa itu 'latency'?", "id": "Sinonim untuk latensi." },
  { "en": "Apa itu 'jitter'?", "id": "Sinonim untuk jitter." },
  { "en": "Apa itu 'protocol'?", "id": "Sinonim untuk protokol." },
  { "en": "Apa itu 'topology'?", "id": "Sinonim untuk topologi." },
  { "en": "Apa itu 'node'?", "id": "Titik koneksi dalam jaringan." },
  { "en": "Apa itu 'link'?", "id": "Koneksi antara dua node." },
  { "en": "Apa itu 'network traffic'?", "id": "Data yang bergerak melalui jaringan." },
  { "en": "Apa itu 'congestion'?", "id": "Kondisi di mana jaringan kelebihan beban." },
  { "en": "Apa itu 'quality of experience' (QoE)?", "id": "Persepsi kualitas layanan oleh pengguna." },
  { "en": "Singkatan QoE (Quality of Experience)?", "id": "Quality of Experience." },
  { "en": "Apa itu 'optical fiber sensing'?", "id": "Penggunaan serat optik sebagai sensor." },
  { "en": "Sensor regangan serat optik?", "id": "FBG (Fiber Bragg Grating)." },
  { "en": "Sensor suhu serat optik?", "id": "FBG atau Raman/Brillouin." },
  { "en": "Sensor rotasi serat optik?", "id": "FOG (Fiber Optic Gyroscope)." },
  { "en": "Apa itu 'acceptance angle'?", "id": "Sudut maksimum cahaya bisa masuk." },
  { "en": "Apa itu 'graded-index'?", "id": "Profil indeks bias tidak uniform." },
  { "en": "Apa itu 'step-index'?", "id": "Profil indeks bias uniform." },
  { "en": "Apa itu 'cladding'?", "id": "Lapisan luar dari inti serat." },
  { "en": "Apa itu 'core'?", "id": "Bagian tengah tempat cahaya merambat." },
  { "en": "Fungsi utama 'cladding'?", "id": "Menjaga cahaya tetap di dalam core." },
  { "en": "TIR (Total Internal Reflection) terjadi di mana?", "id": "Perbatasan antara core dan cladding." }



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
