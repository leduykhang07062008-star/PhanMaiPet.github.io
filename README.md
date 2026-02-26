# PhanMaiPet.github.io
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Khu Vườn Thú Cưng Lung Linh</title>
    <style>
        :root {
            --bg-color: #f0faff;
            --card-color: #ffffff;
            --accent-color: #ffcfdf;
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
            padding: 30px;
            border-radius: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.05);
            text-align: center;
            width: 400px;
            position: relative;
            border: 5px solid var(--accent-color);
        }

        h1 { font-size: 1.5rem; margin-bottom: 20px; color: #ff9aaf; }

        /* Màn hình chọn thú */
        .selection-group {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
        }

        .pet-option {
            font-size: 50px;
            cursor: pointer;
            transition: transform 0.3s;
            padding: 10px;
            border-radius: 20px;
        }

        .pet-option:hover { transform: scale(1.2); background: #fff0f5; }

        /* Khu vực chính */
        #main-game { display: none; }

        #pet-display {
            font-size: 100px;
            margin: 20px 0;
            display: block;
            transition: all 0.5s;
        }

        .action-btn {
            font-size: 40px;
            background: none;
            border: none;
            cursor: pointer;
            margin: 10px;
            padding: 15px;
            border-radius: 50%;
            background: #fdfdfd;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            transition: 0.3s;
        }

        .action-btn:hover:not(:disabled) { transform: translateY(-5px); box-shadow: 0 6px 15px rgba(0,0,0,0.15); }
        .action-btn:disabled { opacity: 0.3; cursor: not-allowed; filter: grayscale(1); }

        .hidden { display: none; }

        /* Hiệu ứng lung linh */
        .sparkle {
            position: absolute;
            color: #ffd700;
            pointer-events: none;
            animation: fly 1s forwards;
        }

        @keyframes fly {
            0% { transform: translateY(0) scale(1); opacity: 1; }
            100% { transform: translateY(-50px) scale(1.5); opacity: 0; }
        }

        .bounce { animation: bounce 0.5s infinite alternate; }
        @keyframes bounce { from { transform: translateY(0); } to { transform: translateY(-10px); } }
    </style>
</head>
<body>

<div id="game-container">
    <div id="selection-screen">
        <h1>Chào An, chọn bạn mới nhé!</h1>
        <div class="selection-group">
            <div class="pet-option" onclick="startGame('🐶')">🐶<br><small style="font-size: 14px">Cún</small></div>
            <div class="pet-option" onclick="startGame('🐱')">🐱<br><small style="font-size: 14px">Mèo</small></div>
            <div class="pet-option" onclick="startGame('🦜')">🦜<br><small style="font-size: 14px">Vẹt</small></div>
        </div>
    </div>

    <div id="main-game">
        <h1 id="status-text">Cho bạn ấy ăn nhé!</h1>
        <div id="pet-display"></div>
        
        <div class="actions">
            <button id="feed-btn" class="action-btn" onclick="feedPet()">🥣</button>
            <button id="groom-btn" class="action-btn hidden" onclick="groomPet()">🪮</button>
        </div>
        
        <p id="message" style="color: #999; font-style: italic;"></p>
    </div>
</div>

<script>
    let clickCount = 0;
    const petDisplay = document.getElementById('pet-display');
    const statusText = document.getElementById('status-text');
    const message = document.getElementById('message');

    function startGame(petIcon) {
        document.getElementById('selection-screen').classList.add('hidden');
        document.getElementById('main-game').style.display = 'block';
        petDisplay.innerText = petIcon;
        message.innerText = "Bạn ấy đang đói bụng...";
    }

    function createSparkle(x, y) {
        const span = document.createElement('span');
        span.classList.add('sparkle');
        span.innerHTML = '✨';
        span.style.left = x + 'px';
        span.style.top = y + 'px';
        document.body.appendChild(span);
        setTimeout(() => span.remove(), 1000);
    }

    function feedPet() {
        clickCount++;
        createSparkle(event.pageX, event.pageY);
        
        // Hiệu ứng thú cưng vui vẻ khi ăn
        petDisplay.classList.add('bounce');
        setTimeout(() => petDisplay.classList.remove('bounce'), 500);

        if (clickCount >= 3) {
            statusText.innerText = "Bạn ấy no rồi! Chải lông nào!";
            message.innerText = "Wow, bạn ấy trông rất hài lòng!";
            document.getElementById('feed-btn').classList.add('hidden');
            document.getElementById('groom-btn').classList.remove('hidden');
            clickCount = 0; // Reset để dùng cho việc chải lông
        }
    }

    function groomPet() {
        clickCount++;
        createSparkle(event.pageX, event.pageY);
        
        // Hiệu ứng thú cưng lung linh khi được chải lông
        petDisplay.style.filter = "drop-shadow(0 0 15px #ffcfdf)";
        
        if (clickCount >= 3) {
            statusText.innerText = "Thật là một ngày tuyệt vời!";
            message.innerText = "Bạn thú cưng của bạn đã xinh đẹp và sạch sẽ!";
            document.getElementById('groom-btn').disabled = true;
            petDisplay.innerHTML += "💖";
        }
    }
</script>

</body>
</html>
