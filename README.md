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


  { "en": "Apa Itu Pemrograman?", "id": "Memberi Instruksi Kepada Komputer." },
  { "en": "Apa Itu Program?", "id": "Kumpulan Instruksi Yang Dijalankan Komputer." },
  { "en": "Apa Itu Kode Sumber (Source Code)?", "id": "Kode Program Yang Ditulis Oleh Manusia." },
  { "en": "Apa Itu Algoritma?", "id": "Langkah-Langkah Logis Untuk Menyelesaikan Masalah." },
  { "en": "Apa Itu Bahasa Pemrograman?", "id": "Bahasa Untuk Berkomunikasi Dengan Komputer." },
  { "en": "Bahasa Tingkat Tinggi?", "id": "Lebih Mudah Dipahami Manusia (Python, Java)." },
  { "en": "Bahasa Tingkat Rendah?", "id": "Lebih Dekat Dengan Bahasa Mesin (Assembly)." },
  { "en": "Apa Itu Bahasa Mesin?", "id": "Bahasa Biner Yang Dimengerti CPU." },
  { "en": "Apa Itu Sintaks (Syntax)?", "id": "Aturan Penulisan Dalam Bahasa Pemrograman." },
  { "en": "Apa Itu Semantik?", "id": "Arti Dari Instruksi Yang Ditulis." },
  { "en": "Apa Itu Variabel?", "id": "Wadah Untuk Menyimpan Nilai Atau Data." },
  { "en": "Apa Itu Tipe Data?", "id": "Jenis Nilai Yang Dapat Disimpan Variabel." },
  { "en": "Apa Itu Input?", "id": "Data Yang Diberikan Ke Program." },
  { "en": "Apa Itu Output?", "id": "Hasil Yang Dikeluarkan Oleh Program." },
  { "en": "Apa Itu Kompilasi (Compilation)?", "id": "Menerjemahkan Kode Sumber Menjadi Bahasa Mesin." },
  { "en": "Apa Itu Compiler?", "id": "Program Yang Melakukan Kompilasi." },
  { "en": "Apa Itu Interpretasi?", "id": "Menjalankan Kode Baris Demi Baris." },
  { "en": "Apa Itu Interpreter?", "id": "Program Yang Melakukan Interpretasi." },
  { "en": "Apa Itu Debugging?", "id": "Proses Mencari Dan Memperbaiki Kesalahan." },
  { "en": "Apa Itu Bug?", "id": "Kesalahan Atau Cacat Dalam Program." },
  { "en": "Apa Itu Eksekusi (Execution)?", "id": "Proses Menjalankan Program." },
  { "en": "Apa Itu Pernyataan (Statement)?", "id": "Satu Baris Instruksi Dalam Program." },
  { "en": "Apa Itu Ekspresi (Expression)?", "id": "Kombinasi Nilai, Variabel, Operator." },
  { "en": "Apa Itu Operator?", "id": "Simbol Yang Melakukan Operasi." },
  { "en": "Apa Itu Komentar (Comment)?", "id": "Teks Dalam Kode Yang Diabaikan Komputer." },
  { "en": "Fungsi Komentar?", "id": "Menjelaskan Kode Untuk Programmer Lain." },
  { "en": "Apa Itu Logika Pemrograman?", "id": "Kemampuan Berpikir Terstruktur Untuk Memecahkan Masalah." },
  { "en": "Apa Itu Flowchart?", "id": "Diagram Alir Yang Menggambarkan Algoritma." },
  { "en": "Apa Itu Pseudocode?", "id": "Deskripsi Algoritma Menggunakan Bahasa Sederhana." },
  { "en": "Apa Itu Fungsi?", "id": "Blok Kode Yang Dapat Digunakan Kembali." },
  { "en": "Apa Itu Library?", "id": "Kumpulan Fungsi Dan Kode Yang Telah Dibuat." },
  { "en": "Apa Itu Framework?", "id": "Kerangka Kerja Untuk Membangun Aplikasi." },
  { "en": "Apa Itu API?", "id": "Application Programming Interface." },
  { "en": "Fungsi API?", "id": "Antarmuka Untuk Berinteraksi Dengan Perangkat Lunak." },
  { "en": "Apa Itu IDE?", "id": "Integrated Development Environment." },
  { "en": "Fungsi IDE?", "id": "Lingkungan Terpadu Untuk Menulis Kode." },
  { "en": "Apa Itu Text Editor?", "id": "Program Untuk Menulis Teks (Termasuk Kode)." },
  { "en": "Apa Itu Terminal (Command Line)?", "id": "Antarmuka Teks Untuk Berinteraksi Dengan Komputer." },
  { "en": "Apa Itu Struktur Data?", "id": "Cara Mengorganisir Data Di Memori." },
  { "en": "Apa Itu Array?", "id": "Kumpulan Elemen Data Sejenis." },
  { "en": "Apa Itu String?", "id": "Urutan Karakter (Teks)." },
  { "en": "Apa Itu Boolean?", "id": "Tipe Data Benar (True) Atau Salah (False)." },
  { "en": "Apa Itu Kondisional (Conditional)?", "id": "Pernyataan Yang Dijalankan Berdasarkan Kondisi." },
  { "en": "Contoh Kondisional?", "id": "If, Else, Switch." },
  { "en": "Apa Itu Loop (Perulangan)?", "id": "Mengulangi Blok Kode Beberapa Kali." },
  { "en": "Contoh Loop?", "id": "For, While, Do-While." },
  { "en": "Apa Itu Pemrograman Terstruktur?", "id": "Paradigma Pemrograman Berbasis Fungsi Prosedur." },
  { "en": "Apa Itu OOP?", "id": "Object-Oriented Programming." },
  { "en": "Apa Itu Pemrograman Fungsional?", "id": "Paradigma Berbasis Fungsi Matematika Murni." },
  { "en": "Apa Itu Error Sintaks?", "id": "Kesalahan Dalam Aturan Penulisan Kode." },
  { "en": "Apa Itu Error Logika?", "id": "Program Berjalan Tapi Hasilnya Salah." },
  { "en": "Apa Itu Error Runtime?", "id": "Kesalahan Yang Terjadi Saat Program Berjalan." },
  { "en": "Apa Itu Deklarasi?", "id": "Memperkenalkan Variabel Atau Fungsi." },
  { "en": "Apa Itu Inisialisasi?", "id": "Memberi Nilai Awal Pada Variabel." },
  { "en": "Apa Itu Assignment?", "id": "Memberi Nilai Baru Pada Variabel." },
  { "en": "Apa Itu Keyword (Reserved Word)?", "id": "Kata Yang Memiliki Arti Khusus." },
  { "en": "Apa Itu Identifier?", "id": "Nama Yang Diberikan Untuk Variabel Fungsi." },
  { "en": "Apa Itu Konstanta?", "id": "Variabel Yang Nilainya Tidak Bisa Diubah." },
  { "en": "Apa Itu Literal?", "id": "Nilai Mentah Yang Ditulis Dalam Kode." },
  { "en": "Apa Itu Pemrograman?", "id": "Proses Mendesain, Menulis, Menguji, Memelihara Kode." },
  { "en": "Apa Itu Programmer?", "id": "Orang Yang Menulis Program Komputer." },
  { "en": "Apa Itu Development?", "id": "Proses Membuat Perangkat Lunak." },
  { "en": "Apa Itu Abstraksi?", "id": "Menyembunyikan Detail Kompleks." },
  { "en": "Apa Itu Dekomposisi?", "id": "Memecah Masalah Besar Menjadi Bagian Kecil." },
  { "en": "Apa Itu Pengenalan Pola?", "id": "Mencari Kemiripan Di Antara Masalah." },
  { "en": "Apa Itu Computational Thinking?", "id": "Proses Berpikir Untuk Memformulasikan Masalah." },
  { "en": "Apa Itu Versi Kontrol?", "id": "Sistem Untuk Melacak Perubahan Kode." },
  { "en": "Contoh Versi Kontrol?", "id": "Git." },
  { "en": "Apa Itu Repository?", "id": "Tempat Penyimpanan Proyek Kode." },
  { "en": "Apa Itu Perangkat Lunak (Software)?", "id": "Program Dan Data Terkait." },
  { "en": "Apa Itu Perangkat Keras (Hardware)?", "id": "Komponen Fisik Komputer." },
  { "en": "Apa Itu Sistem Operasi?", "id": "Perangkat Lunak Yang Mengelola Perangkat Keras." },
  { "en": "Apa Itu Memori?", "id": "Tempat Komputer Menyimpan Data." },
  { "en": "Apa Itu CPU?", "id": "Central Processing Unit, Otak Komputer." },
  { "en": "Apa Itu Binary?", "id": "Sistem Bilangan Berbasis Dua (0 Dan 1)." },
  { "en": "Apa Itu Bit?", "id": "Binary Digit." },
  { "en": "Apa Itu Testing?", "id": "Proses Memverifikasi Bahwa Program Bekerja." },
  { "en": "Apa Itu Dokumentasi?", "id": "Menulis Penjelasan Tentang Cara Kerja Kode." },
  { "en": "Apa Itu Pengguna Akhir (End User)?", "id": "Orang Yang Menggunakan Program." },
  { "en": "Apa Itu Antarmuka Pengguna (UI)?", "id": "User Interface, Cara Pengguna Berinteraksi." },
  { "en": "Apa Itu Pengalaman Pengguna (UX)?", "id": "User Experience, Kesan Pengguna." },
  { "en": "Apa Itu Efisiensi Algoritma?", "id": "Seberapa Cepat Dan Hemat Memori." },
  { "en": "Apa Itu Readability?", "id": "Seberapa Mudah Kode Dapat Dibaca Manusia." },
  { "en": "Apa Itu Maintainability?", "id": "Seberapa Mudah Kode Dapat Diubah Diperbaiki." },
  { "en": "Apa Itu Portability?", "id": "Seberapa Mudah Program Dijalankan Di Platform Lain." },
  { "en": "Apa Itu Scalability?", "id": "Kemampuan Sistem Menangani Peningkatan Beban." },
  { "en": "Apa Itu Refactoring?", "id": "Memperbaiki Struktur Internal Kode." },
  { "en": "Apa Itu Konvensi Penamaan?", "id": "Aturan Konsisten Untuk Memberi Nama Identifier." },
  { "en": "Apa Itu Indentasi?", "id": "Menjorokkan Kode Untuk Keterbacaan." },
  { "en": "Apa Itu Hello, World!?", "id": "Program Pertama Yang Biasa Ditulis Pemula." },
  { "en": "Apa Itu Console?", "id": "Output Teks Standar Dari Program." },
  { "en": "Apa Itu Variabel?", "id": "Nama Simbolik Untuk Lokasi Penyimpanan." },
  { "en": "Mengapa Variabel Penting?", "id": "Untuk Menyimpan Dan Memanipulasi Data." },
  { "en": "Apa Itu Deklarasi Variabel?", "id": "Memperkenalkan Nama Dan Tipe Variabel." },
  { "en": "Apa Itu Inisialisasi Variabel?", "id": "Memberikan Nilai Awal Saat Deklarasi." },
  { "en": "Apa Itu Assignment?", "id": "Proses Memberikan Nilai Ke Variabel." },
  { "en": "Apa Itu Tipe Data?", "id": "Klasifikasi Data Yang Menentukan Nilai." },
  { "en": "Apa Itu Tipe Data Primitif?", "id": "Tipe Data Paling Dasar." },
  { "en": "Contoh Tipe Data Primitif?", "id": "Integer, Float, Boolean, Character." },
  { "en": "Apa Itu Integer?", "id": "Tipe Data Untuk Bilangan Bulat." },
  { "en": "Contoh Integer?", "id": "10, -5, 0." },
  { "en": "Apa Itu Float?", "id": "Tipe Data Untuk Bilangan Desimal." },
  { "en": "Contoh Float?", "id": "3.14, -0.01." },
  { "en": "Nama Lain Float?", "id": "Floating-Point Number." },
  { "en": "Apa Itu Double?", "id": "Tipe Data Float Dengan Presisi Ganda." },
  { "en": "Apa Itu Boolean?", "id": "Tipe Data Logis." },
  { "en": "Nilai Boolean Apa Saja?", "id": "True (Benar) Atau False (Salah)." },
  { "en": "Apa Itu Character (Char)?", "id": "Tipe Data Untuk Satu Karakter." },
  { "en": "Contoh Character?", "id": "'A', 'b', '$'." },
  { "en": "Apa Itu String?", "id": "Tipe Data Untuk Urutan Karakter." },
  { "en": "Contoh String?", "id": "\"Halo Dunia\"." },
  { "en": "Apa Itu Tipe Data Komposit?", "id": "Tipe Data Yang Terdiri Dari Tipe Lain." },
  { "en": "Contoh Tipe Data Komposit?", "id": "Array, Struct, Class." },
  { "en": "Apa Itu Static Typing?", "id": "Tipe Data Diperiksa Saat Kompilasi." },
  { "en": "Apa Itu Dynamic Typing?", "id": "Tipe Data Diperiksa Saat Runtime." },
  { "en": "Apa Itu Type Casting?", "id": "Mengubah Variabel Dari Satu Tipe Ke Tipe Lain." },
  { "en": "Apa Itu Implicit Casting?", "id": "Konversi Tipe Otomatis Oleh Compiler." },
  { "en": "Apa Itu Explicit Casting?", "id": "Konversi Tipe Yang Dilakukan Programmer." },
  { "en": "Apa Itu Operator?", "id": "Simbol Untuk Melakukan Operasi." },
  { "en": "Apa Itu Operand?", "id": "Nilai Atau Variabel Yang Dioperasikan." },
  { "en": "Apa Itu Operator Aritmatika?", "id": "Operator Untuk Perhitungan Matematika." },
  { "en": "Contoh Operator Aritmatika?", "id": "+, -, *, /." },
  { "en": "Apa Itu Operator Modulo (%)?", "id": "Memberikan Sisa Hasil Bagi." },
  { "en": "Apa Itu Operator Increment (++)?", "id": "Menambah Nilai Variabel Sebesar Satu." },
  { "en": "Apa Itu Operator Decrement (--)?", "id": "Mengurangi Nilai Variabel Sebesar Satu." },
  { "en": "Apa Itu Operator Assignment?", "id": "Memberikan Nilai Ke Variabel." },
  { "en": "Contoh Operator Assignment?", "id": "=, +=, -=, *=" },
  { "en": "Apa Itu Operator Perbandingan?", "id": "Membandingkan Dua Nilai." },
  { "en": "Hasil Operator Perbandingan?", "id": "Boolean (True Atau False)." },
  { "en": "Contoh Operator Perbandingan?", "id": "==, !=, <, >." },
  { "en": "Perbedaan '=' Dan '=='?", "id": "'=' Untuk Assignment, '==' Untuk Perbandingan." },
  { "en": "Apa Itu Operator Logika?", "id": "Menggabungkan Atau Memodifikasi Ekspresi Boolean." },
  { "en": "Contoh Operator Logika?", "id": "AND, OR, NOT." },
  { "en": "Operator AND Logika?", "id": "Benar Jika Kedua Sisi Benar." },
  { "en": "Operator OR Logika?", "id": "Benar Jika Salah Satu Sisi Benar." },
  { "en": "Operator NOT Logika?", "id": "Membalik Nilai Boolean." },
  { "en": "Apa Itu Operator Bitwise?", "id": "Melakukan Operasi Pada Level Bit." },
  { "en": "Contoh Operator Bitwise?", "id": "&, |, ^, ~, <<, >>." },
  { "en": "Apa Itu Operator Ternary?", "id": "Operator Kondisional Singkat (? :)." },
  { "en": "Apa Itu Precedence Operator?", "id": "Urutan Evaluasi Operator Dalam Ekspresi." },
  { "en": "Mana Yang Didahulukan, '*' Atau '+'?", "id": "Perkalian (*)." },
  { "en": "Apa Itu Asosiativitas Operator?", "id": "Urutan Evaluasi Jika Precedence Sama." },
  { "en": "Apa Itu Konstanta?", "id": "Variabel Yang Nilainya Tetap." },
  { "en": "Kata Kunci Untuk Konstanta?", "id": "Const Atau Final." },
  { "en": "Apa Itu Literal?", "id": "Representasi Nilai Tetap Dalam Kode." },
  { "en": "Contoh Literal?", "id": "123, 3.14, 'A', \"Teks\"." },
  { "en": "Apa Itu Null?", "id": "Merepresentasikan Ketiadaan Nilai." },
  { "en": "Apa Itu Scope (Cakupan)?", "id": "Wilayah Kode Dimana Variabel Dapat Diakses." },
  { "en": "Apa Itu Variabel Global?", "id": "Dapat Diakses Dari Mana Saja." },
  { "en": "Apa Itu Variabel Lokal?", "id": "Hanya Dapat Diakses Dalam Blok Tertentu." },
  { "en": "Apa Itu Shadowing Variabel?", "id": "Variabel Lokal Menyembunyikan Variabel Global." },
  { "en": "Apa Itu Lifetime Variabel?", "id": "Waktu Selama Variabel Ada Di Memori." },
  { "en": "Apa Itu Concatenation?", "id": "Menggabungkan Dua String Menjadi Satu." },
  { "en": "Operator Untuk Concatenation?", "id": "Biasanya Operator Tambah (+)." },
  { "en": "Apa Itu Escape Sequence?", "id": "Karakter Khusus Yang Diawali Backslash." },
  { "en": "Contoh Escape Sequence?", "id": "\\n (Baris Baru), \\t (Tab)." },
  { "en": "Apa Itu Integer Overflow?", "id": "Hasil Perhitungan Melebihi Kapasitas Tipe Data." },
  { "en": "Apa Itu Underflow?", "id": "Hasil Perhitungan Terlalu Kecil." },
  { "en": "Apa Itu Presisi Floating-Point?", "id": "Jumlah Digit Signifikan Yang Dapat Disimpan." },
  { "en": "Masalah Presisi Floating-Point?", "id": "Tidak Semua Bilangan Desimal Dapat Direpresentasikan." },
  { "en": "Apa Itu Enum (Enumeration)?", "id": "Tipe Data Yang Terdiri Dari Kumpulan Konstanta." },
  { "en": "Apa Itu Type Inference?", "id": "Compiler Menebak Tipe Data Secara Otomatis." },
  { "en": "Apa Itu Variabel Reference?", "id": "Variabel Yang Merujuk Ke Alamat Memori." },
  { "en": "Apa Itu Pointer?", "id": "Variabel Yang Menyimpan Alamat Memori." },
  { "en": "Apa Itu Unicode?", "id": "Standar Pengkodean Karakter Internasional." },
  { "en": "Apa Itu ASCII?", "id": "American Standard Code for Information Interchange." },
  { "en": "Apa Itu Big Endian?", "id": "Menyimpan Byte Paling Signifikan Terlebih Dahulu." },
  { "en": "Apa Itu Little Endian?", "id": "Menyimpan Byte Paling Tidak Signifikan Dahulu." },
  { "en": "Apa Itu Short-Circuit Evaluation?", "id": "Mengevaluasi Sisi Kedua Jika Perlu." },
  { "en": "Contoh Short-Circuit?", "id": "Dalam (A && B), B Tidak Dievaluasi Jika A Salah." },
  { "en": "Apa Itu Operasi Unary?", "id": "Operasi Dengan Satu Operand." },
  { "en": "Apa Itu Operasi Binary?", "id": "Operasi Dengan Dua Operand." },
  { "en": "Apa Itu Strong Typing?", "id": "Bahasa Pemrograman Dengan Aturan Tipe Ketat." },
  { "en": "Apa Itu Weak Typing?", "id": "Bahasa Pemrograman Dengan Aturan Tipe Longgar." },
  { "en": "Apa Itu Garbage Collection?", "id": "Manajemen Memori Otomatis." },
  { "en": "Apa Itu Signed Integer?", "id": "Integer Yang Dapat Menyimpan Nilai Negatif." },
  { "en": "Apa Itu Unsigned Integer?", "id": "Integer Yang Hanya Menyimpan Nilai Positif." },
  { "en": "Apa Itu Memory Address?", "id": "Lokasi Unik Data Di Memori Komputer." },
  { "en": "Apa Itu Nilai Default Variabel?", "id": "Nilai Awal Jika Tidak Diinisialisasi." },
  { "en": "Apa Itu Mutability?", "id": "Apakah Nilai Objek Dapat Diubah." },
  { "en": "Apa Itu Objek Immutable?", "id": "Objek Yang Nilainya Tidak Dapat Diubah." },
  { "en": "Apa Itu Side Effect?", "id": "Fungsi Mengubah Keadaan Di Luar Cakupannya." },
  { "en": "Apa Itu Pure Function?", "id": "Fungsi Tanpa Side Effect." },
  { "en": "Apa Itu Aliran Kontrol?", "id": "Urutan Eksekusi Pernyataan Dalam Program." },
  { "en": "Aliran Kontrol Normal?", "id": "Sekuensial, Dari Atas Ke Bawah." },
  { "en": "Dua Jenis Utama Aliran Kontrol?", "id": "Percabangan Dan Perulangan." },
  { "en": "Apa Itu Percabangan (Branching)?", "id": "Memilih Jalur Eksekusi Berdasarkan Kondisi." },
  { "en": "Nama Lain Percabangan?", "id": "Seleksi, Kondisional." },
  { "en": "Pernyataan Apa Untuk Percabangan?", "id": "If, Else, Else If, Switch." },
  { "en": "Apa Itu Pernyataan If?", "id": "Menjalankan Blok Kode Jika Kondisi Benar." },
  { "en": "Apa Itu Kondisi?", "id": "Ekspresi Yang Mengevaluasi Ke True Atau False." },
  { "en": "Apa Itu Pernyataan Else?", "id": "Menjalankan Blok Kode Jika Kondisi If Salah." },
  { "en": "Apa Itu Pernyataan Else If?", "id": "Memeriksa Kondisi Baru Jika If Sebelumnya Salah." },
  { "en": "Apa Itu Nested If?", "id": "Pernyataan If Di Dalam Pernyataan If Lain." },
  { "en": "Apa Itu Blok Kode?", "id": "Satu Atau Lebih Pernyataan Yang Dikelompokkan." },
  { "en": "Bagaimana Blok Kode Ditandai?", "id": "Biasanya Dengan Kurung Kurawal {} Atau Indentasi." },
  { "en": "Apa Itu Pernyataan Switch?", "id": "Memilih Satu Dari Banyak Blok Kode." },
  { "en": "Dasar Seleksi Switch?", "id": "Berdasarkan Nilai Sebuah Variabel Atau Ekspresi." },
  { "en": "Apa Itu Case Dalam Switch?", "id": "Label Untuk Blok Kode Tertentu." },
  { "en": "Apa Itu Pernyataan Break?", "id": "Keluar Dari Blok Switch Atau Loop." },
  { "en": "Apa Itu Default Case?", "id": "Dijalankan Jika Tidak Ada Case Yang Cocok." },
  { "en": "Apa Itu Perulangan (Looping)?", "id": "Mengulangi Eksekusi Blok Kode." },
  { "en": "Mengapa Perulangan Dibutuhkan?", "id": "Untuk Mengotomatiskan Tugas Berulang." },
  { "en": "Tiga Jenis Utama Loop?", "id": "While, For, Do-While." },
  { "en": "Apa Itu While Loop?", "id": "Mengulang Selama Kondisi Benar." },
  { "en": "Kapan Kondisi While Loop Diperiksa?", "id": "Sebelum Setiap Iterasi." },
  { "en": "Apa Itu Infinite Loop?", "id": "Perulangan Yang Tidak Pernah Berhenti." },
  { "en": "Penyebab Infinite Loop?", "id": "Kondisi Loop Selalu Benar." },
  { "en": "Apa Itu Iterasi?", "id": "Satu Kali Eksekusi Badan Loop." },
  { "en": "Apa Itu For Loop?", "id": "Loop Dengan Inisialisasi, Kondisi, Dan Pembaruan." },
  { "en": "Bagian Inisialisasi For Loop?", "id": "Dijalankan Satu Kali Sebelum Loop Dimulai." },
  { "en": "Bagian Kondisi For Loop?", "id": "Diperiksa Sebelum Setiap Iterasi." },
  { "en": "Bagian Pembaruan For Loop?", "id": "Dijalankan Setelah Setiap Iterasi." },
  { "en": "Apa Itu Do-While Loop?", "id": "Mirip While, Tapi Kondisi Diperiksa Di Akhir." },
  { "en": "Apa Jaminan Do-While Loop?", "id": "Badan Loop Dijalankan Minimal Satu Kali." },
  { "en": "Apa Itu Nested Loop?", "id": "Loop Di Dalam Loop Lain." },
  { "en": "Pernyataan Break Dalam Loop?", "id": "Menghentikan Loop Secara Paksa." },
  { "en": "Apa Itu Pernyataan Continue?", "id": "Melewatkan Sisa Iterasi Saat Ini." },
  { "en": "Apa Itu Loop Counter?", "id": "Variabel Untuk Menghitung Jumlah Iterasi." },
  { "en": "Apa Itu Entry-Controlled Loop?", "id": "Kondisi Diperiksa Di Awal." },
  { "en": "Contoh Entry-Controlled Loop?", "id": "For Dan While." },
  { "en": "Apa Itu Exit-Controlled Loop?", "id": "Kondisi Diperiksa Di Akhir." },
  { "en": "Contoh Exit-Controlled Loop?", "id": "Do-While." },
  { "en": "Apa Itu For-Each Loop?", "id": "Loop Untuk Melintasi Elemen Koleksi." },
  { "en": "Apa Itu Flag?", "id": "Variabel Boolean Untuk Mengontrol Aliran Program." },
  { "en": "Apa Itu Pernyataan Goto?", "id": "Melompat Ke Label Tertentu Dalam Kode." },
  { "en": "Mengapa Goto Dihindari?", "id": "Membuat Kode Sulit Dibaca Dan Diikuti." },
  { "en": "Apa Itu Spaghetti Code?", "id": "Kode Dengan Aliran Kontrol Yang Rumit." },
  { "en": "Apa Itu Early Exit?", "id": "Keluar Dari Fungsi Atau Loop Sebelum Akhir." },
  { "en": "Apa Itu Guard Clause?", "id": "Memeriksa Kondisi Tidak Valid Di Awal." },
  { "en": "Apa Itu Kondisi Batas (Boundary Condition)?", "id": "Nilai Ekstrem Yang Harus Diuji." },
  { "en": "Apa Itu Truthy Dan Falsy?", "id": "Nilai Yang Dianggap True Atau False." },
  { "en": "Contoh Nilai Falsy?", "id": "0, Null, String Kosong." },
  { "en": "Struktur If-Else If-Else?", "id": "Memilih Satu Dari Beberapa Pilihan." },
  { "en": "Apa Itu Kondisi Kompleks?", "id": "Kondisi Yang Menggabungkan Beberapa Ekspresi." },
  { "en": "Kapan Menggunakan While vs For?", "id": "For Untuk Iterasi, While Untuk Kondisi." },
  { "en": "Apa Itu Badan Loop (Loop Body)?", "id": "Blok Kode Di Dalam Perulangan." },
  { "en": "Apa Itu Kondisi Terminasi?", "id": "Kondisi Yang Membuat Loop Berhenti." },
  { "en": "Pentingnya Memperbarui Variabel Loop?", "id": "Untuk Menghindari Infinite Loop." },
  { "en": "Apa Itu Iterasi Melalui Array?", "id": "Mengunjungi Setiap Elemen Dalam Array." },
  { "en": "Bagaimana Menggunakan For Loop Untuk Array?", "id": "Menggunakan Indeks Dari 0 Hingga Panjang-1." },
  { "en": "Apa Itu Breakpoint?", "id": "Titik Henti Sementara Saat Debugging." },
  { "en": "Apa Itu Stepping Through Code?", "id": "Menjalankan Kode Baris Demi Baris." },
  { "en": "Apa Itu Conditional Operator?", "id": "Nama Lain Untuk Operator Ternary." },
  { "en": "Apa Itu Fall-through Dalam Switch?", "id": "Eksekusi Berlanjut Ke Case Berikutnya." },
  { "en": "Bagaimana Mencegah Fall-through?", "id": "Menggunakan Pernyataan Break." },
  { "en": "Apa Itu Pemrograman Terstruktur?", "id": "Menggunakan Struktur Kontrol (Seleksi, Perulangan)." },
  { "en": "Apa Itu Short-Circuiting?", "id": "Evaluasi Operator Logika Yang Efisien." },
  { "en": "Contoh Short-Circuiting?", "id": "Dalam (A || B), B Tidak Dievaluasi Jika A Benar." },
  { "en": "Apa Itu State Machine?", "id": "Model Perilaku Dengan Keadaan Dan Transisi." },
  { "en": "Bagaimana Mengimplementasikan State Machine?", "id": "Menggunakan Switch Atau If-Else." },
  { "en": "Apa Itu Sentinel Value?", "id": "Nilai Khusus Untuk Menghentikan Loop." },
  { "en": "Apa Itu Loop Invariant?", "id": "Kondisi Yang Selalu Benar." },
  { "en": "Tujuan Loop Invariant?", "id": "Untuk Membuktikan Kebenaran Algoritma." },
  { "en": "Apa Itu Pernyataan Return?", "id": "Mengakhiri Eksekusi Fungsi." },
  { "en": "Apa Itu Pernyataan Exit?", "id": "Menghentikan Seluruh Program." },
  { "en": "Apa Itu Dry Run?", "id": "Menelusuri Kode Secara Manual Di Atas Kertas." },
  { "en": "Apa Itu Off-by-One Error?", "id": "Kesalahan Umum Dalam Kondisi Loop." },
  { "en": "Apa Itu Logic Error?", "id": "Kesalahan Dalam Logika Program." },
  { "en": "Apa Itu Exception Handling?", "id": "Mekanisme Menangani Error Saat Runtime." },
  { "en": "Blok Try-Catch?", "id": "Struktur Untuk Exception Handling." },
  { "en": "Apa Itu Pernyataan Throw?", "id": "Melemparkan Sebuah Exception." },
  { "en": "Apa Itu Rekursi?", "id": "Fungsi Yang Memanggil Dirinya Sendiri." },
  { "en": "Rekursi Sebagai Alternatif Loop?", "id": "Ya, Untuk Masalah Tertentu." },
  { "en": "Apa Itu Kasus Dasar (Base Case)?", "id": "Kondisi Untuk Menghentikan Rekursi." },
  { "en": "Apa Itu Tail Recursion?", "id": "Rekursi Dimana Panggilan Adalah Operasi Terakhir." },
  { "en": "Apa Itu Label?", "id": "Penanda Lokasi Dalam Kode Untuk Goto." },
  { "en": "Apa Itu Loop Tak Terbatas?", "id": "Sama Dengan Infinite Loop." },
  { "en": "Apa Itu Iterabel?", "id": "Objek Yang Dapat Diiterasi (Array, List)." },
  { "en": "Apa Itu Iterator?", "id": "Objek Yang Melacak Iterasi." },
  { "en": "Apa Itu Generator?", "id": "Fungsi Khusus Untuk Membuat Iterator." },
  { "en": "Apa Itu Yield?", "id": "Kata Kunci Dalam Generator." },
  { "en": "Apa Itu Branch Prediction?", "id": "Teknik CPU Untuk Mempercepat Percabangan." },
  { "en": "Apa Itu Conditional Move?", "id": "Instruksi CPU Untuk Menghindari Percabangan." },
  { "en": "Apa Itu Block Scope?", "id": "Variabel Hanya Ada Di Dalam Blok." },
  { "en": "Apa Itu Loop Unrolling?", "id": "Optimisasi Compiler Menggandakan Badan Loop." },
  { "en": "Tujuan Loop Unrolling?", "id": "Mengurangi Overhead Percabangan." },
  { "en": "Apa Itu Fungsi (Function)?", "id": "Blok Kode Bernama Yang Melakukan Tugas." },
  { "en": "Nama Lain Fungsi?", "id": "Subroutine, Prosedur, Metode." },
  { "en": "Mengapa Menggunakan Fungsi?", "id": "Untuk Reusabilitas Dan Organisasi Kode." },
  { "en": "Apa Itu Reusabilitas Kode?", "id": "Menggunakan Kembali Kode Yang Sama Tanpa Menulis Ulang." },
  { "en": "Apa Itu Modularitas?", "id": "Memecah Program Menjadi Modul-Modul Independen." },
  { "en": "Apa Itu Deklarasi Fungsi?", "id": "Memperkenalkan Nama, Parameter, Dan Tipe Kembalian." },
  { "en": "Nama Lain Deklarasi Fungsi?", "id": "Prototipe Fungsi." },
  { "en": "Apa Itu Definisi Fungsi?", "id": "Berisi Kode Aktual Yang Dijalankan Fungsi." },
  { "en": "Apa Itu Panggilan Fungsi (Function Call)?", "id": "Mengeksekusi Kode Di Dalam Fungsi." },
  { "en": "Apa Itu Parameter?", "id": "Variabel Input Untuk Sebuah Fungsi." },
  { "en": "Apa Itu Argumen?", "id": "Nilai Aktual Yang Diberikan Saat Panggilan." },
  { "en": "Apa Itu Tipe Kembalian (Return Type)?", "id": "Tipe Data Dari Nilai Yang Dikembalikan." },
  { "en": "Apa Itu Pernyataan Return?", "id": "Mengirim Nilai Kembali Dari Fungsi." },
  { "en": "Apa Itu Fungsi Void?", "id": "Fungsi Yang Tidak Mengembalikan Nilai." },
  { "en": "Apa Perbedaan Fungsi Dan Prosedur?", "id": "Fungsi Mengembalikan Nilai, Prosedur Tidak." },
  { "en": "Apa Itu Parameter Formal?", "id": "Nama Parameter Dalam Definisi Fungsi." },
  { "en": "Apa Itu Argumen Aktual?", "id": "Nilai Yang Dilewatkan Saat Fungsi Dipanggil." },
  { "en": "Apa Itu Pass by Value?", "id": "Menyalin Nilai Argumen Ke Parameter." },
  { "en": "Apa Itu Pass by Reference?", "id": "Memberikan Referensi (Alamat Memori) Argumen." },
  { "en": "Efek Pass by Value?", "id": "Fungsi Tidak Dapat Mengubah Variabel Asli." },
  { "en": "Efek Pass by Reference?", "id": "Fungsi Dapat Mengubah Variabel Asli." },
  { "en": "Apa Itu Signature Fungsi?", "id": "Nama Fungsi Dan Tipe Parameternya." },
  { "en": "Apa Itu Overloading Fungsi?", "id": "Beberapa Fungsi Dengan Nama Sama." },
  { "en": "Syarat Overloading?", "id": "Harus Memiliki Daftar Parameter Yang Berbeda." },
  { "en": "Apa Itu Program Utama?", "id": "Titik Awal Eksekusi Program (Main Function)." },
  { "en": "Apa Itu Variabel Lokal?", "id": "Variabel Yang Dideklarasikan Di Dalam Fungsi." },
  { "en": "Apa Itu Variabel Global?", "id": "Variabel Yang Dideklarasikan Di Luar Fungsi." },
  { "en": "Mengapa Variabel Global Dihindari?", "id": "Membuat Kode Sulit Dilacak Dan Diuji." },
  { "en": "Apa Itu Rekursi?", "id": "Fungsi Yang Memanggil Dirinya Sendiri." },
  { "en": "Apa Itu Kasus Dasar (Base Case)?", "id": "Kondisi Untuk Menghentikan Rekursi." },
  { "en": "Apa Itu Kasus Rekursif?", "id": "Langkah Yang Mendekatkan Ke Kasus Dasar." },
  { "en": "Apa Itu Stack Overflow?", "id": "Error Akibat Rekursi Yang Terlalu Dalam." },
  { "en": "Apa Itu Call Stack?", "id": "Struktur Data Untuk Melacak Panggilan Fungsi." },
  { "en": "Apa Itu Stack Frame?", "id": "Data Terkait Panggilan Fungsi Tunggal." },
  { "en": "Apa Itu Fungsi Pustaka (Library Function)?", "id": "Fungsi Yang Telah Disediakan Oleh Bahasa." },
  { "en": "Contoh Fungsi Pustaka?", "id": "Println, Sqrt, Random." },
  { "en": "Apa Itu Modul?", "id": "File Yang Berisi Kode Terkait." },
  { "en": "Apa Itu Impor (Import)?", "id": "Menggunakan Kode Dari Modul Lain." },
  { "en": "Apa Itu Namespace?", "id": "Konteks Untuk Menghindari Konflik Penamaan." },
  { "en": "Apa Itu Fungsi Anonim?", "id": "Fungsi Tanpa Nama." },
  { "en": "Nama Lain Fungsi Anonim?", "id": "Ekspresi Lambda." },
  { "en": "Apa Itu Higher-Order Function?", "id": "Fungsi Yang Menerima Atau Mengembalikan Fungsi." },
  { "en": "Apa Itu Callback?", "id": "Fungsi Yang Dilewatkan Sebagai Argumen." },
  { "en": "Apa Itu Closure?", "id": "Fungsi Yang Mengingat Lingkungan Tempat Dibuat." },
  { "en": "Apa Itu Side Effect?", "id": "Fungsi Mengubah Keadaan Di Luar Cakupannya." },
  { "en": "Apa Itu Pure Function?", "id": "Fungsi Tanpa Side Effect." },
  { "en": "Keuntungan Pure Function?", "id": "Mudah Diuji Dan Dinalar." },
  { "en": "Apa Itu Abstraksi Prosedural?", "id": "Menyembunyikan Detail Implementasi Fungsi." },
  { "en": "Apa Itu Refactoring?", "id": "Mengubah Struktur Kode Tanpa Mengubah Perilaku." },
  { "en": "Contoh Refactoring?", "id": "Mengekstrak Kode Menjadi Fungsi Baru." },
  { "en": "Apa Itu Header File?", "id": "File Yang Berisi Deklarasi Fungsi (C/C++)." },
  { "en": "Apa Itu Linker?", "id": "Menggabungkan Kode Terkompilasi Menjadi Program." },
  { "en": "Apa Itu Inline Function?", "id": "Saran Ke Compiler Untuk Menyalin Kode." },
  { "en": "Apa Itu Parameter Default?", "id": "Parameter Dengan Nilai Default." },
  { "en": "Apa Itu Named Argument?", "id": "Melewatkan Argumen Dengan Menyebut Nama Parameter." },
  { "en": "Apa Itu Variable-length Argument List?", "id": "Fungsi Yang Menerima Jumlah Argumen Bervariasi." },
  { "en": "Apa Itu Black Box?", "id": "Melihat Fungsi Hanya Dari Input Outputnya." },
  { "en": "Apa Itu API Documentation?", "id": "Dokumentasi Cara Menggunakan Fungsi Atau Library." },
  { "en": "Apa Itu Stub?", "id": "Implementasi Fungsi Palsu Untuk Pengujian." },
  { "en": "Apa Itu Mock Object?", "id": "Objek Palsu Untuk Mensimulasikan Perilaku." },
  { "en": "Apa Itu Unit Testing?", "id": "Menguji Bagian-Bagian Kecil (Fungsi) Secara Terisolasi." },
  { "en": "Apa Itu Integration Testing?", "id": "Menguji Bagaimana Bagian-Bagian Bekerja Sama." },
  { "en": "Apa Itu DRY Principle?", "id": "Don't Repeat Yourself." },
  { "en": "Apa Itu KISS Principle?", "id": "Keep It Simple, Stupid." },
  { "en": "Apa Itu YAGNI Principle?", "id": "You Ain't Gonna Need It." },
  { "en": "Apa Itu Single Responsibility Principle (SRP)?", "id": "Satu Fungsi Harus Memiliki Satu Tanggung Jawab." },
  { "en": "Apa Itu Entry Point?", "id": "Tempat Dimana Program Mulai Dieksekusi." },
  { "en": "Apa Itu Exit Point?", "id": "Tempat Dimana Program Atau Fungsi Berakhir." },
  { "en": "Apa Itu Top-Down Design?", "id": "Memulai Dari Gambaran Besar Ke Detail." },
  { "en": "Apa Itu Bottom-Up Design?", "id": "Memulai Dari Detail Kecil Ke Sistem Besar." },
  { "en": "Apa Itu Coupling?", "id": "Tingkat Ketergantungan Antar Modul." },
  { "en": "Coupling Yang Baik?", "id": "Loose Coupling (Ketergantungan Rendah)." },
  { "en": "Apa Itu Cohesion?", "id": "Tingkat Keterkaitan Di Dalam Modul." },
  { "en": "Cohesion Yang Baik?", "id": "High Cohesion (Sangat Terkait)." },
  { "en": "Apa Itu Parameter Output?", "id": "Parameter Untuk Mengembalikan Nilai Tambahan." },
  { "en": "Apa Itu Scope Resolution Operator?", "id": "Mengakses Variabel Dari Namespace Tertentu (::)." },
  { "en": "Apa Itu Fungsi Helper?", "id": "Fungsi Kecil Untuk Membantu Fungsi Lain." },
  { "en": "Apa Itu Arity?", "id": "Jumlah Argumen Yang Diterima Fungsi." },
  { "en": "Apa Itu Currying?", "id": "Mengubah Fungsi Banyak Argumen." },
  { "en": "Apa Itu Partial Application?", "id": "Membuat Fungsi Baru Dengan Memperbaiki Argumen." },
  { "en": "Apa Itu Memoization?", "id": "Menyimpan Hasil Fungsi Untuk Menghindari Komputasi Ulang." },
  { "en": "Apa Itu Tail Call Optimization?", "id": "Optimisasi Untuk Menghindari Stack Overflow." },
  { "en": "Apa Itu Precondition?", "id": "Kondisi Yang Harus Benar Sebelum Fungsi Dijalankan." },
  { "en": "Apa Itu Postcondition?", "id": "Kondisi Yang Harus Benar Setelah Fungsi Selesai." },
  { "en": "Apa Itu Invariant?", "id": "Kondisi Yang Selalu Benar." },
  { "en": "Apa Itu Design by Contract?", "id": "Metodologi Desain Menggunakan Pre/Postcondition." },
  { "en": "Apa Itu Idempotent Function?", "id": "Fungsi Yang Memberi Hasil Sama." },
  { "en": "Apa Itu Standard Library?", "id": "Pustaka Inti Yang Datang Dengan Bahasa." },
  { "en": "Apa Itu Domain-Specific Language (DSL)?", "id": "Bahasa Yang Dibuat Untuk Masalah Tertentu." },
  { "en": "Apa Itu First-Class Function?", "id": "Fungsi Dapat Diperlakukan Seperti Variabel Lain." },
  { "en": "Apa Itu Struktur Data?", "id": "Cara Mengorganisir Data Untuk Akses Efisien." },
  { "en": "Mengapa Struktur Data Penting?", "id": "Mempengaruhi Kinerja Dan Efisiensi Algoritma." },
  { "en": "Apa Itu Abstract Data Type (ADT)?", "id": "Model Matematis Untuk Struktur Data." },
  { "en": "Apa Itu Array?", "id": "Kumpulan Elemen Berurutan Di Memori." },
  { "en": "Karakteristik Array?", "id": "Ukuran Tetap, Elemen Sejenis." },
  { "en": "Apa Itu Indeks?", "id": "Posisi Elemen Dalam Array." },
  { "en": "Indeks Array Dimulai Dari Mana?", "id": "Biasanya Dimulai Dari Nol." },
  { "en": "Akses Elemen Array?", "id": "Sangat Cepat Menggunakan Indeks." },
  { "en": "Kekurangan Array?", "id": "Ukuran Tetap, Sisip/Hapus Lambat." },
  { "en": "Apa Itu Array Multidimensi?", "id": "Array Di Dalam Array (Tabel/Matriks)." },
  { "en": "Apa Itu String?", "id": "Array Dari Karakter." },
  { "en": "Apa Itu Linked List (Senarai Berantai)?", "id": "Kumpulan Elemen Yang Dihubungkan Pointer." },
  { "en": "Apa Itu Node?", "id": "Elemen Dalam Linked List." },
  { "en": "Isi Sebuah Node?", "id": "Data Dan Pointer Ke Node Berikutnya." },
  { "en": "Keuntungan Linked List?", "id": "Ukuran Dinamis, Sisip/Hapus Cepat." },
  { "en": "Kekurangan Linked List?", "id": "Akses Elemen Lambat (Harus Berurutan)." },
  { "en": "Apa Itu Head?", "id": "Pointer Ke Elemen Pertama Linked List." },
  { "en": "Apa Itu Tail?", "id": "Pointer Ke Elemen Terakhir Linked List." },
  { "en": "Apa Itu Doubly Linked List?", "id": "Node Memiliki Pointer Ke Berikutnya Dan Sebelumnya." },
  { "en": "Apa Itu Circular Linked List?", "id": "Node Terakhir Menunjuk Ke Node Pertama." },
  { "en": "Apa Itu Stack (Tumpukan)?", "id": "Struktur Data LIFO." },
  { "en": "Apa Itu LIFO?", "id": "Last-In, First-Out." },
  { "en": "Operasi Utama Stack?", "id": "Push Dan Pop." },
  { "en": "Operasi Push?", "id": "Menambahkan Elemen Ke Atas Tumpukan." },
  { "en": "Operasi Pop?", "id": "Menghapus Elemen Dari Atas Tumpukan." },
  { "en": "Operasi Peek (Top)?", "id": "Melihat Elemen Teratas Tanpa Menghapus." },
  { "en": "Aplikasi Stack?", "id": "Undo, Panggilan Fungsi, Cek Kurung." },
  { "en": "Apa Itu Queue (Antrean)?", "id": "Struktur Data FIFO." },
  { "en": "Apa Itu FIFO?", "id": "First-In, First-Out." },
  { "en": "Operasi Utama Queue?", "id": "Enqueue Dan Dequeue." },
  { "en": "Operasi Enqueue?", "id": "Menambahkan Elemen Ke Belakang Antrean." },
  { "en": "Operasi Dequeue?", "id": "Menghapus Elemen Dari Depan Antrean." },
  { "en": "Aplikasi Queue?", "id": "Simulasi Antrean, Print Spooling, BFS." },
  { "en": "Apa Itu Circular Queue?", "id": "Implementasi Queue Menggunakan Array." },
  { "en": "Apa Itu Priority Queue?", "id": "Setiap Elemen Memiliki Prioritas." },
  { "en": "Apa Itu Deque (Double-Ended Queue)?", "id": "Bisa Tambah/Hapus Dari Depan Belakang." },
  { "en": "Apa Itu Tree (Pohon)?", "id": "Struktur Data Hierarkis." },
  { "en": "Apa Itu Root?", "id": "Node Paling Atas Di Tree." },
  { "en": "Apa Itu Parent?", "id": "Node Yang Berada Langsung Di Atas Node Lain." },
  { "en": "Apa Itu Child?", "id": "Node Yang Berada Langsung Di Bawah Node Lain." },
  { "en": "Apa Itu Sibling?", "id": "Node Dengan Parent Yang Sama." },
  { "en": "Apa Itu Leaf?", "id": "Node Tanpa Child." },
  { "en": "Apa Itu Kedalaman (Depth) Node?", "id": "Jarak Dari Root." },
  { "en": "Apa Itu Tinggi (Height) Tree?", "id": "Kedalaman Maksimum Dari Semua Node." },
  { "en": "Apa Itu Binary Tree?", "id": "Setiap Node Memiliki Maksimal Dua Child." },
  { "en": "Child Pada Binary Tree?", "id": "Left Child Dan Right Child." },
  { "en": "Apa Itu Binary Search Tree (BST)?", "id": "Binary Tree Dengan Properti Pencarian." },
  { "en": "Properti BST?", "id": "Kiri < Induk < Kanan." },
  { "en": "Keuntungan BST?", "id": "Pencarian, Sisip, Hapus Cepat." },
  { "en": "Apa Itu Tree Traversal?", "id": "Proses Mengunjungi Setiap Node Di Tree." },
  { "en": "Jenis Tree Traversal?", "id": "In-order, Pre-order, Post-order." },
  { "en": "Traversal In-order?", "id": "Kiri, Induk, Kanan." },
  { "en": "Traversal Pre-order?", "id": "Induk, Kiri, Kanan." },
  { "en": "Traversal Post-order?", "id": "Kiri, Kanan, Induk." },
  { "en": "Apa Itu Level-Order Traversal?", "id": "Mengunjungi Node Level Demi Level." },
  { "en": "Apa Itu Self-Balancing Tree?", "id": "BST Yang Menjaga Keseimbangan Tingginya." },
  { "en": "Contoh Self-Balancing Tree?", "id": "AVL Tree, Red-Black Tree." },
  { "en": "Apa Itu Heap?", "id": "Struktur Data Tree Khusus." },
  { "en": "Properti Heap?", "id": "Parent Selalu Lebih Besar/Kecil Dari Child." },
  { "en": "Apa Itu Max-Heap?", "id": "Parent Selalu Lebih Besar Dari Child." },
  { "en": "Apa Itu Min-Heap?", "id": "Parent Selalu Lebih Kecil Dari Child." },
  { "en": "Aplikasi Heap?", "id": "Priority Queue, Heapsort." },
  { "en": "Apa Itu Graph (Graf)?", "id": "Struktur Data Jaringan." },
  { "en": "Graph Terdiri Dari Apa?", "id": "Simpul (Vertex) Dan Sisi (Edge)." },
  { "en": "Apa Itu Sisi (Edge)?", "id": "Koneksi Antara Dua Simpul." },
  { "en": "Apa Itu Graph Berarah (Directed)?", "id": "Sisi Memiliki Arah." },
  { "en": "Apa Itu Graph Tak Berarah (Undirected)?", "id": "Sisi Tidak Memiliki Arah." },
  { "en": "Apa Itu Graph Berbobot (Weighted)?", "id": "Setiap Sisi Memiliki Nilai." },
  { "en": "Representasi Graph?", "id": "Adjacency Matrix, Adjacency List." },
  { "en": "Apa Itu Adjacency Matrix?", "id": "Matriks Yang Menunjukkan Koneksi Antar Simpul." },
  { "en": "Apa Itu Adjacency List?", "id": "Daftar Simpul Yang Terhubung Untuk Setiap Simpul." },
  { "en": "Apa Itu Graph Traversal?", "id": "Proses Mengunjungi Setiap Simpul." },
  { "en": "Algoritma Graph Traversal?", "id": "BFS Dan DFS." },
  { "en": "Apa Itu BFS?", "id": "Breadth-First Search." },
  { "en": "Apa Itu DFS?", "id": "Depth-First Search." },
  { "en": "Aplikasi Graph?", "id": "Jaringan Sosial, Peta, Internet." },
  { "en": "Apa Itu Hash Table?", "id": "Struktur Data Untuk Pemetaan Kunci-Nilai." },
  { "en": "Bagaimana Hash Table Bekerja?", "id": "Menggunakan Fungsi Hash Untuk Menghitung Indeks." },
  { "en": "Apa Itu Fungsi Hash?", "id": "Fungsi Yang Mengubah Kunci Menjadi Indeks." },
  { "en": "Apa Itu Kolisi (Collision)?", "id": "Dua Kunci Menghasilkan Indeks Yang Sama." },
  { "en": "Bagaimana Mengatasi Kolisi?", "id": "Chaining Atau Open Addressing." },
  { "en": "Keuntungan Hash Table?", "id": "Akses, Sisip, Hapus Sangat Cepat." },
  { "en": "Apa Itu Set?", "id": "Kumpulan Elemen Unik." },
  { "en": "Apa Itu Map (Dictionary)?", "id": "Kumpulan Pasangan Kunci-Nilai." },
  { "en": "Apa Itu Trie?", "id": "Struktur Data Tree Untuk Menyimpan String." },
  { "en": "Aplikasi Trie?", "id": "Autocomplete, Spell Checker." },
  { "en": "Apa Itu Data Kontigu?", "id": "Data Disimpan Berurutan Di Memori." },
  { "en": "Contoh Data Kontigu?", "id": "Array." },
  { "en": "Apa Itu Data Berantai?", "id": "Data Disimpan Tersebar Di Memori." },
  { "en": "Contoh Data Berantai?", "id": "Linked List." },
  { "en": "Apa Itu Array?", "id": "Struktur Data Linear Dengan Elemen Berurutan." },
  { "en": "Apa Itu Elemen?", "id": "Satu Item Data Di Dalam Array." },
  { "en": "Apa Itu Indeks?", "id": "Posisi Numerik Dari Sebuah Elemen." },
  { "en": "Indeks Array Umumnya Dimulai Dari?", "id": "Nol (Zero-based indexing)." },
  { "en": "Apa Itu Ukuran (Size) Array?", "id": "Jumlah Total Elemen Yang Dapat Disimpan." },
  { "en": "Apa Itu Panjang (Length) Array?", "id": "Jumlah Elemen Yang Saat Ini Ada." },
  { "en": "Bagaimana Mengakses Elemen Array?", "id": "Menggunakan Nama Array Dan Indeksnya." },
  { "en": "Kecepatan Akses Elemen Array?", "id": "Sangat Cepat, Waktu Konstan (O(1))." },
  { "en": "Apa Itu Array Statis?", "id": "Ukurannya Ditetapkan Saat Kompilasi." },
  { "en": "Apa Itu Array Dinamis?", "id": "Ukurannya Dapat Berubah Saat Runtime." },
  { "en": "Bagaimana Array Dinamis Bekerja?", "id": "Mengalokasikan Blok Memori Baru." },
  { "en": "Apa Itu Array Satu Dimensi?", "id": "Array Linear Sederhana." },
  { "en": "Apa Itu Array Dua Dimensi?", "id": "Array Dari Array, Membentuk Tabel." },
  { "en": "Nama Lain Array Dua Dimensi?", "id": "Matriks." },
  { "en": "Bagaimana Mengakses Elemen Array 2D?", "id": "Menggunakan Dua Indeks (Baris Dan Kolom)." },
  { "en": "Apa Itu Array Asosiatif?", "id": "Menggunakan Kunci (Key) Alih-alih Indeks Numerik." },
  { "en": "Nama Lain Array Asosiatif?", "id": "Map, Dictionary, Hash Table." },
  { "en": "Apa Operasi Umum Pada Array?", "id": "Traversal, Pencarian, Penyisipan, Penghapusan." },
  { "en": "Apa Itu Traversal?", "id": "Mengunjungi Setiap Elemen Dalam Array." },
  { "en": "Bagaimana Melakukan Traversal?", "id": "Menggunakan Loop Dari Indeks 0 Hingga Akhir." },
  { "en": "Kompleksitas Penyisipan Di Awal Array?", "id": "Lambat, O(n)." },
  { "en": "Kompleksitas Penghapusan Di Awal Array?", "id": "Lambat, O(n)." },
  { "en": "Kompleksitas Penambahan Di Akhir Array?", "id": "Cepat, O(1) (Jika Ada Ruang)." },
  { "en": "Apa Itu 'Array Index Out of Bounds'?", "id": "Error Saat Mengakses Indeks Di Luar Rentang." },
  { "en": "Apa Itu String?", "id": "Urutan Atau Array Dari Karakter." },
  { "en": "Bagaimana String Diakhiri Di Bahasa C?", "id": "Dengan Karakter Null ('\\0')." },
  { "en": "Apa Itu Substring?", "id": "Bagian Dari Sebuah String." },
  { "en": "Apa Itu Concatenation?", "id": "Menggabungkan Dua String Atau Lebih." },
  { "en": "Apa Itu Panjang String?", "id": "Jumlah Karakter Dalam String." },
  { "en": "Bagaimana Membandingkan String?", "id": "Harus Membandingkan Karakter Demi Karakter." },
  { "en": "Apa Itu Pencarian Substring?", "id": "Mencari Kemunculan Satu String Di Dalam Lainnya." },
  { "en": "Apa Itu Palindrom?", "id": "String Yang Dibaca Sama Dari Depan Belakang." },
  { "en": "Apa Itu Anagram?", "id": "Dua String Dengan Karakter Sama." },
  { "en": "Apa Itu Mutasi String?", "id": "Mengubah Isi Dari Sebuah String." },
  { "en": "Apakah String Immutable?", "id": "Di Beberapa Bahasa, Ya (Java, Python)." },
  { "en": "Apa Arti Immutable?", "id": "Nilainya Tidak Dapat Diubah Setelah Dibuat." },
  { "en": "Apa Itu Tokenization (Splitting)?", "id": "Memecah String Menjadi Bagian-Bagian." },
  { "en": "Apa Itu Delimiter?", "id": "Karakter Yang Digunakan Sebagai Pemisah." },
  { "en": "Apa Itu Regular Expression (Regex)?", "id": "Pola Untuk Mencocokkan Teks." },
  { "en": "Apa Itu Parsing?", "id": "Menganalisis String Untuk Mengekstrak Informasi." },
  { "en": "Contoh Parsing?", "id": "Mengubah String \"123\" Menjadi Angka 123." },
  { "en": "Apa Itu Konversi Case?", "id": "Mengubah Huruf Besar Ke Kecil Atau Sebaliknya." },
  { "en": "Apa Itu Trimming?", "id": "Menghapus Spasi Di Awal Dan Akhir String." },
  { "en": "Apa Itu Padding?", "id": "Menambahkan Karakter Ke String." },
  { "en": "Apa Itu Reversing String?", "id": "Membalik Urutan Karakter Dalam String." },
  { "en": "Apa Itu Matriks?", "id": "Array Dua Dimensi." },
  { "en": "Apa Itu Matriks Persegi?", "id": "Jumlah Baris Sama Dengan Jumlah Kolom." },
  { "en": "Apa Itu Diagonal Utama Matriks?", "id": "Elemen Dari Kiri Atas Ke Kanan Bawah." },
  { "en": "Apa Itu Transpose Matriks?", "id": "Menukar Baris Dan Kolom." },
  { "en": "Apa Itu Penjumlahan Matriks?", "id": "Menjumlahkan Elemen Yang Sesuai." },
  { "en": "Syarat Penjumlahan Matriks?", "id": "Harus Memiliki Dimensi Yang Sama." },
  { "en": "Apa Itu Perkalian Matriks?", "id": "Operasi Perkalian Baris-Kolom." },
  { "en": "Syarat Perkalian Matriks (A*B)?", "id": "Kolom A Harus Sama Dengan Baris B." },
  { "en": "Apa Itu Matriks Identitas?", "id": "Matriks Dengan Angka 1 Di Diagonal." },
  { "en": "Apa Itu Matriks Jarang (Sparse)?", "id": "Matriks Yang Sebagian Besar Elemennya Nol." },
  { "en": "Apa Itu Representasi Row-Major?", "id": "Menyimpan Elemen Matriks Baris Demi Baris." },
  { "en": "Apa Itu Representasi Column-Major?", "id": "Menyimpan Elemen Matriks Kolom Demi Kolom." },
  { "en": "Apa Itu Array Jagged?", "id": "Array Dari Array Dimana Baris Berbeda Panjang." },
  { "en": "Apa Itu Sliding Window?", "id": "Teknik Algoritma Menggunakan Sub-Array." },
  { "en": "Apa Itu Prefix Sum Array?", "id": "Teknik Untuk Menghitung Jumlah Rentang Cepat." },
  { "en": "Apa Itu Kadane's Algorithm?", "id": "Algoritma Mencari Sub-Array Dengan Jumlah Maksimum." },
  { "en": "Apa Itu Dutch National Flag Problem?", "id": "Mengurutkan Array Dengan Tiga Nilai." },
  { "en": "Apa Itu Two Pointers Technique?", "id": "Teknik Menggunakan Dua Indeks Dalam Array." },
  { "en": "Apa Itu Spiral Traversal?", "id": "Mengunjungi Elemen Matriks Dalam Pola Spiral." },
  { "en": "Apa Itu Rotasi Matriks?", "id": "Memutar Elemen Matriks Sebesar 90 Derajat." },
  { "en": "Apa Itu Buffer?", "id": "Area Memori Sementara." },
  { "en": "Apa Itu Circular Buffer?", "id": "Buffer Yang Menggunakan Array Secara Melingkar." },
  { "en": "Nama Lain Circular Buffer?", "id": "Ring Buffer." },
  { "en": "Aplikasi Circular Buffer?", "id": "Streaming Data, Implementasi Queue." },
  { "en": "Apa Itu Set Bit?", "id": "Menaikkan Nilai Bit Menjadi 1." },
  { "en": "Apa Itu Clear Bit?", "id": "Menurunkan Nilai Bit Menjadi 0." },
  { "en": "Apa Itu Toggle Bit?", "id": "Membalik Nilai Sebuah Bit." },
  { "en": "Apa Itu Bitmask?", "id": "Menggunakan Integer Untuk Menyimpan Status Boolean." },
  { "en": "Apa Itu Set Disjoint?", "id": "Struktur Data Untuk Melacak Partisi." },
  { "en": "Nama Lain Set Disjoint?", "id": "Union-Find." },
  { "en": "Apa Itu Encoding Karakter?", "id": "Cara Merepresentasikan Karakter Sebagai Angka." },
  { "en": "Apa Itu ASCII?", "id": "Standar Encoding Karakter Untuk Bahasa Inggris." },
  { "en": "Apa Itu UTF-8?", "id": "Encoding Karakter Paling Umum Di Web." },
  { "en": "Apa Itu String Matching?", "id": "Mencari Pola Dalam Teks." },
  { "en": "Algoritma String Matching Naif?", "id": "Memeriksa Setiap Kemungkinan Posisi." },
  { "en": "Apa Itu Algoritma KMP?", "id": "Algoritma String Matching Yang Efisien." },
  { "en": "Apa Itu Algoritma Rabin-Karp?", "id": "Menggunakan Hashing Untuk String Matching." },
  { "en": "Apa Itu Longest Common Subsequence?", "id": "Sub-Urutan Terpanjang Yang Muncul." },
  { "en": "Apa Itu Edit Distance?", "id": "Jumlah Operasi Minimum Mengubah String." },
  { "en": "Operasi Edit Distance?", "id": "Sisip, Hapus, Ganti." },
  { "en": "Nama Lain Edit Distance?", "id": "Levenshtein Distance." },
  { "en": "Apa Itu Suffix Tree?", "id": "Struktur Data Untuk Operasi String Cepat." },
  { "en": "Apa Itu Suffix Array?", "id": "Array Terurut Dari Semua Suffix." },
  { "en": "Apa Itu Pointer Aritmatika?", "id": "Melakukan Operasi Matematika Pada Alamat Memori." },
  { "en": "Hubungan Pointer Dan Array?", "id": "Nama Array Adalah Pointer Ke Elemen Pertama." },
  { "en": "Apa Itu Array of Pointers?", "id": "Array Yang Elemennya Adalah Alamat Memori." },
  { "en": "Apa Itu Pointer to Pointer?", "id": "Pointer Yang Menunjuk Ke Pointer Lain." },
  { "en": "Apa Itu Algoritma?", "id": "Urutan Langkah Untuk Menyelesaikan Suatu Tugas." },
  { "en": "Karakteristik Algoritma Yang Baik?", "id": "Benar, Efisien, Terdefinisi, Finit." },
  { "en": "Apa Artinya 'Benar'?", "id": "Memberikan Output Yang Sesuai Untuk Semua Input." },
  { "en": "Apa Artinya 'Efisien'?", "id": "Menggunakan Sumber Daya (Waktu, Memori) Minimum." },
  { "en": "Apa Artinya 'Terdefinisi'?", "id": "Setiap Langkah Jelas Dan Tidak Ambigu." },
  { "en": "Apa Artinya 'Finit'?", "id": "Algoritma Harus Selalu Berakhir." },
  { "en": "Bagaimana Merepresentasikan Algoritma?", "id": "Pseudocode, Flowchart, Bahasa Natural." },
  { "en": "Apa Itu Analisis Algoritma?", "id": "Mempelajari Kinerja Dan Efisiensi Algoritma." },
  { "en": "Apa Itu Kompleksitas Waktu?", "id": "Ukuran Waktu Eksekusi Algoritma." },
  { "en": "Apa Itu Kompleksitas Ruang?", "id": "Ukuran Memori Yang Digunakan Algoritma." },
  { "en": "Apa Itu Notasi Big O?", "id": "Mendeskripsikan Perilaku Asimtotik (Batas Atas)." },
  { "en": "Apa Itu Asimtotik?", "id": "Perilaku Algoritma Saat Ukuran Input Tumbuh." },
  { "en": "Apa Itu O(1)?", "id": "Waktu Konstan." },
  { "en": "Apa Itu O(log n)?", "id": "Waktu Logaritmik." },
  { "en": "Apa Itu O(n)?", "id": "Waktu Linear." },
  { "en": "Apa Itu O(n log n)?", "id": "Waktu Log-Linear." },
  { "en": "Apa Itu O(n²)?", "id": "Waktu Kuadratik." },
  { "en": "Apa Itu O(2^n)?", "id": "Waktu Eksponensial." },
  { "en": "Apa Itu O(n!)?", "id": "Waktu Faktorial." },
  { "en": "Algoritma Mana Yang Lebih Cepat, O(n) Atau O(n²)?", "id": "O(n)." },
  { "en": "Apa Itu Kasus Terbaik (Best Case)?", "id": "Input Yang Membuat Algoritma Tercepat." },
  { "en": "Apa Itu Kasus Terburuk (Worst Case)?", "id": "Input Yang Membuat Algoritma Terlambat." },
  { "en": "Apa Itu Kasus Rata-rata (Average Case)?", "id": "Performa Rata-rata Untuk Semua Input." },
  { "en": "Analisis Biasanya Fokus Pada Kasus Mana?", "id": "Kasus Terburuk (Worst Case)." },
  { "en": "Apa Itu Algoritma Brute-Force?", "id": "Mencoba Semua Kemungkinan Solusi." },
  { "en": "Apa Itu Algoritma Greedy?", "id": "Membuat Pilihan Terbaik Lokal Saat Ini." },
  { "en": "Apakah Algoritma Greedy Selalu Optimal?", "id": "Tidak Selalu, Tapi Seringkali Cukup Baik." },
  { "en": "Apa Itu Algoritma Divide and Conquer?", "id": "Memecah Masalah, Menyelesaikan, Menggabungkan." },
  { "en": "Tiga Langkah Divide and Conquer?", "id": "Divide, Conquer, Combine." },
  { "en": "Contoh Divide and Conquer?", "id": "Merge Sort, Quick Sort, Binary Search." },
  { "en": "Apa Itu Pemrograman Dinamis (Dynamic Programming)?", "id": "Memecahkan Masalah Dengan Menyimpan Hasil Sub-masalah." },
  { "en": "Properti Masalah DP?", "id": "Substruktur Optimal Dan Sub-masalah Tumpang Tindih." },
  { "en": "Apa Itu Memoization?", "id": "Teknik Top-Down Dalam DP." },
  { "en": "Apa Itu Tabulation?", "id": "Teknik Bottom-Up Dalam DP." },
  { "en": "Contoh Masalah DP?", "id": "Fibonacci, Knapsack Problem." },
  { "en": "Apa Itu Algoritma Backtracking?", "id": "Membangun Solusi Secara Bertahap." },
  { "en": "Bagaimana Backtracking Bekerja?", "id": "Mundur Jika Jalan Saat Ini Tidak Menuju Solusi." },
  { "en": "Contoh Masalah Backtracking?", "id": "N-Queens Problem, Sudoku Solver." },
  { "en": "Apa Itu Algoritma Rekursif?", "id": "Algoritma Yang Memanggil Dirinya Sendiri." },
  { "en": "Apa Itu Algoritma Iteratif?", "id": "Algoritma Yang Menggunakan Perulangan." },
  { "en": "Apa Itu Algoritma Pencarian (Searching)?", "id": "Menemukan Item Tertentu Dalam Kumpulan Data." },
  { "en": "Apa Itu Algoritma Pengurutan (Sorting)?", "id": "Menyusun Elemen Dalam Urutan Tertentu." },
  { "en": "Apa Itu Algoritma Graf?", "id": "Algoritma Yang Beroperasi Pada Struktur Graf." },
  { "en": "Apa Itu Algoritma Acak (Randomized)?", "id": "Menggunakan Elemen Acak Dalam Logikanya." },
  { "en": "Apa Itu Algoritma Heuristik?", "id": "Algoritma Cepat Yang Menemukan Solusi Aproksimasi." },
  { "en": "Apa Itu Algoritma Aproksimasi?", "id": "Menemukan Solusi Yang Cukup Dekat Dengan Optimal." },
  { "en": "Apa Itu Paradigma Algoritma?", "id": "Pendekatan Atau Strategi Umum Untuk Desain." },
  { "en": "Apa Itu Trade-off?", "id": "Mengorbankan Satu Hal Untuk Mendapatkan Hal Lain." },
  { "en": "Contoh Trade-off?", "id": "Waktu vs Ruang (Memori)." },
  { "en": "Apa Itu Lower Bound?", "id": "Kompleksitas Minimum Teoretis Untuk Suatu Masalah." },
  { "en": "Apa Itu Upper Bound?", "id": "Kompleksitas Kasus Terburuk Dari Suatu Algoritma." },
  { "en": "Apa Itu Notasi Omega (Ω)?", "id": "Mendeskripsikan Batas Bawah Asimtotik." },
  { "en": "Apa Itu Notasi Theta (Θ)?", "id": "Mendeskripsikan Batas Ketat Asimtotik." },
  { "en": "Apa Itu Analisis Amortisasi?", "id": "Menganalisis Biaya Rata-rata Serangkaian Operasi." },
  { "en": "Apa Itu Optimisasi?", "id": "Proses Membuat Sesuatu Menjadi Terbaik." },
  { "en": "Apa Itu Masalah Optimisasi?", "id": "Mencari Solusi Terbaik Dari Semua Solusi." },
  { "en": "Apa Itu Fungsi Objektif?", "id": "Fungsi Yang Ingin Dimaksimalkan Atau Diminimalkan." },
  { "en": "Apa Itu Kendala (Constraint)?", "id": "Kondisi Yang Harus Dipenuhi Oleh Solusi." },
  { "en": "Apa Itu Pemrograman Linear?", "id": "Masalah Optimisasi Dengan Fungsi Linear." },
  { "en": "Apa Itu Bukti Kebenaran Algoritma?", "id": "Membuktikan Secara Formal Bahwa Algoritma Benar." },
  { "en": "Apa Itu Induksi Matematika?", "id": "Teknik Pembuktian Yang Sering Digunakan." },
  { "en": "Apa Itu Loop Invariant?", "id": "Kondisi Yang Benar Sebelum Dan Sesudah Iterasi." },
  { "en": "Apa Itu In-place Algorithm?", "id": "Algoritma Yang Tidak Membutuhkan Memori Tambahan." },
  { "en": "Apa Itu Stable Algorithm?", "id": "Algoritma Pengurutan Yang Menjaga Urutan Relatif." },
  { "en": "Apa Itu Adaptive Algorithm?", "id": "Performa Membaik Jika Input Hampir Terurut." },
  { "en": "Apa Itu Algoritma Online?", "id": "Memproses Input Sedikit Demi Sedikit." },
  { "en": "Apa Itu Algoritma Offline?", "id": "Membutuhkan Semua Input Sejak Awal." },
  { "en": "Apa Itu Masalah Keputusan?", "id": "Masalah Dengan Jawaban Ya Atau Tidak." },
  { "en": "Apa Itu Kelas P?", "id": "Masalah Yang Dapat Diselesaikan Dalam Waktu Polinomial." },
  { "en": "Apa Itu Kelas NP?", "id": "Masalah Yang Solusinya Dapat Diverifikasi Cepat." },
  { "en": "Apa Itu P vs NP Problem?", "id": "Pertanyaan Terbuka Terbesar Dalam Ilmu Komputer." },
  { "en": "Apa Itu Masalah NP-Complete?", "id": "Masalah Tersulit Di Kelas NP." },
  { "en": "Apa Itu Reduksi?", "id": "Mengubah Satu Masalah Menjadi Masalah Lain." },
  { "en": "Apa Itu Desain Algoritma?", "id": "Proses Menciptakan Sebuah Algoritma." },
  { "en": "Pentingnya Memahami Masalah?", "id": "Langkah Pertama Sebelum Merancang Algoritma." },
  { "en": "Pentingnya Memilih Struktur Data?", "id": "Sangat Mempengaruhi Desain Dan Efisiensi." },
  { "en": "Apa Itu Kompleksitas Komputasi?", "id": "Studi Tentang Sumber Daya Yang Dibutuhkan." },
  { "en": "Apa Itu Pre-computation?", "id": "Melakukan Perhitungan Di Awal Untuk Menghemat Waktu." },
  { "en": "Contoh Pre-computation?", "id": "Membangun Tabel Pencarian." },
  { "en": "Apa Itu Pemrograman Kompetitif?", "id": "Ajang Adu Cepat Menyelesaikan Masalah Algoritmik." },
  { "en": "Pentingnya Latihan?", "id": "Kunci Untuk Menjadi Baik Dalam Algoritma." },
  { "en": "Apa Itu Koreksi (Correctness)?", "id": "Apakah Algoritma Memberikan Jawaban Benar." },
  { "en": "Apa Itu Kompleksitas (Complexity)?", "id": "Berapa Banyak Sumber Daya Yang Digunakan." },
  { "en": "Apa Itu Kejelasan (Clarity)?", "id": "Seberapa Mudah Algoritma Dipahami." },
  { "en": "Apa Itu Kesederhanaan (Simplicity)?", "id": "Apakah Algoritma Sederhana Untuk Diimplementasikan." },
  { "en": "Apa Itu Optimalitas?", "id": "Apakah Ada Algoritma Yang Lebih Baik." },
  { "en": "Apa Itu Branch and Bound?", "id": "Algoritma Pencarian Untuk Masalah Optimisasi." },
  { "en": "Apa Itu Algoritma Geometris?", "id": "Algoritma Untuk Menyelesaikan Masalah Geometris." },
  { "en": "Apa Itu Convex Hull?", "id": "Bentuk Konveks Terkecil Yang Melingkupi Titik." },
  { "en": "Apa Itu Algoritma Numerik?", "id": "Algoritma Untuk Menyelesaikan Masalah Matematis." },
  { "en": "Apa Itu Akar Persamaan?", "id": "Mencari Nilai X Dimana f(x)=0." },
  { "en": "Apa Itu Algoritma String?", "id": "Algoritma Yang Beroperasi Pada Teks." },
  { "en": "Apa Itu Kriptografi?", "id": "Ilmu Mengamankan Komunikasi." },
  { "en": "Apa Itu Algoritma Enkripsi?", "id": "Mengubah Teks Biasa Menjadi Teks Sandi." },
  { "en": "Apa Itu Pengurutan (Sorting)?", "id": "Menyusun Data Dalam Urutan Tertentu." },
  { "en": "Urutan Apa Saja?", "id": "Naik (Ascending) Atau Turun (Descending)." },
  { "en": "Mengapa Pengurutan Penting?", "id": "Memudahkan Pencarian Dan Pemrosesan Data." },
  { "en": "Apa Itu Kunci (Key)?", "id": "Nilai Yang Digunakan Sebagai Dasar Pengurutan." },
  { "en": "Apa Itu Pengurutan Internal?", "id": "Semua Data Muat Di Memori Utama (RAM)." },
  { "en": "Apa Itu Pengurutan Eksternal?", "id": "Data Terlalu Besar, Disimpan Di Disk." },
  { "en": "Apa Itu Pengurutan Stabil (Stable)?", "id": "Menjaga Urutan Relatif Elemen Yang Sama." },
  { "en": "Apa Itu Pengurutan In-place?", "id": "Hanya Membutuhkan Memori Tambahan Yang Konstan." },
  { "en": "Apa Itu Bubble Sort?", "id": "Algoritma Pengurutan Sederhana." },
  { "en": "Bagaimana Bubble Sort Bekerja?", "id": "Membandingkan Dan Menukar Elemen Berdampingan." },
  { "en": "Kompleksitas Waktu Bubble Sort?", "id": "O(n²)." },
  { "en": "Apakah Bubble Sort Stabil?", "id": "Ya, Jika Diimplementasikan Dengan Benar." },
  { "en": "Apakah Bubble Sort In-place?", "id": "Ya." },
  { "en": "Apa Itu Selection Sort?", "id": "Algoritma Pengurutan Berbasis Pemilihan." },
  { "en": "Bagaimana Selection Sort Bekerja?", "id": "Mencari Minimum, Menukar Ke Posisi Awal." },
  { "en": "Kompleksitas Waktu Selection Sort?", "id": "O(n²)." },
  { "en": "Apakah Selection Sort Stabil?", "id": "Tidak, Secara Umum Tidak Stabil." },
  { "en": "Apakah Selection Sort In-place?", "id": "Ya." },
  { "en": "Apa Itu Insertion Sort?", "id": "Algoritma Pengurutan Berbasis Penyisipan." },
  { "en": "Bagaimana Insertion Sort Bekerja?", "id": "Menyisipkan Elemen Ke Posisi Benar." },
  { "en": "Kompleksitas Waktu Insertion Sort?", "id": "O(n²) Kasus Terburuk." },
  { "en": "Kasus Terbaik Insertion Sort?", "id": "O(n), Jika Array Sudah Terurut." },
  { "en": "Apakah Insertion Sort Stabil?", "id": "Ya." },
  { "en": "Apakah Insertion Sort In-place?", "id": "Ya." },
  { "en": "Kapan Insertion Sort Baik Digunakan?", "id": "Untuk Kumpulan Data Kecil Atau Hampir Terurut." },
  { "en": "Apa Itu Shell Sort?", "id": "Peningkatan Dari Insertion Sort." },
  { "en": "Bagaimana Shell Sort Bekerja?", "id": "Mengurutkan Elemen Dengan Jarak Tertentu." },
  { "en": "Apa Itu Merge Sort?", "id": "Algoritma Pengurutan Divide and Conquer." },
  { "en": "Bagaimana Merge Sort Bekerja?", "id": "Membagi, Mengurutkan, Dan Menggabungkan." },
  { "en": "Kompleksitas Waktu Merge Sort?", "id": "O(n log n)." },
  { "en": "Apakah Merge Sort Stabil?", "id": "Ya." },
  { "en": "Apakah Merge Sort In-place?", "id": "Tidak, Membutuhkan Memori Tambahan." },
  { "en": "Apa Itu Operasi 'Merge'?", "id": "Menggabungkan Dua Sub-Array Terurut." },
  { "en": "Apa Itu Quick Sort?", "id": "Algoritma Pengurutan Divide and Conquer Lainnya." },
  { "en": "Bagaimana Quick Sort Bekerja?", "id": "Memilih Pivot, Mempartisi Array." },
  { "en": "Apa Itu Pivot?", "id": "Elemen Yang Digunakan Untuk Mempartisi." },
  { "en": "Apa Itu Partisi?", "id": "Menyusun Ulang Elemen Di Sekitar Pivot." },
  { "en": "Kompleksitas Waktu Rata-rata Quick Sort?", "id": "O(n log n)." },
  { "en": "Kompleksitas Waktu Terburuk Quick Sort?", "id": "O(n²)." },
  { "en": "Kapan Kasus Terburuk Quick Sort Terjadi?", "id": "Saat Pivot Selalu Elemen Terkecil/Terbesar." },
  { "en": "Apakah Quick Sort Stabil?", "id": "Tidak, Secara Umum Tidak Stabil." },
  { "en": "Apakah Quick Sort In-place?", "id": "Ya, Biasanya In-place." },
  { "en": "Apa Itu Heap Sort?", "id": "Algoritma Pengurutan Berbasis Struktur Data Heap." },
  { "en": "Bagaimana Heap Sort Bekerja?", "id": "Membangun Heap, Kemudian Mengekstrak Elemen." },
  { "en": "Kompleksitas Waktu Heap Sort?", "id": "O(n log n)." },
  { "en": "Apakah Heap Sort Stabil?", "id": "Tidak." },
  { "en": "Apakah Heap Sort In-place?", "id": "Ya." },
  { "en": "Apa Itu Counting Sort?", "id": "Algoritma Pengurutan Non-Perbandingan." },
  { "en": "Bagaimana Counting Sort Bekerja?", "id": "Menghitung Frekuensi Setiap Elemen." },
  { "en": "Kapan Counting Sort Efisien?", "id": "Saat Rentang Nilai Elemen Kecil." },
  { "en": "Kompleksitas Waktu Counting Sort?", "id": "O(n + k), Dimana k Adalah Rentang." },
  { "en": "Apakah Counting Sort Stabil?", "id": "Ya, Dapat Dibuat Stabil." },
  { "en": "Apakah Counting Sort In-place?", "id": "Tidak." },
  { "en": "Apa Itu Radix Sort?", "id": "Mengurutkan Berdasarkan Digit Individual." },
  { "en": "Bagaimana Radix Sort Bekerja?", "id": "Mengurutkan Dari Digit Terendah Ke Tertinggi." },
  { "en": "Kompleksitas Waktu Radix Sort?", "id": "O(d * (n + b))." },
  { "en": "Apakah Radix Sort Stabil?", "id": "Ya, Jika Pengurutan Perantara Stabil." },
  { "en": "Apakah Radix Sort In-place?", "id": "Tidak." },
  { "en": "Apa Itu Bucket Sort (Bin Sort)?", "id": "Mendistribusikan Elemen Ke Dalam Bucket." },
  { "en": "Bagaimana Bucket Sort Bekerja?", "id": "Mengurutkan Setiap Bucket, Lalu Menggabungkan." },
  { "en": "Kompleksitas Waktu Rata-rata Bucket Sort?", "id": "O(n)." },
  { "en": "Apa Itu Timsort?", "id": "Algoritma Hibrida." },
  { "en": "Gabungan Apa Timsort Itu?", "id": "Merge Sort Dan Insertion Sort." },
  { "en": "Di Mana Timsort Digunakan?", "id": "Pengurutan Default Di Python Dan Java." },
  { "en": "Apa Itu Introsort?", "id": "Algoritma Hibrida Lainnya." },
  { "en": "Gabungan Apa Introsort Itu?", "id": "Quick Sort, Heap Sort, Insertion Sort." },
  { "en": "Tujuan Introsort?", "id": "Menghindari Kasus Terburuk Quick Sort." },
  { "en": "Apa Itu Bogo Sort?", "id": "Algoritma Pengurutan Yang Sangat Tidak Efisien." },
  { "en": "Bagaimana Bogo Sort Bekerja?", "id": "Mengacak Array Sampai Terurut." },
  { "en": "Apa Itu Pengurutan Berbasis Perbandingan?", "id": "Algoritma Yang Menggunakan Operasi Perbandingan." },
  { "en": "Contoh Pengurutan Berbasis Perbandingan?", "id": "Bubble, Merge, Quick Sort." },
  { "en": "Batas Bawah Pengurutan Berbasis Perbandingan?", "id": "Ω(n log n)." },
  { "en": "Apa Itu Pengurutan Non-Perbandingan?", "id": "Tidak Bergantung Pada Perbandingan Elemen." },
  { "en": "Contoh Pengurutan Non-Perbandingan?", "id": "Counting Sort, Radix Sort." },
  { "en": "Apa Itu Topologi Sort?", "id": "Pengurutan Linear Untuk Simpul Graf Berarah." },
  { "en": "Apa Itu Pancake Sort?", "id": "Mengurutkan Dengan Membalik Prefiks." },
  { "en": "Apa Itu Cocktail Shaker Sort?", "id": "Varian Bubble Sort Dua Arah." },
  { "en": "Nama Lain Cocktail Shaker Sort?", "id": "Bidirectional Bubble Sort." },
  { "en": "Apa Itu Comb Sort?", "id": "Peningkatan Lain Dari Bubble Sort." },
  { "en": "Apa Itu Gnome Sort?", "id": "Mirip Insertion Sort Tapi Lebih Sederhana." },
  { "en": "Apa Itu Sleep Sort?", "id": "Algoritma Pengurutan Konseptual." },
  { "en": "Apa Itu Cycle Sort?", "id": "Optimal Dalam Jumlah Penulisan Ke Memori." },
  { "en": "Apa Itu Postman's Sort?", "id": "Varian Dari Bucket Sort." },
  { "en": "Apa Itu Library Sort Function?", "id": "Fungsi Pengurutan Bawaan Bahasa Pemrograman." },
  { "en": "Apakah Cukup Menggunakan Library Sort?", "id": "Ya, Untuk Sebagian Besar Kasus." },
  { "en": "Kapan Perlu Implementasi Sendiri?", "id": "Untuk Pembelajaran Atau Persyaratan Khusus." },
  { "en": "Pengurutan Elemen String?", "id": "Biasanya Berdasarkan Urutan Leksikografis." },
  { "en": "Pengurutan Objek?", "id": "Harus Mendefinisikan Kriteria Perbandingan." },
  { "en": "Apa Itu Kunci Komposit?", "id": "Mengurutkan Berdasarkan Beberapa Atribut." },
  { "en": "Pengurutan Terdistribusi?", "id": "Mengurutkan Data Di Banyak Komputer." },
  { "en": "Apa Itu Masalah Pemilihan?", "id": "Mencari Elemen Terkecil Ke-k." },
  { "en": "Algoritma Quickselect?", "id": "Algoritma Efisien Untuk Masalah Pemilihan." },
  { "en": "Apa Itu Median?", "id": "Nilai Tengah Dari Kumpulan Data." },
  { "en": "Apa Itu Pencarian (Searching)?", "id": "Proses Menemukan Lokasi Suatu Elemen." },
  { "en": "Apa Itu Kunci Pencarian (Search Key)?", "id": "Nilai Yang Ingin Dicari." },
  { "en": "Hasil Pencarian Berhasil?", "id": "Elemen Ditemukan, Mengembalikan Lokasi." },
  { "en": "Hasil Pencarian Gagal?", "id": "Elemen Tidak Ditemukan." },
  { "en": "Apa Itu Pencarian Linear?", "id": "Algoritma Pencarian Paling Dasar." },
  { "en": "Nama Lain Pencarian Linear?", "id": "Pencarian Sekuensial." },
  { "en": "Bagaimana Pencarian Linear Bekerja?", "id": "Memeriksa Setiap Elemen Satu Per Satu." },
  { "en": "Kapan Pencarian Linear Digunakan?", "id": "Untuk Data Yang Tidak Terurut." },
  { "en": "Kompleksitas Waktu Kasus Terburuk Linear Search?", "id": "O(n)." },
  { "en": "Kompleksitas Waktu Kasus Rata-rata Linear Search?", "id": "O(n)." },
  { "en": "Apa Itu Pencarian Biner?", "id": "Algoritma Pencarian Yang Sangat Efisien." },
  { "en": "Syarat Utama Pencarian Biner?", "id": "Kumpulan Data Harus Sudah Terurut." },
  { "en": "Bagaimana Pencarian Biner Bekerja?", "id": "Membagi Ruang Pencarian Menjadi Dua." },
  { "en": "Langkah Pencarian Biner?", "id": "Bandingkan Dengan Elemen Tengah." },
  { "en": "Kompleksitas Waktu Pencarian Biner?", "id": "O(log n)." },
  { "en": "Apakah Pencarian Biner Lebih Cepat?", "id": "Ya, Jauh Lebih Cepat Dari Linear." },
  { "en": "Apa Itu Pencarian Interpolasi?", "id": "Peningkatan Dari Pencarian Biner." },
  { "en": "Asumsi Pencarian Interpolasi?", "id": "Data Terdistribusi Secara Seragam." },
  { "en": "Bagaimana Pencarian Interpolasi Bekerja?", "id": "Menebak Posisi Berdasarkan Nilai Kunci." },
  { "en": "Kompleksitas Rata-rata Pencarian Interpolasi?", "id": "O(log log n)." },
  { "en": "Apa Itu Jump Search?", "id": "Melompat Maju Dengan Blok Berukuran Tetap." },
  { "en": "Kompleksitas Waktu Jump Search?", "id": "O(√n)." },
  { "en": "Apa Itu Exponential Search?", "id": "Mencari Rentang, Kemudian Pencarian Biner." },
  { "en": "Kapan Exponential Search Berguna?", "id": "Untuk Kumpulan Data Tidak Terbatas." },
  { "en": "Apa Itu Ternary Search?", "id": "Membagi Ruang Pencarian Menjadi Tiga." },
  { "en": "Apa Itu Hashing?", "id": "Teknik Pemetaan Kunci Ke Indeks." },
  { "en": "Struktur Data Yang Menggunakan Hashing?", "id": "Hash Table." },
  { "en": "Kompleksitas Waktu Pencarian Di Hash Table?", "id": "Rata-rata O(1)." },
  { "en": "Apa Itu Pencarian Pada Tree?", "id": "Melintasi Pohon Untuk Menemukan Nilai." },
  { "en": "Apa Itu Binary Search Tree (BST)?", "id": "Pohon Yang Dioptimalkan Untuk Pencarian." },
  { "en": "Kompleksitas Pencarian Di BST Seimbang?", "id": "O(log n)." },
  { "en": "Apa Itu Pencarian Graf?", "id": "Menjelajahi Graf Untuk Menemukan Simpul." },
  { "en": "Apa Itu Breadth-First Search (BFS)?", "id": "Menjelajahi Level Demi Level." },
  { "en": "Struktur Data Untuk BFS?", "id": "Menggunakan Queue (Antrean)." },
  { "en": "Apa Itu Depth-First Search (DFS)?", "id": "Menjelajahi Sedalam Mungkin Sebelum Mundur." },
  { "en": "Struktur Data Untuk DFS?", "id": "Menggunakan Stack (Tumpukan)." },
  { "en": "Aplikasi BFS?", "id": "Mencari Jalur Terpendek (Tak Berbobot)." },
  { "en": "Aplikasi DFS?", "id": "Deteksi Siklus, Pengurutan Topologis." },
  { "en": "Apa Itu Pencarian Substring?", "id": "Mencari Pola Di Dalam Teks." },
  { "en": "Nama Lain Pencarian Substring?", "id": "String Matching." },
  { "en": "Algoritma String Matching Naif?", "id": "Memeriksa Setiap Posisi Awal." },
  { "en": "Kompleksitas Algoritma Naif?", "id": "O(m*n)." },
  { "en": "Apa Itu Algoritma Knuth-Morris-Pratt (KMP)?", "id": "Algoritma Pencarian String Efisien." },
  { "en": "Apa Itu Algoritma Boyer-Moore?", "id": "Algoritma Pencarian String Cepat Lainnya." },
  { "en": "Apa Itu Algoritma Rabin-Karp?", "id": "Menggunakan Hashing Untuk Pencarian String." },
  { "en": "Apa Itu Pencarian Heuristik?", "id": "Menggunakan Fungsi Heuristik Untuk Memandu Pencarian." },
  { "en": "Apa Itu Fungsi Heuristik?", "id": "Estimasi Biaya Dari Keadaan Saat Ini." },
  { "en": "Apa Itu Algoritma A* (A-Star)?", "id": "Algoritma Pencarian Graf Heuristik." },
  { "en": "Aplikasi A*?", "id": "Pencarian Jalur Di Game, Robotika." },
  { "en": "Apa Itu Greedy Best-First Search?", "id": "Hanya Mempertimbangkan Biaya Heuristik." },
  { "en": "Apa Itu Uniform Cost Search (UCS)?", "id": "Mencari Jalur Dengan Biaya Terendah." },
  { "en": "Apa Itu Pencarian Lokal?", "id": "Memulai Dari Solusi Dan Mencari Tetangga." },
  { "en": "Apa Itu Hill Climbing?", "id": "Pencarian Lokal Yang Selalu Bergerak Naik." },
  { "en": "Masalah Hill Climbing?", "id": "Dapat Terjebak Di Optimum Lokal." },
  { "en": "Apa Itu Simulated Annealing?", "id": "Pencarian Lokal Yang Dapat Menerima Langkah Buruk." },
  { "en": "Apa Itu Algoritma Genetika?", "id": "Pencarian Terinspirasi Evolusi Biologis." },
  { "en": "Apa Itu Pencarian Adversarial?", "id": "Pencarian Dalam Konteks Permainan." },
  { "en": "Contoh Pencarian Adversarial?", "id": "Catur, Tic-Tac-Toe." },
  { "en": "Apa Itu Algoritma Minimax?", "id": "Algoritma Dasar Untuk Permainan Dua Pemain." },
  { "en": "Apa Itu Alpha-Beta Pruning?", "id": "Optimisasi Untuk Algoritma Minimax." },
  { "en": "Apa Itu Monte Carlo Tree Search (MCTS)?", "id": "Teknik Pencarian Modern Untuk Permainan Kompleks." },
  { "en": "Apa Itu Pencarian Eksternal?", "id": "Mencari Data Di Penyimpanan Eksternal." },
  { "en": "Contoh Pencarian Eksternal?", "id": "Mencari Di File Besar Atau Database." },
  { "en": "Apa Itu Pengindeksan (Indexing)?", "id": "Membuat Struktur Data Untuk Mempercepat Pencarian." },
  { "en": "Contoh Indeks?", "id": "B-Tree Di Database." },
  { "en": "Apa Itu Pencarian Teks Penuh (Full-Text Search)?", "id": "Mencari Kata Di Dalam Kumpulan Dokumen." },
  { "en": "Apa Itu Inverted Index?", "id": "Struktur Data Inti Untuk Mesin Pencari." },
  { "en": "Apa Itu Mesin Pencari (Search Engine)?", "id": "Sistem Untuk Menemukan Informasi Di Web." },
  { "en": "Bagaimana Mesin Pencari Bekerja?", "id": "Crawling, Indexing, Ranking." },
  { "en": "Apa Itu Crawling?", "id": "Proses Mengumpulkan Halaman Web." },
  { "en": "Apa Itu PageRank?", "id": "Algoritma Ranking Awal Google." },
  { "en": "Apa Itu Pencarian Terdekat (Nearest Neighbor)?", "id": "Menemukan Titik Data Terdekat." },
  { "en": "Aplikasi Pencarian Terdekat?", "id": "Sistem Rekomendasi, Pengenalan Pola." },
  { "en": "Apa Itu k-d Tree?", "id": "Struktur Data Untuk Mempercepat Pencarian Terdekat." },
  { "en": "Apa Itu Fibonacci Search?", "id": "Mirip Pencarian Biner Tapi Menggunakan Fibonacci." },
  { "en": "Apa Itu Sentinel Linear Search?", "id": "Optimisasi Kecil Untuk Pencarian Linear." },
  { "en": "Pencarian Untuk Elemen Terkecil/Terbesar?", "id": "Dapat Dilakukan Dalam Waktu Linear." },
  { "en": "Apa Itu Masalah Pencarian?", "id": "Salah Satu Masalah Fundamental Dalam Ilmu Komputer." },
  { "en": "Pencarian vs Pengurutan?", "id": "Pengurutan Seringkali Membantu Mempercepat Pencarian." },
  { "en": "Apa Itu Tabel Simbol?", "id": "Struktur Data Yang Memetakan Kunci Ke Nilai." },
  { "en": "Pencarian Pada Tabel Simbol?", "id": "Menemukan Nilai Berdasarkan Kunci." },
  { "en": "Apa Itu Hash Collision?", "id": "Saat Dua Kunci Berbeda Menghasilkan Hash Sama." },
  { "en": "Pencarian Pada Linked List?", "id": "Harus Dilakukan Secara Linear." },
  { "en": "Pencarian Pada Array Tak Terurut?", "id": "Pencarian Linear." },
  { "en": "Pencarian Pada Array Terurut?", "id": "Pencarian Biner." },
  { "en": "Apa Itu Bidirectional Search?", "id": "Mencari Dari Awal Dan Akhir Secara Bersamaan." },
  { "en": "Apa Itu Beam Search?", "id": "Varian Dari Best-First Search." },
  { "en": "Apa Itu Iterative Deepening DFS (IDDFS)?", "id": "Menggabungkan Keuntungan DFS Dan BFS." },
  { "en": "Apa Itu Ruang Pencarian (Search Space)?", "id": "Kumpulan Semua Kemungkinan Solusi." },
  { "en": "Pencarian Brute-Force?", "id": "Mengeksplorasi Seluruh Ruang Pencarian." },
  { "en": "Apa Itu Pruning?", "id": "Menghilangkan Cabang Pencarian Yang Tidak Menjanjikan." },
  { "en": "Apa Itu Query?", "id": "Permintaan Untuk Mencari Informasi." },
  { "en": "Apa Itu Database?", "id": "Kumpulan Data Terstruktur." },
  { "en": "Pencarian Dalam Database?", "id": "Menggunakan Bahasa Query Seperti SQL." },
  { "en": "Apa Itu Optimalitas Pencarian?", "id": "Apakah Algoritma Dijamin Menemukan Solusi Terbaik." },
  { "en": "BFS Optimal?", "id": "Ya, Untuk Jalur Terpendek." },
  { "en": "DFS Optimal?", "id": "Tidak Selalu." },
  { "en": "A* Optimal?", "id": "Ya, Jika Heuristiknya Admisibel." },
  { "en": "Apa Itu Analisis Kompleksitas?", "id": "Mempelajari Kebutuhan Sumber Daya Algoritma." },
  { "en": "Sumber Daya Apa Yang Dianalisis?", "id": "Waktu (Kompleksitas Waktu) Dan Memori (Kompleksitas Ruang)." },
  { "en": "Mengapa Analisis Kompleksitas Penting?", "id": "Untuk Membandingkan Dan Memilih Algoritma Terbaik." },
  { "en": "Apa Itu Ukuran Input (n)?", "id": "Parameter Yang Merepresentasikan Ukuran Masalah." },
  { "en": "Apa Itu Kompleksitas Waktu?", "id": "Bagaimana Waktu Eksekusi Tumbuh Dengan Ukuran Input." },
  { "en": "Apa Itu Kompleksitas Ruang?", "id": "Bagaimana Kebutuhan Memori Tumbuh Dengan Ukuran Input." },
  { "en": "Apa Itu Notasi Asimtotik?", "id": "Mendeskripsikan Perilaku Algoritma Untuk Input Besar." },
  { "en": "Tiga Notasi Asimtotik Utama?", "id": "Big O, Big Omega, Big Theta." },
  { "en": "Apa Itu Notasi Big O?", "id": "Memberikan Batas Atas (Upper Bound)." },
  { "en": "Apa Arti O(f(n))?", "id": "Waktu Tumbuh Tidak Lebih Cepat Dari f(n)." },
  { "en": "Notasi Big O Fokus Pada?", "id": "Kasus Terburuk (Worst-case)." },
  { "en": "Apa Itu Notasi Big Omega (Ω)?", "id": "Memberikan Batas Bawah (Lower Bound)." },
  { "en": "Apa Arti Ω(f(n))?", "id": "Waktu Tumbuh Tidak Lebih Lambat Dari f(n)." },
  { "en": "Notasi Omega Fokus Pada?", "id": "Kasus Terbaik (Best-case)." },
  { "en": "Apa Itu Notasi Big Theta (Θ)?", "id": "Memberikan Batas Ketat (Tight Bound)." },
  { "en": "Apa Arti Θ(f(n))?", "id": "Waktu Tumbuh Sama Cepatnya Dengan f(n)." },
  { "en": "Notasi Theta Fokus Pada?", "id": "Kasus Rata-rata (Average-case)." },
  { "en": "Kelas Kompleksitas Umum?", "id": "Konstan, Logaritmik, Linear, Kuadratik, Eksponensial." },
  { "en": "Apa Itu Waktu Konstan - O(1)?", "id": "Waktu Eksekusi Tidak Bergantung Pada Ukuran Input." },
  { "en": "Contoh Operasi O(1)?", "id": "Akses Elemen Array, Operasi Aritmatika." },
  { "en": "Apa Itu Waktu Logaritmik - O(log n)?", "id": "Waktu Eksekusi Tumbuh Sangat Lambat." },
  { "en": "Contoh Algoritma O(log n)?", "id": "Pencarian Biner (Binary Search)." },
  { "en": "Apa Itu Waktu Linear - O(n)?", "id": "Waktu Eksekusi Sebanding Dengan Ukuran Input." },
  { "en": "Contoh Algoritma O(n)?", "id": "Pencarian Linear, Menemukan Maksimum." },
  { "en": "Apa Itu Waktu Log-Linear - O(n log n)?", "id": "Sedikit Lebih Lambat Dari Linear." },
  { "en": "Contoh Algoritma O(n log n)?", "id": "Merge Sort, Quick Sort (Rata-rata)." },
  { "en": "Apa Itu Waktu Kuadratik - O(n²)?", "id": "Waktu Eksekusi Tumbuh Sangat Cepat." },
  { "en": "Contoh Algoritma O(n²)?", "id": "Bubble Sort, Selection Sort, Nested Loop." },
  { "en": "Apa Itu Waktu Polinomial?", "id": "Kompleksitas O(n^k) Untuk Konstanta k." },
  { "en": "Apa Itu Waktu Eksponensial - O(2^n)?", "id": "Sangat Lambat, Hanya Praktis Untuk n Kecil." },
  { "en": "Contoh Algoritma Eksponensial?", "id": "Solusi Brute-force Untuk Traveling Salesman." },
  { "en": "Apa Itu Waktu Faktorial - O(n!)?", "id": "Bahkan Lebih Lambat Dari Eksponensial." },
  { "en": "Urutan Pertumbuhan Dari Tercepat Ke Terlambat?", "id": "O(1), O(log n), O(n), O(n²)." },
  { "en": "Bagaimana Menganalisis Kompleksitas Loop?", "id": "Jumlah Iterasi Dikali Kompleksitas Badan Loop." },
  { "en": "Kompleksitas Nested Loop?", "id": "Perkalian Dari Jumlah Iterasi Masing-masing." },
  { "en": "Bagaimana Menganalisis Kompleksitas Percabangan?", "id": "Ambil Kompleksitas Maksimum Dari Cabang." },
  { "en": "Bagaimana Menganalisis Kompleksitas Rekursi?", "id": "Menggunakan Relasi Rekurensi." },
  { "en": "Apa Itu Relasi Rekurensi?", "id": "Persamaan Yang Mendefinisikan Fungsi Rekursif." },
  { "en": "Apa Itu Teorema Master?", "id": "Metode Untuk Menyelesaikan Relasi Rekurensi." },
  { "en": "Apa Itu Analisis Amortisasi?", "id": "Menghitung Rata-rata Waktu Per Operasi." },
  { "en": "Kapan Analisis Amortisasi Digunakan?", "id": "Saat Operasi Mahal Jarang Terjadi." },
  { "en": "Apa Itu Kompleksitas Ruang?", "id": "Jumlah Memori Yang Digunakan." },
  { "en": "Apa Itu Ruang Tambahan (Auxiliary Space)?", "id": "Memori Sementara Yang Digunakan Algoritma." },
  { "en": "Apa Itu Algoritma In-place?", "id": "Kompleksitas Ruang Tambahan Adalah O(1)." },
  { "en": "Kompleksitas Ruang Bubble Sort?", "id": "O(1)." },
  { "en": "Kompleksitas Ruang Merge Sort?", "id": "O(n)." },
  { "en": "Apa Itu Trade-off Waktu-Ruang?", "id": "Menggunakan Lebih Banyak Memori Untuk Waktu Lebih Cepat." },
  { "en": "Contoh Trade-off Waktu-Ruang?", "id": "Hashing, Memoization." },
  { "en": "Apakah Konstanta Penting Di Notasi Big O?", "id": "Tidak, Konstanta Dan Suku Orde Rendah Diabaikan." },
  { "en": "Mengapa Konstanta Diabaikan?", "id": "Karena Fokus Pada Laju Pertumbuhan Asimtotik." },
  { "en": "Apa Itu Algoritma Efisien?", "id": "Biasanya Berarti Berjalan Dalam Waktu Polinomial." },
  { "en": "Apa Itu Masalah Intractable?", "id": "Masalah Yang Tidak Memiliki Solusi Polinomial." },
  { "en": "Contoh Masalah Intractable?", "id": "Masalah NP-Complete." },
  { "en": "Apa Itu Kelas P?", "id": "Masalah Yang Dapat Diselesaikan Waktu Polinomial." },
  { "en": "Apa Itu Kelas NP?", "id": "Solusi Dapat Diverifikasi Waktu Polinomial." },
  { "en": "Apakah P = NP?", "id": "Pertanyaan Terbuka Terbesar Dalam Ilmu Komputer." },
  { "en": "Apa Itu Pseudopolynomial Time?", "id": "Waktu Polinomial Dalam Nilai Input Numerik." },
  { "en": "Kompleksitas Kasus Terbaik?", "id": "Ditandai Dengan Notasi Big Omega (Ω)." },
  { "en": "Kompleksitas Kasus Rata-rata?", "id": "Ditandai Dengan Notasi Big Theta (Θ)." },
  { "en": "Apa Itu Operasi Dominan?", "id": "Operasi Yang Paling Sering Dijalankan." },
  { "en": "Pentingnya Analisis Empiris?", "id": "Mengukur Waktu Eksekusi Aktual." },
  { "en": "Keterbatasan Analisis Empiris?", "id": "Bergantung Pada Mesin, Bahasa, Dan Compiler." },
  { "en": "Keterbatasan Analisis Teoretis?", "id": "Mengabaikan Konstanta Dan Faktor Dunia Nyata." },
  { "en": "Apa Itu Profiling?", "id": "Menganalisis Bagian Mana Dari Kode Lambat." },
  { "en": "Apa Itu Bottleneck?", "id": "Bagian Paling Lambat Dari Sistem." },
  { "en": "Apa Itu Little O Notation (o)?", "id": "Batas Atas Yang Tidak Ketat." },
  { "en": "Apa Itu Little Omega Notation (ω)?", "id": "Batas Bawah Yang Tidak Ketat." },
  { "en": "Kompleksitas Waktu Rata-rata Quick Sort?", "id": "O(n log n)." },
  { "en": "Kompleksitas Waktu Terburuk Quick Sort?", "id": "O(n²)." },
  { "en": "Kompleksitas Pencarian Biner?", "id": "O(log n)." },
  { "en": "Kompleksitas Pencarian Linear?", "id": "O(n)." },
  { "en": "Kompleksitas DFS/BFS Pada Graf?", "id": "O(V + E)." },
  { "en": "V dan E Merepresentasikan Apa?", "id": "Jumlah Simpul (Vertices) Dan Sisi (Edges)." },
  { "en": "Kompleksitas Algoritma Dijkstra?", "id": "Tergantung Implementasi (Contoh: O(E log V))." },
  { "en": "Kompleksitas Penyisipan Di BST Seimbang?", "id": "O(log n)." },
  { "en": "Kompleksitas Akses Di Hash Table?", "id": "Rata-rata O(1)." },
  { "en": "Kompleksitas Terburuk Hash Table?", "id": "O(n)." },
  { "en": "Apa Itu Skalabilitas (Scalability)?", "id": "Bagaimana Kinerja Berubah Saat Beban Meningkat." },
  { "en": "Algoritma Skalabel?", "id": "Tetap Berkinerja Baik Dengan Input Sangat Besar." },
  { "en": "Apa Itu Algoritma Online?", "id": "Memproses Input Satu Per Satu." },
  { "en": "Apa Itu Algoritma Offline?", "id": "Membutuhkan Semua Input Sejak Awal." },
  { "en": "Apa Itu Algoritma Streaming?", "id": "Memproses Aliran Data Yang Kontinu." },
  { "en": "Tantangan Algoritma Streaming?", "id": "Memori Yang Sangat Terbatas." },
  { "en": "Apa Itu Algoritma Geometris Komputasi?", "id": "Berurusan Dengan Objek Geometris." },
  { "en": "Apa Itu Algoritma String?", "id": "Berurusan Dengan Pemrosesan Teks." },
  { "en": "Apa Itu Kompleksitas Komunikasi?", "id": "Jumlah Data Yang Perlu Ditukar." },
  { "en": "Apa Itu Efek Memori Cache?", "id": "Dapat Mempengaruhi Kinerja Algoritma Secara Signifikan." },
  { "en": "Apa Itu Data Locality?", "id": "Mengakses Data Yang Berdekatan Di Memori." },
  { "en": "Mengapa Data Locality Penting?", "id": "Karena Cara Kerja Cache Memory." },
  { "en": "Apa Itu Hitungan Operasi?", "id": "Cara Sederhana Memperkirakan Kompleksitas." },
  { "en": "Apa Itu Pohon Rekursi?", "id": "Alat Visual Menganalisis Algoritma Rekursif." },
  { "en": "Apa Itu Problem Reduction?", "id": "Mengubah Masalah Sulit Menjadi Yang Lebih Mudah." },
  { "en": "Pentingnya Memahami Batas Bawah?", "id": "Mengetahui Apakah Algoritma Kita Sudah Optimal." },
  { "en": "Bisakah Kita Mengurutkan Lebih Cepat Dari O(n log n)?", "id": "Tidak, Dengan Pengurutan Berbasis Perbandingan." },
  { "en": "Apa Itu Algoritma Paralel?", "id": "Dirancang Untuk Dijalankan Di Banyak Prosesor." },
  { "en": "Apa Itu Speedup?", "id": "Rasio Waktu Eksekusi Sekuensial Dan Paralel." },
  { "en": "Apa Itu Efisiensi Paralel?", "id": "Ukuran Seberapa Baik Prosesor Dimanfaatkan." },
  { "en": "Apa Itu Algoritma Terdistribusi?", "id": "Berjalan Di Komputer Terpisah Yang Berkomunikasi." },
  { "en": "Kompleksitas Ruang Rekursi?", "id": "Tergantung Pada Kedalaman Tumpukan Rekursi." },
  { "en": "Apa Itu OOP?", "id": "Object-Oriented Programming." },
  { "en": "Apa Itu Paradigma Pemrograman?", "id": "Gaya Atau Cara Pemrograman." },
  { "en": "OOP Berbasis Apa?", "id": "Konsep Objek Dan Kelas." },
  { "en": "Apa Itu Objek?", "id": "Entitas Yang Memiliki Keadaan Dan Perilaku." },
  { "en": "Apa Itu Keadaan (State)?", "id": "Data Atau Atribut Dari Objek." },
  { "en": "Apa Itu Perilaku (Behavior)?", "id": "Aksi Atau Metode Dari Objek." },
  { "en": "Apa Itu Kelas?", "id": "Cetakan Biru (Blueprint) Untuk Membuat Objek." },
  { "en": "Apa Itu Instansiasi?", "id": "Proses Membuat Objek Dari Kelas." },
  { "en": "Apa Itu Instans (Instance)?", "id": "Nama Lain Untuk Sebuah Objek." },
  { "en": "Empat Pilar Utama OOP?", "id": "Enkapsulasi, Abstraksi, Pewarisan, Polimorfisme." },
  { "en": "Apa Itu Enkapsulasi?", "id": "Membungkus Data Dan Metode Dalam Satu Unit." },
  { "en": "Tujuan Enkapsulasi?", "id": "Menyembunyikan Detail Implementasi (Information Hiding)." },
  { "en": "Apa Itu Information Hiding?", "id": "Menyembunyikan Data Internal Dari Akses Luar." },
  { "en": "Bagaimana Mencapai Enkapsulasi?", "id": "Menggunakan Access Modifier (Public, Private)." },
  { "en": "Apa Itu Public?", "id": "Dapat Diakses Dari Mana Saja." },
  { "en": "Apa Itu Private?", "id": "Hanya Dapat Diakses Dari Dalam Kelas." },
  { "en": "Apa Itu Protected?", "id": "Dapat Diakses Oleh Kelas Turunan." },
  { "en": "Apa Itu Abstraksi?", "id": "Menampilkan Fitur Esensial, Menyembunyikan Kompleksitas." },
  { "en": "Contoh Abstraksi Dunia Nyata?", "id": "Mengemudi Mobil Tanpa Tahu Detail Mesin." },
  { "en": "Apa Itu Pewarisan (Inheritance)?", "id": "Kelas Baru Mewarisi Sifat Kelas Lain." },
  { "en": "Tujuan Pewarisan?", "id": "Untuk Reusabilitas Kode Dan Hierarki." },
  { "en": "Apa Itu Superclass?", "id": "Kelas Induk (Parent Class)." },
  { "en": "Nama Lain Superclass?", "id": "Base Class." },
  { "en": "Apa Itu Subclass?", "id": "Kelas Anak (Child Class)." },
  { "en": "Nama Lain Subclass?", "id": "Derived Class." },
  { "en": "Hubungan 'Is-A'?", "id": "Menunjukkan Hubungan Pewarisan (Mobil Adalah Kendaraan)." },
  { "en": "Apa Itu Method Overriding?", "id": "Subclass Menyediakan Implementasi Ulang Metode Superclass." },
  { "en": "Apa Itu Polimorfisme?", "id": "Kemampuan Objek Mengambil Banyak Bentuk." },
  { "en": "Arti Polimorfisme?", "id": "Satu Antarmuka, Banyak Implementasi." },
  { "en": "Contoh Polimorfisme Dunia Nyata?", "id": "Tombol 'Play' Untuk Lagu, Video, Podcast." },
  { "en": "Bagaimana Polimorfisme Dicapai?", "id": "Melalui Pewarisan Dan Method Overriding." },
  { "en": "Apa Itu Atribut?", "id": "Variabel Yang Menjadi Bagian Dari Objek." },
  { "en": "Nama Lain Atribut?", "id": "Properti, Field, Member Variable." },
  { "en": "Apa Itu Metode?", "id": "Fungsi Yang Menjadi Bagian Dari Kelas." },
  { "en": "Nama Lain Metode?", "id": "Member Function." },
  { "en": "Apa Itu Konstruktor?", "id": "Metode Khusus Untuk Menginisialisasi Objek." },
  { "en": "Kapan Konstruktor Dipanggil?", "id": "Saat Objek Baru Dibuat." },
  { "en": "Apa Itu Destruktor?", "id": "Metode Khusus Untuk Membersihkan Objek." },
  { "en": "Kapan Destruktor Dipanggil?", "id": "Saat Objek Dihancurkan." },
  { "en": "Apa Itu 'this' Keyword?", "id": "Merujuk Pada Instans Objek Saat Ini." },
  { "en": "Apa Itu 'super' Keyword?", "id": "Merujuk Pada Superclass Dari Objek." },
  { "en": "Apa Itu Komposisi?", "id": "Membangun Objek Dengan Menggabungkan Objek Lain." },
  { "en": "Hubungan 'Has-A'?", "id": "Menunjukkan Hubungan Komposisi (Mobil Punya Mesin)." },
  { "en": "Komposisi vs Pewarisan?", "id": "Favor Composition over Inheritance." },
  { "en": "Apa Itu Agregasi?", "id": "Bentuk Komposisi Yang Lebih Lemah." },
  { "en": "Apa Itu Antarmuka (Interface)?", "id": "Kumpulan Definisi Metode Abstrak." },
  { "en": "Fungsi Antarmuka?", "id": "Mendefinisikan 'Kontrak' Yang Harus Dipenuhi." },
  { "en": "Dapatkah Antarmuka Diinstansiasi?", "id": "Tidak, Antarmuka Harus Diimplementasikan." },
  { "en": "Apa Itu Kelas Abstrak?", "id": "Kelas Yang Tidak Dapat Diinstansiasi." },
  { "en": "Tujuan Kelas Abstrak?", "id": "Menjadi Kelas Dasar Untuk Kelas Lain." },
  { "en": "Apa Itu Metode Abstrak?", "id": "Metode Tanpa Implementasi." },
  { "en": "Perbedaan Kelas Abstrak Dan Antarmuka?", "id": "Kelas Abstrak Dapat Memiliki Metode Konkret." },
  { "en": "Apa Itu Multiple Inheritance?", "id": "Kelas Mewarisi Dari Lebih Satu Superclass." },
  { "en": "Masalah Multiple Inheritance?", "id": "Diamond Problem." },
  { "en": "Bahasa Yang Mendukung Multiple Inheritance?", "id": "C++, Python." },
  { "en": "Bagaimana Java Mengatasi Ini?", "id": "Dengan Mengizinkan Implementasi Banyak Antarmuka." },
  { "en": "Apa Itu Static Member?", "id": "Atribut Atau Metode Milik Kelas." },
  { "en": "Apa Itu Instance Member?", "id": "Atribut Atau Metode Milik Objek." },
  { "en": "Bagaimana Mengakses Static Member?", "id": "Langsung Melalui Nama Kelas." },
  { "en": "Apa Itu Getter?", "id": "Metode Untuk Mengambil Nilai Atribut Private." },
  { "en": "Apa Itu Setter?", "id": "Metode Untuk Mengubah Nilai Atribut Private." },
  { "en": "Apa Itu SOLID Principles?", "id": "Lima Prinsip Desain Untuk OOP." },
  { "en": "S Dalam SOLID?", "id": "Single Responsibility Principle." },
  { "en": "O Dalam SOLID?", "id": "Open/Closed Principle." },
  { "en": "L Dalam SOLID?", "id": "Liskov Substitution Principle." },
  { "en": "I Dalam SOLID?", "id": "Interface Segregation Principle." },
  { "en": "D Dalam SOLID?", "id": "Dependency Inversion Principle." },
  { "en": "Apa Itu Design Pattern?", "id": "Solusi Umum Untuk Masalah Desain." },
  { "en": "Kategori Design Pattern?", "id": "Creational, Structural, Behavioral." },
  { "en": "Contoh Creational Pattern?", "id": "Singleton, Factory." },
  { "en": "Pola Singleton?", "id": "Memastikan Kelas Hanya Memiliki Satu Instans." },
  { "en": "Contoh Structural Pattern?", "id": "Adapter, Decorator, Facade." },
  { "en": "Pola Adapter?", "id": "Membuat Antarmuka Yang Tidak Kompatibel Bekerja." },
  { "en": "Contoh Behavioral Pattern?", "id": "Observer, Strategy, Command." },
  { "en": "Pola Observer?", "id": "Objek Memberitahu Dependennya Tentang Perubahan." },
  { "en": "Apa Itu Object Lifetime?", "id": "Waktu Dari Penciptaan Hingga Penghancuran Objek." },
  { "en": "Apa Itu Garbage Collection?", "id": "Proses Otomatis Menghapus Objek Tak Terpakai." },
  { "en": "Apa Itu Reference Counting?", "id": "Salah Satu Teknik Garbage Collection." },
  { "en": "Apa Itu Memory Leak?", "id": "Objek Tak Terpakai Tapi Tidak Dihapus." },
  { "en": "Apa Itu Class Diagram?", "id": "Diagram UML Untuk Memodelkan Kelas." },
  { "en": "Apa Itu UML?", "id": "Unified Modeling Language." },
  { "en": "Apa Itu Class Library?", "id": "Kumpulan Kelas Yang Dapat Digunakan Kembali." },
  { "en": "Apa Itu Framework?", "id": "Class Library Yang Menyediakan Struktur Aplikasi." },
  { "en": "Apa Itu Casting Objek?", "id": "Memperlakukan Objek Sebagai Tipe Kelas Lain." },
  { "en": "Apa Itu Upcasting?", "id": "Casting Dari Subclass Ke Superclass." },
  { "en": "Apa Itu Downcasting?", "id": "Casting Dari Superclass Ke Subclass." },
  { "en": "Apa Itu Late Binding?", "id": "Metode Yang Dipanggil Ditentukan Saat Runtime." },
  { "en": "Nama Lain Late Binding?", "id": "Dynamic Binding." },
  { "en": "Apa Itu Early Binding?", "id": "Metode Yang Dipanggil Ditentukan Saat Kompilasi." },
  { "en": "Nama Lain Early Binding?", "id": "Static Binding." },
  { "en": "Polimorfisme Menggunakan Binding Apa?", "id": "Late Binding." },
  { "en": "Apa Itu Final Class?", "id": "Kelas Yang Tidak Dapat Diwariskan." },
  { "en": "Apa Itu Final Method?", "id": "Metode Yang Tidak Dapat Di-override." },
  { "en": "Apa Itu Nested Class?", "id": "Kelas Yang Didefinisikan Di Dalam Kelas Lain." },
  { "en": "Apa Itu Rekursi?", "id": "Fungsi Yang Memanggil Dirinya Sendiri." },
  { "en": "Kapan Rekursi Digunakan?", "id": "Untuk Masalah Yang Dapat Dipecah." },
  { "en": "Dua Bagian Kunci Fungsi Rekursif?", "id": "Kasus Dasar Dan Kasus Rekursif." },
  { "en": "Apa Itu Kasus Dasar (Base Case)?", "id": "Kondisi Yang Menghentikan Rekursi." },
  { "en": "Apa Yang Terjadi Tanpa Kasus Dasar?", "id": "Rekursi Tak Terbatas (Infinite Recursion)." },
  { "en": "Apa Itu Kasus Rekursif?", "id": "Langkah Yang Memecah Masalah Lebih Kecil." },
  { "en": "Bagaimana Kasus Rekursif Bekerja?", "id": "Memanggil Fungsi Yang Sama Dengan Input Berbeda." },
  { "en": "Contoh Klasik Masalah Rekursif?", "id": "Faktorial, Fibonacci, Menara Hanoi." },
  { "en": "Bagaimana Menghitung Faktorial Secara Rekursif?", "id": "n! = n * (n-1)!." },
  { "en": "Kasus Dasar Untuk Faktorial?", "id": "Faktorial dari 0 adalah 1." },
  { "en": "Bagaimana Menghitung Fibonacci Secara Rekursif?", "id": "fib(n) = fib(n-1) + fib(n-2)." },
  { "en": "Kasus Dasar Untuk Fibonacci?", "id": "fib(0) = 0, fib(1) = 1." },
  { "en": "Kelemahan Rekursi Fibonacci Naif?", "id": "Sangat Tidak Efisien (Perhitungan Berulang)." },
  { "en": "Apa Itu Call Stack?", "id": "Struktur Data Yang Mengelola Panggilan Fungsi." },
  { "en": "Bagaimana Rekursi Menggunakan Call Stack?", "id": "Setiap Panggilan Menambahkan Frame Ke Stack." },
  { "en": "Apa Itu Stack Overflow Error?", "id": "Call Stack Penuh Akibat Rekursi Terlalu Dalam." },
  { "en": "Rekursi vs Iterasi (Loop)?", "id": "Dua Cara Untuk Menyelesaikan Tugas Berulang." },
  { "en": "Kapan Rekursi Lebih Elegan?", "id": "Untuk Struktur Data Seperti Pohon." },
  { "en": "Kapan Iterasi Lebih Efisien?", "id": "Biasanya Menggunakan Memori Lebih Sedikit." },
  { "en": "Bisakah Setiap Rekursi Diubah Menjadi Iterasi?", "id": "Ya, Menggunakan Stack Eksplisit." },
  { "en": "Bisakah Setiap Iterasi Diubah Menjadi Rekursi?", "id": "Ya, Meskipun Terkadang Tidak Alami." },
  { "en": "Apa Itu Rekursi Langsung?", "id": "Fungsi A Memanggil Langsung Fungsi A." },
  { "en": "Apa Itu Rekursi Tidak Langsung?", "id": "Fungsi A Memanggil B, B Memanggil A." },
  { "en": "Apa Itu Rekursi Ekor (Tail Recursion)?", "id": "Panggilan Rekursif Adalah Operasi Terakhir." },
  { "en": "Keuntungan Rekursi Ekor?", "id": "Dapat Dioptimalkan Menjadi Iterasi." },
  { "en": "Apa Itu Tail Call Optimization?", "id": "Optimisasi Compiler Untuk Rekursi Ekor." },
  { "en": "Apa Itu Pohon Rekursi?", "id": "Diagram Yang Memvisualisasikan Panggilan Rekursif." },
  { "en": "Pohon Rekursi Fibonacci?", "id": "Menunjukkan Banyaknya Panggilan Yang Tumpang Tindih." },
  { "en": "Bagaimana Memperbaiki Rekursi Fibonacci?", "id": "Menggunakan Memoization (Pemrograman Dinamis)." },
  { "en": "Apa Itu Memoization?", "id": "Menyimpan Hasil Komputasi Untuk Digunakan Kembali." },
  { "en": "Rekursi Untuk Traversal Pohon?", "id": "Sangat Alami Dan Sering Digunakan." },
  { "en": "Pre-order Traversal Secara Rekursif?", "id": "Proses Node, Rekursi Kiri, Rekursi Kanan." },
  { "en": "In-order Traversal Secara Rekursif?", "id": "Rekursi Kiri, Proses Node, Rekursi Kanan." },
  { "en": "Post-order Traversal Secara Rekursif?", "id": "Rekursi Kiri, Rekursi Kanan, Proses Node." },
  { "en": "Rekursi Pada Pencarian Biner?", "id": "Memanggil Pencarian Pada Setengah Bagian Yang Relevan." },
  { "en": "Rekursi Pada Merge Sort?", "id": "Mengurutkan Setengah Kiri, Mengurutkan Setengah Kanan." },
  { "en": "Rekursi Pada Quick Sort?", "id": "Mempartisi, Lalu Mengurutkan Dua Bagian." },
  { "en": "Apa Itu Divide and Conquer?", "id": "Paradigma Algoritma Yang Sering Menggunakan Rekursi." },
  { "en": "Apa Itu Backtracking?", "id": "Teknik Algoritma Yang Sering Diimplementasikan Rekursif." },
  { "en": "Struktur Fungsi Backtracking?", "id": "Coba Pilihan, Rekursi, Batalkan Pilihan." },
  { "en": "Apa Itu Kedalaman Rekursi?", "id": "Jumlah Panggilan Rekursif Maksimum Yang Aktif." },
  { "en": "Apa Itu Rekursi Ganda?", "id": "Fungsi Memanggil Dirinya Sendiri Dua Kali." },
  { "en": "Contoh Rekursi Ganda?", "id": "Fibonacci, Menara Hanoi." },
  { "en": "Apa Itu Rekursi Linear?", "id": "Fungsi Memanggil Dirinya Sendiri Satu Kali." },
  { "en": "Contoh Rekursi Linear?", "id": "Faktorial." },
  { "en": "Apa Itu Rekursi Berpohon (Tree Recursion)?", "id": "Menjelajahi Kemungkinan Seperti Cabang Pohon." },
  { "en": "Apa Itu Mutual Recursion?", "id": "Sama Dengan Rekursi Tidak Langsung." },
  { "en": "Berpikir Secara Rekursif?", "id": "Asumsikan Masalah Lebih Kecil Sudah Terpecahkan." },
  { "en": "Apa Itu Langkah Induktif?", "id": "Mirip Dengan Kasus Rekursif." },
  { "en": "Apa Itu Hipotesis Induktif?", "id": "Asumsi Bahwa Panggilan Rekursif Bekerja Benar." },
  { "en": "Pentingnya Melewatkan Parameter?", "id": "Untuk Melacak Kemajuan Dan Keadaan." },
  { "en": "Apa Itu Fungsi Helper Rekursif?", "id": "Fungsi Internal Yang Melakukan Rekursi Sebenarnya." },
  { "en": "Mengapa Menggunakan Fungsi Helper?", "id": "Untuk Menyembunyikan Detail Implementasi Rekursif." },
  { "en": "Rekursi vs Loop: Keterbacaan?", "id": "Rekursi Seringkali Lebih Mudah Dibaca." },
  { "en": "Rekursi vs Loop: Kinerja?", "id": "Loop Seringkali Lebih Cepat." },
  { "en": "Apa Itu Overhead Panggilan Fungsi?", "id": "Biaya Waktu Dan Memori Untuk Memanggil Fungsi." },
  { "en": "Rekursi Memiliki Overhead?", "id": "Ya, Lebih Besar Daripada Iterasi." },
  { "en": "Apa Itu Fraktal?", "id": "Objek Geometris Yang Didefinisikan Secara Rekursif." },
  { "en": "Contoh Fraktal?", "id": "Koch Snowflake, Mandelbrot Set." },
  { "en": "Struktur Data Rekursif?", "id": "Struktur Data Yang Didefinisikan Dalam Termanya Sendiri." },
  { "en": "Contoh Struktur Data Rekursif?", "id": "Pohon Dan Linked List." },
  { "en": "Bagaimana Linked List Rekursif?", "id": "Daftar Adalah Node Diikuti Oleh Daftar." },
  { "en": "Bagaimana Pohon Rekursif?", "id": "Pohon Adalah Node Dengan Beberapa Pohon." },
  { "en": "Apa Itu Relasi Rekurensi?", "id": "Mendeskripsikan Kompleksitas Waktu Algoritma Rekursif." },
  { "en": "Bagaimana Mengubah Rekursi Menjadi Iterasi?", "id": "Menggunakan Stack Secara Eksplisit." },
  { "en": "Menelusuri Kode Rekursif?", "id": "Bisa Sulit, Gunakan Pohon Rekursi." },
  { "en": "Apa Itu Akumulator?", "id": "Parameter Untuk Mengumpulkan Hasil Dalam Rekursi." },
  { "en": "Rekursi Dengan Akumulator Seringkali?", "id": "Merupakan Rekursi Ekor (Tail Recursive)." },
  { "en": "Bahaya Rekursi Tanpa Batas?", "id": "Menyebabkan Stack Overflow." },
  { "en": "Kapan Harus Menghindari Rekursi?", "id": "Saat Kinerja Kritis Dan Kedalaman Besar." },
  { "en": "Apa Itu Rekursi Berlebihan?", "id": "Menghitung Ulang Sub-masalah Yang Sama." },
  { "en": "Contoh Rekursi Berlebihan?", "id": "Implementasi Fibonacci Naif." },
  { "en": "Rekursi Pada DFS (Depth-First Search)?", "id": "Implementasi Alami Menggunakan Rekursi." },
  { "en": "Rekursi Pada BFS (Breadth-First Search)?", "id": "Biasanya Diimplementasikan Secara Iteratif." },
  { "en": "Rekursi Pada Permutasi?", "id": "Membangun Semua Kemungkinan Urutan Secara Rekursif." },
  { "en": "Rekursi Pada Kombinasi?", "id": "Memilih Elemen Dan Rekursi Pada Sisanya." },
  { "en": "Apa Itu Pernyataan Guard?", "id": "Pemeriksaan Awal Dalam Fungsi (Bisa Kasus Dasar)." },
  { "en": "Pentingnya Mengidentifikasi Kasus Dasar?", "id": "Kunci Utama Agar Rekursi Benar." },
  { "en": "Apa Itu Infinite Descent?", "id": "Argumen Logis Terkait Rekursi." },
  { "en": "Apa Itu Corecursion?", "id": "Proses Yang Menghasilkan Data Secara Potensial Tak Terbatas." },
  { "en": "Apa Itu Lazy Evaluation?", "id": "Menunda Komputasi Hingga Hasilnya Dibutuhkan." },
  { "en": "Hubungan Lazy Evaluation Dan Corecursion?", "id": "Memungkinkan Bekerja Dengan Struktur Tak Terbatas." },
  { "en": "Pemahaman Rekursi Adalah Kunci Untuk?", "id": "Konsep Lanjutan Seperti Pemrograman Dinamis." },
  { "en": "Mendebug Rekursi?", "id": "Periksa Kasus Dasar Dan Langkah Rekursif." },
  { "en": "Apa Itu Stack Frame?", "id": "Berisi Parameter Lokal Dan Alamat Kembali." },
  { "en": "Bagaimana Stack Bekerja?", "id": "LIFO (Last-In, First-Out)." },
  { "en": "Apa Itu Heap?", "id": "Area Memori Untuk Alokasi Dinamis." },
  { "en": "Apakah Rekursi Menggunakan Heap?", "id": "Tidak Secara Langsung, Tapi Objeknya Bisa." },
  { "en": "Rekursi Untuk Menara Hanoi?", "id": "Solusi Paling Elegan Dan Sederhana." },
  { "en": "Kompleksitas Menara Hanoi?", "id": "Eksponensial, O(2^n)." },
  { "en": "Rekursi Dan Fraktal?", "id": "Fraktal Adalah Visualisasi Dari Rekursi." },
  { "en": "Fungsi Ackermann?", "id": "Contoh Fungsi Rekursif Yang Tumbuh Cepat." },
  { "en": "Apa Itu Primitive Recursive Function?", "id": "Kelas Fungsi Yang Dapat Dihitung." },
  { "en": "Apakah Fungsi Ackermann Primitif Rekursif?", "id": "Tidak." },
  { "en": "Rekursi Dalam Bahasa Fungsional?", "id": "Merupakan Struktur Kontrol Utama." },
  { "en": "Bahasa LISP?", "id": "Sangat Bergantung Pada Rekursi Dan Daftar." },
  { "en": "Apa Itu Stack (Tumpukan)?", "id": "Struktur Data Linear Abstrak." },
  { "en": "Prinsip Operasi Stack?", "id": "LIFO (Last-In, First-Out)." },
  { "en": "Analogi Dunia Nyata Stack?", "id": "Tumpukan Piring." },
  { "en": "Operasi Utama Pada Stack?", "id": "Push, Pop, Peek." },
  { "en": "Apa Itu Operasi Push?", "id": "Menambahkan Elemen Ke Atas Tumpukan." },
  { "en": "Apa Itu Operasi Pop?", "id": "Menghapus Elemen Dari Atas Tumpukan." },
  { "en": "Apa Itu Operasi Peek (Atau Top)?", "id": "Melihat Elemen Teratas Tanpa Menghapus." },
  { "en": "Operasi Lain Pada Stack?", "id": "IsEmpty, IsFull, Size." },
  { "en": "Apa Itu Stack Overflow?", "id": "Mencoba Push Ke Stack Yang Sudah Penuh." },
  { "en": "Apa Itu Stack Underflow?", "id": "Mencoba Pop Dari Stack Yang Kosong." },
  { "en": "Bagaimana Mengimplementasikan Stack?", "id": "Menggunakan Array Atau Linked List." },
  { "en": "Implementasi Stack Dengan Array?", "id": "Menggunakan Pointer Ke Elemen Teratas." },
  { "en": "Implementasi Stack Dengan Linked List?", "id": "Menambah Dan Menghapus Dari Head." },
  { "en": "Aplikasi Stack: Undo/Redo?", "id": "Menyimpan Aksi Pengguna Dalam Tumpukan." },
  { "en": "Aplikasi Stack: Panggilan Fungsi?", "id": "Call Stack Mengelola Fungsi Aktif." },
  { "en": "Aplikasi Stack: Evaluasi Ekspresi?", "id": "Mengubah Infix Menjadi Postfix." },
  { "en": "Apa Itu Notasi Infix?", "id": "Operator Di Antara Operand (A + B)." },
  { "en": "Apa Itu Notasi Postfix (Reverse Polish)?", "id": "Operator Setelah Operand (A B +)." },
  { "en": "Aplikasi Stack: Navigasi Browser?", "id": "Tombol 'Back' Menggunakan Stack." },
  { "en": "Aplikasi Stack: Pengecekan Kurung?", "id": "Memastikan Setiap Kurung Buka Memiliki Pasangan." },
  { "en": "Kompleksitas Waktu Operasi Stack?", "id": "O(1) Atau Waktu Konstan." },
  { "en": "Apa Itu Queue (Antrean)?", "id": "Struktur Data Linear Abstrak Lainnya." },
  { "en": "Prinsip Operasi Queue?", "id": "FIFO (First-In, First-Out)." },
  { "en": "Analogi Dunia Nyata Queue?", "id": "Antrean Orang Di Kasir." },
  { "en": "Operasi Utama Pada Queue?", "id": "Enqueue Dan Dequeue." },
  { "en": "Apa Itu Operasi Enqueue?", "id": "Menambahkan Elemen Ke Belakang Antrean." },
  { "en": "Apa Itu Operasi Dequeue?", "id": "Menghapus Elemen Dari Depan Antrean." },
  { "en": "Operasi Lain Pada Queue?", "id": "Peek (Front), IsEmpty, IsFull." },
  { "en": "Bagaimana Mengimplementasikan Queue?", "id": "Menggunakan Array Atau Linked List." },
  { "en": "Implementasi Queue Dengan Linked List?", "id": "Menggunakan Pointer Head Dan Tail." },
  { "en": "Masalah Implementasi Queue Dengan Array?", "id": "Pergeseran Elemen Yang Tidak Efisien." },
  { "en": "Solusi Untuk Masalah Tersebut?", "id": "Menggunakan Circular Queue." },
  { "en": "Apa Itu Circular Queue?", "id": "Antrean Yang Menggunakan Array Secara Melingkar." },
  { "en": "Aplikasi Queue: Print Spooling?", "id": "Mengelola Pekerjaan Pencetakan Dalam Urutan." },
  { "en": "Aplikasi Queue: Penjadwalan CPU?", "id": "Menjadwalkan Proses Yang Menunggu CPU." },
  { "en": "Aplikasi Queue: BFS (Breadth-First Search)?", "id": "Algoritma Pencarian Graf Menggunakan Queue." },
  { "en": "Aplikasi Queue: Buffering Data?", "id": "Menyimpan Data Sementara Antara Dua Proses." },
  { "en": "Kompleksitas Waktu Operasi Queue?", "id": "O(1) Atau Waktu Konstan." },
  { "en": "Apa Itu Priority Queue?", "id": "Antrean Dimana Setiap Elemen Memiliki Prioritas." },
  { "en": "Bagaimana Priority Queue Bekerja?", "id": "Elemen Prioritas Tertinggi Dihapus Terlebih Dahulu." },
  { "en": "Implementasi Priority Queue?", "id": "Seringkali Menggunakan Struktur Data Heap." },
  { "en": "Apa Itu Deque (Double-Ended Queue)?", "id": "Antrean Dengan Dua Ujung." },
  { "en": "Operasi Pada Deque?", "id": "Bisa Tambah/Hapus Dari Depan Dan Belakang." },
  { "en": "Stack Adalah Kasus Khusus Deque?", "id": "Ya, Jika Hanya Menggunakan Satu Ujung." },
  { "en": "Queue Adalah Kasus Khusus Deque?", "id": "Ya, Jika Menggunakan Ujung Berlawanan." },
  { "en": "Apa Itu Bounded Stack/Queue?", "id": "Memiliki Kapasitas Maksimum Yang Terbatas." },
  { "en": "Apa Itu Unbounded Stack/Queue?", "id": "Tidak Memiliki Batas Kapasitas Teoretis." },
  { "en": "Apa Itu Two-Stack Queue?", "id": "Mengimplementasikan Antrean Menggunakan Dua Tumpukan." },
  { "en": "Apa Itu Two-Queue Stack?", "id": "Mengimplementasikan Tumpukan Menggunakan Dua Antrean." },
  { "en": "Apa Itu Stack Frame?", "id": "Digunakan Dalam Call Stack." },
  { "en": "Apa Yang Disimpan Di Stack Frame?", "id": "Parameter, Variabel Lokal, Alamat Kembali." },
  { "en": "Stack vs Heap (Memori)?", "id": "Dua Area Memori Yang Berbeda." },
  { "en": "Memori Stack Untuk Apa?", "id": "Alokasi Statis (Variabel Lokal)." },
  { "en": "Memori Heap Untuk Apa?", "id": "Alokasi Dinamis (Objek)." },
  { "en": "Apa Itu Stack Smashing Attack?", "id": "Serangan Keamanan Yang Mengeksploitasi Buffer Overflow." },
  { "en": "Apa Itu Call Stack?", "id": "Tumpukan Panggilan Fungsi." },
  { "en": "Apa Itu Front (Depan)?", "id": "Pointer Ke Elemen Pertama Antrean." },
  { "en": "Apa Itu Rear (Belakang)?", "id": "Pointer Ke Elemen Terakhir Antrean." },
  { "en": "Apa Itu Top (Atas)?", "id": "Pointer Ke Elemen Teratas Tumpukan." },
  { "en": "Apa Itu Buffer?", "id": "Area Memori Sementara Untuk Data." },
  { "en": "Queue Sering Digunakan Sebagai Apa?", "id": "Sebagai Buffer." },
  { "en": "Apa Itu Thread Pool?", "id": "Menggunakan Antrean Untuk Mengelola Tugas." },
  { "en": "Apa Itu Message Queue?", "id": "Antrean Untuk Komunikasi Antar Proses." },
  { "en": "Apa Itu Event Queue?", "id": "Antrean Yang Menyimpan Event Untuk Diproses." },
  { "en": "Aplikasi Stack: Mundur Dalam Sejarah?", "id": "Seperti Tombol 'Back' Di Browser." },
  { "en": "Aplikasi Queue: Sistem Tiket?", "id": "Melayani Pelanggan Sesuai Urutan Kedatangan." },
  { "en": "Apa Itu Blocking Queue?", "id": "Antrean Yang Memblokir Saat Penuh Atau Kosong." },
  { "en": "Apa Itu Concurrent Queue?", "id": "Antrean Yang Aman Digunakan Oleh Banyak Thread." },
  { "en": "Pengecekan Keseimbangan Simbol?", "id": "Aplikasi Klasik Dari Stack." },
  { "en": "Apa Itu Notasi Polish (Prefix)?", "id": "Operator Sebelum Operand (+ A B)." },
  { "en": "Bagaimana Mengevaluasi Notasi Postfix?", "id": "Menggunakan Stack." },
  { "en": "Apa Itu Menara Hanoi?", "id": "Masalah Rekursif Yang Dapat Diselesaikan Iteratif." },
  { "en": "Apa Itu Abstract Data Type (ADT)?", "id": "Stack Dan Queue Adalah Contoh ADT." },
  { "en": "Fokus ADT?", "id": "Pada 'Apa' (Operasi), Bukan 'Bagaimana' (Implementasi)." },
  { "en": "Implementasi Stack Yang Paling Umum?", "id": "Biasanya Menggunakan Array Dinamis." },
  { "en": "Implementasi Queue Yang Paling Umum?", "id": "Biasanya Menggunakan Linked List." },
  { "en": "Mengapa Linked List Baik Untuk Queue?", "id": "Operasi Enqueue Dan Dequeue Selalu O(1)." },
  { "en": "Apa Itu Breadth-First Traversal?", "id": "Mirip Dengan BFS, Menggunakan Queue." },
  { "en": "Apa Itu Depth-First Traversal?", "id": "Mirip Dengan DFS, Menggunakan Stack." },
  { "en": "Apa Itu Algoritma Shunting-Yard?", "id": "Mengubah Infix Ke Postfix Menggunakan Stack." },
  { "en": "Apa Itu Palindrom?", "id": "Dapat Diperiksa Menggunakan Stack Dan Queue." },
  { "en": "Apa Itu Job Scheduling?", "id": "Aplikasi Dari Priority Queue." },
  { "en": "Apa Itu Circular Buffer?", "id": "Nama Lain Untuk Circular Queue." },
  { "en": "Apa Itu Double Stack?", "id": "Mengimplementasikan Dua Stack Dalam Satu Array." },
  { "en": "Aplikasi Deque?", "id": "Manajemen Riwayat, Algoritma Sliding Window." },
  { "en": "Apa Itu Keadaan Penuh (Full)?", "id": "Saat Buffer Atau Struktur Data Tidak Bisa Menambah." },
  { "en": "Apa Itu Keadaan Kosong (Empty)?", "id": "Saat Buffer Atau Struktur Data Tidak Ada Elemen." },
  { "en": "Apa Itu Bounded Buffer Problem?", "id": "Masalah Sinkronisasi Klasik." },
  { "en": "Bagaimana Mengimplementasikan Priority Queue?", "id": "Menggunakan Heap." },
  { "en": "Apa Itu Heap?", "id": "Struktur Data Pohon Biner." },
  { "en": "Apa Itu Monotonic Queue?", "id": "Antrean Dimana Elemennya Selalu Terurut." },
  { "en": "Aplikasi Monotonic Queue?", "id": "Masalah Sliding Window Maximum." },
  { "en": "Apa Itu Stack Permisif?", "id": "Stack Yang Memungkinkan Beberapa Operasi Bersamaan." },
  { "en": "Struktur Data Linear?", "id": "Elemen Disusun Secara Berurutan." },
  { "en": "Struktur Data Non-Linear?", "id": "Elemen Tidak Berurutan (Pohon, Graf)." },
  { "en": "Apa Itu Linked List?", "id": "Struktur Data Linear Dari Elemen Node." },
  { "en": "Bagaimana Elemen Linked List Disimpan?", "id": "Tidak Harus Berdekatan Di Memori." },
  { "en": "Bagaimana Node Terhubung?", "id": "Melalui Pointer Atau Referensi." },
  { "en": "Apa Itu Node?", "id": "Blok Bangunan Dasar Linked List." },
  { "en": "Apa Isi Sebuah Node?", "id": "Data Dan Pointer Ke Node Berikutnya." },
  { "en": "Apa Itu Head?", "id": "Pointer Yang Menunjuk Ke Node Pertama." },
  { "en": "Apa Itu Tail?", "id": "Node Terakhir Dalam Daftar." },
  { "en": "Pointer Node Terakhir Menunjuk Ke Mana?", "id": "Null, Menandakan Akhir Daftar." },
  { "en": "Keuntungan Utama Linked List?", "id": "Ukuran Dinamis, Penyisipan/Penghapusan Efisien." },
  { "en": "Kekurangan Utama Linked List?", "id": "Akses Acak Lambat." },
  { "en": "Mengapa Akses Acak Lambat?", "id": "Harus Melintasi Daftar Dari Awal." },
  { "en": "Kompleksitas Akses Elemen Ke-i?", "id": "O(n)." },
  { "en": "Kompleksitas Penyisipan Di Awal?", "id": "O(1)." },
  { "en": "Kompleksitas Penghapusan Di Awal?", "id": "O(1)." },
  { "en": "Linked List vs Array?", "id": "Trade-off Antara Akses Dan Modifikasi." },
  { "en": "Apa Itu Singly Linked List?", "id": "Setiap Node Menunjuk Ke Node Berikutnya." },
  { "en": "Apa Itu Doubly Linked List?", "id": "Setiap Node Memiliki Dua Pointer." },
  { "en": "Pointer Pada Doubly Linked List?", "id": "Menunjuk Ke Node Berikutnya Dan Sebelumnya." },
  { "en": "Keuntungan Doubly Linked List?", "id": "Dapat Dilintasi Maju Dan Mundur." },
  { "en": "Kekurangan Doubly Linked List?", "id": "Membutuhkan Memori Tambahan Untuk Pointer." },
  { "en": "Apa Itu Circular Linked List?", "id": "Node Terakhir Menunjuk Kembali Ke Head." },
  { "en": "Keuntungan Circular Linked List?", "id": "Dapat Melintasi Tanpa Henti." },
  { "en": "Aplikasi Circular Linked List?", "id": "Penjadwalan Round-Robin, Aplikasi Multiplayer." },
  { "en": "Apa Itu Traversal?", "id": "Proses Mengunjungi Setiap Node Dalam Daftar." },
  { "en": "Bagaimana Melakukan Traversal?", "id": "Memulai Dari Head Dan Mengikuti Pointer." },
  { "en": "Bagaimana Mencari Elemen?", "id": "Melakukan Traversal Hingga Elemen Ditemukan." },
  { "en": "Bagaimana Menyisipkan Node Baru?", "id": "Mengubah Pointer Node Sekitarnya." },
  { "en": "Bagaimana Menghapus Node?", "id": "Menghubungkan Pointer Node Sebelumnya Dan Sesudahnya." },
  { "en": "Pentingnya Menangani Kasus Tepi?", "id": "Daftar Kosong, Satu Elemen, Head/Tail." },
  { "en": "Apa Itu Alokasi Memori Dinamis?", "id": "Linked List Bergantung Pada Ini." },
  { "en": "Bagaimana Linked List Mengimplementasikan Stack?", "id": "Operasi Push/Pop Di Head." },
  { "en": "Bagaimana Linked List Mengimplementasikan Queue?", "id": "Enqueue Di Tail, Dequeue Di Head." },
  { "en": "Apa Itu Masalah Siklus?", "id": "Menentukan Apakah Ada Loop Dalam Daftar." },
  { "en": "Algoritma Floyd's Cycle-Finding?", "id": "Menggunakan Dua Pointer (Cepat Dan Lambat)." },
  { "en": "Nama Lain Algoritma Floyd?", "id": "Algoritma Kura-kura Dan Kelinci." },
  { "en": "Bagaimana Menemukan Node Tengah?", "id": "Menggunakan Dua Pointer (Cepat Dan Lambat)." },
  { "en": "Bagaimana Membalik Linked List?", "id": "Mengubah Arah Pointer Secara Iteratif." },
  { "en": "Apa Itu Penggabungan Dua Daftar Terurut?", "id": "Operasi Kunci Dalam Merge Sort." },
  { "en": "Apa Itu Unrolled Linked List?", "id": "Setiap Node Menyimpan Array Elemen." },
  { "en": "Apa Itu Skip List?", "id": "Linked List Dengan Lapisan Pointer Tambahan." },
  { "en": "Tujuan Skip List?", "id": "Mempercepat Pencarian (Mirip Pencarian Biner)." },
  { "en": "Apa Itu Self-Organizing List?", "id": "Menyusun Ulang Elemen Berdasarkan Frekuensi Akses." },
  { "en": "Apa Itu XOR Linked List?", "id": "Menghemat Memori Dengan Menyimpan XOR Pointer." },
  { "en": "Apa Itu Garbage Collection?", "id": "Penting Untuk Mengelola Memori Node." },
  { "en": "Apa Itu Null Pointer Exception?", "id": "Error Saat Mengakses Pointer Null." },
  { "en": "Apa Itu Dangling Pointer?", "id": "Pointer Yang Menunjuk Ke Memori Bebas." },
  { "en": "Apa Itu Memory Leak?", "id": "Node Yang Dihapus Tapi Memorinya Tidak Dibebaskan." },
  { "en": "Apa Itu Sentinel Node?", "id": "Node Dummy Untuk Menyederhanakan Kode." },
  { "en": "Fungsi Sentinel Node?", "id": "Menghilangkan Kasus Tepi Seperti Daftar Kosong." },
  { "en": "Apa Itu Recursion Pada Linked List?", "id": "Banyak Operasi Dapat Diimplementasikan Rekursif." },
  { "en": "Apa Itu Flattening Linked List?", "id": "Mengubah Daftar Multi-Level Menjadi Satu Level." },
  { "en": "Menghapus Duplikat Dari Daftar?", "id": "Masalah Umum Linked List." },
  { "en": "Rotasi Linked List?", "id": "Menggeser Elemen Daftar Sejumlah Posisi." },
  { "en": "Menemukan Titik Pertemuan Dua Daftar?", "id": "Masalah Yang Melibatkan Traversal." },
  { "en": "Apa Itu Palindrom Linked List?", "id": "Memeriksa Apakah Daftar Adalah Palindrom." },
  { "en": "Menambahkan Dua Angka Sebagai Linked List?", "id": "Setiap Node Merepresentasikan Satu Digit." },
  { "en": "Apa Itu Deep Copy?", "id": "Membuat Salinan Daftar Dengan Node Baru." },
  { "en": "Apa Itu Shallow Copy?", "id": "Hanya Menyalin Pointer Head." },
  { "en": "Apa Itu Abstract Data Type (ADT)?", "id": "Linked List Adalah Implementasi Dari ADT List." },
  { "en": "Apa Itu Koleksi (Collection)?", "id": "Grup Objek, Seperti Linked List." },
  { "en": "Apa Itu Iterator?", "id": "Objek Untuk Melintasi Elemen Koleksi." },
  { "en": "Implementasi Linked List Di Java?", "id": "Kelas `LinkedList`." },
  { "en": "Implementasi Linked List Di Python?", "id": "Tidak Ada Bawaan, Biasanya `deque`." },
  { "en": "Implementasi Linked List Di C/C++?", "id": "Menggunakan Struct Dan Pointer." },
  { "en": "Apa Itu Multi-level Linked List?", "id": "Node Memiliki Pointer `next` Dan `down`." },
  { "en": "Apa Itu Payload?", "id": "Data Yang Disimpan Dalam Node." },
  { "en": "Apa Itu Overhead?", "id": "Memori Tambahan Yang Digunakan Untuk Pointer." },
  { "en": "Trade-off Array vs Linked List?", "id": "Memori Cache, Alokasi, Kompleksitas Operasi." },
  { "en": "Array Lebih Baik Untuk Cache?", "id": "Ya, Karena Elemennya Berdekatan." },
  { "en": "Linked List Lebih Fleksibel Dalam Memori?", "id": "Ya, Node Dapat Dialokasikan Di Mana Saja." },
  { "en": "Penyisipan Di Tengah Array?", "id": "Lambat, O(n)." },
  { "en": "Penyisipan Di Tengah Linked List?", "id": "Cepat, O(1) Jika Node Diketahui." },
  { "en": "Menghapus Elemen Terakhir Doubly Linked List?", "id": "Cepat, O(1)." },
  { "en": "Menghapus Elemen Terakhir Singly Linked List?", "id": "Lambat, O(n)." },
  { "en": "Apa Itu Free List?", "id": "Daftar Node Memori Yang Siap Digunakan." },
  { "en": "Apa Itu Custom Allocator?", "id": "Manajemen Memori Khusus Untuk Performa." },
  { "en": "Apa Itu Thread Safety?", "id": "Aman Digunakan Oleh Banyak Thread." },
  { "en": "Membuat Linked List Thread-Safe?", "id": "Membutuhkan Mekanisme Penguncian (Lock)." },
  { "en": "Apa Itu Persistent Data Structure?", "id": "Struktur Data Yang Menjaga Versi Sebelumnya." },
  { "en": "Bisakah Linked List Persisten?", "id": "Ya, Sangat Cocok Untuk Implementasi Persisten." },
  { "en": "Apa Itu Zipper?", "id": "Struktur Data Fungsional Untuk Traversing." },
  { "en": "Apa Itu Pointer Jumping?", "id": "Teknik Algoritma Paralel Pada Linked List." },
  { "en": "Apa Itu Masalah Josephus?", "id": "Masalah Eliminasi Klasik Yang Menggunakan Daftar." },
  { "en": "Implementasi LRU Cache?", "id": "Menggunakan Hash Map Dan Doubly Linked List." },
  { "en": "Mengapa Doubly Linked List Untuk LRU?", "id": "Untuk Pemindahan Node Cepat Ke Depan/Belakang." },
  { "en": "Linked List vs Array Dinamis?", "id": "Linked List Lebih Baik Untuk Banyak Sisip/Hapus." },
  { "en": "Aplikasi Linked List Dalam Sistem Operasi?", "id": "Manajemen Proses, Alokasi Memori." },
  { "en": "Aplikasi Linked List Dalam Compiler?", "id": "Manajemen Tabel Simbol." },
  { "en": "Apa Itu Tombol Maju/Mundur Browser?", "id": "Dapat Diimplementasikan Dengan Doubly Linked List." },
  { "en": "Apa Itu Playlist Lagu?", "id": "Dapat Diimplementasikan Dengan Doubly Circular List." },
  { "en": "Apa Itu Adjacency List?", "id": "Representasi Graf Menggunakan Array Linked List." },
  { "en": "Apa Itu Sparse Matrix?", "id": "Dapat Diimplementasikan Secara Efisien Dengan Linked List." },
  { "en": "Apa Itu Struktur Data Non-Linear?", "id": "Elemen Tidak Disusun Secara Berurutan." },
  { "en": "Contoh Struktur Non-Linear?", "id": "Pohon (Tree) Dan Graf (Graph)." },
  { "en": "Apa Itu Pohon (Tree)?", "id": "Struktur Data Hierarkis." },
  { "en": "Terminologi Pohon?", "id": "Root, Node, Edge, Parent, Child, Leaf." },
  { "en": "Apa Itu Root?", "id": "Node Paling Atas, Tanpa Parent." },
  { "en": "Apa Itu Leaf?", "id": "Node Paling Bawah, Tanpa Child." },
  { "en": "Apa Itu Edge?", "id": "Koneksi Antara Dua Node." },
  { "en": "Apa Itu Subtree?", "id": "Pohon Yang Terdiri Dari Child Dan Keturunannya." },
  { "en": "Apa Itu Tinggi (Height) Pohon?", "id": "Panjang Jalur Terpanjang Dari Root Ke Leaf." },
  { "en": "Apa Itu Kedalaman (Depth) Node?", "id": "Jarak Dari Root Ke Node Tersebut." },
  { "en": "Apa Itu Pohon Biner (Binary Tree)?", "id": "Setiap Node Memiliki Paling Banyak Dua Child." },
  { "en": "Child Pada Pohon Biner?", "id": "Left Child Dan Right Child." },
  { "en": "Pohon Biner Penuh (Full)?", "id": "Setiap Node Memiliki 0 Atau 2 Child." },
  { "en": "Pohon Biner Sempurna (Perfect)?", "id": "Pohon Penuh Dimana Semua Leaf Sejajar." },
  { "en": "Pohon Biner Lengkap (Complete)?", "id": "Semua Level Terisi Penuh Kecuali Mungkin." },
  { "en": "Apa Itu Pohon Pencarian Biner (BST)?", "id": "Binary Search Tree." },
  { "en": "Properti Kunci BST?", "id": "Kiri < Induk < Kanan." },
  { "en": "Kompleksitas Operasi BST Seimbang?", "id": "O(log n)." },
  { "en": "Apa Itu Pohon Tidak Seimbang?", "id": "Tingginya Menjadi Seperti Linked List." },
  { "en": "Kompleksitas Terburuk BST?", "id": "O(n)." },
  { "en": "Apa Itu Pohon Seimbang (Balanced Tree)?", "id": "Pohon Yang Menjaga Tingginya Tetap Logaritmik." },
  { "en": "Contoh Pohon Seimbang?", "id": "AVL Tree, Red-Black Tree." },
  { "en": "Apa Itu AVL Tree?", "id": "Menjaga Keseimbangan Dengan Faktor Keseimbangan." },
  { "en": "Apa Itu Red-Black Tree?", "id": "Menggunakan Warna Node (Merah/Hitam) Untuk Keseimbangan." },
  { "en": "Apa Itu Heap?", "id": "Pohon Biner Lengkap Dengan Properti Heap." },
  { "en": "Properti Max-Heap?", "id": "Nilai Parent Selalu Lebih Besar Dari Child." },
  { "en": "Properti Min-Heap?", "id": "Nilai Parent Selalu Lebih Kecil Dari Child." },
  { "en": "Aplikasi Heap?", "id": "Priority Queue, Algoritma Heapsort." },
  { "en": "Apa Itu Trie (Prefix Tree)?", "id": "Pohon Untuk Menyimpan Dan Mencari String." },
  { "en": "Apa Itu B-Tree?", "id": "Pohon Pencarian Umum Untuk Database Dan File System." },
  { "en": "Karakteristik B-Tree?", "id": "Node Dapat Memiliki Banyak Child." },
  { "en": "Apa Itu Penjelajahan Pohon (Tree Traversal)?", "id": "Mengunjungi Semua Node Secara Sistematis." },
  { "en": "Traversal In-order?", "id": "Kiri - Induk - Kanan." },
  { "en": "Traversal Pre-order?", "id": "Induk - Kiri - Kanan." },
  { "en": "Traversal Post-order?", "id": "Kiri - Kanan - Induk." },
  { "en": "Traversal Level-order?", "id": "Mengunjungi Node Level Demi Level." },
  { "en": "Aplikasi Pohon?", "id": "Sistem File, Struktur XML/HTML (DOM)." },
  { "en": "Apa Itu Graf (Graph)?", "id": "Kumpulan Simpul (Vertices) Dan Sisi (Edges)." },
  { "en": "Graf Merepresentasikan Apa?", "id": "Hubungan Antar Objek." },
  { "en": "Apa Itu Simpul (Vertex)?", "id": "Merepresentasikan Objek Atau Titik." },
  { "en": "Apa Itu Sisi (Edge)?", "id": "Merepresentasikan Hubungan Antara Dua Simpul." },
  { "en": "Graf Tak Berarah (Undirected)?", "id": "Sisi Tidak Memiliki Arah." },
  { "en": "Graf Berarah (Directed)?", "id": "Sisi Memiliki Arah." },
  { "en": "Graf Berbobot (Weighted)?", "id": "Setiap Sisi Memiliki Nilai (Bobot)." },
  { "en": "Apa Itu Derajat (Degree) Simpul?", "id": "Jumlah Sisi Yang Terhubung." },
  { "en": "Apa Itu In-degree?", "id": "Jumlah Sisi Yang Masuk (Graf Berarah)." },
  { "en": "Apa Itu Out-degree?", "id": "Jumlah Sisi Yang Keluar (Graf Berarah)." },
  { "en": "Apa Itu Adjacency?", "id": "Dua Simpul Terhubung Langsung Oleh Sisi." },
  { "en": "Apa Itu Path (Jalur)?", "id": "Urutan Simpul Yang Terhubung Sisi." },
  { "en": "Apa Itu Siklus (Cycle)?", "id": "Jalur Yang Dimulai Dan Diakhiri Di Simpul Sama." },
  { "en": "Graf Terhubung (Connected)?", "id": "Ada Jalur Antara Setiap Pasang Simpul." },
  { "en": "Graf Lengkap (Complete)?", "id": "Setiap Pasang Simpul Terhubung Sisi." },
  { "en": "Apa Itu Representasi Graf?", "id": "Cara Menyimpan Struktur Graf Di Komputer." },
  { "en": "Apa Itu Adjacency Matrix?", "id": "Matriks Biner Yang Menunjukkan Hubungan." },
  { "en": "Apa Itu Adjacency List?", "id": "Daftar Simpul Tetangga Untuk Setiap Simpul." },
  { "en": "Kapan Menggunakan Adjacency Matrix?", "id": "Untuk Graf Padat (Dense)." },
  { "en": "Kapan Menggunakan Adjacency List?", "id": "Untuk Graf Jarang (Sparse)." },
  { "en": "Apa Itu Penjelajahan Graf (Graph Traversal)?", "id": "Proses Mengunjungi Semua Simpul." },
  { "en": "Apa Itu BFS (Breadth-First Search)?", "id": "Menjelajahi Tetangga Terdekat Terlebih Dahulu." },
  { "en": "BFS Menggunakan Struktur Data Apa?", "id": "Antrean (Queue)." },
  { "en": "Apa Itu DFS (Depth-First Search)?", "id": "Menjelajahi Sejauh Mungkin Sebelum Mundur." },
  { "en": "DFS Menggunakan Struktur Data Apa?", "id": "Tumpukan (Stack), Bisa Implisit (Rekursi)." },
  { "en": "Aplikasi Graf?", "id": "Jaringan Sosial, Peta Jalan, Internet." },
  { "en": "Apa Itu Algoritma Jalur Terpendek?", "id": "Mencari Jalur Dengan Total Bobot Minimum." },
  { "en": "Apa Itu Algoritma Dijkstra?", "id": "Mencari Jalur Terpendek Dari Satu Sumber." },
  { "en": "Syarat Algoritma Dijkstra?", "id": "Bobot Sisi Tidak Boleh Negatif." },
  { "en": "Apa Itu Algoritma Bellman-Ford?", "id": "Dapat Menangani Bobot Sisi Negatif." },
  { "en": "Apa Itu Algoritma Floyd-Warshall?", "id": "Mencari Jalur Terpendek Antara Semua Pasang Simpul." },
  { "en": "Apa Itu Minimum Spanning Tree (MST)?", "id": "Sub-graf Yang Menghubungkan Semua Simpul." },
  { "en": "Properti MST?", "id": "Tidak Memiliki Siklus, Total Bobot Minimum." },
  { "en": "Apa Itu Algoritma Prim?", "id": "Algoritma Greedy Untuk Menemukan MST." },
  { "en": "Apa Itu Algoritma Kruskal?", "id": "Algoritma Greedy Lain Untuk Menemukan MST." },
  { "en": "Apa Itu Pengurutan Topologis?", "id": "Urutan Linear Simpul Graf Berarah Asiklik." },
  { "en": "Syarat Pengurutan Topologis?", "id": "Graf Harus Berarah Dan Tidak Punya Siklus (DAG)." },
  { "en": "Apa Itu DAG?", "id": "Directed Acyclic Graph." },
  { "en": "Aplikasi Pengurutan Topologis?", "id": "Penjadwalan Tugas Dengan Dependensi." },
  { "en": "Apa Itu Strongly Connected Components?", "id": "Komponen Terhubung Kuat (Graf Berarah)." },
  { "en": "Apa Itu Masalah Aliran Maksimum?", "id": "Mencari Aliran Maksimum Dari Sumber Ke Sink." },
  { "en": "Apa Itu Jaringan Aliran (Flow Network)?", "id": "Graf Berarah Dengan Kapasitas." },
  { "en": "Apa Itu Masalah Traveling Salesperson (TSP)?", "id": "Mencari Tur Terpendek Yang Mengunjungi Semua Kota." },
  { "en": "Apakah TSP Sulit?", "id": "Ya, Merupakan Masalah NP-Hard." },
  { "en": "Apa Itu Masalah Pewarnaan Graf?", "id": "Mewarnai Simpul Agar Tidak Ada Tetangga Sama." },
  { "en": "Apa Itu Pohon Ekspresi?", "id": "Pohon Biner Untuk Merepresentasikan Ekspresi Aritmatika." },
  { "en": "Apa Itu Paradigma Pemrograman?", "id": "Gaya, Filosofi, Atau Cara Pemrograman." },
  { "en": "Apa Itu Pemrograman Imperatif?", "id": "Mendeskripsikan 'Bagaimana' Komputer Menyelesaikan Tugas." },
  { "en": "Karakteristik Pemrograman Imperatif?", "id": "Menggunakan Pernyataan Yang Mengubah Keadaan Program." },
  { "en": "Apa Itu Pemrograman Prosedural?", "id": "Bentuk Pemrograman Imperatif Berbasis Prosedur." },
  { "en": "Apa Itu Prosedur?", "id": "Nama Lain Untuk Fungsi Atau Subroutine." },
  { "en": "Apa Itu Pemrograman Berorientasi Objek (OOP)?", "id": "Paradigma Berbasis Objek Dan Kelas." },
  { "en": "Apa Itu Pemrograman Deklaratif?", "id": "Mendeskripsikan 'Apa' Hasil Yang Diinginkan." },
  { "en": "Karakteristik Pemrograman Deklaratif?", "id": "Tidak Secara Eksplisit Menyebutkan Aliran Kontrol." },
  { "en": "Apa Itu Pemrograman Fungsional?", "id": "Bentuk Pemrograman Deklaratif Berbasis Fungsi." },
  { "en": "Ciri Khas Pemrograman Fungsional?", "id": "Fungsi Murni, Immutability, Hindari Side Effect." },
  { "en": "Apa Itu Pemrograman Logika?", "id": "Berdasarkan Aturan Dan Fakta Logika Formal." },
  { "en": "Contoh Bahasa Logika?", "id": "Prolog." },
  { "en": "Apa Itu Pemrograman Berbasis Event?", "id": "Aliran Program Ditentukan Oleh Kejadian (Event)." },
  { "en": "Contoh Event?", "id": "Klik Mouse, Penekanan Tombol, Data Diterima." },
  { "en": "Di Mana Pemrograman Berbasis Event Digunakan?", "id": "Aplikasi Antarmuka Grafis (GUI), Server." },
  { "en": "Apa Itu Pemrograman Konkuren?", "id": "Menjalankan Beberapa Tugas Secara Tumpang Tindih." },
  { "en": "Apa Itu Pemrograman Paralel?", "id": "Menjalankan Beberapa Tugas Secara Bersamaan." },
  { "en": "Apa Itu Thread?", "id": "Unit Eksekusi Terkecil." },
  { "en": "Apa Itu Proses?", "id": "Instans Program Yang Sedang Berjalan." },
  { "en": "Apa Itu Sinkronisasi?", "id": "Mengoordinasikan Eksekusi Banyak Thread." },
  { "en": "Apa Itu Race Condition?", "id": "Error Akibat Akses Bersamaan Ke Sumber Daya." },
  { "en": "Apa Itu Deadlock?", "id": "Dua Atau Lebih Thread Saling Menunggu." },
  { "en": "Apa Itu Mutex (Lock)?", "id": "Mekanisme Untuk Menjamin Akses Eksklusif." },
  { "en": "Apa Itu Semaphore?", "id": "Mekanisme Sinkronisasi Lainnya." },
  { "en": "Apa Itu Pemrograman Asinkron?", "id": "Memulai Tugas Tanpa Menunggu Selesai." },
  { "en": "Manfaat Asinkron?", "id": "Meningkatkan Responsivitas, Terutama Untuk I/O." },
  { "en": "Apa Itu Callback?", "id": "Fungsi Yang Dijalankan Saat Tugas Asinkron Selesai." },
  { "en": "Apa Itu Promise (Future)?", "id": "Objek Yang Merepresentasikan Hasil Operasi Asinkron." },
  { "en": "Apa Itu Async/Await?", "id": "Sintaks Untuk Menyederhanakan Pemrograman Asinkron." },
  { "en": "Apa Itu Siklus Hidup Pengembangan Perangkat Lunak (SDLC)?", "id": "Software Development Life Cycle." },
  { "en": "Tahapan SDLC?", "id": "Perencanaan, Analisis, Desain, Implementasi, Pengujian, Pemeliharaan." },
  { "en": "Apa Itu Model Waterfall?", "id": "Model SDLC Sekuensial Dan Linear." },
  { "en": "Kekurangan Model Waterfall?", "id": "Kaku Dan Sulit Mengakomodasi Perubahan." },
  { "en": "Apa Itu Model Agile?", "id": "Pendekatan Iteratif Dan Inkremental." },
  { "en": "Apa Itu Scrum?", "id": "Kerangka Kerja Agile Yang Populer." },
  { "en": "Apa Itu Sprint?", "id": "Siklus Kerja Singkat Dalam Scrum." },
  { "en": "Apa Itu Kanban?", "id": "Metode Agile Yang Berfokus Pada Aliran Kerja." },
  { "en": "Apa Itu DevOps?", "id": "Kultur Dan Praktik Mengintegrasikan Development Dan Operations." },
  { "en": "Tujuan DevOps?", "id": "Mempercepat Dan Mengotomatiskan Pengiriman Perangkat Lunak." },
  { "en": "Apa Itu Continuous Integration (CI)?", "id": "Mengintegrasikan Perubahan Kode Secara Teratur." },
  { "en": "Apa Itu Continuous Delivery/Deployment (CD)?", "id": "Merilis Perangkat Lunak Ke Produksi Otomatis." },
  { "en": "Apa Itu Version Control System (VCS)?", "id": "Sistem Untuk Melacak Perubahan Kode." },
  { "en": "Apa Itu Git?", "id": "Sistem Kontrol Versi Terdistribusi Paling Populer." },
  { "en": "Apa Itu Repository?", "id": "Penyimpanan Pusat Untuk Proyek Kode." },
  { "en": "Apa Itu Commit?", "id": "Menyimpan Snapshot Perubahan Ke Repository." },
  { "en": "Apa Itu Branch?", "id": "Garis Pengembangan Independen." },
  { "en": "Apa Itu Merge?", "id": "Menggabungkan Perubahan Dari Branch Berbeda." },
  { "en": "Apa Itu Pengujian Perangkat Lunak (Testing)?", "id": "Proses Memverifikasi Fungsionalitas." },
  { "en": "Apa Itu Unit Testing?", "id": "Menguji Unit Kode Terkecil (Fungsi/Metode)." },
  { "en": "Apa Itu Integration Testing?", "id": "Menguji Interaksi Antara Beberapa Unit." },
  { "en": "Apa Itu System Testing?", "id": "Menguji Sistem Lengkap Secara Keseluruhan." },
  { "en": "Apa Itu Acceptance Testing?", "id": "Pengujian Oleh Pengguna Atau Klien." },
  { "en": "Apa Itu Black-Box Testing?", "id": "Menguji Tanpa Mengetahui Struktur Internal." },
  { "en": "Apa Itu White-Box Testing?", "id": "Menguji Dengan Mengetahui Struktur Internal." },
  { "en": "Apa Itu Regression Testing?", "id": "Memastikan Perubahan Baru Tidak Merusak Fitur Lama." },
  { "en": "Apa Itu Test-Driven Development (TDD)?", "id": "Menulis Tes Sebelum Menulis Kode." },
  { "en": "Apa Itu Kualitas Kode?", "id": "Ukuran Seberapa Baik Kode Ditulis." },
  { "en": "Apa Itu Keterbacaan (Readability)?", "id": "Seberapa Mudah Kode Dapat Dibaca." },
  { "en": "Apa Itu Kemudahan Perawatan (Maintainability)?", "id": "Seberapa Mudah Kode Dapat Dimodifikasi." },
  { "en": "Apa Itu Refactoring?", "id": "Memperbaiki Desain Internal Kode." },
  { "en": "Apa Itu Code Smell?", "id": "Indikasi Adanya Masalah Desain Dalam Kode." },
  { "en": "Apa Itu Technical Debt?", "id": "Implikasi Jangka Panjang Dari Jalan Pintas." },
  { "en": "Apa Itu Desain Perangkat Lunak?", "id": "Proses Merencanakan Struktur Perangkat Lunak." },
  { "en": "Apa Itu Pola Desain (Design Pattern)?", "id": "Solusi Umum Yang Dapat Digunakan Kembali." },
  { "en": "Apa Itu Arsitektur Perangkat Lunak?", "id": "Struktur Tingkat Tinggi Dari Suatu Sistem." },
  { "en": "Apa Itu Arsitektur Monolitik?", "id": "Aplikasi Dibangun Sebagai Satu Unit Tunggal." },
  { "en": "Apa Itu Arsitektur Microservices?", "id": "Aplikasi Dibangun Dari Kumpulan Layanan Kecil." },
  { "en": "Apa Itu API (Application Programming Interface)?", "id": "Kontrak Antara Komponen Perangkat Lunak." },
  { "en": "Apa Itu REST (Representational State Transfer)?", "id": "Gaya Arsitektur Populer Untuk API Web." },
  { "en": "Apa Itu Database?", "id": "Kumpulan Data Terorganisir." },
  { "en": "Apa Itu SQL?", "id": "Structured Query Language." },
  { "en": "Fungsi SQL?", "id": "Bahasa Untuk Berinteraksi Dengan Database Relasional." },
  { "en": "Apa Itu NoSQL?", "id": "Database Non-Relasional." },
  { "en": "Apa Itu Dokumentasi?", "id": "Materi Penjelasan Tentang Perangkat Lunak." },
  { "en": "Apa Itu Lisensi Perangkat Lunak?", "id": "Izin Legal Untuk Menggunakan Perangkat Lunak." },
  { "en": "Apa Itu Open Source?", "id": "Perangkat Lunak Dengan Kode Sumber Terbuka." },
  { "en": "Apa Itu Free Software?", "id": "Perangkat Lunak Yang Menghormati Kebebasan Pengguna." },
  { "en": "Apa Itu Build Automation?", "id": "Mengotomatiskan Proses Kompilasi Dan Pengemasan." },
  { "en": "Apa Itu Paket Manajer?", "id": "Alat Untuk Mengelola Library Pihak Ketiga." },
  { "en": "Apa Itu Dependensi?", "id": "Ketergantungan Suatu Proyek Pada Library Lain." },
  { "en": "Apa Itu Web Development?", "id": "Membangun Situs Web Dan Aplikasi Web." },
  { "en": "Apa Itu Front-end?", "id": "Bagian Aplikasi Yang Dilihat Pengguna (Browser)." },
  { "en": "Apa Itu Back-end?", "id": "Bagian Aplikasi Di Sisi Server." },
  { "en": "Apa Itu Full-stack?", "id": "Mengerjakan Baik Front-end Maupun Back-end." },
  { "en": "Apa Itu Mobile Development?", "id": "Membangun Aplikasi Untuk Perangkat Seluler." },
  { "en": "Apa Itu UI (User Interface)?", "id": "Antarmuka Visual Pengguna." },
  { "en": "Apa Itu UX (User Experience)?", "id": "Pengalaman Keseluruhan Pengguna." },
  { "en": "Apa Itu Aksesibilitas?", "id": "Membuat Aplikasi Dapat Digunakan Oleh Semua Orang." },
  { "en": "Apa Itu Lingkungan Pengembangan?", "id": "Kumpulan Alat Untuk Membuat Perangkat Lunak." },
  { "en": "Apa Itu Build System?", "id": "Mengotomatiskan Proses Kompilasi Dan Build." },
  { "en": "Contoh Build System?", "id": "Make, Gradle, Maven." },
  { "en": "Apa Itu Linker?", "id": "Menggabungkan File Objek Menjadi Program Eksekutabel." },
  { "en": "Apa Itu Loader?", "id": "Memuat Program Ke Memori Untuk Dijalankan." },
  { "en": "Apa Itu Pustaka Statis (Static Library)?", "id": "Kode Pustaka Disalin Saat Kompilasi." },
  { "en": "Apa Itu Pustaka Dinamis (Dynamic Library)?", "id": "Kode Pustaka Dimuat Saat Program Berjalan." },
  { "en": "Apa Itu Paket Manajer?", "id": "Mengelola Dependensi Dan Pustaka Proyek." },
  { "en": "Contoh Paket Manajer?", "id": "NPM (JavaScript), Pip (Python), Maven (Java)." },
  { "en": "Apa Itu Dependensi?", "id": "Ketergantungan Proyek Pada Pustaka Eksternal." },
  { "en": "Apa Itu Virtual Environment?", "id": "Lingkungan Terisolasi Untuk Dependensi Proyek." },
  { "en": "Apa Itu Containerization?", "id": "Mengemas Aplikasi Beserta Dependensinya." },
  { "en": "Contoh Teknologi Container?", "id": "Docker." },
  { "en": "Apa Itu Virtual Machine (VM)?", "id": "Emulasi Lengkap Dari Sistem Komputer." },
  { "en": "Container vs VM?", "id": "Container Lebih Ringan Dan Cepat." },
  { "en": "Apa Itu Linter?", "id": "Alat Analisis Kode Statis Untuk Gaya." },
  { "en": "Apa Itu Code Formatter?", "id": "Mengatur Format Kode Secara Otomatis." },
  { "en": "Apa Itu Profiler?", "id": "Alat Untuk Menganalisis Kinerja Program." },
  { "en": "Apa Itu Bottleneck?", "id": "Bagian Paling Lambat Dari Program." },
  { "en": "Apa Itu Serialisasi?", "id": "Mengubah Objek Menjadi Format Untuk Disimpan." },
  { "en": "Apa Itu Deserialisasi?", "id": "Mengubah Format Simpanan Kembali Menjadi Objek." },
  { "en": "Apa Itu JSON?", "id": "JavaScript Object Notation." },
  { "en": "Fungsi JSON?", "id": "Format Pertukaran Data Yang Ringan." },
  { "en": "Apa Itu XML?", "id": "Extensible Markup Language." },
  { "en": "Apa Itu YAML?", "id": "Format Serialisasi Data Yang Mudah Dibaca." },
  { "en": "Apa Itu CSV?", "id": "Comma-Separated Values." },
  { "en": "Fungsi CSV?", "id": "Format Teks Sederhana Untuk Data Tabular." },
  { "en": "Apa Itu Karakter Encoding?", "id": "Cara Angka Biner Merepresentasikan Teks." },
  { "en": "Apa Itu UTF-8?", "id": "Encoding Karakter Paling Umum Di Web." },
  { "en": "Apa Itu Internationalization (i18n)?", "id": "Mendesain Aplikasi Untuk Berbagai Bahasa." },
  { "en": "Apa Itu Localization (l10n)?", "id": "Mengadaptasi Aplikasi Untuk Lokasi Tertentu." },
  { "en": "Apa Itu Thread?", "id": "Jalur Eksekusi Independen Di Dalam Proses." },
  { "en": "Apa Itu Multithreading?", "id": "Menjalankan Beberapa Thread Secara Bersamaan." },
  { "en": "Apa Itu Race Condition?", "id": "Hasil Bergantung Pada Urutan Eksekusi Thread." },
  { "en": "Apa Itu Deadlock?", "id": "Saat Dua Thread Saling Menunggu." },
  { "en": "Apa Itu Mutex?", "id": "Mekanisme Kunci Untuk Mencegah Race Condition." },
  { "en": "Apa Itu Semaphore?", "id": "Mengizinkan Sejumlah Thread Mengakses Sumber Daya." },
  { "en": "Apa Itu Konsep Immutable?", "id": "Objek Yang Tidak Dapat Diubah." },
  { "en": "Manfaat Immutable?", "id": "Aman Untuk Digunakan Dalam Multithreading." },
  { "en": "Apa Itu Thread Pool?", "id": "Kumpulan Thread Yang Siap Mengerjakan Tugas." },
  { "en": "Apa Itu GUI?", "id": "Graphical User Interface." },
  { "en": "Apa Itu CLI?", "id": "Command-Line Interface." },
  { "en": "Apa Itu Event Loop?", "id": "Model Pemrograman Untuk Aplikasi GUI Asinkron." },
  { "en": "Apa Itu HTTP?", "id": "Hypertext Transfer Protocol." },
  { "en": "Fungsi HTTP?", "id": "Protokol Fondasi Untuk World Wide Web." },
  { "en": "Apa Itu Metode HTTP GET?", "id": "Meminta Data Dari Server." },
  { "en": "Apa Itu Metode HTTP POST?", "id": "Mengirim Data Ke Server." },
  { "en": "Apa Itu Status Code 200 OK?", "id": "Permintaan Berhasil." },
  { "en": "Apa Itu Status Code 404 Not Found?", "id": "Sumber Daya Yang Diminta Tidak Ditemukan." },
  { "en": "Apa Itu HTML?", "id": "HyperText Markup Language." },
  { "en": "Fungsi HTML?", "id": "Menstrukturkan Konten Halaman Web." },
  { "en": "Apa Itu CSS?", "id": "Cascading Style Sheets." },
  { "en": "Fungsi CSS?", "id": "Menata Tampilan Halaman Web." },
  { "en": "Apa Itu JavaScript?", "id": "Bahasa Pemrograman Untuk Interaktivitas Web." },
  { "en": "Apa Itu DOM?", "id": "Document Object Model." },
  { "en": "Fungsi DOM?", "id": "Representasi Pohon Dari Halaman HTML." },
  { "en": "Apa Itu Keamanan Perangkat Lunak?", "id": "Melindungi Perangkat Lunak Dari Ancaman." },
  { "en": "Apa Itu Enkripsi?", "id": "Mengubah Data Menjadi Format Tidak Terbaca." },
  { "en": "Apa Itu Dekripsi?", "id": "Mengembalikan Data Terenkripsi Ke Bentuk Asli." },
  { "en": "Apa Itu Hashing?", "id": "Menghasilkan Nilai Unik Berukuran Tetap." },
  { "en": "Fungsi Hashing?", "id": "Verifikasi Integritas Data, Penyimpanan Kata Sandi." },
  { "en": "Apakah Hashing Bisa Dibalik?", "id": "Tidak, Hashing Adalah Fungsi Satu Arah." },
  { "en": "Apa Itu SQL Injection?", "id": "Jenis Serangan Terhadap Database." },
  { "en": "Apa Itu Cross-Site Scripting (XSS)?", "id": "Jenis Serangan Yang Menyuntikkan Skrip." },
  { "en": "Apa Itu Otentikasi?", "id": "Proses Memverifikasi Siapa Anda." },
  { "en": "Apa Itu Otorisasi?", "id": "Proses Menentukan Apa Yang Boleh Anda Lakukan." },
  { "en": "Apa Itu Cookie?", "id": "Data Kecil Yang Disimpan Oleh Browser." },
  { "en": "Apa Itu Session?", "id": "Mekanisme Untuk Melacak Pengguna." },
  { "en": "Apa Itu Caching?", "id": "Menyimpan Salinan Data Untuk Akses Cepat." },
  { "en": "Apa Itu CDN?", "id": "Content Delivery Network." },
  { "en": "Fungsi CDN?", "id": "Menyebarkan Konten Lebih Dekat Ke Pengguna." },
  { "en": "Apa Itu Load Balancing?", "id": "Mendistribusikan Beban Lalu Lintas." },
  { "en": "Apa Itu Skalabilitas Vertikal?", "id": "Meningkatkan Kekuatan Satu Server." },
  { "en": "Apa Itu Skalabilitas Horisontal?", "id": "Menambah Lebih Banyak Server." },
  { "en": "Apa Itu Refleksi (Reflection)?", "id": "Kemampuan Program Untuk Memeriksa Dirinya Sendiri." },
  { "en": "Apa Itu Pola Desain (Design Pattern)?", "id": "Solusi Teruji Untuk Masalah Desain Umum." },
  { "en": "Apa Itu Pola Singleton?", "id": "Memastikan Sebuah Kelas Hanya Punya Satu Instans." },
  { "en": "Apa Itu Pola Factory?", "id": "Menyederhanakan Proses Pembuatan Objek." },
  { "en": "Apa Itu Pola Observer?", "id": "Memungkinkan Objek Berlangganan Notifikasi." },
  { "en": "Apa Itu Pola MVC?", "id": "Model-View-Controller." },
  { "en": "Fungsi Pola MVC?", "id": "Pola Arsitektur Untuk Memisahkan Logika." },
  { "en": "Apa Itu Kode Prosedural?", "id": "Kode Yang Ditulis Dalam Urutan Langkah." },
  { "en": "Apa Itu Kode Deklaratif?", "id": "Kode Yang Mendeskripsikan Hasil Akhir." },
  { "en": "Apa Itu Domain-Specific Language (DSL)?", "id": "Bahasa Yang Dibuat Untuk Domain Masalah." },
  { "en": "Apa Itu Metaprogramming?", "id": "Program Yang Menulis Atau Memanipulasi Program." },
  { "en": "Apa Itu Functional Programming?", "id": "Paradigma Yang Memperlakukan Komputasi Sebagai Fungsi." },
  { "en": "Apa Itu Pure Function?", "id": "Fungsi Tanpa Efek Samping (Side Effects)." },
  { "en": "Apa Itu Immutability?", "id": "Data Yang Tidak Dapat Diubah." },
  { "en": "Apa Itu Higher-Order Function?", "id": "Fungsi Yang Menerima Atau Mengembalikan Fungsi." },
  { "en": "Apa Itu DRY?", "id": "Don't Repeat Yourself." },
  { "en": "Apa Itu KISS?", "id": "Keep It Simple, Stupid." },
  { "en": "Apa Itu YAGNI?", "id": "You Ain't Gonna Need It." },
  { "en": "Apa Itu SOLID?", "id": "Lima Prinsip Desain Berorientasi Objek." }



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
