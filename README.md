<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Học Tiếng Trung - Luyện Phát Âm</title>
    <style>
        :root {
            --primary-color: #e74c3c;
            --secondary-color: #2ecc71;
            --bg-color: #f9f9f9;
            --text-color: #2c3e50;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            background: white;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            max-width: 500px;
            width: 100%;
            text-align: center;
        }

        h1 {
            color: var(--primary-color);
            margin-bottom: 30px;
        }

        .word-card {
            background: #fff8f7;
            border: 2px solid #ffbcbc;
            border-radius: 8px;
            padding: 20px;
            margin-bottom: 20px;
        }

        .chinese-text {
            font-size: 48px;
            font-weight: bold;
            margin: 0;
            color: #333;
        }

        .pinyin {
            font-size: 20px;
            color: #7f8c8d;
            margin: 5px 0 10px 0;
        }

        .meaning {
            font-size: 16px;
            color: #555;
            font-style: italic;
        }

        .btn {
            background-color: var(--primary-color);
            color: white;
            border: none;
            padding: 12px 24px;
            font-size: 16px;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 5px;
        }

        .btn:hover {
            opacity: 0.9;
            transform: scale(1.05);
        }

        .btn-listen {
            background-color: #3498db;
        }

        .btn-speak {
            background-color: #e67e22;
        }

        .btn-speak.recording {
            background-color: #c0392b;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .result-box {
            margin-top: 25px;
            padding: 15px;
            border-radius: 6px;
            font-weight: bold;
            display: none;
        }

        .success {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .fail {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .user-transcript {
            font-size: 14px;
            color: #666;
            margin-top: 5px;
            font-weight: normal;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>汉语口语练习</h1>
    
    <div class="word-card">
        <p class="chinese-text" id="chineseWord">你好</p>
        <p class="pinyin" id="pinyinText">nǐ hǎo</p>
        <p class="meaning" id="meaningText">Xin chào</p>
    </div>

    <div>
        <button class="btn btn-listen" id="btnListen">🔊 Nghe mẫu</button>
        <button class="btn btn-speak" id="btnSpeak">🎤 Nhấn để nói</button>
        <button class="btn" id="btnNext" style="background-color: #2ecc71;">Tiếp theo ➔</button>
    </div>

    <div class="result-box" id="resultBox">
        <span id="resultMessage"></span>
        <div class="user-transcript" id="userTranscript"></div>
    </div>
</div>

<script>
    // Dữ liệu câu hỏi mẫu
    const wordList = [
        { word: "你好", pinyin: "nǐ hǎo", meaning: "Xin chào" },
        { word: "谢谢", pinyin: "xièxie", meaning: "Cảm ơn" },
        { word: "再见", pinyin: "zàijiàn", meaning: "Tạm biệt" },
        { word: "我爱你", pinyin: "wǒ ài nǐ", meaning: "Tôi yêu bạn" },
        { word: "你想吃什么？", pinyin: "nǐ xiǎng chī shénme?", meaning: "Bạn muốn ăn gì?" }
    ];

    let currentIndex = 0;

    // DOM Elements
    const chineseWord = document.getElementById('chineseWord');
    const pinyinText = document.getElementById('pinyinText');
    const meaningText = document.getElementById('meaningText');
    const btnListen = document.getElementById('btnListen');
    const btnSpeak = document.getElementById('btnSpeak');
    const btnNext = document.getElementById('btnNext');
    const resultBox = document.getElementById('resultBox');
    const resultMessage = document.getElementById('resultMessage');
    const userTranscript = document.getElementById('userTranscript');

    // Cập nhật giao diện từ vựng
    function loadWord(index) {
        chineseWord.textContent = wordList[index].word;
        pinyinText.textContent = wordList[index].pinyin;
        meaningText.textContent = wordList[index].meaning;
        resultBox.style.display = 'none';
    }

    // 1. Tính năng Phát âm mẫu (Text-to-Speech)
    btnListen.addEventListener('click', () => {
        const utterance = new SpeechSynthesisUtterance(wordList[currentIndex].word);
        utterance.lang = 'zh-CN'; // Đặt ngôn ngữ là tiếng Trung Phổ Thông
        window.speechSynthesis.speak(utterance);
    });

    // 2. Tính năng Nhận diện giọng nói (Speech-to-Text)
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    
    if (!SpeechRecognition) {
        alert("Trình duyệt của bạn không hỗ trợ nhận diện giọng nói. Vui lòng dùng Google Chrome.");
    } else {
        const recognition = new SpeechRecognition();
        recognition.lang = 'zh-CN'; // Nhận diện tiếng Trung
        recognition.interimResults = false;
        recognition.maxAlternatives = 1;

        btnSpeak.addEventListener('click', () => {
            recognition.start();
            btnSpeak.classList.add('recording');
            btnSpeak.textContent = "🎙️ Đang lắng nghe...";
        });

        recognition.onresult = (event) => {
            const speechResult = event.results[0][0].transcript;
            const targetWord = wordList[currentIndex].word;

            // Xử lý chuẩn hóa chuỗi để so sánh (xóa dấu câu cơ bản)
            const cleanSpeech = speechResult.replace(/[.,\/#!$%\^&\*;:{}=\-_`~()？。，]/g,"");
            const cleanTarget = targetWord.replace(/[.,\/#!$%\^&\*;:{}=\-_`~()？。，]/g,"");

            resultBox.style.display = 'block';
            userTranscript.textContent = `Bạn đã nói: "${speechResult}"`;

            if (cleanSpeech === cleanTarget) {
                resultBox.className = "result-box success";
                resultMessage.textContent = "🎉 Tuyệt vời! Phát âm chính xác.";
            } else {
                resultBox.className = "result-box fail";
                resultMessage.textContent = "❌ Chưa chính xác. Hãy thử lại!";
            }
        };

        recognition.onspeechend = () => {
            recognition.stop();
            btnSpeak.classList.remove('recording');
            btnSpeak.textContent = "🎤 Nhấn để nói";
        };

        recognition.onerror = (event) => {
            btnSpeak.classList.remove('recording');
            btnSpeak.textContent = "🎤 Nhấn để nói";
            alert("Đã xảy ra lỗi nhận diện: " + event.error);
        };
    }

    // Chuyển câu tiếp theo
    btnNext.addEventListener('click', () => {
        currentIndex = (currentIndex + 1) % wordList.length;
        loadWord(currentIndex);
    });

    // Khởi tạo lần đầu
    loadWord(currentIndex);
</script>

</body>
</html>
