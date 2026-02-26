<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Khu Vườn Thú Cưng Lung Linh 2.0</title>
    <style>
        :root {
            --bg-color: #f0faff;
            --card-color: #ffffff;
            --accent-color: #ffdae9;
            --pet-color-main: #b5e48c; /* Màu mặc định */
            --text-color: #5a5a5a;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            overflow: hidden;
        }

        #game-container {
            background: var(--card-color);
            padding: 40px;
            border-radius: 40px;
            box-shadow: 0 15px 40px rgba(0,0,0,0.05);
            text-align: center;
            width: 450px;
            position: relative;
            border: 8px solid var(--accent-color);
        }

        h1 { font-size: 1.6rem; margin-bottom: 25px; color: #ff9aaf; }

        /* Màn hình chọn thú */
        .selection-group { display: flex; justify-content: space-around; margin-top: 30px; }
        .pet-card {
            cursor: pointer;
            transition: transform 0.3s, box-shadow 0.3s;
            padding: 15px;
            border-radius: 20px;
            border: 2px solid transparent;
        }
        .pet-card:hover { transform: scale(1.1); background: #fff0f5; box-shadow: 0 5px 15px rgba(0,0,0,0.05); }
        .pet-card small { display: block; margin-top: 10px; font-weight: bold; color: #888; }

        /* Khu vực chính */
        #main-game { display: none; }
        
        /* Container chứa hình ảnh con vật */
        #pet-stage {
            width: 200px;
            height: 200px;
            margin: 20px auto;
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
        }

        /* --- ĐỒ HỌA SVG (Nâng cấp) --- */
        .pet-svg { width: 100%; height: 100%; transition: all 0.5s; }
        
        /* Hiệu ứng chớp mắt (Dùng chung) */
        @keyframes blink {
            0%, 90%, 100% { transform: scaleY(1); }
            95% { transform: scaleY(0.1); }
        }
        .pet-eye { animation: blink 4s infinite; transform-origin: center; }

        /* Hiệu ứng vẫy đuôi/cánh */
        @keyframes wag { from { transform: rotate(-5deg); } to { transform: rotate(5deg); } }
        .pet-tail { animation: wag 0.6s infinite alternate; transform-origin: top; }
        .pet-wing { animation: wag 0.3s infinite alternate; transform-origin: center; }

        /* Nút hành động */
        .action-btn {
            font-size: 45px;
            background: none;
            border: none;
            cursor: pointer;
            margin: 15px;
            padding: 20px;
            border-radius: 50%;
            background: #fdfdfd;
            box-shadow: 0 6px 15px rgba(0,0,0,0.08);
            transition: 0.3s;
            position: relative;
        }
        .action-btn:hover:not(:disabled) { transform: translateY(-5px) scale(1.05); }
        .action-btn:disabled { opacity: 0.2; cursor: not-allowed; }
        .hidden { display: none !important; }

        /* Hiệu ứng lung linh ✨ */
        .sparkle {
            position: absolute;
            color: #ffd700;
            pointer-events: none;
            animation: fly 1.2s forwards;
            font-size: 20px;
        }
        @keyframes fly {
            0% { transform: translate(0, 0) scale(1) rotate(0deg); opacity: 1; }
            100% { transform: translate(var(--dx), var(--dy)) scale(1.5) rotate(360deg); opacity: 0; }
        }

        /* Hiệu ứng khi ăn */
        .bounce { animation: bounce 0.4s ease; }
        @keyframes bounce { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.1) translateY(-10px); } }

        /* Hiệu ứng lung linh khi được chải lông */
        .glow { filter: drop-shadow(0 0 15px #ffcfdf) drop-shadow(0 0 5px #ffd700); }

    </style>
</head>
<body>

<div id="game-container">
    <div id="selection-screen">
        <h1>An ơi, chọn một người bạn nhé!</h1>
        <div class="selection-group">
            <div class="pet-card" onclick="startGame('dog')">
                <svg class="pet-svg" viewBox="0 0 100 100">
                    <circle cx="50" cy="50" r="40" fill="#Ffdca9"/> <ellipse cx="50" cy="65" rx="15" ry="10" fill="#fff" opacity="0.5"/> <circle class="pet-eye" cx="35" cy="45" r="4" fill="#333"/> <circle class="pet-eye" cx="65" cy="45" r="4" fill="#333"/> <rect class="pet-tail" x="46" y="80" width="8" height="20" rx="4" fill="#Ffdca9"/> <ellipse cx="15" cy="40" rx="8" ry="15" fill="#e8c08a" transform="rotate(-20 15 40)"/> <ellipse cx="85" cy="40" rx="8" ry="15" fill="#e8c08a" transform="rotate(20 85 40)"/> </svg>
                <small>Cún Con</small>
            </div>
            <div class="pet-card" onclick="startGame('cat')">
                <svg class="pet-svg" viewBox="0 0 100 100">
                    <path d="M50 15 L15 85 L85 85 Z" fill="#e0e0e0" stroke="#ccc" stroke-width="2"/> <circle cx="50" cy="45" r="35" fill="#f3f3f3"/> <circle class="pet-eye" cx="35" cy="40" r="4" fill="#333"/>
                    <circle class="pet-eye" cx="65" cy="40" r="4" fill="#333"/>
                    <polygon points="50,55 45,50 55,50" fill="#ffb6c1"/> <path d="M20 15 L35 30 L25 40 Z" fill="#f3f3f3"/> <path d="M80 15 L65 30 L75 40 Z" fill="#f3f3f3"/> <path class="pet-tail" d="M80 80 Q 95 70, 90 50" stroke="#ccc" stroke-width="6" fill="none" stroke-linecap="round"/>
                </svg>
                <small>Mèo Mây</small>
            </div>
            <div class="pet-card" onclick="startGame('bird')">
                <svg class="pet-svg" viewBox="0 0 100 100">
                    <circle cx="50" cy="50" r="35" fill="#b5e48c"/> <circle class="pet-eye" cx="65" cy="40" r="4" fill="#333"/> <polygon points="80,45 95,50 80,55" fill="#ffb703"/> <ellipse class="pet-wing" cx="30" cy="55" rx="20" ry="10" fill="#99d98c" transform="rotate(-10 30 55)"/> <rect x="40" y="80" width="5" height="15" fill="#a67c52"/> <rect x="55" y="80" width="5" height="15" fill="#a67c52"/> </svg>
                <small>Vẹt Bông</small>
            </div>
        </div>
    </div>

    <div id="main-game">
        <h1 id="status-text">Cho bạn ấy ăn nhé!</h1>
        <div id="pet-stage">
            </div>
        
        <div class="actions">
            <button id="feed-btn" class="action-btn" onclick="feedPet()">🥣</button>
            <button id="groom-btn" class="action-btn hidden" onclick="groomPet()">🪮</button>
        </div>
        
        <p id="message" style="color: #bbb; font-style: italic; font-size: 14px;"></p>
    </div>
</div>

<script>
    let clickCount = 0;
    let currentPetSvg = null;
    const stage = document.getElementById('pet-stage');
    const statusText = document.getElementById('status-text');
    const message = document.getElementById('message');

    // Bắt đầu game: Lấy SVG của con vật được chọn bỏ vào sân khấu chính
    function startGame(petType) {
        document.getElementById('selection-screen').classList.add('hidden');
        document.getElementById('main-game').style.display = 'block';
        
        // Lấy nội dung SVG từ thẻ .pet-card tương ứng
        const selectedCard = document.querySelector(`.pet-card[onclick="startGame('${petType}')"]`);
        const svgContent = selectedCard.querySelector('.pet-svg').outerHTML;
        
        stage.innerHTML = svgContent;
        currentPetSvg = stage.querySelector('.pet-svg'); // Lưu tham chiếu đến SVG mới
        
        message.innerText = "Bạn ấy đang đợi An đấy...";
    }

    // Tạo hiệu ứng lấp lánh tung tóe ✨
    function createSparkles(e) {
        const rect = e.target.getBoundingClientRect();
        const centerX = rect.left + rect.width / 2;
        const centerY = rect.top + rect.height / 2;

        for (let i = 0; i < 8; i++) {
            const span = document.createElement('span');
            span.classList.add('sparkle');
            span.innerHTML = '✨';
            
            // Tạo hướng bay ngẫu nhiên
            const dx = (Math.random() - 0.5) * 150 + 'px';
            const dy = (Math.random() - 0.5) * 150 + 'px';
            span.style.setProperty('--dx', dx);
            span.style.setProperty('--dy', dy);
            
            span.style.left = centerX + 'px';
            span.style.top = centerY + 'px';
            
            document.body.appendChild(span);
            setTimeout(() => span.remove(), 1200);
        }
    }

    function feedPet(e) {
        clickCount++;
        createSparkles(event);
        
        // Hiệu ứng thú cưng nhảy lên vui vẻ khi ăn
        currentPetSvg.classList.add('bounce');
        setTimeout(() => currentPetSvg.classList.remove('bounce'), 400);

        if (clickCount >= 3) {
            statusText.innerText = "Hết trơn rồi! Chải lông cho đẹp nào!";
            message.innerText = "Bụng no căng rồi kìa, thích quá!";
            document.getElementById('feed-btn').classList.add('hidden');
            document.getElementById('groom-btn').classList.remove('hidden');
            clickCount = 0; 
        } else {
            message.innerText = `Ăn thêm ${3 - clickCount} miếng nữa nhé...`;
        }
    }

    function groomPet(e) {
        clickCount++;
        createSparkles(event);
        
        // Hiệu ứng thú cưng tỏa sáng lung linh
        currentPetSvg.classList.add('glow');
        
        if (clickCount >= 3) {
            statusText.innerText = "Ôi, xinh đẹp tuyệt vời!";
            message.innerText = "Thú cưng của An là nhất luôn!";
            document.getElementById('groom-btn').disabled = true;
            
            // Thêm hiệu ứng trái tim cuối cùng
            message.innerHTML += " 💖💖💖";
        } else {
            message.innerText = "Chải chải... sắp đẹp xong rồi...";
        }
    }
</script>

</body>
</html>
