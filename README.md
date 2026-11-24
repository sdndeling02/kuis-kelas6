# kuis-kelas6
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portal Belajar & Kuis Kelas 6 SD</title>
    <style>
        :root {
            --primary: #4F46E5;
            --primary-dark: #3730A3;
            --secondary: #10B981;
            --danger: #EF4444;
            --bg: #F3F4F6;
            --card: #FFFFFF;
            --text: #1F2937;
        }
        * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body { background-color: var(--bg); color: var(--text); margin: 0; padding: 20px; display: flex; justify-content: center; }
        .container { width: 100%; max-width: 750px; background: var(--card); border-radius: 16px; box-shadow: 0 10px 25px rgba(0,0,0,0.08); padding: 25px; overflow: hidden; }
        
        /* Header */
        header { text-align: center; border-bottom: 2px solid #E5E7EB; padding-bottom: 15px; margin-bottom: 20px; }
        header h1 { margin: 0; color: var(--primary); font-size: 1.6rem; }
        header p { margin: 5px 0 0; color: #6B7280; font-size: 0.95rem; }

        /* Form Nama */
        .student-info { background: #EEF2FF; padding: 15px; border-radius: 12px; margin-bottom: 20px; display: flex; gap: 10px; flex-wrap: wrap; }
        .student-info input { flex: 1; min-width: 200px; padding: 10px 14px; border: 2px solid #C7D2FE; border-radius: 8px; font-size: 1rem; outline: none; }
        .student-info input:focus { border-color: var(--primary); }

        /* Grid Mata Pelajaran */
        .subject-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 15px; }
        .subject-card { background: #fff; border: 2px solid #E5E7EB; border-radius: 12px; padding: 20px 15px; text-align: center; cursor: pointer; transition: all 0.2s; }
        .subject-card:hover { transform: translateY(-4px); border-color: var(--primary); box-shadow: 0 6px 15px rgba(79, 70, 229, 0.15); }
        .subject-icon { font-size: 2.2rem; margin-bottom: 8px; display: block; }
        .subject-title { font-weight: bold; font-size: 1rem; color: var(--text); }
        .subject-count { font-size: 0.8rem; color: #6B7280; margin-top: 4px; }

        /* Layar Kuis */
        .hidden { display: none !important; }
        .quiz-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; font-weight: bold; color: var(--primary); background: #EEF2FF; padding: 10px 15px; border-radius: 8px; }
        .question-box { font-size: 1.15rem; font-weight: 600; line-height: 1.6; margin-bottom: 20px; background: #F9FAFB; padding: 18px; border-radius: 10px; border-left: 5px solid var(--primary); }
        
        .options-list { display: flex; flex-direction: column; gap: 12px; }
        .option-btn { text-align: left; padding: 14px 18px; font-size: 1rem; background: #fff; border: 2px solid #D1D5DB; border-radius: 10px; cursor: pointer; transition: all 0.15s; font-weight: 500; }
        .option-btn:hover:not(:disabled) { background: #F3F4F6; border-color: var(--primary); }
        .option-btn.correct { background: #D1FAE5; border-color: var(--secondary); color: #065F46; font-weight: bold; }
        .option-btn.wrong { background: #FEE2E2; border-color: var(--danger); color: #991B1B; }
        .option-btn:disabled { cursor: default; }

        /* Pembahasan */
        .explanation-box { margin-top: 18px; padding: 15px; border-radius: 10px; background: #FFFBEB; border: 1px solid #FCD34D; color: #92400E; font-size: 0.95rem; line-height: 1.5; }
        
        /* Tombol Navigasi */
        .btn { background: var(--primary); color: white; border: none; padding: 12px 24px; font-size: 1rem; font-weight: bold; border-radius: 8px; cursor: pointer; margin-top: 20px; width: 100%; transition: 0.2s; }
        .btn:hover { background: var(--primary-dark); }
        .btn-secondary { background: #6B7280; margin-top: 10px; }
        .btn-wa { background: #25D366; }
        .btn-wa:hover { background: #1EBE5D; }

        /* Layar Hasil */
        .result-box { text-align: center; padding: 20px 10px; }
        .score-circle { width: 130px; height: 130px; border-radius: 50%; background: var(--primary); color: white; display: flex; flex-direction: column; justify-content: center; align-items: center; margin: 0 auto 20px; font-size: 2.2rem; font-weight: bold; box-shadow: 0 8px 20px rgba(79, 70, 229, 0.3); }
        .score-circle span { font-size: 0.85rem; font-weight: normal; opacity: 0.9; }
    </style>
</head>
<body>

<div class="container">
    <header>
        <h1>🎓 Zona Belajar Kelas 6 SD</h1>
        <p>Latihan Soal Interaktif Semua Mata Pelajaran</p>
    </header>

    <!-- 1. HALAMAN UTAMA (PILIH MAPEL) -->
    <div id="home-screen">
        <div class="student-info">
            <input type="text" id="student-name" placeholder="Ketik Nama Lengkap Siswa..." required>
            <input type="text" id="student-class" placeholder="Kelas (Contoh: 6A)" value="Kelas 6">
        </div>
        <h3 style="margin-top:0;">Pilih Mata Pelajaran:</h3>
        <div class="subject-grid" id="subject-list"></div>
    </div>

    <!-- 2. HALAMAN PENGERJAAN KUIS -->
    <div id="quiz-screen" class="hidden">
        <div class="quiz-header">
            <span id="quiz-subject-title">Mata Pelajaran</span>
            <span id="question-counter">Soal 1 / 5</span>
        </div>
        <div class="question-box" id="question-text">Teks pertanyaan muncul di sini...</div>
        <div class="options-list" id="options-container"></div>
        <div id="explanation" class="explanation-box hidden"></div>
        <button id="next-btn" class="btn hidden" onclick="nextQuestion()">Soal Berikutnya ➡️</button>
        <button class="btn btn-secondary" onclick="goHome()">🏠 Batal & Kembali ke Menu</button>
    </div>

    <!-- 3. HALAMAN HASIL AKHIR -->
    <div id="result-screen" class="hidden">
        <div class="result-box">
            <h2 id="result-greeting">Hebat, Siswa!</h2>
            <p id="result-detail" style="color: #4B5563; margin-bottom: 20px;"></p>
            <div class="score-circle">
                <div id="final-score">100</div>
                <span>NILAI AKHIR</span>
            </div>
            <p id="result-stats" style="font-weight: bold; font-size: 1.1rem;"></p>
            <button class="btn btn-wa" onclick="sendWhatsApp()">📲 Kirim Nilai ke WhatsApp Guru / Ortu</button>
            <button class="btn" onclick="goHome()">🔄 Pilih Mata Pelajaran Lain</button>
        </div>
    </div>
</div>

<script>
// =========================================================================
// PENGATURAN NOMOR WHATSAPP GURU / ORANG TUA (Ganti dengan nomor Anda, awali 62)
// =========================================================================
const NOMOR_WA_TUJUAN = "6281234567890"; 

// =========================================================================
// BANK SOAL KELAS 6 SD (Anda bisa menambah/mengedit soal di bagian ini)
// Kunci jawaban: 0 = A, 1 = B, 2 = C, 3 = D
// =========================================================================
const bankSoal = {
    "Matematika": {
        icon: "📐",
        soal: [
            {
                tanya: "Hasil dari -15 + 28 - (-12) adalah ....",
                pilihan: ["1", "25", "-25", "55"],
                jawaban: 1,
                pembahasan: "-15 + 28 = 13. Karena bertemu tanda minus ganda (- (-12)), maka menjadi +12. Jadi, 13 + 12 = 25."
            },
            {
                tanya: "Sebuah lingkaran memiliki jari-jari 14 cm. Berapakah luas lingkaran tersebut? (π = 22/7)",
                pilihan: ["88 cm²", "154 cm²", "616 cm²", "1.232 cm²"],
                jawaban: 2,
                pembahasan: "Luas = π × r × r = 22/7 × 14 × 14 = 22 × 2 × 14 = 616 cm²."
            },
            {
                tanya: "Bentuk pecahan desimal dari 3/8 adalah ....",
                pilihan: ["0,375", "0,38", "0,625", "0,125"],
                jawaban: 0,
                pembahasan: "3 dibagi 8 sama dengan 0,375 (atau 3/8 × 125/125 = 375/1000 = 0,375)."
            }
        ]
    },
    "IPAS": {
        icon: "🔬",
        soal: [
            {
                tanya: "Tumbuhan kaktus memiliki daun yang berbentuk duri dengan tujuan untuk ....",
                pilihan: ["Menarik serangga", "Mengurangi penguapan air", "Menyimpan cadangan makanan", "Mempercepat fotosintesis"],
                jawaban: 1,
                pembahasan: "Kaktus hidup di daerah kering (gurun), sehingga daunnya termodifikasi menjadi duri untuk menekan penguapan air."
            },
            {
                tanya: "Planet terbesar dalam tata surya kita adalah ....",
                pilihan: ["Saturnus", "Uranus", "Jupiter", "Neptunus"],
                jawaban: 2,
                pembahasan: "Jupiter adalah planet terbesar di tata surya dengan diameter sekitar 11 kali diameter Bumi."
            },
            {
                tanya: "Perubahan energi yang terjadi pada panel surya adalah ....",
                pilihan: ["Energi listrik menjadi energi panas", "Energi cahaya menjadi energi listrik", "Energi gerak menjadi energi listrik", "Energi kimia menjadi energi cahaya"],
                jawaban: 1,
                pembahasan: "Panel surya menangkap energi cahaya matahari dan mengubahnya langsung menjadi energi listrik."
            }
        ]
    },
    "Bahasa Indonesia": {
        icon: "📚",
        soal: [
            {
                tanya: "Kalimat utama yang terletak di akhir paragraf disebut paragraf ....",
                pilihan: ["Deduktif", "Induktif", "Campuran", "Naratif"],
                jawaban: 1,
                pembahasan: "Deduktif = di awal paragraf, Induktif = di akhir paragraf, Campuran = di awal dan akhir."
            },
            {
                tanya: "Penulisan kata baku yang benar di bawah ini adalah ....",
                pilihan: ["Apotik dan Praktek", "Apotek dan Praktik", "Apotik dan Praktik", "Apotek dan Praktek"],
                jawaban: 1,
                pembahasan: "Sesuai KBBI, bentuk baku yang tepat adalah 'apotek' dan 'praktik'."
            }
        ]
    },
    "Pendidikan Pancasila": {
        icon: "🦅",
        soal: [
            {
                tanya: "Mengutamakan musyawarah untuk mufakat dalam mengambil keputusan bersama merupakan pengamalan Pancasila sila ke- ....",
                pilihan: ["Dua", "Tiga", "Empat", "Lima"],
                jawaban: 2,
                pembahasan: "Musyawarah dan demokrasi merupakan nilai utama dari sila ke-4 Pancasila."
            },
            {
                tanya: "Semboyan 'Bhinneka Tunggal Ika' diambil dari kitab Sutasoma karangan ....",
                pilihan: ["Mpu Tantular", "Mpu Prapanca", "Mpu Gandring", "Mpu Sedah"],
                jawaban: 0,
                pembahasan: "Semboyan Bhinneka Tunggal Ika ditulis oleh Mpu Tantular pada masa Kerajaan Majapahit."
            }
        ]
    },
    "Bahasa Inggris": {
        icon: "🔤",
        soal: [
            {
                tanya: "Rina ... to Bali with her family last holiday.",
                pilihan: ["go", "goes", "went", "going"],
                jawaban: 2,
                pembahasan: "Keterangan waktu 'last holiday' menandakan Simple Past Tense (masa lampau), sehingga menggunakan Verb 2 dari 'go' yaitu 'went'."
            },
            {
                tanya: "What time is 07.30 in English?",
                pilihan: ["Half past seven", "Half to seven", "Seven past thirty", "A quarter past seven"],
                jawaban: 0,
                pembahasan: "07.30 berarti lewat 30 menit (setengah jam) dari pukul 7, sehingga disebut 'Half past seven'."
            }
        ]
    },
    "Seni Budaya": {
        icon: "🎨",
        soal: [
            {
                tanya: "Lagu daerah 'Apuse' berasal dari provinsi ....",
                pilihan: ["Maluku", "Papua", "Sulawesi Utara", "Nusa Tenggara Timur"],
                jawaban: 1,
                pembahasan: "Lagu Apuse merupakan lagu daerah terkenal yang berasal dari Papua."
            }
        ]
    },
    "PJOK": {
        icon: "⚽",
        soal: [
            {
                tanya: "Jumlah pemain inti dalam satu regu permainan bola voli adalah ....",
                pilihan: ["5 orang", "6 orang", "11 orang", "12 orang"],
                jawaban: 1,
                pembahasan: "Setiap regu bola voli terdiri dari 6 orang pemain inti di lapangan."
            }
        ]
    }
};

// =========================================================================
// LOGIKA PROGRAM KUIS (Tidak perlu diubah kecuali ingin kustomisasi)
// =========================================================================
let currentSubject = "";
let currentQuestions = [];
let currentIndex = 0;
let scoreCount = 0;

// Efek suara sederhana menggunakan Web Audio API (tanpa file mp3 eksternal)
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
function playTone(freq, type, duration) {
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();
    osc.type = type;
    osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
    gain.gain.exponentialRampToValueAtTime(0.00001, audioCtx.currentTime + duration);
    osc.connect(gain);
    gain.connect(audioCtx.destination);
    osc.start();
    osc.stop(audioCtx.currentTime + duration);
}

// Render Daftar Mata Pelajaran di Halaman Depan
function renderSubjects() {
    const grid = document.getElementById("subject-list");
    grid.innerHTML = "";
    for (let mapel in bankSoal) {
        const data = bankSoal[mapel];
        const card = document.createElement("div");
        card.className = "subject-card";
        card.innerHTML = `
            <span class="subject-icon">${data.icon}</span>
            <div class="subject-title">${mapel}</div>
            <div class="subject-count">${data.soal.length} Soal Latihan</div>
        `;
        card.onclick = () => startQuiz(mapel);
        grid.appendChild(card);
    }
}

function startQuiz(mapel) {
    const nama = document.getElementById("student-name").value.trim();
    if (!nama) {
        alert("Silakan isi Nama Lengkap Siswa terlebih dahulu ya!");
        document.getElementById("student-name").focus();
        return;
    }
    currentSubject = mapel;
    currentQuestions = bankSoal[mapel].soal;
    currentIndex = 0;
    scoreCount = 0;

    document.getElementById("home-screen").classList.add("hidden");
    document.getElementById("result-screen").classList.add("hidden");
    document.getElementById("quiz-screen").classList.remove("hidden");
    document.getElementById("quiz-subject-title").innerText = `${bankSoal[mapel].icon} ${mapel}`;
    
    loadQuestion();
}

function loadQuestion() {
    const q = currentQuestions[currentIndex];
    document.getElementById("question-counter").innerText = `Soal ${currentIndex + 1} / ${currentQuestions.length}`;
    document.getElementById("question-text").innerText = `${currentIndex + 1}. ${q.tanya}`;
    
    const optionsContainer = document.getElementById("options-container");
    optionsContainer.innerHTML = "";
    
    const expBox = document.getElementById("explanation");
    expBox.classList.add("hidden");
    document.getElementById("next-btn").classList.add("hidden");

    const huruf = ["A", "B", "C", "D"];
    q.pilihan.forEach((opt, idx) => {
        const btn = document.createElement("button");
        btn.className = "option-btn";
        btn.innerText = `${huruf[idx]}. ${opt}`;
        btn.onclick = () => checkAnswer(idx, btn);
        optionsContainer.appendChild(btn);
    });
}

function checkAnswer(selectedIndex, btnElement) {
    const q = currentQuestions[currentIndex];
    const allButtons = document.querySelectorAll(".option-btn");
    allButtons.forEach(b => b.disabled = true);

    if (selectedIndex === q.jawaban) {
        btnElement.classList.add("correct");
        scoreCount++;
        playTone(600, "sine", 0.25); // Suara Benar
    } else {
        btnElement.classList.add("wrong");
        allButtons[q.jawaban].classList.add("correct");
        playTone(200, "sawtooth", 0.3); // Suara Salah
    }

    const expBox = document.getElementById("explanation");
    expBox.innerHTML = `<strong>💡 Pembahasan:</strong> ${q.pembahasan}`;
    expBox.classList.remove("hidden");

    const nextBtn = document.getElementById("next-btn");
    nextBtn.innerText = (currentIndex === currentQuestions.length - 1) ? "Lihat Nilai Akhir 🏆" : "Soal Berikutnya ➡️";
    nextBtn.classList.remove("hidden");
}

function nextQuestion() {
    currentIndex++;
    if (currentIndex < currentQuestions.length) {
        loadQuestion();
    } else {
        showResult();
    }
}

function showResult() {
    document.getElementById("quiz-screen").classList.add("hidden");
    document.getElementById("result-screen").classList.remove("hidden");

    const nama = document.getElementById("student-name").value.trim();
    const kelas = document.getElementById("student-class").value.trim();
    const nilaiAkhir = Math.round((scoreCount / currentQuestions.length) * 100);

    document.getElementById("result-greeting").innerText = `Selesai, ${nama}!`;
    document.getElementById("result-detail").innerText = `${kelas} • Mata Pelajaran: ${currentSubject}`;
    document.getElementById("final-score").innerText = nilaiAkhir;
    document.getElementById("result-stats").innerText = `Jawaban Benar: ${scoreCount} dari ${currentQuestions.length} soal`;
}

function sendWhatsApp() {
    const nama = document.getElementById("student-name").value.trim();
    const kelas = document.getElementById("student-class").value.trim();
    const nilaiAkhir = Math.round((scoreCount / currentQuestions.length) * 100);
    
    const pesan = `Halo, berikut laporan hasil latihan soal online:\n\n` +
                  `👤 Nama: *${nama}*\n` +
                  `🏫 Kelas: *${kelas}*\n` +
                  `📚 Mapel: *${currentSubject}*\n` +
                  `✅ Benar: ${scoreCount}/${currentQuestions.length}\n` +
                  `🏆 Nilai Akhir: *${nilaiAkhir}*`;
                  
    const url = `https://wa.me/${NOMOR_WA_TUJUAN}?text=${encodeURIComponent(pesan)}`;
    window.open(url, "_blank");
}

function goHome() {
    document.getElementById("quiz-screen").classList.add("hidden");
    document.getElementById("result-screen").classList.add("hidden");
    document.getElementById("home-screen").classList.remove("hidden");
    renderSubjects();
}

// Jalankan saat pertama kali dibuka
renderSubjects();
</script>

</body>
</html>
