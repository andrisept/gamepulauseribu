<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Eksplorasi Bijak Kepulauan Seribu</title>
    <!-- Tailwind CSS CDN untuk styling yang cepat dan responsif -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #e0f2f7; /* Light blue background */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px; /* Padding for mobile view */
            box-sizing: border-box;
        }
        #game-container {
            background-color: #ffffff;
            border-radius: 1.5rem; /* Rounded corners */
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 768px; /* Max width for larger screens */
            padding: 1.5rem; /* Padding for content */
            text-align: center;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 500px; /* Minimum height to keep structure */
        }
        .btn {
            padding: 0.75rem 1.5rem;
            border-radius: 0.75rem; /* Rounded corners for buttons */
            font-weight: 600;
            transition: all 0.2s ease-in-out;
            cursor: pointer;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            margin: 0.5rem;
            display: flex; /* Use flex to center icon and text */
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
        }
        .btn-primary {
            background-color: #007bff; /* Blue */
            color: white;
            border: none;
        }
        .btn-primary:hover {
            background-color: #0056b3;
            transform: translateY(-2px);
            box-shadow: 0 6px 10px rgba(0, 0, 0, 0.15);
        }
        .btn-secondary {
            background-color: #f0f0f0; /* Light gray */
            color: #333;
            border: 1px solid #ccc;
        }
        .btn-secondary:hover {
            background-color: #e0e0e0;
            transform: translateY(-2px);
            box-shadow: 0 6px 10px rgba(0, 0, 0, 0.15);
        }
        .option-btn {
            width: 100%;
            text-align: left;
            margin-bottom: 0.75rem;
            border: 1px solid #d1d5db; /* Light gray border */
            background-color: #f9fafb; /* Off-white background */
            color: #374151; /* Dark gray text */
        }
        .option-btn:hover:not(.correct):not(.incorrect) {
            background-color: #e5e7eb;
            transform: translateY(0); /* Prevent transform on hover to keep selected state clear */
        }
        .option-btn.correct {
            background-color: #d1fae5; /* Green light */
            border-color: #10b981; /* Green */
            color: #10b981;
            font-weight: bold;
        }
        .option-btn.incorrect {
            background-color: #fee2e2; /* Red light */
            border-color: #ef4444; /* Red */
            color: #ef4444;
            font-weight: bold;
        }
        .feedback-message {
            margin-top: 1rem;
            font-size: 1rem;
            font-weight: 600;
            padding: 0.75rem;
            border-radius: 0.5rem;
            width: 100%;
        }
        .feedback-correct {
            background-color: #d1fae5;
            color: #10b981;
        }
        .feedback-incorrect {
            background-color: #fee2e2;
            color: #ef4444;
        }
        .explanation {
            font-size: 0.9rem;
            color: #4b5563; /* Dark gray */
            margin-top: 0.5rem;
            text-align: left;
            width: 100%;
            line-height: 1.4;
        }
        .ai-output-container {
            margin-top: 1.5rem;
            padding: 1rem;
            background-color: #f0f9ff; /* Very light blue */
            border-radius: 0.75rem;
            border: 1px solid #bfdbfe; /* Light blue border */
            text-align: left;
            font-size: 0.9rem;
            color: #1e3a8a; /* Darker blue text */
            box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.05);
        }
        .loading-spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            border-left-color: #007bff;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
            display: inline-block;
            vertical-align: middle;
            margin-right: 0.5rem;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @media (max-width: 640px) {
            #game-container {
                padding: 1rem;
                min-height: 400px;
            }
            .btn {
                width: 100%;
                margin: 0.4rem 0;
            }
            .option-btn {
                font-size: 0.9rem;
                padding: 0.6rem 1rem;
            }
            h1 {
                font-size: 1.5rem;
            }
            #question-text {
                font-size: 1.1rem;
            }
            .ai-output-container {
                font-size: 0.85rem;
            }
        }
    </style>
