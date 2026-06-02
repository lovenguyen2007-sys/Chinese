<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Học Tiếng Trung Mỗi Ngày</title>
    <style>
        :root {
            --primary-color: #de2910; /* Màu đỏ cờ Trung Quốc */
            --secondary-color: #ffde00; /* Màu vàng */
            --text-color: #333;
            --bg-color: #f7f9fa;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            line-height: 1.6;
        }

        header {
            background-color: var(--primary-color);
            color: white;
            text-align: center;
            padding: 2rem 1rem;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        header h1 {
            font-size: 2.5rem;
            margin-bottom: 0.5rem;
        }

        header p {
            color: var(--secondary-color);
            font-weight: bold;
        }

        nav {
            display: flex;
            justify-content: center;
            background: #fff;
            padding: 1rem;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
            margin-bottom: 2rem;
        }

        nav button {
            background: none;
            border: 2px solid transparent;
            font-size: 1.1rem;
            font-weight: 600;
            padding: 0.5rem 1.5rem;
            margin: 0 0.5rem;
            cursor: pointer;
            transition: all 0.3s ease;
            border-radius: 5px;
        }

        nav button.active, nav button:hover {
            border-color: var(--primary-color);
            color: var(--primary-color);
        }

        .container {
            max-width: 900px;
            margin: 0 auto;
            padding: 0 1rem;
        }

        .lesson-section {
            display: none;
            background: white;
            padding: 2rem;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
            animation: fadeIn 0.5s ease;
        }

        .lesson-section.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        h2 {
            margin-bottom: 1.5rem;
            color: var(--primary-color);
            border-bottom: 2px solid #eee;
            padding-bottom: 0.5rem;
        }

        .word-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 1.5rem;
        }

        .word-card {
            background: #fff;
            border: 1px solid #e1e8ed;
            border-radius: 8px;
            padding: 1.5rem;
            text-align: center;
            transition: transform 0.2s, box-shadow 0.2s;
            position: relative;
        }

        .word-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 6px 15px rgba(0,0,0,0.1);
        }

        .chinese {
            font-size: 2.5rem;
            color: var(--primary-color);
            margin-bottom: 0.5rem;
            font-weight: bold;
        }

        .pinyin {
            font-size: 1.1rem;
            color: #777;
            font-style: italic;
            margin-bottom: 0.5rem;
        }

        .meaning {
            font-size: 1.2rem;
            font-weight: 500;
            color: var(--text-color);
            margin-bottom: 1rem;
        }

        .btn-audio {
            background-color: var(--secondary-color);
            border: none;
            color: #333;
            padding: 0.4rem 1rem;
            font-size: 0.9rem;
            font-weight: bold;
            border-radius: 20px;
            cursor: pointer;
            transition: background 0.2s;
        }

        .btn-audio:hover {
            background-color: #ffd000;
        }

        footer {
            text-align: center;
            padding: 2rem;
            margin-top: 3rem;
            color: #888;
            font-size: 0.9rem;
        }
    </style>
</head>
<body>

    <header>
        <h1>Xin Chào! Hán Ngữ Mỗi Ngày</h1>
        <p>Học tiếng Trung giao tiếp từ cơ bản đến nâng cao</p>
    </header>

    <nav>
        <button class="nav-btn active" onclick="switchLesson('lesson1')">Bài 1: Chào hỏi</button>
        <button class="nav-btn" onclick="switchLesson('lesson2')">Bài 2: Số đếm</button>
        <button class="nav-btn" onclick="switchLesson('lesson3')">Bài 3: Gia đình</button>
    </nav>

    <div class="container">
        <section id="lesson1" class="lesson-section active">
            <h2>Bài 1: Chào hỏi cơ bản</h2>
            <div class="word-grid" id="grid-l1"></div>
        </section>

        <section id="lesson2" class="lesson-section">
            <h2>Bài 2: Số đếm (1 - 10)</h2>
            <div class="word-grid" id="grid-l2"></div>
        </section>

        <section id="lesson3" class="lesson-section">
            <h2>Bài 3: Thành viên Gia đình</h2>
            <div class="word-grid" id="grid-l3"></div>
        </section>
    </div>

    <footer>
        <p>© 2026 Trang Web Học Tiếng Trung. Phát triển phục vụ mục đích học tập.</p>
    </footer>

    <script>
        // Dữ liệu các bài học
        const lessonsData = {
            lesson1: [
                { hz: "你好", py: "nǐ hǎo", vn: "Xin chào" },
                { hz: "谢谢", py: "xièxie", vn: "Cảm ơn" },
                { hz: "再见", py: "zàijiàn", vn: "Tạm biệt" },
                { hz: "对不起", py: "duìbuqǐ", vn: "Xin lỗi" },
                { hz: "没关系", py: "méi guānxi", vn: "Không có gì/Không sao" }
            ],
            lesson2: [
                { hz: "一", py: "yī", vn: "Số 1" },
                { hz: "二", py: "èr", vn: "Số 2" },
                { hz: "三", py: "sān", vn: "Số 3" },
                { hz: "五", py: "wǔ", vn: "Số 5" },
                { hz: "十", py: "shí", vn: "Số 10" }
            ],
            lesson3: [
                { hz: "爸爸", py: "bàba", vn: "Bố" },
                { hz: "妈妈", py: "māma", vn: "Mẹ" },
                { hz: "哥哥", py: "gēge", vn: "Anh trai" },
                { hz: "姐姐", py: "jiějie", vn: "Chị gái" },
                { hz: "弟弟", py: "dìdi", vn: "Em trai" }
            ]
        };

        // Hàm hiển thị dữ liệu lên giao diện
        function renderLessons() {
            for (let lessonId in lessonsData) {
                const grid = document.getElementById(`grid-${lessonId.replace('lesson', 'l')}`);
                if (!grid) continue;
                
                lessonsData[lessonId].forEach(word => {
                    const card = document.createElement('div');
                    card.className = 'word-card';
                    card.innerHTML = `
                        <div class="chinese">${word.hz}</div>
                        <div class="pinyin">${word.py}</div>
                        <div class="meaning">${word.vn}</div>
                        <button class="btn-audio" onclick="speak('${word.hz}')">🔊 Nghe</button>
                    `;
                    grid.appendChild(card);
                });
            }
        }

        // Hàm chuyển đổi qua lại giữa các bài học
        function switchLesson(lessonId) {
            // Ẩn tất cả các section
            document.querySelectorAll('.lesson-section').forEach(sec => sec.classList.remove('active'));
            // Hiện section được chọn
            document.getElementById(lessonId).classList.add('active');

            // Cập nhật trạng thái nút bấm active ở thanh menu
            document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
        }

        // Hàm phát âm Text-to-Speech bằng tiếng Trung chuẩn (zh-CN)
        function speak(text) {
            if ('speechSynthesis' in window) {
                const utterance = new SpeechSynthesisUtterance(text);
                utterance.lang = 'zh-CN'; // Cấu hình giọng đọc tiếng Trung phổ thông
                utterance.rate = 0.8; // Tốc độ đọc hơi chậm một chút để dễ nghe
                window.speechSynthesis.cancel(); // Dừng âm thanh đang phát trước đó (nếu có)
                window.speechSynthesis.speak(utterance);
            } else {
                alert("Trình duyệt của bạn không hỗ trợ tính năng phát âm.");
            }
        }

        // Khởi chạy khi trang web tải xong
        window.onload = renderLessons;
    </script>
</body>
</html>
