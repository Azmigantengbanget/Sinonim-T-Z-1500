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


  { "en": "Tera?", "id": "Cap." },
  { "en": "Teracak?", "id": "Acak." },
  { "en": "Teradat?", "id": "Biasa (arkais)." },
  { "en": "Terado?", "id": "Nama tarian." },
  { "en": "Teraju?", "id": "Neraca." },
  { "en": "Terak?", "id": "Sisa pembakaran." },
  { "en": "Terakota?", "id": "Tanah liat bakar." },
  { "en": "Teral?", "id": "Terali." },
  { "en": "Terali?", "id": "Jeruji." },
  { "en": "Teramisin?", "id": "Antibiotik." },
  { "en": "Terampil?", "id": "Mahir." },
  { "en": "Teran?", "id": "Mengeran." },
  { "en": "Terang?", "id": "Jelas." },
  { "en": "Terap?", "id": "Pohon." },
  { "en": "Terapi?", "id": "Pengobatan." },
  { "en": "Terapis?", "id": "Ahli terapi." },
  { "en": "Terapung?", "id": "Mengambang." },
  { "en": "Teras?", "id": "Beranda." },
  { "en": "Terasi?", "id": "Bumbu." },
  { "en": "Teraso?", "id": "Bahan lantai." },
  { "en": "Teratai?", "id": "Bunga." },
  { "en": "Teratak?", "id": "Gubuk." },
  { "en": "Teratogen?", "id": "Penyebab cacat lahir." },
  { "en": "Teratologi?", "id": "Ilmu kelainan." },
  { "en": "Teratoma?", "id": "Tumor." },
  { "en": "Terau?", "id": "Garuk (arkais)." },
  { "en": "Terawang?", "id": "Tembus pandang." },
  { "en": "Terawih?", "id": "Tarawih." },
  { "en": "Terbaru?", "id": "Mutakhir." },
  { "en": "Terbatas?", "id": "Limit." },
  { "en": "Terbengkalai?", "id": "Terlantar." },
  { "en": "Terbit?", "id": "Muncul." },
  { "en": "Terbium?", "id": "Unsur kimia." },
  { "en": "Terbuka?", "id": "Jelas." },
  { "en": "Tercengang?", "id": "Heran." },
  { "en": "Tercer?", "id": "Tumpah (arkais)." },
  { "en": "Terdepan?", "id": "Paling depan." },
  { "en": "Terdidik?", "id": "Terpelajar." },
  { "en": "Teredo?", "id": "Cacing laut." },
  { "en": "Teri?", "id": "Ikan kecil." },
  { "en": "Teriak?", "id": "Pekik." },
  { "en": "Terigu?", "id": "Tepung gandum." },
  { "en": "Terik?", "id": "Sangat panas." },
  { "en": "Terika?", "id": "Setrika (arkais)." },
  { "en": "Terima?", "id": "Sambut." },
  { "en": "Teripang?", "id": "Hewan laut." },
  { "en": "Terista?", "id": "Sangat hina." },
  { "en": "Teritih?", "id": "Cacing." },
  { "en": "Terjal?", "id": "Curam." },
  { "en": "Terjang?", "id": "Serang." },
  { "en": "Terjemah?", "id": "Alih bahasa." },
  { "en": "Terjemahan?", "id": "Hasil terjemah." },
  { "en": "Terjun?", "id": "Lompat ke bawah." },
  { "en": "Terka?", "id": "Tebak." },
  { "en": "Terkadang?", "id": "Adakala." },
  { "en": "Terkam?", "id": "Sergap." },
  { "en": "Terkap?", "id": "Duduk (arkais)." },
  { "en": "Terkenal?", "id": "Populer." },
  { "en": "Terkul?", "id": "Mengantuk." },
  { "en": "Terkup?", "id": "Tutup (arkais)." },
  { "en": "Terlalu?", "id": "Sangat." },
  { "en": "Terlambat?", "id": "Telat." },
  { "en": "Terlanjur?", "id": "Sudah." },
  { "en": "Terlarang?", "id": "Haram." },
  { "en": "Terlena?", "id": "Lalai." },
  { "en": "Terlentang?", "id": "Berbaring." },
  { "en": "Terlezat?", "id": "Paling enak." },
  { "en": "Terma?", "id": "Istilah." },
  { "en": "Termal?", "id": "Panas." },
  { "en": "Termasyhur?", "id": "Terkenal." },
  { "en": "Termin?", "id": "Tahap pembayaran." },
  { "en": "Terminal?", "id": "Stasiun." },
  { "en": "Terminasi?", "id": "Pengakhiran." },
  { "en": "Terminologi?", "id": "Peristilahan." },
  { "en": "Termion?", "id": "Elektron panas." },
  { "en": "Termionika?", "id": "Ilmu termion." },
  { "en": "Termistor?", "id": "Resistor peka suhu." },
  { "en": "Termit?", "id": "Rayap." },
  { "en": "Termodinamika?", "id": "Ilmu panas gerak." },
  { "en": "Termoelektrik?", "id": "Listrik panas." },
  { "en": "Termoelemen?", "id": "Elemen panas." },
  { "en": "Termofili?", "id": "Suka panas." },
  { "en": "Termofobia?", "id": "Takut panas." },
  { "en": "Termofor?", "id": "Pembawa panas." },
  { "en": "Termograf?", "id": "Pencatat suhu." },
  { "en": "Termogram?", "id": "Hasil catatan suhu." },
  { "en": "Termohigrograf?", "id": "Pencatat suhu kelembapan." },
  { "en": "Termokimia?", "id": "Kimia panas." },
  { "en": "Termoklin?", "id": "Lapisan suhu air." },
  { "en": "Termokopel?", "id": "Pengukur suhu." },
  { "en": "Termolabil?", "id": "Tidak tahan panas." },
  { "en": "Termolisis?", "id": "Penguraian panas." },
  { "en": "Termoluminesens?", "id": "Pendaran panas." },
  { "en": "Termometer?", "id": "Pengukur suhu." },
  { "en": "Termonuklir?", "id": "Reaksi inti." },
  { "en": "Termoplastik?", "id": "Plastik leleh." },
  { "en": "Termos?", "id": "Wadah air panas." },
  { "en": "Termosfer?", "id": "Lapisan atmosfer." },
  { "en": "Termostat?", "id": "Pengatur suhu." },
  { "en": "Ternak?", "id": "Hewan peliharaan." },
  { "en": "Ternama?", "id": "Terkenal." },
  { "en": "Ternari?", "id": "Tigaan." },
  { "en": "Ternit?", "id": "Atap." },
  { "en": "Terok?", "id": "Telur (arkais)." },
  { "en": "Terombon?", "id": "Alat musik." },
  { "en": "Terompet?", "id": "Alat musik tiup." },
  { "en": "Teromol?", "id": "Silinder." },
  { "en": "Terondol?", "id": "Gundul." },
  { "en": "Terong?", "id": "Sayuran." },
  { "en": "Teropong?", "id": "Alat lihat jauh." },
  { "en": "Teror?", "id": "Ancaman." },
  { "en": "Teroris?", "id": "Pelaku teror." },
  { "en": "Terorisme?", "id": "Paham teror." },
  { "en": "Terowongan?", "id": "Lorong bawah tanah." },
  { "en": "Terpa?", "id": "Serang." },
  { "en": "Terpal?", "id": "Kain tebal tahan air." },
  { "en": "Terpana?", "id": "Terkejut." },
  { "en": "Terpencil?", "id": "Jauh." },
  { "en": "Terpentin?", "id": "Minyak." },
  { "en": "Terperenyak?", "id": "Terduduk." },
  { "en": "Terpikat?", "id": "Tertarik." },
  { "en": "Terpojok?", "id": "Tersudut." },
  { "en": "Tertawa?", "id": "Gelak." },
  { "en": "Tertib?", "id": "Teratur." },
  { "en": "Terubuk?", "id": "Ikan." },
  { "en": "Terucuk?", "id": "Burung." },
  { "en": "Teruk?", "id": "Parah." },
  { "en": "Terum?", "id": "Kandang (arkais)." },
  { "en": "Terumbu?", "id": "Karang." },
  { "en": "Terung?", "id": "Terong." },
  { "en": "Terungku?", "id": "Penjara." },
  { "en": "Teruntum?", "id": "Pohon." },
  { "en": "Terup?", "id": "Kartu." },
  { "en": "Terus?", "id": "Lanjut." },
  { "en": "Terusi?", "id": "Zat kimia." },
  { "en": "Terwelu?", "id": "Kelinci." },
  { "en": "Tes?", "id": "Ujian." },
  { "en": "Tesa?", "id": "Tesis (arkais)." },
  { "en": "Tesis?", "id": "Pernyataan." },
  { "en": "Tesla?", "id": "Satuan medan magnet." },
  { "en": "Test?", "id": "Tes (Inggris)." },
  { "en": "Testa?", "id": "Kulit biji." },
  { "en": "Testamen?", "id": "Surat wasiat." },
  { "en": "Tester?", "id": "Penguji." },
  { "en": "Testikel?", "id": "Buah zakar." },
  { "en": "Testimoni?", "id": "Kesaksian." },
  { "en": "Testimonial?", "id": "Berkaitan kesaksian." },
  { "en": "Testis?", "id": "Testikel." },
  { "en": "Testosteron?", "id": "Hormon pria." },
  { "en": "Tetak?", "id": "Potong." },
  { "en": "Tetal?", "id": "Padat." },
  { "en": "Tetampan?", "id": "Nampan." },
  { "en": "Tetamu?", "id": "Tamu." },
  { "en": "Tetangga?", "id": "Jiran." },
  { "en": "Tetap?", "id": "Konstan." },
  { "en": "Tetapan?", "id": "Konstanta." },
  { "en": "Tetap?", "id": "Stabil." },
  { "en": "Tetar?", "id": "Pukul (arkais)." },
  { "en": "Tetas?", "id": "Mecah (telur)." },
  { "en": "Tetelo?", "id": "Penyakit ayam." },
  { "en": "Teter?", "id": "Gemetar (arkais)." },
  { "en": "Tetes?", "id": "Titik air." },
  { "en": "Tetirah?", "id": "Berlibur." },
  { "en": "Tetoron?", "id": "Serat sintetis." },
  { "en": "Tetra?", "id": "Empat (awalan)." },
  { "en": "Tetrahidrokanabinol?", "id": "Zat ganja." },
  { "en": "Tetraklorida?", "id": "Senyawa." },
  { "en": "Tetrakord?", "id": "Empat nada." },
  { "en": "Tetrahedron?", "id": "Bidang empat." },
  { "en": "Tetralogi?", "id": "Karya empat seri." },
  { "en": "Tetramer?", "id": "Molekul." },
  { "en": "Tetrapoda?", "id": "Hewan berkaki empat." },
  { "en": "Tetrasiklin?", "id": "Antibiotik." },
  { "en": "Tetris?", "id": "Permainan." },
  { "en": "Tettu?", "id": "Pasti (Bugis)." },
  { "en": "Teuku?", "id": "Gelar bangsawan Aceh." },
  { "en": "Teyan?", "id": "Tahan (arkais)." },
  { "en": "Teisme?", "id": "Paham ketuhanan." },
  { "en": "Teokratis?", "id": "Teokratis." },
  { "en": "Tiada?", "id": "Tidak ada." },
  { "en": "Tiaga?", "id": "Sedia (arkais)." },
  { "en": "Tian?", "id": "Perut (arkais)." },
  { "en": "Tiansi?", "id": "Malaikat (Tionghoa)." },
  { "en": "Tiap?", "id": "Setiap." },
  { "en": "Tiar?", "id": "Mahkota Paus." },
  { "en": "Tiara?", "id": "Mahkota." },
  { "en": "Tiarap?", "id": "Tengkurap." },
  { "en": "Tib?", "id": "Ilmu kedokteran (Arab)." },
  { "en": "Tiba?", "id": "Sampai." },
  { "en": "Tiban?", "id": "Tiban." },
  { "en": "Tidak?", "id": "Bukan." },
  { "en": "Tidur?", "id": "Lelap." },
  { "en": "Tifa?", "id": "Gendang." },
  { "en": "Tifus?", "id": "Penyakit." },
  { "en": "Tiga?", "id": "Angka." },
  { "en": "Tigari?", "id": "Tiga hari (arkais)." },
  { "en": "Tih?", "id": "Kering (arkais)." },
  { "en": "Tihang?", "id": "Tiang (arkais)." },
  { "en": "Tik?", "id": "Bunyi." },
  { "en": "Tikai?", "id": "Sengketa." },
  { "en": "Tikam?", "id": "Tusuk." },
  { "en": "Tikar?", "id": "Alas." },
  { "en": "Tikas?", "id": "Cepat (arkais)." },
  { "en": "Tike?", "id": "Kutu anjing." },
  { "en": "Tiket?", "id": "Karcis." },
  { "en": "Tikim?", "id": "Hukum (arkais)." },
  { "en": "Tikpih?", "id": "Pipih (arkais)." },
  { "en": "Tikung?", "id": "Belok." },
  { "en": "Tikus?", "id": "Hewan pengerat." },
  { "en": "Tilam?", "id": "Kasur." },
  { "en": "Tilan?", "id": "Bekas." },
  { "en": "Tilang?", "id": "Bukti pelanggaran." },
  { "en": "Tilap?", "id": "Curi." },
  { "en": "Tilas?", "id": "Bekas." },
  { "en": "Tilde?", "id": "Tanda baca." },
  { "en": "Tim?", "id": "Kelompok." },
  { "en": "Timah?", "id": "Logam." },
  { "en": "Timang?", "id": "Goyang." },
  { "en": "Timarah?", "id": "Buah (arkais)." },
  { "en": "Timba?", "id": "Wadah air." },
  { "en": "Timbal?", "id": "Logam." },
  { "en": "Timbang?", "id": "Ukur berat." },
  { "en": "Timbar?", "id": "Samping (arkais)." },
  { "en": "Timbel?", "id": "Nasi (Sunda)." },
  { "en": "Timbil?", "id": "Bintitan." },
  { "en": "Timbo?", "id": "Tuba." },
  { "en": "Timbru?", "id": "Warna nada." },
  { "en": "Timbuktu?", "id": "Kota." },
  { "en": "Timbul?", "id": "Muncul." },
  { "en": "Timbun?", "id": "Tumpuk." },
  { "en": "Timi?", "id": "Kelenjar." },
  { "en": "Timidin?", "id": "Nukleosida." },
  { "en": "Timina?", "id": "Basa nitrogen." },
  { "en": "Timokrasi?", "id": "Pemerintahan kehormatan." },
  { "en": "Timol?", "id": "Senyawa." },
  { "en": "Timpani?", "id": "Gendang." },
  { "en": "Timpanitis?", "id": "Kembung." },
  { "en": "Timpanum?", "id": "Gendang telinga." },
  { "en": "Timpa?", "id": "Tindih." },
  { "en": "Timpal?", "id": "Seimbang." },
  { "en": "Timpas?", "id": "Habis." },
  { "en": "Timpuh?", "id": "Simpuh." },
  { "en": "Timpuk?", "id": "Lempar." },
  { "en": "Timpus?", "id": "Tumpul (arkais)." },
  { "en": "Timur?", "id": "Arah matahari terbit." },
  { "en": "Tin?", "id": "Buah." },
  { "en": "Tindak?", "id": "Aksi." },
  { "en": "Tindakan?", "id": "Perbuatan." },
  { "en": "Tindas?", "id": "Tekan." },
  { "en": "Tindih?", "id": "Timpa." },
  { "en": "Tiner?", "id": "Pengencer cat." },
  { "en": "Ting?", "id": "Bunyi." },
  { "en": "Tinggal?", "id": "Diam." },
  { "en": "Tinggam?", "id": "Getah." },
  { "en": "Tinggang?", "id": "Selisih (arkais)." },
  { "en": "Tinggar?", "id": "Tengger." },
  { "en": "Tingi?", "id": "Pohon." },
  { "en": "Tingih?", "id": "Tinggi (arkais)." },
  { "en": "Tingkah?", "id": "Perilaku." },
  { "en": "Tingkal?", "id": "Boraks." },
  { "en": "Tingkap?", "id": "Jendela." },
  { "en": "Tingkar?", "id": "Pecah (arkais)." },
  { "en": "Tingkas?", "id": "Siap." },
  { "en": "Tingkat?", "id": "Level." },
  { "en": "Tingkil?", "id": "Benjol." },
  { "en": "Tingkrang?", "id": "Duduk mengangkang." },
  { "en": "Tinjau?", "id": "Lihat." },
  { "en": "Tinjauan?", "id": "Kajian." },
  { "en": "Tinja?", "id": "Feses." },
  { "en": "Tinjak?", "id": "Injak (arkais)." },
  { "en": "Tinju?", "id": "Pukulan tangan." },
  { "en": "Tinta?", "id": "Cairan tulis." },
  { "en": "Tinting?", "id": "Uji." },
  { "en": "Tintir?", "id": "Lampu kecil." },
  { "en": "Tipe?", "id": "Jenis." },
  { "en": "Tipis?", "id": "Halus." },
  { "en": "Tipografi?", "id": "Seni cetak." },
  { "en": "Tipologi?", "id": "Ilmu tipe." },
  { "en": "Tipu?", "id": "Muslihat." },
  { "en": "Tirah?", "id": "Pindah (arkais)." },
  { "en": "Tirai?", "id": "Tabir." },
  { "en": "Tirakat?", "id": "Bertapa." },
  { "en": "Tiram?", "id": "Kerang." },
  { "en": "Tiran?", "id": "Penguasa kejam." },
  { "en": "Tirani?", "id": "Kekejaman." },
  { "en": "Tiras?", "id": "Oplah." },
  { "en": "Tirau?", "id": "Hantu." },
  { "en": "Tiri?", "id": "Bukan kandung." },
  { "en": "Tiris?", "id": "Bocor." },
  { "en": "Tirol?", "id": "Daerah Austria." },
  { "en": "Tiru?", "id": "Jiplak." },
  { "en": "Tiruan?", "id": "Imitasi." },
  { "en": "Tirus?", "id": "Runcing." },
  { "en": "Tis?", "id": "Seruan." },
  { "en": "Tisik?", "id": "Jahit." },
  { "en": "Tisu?", "id": "Kertas pembersih." },
  { "en": "Titan?", "id": "Raksasa mitologi." },
  { "en": "Titah?", "id": "Perintah." },
  { "en": "Titanium?", "id": "Unsur kimia." },
  { "en": "Titar?", "id": "Getar (arkais)." },
  { "en": "Titer?", "id": "Ukuran larutan." },
  { "en": "Titik?", "id": "Noktah." },
  { "en": "Titil?", "id": "Sedikit (arkais)." },
  { "en": "Titinada?", "id": "Not musik." },
  { "en": "Titip?", "id": "Amanat." },
  { "en": "Titir?", "id": "Kentungan." },
  { "en": "Titis?", "id": "Tetes." },
  { "en": "Titisan?", "id": "Penjelmaan." },
  { "en": "Titrasi?", "id": "Analisis kimia." },
  { "en": "Titrimetri?", "id": "Titrasi." },
  { "en": "Titular?", "id": "Nama jabatan." },
  { "en": "Tius?", "id": "Bau (arkais)." },
  { "en": "Tmesis?", "id": "Pemisahan kata." },
  { "en": "Toa?", "id": "Pengeras suara." },
  { "en": "Toapekong?", "id": "Dewa Tionghoa." },
  { "en": "Tobak?", "id": "Bertobat (arkais)." },
  { "en": "Tobat?", "id": "Insaf." },
  { "en": "Tobo?", "id": "Kelompok (Jepang)." },
  { "en": "Tobong?", "id": "Tempat pembakaran." },
  { "en": "Todak?", "id": "Ikan." },
  { "en": "Todong?", "id": "Ancam." },
  { "en": "Toga?", "id": "Jubah." },
  { "en": "Togan?", "id": "Kokoh (arkais)." },
  { "en": "Togel?", "id": "Buntung." },
  { "en": "Togok?", "id": "Patung." },
  { "en": "Toh?", "id": "Seruan." },
  { "en": "Tohok?", "id": "Tusuk." },
  { "en": "Tohor?", "id": "Dangkal." },
  { "en": "Toi?", "id": "Tanah (arkais)." },
  { "en": "Toilet?", "id": "WC." },
  { "en": "Tok?", "id": "Pukul (Belanda)." },
  { "en": "Tokak?", "id": "Gigit (arkais)." },
  { "en": "Tokcer?", "id": "Bagus." },
  { "en": "Tokek?", "id": "Cicak besar." },
  { "en": "Toko?", "id": "Kedai." },
  { "en": "Tokoh?", "id": "Figur." },
  { "en": "Tokok?", "id": "Tambah (arkais)." },
  { "en": "Tokong?", "id": "Pulau kecil." },
  { "en": "Toksemia?", "id": "Keracunan darah." },
  { "en": "Toksik?", "id": "Beracun." },
  { "en": "Toksikogenik?", "id": "Menghasilkan racun." },
  { "en": "Toksikolog?", "id": "Ahli racun." },
  { "en": "Toksikologi?", "id": "Ilmu racun." },
  { "en": "Toksin?", "id": "Racun." },
  { "en": "Toksoplasma?", "id": "Parasit." },
  { "en": "Tol?", "id": "Jalan bebas hambatan." },
  { "en": "Tolak?", "id": "Sanggah." },
  { "en": "Tolan?", "id": "Sahabat." },
  { "en": "Toleh?", "id": "Menoleh." },
  { "en": "Toleran?", "id": "Tenggang rasa." },
  { "en": "Toleransi?", "id": "Sikap tenggang rasa." },
  { "en": "Tolo?", "id": "Bodoh (Jawa)." },
  { "en": "Tolok?", "id": "Ukuran." },
  { "en": "Tolol?", "id": "Bodoh." },
  { "en": "Tolong?", "id": "Bantu." },
  { "en": "Tom?", "id": "Kekang kuda." },
  { "en": "Tomahawk?", "id": "Kapak Indian." },
  { "en": "Toman?", "id": "Ikan." },
  { "en": "Tomat?", "id": "Buah." },
  { "en": "Tombak?", "id": "Senjata." },
  { "en": "Tomboi?", "id": "Gadis kelaki-lakian." },
  { "en": "Tombol?", "id": "Kancing." },
  { "en": "Tombong?", "id": "Pangkal kelapa." },
  { "en": "Tomografi?", "id": "Teknik pemindaian." },
  { "en": "Tomong?", "id": "Meriam kecil." },
  { "en": "Tompel?", "id": "Tahi lalat." },
  { "en": "Ton?", "id": "Satuan berat." },
  { "en": "Tona?", "id": "Warna." },
  { "en": "Tonase?", "id": "Daya angkut kapal." },
  { "en": "Tonder?", "id": "Guntur (Belanda)." },
  { "en": "Tong?", "id": "Wadah besar." },
  { "en": "Tonggak?", "id": "Tiang." },
  { "en": "Tonggeret?", "id": "Serangga." },
  { "en": "Tonggok?", "id": "Tumpuk." },
  { "en": "Tonggos?", "id": "Menonjol (gigi)." },
  { "en": "Tongkah?", "id": "Papan luncur lumpur." },
  { "en": "Tongkang?", "id": "Perahu besar." },
  { "en": "Tongkat?", "id": "Galah." },
  { "en": "Tongkeng?", "id": "Tulang ekor." },
  { "en": "Tongkol?", "id": "Ikan." },
  { "en": "Tongkrong?", "id": "Duduk." },
  { "en": "Tongol?", "id": "Muncul." },
  { "en": "Tongseng?", "id": "Masakan." },
  { "en": "Tonik?", "id": "Obat penguat." },
  { "en": "Tonil?", "id": "Sandiwara." },
  { "en": "Tonisitas?", "id": "Ketegangan otot." },
  { "en": "Tonjok?", "id": "Tinju." },
  { "en": "Tonjol?", "id": "Menonjol." },
  { "en": "Tonometer?", "id": "Pengukur tekanan." },
  { "en": "Tonsil?", "id": "Amandel." },
  { "en": "Tonsilitis?", "id": "Radang amandel." },
  { "en": "Tonto?", "id": "Bodoh (arkais)." },
  { "en": "Tonton?", "id": "Lihat." },
  { "en": "Top?", "id": "Atas (Inggris)." },
  { "en": "Topan?", "id": "Badai." },
  { "en": "Topang?", "id": "Sangga." },
  { "en": "Topas?", "id": "Batu permata." },
  { "en": "Topdal?", "id": "Meriam." },
  { "en": "Topek?", "id": "Tari (arkais)." },
  { "en": "Topeng?", "id": "Kedok." },
  { "en": "Topes?", "id": "Penyakit." },
  { "en": "Topi?", "id": "Penutup kepala." },
  { "en": "Topiari?", "id": "Seni pangkas tanaman." },
  { "en": "Topik?", "id": "Pokok bahasan." },
  { "en": "Topikal?", "id": "Lokal." },
  { "en": "Topikalisasi?", "id": "Penonjolan topik." },
  { "en": "Topinambur?", "id": "Tanaman." },
  { "en": "Toples?", "id": "Wadah." },
  { "en": "Topo?", "id": "Bertapa (Jawa)." },
  { "en": "Topografi?", "id": "Ilmu bentuk permukaan." },
  { "en": "Topografis?", "id": "Berkaitan topografi." },
  { "en": "Topong?", "id": "Topi (arkais)." },
  { "en": "Toponimi?", "id": "Ilmu nama tempat." },
  { "en": "Torak?", "id": "Silinder." },
  { "en": "Toraks?", "id": "Dada." },
  { "en": "Torani?", "id": "Prajurit (Bugis)." },
  { "en": "Toreh?", "id": "Gores." },
  { "en": "Torek?", "id": "Tuli." },
  { "en": "Toren?", "id": "Menara air." },
  { "en": "Tornado?", "id": "Angin puting beliung." },
  { "en": "Torne?", "id": "Turnamen (Belanda)." },
  { "en": "Toroid?", "id": "Bentuk." },
  { "en": "Torpedo?", "id": "Senjata laut." },
  { "en": "Torsi?", "id": "Daya putar." },
  { "en": "Torso?", "id": "Badan." },
  { "en": "Tortikolis?", "id": "Leher kaku." },
  { "en": "Total?", "id": "Jumlah." },
  { "en": "Totalisator?", "id": "Mesin judi." },
  { "en": "Totaliter?", "id": "Mutlak." },
  { "en": "Totaliterisme?", "id": "Paham totaliter." },
  { "en": "Totalitas?", "id": "Keseluruhan." },
  { "en": "Totau?", "id": "Orang Tionghoa." },
  { "en": "Totebag?", "id": "Tas jinjing." },
  { "en": "Totem?", "id": "Simbol suku." },
  { "en": "Totemisme?", "id": "Pemujaan totem." },
  { "en": "Totok?", "id": "Asli (Tionghoa)." },
  { "en": "Totol?", "id": "Bintik." },
  { "en": "Tower?", "id": "Menara (Inggris)." },
  { "en": "Toyib?", "id": "Baik (Arab)." },
  { "en": "Trabekula?", "id": "Jaringan." },
  { "en": "Tradisi?", "id": "Adat." },
  { "en": "Tradisional?", "id": "Konvensional." },
  { "en": "Trafalgar?", "id": "Pertempuran." },
  { "en": "Tragedi?", "id": "Kisah sedih." },
  { "en": "Tragikomedi?", "id": "Drama." },
  { "en": "Tragis?", "id": "Menyedihkan." },
  { "en": "Trakea?", "id": "Batang tenggorok." },
  { "en": "Trakeida?", "id": "Sel tumbuhan." },
  { "en": "Trakeofita?", "id": "Tumbuhan berpembuluh." },
  { "en": "Traktat?", "id": "Perjanjian." },
  { "en": "Traktir?", "id": "Bayari." },
  { "en": "Traktor?", "id": "Kendaraan pertanian." },
  { "en": "Traktus?", "id": "Saluran." },
  { "en": "Tram?", "id": "Kereta listrik." },
  { "en": "Trampolin?", "id": "Alat loncat." },
  { "en": "Trans?", "id": "Keadaan tak sadar." },
  { "en": "Transaksi?", "id": "Jual beli." },
  { "en": "Transducer?", "id": "Pengubah energi." },
  { "en": "Transek?", "id": "Garis survei." },
  { "en": "Transeksual?", "id": "Mengubah kelamin." },
  { "en": "Transfer?", "id": "Pemindahan." },
  { "en": "Transfigurasi?", "id": "Perubahan rupa." },
  { "en": "Transformasi?", "id": "Perubahan." },
  { "en": "Transformatif?", "id": "Mengubah." },
  { "en": "Transformator?", "id": "Alat listrik." },
  { "en": "Transfusi?", "id": "Pemindahan darah." },
  { "en": "Transgenik?", "id": "Rekayasa genetik." },
  { "en": "Transgresi?", "id": "Pelanggaran." },
  { "en": "Transisi?", "id": "Peralihan." },
  { "en": "Transisional?", "id": "Bersifat peralihan." },
  { "en": "Transistor?", "id": "Komponen elektronik." },
  { "en": "Transit?", "id": "Singgah." },
  { "en": "Transkrip?", "id": "Salinan." },
  { "en": "Transkripsi?", "id": "Penyalinan." },
  { "en": "Transliterasi?", "id": "Alih aksara." },
  { "en": "Translokasi?", "id": "Pemindahan." },
  { "en": "Translusen?", "id": "Tembus cahaya." },
  { "en": "Transmigran?", "id": "Orang bertransmigrasi." },
  { "en": "Transmigrasi?", "id": "Perpindahan penduduk." },
  { "en": "Transmisi?", "id": "Penularan." },
  { "en": "Transmiter?", "id": "Pemancar." },
  { "en": "Transmutasi?", "id": "Perubahan unsur." },
  { "en": "Transnasional?", "id": "Lintas negara." },
  { "en": "Transonik?", "id": "Kecepatan suara." },
  { "en": "Transparan?", "id": "Tembus pandang." },
  { "en": "Transparansi?", "id": "Keterbukaan." },
  { "en": "Transpirasi?", "id": "Penguapan tumbuhan." },
  { "en": "Transplantasi?", "id": "Pencangkokan." },
  { "en": "Transpor?", "id": "Pengangkutan." },
  { "en": "Transportasi?", "id": "Pengangkutan." },
  { "en": "Transversal?", "id": "Melintang." },
  { "en": "Trap?", "id": "Perangkap (Inggris)." },
  { "en": "Trap?", "id": "Tangga (Belanda)." },
  { "en": "Trapesium?", "id": "Bentuk geometri." },
  { "en": "Trapezoid?", "id": "Trapesium." },
  { "en": "Trauma?", "id": "Cedera jiwa." },
  { "en": "Traumatis?", "id": "Mencederai." },
  { "en": "Trawal?", "id": "Pukat." },
  { "en": "Trawas?", "id": "Terawang." },
  { "en": "Trecet?", "id": "Ular (arkais)." },
  { "en": "Trem?", "id": "Tram." },
  { "en": "Trematoda?", "id": "Cacing." },
  { "en": "Trembesi?", "id": "Pohon." },
  { "en": "Tremor?", "id": "Getaran." },
  { "en": "Tren?", "id": "Kecenderungan." },
  { "en": "Trenggiling?", "id": "Tenggiling." },
  { "en": "Trengginas?", "id": "Tangkas." },
  { "en": "Trenyuh?", "id": "Terharu." },
  { "en": "Tres?", "id": "Sangat (arkais)." },
  { "en": "Tresna?", "id": "Cinta (Jawa)." },
  { "en": "Tri?", "id": "Tiga." },
  { "en": "Trias?", "id": "Zaman geologi." },
  { "en": "Triatlon?", "id": "Lomba tiga cabang." },
  { "en": "Tribal?", "id": "Kesukuan." },
  { "en": "Tribalisme?", "id": "Paham kesukuan." },
  { "en": "Tribokelistrikan?", "id": "Listrik gesekan." },
  { "en": "Tribologi?", "id": "Ilmu gesekan." },
  { "en": "Tribun?", "id": "Panggung penonton." },
  { "en": "Tribuna?", "id": "Mimbari." },
  { "en": "Tribunal?", "id": "Pengadilan." },
  { "en": "Tributa?", "id": "Upeti." },
  { "en": "Tridarma?", "id": "Tiga kewajiban." },
  { "en": "Tridatu?", "id": "Tiga warna (Bali)." },
  { "en": "Tridharma?", "id": "Tridarma." },
  { "en": "Trident?", "id": "Tombak trisula." },
  { "en": "Trienial?", "id": "Tiga tahunan." },
  { "en": "Trifoliat?", "id": "Berdaun tiga." },
  { "en": "Trigatra?", "id": "Tiga aspek." },
  { "en": "Trigemius?", "id": "Saraf." },
  { "en": "Trigliserida?", "id": "Lemak." },
  { "en": "Trigonometri?", "id": "Ilmu ukur segitiga." },
  { "en": "Triko?", "id": "Kain rajut." },
  { "en": "Trikora?", "id": "Tri Komando Rakyat." },
  { "en": "Trikotomi?", "id": "Pembagian tiga." },
  { "en": "Tril?", "id": "Getaran suara." },
  { "en": "Trilipat?", "id": "Tiga lipat." },
  { "en": "Trilingga?", "id": "Pengulangan tiga kali." },
  { "en": "Trilogi?", "id": "Karya tiga seri." },
  { "en": "Trilon?", "id": "Serat sintetis." },
  { "en": "Trilumin?", "id": "Zat." },
  { "en": "Trim?", "id": "Potong rapi." },
  { "en": "Trimar?", "id": "Kapal (arkais)." },
  { "en": "Trimatra?", "id": "Tiga dimensi." },
  { "en": "Trimester?", "id": "Tiga bulan." },
  { "en": "Trimurti?", "id": "Tiga dewa Hindu." },
  { "en": "Trindil?", "id": "Habis (Jawa)." },
  { "en": "Trinitas?", "id": "Tritunggal." },
  { "en": "Trinitrotoluena?", "id": "TNT." },
  { "en": "Trio?", "id": "Kelompok tiga." },
  { "en": "Trioda?", "id": "Tabung elektron." },
  { "en": "Trip?", "id": "Perjalanan (Inggris)." },
  { "en": "Tripang?", "id": "Teripang." },
  { "en": "Tripartit?", "id": "Tiga pihak." },
  { "en": "Tripleks?", "id": "Kayu lapis." },
  { "en": "Triplet?", "id": "Kembar tiga." },
  { "en": "Triploblastik?", "id": "Tiga lapisan embrio." },
  { "en": "Tripod?", "id": "Kaki tiga." },
  { "en": "Triptofan?", "id": "Asam amino." },
  { "en": "Triptotos?", "id": "Kata tiga kasus." },
  { "en": "Trisep?", "id": "Otot lengan." },
  { "en": "Trisila?", "id": "Tiga sila." },
  { "en": "Trisula?", "id": "Tombak tiga mata." },
  { "en": "Tritagonis?", "id": "Tokoh ketiga." },
  { "en": "Tritangtu?", "id": "Tiga penguasa Sunda." },
  { "en": "Tritikal?", "id": "Tanaman." },
  { "en": "Tritium?", "id": "Isotop hidrogen." },
  { "en": "Triton?", "id": "Moluska laut." },
  { "en": "Tritura?", "id": "Tri Tuntutan Rakyat." },
  { "en": "Triturasi?", "id": "Penggerusan." },
  { "en": "Triwarsa?", "id": "Tiga tahun." },
  { "en": "Triwindu?", "id": "Duapuluh empat tahun." },
  { "en": "Triwulan?", "id": "Tiga bulan." },
  { "en": "Trofi?", "id": "Piala." },
  { "en": "Troglobit?", "id": "Hewan gua." },
  { "en": "Trogon?", "id": "Burung." },
  { "en": "Troika?", "id": "Tiga serangkai." },
  { "en": "Troli?", "id": "Kereta dorong." },
  { "en": "Trombin?", "id": "Enzim." },
  { "en": "Trombon?", "id": "Alat musik tiup." },
  { "en": "Trombosis?", "id": "Pembekuan darah." },
  { "en": "Trombosit?", "id": "Keping darah." },
  { "en": "Trombus?", "id": "Bekuan darah." },
  { "en": "Tropi?", "id": "Trofi." },
  { "en": "Tropika?", "id": "Daerah tropis." },
  { "en": "Tropis?", "id": "Iklim." },
  { "en": "Tropisme?", "id": "Gerak tumbuhan." },
  { "en": "Tropopause?", "id": "Lapisan atmosfer." },
  { "en": "Troposfer?", "id": "Lapisan atmosfer." },
  { "en": "Trotok?", "id": "Bunyi (arkais)." },
  { "en": "Trotoar?", "id": "Jalur pejalan kaki." },
  { "en": "Truf?", "id": "Kartu." },
  { "en": "Truk?", "id": "Mobil besar." },
  { "en": "Trusa?", "id": "Penyakit." },
  { "en": "Tsunami?", "id": "Gelombang laut besar." },
  { "en": "Tu?", "id": "Kamu (Prancis)." },
  { "en": "Tua?", "id": "Lanjut usia." },
  { "en": "Tuah?", "id": "Berkat." },
  { "en": "Tuai?", "id": "Panen." },
  { "en": "Tuak?", "id": "Minuman beralkohol." },
  { "en": "Tual?", "id": "Potongan kayu." },
  { "en": "Tuala?", "id": "Handuk." },
  { "en": "Tualang?", "id": "Pengembara." },
  { "en": "Tuam?", "id": "Kompres panas." },
  { "en": "Tuan?", "id": "Panggilan hormat pria." },
  { "en": "Tuang?", "id": "Cucur." },
  { "en": "Tuanku?", "id": "Tuanku." },
  { "en": "Tuap?", "id": "Uap (arkais)." },
  { "en": "Tuas?", "id": "Pengungkit." },
  { "en": "Tuba?", "id": "Racun ikan." },
  { "en": "Tubagus?", "id": "Gelar bangsawan Banten." },
  { "en": "Tubal?", "id": "Berkaitan tuba." },
  { "en": "Tubektomi?", "id": "Sterilisasi wanita." },
  { "en": "Tuberkel?", "id": "Benjolan." },
  { "en": "Tuberkulin?", "id": "Zat." },
  { "en": "Tuberkulosis?", "id": "TBC." },
  { "en": "Tuberositas?", "id": "Tonjolan tulang." },
  { "en": "Tubir?", "id": "Tepi jurang." },
  { "en": "Tubruk?", "id": "Tabrak." },
  { "en": "Tubuh?", "id": "Badan." },
  { "en": "Tuduh?", "id": "Sangka." },
  { "en": "Tudung?", "id": "Penutup." },
  { "en": "Tufa?", "id": "Batuan." },
  { "en": "Tufah?", "id": "Apel (Arab)." },
  { "en": "Tugal?", "id": "Alat tanam." },
  { "en": "Tugas?", "id": "Pekerjaan." },
  { "en": "Tugel?", "id": "Patah (Jawa)." },
  { "en": "Tugu?", "id": "Monumen." },
  { "en": "Tuhan?", "id": "Pencipta." },
  { "en": "Tuhfah?", "id": "Hadiah (Arab)." },
  { "en": "Tuhmah?", "id": "Tuduhan (Arab)." },
  { "en": "Tuhfatulajan?", "id": "Hadiah istimewa." },
  { "en": "Tuhu?", "id": "Benar (arkais)." },
  { "en": "Tui?", "id": "Penyakit." },
  { "en": "Tuil?", "id": "Tuas." },
  { "en": "Tuis?", "id": "Duri (Belanda)." },
  { "en": "Tujah?", "id": "Tusuk." },
  { "en": "Tuju?", "id": "Arah." },
  { "en": "Tujuan?", "id": "Maksud." },
  { "en": "Tujuh?", "id": "Angka." },
  { "en": "Tuk?", "id": "Mata air (Jawa)." },
  { "en": "Tukak?", "id": "Luka." },
  { "en": "Tukal?", "id": "Gulungan benang." },
  { "en": "Tukam?", "id": "Terima (arkais)." },
  { "en": "Tukang?", "id": "Ahli." },
  { "en": "Tukar?", "id": "Ganti." },
  { "en": "Tukas?", "id": "Jawab." },
  { "en": "Tuki?", "id": "Gendang." },
  { "en": "Tukik?", "id": "Anak penyu." },
  { "en": "Tuksido?", "id": "Jas." },
  { "en": "Tuku?", "id": "Beli (Jawa)." },
  { "en": "Tukul?", "id": "Palu." },
  { "en": "Tukun?", "id": "Karang." },
  { "en": "Tukus?", "id": "Kecil (arkais)." },
  { "en": "Tul?", "id": "Kain (Belanda)." },
  { "en": "Tula?", "id": "Penyakit." },
  { "en": "Tula?", "id": "Timbangan (Sanskerta)." },
  { "en": "Tulah?", "id": "Celaka." },
  { "en": "Tulang?", "id": "Rangka." },
  { "en": "Tular?", "id": "Jangkit." },
  { "en": "Tulat?", "id": "Lusa (setelah)." },
  { "en": "Tule?", "id": "Kain." },
  { "en": "Tuli?", "id": "Tidak mendengar." },
  { "en": "Tulip?", "id": "Bunga." },
  { "en": "Tulis?", "id": "Catat." },
  { "en": "Tulu?", "id": "Tolong (arkais)." },
  { "en": "Tulup?", "id": "Sumpit." },
  { "en": "Tulus?", "id": "Ikhlas." },
  { "en": "Tum?", "id": "Makanan." },
  { "en": "Tuma?", "id": "Kutu." },
  { "en": "Tuman?", "id": "Kebiasaan buruk (Jawa)." },
  { "en": "Tumang?", "id": "Tungku." },
  { "en": "Tumbakan?", "id": "Tombak." },
  { "en": "Tumbal?", "id": "Korban." },
  { "en": "Tumbang?", "id": "Roboh." },
  { "en": "Tumbas?", "id": "Beli (Jawa)." },
  { "en": "Tumben?", "id": "Jarang." },
  { "en": "Tumbu?", "id": "Wadah." },
  { "en": "Tumbuh?", "id": "Berkembang." },
  { "en": "Tumbuhan?", "id": "Flora." },
  { "en": "Tumbuk?", "id": "Lumat." },
  { "en": "Tumbung?", "id": "Penyakit." },
  { "en": "Tumenggung?", "id": "Temenggung." },
  { "en": "Tumis?", "id": "Masak." },
  { "en": "Tumit?", "id": "Bagian kaki." },
  { "en": "Tumor?", "id": "Benjolan." },
  { "en": "Tumpah?", "id": "Cecer." },
  { "en": "Tumpak?", "id": "Naik (arkais)." },
  { "en": "Tumpal?", "id": "Motif batik." },
  { "en": "Tumpang?", "id": "Ikut." },
  { "en": "Tumpas?", "id": "Basmi." },
  { "en": "Tumpat?", "id": "Padat." },
  { "en": "Tumpeng?", "id": "Nasi." },
  { "en": "Tumpil?", "id": "Dukung (arkais)." },
  { "en": "Tumpu?", "id": "Sangga." },
  { "en": "Tumpuk?", "id": "Susun." },
  { "en": "Tumpul?", "id": "Tidak tajam." },
  { "en": "Tumpur?", "id": "Hancur." },
  { "en": "Tun?", "id": "Gelar bangsawan Melayu." },
  { "en": "Tuna?", "id": "Ikan." },
  { "en": "Tunadaksa?", "id": "Cacat tubuh." },
  { "en": "Tunagrahita?", "id": "Cacat mental." },
  { "en": "Tunai?", "id": "Kontan." },
  { "en": "Tunakarya?", "id": "Pengangguran." },
  { "en": "Tunalaras?", "id": "Cacat sosial." },
  { "en": "Tunanetra?", "id": "Buta." },
  { "en": "Tunang?", "id": "Calon pasangan." },
  { "en": "Tunarungu?", "id": "Tuli." },
  { "en": "Tunas?", "id": "Bibit." },
  { "en": "Tunatenaga?", "id": "Lemah (arkais)." },
  { "en": "Tunawicara?", "id": "Bisu." },
  { "en": "Tunawisma?", "id": "Gelandangan." },
  { "en": "Tunda?", "id": "Tangguh." },
  { "en": "Tundan?", "id": "Tingkat (arkais)." },
  { "en": "Tundang?", "id": "Ajak (arkais)." },
  { "en": "Tundra?", "id": "Padang lumut." },
  { "en": "Tunduk?", "id": "Patuh." },
  { "en": "Tundun?", "id": "Tandan." },
  { "en": "Tungau?", "id": "Kutu." },
  { "en": "Tunggak?", "id": "Sisa." },
  { "en": "Tunggal?", "id": "Satu." },
  { "en": "Tunggang?", "id": "Naik." },
  { "en": "Tunggik?", "id": "Bokong." },
  { "en": "Tunggu?", "id": "Nanti." },
  { "en": "Tunggul?", "id": "Panji." },
  { "en": "Tungik?", "id": "Dungu (arkais)." },
  { "en": "Tungkai?", "id": "Kaki." },
  { "en": "Tungkak?", "id": "Tumit." },
  { "en": "Tungkik?", "id": "Turun curam." },
  { "en": "Tungkrap?", "id": "Tutup (arkais)." },
  { "en": "Tungku?", "id": "Dapur." },
  { "en": "Tungkup?", "id": "Tutup." },
  { "en": "Tungkus?", "id": "Bungkus." },
  { "en": "Tunjal?", "id": "Dorong." },
  { "en": "Tunjam?", "id": "Tancap." },
  { "en": "Tunjang?", "id": "Sokong." },
  { "en": "Tunjuk?", "id": "Arahkan." },
  { "en": "Tunjung?", "id": "Teratai." },
  { "en": "Tuntas?", "id": "Selesai." },
  { "en": "Tuntun?", "id": "Bimbing." },
  { "en": "Tuntut?", "id": "Tagih." },
  { "en": "Tunu?", "id": "Bakar." },
  { "en": "Tupai?", "id": "Hewan." },
  { "en": "Tur?", "id": "Perjalanan." },
  { "en": "Tura?", "id": "Bicara (arkais)." },
  { "en": "Turang?", "id": "Kakak (Batak)." },
  { "en": "Turangga?", "id": "Kuda (Sanskerta)." },
  { "en": "Turap?", "id": "Lapisan." },
  { "en": "Turbiditas?", "id": "Kekeruhan." },
  { "en": "Turbin?", "id": "Mesin." },
  { "en": "Turbojet?", "id": "Mesin jet." },
  { "en": "Turbulen?", "id": "Kacau." },
  { "en": "Turbulensi?", "id": "Kekacauan." },
  { "en": "Turi?", "id": "Pohon." },
  { "en": "Turinisasi?", "id": "Penyesuaian turis." },
  { "en": "Turis?", "id": "Wisatawan." },
  { "en": "Turistik?", "id": "Bersifat wisata." },
  { "en": "Turisme?", "id": "Pariwisata." },
  { "en": "Turk?", "id": "Bangsa Turki." },
  { "en": "Turki?", "id": "Negara." },
  { "en": "Turmalin?", "id": "Batu permata." },
  { "en": "Turnamen?", "id": "Perlombaan." },
  { "en": "Turne?", "id": "Perjalanan dinas." },
  { "en": "Turnoi?", "id": "Turnamen (arkais)." },
  { "en": "Turun?", "id": "Menurun." },
  { "en": "Turus?", "id": "Pancang." },
  { "en": "Turut?", "id": "Ikut." },
  { "en": "Tus?", "id": "Suara (arkais)." },
  { "en": "Tusam?", "id": "Pinus." },
  { "en": "Tusir?", "id": "Lukis (arkais)." },
  { "en": "Tusk?", "id": "Gading (Inggris)." },
  { "en": "Tuslah?", "id": "Biaya tambahan." },
  { "en": "Tusuk?", "id": "Tikam." },
  { "en": "Tuter?", "id": "Klakson (Belanda)." },
  { "en": "Tuts?", "id": "Tombol piano." },
  { "en": "Tutuh?", "id": "Potong (dahan)." },
  { "en": "Tutuk?", "id": "Patuk (arkais)." },
  { "en": "Tutul?", "id": "Bintik." },
  { "en": "Tutung?", "id": "Hangus." },
  { "en": "Tutup?", "id": "Sumbat." },
  { "en": "Tutur?", "id": "Bicara." },
  { "en": "Tuwuhan?", "id": "Hiasan pernikahan Jawa." },
  { "en": "Tuwung?", "id": "Mangkuk (Jawa)." },
  { "en": "Uah?", "id": "Seruan." },
  { "en": "Uak?", "id": "Paman/Bibi (Minang)." },
  { "en": "Uang?", "id": "Duit." },
  { "en": "Uap?", "id": "Gas." },
  { "en": "Uar?", "id": "Umumkan (arkais)." },
  { "en": "Ubah?", "id": "Ganti." },
  { "en": "Uban?", "id": "Rambut putih." },
  { "en": "Ubar?", "id": "Cat." },
  { "en": "Ubat?", "id": "Obat (arkais)." },
  { "en": "Ubel?", "id": "Ikat kepala." },
  { "en": "Uber?", "id": "Kejar." },
  { "en": "Ubi?", "id": "Ketela." },
  { "en": "Ubidah?", "id": "Pemujaan (arkais)." },
  { "en": "Ubin?", "id": "Tegel." },
  { "en": "Ubit?", "id": "Goyang (arkais)." },
  { "en": "Ubled?", "id": "Kental (Jawa)." },
  { "en": "Ubrak-abrik?", "id": "Acak-acak." },
  { "en": "Ubrek?", "id": "Sibuk (Jawa)." },
  { "en": "Ubub?", "id": "Tiup (arkais)." },
  { "en": "Ubun-ubun?", "id": "Bagian kepala." },
  { "en": "Ubur-ubur?", "id": "Hewan laut." },
  { "en": "Ucap?", "id": "Tutur." },
  { "en": "Ucapan?", "id": "Tuturan." },
  { "en": "Ucek?", "id": "Kucek." },
  { "en": "Ucet?", "id": "Seruan." },
  { "en": "Uda?", "id": "Kakak laki-laki (Minang)." },
  { "en": "Udak?", "id": "Aduk." },
  { "en": "Udam?", "id": "Redup." },
  { "en": "Udang?", "id": "Hewan air." },
  { "en": "Udap?", "id": "Urap." },
  { "en": "Udap-udapan?", "id": "Makanan ringan." },
  { "en": "Udar?", "id": "Bubar." },
  { "en": "Udara?", "id": "Hawa." },
  { "en": "Udeh?", "id": "Sudah (Betawi)." },
  { "en": "Udek?", "id": "Aduk (arkais)." },
  { "en": "Udel?", "id": "Pusar (Jawa)." },
  { "en": "Uden?", "id": "Panggilan (arkais)." },
  { "en": "Udeng?", "id": "Ikat kepala." },
  { "en": "Udet?", "id": "Sabuk." },
  { "en": "Udit?", "id": "Paling belakang." },
  { "en": "Udu?", "id": "Wudu (arkais)." },
  { "en": "Uduh?", "id": "Sayur (arkais)." },
  { "en": "Uduk?", "id": "Nasi." },
  { "en": "Ufuk?", "id": "Cakrawala." },
  { "en": "Uga?", "id": "Juga (arkais)." },
  { "en": "Ugahari?", "id": "Sederhana." },
  { "en": "Ugal-ugalan?", "id": "Tidak tertib." },
  { "en": "Ugama?", "id": "Agama (arkais)." },
  { "en": "Ugep?", "id": "Tari (arkais)." },
  { "en": "Ugeran?", "id": "Aturan (Jawa)." },
  { "en": "Ugit?", "id": "Geser (arkais)." },
  { "en": "Uh?", "id": "Seruan." },
  { "en": "Uih?", "id": "Seruan." },
  { "en": "Uis?", "id": "Kain (Batak)." },
  { "en": "Uit?", "id": "Uang (Belanda)." },
  { "en": "Ujah?", "id": "Ujar (arkais)." },
  { "en": "Ujal?", "id": "Paksa (arkais)." },
  { "en": "Ujam?", "id": "Kaji (arkais)." },
  { "en": "Ujang?", "id": "Panggilan anak laki-laki." },
  { "en": "Ujar?", "id": "Kata." },
  { "en": "Uji?", "id": "Tes." },
  { "en": "Ujian?", "id": "Tes." },
  { "en": "Ujub?", "id": "Sombong (Arab)." },
  { "en": "Ujud?", "id": "Wujud." },
  { "en": "Ujung?", "id": "Akhir." },
  { "en": "Ukas?", "id": "Perintah (Rusia)." },
  { "en": "Ukel?", "id": "Sanggul (Jawa)." },
  { "en": "Ukik?", "id": "Tajam (arkais)." },
  { "en": "Ukir?", "id": "Pahat." },
  { "en": "Uktab?", "id": "Oktaf (arkais)." },
  { "en": "Ukulele?", "id": "Alat musik." },
  { "en": "Ukup?", "id": "Dupa." },
  { "en": "Ukur?", "id": "Mengukur." },
  { "en": "Ukuran?", "id": "Dimensi." },
  { "en": "Ula-ula?", "id": "Selendang." },
  { "en": "Ulah?", "id": "Tingkah." },
  { "en": "Ulak?", "id": "Pusaran." },
  { "en": "Ulam?", "id": "Lalapan." },
  { "en": "Ulan?", "id": "Hujan (arkais)." },
  { "en": "Ulang?", "id": "Kembali." },
  { "en": "Ulap?", "id": "Kilau (arkais)." },
  { "en": "Ular?", "id": "Reptil." },
  { "en": "Ulas?", "id": "Kupas." },
  { "en": "Ulat?", "id": "Larva." },
  { "en": "Ulayat?", "id": "Adat." },
  { "en": "Ulek?", "id": "Lumat." },
  { "en": "Ulem?", "id": "Undang (Jawa)." },
  { "en": "Ulen?", "id": "Adon." },
  { "en": "Uler?", "id": "Ulat (Jawa)." },
  { "en": "Ules?", "id": "Warna (Jawa)." },
  { "en": "Ulet?", "id": "Tekun." },
  { "en": "Ulih?", "id": "Oleh (arkais)." },
  { "en": "Ulik?", "id": "Selidik." },
  { "en": "Ulin?", "id": "Kayu." },
  { "en": "Ulir?", "id": "Spiral." },
  { "en": "Ulit?", "id": "Lilit (arkais)." },
  { "en": "Ultima?", "id": "Akhir (Latin)." },
  { "en": "Ultimatum?", "id": "Peringatan terakhir." },
  { "en": "Ultimo?", "id": "Akhir bulan." },
  { "en": "Ultra?", "id": "Sangat." },
  { "en": "Ultrafik?", "id": "Penyaring sangat halus." },
  { "en": "Ultrafilter?", "id": "Saringan." },
  { "en": "Ultramarin?", "id": "Warna biru." },
  { "en": "Ultramikroskopik?", "id": "Sangat kecil." },
  { "en": "Ultramikron?", "id": "Ukuran sangat kecil." },
  { "en": "Ultrasonik?", "id": "Suara frekuensi tinggi." },
  { "en": "Ultrasonografi?", "id": "USG." },
  { "en": "Ultrastruktur?", "id": "Struktur sangat halus." },
  { "en": "Ultratinggi?", "id": "Sangat tinggi." },
  { "en": "Ultraviolet?", "id": "Sinar." },
  { "en": "Ulu?", "id": "Kepala." },
  { "en": "Ulubelalang?", "id": "Hulubalang." },
  { "en": "Ulun?", "id": "Saya (Banjar)." },
  { "en": "Ulung?", "id": "Mahir." },
  { "en": "Ulur?", "id": "Julur." },
  { "en": "Ulus-ulus?", "id": "Bujuk (arkais)." },
  { "en": "Uma?", "id": "Sawah (Bali)." },
  { "en": "Umah?", "id": "Rumah (Jawa)." },
  { "en": "Umai?", "id": "Ibu (Dayak)." },
  { "en": "Umak?", "id": "Ibu (arkais)." },
  { "en": "Umal?", "id": "Lunak (arkais)." },
  { "en": "Umami?", "id": "Rasa gurih." },
  { "en": "Uman?", "id": "Sopan (arkais)." },
  { "en": "Umang-umang?", "id": "Kelomang." },
  { "en": "Umar?", "id": "Nama khalifah." },
  { "en": "Umat?", "id": "Pengikut." },
  { "en": "Umbai?", "id": "Jumbai." },
  { "en": "Umbak?", "id": "Ombak (arkais)." },
  { "en": "Umban?", "id": "Tali pelontar." },
  { "en": "Umbang?", "id": "Terapung." },
  { "en": "Umbar?", "id": "Lepas." },
  { "en": "Umbara?", "id": "Kembara." },
  { "en": "Umbi?", "id": "Akar." },
  { "en": "Umbilikus?", "id": "Pusar." },
  { "en": "Umbra?", "id": "Bayangan inti." },
  { "en": "Umbu?", "id": "Gelar bangsawan Sumba." },
  { "en": "Umbul?", "id": "Mata air." },
  { "en": "Umbut?", "id": "Bagian muda palma." },
  { "en": "Umi?", "id": "Ibu (Arab)." },
  { "en": "Umpak?", "id": "Alas tiang." },
  { "en": "Umpama?", "id": "Misal." },
  { "en": "Umpan?", "id": "Pancingan." },
  { "en": "Umpat?", "id": "Maki." },
  { "en": "Umpel?", "id": "Ingus (Jawa)." },
  { "en": "Umpet?", "id": "Sembunyi." },
  { "en": "Umpil?", "id": "Ungkit." },
  { "en": "Umum?", "id": "Awam." },
  { "en": "Umur?", "id": "Usia." },
  { "en": "Un?", "id": "Dan (Jerman)." },
  { "en": "Unam?", "id": "Pucat (arkais)." },
  { "en": "Uncang?", "id": "Kantong." },
  { "en": "Uncit?", "id": "Buncit (arkais)." },
  { "en": "Unda?", "id": "Ibu (arkais)." },
  { "en": "Undagi?", "id": "Ahli." },
  { "en": "Undak?", "id": "Tingkat." },
  { "en": "Undan?", "id": "Burung." },
  { "en": "Undang?", "id": "Ajak." },
  { "en": "Undar?", "id": "Putar (arkais)." },
  { "en": "Undi?", "id": "Lot." },
  { "en": "Unduh?", "id": "Panen." },
  { "en": "Unduk-unduk?", "id": "Kuda laut." },
  { "en": "Undur?", "id": "Mundur." },
  { "en": "Unga?", "id": "Bunga (arkais)." },
  { "en": "Ungah-angih?", "id": "Terengah-engah." },
  { "en": "Ungam?", "id": "Bisu (arkais)." },
  { "en": "Ungar?", "id": "Ikan." },
  { "en": "Unggas?", "id": "Burung." },
  { "en": "Unggat-anggit?", "id": "Goyang." },
  { "en": "Unggis?", "id": "Gigit." },
  { "en": "Unggit?", "id": "Ungkit." },
  { "en": "Unggul?", "id": "Terbaik." },
  { "en": "Unggun?", "id": "Tumpukan kayu bakar." },
  { "en": "Unggut?", "id": "Angguk." },
  { "en": "Ungka?", "id": "Kera." },
  { "en": "Ungkai?", "id": "Buka." },
  { "en": "Ungkap?", "id": "Beber." },
  { "en": "Ungkapan?", "id": "Frasa." },
  { "en": "Ungkat?", "id": "Ulang." },
  { "en": "Ungkeb?", "id": "Masak." },
  { "en": "Ungkep?", "id": "Ungkeb." },
  { "en": "Ungkil?", "id": "Cungkil." },
  { "en": "Ungkir?", "id": "Ingkar." },
  { "en": "Ungku?", "id": "Panggilan (Melayu)." },
  { "en": "Ungkul?", "id": "Benjol (arkais)." },
  { "en": "Ungkur?", "id": "Belakang (arkais)." },
  { "en": "Ungsi?", "id": "Evakuasi." },
  { "en": "Ungu?", "id": "Warna." },
  { "en": "Uni?", "id": "Kakak perempuan (Minang)." },
  { "en": "Uniat?", "id": "Gereja." },
  { "en": "Unifikasi?", "id": "Penyatuan." },
  { "en": "Unifilar?", "id": "Satu kawat." },
  { "en": "Uniform?", "id": "Seragam." },
  { "en": "Unik?", "id": "Aneh." },
  { "en": "Unilateral?", "id": "Satu pihak." },
  { "en": "Unilin?", "id": "Serat." },
  { "en": "Union?", "id": "Serikat." },
  { "en": "Uniseks?", "id": "Untuk semua jenis kelamin." },
  { "en": "Uniseluler?", "id": "Bersel satu." },
  { "en": "Unisex?", "id": "Uniseks." },
  { "en": "Unisono?", "id": "Suara sama." },
  { "en": "Unit?", "id": "Satuan." },
  { "en": "Unitaris?", "id": "Penganut negara kesatuan." },
  { "en": "Unitarisme?", "id": "Paham negara kesatuan." },
  { "en": "Universal?", "id": "Umum." },
  { "en": "Universalisme?", "id": "Paham universal." },
  { "en": "Universalitas?", "id": "Keumuman." },
  { "en": "Universiade?", "id": "Olimpiade mahasiswa." },
  { "en": "Universitas?", "id": "Perguruan tinggi." },
  { "en": "Universum?", "id": "Alam semesta." },
  { "en": "Unjam?", "id": "Hunjam." },
  { "en": "Unjuk?", "id": "Tunjuk." },
  { "en": "Unjung?", "id": "Kunjung (arkais)." },
  { "en": "Unsur?", "id": "Elemen." },
  { "en": "Unsuri?", "id": "Bersifat unsur." },
  { "en": "Unta?", "id": "Hewan." },
  { "en": "Untai?", "id": "Rangkai." },
  { "en": "Untal?", "id": "Telan." },
  { "en": "Untang-anting?", "id": "Bergoyang." },
  { "en": "Unting?", "id": "Ikat kecil." },
  { "en": "Untuk?", "id": "Bagi." },
  { "en": "Untung?", "id": "Laba." },
  { "en": "Unun?", "id": "Nama unsur (sementara)." },
  { "en": "Upa?", "id": "Nasi (Sanskerta)." },
  { "en": "Upaboga?", "id": "Konsumsi." },
  { "en": "Upacara?", "id": "Seremoni." },
  { "en": "Upaduta?", "id": "Duta pembantu." },
  { "en": "Upah?", "id": "Gaji." },
  { "en": "Upak?", "id": "Kupas (arkais)." },
  { "en": "Upakara?", "id": "Perlengkapan upacara Bali." },
  { "en": "Upakarti?", "id": "Penghargaan." },
  { "en": "Upal?", "id": "Lunak (arkais)." },
  { "en": "Upama?", "id": "Umpama." },
  { "en": "Upanisad?", "id": "Kitab Hindu." },
  { "en": "Upas?", "id": "Racun." },
  { "en": "Upaya?", "id": "Usaha." },
  { "en": "Upet?", "id": "Kotoran telinga." },
  { "en": "Upeti?", "id": "Pajak." },
  { "en": "Upik?", "id": "Panggilan anak perempuan." },
  { "en": "Upsilon?", "id": "Huruf Yunani." },
  { "en": "Urai?", "id": "Jelas." },
  { "en": "Uraian?", "id": "Deskripsi." },
  { "en": "Urak?", "id": "Buka (arkais)." },
  { "en": "Urakan?", "id": "Tidak teratur." },
  { "en": "Uran-uran?", "id": "Nyanyian Jawa." },
  { "en": "Uranium?", "id": "Unsur kimia." },
  { "en": "Uranus?", "id": "Planet." },
  { "en": "Urap?", "id": "Makanan." },
  { "en": "Uras?", "id": "Obat." },
  { "en": "Urat?", "id": "Pembuluh." },
  { "en": "Urban?", "id": "Kota." },
  { "en": "Urbanisasi?", "id": "Perpindahan ke kota." },
  { "en": "Urci?", "id": "Uji (arkais)." },
  { "en": "Urdu?", "id": "Bahasa Pakistan." },
  { "en": "Urea?", "id": "Senyawa." },
  { "en": "Uredium?", "id": "Spora." },
  { "en": "Uremia?", "id": "Penyakit." },
  { "en": "Ureter?", "id": "Saluran kencing." },
  { "en": "Uretra?", "id": "Saluran kencing." },
  { "en": "Uretritis?", "id": "Radang uretra." },
  { "en": "Urgen?", "id": "Mendesak." },
  { "en": "Urgensi?", "id": "Kemendesakan." },
  { "en": "Uri?", "id": "Plasenta." },
  { "en": "Urian?", "id": "Uraian." },
  { "en": "Urik?", "id": "Aduk (arkais)." },
  { "en": "Urim?", "id": "Alat ramal Yahudi." },
  { "en": "Urin?", "id": "Air kencing." },
  { "en": "Urinalisis?", "id": "Analisis urin." },
  { "en": "Uring?", "id": "Bau pesing." },
  { "en": "Urip?", "id": "Hidup (Jawa)." },
  { "en": "Urit?", "id": "Urut." },
  { "en": "Urolog?", "id": "Ahli saluran kemih." },
  { "en": "Urologi?", "id": "Ilmu saluran kemih." },
  { "en": "Urna?", "id": "Wadah abu mayat." },
  { "en": "Urobilin?", "id": "Pigmen empedu." },
  { "en": "Urofagi?", "id": "Makan urin." },
  { "en": "Urogenital?", "id": "Kemih kelamin." },
  { "en": "Urogram?", "id": "Foto saluran kemih." },
  { "en": "Urokinase?", "id": "Enzim." },
  { "en": "Urolitiasis?", "id": "Batu ginjal." },
  { "en": "Uropoda?", "id": "Kaki udang." },
  { "en": "Uroskopi?", "id": "Pemeriksaan urin." },
  { "en": "Ursa?", "id": "Beruang (Latin)." },
  { "en": "Uruf?", "id": "Huruf (Arab)." },
  { "en": "Urung?", "id": "Batal." },
  { "en": "Urus?", "id": "Atur." },
  { "en": "Urusan?", "id": "Perkara." },
  { "en": "Urut?", "id": "Pijat." },
  { "en": "Usada?", "id": "Obat." },
  { "en": "Usah?", "id": "Perlu." },
  { "en": "Usaha?", "id": "Ikhtiar." },
  { "en": "Usai?", "id": "Selesai." },
  { "en": "Usali?", "id": "Niat salat." },
  { "en": "Usam?", "id": "Kusam." },
  { "en": "Usang?", "id": "Lama." },
  { "en": "Usap?", "id": "Sapu." },
  { "en": "Usar?", "id": "Akar wangi." },
  { "en": "Usia?", "id": "Umur." },
  { "en": "Usik?", "id": "Ganggu." },
  { "en": "Usil?", "id": "Jahil." },
  { "en": "Usir?", "id": "Halau." },
  { "en": "Uskup?", "id": "Pemimpin gereja." },
  { "en": "Uslub?", "id": "Gaya bahasa (Arab)." },
  { "en": "Usmal?", "id": "Panas (arkais)." },
  { "en": "Ustaz?", "id": "Guru agama." },
  { "en": "Ustazah?", "id": "Guru agama wanita." },
  { "en": "Usuk?", "id": "Rusuk atap." },
  { "en": "Usul?", "id": "Saran." },
  { "en": "Usuluddin?", "id": "Dasar agama." },
  { "en": "Usung?", "id": "Angkut." },
  { "en": "Usur?", "id": "Tanah milik negara." },
  { "en": "Usus?", "id": "Bagian pencernaan." },
  { "en": "Utama?", "id": "Primer." },
  { "en": "Utang?", "id": "Pinjaman." },
  { "en": "Utara?", "id": "Arah mata angin." },
  { "en": "Utas?", "id": "Benang." },
  { "en": "Uter?", "id": "Kain (arkais)." },
  { "en": "Uterus?", "id": "Rahim." },
  { "en": "Utik?", "id": "Gerak (arkais)." },
  { "en": "Utilitarian?", "id": "Penganut utilitarianisme." },
  { "en": "Utilitarianisme?", "id": "Paham kegunaan." },
  { "en": "Utilitas?", "id": "Kegunaan." },
  { "en": "Utopis?", "id": "Khayal." },
  { "en": "Utuh?", "id": "Lengkap." },
  { "en": "Utsman?", "id": "Nama khalifah." },
  { "en": "Utus?", "id": "Kirim." },
  { "en": "Uvula?", "id": "Anak tekak." },
  { "en": "Uvular?", "id": "Bunyi anak tekak." },
  { "en": "Uwak?", "id": "Paman/Bibi." },
  { "en": "Uwar?", "id": "Umumkan (arkais)." },
  { "en": "Uwel?", "id": "Tidak mau lepas." },
  { "en": "Uwik-uwik?", "id": "Gelisah." },
  { "en": "Uwir-uwir?", "id": "Gantung." },
  { "en": "Uyah?", "id": "Garam (Jawa)." },
  { "en": "Uyuh?", "id": "Kencing (Banjar)." },
  { "en": "V?", "id": "Huruf." },
  { "en": "Vak?", "id": "Bidang (Belanda)." },
  { "en": "Vakansi?", "id": "Liburan." },
  { "en": "Vakbon?", "id": "Serikat pekerja (Belanda)." },
  { "en": "Vaksin?", "id": "Bibit penyakit." },
  { "en": "Vaksinasi?", "id": "Imunisasi." },
  { "en": "Vakum?", "id": "Hampa udara." },
  { "en": "Vakuol?", "id": "Rongga sel." },
  { "en": "Vakuola?", "id": "Vakuol." },
  { "en": "Valas?", "id": "Valuta asing." },
  { "en": "Valensi?", "id": "Derajat gabung atom." },
  { "en": "Valent?", "id": "Pangkat (arkais)." },
  { "en": "Valeriana?", "id": "Tanaman." },
  { "en": "Valet?", "id": "Pelayan pria." },
  { "en": "Valid?", "id": "Sah." },
  { "en": "Validitas?", "id": "Kesahan." },
  { "en": "Valin?", "id": "Asam amino." },
  { "en": "Valis?", "id": "Koper (Belanda)." },
  { "en": "Valium?", "id": "Obat penenang." },
  { "en": "Valor?", "id": "Keberanian (Inggris)." },
  { "en": "Valuta?", "id": "Mata uang." },
  { "en": "Vampir?", "id": "Penghisap darah." },
  { "en": "Van?", "id": "Dari (Belanda)." },
  { "en": "Vanadinit?", "id": "Mineral." },
  { "en": "Vanadium?", "id": "Unsur kimia." },
  { "en": "Vandal?", "id": "Perusak." },
  { "en": "Vandalisme?", "id": "Perusakan." },
  { "en": "Vandel?", "id": "Bendera." },
  { "en": "Vanila?", "id": "Rempah." },
  { "en": "Vanili?", "id": "Vanila." },
  { "en": "Vanilin?", "id": "Zat vanili." },
  { "en": "Varak?", "id": "Penyakit (arkais)." },
  { "en": "Varia?", "id": "Beraneka." },
  { "en": "Variabel?", "id": "Faktor berubah." },
  { "en": "Varian?", "id": "Bentuk berbeda." },
  { "en": "Varians?", "id": "Ukuran statistik." },
  { "en": "Variasi?", "id": "Ragam." },
  { "en": "Varietas?", "id": "Jenis." },
  { "en": "Varikokel?", "id": "Varises testis." },
  { "en": "Varikosis?", "id": "Varises." },
  { "en": "Variola?", "id": "Cacar." },
  { "en": "Varisela?", "id": "Cacar air." },
  { "en": "Varises?", "id": "Pembuluh bengkak." },
  { "en": "Vas?", "id": "Wadah bunga." },
  { "en": "Vasal?", "id": "Bawahan." },
  { "en": "Vasdeferens?", "id": "Saluran sperma." },
  { "en": "Vasektomi?", "id": "Sterilisasi pria." },
  { "en": "Vaselin?", "id": "Lemak." },
  { "en": "Vaskular?", "id": "Pembuluh." },
  { "en": "Vaskularisasi?", "id": "Pembentukan pembuluh." },
  { "en": "Vaskulum?", "id": "Kotak botani." },
  { "en": "Vat?", "id": "Tong besar." },
  { "en": "Vatikan?", "id": "Negara." },
  { "en": "Veda?", "id": "Weda." },
  { "en": "Vedda?", "id": "Suku Sri Lanka." },
  { "en": "Vegeta?", "id": "Sayuran (Spanyol)." },
  { "en": "Vegetaris?", "id": "Pemakan sayuran." },
  { "en": "Vegetarisme?", "id": "Paham vegetaris." },
  { "en": "Vegetasi?", "id": "Tumbuhan." },
  { "en": "Vegetatif?", "id": "Tak kawin." },
  { "en": "Veil?", "id": "Kerudung (Inggris)." },
  { "en": "Velamentum?", "id": "Selubung." },
  { "en": "Velar?", "id": "Bunyi langit-langit lunak." },
  { "en": "Velarisasi?", "id": "Proses velar." },
  { "en": "Veld?", "id": "Padang rumput (Belanda)." },
  { "en": "Velodrom?", "id": "Arena balap sepeda." },
  { "en": "Velosiped?", "id": "Sepeda kuno." },
  { "en": "Velositas?", "id": "Kecepatan." },
  { "en": "Velum?", "id": "Langit-langit lunak." },
  { "en": "Velur?", "id": "Beludru." },
  { "en": "Vena?", "id": "Pembuluh balik." },
  { "en": "Venal?", "id": "Dapat disuap." },
  { "en": "Venalitas?", "id": "Sifat venal." },
  { "en": "Vendeta?", "id": "Balas dendam." },
  { "en": "Vendor?", "id": "Penjual." },
  { "en": "Veneri?", "id": "Penyakit kelamin." },
  { "en": "Venerologi?", "id": "Ilmu penyakit kelamin." },
  { "en": "Venesekasi?", "id": "Pengambilan darah." },
  { "en": "Ventilasi?", "id": "Lubang udara." },
  { "en": "Ventilator?", "id": "Alat ventilasi." },
  { "en": "Ventrikel?", "id": "Bilik jantung." },
  { "en": "Ventrikulus?", "id": "Ventrikel." },
  { "en": "Venus?", "id": "Planet." },
  { "en": "Veracious?", "id": "Jujur (Inggris)." },
  { "en": "Veranda?", "id": "Beranda." },
  { "en": "Verba?", "id": "Kata kerja." },
  { "en": "Verbal?", "id": "Lisan." },
  { "en": "Verbalisan?", "id": "Petugas." },
  { "en": "Verbalisme?", "id": "Pengutamaan kata." },
  { "en": "Verbatim?", "id": "Persis kata." },
  { "en": "Verbasir?", "id": "Rumput." },
  { "en": "Verben?", "id": "Tanaman." },
  { "en": "Verdik?", "id": "Putusan juri." },
  { "en": "Verdigris?", "id": "Karat tembaga." },
  { "en": "Verifikas?", "id": "Pembuktian." },
  { "en": "Verifikatif?", "id": "Membuktikan." },
  { "en": "Verifikator?", "id": "Pembukti." },
  { "en": "Vermes?", "id": "Cacing (Latin)." },
  { "en": "Vermikultur?", "id": "Budidaya cacing." },
  { "en": "Vermiseli?", "id": "Mi." },
  { "en": "Vermisida?", "id": "Pembasmi cacing." },
  { "en": "Vernakular?", "id": "Bahasa daerah." },
  { "en": "Vernier?", "id": "Skala ukur." },
  { "en": "Veronika?", "id": "Tanaman." },
  { "en": "Versi?", "id": "Model." },
  { "en": "Versikuli?", "id": "Sajak pendek." },
  { "en": "Verso?", "id": "Halaman kiri." },
  { "en": "Vertebra?", "id": "Ruas tulang belakang." },
  { "en": "Vertebrata?", "id": "Hewan bertulang belakang." },
  { "en": "Vertikal?", "id": "Tegak." },
  { "en": "Vertigo?", "id": "Pusing." },
  { "en": "Verzet?", "id": "Perlawanan (Belanda)." },
  { "en": "Vesikel?", "id": "Kantung kecil." },
  { "en": "Vesikula?", "id": "Vesikel." },
  { "en": "Vesper?", "id": "Ibadah sore." },
  { "en": "Vest?", "id": "Rompi." },
  { "en": "Vesta?", "id": "Dewi Romawi." },
  { "en": "Vestibul?", "id": "Serambi." },
  { "en": "Vestibulum?", "id": "Vestibul." },
  { "en": "Veteran?", "id": "Pensiunan tentara." },
  { "en": "Veteriner?", "id": "Kedokteran hewan." },
  { "en": "Veto?", "id": "Hak batal." },
  { "en": "Vetsin?", "id": "MSG." },
  { "en": "Via?", "id": "Melalui." },
  { "en": "Viabel?", "id": "Layak hidup." },
  { "en": "Viabilitas?", "id": "Kelayakan hidup." },
  { "en": "Viaduk?", "id": "Jembatan." },
  { "en": "Viatikum?", "id": "Sakramen." },
  { "en": "Vibran?", "id": "Bergetar." },
  { "en": "Vibrasi?", "id": "Getaran." },
  { "en": "Vibrator?", "id": "Penggetar." },
  { "en": "Vibrio?", "id": "Bakteri." },
  { "en": "Video?", "id": "Rekaman gambar." },
  { "en": "Videofon?", "id": "Telepon video." },
  { "en": "Videoklip?", "id": "Klip video." },
  { "en": "Videoteks?", "id": "Layanan informasi." },
  { "en": "Vigia?", "id": "Peringatan bahaya laut." },
  { "en": "Vigilansia?", "id": "Kewaspadaan." },
  { "en": "Vignyet?", "id": "Hiasan gambar." },
  { "en": "Vihara?", "id": "Kuil Buddha." },
  { "en": "Viking?", "id": "Bangsa Skandinavia." },
  { "en": "Vila?", "id": "Rumah peristirahatan." },
  { "en": "Vinil?", "id": "Plastik." },
  { "en": "Vinyet?", "id": "Vignyet." },
  { "en": "Viola?", "id": "Biola." },
  { "en": "Violces?", "id": "Alat musik." },
  { "en": "Violin?", "id": "Biola." },
  { "en": "Violinis?", "id": "Pemain biola." },
  { "en": "Viper?", "id": "Ular berbisa." },
  { "en": "Virga?", "id": "Jalur hujan." },
  { "en": "Virgin?", "id": "Perawan (Inggris)." },
  { "en": "Virgo?", "id": "Rasi bintang." },
  { "en": "Virilis?", "id": "Kejantanan." },
  { "en": "Virilisme?", "id": "Sifat jantan (wanita)." },
  { "en": "Virilitas?", "id": "Kejantanan." },
  { "en": "Virolog?", "id": "Ahli virus." },
  { "en": "Virologi?", "id": "Ilmu virus." },
  { "en": "Virtual?", "id": "Maya." },
  { "en": "Virtuoso?", "id": "Ahli musik." },
  { "en": "Virulen?", "id": "Ganas." },
  { "en": "Virulensi?", "id": "Kegansan." },
  { "en": "Virus?", "id": "Kuman." },
  { "en": "Visa?", "id": "Izin masuk negara." },
  { "en": "Visera?", "id": "Jeroan." },
  { "en": "Visi?", "id": "Pandangan." },
  { "en": "Visibel?", "id": "Terlihat." },
  { "en": "Visibilitas?", "id": "Jarak pandang." },
  { "en": "Visioner?", "id": "Berpandangan jauh." },
  { "en": "Visitasi?", "id": "Kunjungan." },
  { "en": "Visitor?", "id": "Pengunjung." },
  { "en": "Viskometer?", "id": "Pengukur kekentalan." },
  { "en": "Viskositas?", "id": "Kekentalan." },
  { "en": "Viskus?", "id": "Kental." },
  { "en": "Visual?", "id": "Tampak." },
  { "en": "Visualisasi?", "id": "Penggambaran." },
  { "en": "Vitak?", "id": "Pintar (arkais)." },
  { "en": "Vital?", "id": "Penting." },
  { "en": "Vitalia?", "id": "Kebutuhan hidup." },
  { "en": "Vitalitas?", "id": "Daya hidup." },
  { "en": "Vitamin?", "id": "Zat gizi." },
  { "en": "Vitiligo?", "id": "Penyakit kulit." },
  { "en": "Vitreositas?", "id": "Sifat kaca." },
  { "en": "Vitrifikasi?", "id": "Pengkacaan." },
  { "en": "Vitriol?", "id": "Asam sulfat." },
  { "en": "Vivarium?", "id": "Kandang hewan." },
  { "en": "Vivifikasi?", "id": "Menghidupkan." },
  { "en": "Vivipar?", "id": "Melahirkan." },
  { "en": "Viviseksi?", "id": "Pembedahan hewan hidup." },
  { "en": "Voal?", "id": "Kain." },
  { "en": "Vokabulari?", "id": "Kosakata." },
  { "en": "Vokabuler?", "id": "Vokabulari." },
  { "en": "Vokabularium?", "id": "Kamus." },
  { "en": "Vokal?", "id": "Bunyi hidup." },
  { "en": "Vokalia?", "id": "Paduan suara." },
  { "en": "Vokalis?", "id": "Penyanyi." },
  { "en": "Vokasional?", "id": "Keterampilan." },
  { "en": "Vokatif?", "id": "Seruan." },
  { "en": "Voli?", "id": "Olahraga." },
  { "en": "Volt?", "id": "Satuan tegangan." },
  { "en": "Voltameter?", "id": "Pengukur listrik." },
  { "en": "Volume?", "id": "Isi." },
  { "en": "Volumetri?", "id": "Pengukuran isi." },
  { "en": "Volumometer?", "id": "Pengukur volume." },
  { "en": "Volunter?", "id": "Sukarelawan." },
  { "en": "Vonis?", "id": "Putusan." },
  { "en": "Vopo?", "id": "Polisi Jerman Timur." },
  { "en": "Vorasitas?", "id": "Kerakusan." },
  { "en": "Vorteks?", "id": "Pusaran." },
  { "en": "Votal?", "id": "Suara (arkais)." },
  { "en": "Votif?", "id": "Nazar." },
  { "en": "Votum?", "id": "Suara (pemilihan)." },
  { "en": "Voyager?", "id": "Pengembara (Prancis)." },
  { "en": "Vulgar?", "id": "Kasar." },
  { "en": "Vulgarisasi?", "id": "Pengasaran." },
  { "en": "Vulgarisme?", "id": "Kata kasar." },
  { "en": "Vulkan?", "id": "Gunung berapi." },
  { "en": "Vulkanis?", "id": "Bersifat vulkan." },
  { "en": "Vulkanisasi?", "id": "Proses karet." },
  { "en": "Vulkanolog?", "id": "Ahli gunung berapi." },
  { "en": "Vulkanologi?", "id": "Ilmu gunung berapi." },
  { "en": "Vulpinit?", "id": "Mineral." },
  { "en": "Vulva?", "id": "Kelamin wanita luar." },
  { "en": "Vulvektomi?", "id": "Operasi." },
  { "en": "Vulvitis?", "id": "Radang vulva." },
  { "en": "Vulvovaginitis?", "id": "Radang." },
  { "en": "Wa?", "id": "Dan (Arab)." },
  { "en": "Waad?", "id": "Janji (Arab)." },
  { "en": "Waadah?", "id": "Lembah (Arab)." },
  { "en": "Wabah?", "id": "Epidemi." },
  { "en": "Wacana?", "id": "Pembicaraan." },
  { "en": "Wadah?", "id": "Tempat." },
  { "en": "Wadak?", "id": "Badan (arkais)." },
  { "en": "Wadal?", "id": "Tumbal." },
  { "en": "Wadam?", "id": "Waria." },
  { "en": "Wadat?", "id": "Tidak kawin." },
  { "en": "Wade?", "id": "Ibu (arkais)." },
  { "en": "Waduh?", "id": "Aduh." },
  { "en": "Waduk?", "id": "Bendungan." },
  { "en": "Wadung?", "id": "Kapak." },
  { "en": "Wafa?", "id": "Setia (Arab)." },
  { "en": "Wafat?", "id": "Meninggal." },
  { "en": "Wafel?", "id": "Kue." },
  { "en": "Wafiat?", "id": "Kematian (Arab)." },
  { "en": "Wafi?", "id": "Sempurna (Arab)." },
  { "en": "Wagen?", "id": "Gerobak (Belanda)." },
  { "en": "Wagon?", "id": "Gerbong." },
  { "en": "Wah?", "id": "Seruan." },
  { "en": "Wahah?", "id": "Oasis." },
  { "en": "Wahad?", "id": "Satu (Arab)." },
  { "en": "Wahadat?", "id": "Keesaan (Arab)." },
  { "en": "Wahadaniah?", "id": "Keesaan Tuhan." },
  { "en": "Wahai?", "id": "Seruan." },
  { "en": "Waham?", "id": "Keyakinan salah." },
  { "en": "Wahana?", "id": "Sarana." },
  { "en": "Wahib?", "id": "Pemberi (Arab)." },
  { "en": "Wahid?", "id": "Tunggal (Arab)." },
  { "en": "Wahyu?", "id": "Petunjuk Tuhan." },
  { "en": "Wai?", "id": "Air (Lampung)." },
  { "en": "Wain?", "id": "Anggur (Belanda)." },
  { "en": "Waisak?", "id": "Hari raya Buddha." },
  { "en": "Waisya?", "id": "Kasta Hindu." },
  { "en": "Waitankung?", "id": "Senam." },
  { "en": "Waja?", "id": "Baja." },
  { "en": "Wajah?", "id": "Muka." },
  { "en": "Wajan?", "id": "Alat masak." },
  { "en": "Wajar?", "id": "Biasa." },
  { "en": "Wajib?", "id": "Harus." },
  { "en": "Wajik?", "id": "Kue." },
  { "en": "Wak?", "id": "Panggilan (Melayu)." },
  { "en": "Wakaf?", "id": "Sumbangan amal." },
  { "en": "Wakif?", "id": "Pemberi wakaf." },
  { "en": "Wakil?", "id": "Pengganti." },
  { "en": "Wakuncar?", "id": "Wawancara (prokem)." },
  { "en": "Waktu?", "id": "Tempo." },
  { "en": "Walabi?", "id": "Kanguru kecil." },
  { "en": "Walad?", "id": "Anak (Arab)." },
  { "en": "Walafiat?", "id": "Sehat." },
  { "en": "Walak?", "id": "Batas (arkais)." },
  { "en": "Walakhir?", "id": "Akhirnya (Arab)." },
  { "en": "Walang?", "id": "Belalang (Jawa)." },
  { "en": "Walangkopo?", "id": "Serangga." },
  { "en": "Walangsangit?", "id": "Serangga." },
  { "en": "Walapun?", "id": "Walaupun." },
  { "en": "Walat?", "id": "Kualat." },
  { "en": "Walau?", "id": "Meskipun." },
  { "en": "Walaupun?", "id": "Meskipun." },
  { "en": "Waleh?", "id": "Jelas (Jawa)." },
  { "en": "Walet?", "id": "Burung." },
  { "en": "Wali?", "id": "Pelindung." },
  { "en": "Walikota?", "id": "Kepala kota." },
  { "en": "Walimah?", "id": "Pesta nikah." },
  { "en": "Walingi?", "id": "Padi." },
  { "en": "Walirah?", "id": "Uang (arkais)." },
  { "en": "Walisongo?", "id": "Sembilan wali." },
  { "en": "Walki-talki?", "id": "Alat komunikasi." },
  { "en": "Wallah?", "id": "Demi Allah." },
  { "en": "Wallahualam?", "id": "Allah lebih tahu." },
  { "en": "Waluh?", "id": "Labu." },
  { "en": "Walsa?", "id": "Tarian." },
  { "en": "Waluh?", "id": "Labu (Jawa)." },
  { "en": "Wambik?", "id": "Kelelawar." },
  { "en": "Wan?", "id": "Gelar (Melayu)." },
  { "en": "Wana?", "id": "Hutan." },
  { "en": "Wanara?", "id": "Kera (Sanskerta)." },
  { "en": "Wanawisata?", "id": "Wisata hutan." },
  { "en": "Wanda?", "id": "Tubuh (Jawa)." },
  { "en": "Wang?", "id": "Uang (arkais)." },
  { "en": "Wangi?", "id": "Harum." },
  { "en": "Wangkang?", "id": "Kapal Tiongkok." },
  { "en": "Wangsa?", "id": "Dinasti." },
  { "en": "Wangsit?", "id": "Ilham." },
  { "en": "Wanita?", "id": "Perempuan." },
  { "en": "Wantah?", "id": "Biasa (Jawa)." },
  { "en": "Wantek?", "id": "Pewarna kain." },
  { "en": "Wantilan?", "id": "Balai (Bali)." },
  { "en": "Wanti-wanti?", "id": "Peringatan." },
  { "en": "Wap?", "id": "Uap (arkais)." },
  { "en": "Wapres?", "id": "Wakil presiden." },
  { "en": "War?", "id": "Surat (Belanda)." },
  { "en": "Wara?", "id": "Umumkan." },
  { "en": "Warak?", "id": "Badak (Jawa)." },
  { "en": "Waralaba?", "id": "Franchise." },
  { "en": "Warangan?", "id": "Racun arsenik." },
  { "en": "Waranggana?", "id": "Penyanyi gamelan." },
  { "en": "Warangka?", "id": "Sarung keris." },
  { "en": "Waras?", "id": "Sehat." },
  { "en": "Warasah?", "id": "Pewaris (Arab)." },
  { "en": "Warda?", "id": "Bunga mawar (Arab)." },
  { "en": "Wardah?", "id": "Warda." },
  { "en": "Wardana?", "id": "Kesucian (Jawa)." },
  { "en": "Warga?", "id": "Penduduk." },
  { "en": "Wargi?", "id": "Keluarga (Sunda)." },
  { "en": "Waria?", "id": "Wanita pria." },
  { "en": "Waringin?", "id": "Beringin." },
  { "en": "Waris?", "id": "Penerima pusaka." },
  { "en": "Warisan?", "id": "Pusaka." },
  { "en": "Warita?", "id": "Berita." },
  { "en": "Warkah?", "id": "Surat." },
  { "en": "Warkat?", "id": "Surat." },
  { "en": "Warna?", "id": "Rona." },
  { "en": "Warna-warni?", "id": "Beragam." },
  { "en": "Warok?", "id": "Tokoh reog." },
  { "en": "Warsa?", "id": "Tahun." },
  { "en": "Warsiki?", "id": "Wangi (Jawa)." },
  { "en": "Warta?", "id": "Berita." },
  { "en": "Wartawan?", "id": "Jurnalis." },
  { "en": "Wartawati?", "id": "Jurnalis wanita." },
  { "en": "Waru?", "id": "Pohon." },
  { "en": "Waruga?", "id": "Kubur batu Minahasa." },
  { "en": "Waruna?", "id": "Dewa laut." },
  { "en": "Warung?", "id": "Kedai." },
  { "en": "Wasak?", "id": "Muatan (Arab)." },
  { "en": "Wasal?", "id": "Tanda sambung Arab." },
  { "en": "Wasan?", "id": "Akhir (arkais)." },
  { "en": "Wasangka?", "id": "Prasangka." },
  { "en": "Wasat?", "id": "Tengah (Arab)." },
  { "en": "Waser?", "id": "Air (Belanda)." },
  { "en": "Wasi?", "id": "Pelaksana wasiat." },
  { "en": "Wasiah?", "id": "Wasiat." },
  { "en": "Wasiat?", "id": "Pesan terakhir." },
  { "en": "Wasil?", "id": "Sampai (Arab)." },
  { "en": "Wasilah?", "id": "Perantara (Arab)." },
  { "en": "Wasir?", "id": "Ambeien." },
  { "en": "Wasit?", "id": "Juri." },
  { "en": "Waskat?", "id": "Pengawasan melekat." },
  { "en": "Waskita?", "id": "Awas." },
  { "en": "Waspada?", "id": "Siaga." },
  { "en": "Wastafel?", "id": "Tempat cuci tangan." },
  { "en": "Waswas?", "id": "Khawatir." },
  { "en": "Wat?", "id": "Satuan daya." },
  { "en": "Watak?", "id": "Karakter." },
  { "en": "Watan?", "id": "Tanah air (Arab)." },
  { "en": "Watas?", "id": "Batas." },
  { "en": "Water?", "id": "Air (Inggris)." },
  { "en": "Watermantel?", "id": "Jas hujan (Belanda)." },
  { "en": "Waterpas?", "id": "Alat ukur datar." },
  { "en": "Waterpruf?", "id": "Tahan air." },
  { "en": "Watt?", "id": "Satuan daya." },
  { "en": "Wau?", "id": "Layang-layang." },
  { "en": "Wawa?", "id": "Kera." },
  { "en": "Wawancara?", "id": "Interviu." },
  { "en": "Wawanmuka?", "id": "Wawancara." },
  { "en": "Wawanrembuk?", "id": "Diskusi." },
  { "en": "Wawas?", "id": "Paham (arkais)." },
  { "en": "Wawasan?", "id": "Pandangan." },
  { "en": "Wawu?", "id": "Kera." },
  { "en": "Wayang?", "id": "Boneka kulit." },
  { "en": "Wazir?", "id": "Menteri." },
  { "en": "WCC?", "id": "Dewan Gereja Sedunia." },
  { "en": "WC?", "id": "Toilet." },
  { "en": "Weda?", "id": "Kitab suci Hindu." },
  { "en": "Wedam?", "id": "Ilmu (Jawa)." },
  { "en": "Wedana?", "id": "Pejabat." },
  { "en": "Wedani?", "id": "Istri wedana." },
  { "en": "Wedang?", "id": "Minuman hangat (Jawa)." },
  { "en": "Wedel?", "id": "Baji." },
  { "en": "Weduk?", "id": "Bedak (Jawa)." },
  { "en": "Wek?", "id": "Jam weker (Belanda)." },
  { "en": "Weker?", "id": "Jam beker." },
  { "en": "Weksel?", "id": "Surat bayar." },
  { "en": "Welas?", "id": "Kasih (Jawa)." },
  { "en": "Welirang?", "id": "Belerang." },
  { "en": "Welit?", "id": "Atap." },
  { "en": "Welter?", "id": "Kelas tinju." },
  { "en": "Welve?", "id": "Katup (Belanda)." },
  { "en": "Wenang?", "id": "Hak." },
  { "en": "Wenter?", "id": "Pewarna (Belanda)." },
  { "en": "Werak?", "id": "Babi hutan (Jawa)." },
  { "en": "Werda?", "id": "Tua (Jawa)." },
  { "en": "Werdha?", "id": "Werda." },
  { "en": "Werek?", "id": "Anjing hutan." },
  { "en": "Wereng?", "id": "Hama padi." },
  { "en": "Werit?", "id": "Sulit (Jawa)." },
  { "en": "Wesel?", "id": "Surat pos." },
  { "en": "Weselbor?", "id": "Papan wesel." },
  { "en": "Westernisasi?", "id": "Pembaratan." },
  { "en": "Western?", "id": "Barat." },
  { "en": "Wetan?", "id": "Timur (Jawa)." },
  { "en": "Wet?", "id": "Undang-undang (Belanda)." },
  { "en": "Wewarah?", "id": "Ajaran (Jawa)." },
  { "en": "Wewaton?", "id": "Dasar (Jawa)." },
  { "en": "Wibawa?", "id": "Karisma." },
  { "en": "Wicaksana?", "id": "Bijaksana (Sanskerta)." },
  { "en": "Wicara?", "id": "Bicara." },
  { "en": "Widara?", "id": "Pohon." },
  { "en": "Widi?", "id": "Izin Tuhan (Jawa)." },
  { "en": "Widiaiswara?", "id": "Pendidik." },
  { "en": "Widiwasa?", "id": "Tuhan (Bali)." },
  { "en": "Widodari?", "id": "Bidadari." },
  { "en": "Widuri?", "id": "Batu permata." },
  { "en": "Wig?", "id": "Rambut palsu (Inggris)." },
  { "en": "Wigata?", "id": "Terkejut (arkais)." },
  { "en": "Wihara?", "id": "Vihara." },
  { "en": "Wijaya?", "id": "Kemenangan (Sanskerta)." },
  { "en": "Wijayakusuma?", "id": "Bunga." },
  { "en": "Wijayamala?", "id": "Batu." },
  { "en": "Wijayamulia?", "id": "Kemenangan mulia." },
  { "en": "Wijen?", "id": "Tanaman." },
  { "en": "Wikalat?", "id": "Perwakilan (Arab)." },
  { "en": "Wiku?", "id": "Pendeta Buddha." },
  { "en": "Wiladah?", "id": "Kelahiran (Arab)." },
  { "en": "Wilahar?", "id": "Danau (arkais)." },
  { "en": "Wilajah?", "id": "Daerah (arkais)." },
  { "en": "Wilalat?", "id": "Ulat." },
  { "en": "Wilang?", "id": "Hitung (arkais)." },
  { "en": "Wilayah?", "id": "Daerah." },
  { "en": "Wiled?", "id": "Lagu (Jawa)." },
  { "en": "Wiler?", "id": "Roda (Belanda)." },
  { "en": "Wili?", "id": "Hantu wanita." },
  { "en": "Wira?", "id": "Pahlawan." },
  { "en": "Wiracarita?", "id": "Epos." },
  { "en": "Wiraga?", "id": "Gerak tari." },
  { "en": "Wirahma?", "id": "Irama." },
  { "en": "Wirang?", "id": "Malu (Jawa)." },
  { "en": "Wiraniaga?", "id": "Penjual." },
  { "en": "Wirausaha?", "id": "Pengusaha." },
  { "en": "Wiraswasta?", "id": "Wirausaha." },
  { "en": "Wirid?", "id": "Zikir." },
  { "en": "Wisa?", "id": "Bisa." },
  { "en": "Wisal?", "id": "Persetubuhan (Arab)." },
  { "en": "Wisata?", "id": "Pelancongan." },
  { "en": "Wisatawan?", "id": "Turis." },
  { "en": "Wisaya?", "id": "Daerah (arkais)." },
  { "en": "Wiski?", "id": "Minuman keras." },
  { "en": "Wisma?", "id": "Rumah." },
  { "en": "Wisnu?", "id": "Dewa Hindu." },
  { "en": "Wisuda?", "id": "Upacara kelulusan." },
  { "en": "Wit?", "id": "Pohon (Jawa)." },
  { "en": "Wiwaha?", "id": "Pernikahan (Sanskerta)." },
  { "en": "Wiweka?", "id": "Bijaksana (Jawa)." },
  { "en": "Wiyaga?", "id": "Penabuh gamelan." },
  { "en": "Wiyata?", "id": "Pendidikan." },
  { "en": "Wolfram?", "id": "Tungsten." },
  { "en": "Wol?", "id": "Bulu domba." },
  { "en": "Won?", "id": "Mata uang Korea." },
  { "en": "Wora-wari?", "id": "Bunga." },
  { "en": "Wortel?", "id": "Sayuran." },
  { "en": "Wosi?", "id": "Paman/Bibi (Papua)." },
  { "en": "Wot?", "id": "Jembatan (Jawa)." },
  { "en": "Wreda?", "id": "Tua." },
  { "en": "Wredatama?", "id": "Pensiunan." },
  { "en": "Wregu?", "id": "Anjing hutan." },
  { "en": "Wrisaba?", "id": "Lembu jantan (Sanskerta)." },
  { "en": "Wu?", "id": "Tidak ada (Tionghoa)." },
  { "en": "Wudu?", "id": "Bersuci." },
  { "en": "Wujud?", "id": "Bentuk." },
  { "en": "Wuker?", "id": "Perangkap (arkais)." },
  { "en": "Wuku?", "id": "Minggu (penanggalan Jawa)." },
  { "en": "Wulan?", "id": "Bulan (Jawa)." },
  { "en": "Wulang?", "id": "Ajar (Jawa)." },
  { "en": "Wulung?", "id": "Hitam." },
  { "en": "Wungkal?", "id": "Batu asah." },
  { "en": "Wungu?", "id": "Ungu (Jawa)." },
  { "en": "Wuwu?", "id": "Perangkap ikan." },
  { "en": "X?", "id": "Huruf." },
  { "en": "Xantat?", "id": "Garam." },
  { "en": "Xantena?", "id": "Senyawa." },
  { "en": "Xantofil?", "id": "Pigmen kuning." },
  { "en": "Xenia?", "id": "Pengaruh serbuk sari." },
  { "en": "Xenofili?", "id": "Suka orang asing." },
  { "en": "Xenofobia?", "id": "Takut orang asing." },
  { "en": "Xenograf?", "id": "Tulisan asing." },
  { "en": "Xenokrasi?", "id": "Pemerintahan asing." },
  { "en": "Xenolit?", "id": "Batuan." },
  { "en": "Xenon?", "id": "Unsur gas." },
  { "en": "Xenotime?", "id": "Mineral." },
  { "en": "Xeroftalmia?", "id": "Mata kering." },
  { "en": "Xerofil?", "id": "Suka kering." },
  { "en": "Xerofit?", "id": "Tumbuhan kering." },
  { "en": "Xerografi?", "id": "Fotokopi." },
  { "en": "Xerosis?", "id": "Kulit kering." },
  { "en": "Xilem?", "id": "Jaringan tumbuhan." },
  { "en": "Xilofag?", "id": "Pemakan kayu." },
  { "en": "Xilofon?", "id": "Alat musik." },
  { "en": "Xilograf?", "id": "Ukiran kayu." },
  { "en": "Xilografi?", "id": "Seni ukir kayu." },
  { "en": "Xiloid?", "id": "Seperti kayu." },
  { "en": "Xiloidina?", "id": "Bahan peledak." },
  { "en": "Xilol?", "id": "Pelarut." },
  { "en": "Xilologi?", "id": "Ilmu kayu." },
  { "en": "Xilosa?", "id": "Gula." },
  { "en": "Xylo?", "id": "Xilo (ejaan alternatif)." },
  { "en": "Ya?", "id": "Iya." },
  { "en": "Yacht?", "id": "Kapal pesiar (Inggris)." },
  { "en": "Yahudi?", "id": "Agama/Bangsa." },
  { "en": "Yahudiah?", "id": "Wanita Yahudi." },
  { "en": "Yahwe?", "id": "Tuhan (Yahudi)." },
  { "en": "Yais?", "id": "Putus asa (Arab)." },
  { "en": "Yaitu?", "id": "Yakni." },
  { "en": "Yakin?", "id": "Percaya." },
  { "en": "Yakis?", "id": "Monyet (arkais)." },
  { "en": "Yakitori?", "id": "Sate Jepang." },
  { "en": "Yakjuj?", "id": "Bangsa perusak." },
  { "en": "Yakni?", "id": "Yaitu." },
  { "en": "Yaksa?", "id": "Raksasa." },
  { "en": "Yakut?", "id": "Batu permata." },
  { "en": "Yam?", "id": "Laut (Ibrani)." },
  { "en": "Yamadipati?", "id": "Dewa maut." },
  { "en": "Yamin?", "id": "Kanan (Arab)." },
  { "en": "Yamtuan?", "id": "Yang Dipertuan." },
  { "en": "Yan?", "id": "Yang (arkais)." },
  { "en": "Yanda?", "id": "Ayahanda." },
  { "en": "Yang?", "id": "Kata hubung." },
  { "en": "Yantra?", "id": "Diagram mistik." },
  { "en": "Yap?", "id": "Ya (prokem)." },
  { "en": "Yard?", "id": "Satuan panjang." },
  { "en": "Yarda?", "id": "Yard." },
  { "en": "Yasin?", "id": "Surat Al-Quran." },
  { "en": "Yasmin?", "id": "Bunga melati." },
  { "en": "Yasti?", "id": "Sumbu (Sanskerta)." },
  { "en": "Yatim?", "id": "Tidak berayah." },
  { "en": "Yati?", "id": "Pendeta (arkais)." },
  { "en": "Yatyalina?", "id": "Kiamat (arkais)." },
  { "en": "Yaum?", "id": "Hari (Arab)." },
  { "en": "Yaumiah?", "id": "Harian." },
  { "en": "Yaumidin?", "id": "Hari kiamat." },
  { "en": "Yayasan?", "id": "Lembaga." },
  { "en": "Yayi?", "id": "Adik (Jawa)." },
  { "en": "Yel?", "id": "Sorak." },
  { "en": "Yelo?", "id": "Sorak (arkais)." },
  { "en": "Yen?", "id": "Mata uang Jepang." },
  { "en": "Yeyunum?", "id": "Usus kosong." },
  { "en": "Yodium?", "id": "Unsur kimia." },
  { "en": "Yoga?", "id": "Latihan fisik mental." },
  { "en": "Yoghurt?", "id": "Susu fermentasi." },
  { "en": "Yogi?", "id": "Praktisi yoga." },
  { "en": "Yohimbina?", "id": "Alkaloid." },
  { "en": "Yojana?", "id": "Ukuran jarak (India)." },
  { "en": "Yoke?", "id": "Kuk (Inggris)." },
  { "en": "Yolk?", "id": "Kuning telur (Inggris)." },
  { "en": "Yoni?", "id": "Simbol kewanitaan." },
  { "en": "Yos?", "id": "Seruan." },
  { "en": "Yos!", "id": "Ya (prokem)." },
  { "en": "Yos!", "id": "Setuju." },
  { "en": "Yos!", "id": "Ayo." },
  { "en": "Yot?", "id": "Sangat kecil (arkais)." },
  { "en": "Yota?", "id": "Awalan (sangat besar)." },
  { "en": "Yoyo?", "id": "Mainan." },
  { "en": "Yu?", "id": "Kakak perempuan (Jawa)." },
  { "en": "Yuan?", "id": "Mata uang Tiongkok." },
  { "en": "Yuana?", "id": "Muda (Sanskerta)." },
  { "en": "Yuda?", "id": "Perang (Sanskerta)." },
  { "en": "Yuddha?", "id": "Yuda." },
  { "en": "Yudikatif?", "id": "Kekuasaan kehakiman." },
  { "en": "Yudisial?", "id": "Kehakiman." },
  { "en": "Yudisium?", "id": "Penentuan kelulusan." },
  { "en": "Yudo?", "id": "Judo." },
  { "en": "Yudoka?", "id": "Pemain yudo." },
  { "en": "Yufu?", "id": "Pakaian (Jepang)." },
  { "en": "Yugo?", "id": "Kuk (Slavia)." },
  { "en": "Yugoperdana?", "id": "Pertama (arkais)." },
  { "en": "Yuguru?", "id": "Guru (arkais)." },
  { "en": "Yuh?", "id": "Seruan." },
  { "en": "Yuhu?", "id": "Seruan." },
  { "en": "Yuyitsu?", "id": "Jujutsu." },
  { "en": "Yuyu?", "id": "Kepiting." },
  { "en": "Yuvinal?", "id": "Muda." },
  { "en": "Yuz?", "id": "Bagian (Arab)." },
  { "en": "Z?", "id": "Huruf." },
  { "en": "Zabaniah?", "id": "Malaikat neraka." },
  { "en": "Zabarjad?", "id": "Batu permata." },
  { "en": "Zabit?", "id": "Perwira (Arab)." },
  { "en": "Zabur?", "id": "Kitab Nabi Daud." },
  { "en": "Zad?", "id": "Bekal (Arab)." },
  { "en": "Zadah?", "id": "Anak haram." },
  { "en": "Zahid?", "id": "Pertapa (Arab)." },
  { "en": "Zahir?", "id": "Lahiriah (Arab)." },
  { "en": "Zai?", "id": "Huruf Arab." },
  { "en": "Zaibatsu?", "id": "Konglomerat Jepang." },
  { "en": "Zaim?", "id": "Pemimpin (Arab)." },
  { "en": "Zair?", "id": "Mengunjungi (Arab)." },
  { "en": "Zaitun?", "id": "Buah." },
  { "en": "Zajal?", "id": "Puisi Arab." },
  { "en": "Zakar?", "id": "Penis." },
  { "en": "Zakat?", "id": "Sumbangan wajib Islam." },
  { "en": "Zakelek?", "id": "Bisnis (Belanda)." },
  { "en": "Zakiah?", "id": "Suci (Arab)." },
  { "en": "Zal?", "id": "Huruf Arab." },
  { "en": "Zalim?", "id": "Kejam." },
  { "en": "Zaman?", "id": "Era." },
  { "en": "Zamindar?", "id": "Tuan tanah India." },
  { "en": "Zamindar?", "id": "Pemungut pajak." },
  { "en": "Zamzam?", "id": "Air suci Mekah." },
  { "en": "Zan?", "id": "Sangkaan (Arab)." },
  { "en": "Zanji?", "id": "Orang Negro (Arab)." },
  { "en": "Zantafil?", "id": "Pigmen." },
  { "en": "Zapin?", "id": "Tarian Melayu." },
  { "en": "Zar?", "id": "Benang emas (Parsi)." },
  { "en": "Zarah?", "id": "Partikel." },
  { "en": "Zaratit?", "id": "Mineral." },
  { "en": "Zaratustra?", "id": "Nabi Persia." },
  { "en": "Zariah?", "id": "Keturunan (Arab)." },
  { "en": "Zariat?", "id": "Keturunan." },
  { "en": "Zarzuela?", "id": "Drama musik Spanyol." },
  { "en": "Zat?", "id": "Substansi." },
  { "en": "Zawal?", "id": "Tengah hari (Arab)." },
  { "en": "Zawiat?", "id": "Sudut (Arab)." },
  { "en": "Zebra?", "id": "Kuda belang." },
  { "en": "Zebu?", "id": "Sapi India." },
  { "en": "Zeisme?", "id": "Gaya bahasa." },
  { "en": "Zel?", "id": "Nama ikan." },
  { "en": "Zelot?", "id": "Orang fanatik." },
  { "en": "Zen?", "id": "Aliran Buddha." },
  { "en": "Zend?", "id": "Bahasa kuno Iran." },
  { "en": "Zend-Avesta?", "id": "Kitab suci Zoroaster." },
  { "en": "Zenith?", "id": "Puncak." },
  { "en": "Zeolit?", "id": "Mineral." },
  { "en": "Zeop?", "id": "Pucat (arkais)." },
  { "en": "Zepelin?", "id": "Balon udara." },
  { "en": "Zepi?", "id": "Nama ikan." },
  { "en": "Zero?", "id": "Nol (Inggris)." },
  { "en": "Zetan?", "id": "Setan (arkais)." },
  { "en": "Zetawatt?", "id": "Satuan daya." },
  { "en": "Zig-zag?", "id": "Berkelok-kelok." },
  { "en": "Zigomorf?", "id": "Simetri bilateral." },
  { "en": "Zigot?", "id": "Sel telur dibuahi." },
  { "en": "Zihar?", "id": "Menyamakan istri dengan ibu." },
  { "en": "Zikir?", "id": "Ingat Allah." },
  { "en": "Zil?", "id": "Simbal kecil." },
  { "en": "Zimase?", "id": "Enzim." },
  { "en": "Zimogen?", "id": "Proenzim." },
  { "en": "Zina?", "id": "Hubungan seks haram." },
  { "en": "Zindik?", "id": "Ateis." },
  { "en": "Zink?", "id": "Seng." },
  { "en": "Zinkografi?", "id": "Cetak seng." },
  { "en": "Zion?", "id": "Bukit Yerusalem." },
  { "en": "Zionis?", "id": "Pendukung Zionisme." },
  { "en": "Zionisme?", "id": "Gerakan Yahudi." },
  { "en": "Zirah?", "id": "Baju besi." },
  { "en": "Zirkon?", "id": "Batu permata." },
  { "en": "Zirkonia?", "id": "Senyawa." },
  { "en": "Zirkonium?", "id": "Unsur kimia." },
  { "en": "Ziter?", "id": "Alat musik." },
  { "en": "Zloty?", "id": "Mata uang Polandia." },
  { "en": "Zo?", "id": "Suku bangsa." },
  { "en": "Zodiac?", "id": "Zodiak." },
  { "en": "Zodiak?", "id": "Rasi bintang." },
  { "en": "Zoetrop?", "id": "Alat ilusi gerak." },
  { "en": "Zoofit?", "id": "Hewan mirip tumbuhan." },
  { "en": "Zoofobia?", "id": "Takut hewan." },
  { "en": "Zoogani?", "id": "Perkembangbiakan hewan." },
  { "en": "Zoogeografi?", "id": "Geografi hewan." },
  { "en": "Zoologi?", "id": "Ilmu hewan." },
  { "en": "Zoom?", "id": "Perbesar (Inggris)." },
  { "en": "Zoomorfis?", "id": "Berbentuk hewan." },
  { "en": "Zoonosis?", "id": "Penyakit hewan menular." },
  { "en": "Zooplankton?", "id": "Plankton hewan." },
  { "en": "Zootekni?", "id": "Ilmu ternak." },
  { "en": "Zoroasterianisme?", "id": "Agama." },
  { "en": "Zoroastrianisme?", "id": "Zoroasterianisme." },
  { "en": "Zuab?", "id": "Prajurit Aljazair." },
  { "en": "Zuhud?", "id": "Menjauhi duniawi." },
  { "en": "Zuhur?", "id": "Waktu salat." },
  { "en": "Zulfikar?", "id": "Pedang Ali." },
  { "en": "Zulhijah?", "id": "Bulan Hijriah." },
  { "en": "Zulkaidah?", "id": "Bulan Hijriah." },
  { "en": "Zulkarnain?", "id": "Tokoh." },
  { "en": "Zulmat?", "id": "Kegelapan (Arab)." },
  { "en": "Zuriat?", "id": "Keturunan." }

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
            }, 4000);
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