</head>
<body>
    <div id="game-container">
        <h1 class="text-3xl font-bold text-gray-800 mb-6">Eksplorasi Bijak Kepulauan Seribu</h1>

        <!-- Start Screen -->
        <div id="start-screen" class="flex flex-col items-center justify-center h-full w-full">
            <p class="text-lg text-gray-700 mb-8 leading-relaxed">
                Selamat datang di kuis interaktif **Eksplorasi Bijak Kepulauan Seribu**! Mari uji pengetahuan Anda sebagai pemandu wisata dan "island host" yang ramah, informatif, bertanggung jawab, serta peduli kearifan lokal dan keberlanjutan lingkungan. Setiap jawaban yang benar akan mendapatkan poin. Mari berpetualang dan belajar bersama!
            </p>
            <button id="start-btn" class="btn btn-primary text-xl px-8 py-3">Mulai Petualangan!</button>
        </div>

        <!-- Quiz Screen -->
        <div id="quiz-screen" class="hidden w-full h-full flex flex-col justify-between">
            <div class="mb-6 w-full">
                <p id="score-display" class="text-lg font-semibold text-gray-600 mb-2 text-left">Skor: 0</p>
                <p id="question-text" class="text-2xl font-semibold text-gray-800 mb-6 text-left"></p>
                <div id="options-container" class="flex flex-col items-center w-full">
                    <!-- Opsi jawaban akan dimuat di sini oleh JavaScript -->
                </div>
                <div id="feedback-area" class="hidden feedback-message w-full"></div>
                <p id="explanation-text" class="explanation hidden"></p>

                <!-- Gemini AI Feature -->
                <button id="ai-generate-btn" class="btn btn-secondary mt-6 hidden">
                    <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M10.707 2.293a1 1 0 00-1.414 0l-7 7a1 1 0 001.414 1.414L4 10.414V17a1 1 0 001 1h2a1 1 0 001-1v-2.586l.293.293a1 1 0 001.414 0l7-7a1 1 0 000-1.414l-7-7z"></path></svg>
                    Kembangkan Cerita ✨
                </button>
                <div id="ai-output" class="ai-output-container hidden mt-4">
                    <div id="ai-loading" class="hidden text-center text-blue-700">
                        <span class="loading-spinner"></span> Memuat cerita...
                    </div>
                    <p id="ai-text"></p>
                </div>
            </div>
            <div class="flex justify-end mt-4 w-full">
                <button id="next-btn" class="btn btn-primary hidden">Pertanyaan Selanjutnya</button>
            </div>
        </div>

        <!-- End Screen -->
        <div id="end-screen" class="hidden flex flex-col items-center justify-center h-full w-full">
            <h2 class="text-2xl font-bold text-gray-800 mb-4">Permainan Selesai!</h2>
            <p id="final-score" class="text-xl text-gray-700 mb-6"></p>
            <p class="text-lg text-gray-700 mb-8 leading-relaxed">
                Terima kasih telah berpartisipasi dalam **Eksplorasi Bijak Kepulauan Seribu**! Ingat, setiap langkah kecil kita dalam menjaga lingkungan dan berbagi kebaikan akan membuat Kepulauan Seribu semakin indah. Mari terus berpetualang dengan bijak!
            </p>
            <button id="restart-btn" class="btn btn-primary text-xl px-8 py-3">Main Lagi</button>
        </div>
    </div>

    <script>
        // Array berisi pertanyaan, opsi jawaban, jawaban benar, dan penjelasan.
        const questions = [
            {
                question: "Apa salah satu manfaat utama hutan bakau (mangrove) bagi masyarakat pesisir di Kepulauan Seribu?",
                options: [
                    "Sebagai tempat parkir kapal besar",
                    "Penahan abrasi dan gelombang laut",
                    "Sumber bahan bakar kayu",
                    "Tempat pembangunan gedung tinggi"
                ],
                correctAnswer: "Penahan abrasi dan gelombang laut",
                explanation: "Hutan bakau sangat penting sebagai benteng alami yang melindungi pantai dari abrasi, ombak besar, bahkan tsunami, serta menjadi habitat bagi berbagai biota laut."
            },
            {
                question: "Bagaimana sikap yang paling tepat saat seorang pemandu menghadapi pertanyaan dari tamu yang tidak Anda ketahui jawabannya?",
                options: [
                    "Mengarang jawaban agar terlihat pintar",
                    "Mengabaikan pertanyaan tersebut",
                    "Menjawab jujur tidak tahu, lalu mencari tahu atau mengarahkan ke ahli lokal",
                    "Mengganti topik pembicaraan"
                ],
                correctAnswer: "Menjawab jujur tidak tahu, lalu mencari tahu atau mengarahkan ke ahli lokal",
                explanation: "Kejujuran dan inisiatif untuk mencari tahu atau mengarahkan ke sumber yang tepat menunjukkan profesionalisme dan kepercayaan."
            },
            {
                question: "Sebagai 'island host', apa yang sebaiknya Anda lakukan jika melihat tamu membuang sampah sembarangan di pantai?",
                options: [
                    "Membiarkannya saja karena bukan tanggung jawab saya",
                    "Langsung memarahi tamu tersebut",
                    "Menegur dengan sopan dan menawarkan tempat sampah terdekat",
                    "Memungutnya tanpa berkata apa-apa"
                ],
                correctAnswer: "Menegur dengan sopan dan menawarkan tempat sampah terdekat",
                explanation: "Menegur dengan sopan adalah cara terbaik untuk mengedukasi dan mendorong perilaku positif tanpa membuat tamu merasa tidak nyaman."
            },
            {
                question: "Peninggalan sejarah apa yang bisa ditemukan di beberapa pulau di Kepulauan Seribu yang menarik untuk diceritakan kepada tamu?",
                options: [
                    "Situs candi kuno",
                    "Bekas benteng pertahanan Belanda atau mercusuar",
                    "Piramida Mesir",
                    "Gua prasejarah"
                ],
                correctAnswer: "Bekas benteng pertahanan Belanda atau mercusuar",
                explanation: "Kepulauan Seribu memiliki banyak peninggalan sejarah maritim, termasuk benteng-benteng Belanda dan mercusuar kuno yang menyimpan banyak cerita."
            },
            {
                question: "Kearifan lokal masyarakat Kepulauan Seribu sering terkait erat dengan...",
                options: [
                    "Perbankan modern",
                    "Teknologi digital",
                    "Hubungan dengan laut dan perikanan",
                    "Pertanian dataran tinggi"
                ],
                correctAnswer: "Hubungan dengan laut dan perikanan",
                explanation: "Sebagai masyarakat kepulauan, kearifan lokal mereka sangat terikat dengan tradisi, cara hidup, dan mata pencarian yang berhubungan erat dengan laut."
            },
            {
                question: "Apa yang harus ditekankan kepada wisatawan saat mereka melakukan snorkeling atau menyelam di terumbu karang?",
                options: [
                    "Boleh menyentuh dan mengambil karang sebagai suvenir",
                    "Hati-hati jangan menginjak atau merusak karang dan biota laut",
                    "Berlomba melihat siapa yang tercepat berenang",
                    "Membuang botol plastik ke laut setelah selesai"
                ],
                correctAnswer: "Hati-hati jangan menginjak atau merusak karang dan biota laut",
                explanation: "Terumbu karang adalah ekosistem yang sangat rapuh dan vital bagi laut. Penting untuk mengedukasi tamu agar menjaganya."
            },
            {
                question: "Bagaimana cara terbaik untuk memastikan tamu merasa nyaman dan dihargai sejak awal kedatangan mereka?",
                options: [
                    "Langsung memberikan tagihan",
                    "Menyambut dengan senyum, sapaan hangat, dan bantuan yang diperlukan",
                    "Membiarkan mereka mencari tahu sendiri",
                    "Berbicara dengan suara keras"
                ],
                correctAnswer: "Menyambut dengan senyum, sapaan hangat, dan bantuan yang diperlukan",
                explanation: "Sambutan yang ramah dan membantu menciptakan kesan pertama yang positif dan membuat tamu merasa dihargai."
            },
            {
                question: "Ketika memandu tur di area yang memiliki tradisi atau adat istiadat tertentu, apa yang penting untuk pemandu sampaikan kepada tamu?",
                options: [
                    "Tidak perlu menyampaikan apa-apa",
                    "Menjelaskan secara singkat tentang tradisi atau adat dan cara menghormatinya",
                    "Meminta tamu mengikuti semua tradisi tanpa penjelasan",
                    "Menjelaskan secara berlebihan dan menakut-nakuti tamu"
                ],
                correctAnswer: "Menjelaskan secara singkat tentang tradisi atau adat dan cara menghormatinya",
                explanation: "Memberikan informasi yang cukup tentang kearifan lokal akan membantu tamu menghormati budaya setempat dan memperkaya pengalaman mereka."
            },
            {
                question: "Apa kontribusi nyata pemandu lokal dalam menjaga keberlanjutan ekowisata di Kepulauan Seribu?",
                options: [
                    "Hanya fokus pada keuntungan finansial",
                    "Menjadi contoh dalam praktik ramah lingkungan dan mengedukasi tamu tentang pentingnya konservasi",
                    "Mendorong pembangunan gedung besar di area konservasi",
                    "Mengambil biota laut untuk dijual"
                ],
                correctAnswer: "Menjadi contoh dalam praktik ramah lingkungan dan mengedukasi tamu tentang pentingnya konservasi",
                explanation: "Peran pemandu sebagai teladan dan edukator sangat vital untuk menjaga keberlanjutan lingkungan dan memastikan ekowisata terus berkembang secara positif."
            },
            {
                question: "Mengapa penting bagi pemandu untuk mengetahui jalur evakuasi atau titik kumpul darurat di pulau?",
                options: [
                    "Agar bisa berlomba cepat sampai duluan",
                    "Sebagai bahan cerita kepada tamu",
                    "Untuk keselamatan tamu dan diri sendiri dalam situasi darurat",
                    "Tidak ada alasan khusus"
                ],
                correctAnswer: "Untuk keselamatan tamu dan diri sendiri dalam situasi darurat",
                explanation: "Keselamatan adalah prioritas utama. Pemandu yang bertanggung jawab selalu siap menghadapi situasi darurat."
            }
        ];

        let currentQuestionIndex = 0;
        let score = 0;
        let answeredThisQuestion = false;

        // Mendapatkan elemen-elemen dari DOM
        const startScreen = document.getElementById('start-screen');
        const quizScreen = document.getElementById('quiz-screen');
        const endScreen = document.getElementById('end-screen');

        const startBtn = document.getElementById('start-btn');
        const questionText = document.getElementById('question-text');
        const optionsContainer = document.getElementById('options-container');
        const feedbackArea = document.getElementById('feedback-area');
        const explanationText = document.getElementById('explanation-text');
        const nextBtn = document.getElementById('next-btn');
        const scoreDisplay = document.getElementById('score-display');
        const finalScore = document.getElementById('final-score');
        const restartBtn = document.getElementById('restart-btn');

        // Elemen baru untuk fitur Gemini AI
        const aiGenerateBtn = document.getElementById('ai-generate-btn');
        const aiOutput = document.getElementById('ai-output');
        const aiLoading = document.getElementById('ai-loading');
        const aiText = document.getElementById('ai-text');

        // Fungsi untuk memulai permainan
        function startGame() {
            startScreen.classList.add('hidden');
            quizScreen.classList.remove('hidden');
            currentQuestionIndex = 0;
            score = 0;
            scoreDisplay.textContent = `Skor: ${score}`;
            loadQuestion();
        }

        // Fungsi untuk memuat pertanyaan
        function loadQuestion() {
            answeredThisQuestion = false;
            feedbackArea.classList.add('hidden');
            explanationText.classList.add('hidden');
            nextBtn.classList.add('hidden');
            aiGenerateBtn.classList.add('hidden'); // Sembunyikan tombol AI saat pertanyaan baru dimuat
            aiOutput.classList.add('hidden'); // Sembunyikan output AI
            aiText.textContent = ''; // Bersihkan teks AI
            optionsContainer.innerHTML = ''; // Mengosongkan opsi sebelumnya

            if (currentQuestionIndex < questions.length) {
                const q = questions[currentQuestionIndex];
                questionText.textContent = `${currentQuestionIndex + 1}. ${q.question}`;

                q.options.forEach(option => {
                    const button = document.createElement('button');
                    button.textContent = option;
                    button.classList.add('btn', 'btn-secondary', 'option-btn');
                    button.addEventListener('click', () => selectAnswer(button, option, q.correctAnswer, q.explanation));
                    optionsContainer.appendChild(button);
                });
            } else {
                // Semua pertanyaan sudah dijawab, tampilkan layar akhir
                endGame();
            }
        }

        // Fungsi untuk memilih jawaban
        function selectAnswer(selectedButton, selectedOption, correctAnswer, explanation) {
            if (answeredThisQuestion) return; // Mencegah menjawab dua kali
            answeredThisQuestion = true;

            // Nonaktifkan semua tombol opsi setelah dipilih
            Array.from(optionsContainer.children).forEach(button => {
                button.disabled = true;
                if (button.textContent === correctAnswer) {
                    button.classList.add('correct');
                    button.classList.remove('btn-secondary');
                } else {
                    button.classList.add('btn-secondary'); // Revert to default for others
                }
            });

            if (selectedOption === correctAnswer) {
                score += 10; // Tambah skor jika benar
                feedbackArea.textContent = "Benar sekali! ✨";
                feedbackArea.classList.add('feedback-correct');
                feedbackArea.classList.remove('feedback-incorrect', 'hidden');
            } else {
                feedbackArea.textContent = "Kurang tepat. Coba lagi lain kali! 🤔";
                feedbackArea.classList.add('feedback-incorrect');
                feedbackArea.classList.remove('feedback-correct', 'hidden');
                selectedButton.classList.add('incorrect'); // Tandai jawaban yang salah
                selectedButton.classList.remove('btn-secondary'); // Hapus styling default
            }

            scoreDisplay.textContent = `Skor: ${score}`;
            explanationText.textContent = `Penjelasan: ${explanation}`;
            explanationText.classList.remove('hidden');
            nextBtn.classList.remove('hidden'); // Tampilkan tombol 'Next'
            aiGenerateBtn.classList.remove('hidden'); // Tampilkan tombol 'Kembangkan Cerita'
        }

        // Fungsi untuk pindah ke pertanyaan berikutnya
        function nextQuestion() {
            currentQuestionIndex++;
            loadQuestion();
        }

        // Fungsi untuk mengakhiri permainan
        function endGame() {
            quizScreen.classList.add('hidden');
            endScreen.classList.remove('hidden');
            finalScore.textContent = `Skor Akhir Anda: ${score} dari ${questions.length * 10} poin.`;
        }

        // Fungsi untuk me-restart permainan
        function restartGame() {
            endScreen.classList.add('hidden');
            startScreen.classList.remove('hidden'); // Kembali ke layar mulai
            currentQuestionIndex = 0;
            score = 0;
            scoreDisplay.textContent = `Skor: ${score}`;
            feedbackArea.classList.add('hidden');
            explanationText.classList.add('hidden');
            nextBtn.classList.add('hidden');
            aiGenerateBtn.classList.add('hidden'); // Sembunyikan tombol AI saat restart
            aiOutput.classList.add('hidden'); // Sembunyikan output AI
            aiText.textContent = ''; // Bersihkan teks AI
        }

        // Fungsi untuk memanggil Gemini API
        aiGenerateBtn.addEventListener('click', async () => {
            aiOutput.classList.remove('hidden');
            aiLoading.classList.remove('hidden');
            aiText.textContent = ''; // Bersihkan teks sebelumnya

            const currentQ = questions[currentQuestionIndex];
            const prompt = `Sebagai pemandu wisata di Kepulauan Seribu, kembangkan penjelasan singkat berikut tentang "${currentQ.correctAnswer}" dan "${currentQ.explanation}" menjadi satu paragraf deskriptif dan menarik yang bisa disampaikan kepada wisatawan. Fokus pada informasi yang relevan dan cara menyampaikannya agar mudah dipahami dan berkesan.`;

            let chatHistory = [];
            chatHistory.push({ role: "user", parts: [{ text: prompt }] });
            const payload = { contents: chatHistory };
            const apiKey = ""; // Canvas akan otomatis menyediakan API key di runtime
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                aiLoading.classList.add('hidden'); // Sembunyikan loading
                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const text = result.candidates[0].content.parts[0].text;
                    aiText.textContent = text;
                } else {
                    aiText.textContent = "Maaf, tidak dapat mengembangkan cerita saat ini. Silakan coba lagi.";
                }
            } catch (error) {
                aiLoading.classList.add('hidden'); // Sembunyikan loading
                aiText.textContent = "Terjadi kesalahan saat menghubungi AI. Silakan coba lagi. " + error.message;
                console.error("Error calling Gemini API:", error);
            }
        });


        // Event Listeners
        startBtn.addEventListener('click', startGame);
        nextBtn.addEventListener('click', nextQuestion);
        restartBtn.addEventListener('click', restartGame);

        // Inisialisasi: Sembunyikan layar kuis dan akhir saat halaman dimuat
        document.addEventListener('DOMContentLoaded', () => {
            quizScreen.classList.add('hidden');
            endScreen.classList.add('hidden');
        });

    </script>
</body>
</html>
