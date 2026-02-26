<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pet Love - Thế Giới Lung Linh</title>
    <style>
        :root {
            --pink-light: #ffebf2;
            --gold: #ffd700;
        }

        body, html {
            margin: 0; padding: 0; width: 100%; height: 100%;
            display: flex; justify-content: center; align-items: center;
            background: #fdf0d5; font-family: 'Segoe UI', sans-serif;
            overflow: hidden; touch-action: none;
        }

        /* Background: Căn phòng chi tiết */
        #room {
            width: 100vw; height: 100vh;
            background: linear-gradient(180deg, #a2d2ff 0%, #ffffff 100%);
            position: relative; display: flex; flex-direction: column; align-items: center;
        }

        /* Chi tiết căn phòng */
        .wall-decor {
            position: absolute; top: 10%; width: 100%; height: 200px;
            background-image: radial-gradient(#ffafcc 1px, transparent 1px);
            background-size: 30px 30px; opacity: 0.3;
        }

        /* Chiếc nệm êm ái */
        .pet-bed {
            position: absolute; bottom: 15%;
            width: 350px; height: 120px;
            background: #ff8fab;
            border-radius: 50%;
            border: 8px solid #fb6f92;
            box-shadow: 0 15px 0 #ef476f, 0 20px 40px rgba(0,0,0,0.1);
            display: flex; justify-content: center; align-items: center;
        }
        .pet-bed::after {
            content: ''; position: absolute; top: 10px; width: 90%; height: 80%;
            background: rgba(255,255,255,0.2); border-radius: 50%;
        }

        /* Lớp phủ UI */
        #ui-overlay {
            position: absolute; z-index: 100; inset: 0;
            background: rgba(255,255,255,0.85); backdrop-filter: blur(15px);
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            transition: 0.8s ease-in-out;
        }

        .pet-card {
            cursor: pointer; transition: 0.3s;
            filter: drop-shadow(0 5px 15px rgba(0,0,0,0.1));
        }
        .pet-card:hover { transform: scale(1.1); }

        /* Đồ họa thú cưng tinh xảo (SVG) */
        .pet-box {
            position: absolute; bottom: 40px; width: 220px; height: 220px;
            z-index: 50; transition: transform 0.5s;
        }

        /* Thức ăn & Lược */
        .item {
            width: 100px; height: 100px;
            position: absolute; bottom: 5%; cursor: grab;
            filter: drop-shadow(0 8px 15px rgba(0,0,0,0.2));
            z-index: 200; touch-action: none;
        }

        /* Hiệu ứng lung linh ✨ */
        .sparkle {
            position: absolute; pointer-events: none;
            animation: fly 0.8s ease-out forwards;
            font-size: 24px; z-index: 300;
        }
        @keyframes fly {
            0% { transform: scale(0); opacity: 1; }
            100% { transform: translate(var(--x), var(--y)) scale(1.5); opacity: 0; }
        }

        input {
            padding: 15px; border-radius: 30px; border: 3px solid #ff8fab;
            width: 250px; text-align: center; font-size: 1.2rem; margin: 20px;
        }
        .btn {
            background: #fb6f92; color: white; padding: 15px 40px;
            border-radius: 30px; border: none; font-weight: bold; cursor: pointer;
        }

        .hidden { display: none !important; }
    </style>
</head>
<body>

<div id="room">
    <div class="wall-decor"></div>
    
    <div id="ui-overlay">
        <h1 style="color: #fb6f92;">Chọn bạn nhỏ của bạn</h1>
        <div style="display: flex; gap: 30px; margin-bottom: 30px;">
            <div class="pet-card" onclick="preSelect('dog')">🐶<br><small>Cún Con</small></div>
            <div class="pet-card" onclick="preSelect('cat')">🐱<br><small>Mèo Mây</small></div>
            <div class="pet-card" onclick="preSelect('bird')">🦜<br><small>Vẹt Xanh</small></div>
        </div>
        <div id="name-box" class="hidden">
            <input type="text" id="input-name" placeholder="Đặt tên cho bạn ấy...">
            <br>
            <button class="btn" onclick="startGame()">Vào Phòng Thôi!</button>
        </div>
    </div>

    <div class="pet-bed">
        <div id="pet-container" class="pet-box"></div>
    </div>

    <div id="item-food" class="item hidden"></div>
    <div id="item-comb" class="item hidden">🪮</div>
    
    <div id="pet-label" style="position: absolute; top: 10%; font-size: 2rem; color: #fb6f92; font-weight: bold; text-shadow: 2px 2px white;"></div>
</div>

<script>
    let selectedPet = '';
    let isDragging = false;
    let dragItem = null;
    let swipeCount = 0;
    let lastX = 0;
    let feedProgress = 0;

    const items = {
        dog: { icon: '🥣', food: '🍖', name: 'Xương Ngon' },
        cat: { icon: '🥣', food: '🐟', name: 'Cá Ngừ' },
        bird: { icon: '🍉', food: '🍉', name: 'Hạt Dưa' }
    };

    function preSelect(type) {
        selectedPet = type;
        document.getElementById('name-box').classList.remove('hidden');
    }

    function startGame() {
        const name = document.getElementById('input-name').value || "Bé cưng";
        document.getElementById('pet-label').innerText = name;
        document.getElementById('ui-overlay').style.transform = 'translateY(-100%)';
        
        renderPet(selectedPet);
        setupFood();
    }

    function renderPet(type) {
        const container = document.getElementById('pet-container');
        let svg = '';
        if(type === 'dog') {
            svg = `<svg viewBox="0 0 100 100"><circle cx="50" cy="50" r="45" fill="#fec89a"/><circle cx="30" cy="40" r="5" fill="#333"/><circle cx="70" cy="40" r="5" fill="#333"/><path d="M40 70 Q 50 80 60 70" fill="none" stroke="#fb6f92" stroke-width="4"/></svg>`;
        } else if(type === 'cat') {
            svg = `<svg viewBox="0 0 100 100"><circle cx="50" cy="50" r="45" fill="#e5e5e5"/><circle cx="35" cy="45" r="4" fill="#333"/><circle cx="65" cy="45" r="4" fill="#333"/><path d="M45 60 Q 50 65 55 60" fill="none" stroke="#333" stroke-width="2"/></svg>`;
        } else {
            svg = `<svg viewBox="0 0 100 100"><circle cx="50" cy="50" r="45" fill="#80ed99"/><path d="M70 45 L90 50 L70 55" fill="#ffb703"/><circle cx="60" cy="40" r="4" fill="#333"/></svg>`;
        }
        container.innerHTML = svg;
    }

    function setupFood() {
        const foodItem = document.getElementById('item-food');
        foodItem.innerHTML = items[selectedPet].food;
        foodItem.classList.remove('hidden');
        foodItem.style.left = '10%';
        makeDraggable(foodItem);
    }

    function makeDraggable(el) {
        el.onmousedown = start;
        el.ontouchstart = start;

        function start(e) {
            isDragging = true;
            dragItem = el;
            el.style.transition = 'none';
        }
    }

    window.onmousemove = move;
    window.ontouchmove = move;
    window.onmouseup = end;
    window.ontouchend = end;

    function move(e) {
        if (!isDragging || !dragItem) return;
        const x = e.clientX || e.touches[0].clientX;
        const y = e.clientY || e.touches[0].clientY;
        
        dragItem.style.left = (x - 50) + 'px';
        dragItem.style.top = (y - 50) + 'px';

        const pet = document.getElementById('pet-container').getBoundingClientRect();
        if (x > pet.left && x < pet.right && y > pet.top && y < pet.bottom) {
            createSparkle(x, y);
            if (dragItem.id === 'item-food') feedProgress++;
            if (dragItem.id === 'item-comb') {
                if (Math.abs(x - lastX) > 30) {
                    swipeCount++;
                    lastX = x;
                }
            }
        }
    }

    function end() {
        if (!isDragging) return;
        isDragging = false;
        
        if (feedProgress > 50) {
            document.getElementById('item-food').classList.add('hidden');
            const comb = document.getElementById('item-comb');
            comb.classList.remove('hidden');
            comb.style.right = '10%';
            comb.style.bottom = '5%';
            makeDraggable(comb);
            feedProgress = 0;
        }

        if (swipeCount >= 10) { // 5 lần qua lại = 10 lần đổi hướng
            document.getElementById('pet-label').innerText += " ✨ Lung Linh!";
            document.getElementById('pet-container').style.filter = "drop-shadow(0 0 20px gold)";
        }
        
        if(dragItem) dragItem.style.transition = '0.5s';
        dragItem = null;
    }

    function createSparkle(x, y) {
        const s = document.createElement('div');
        s.className = 'sparkle';
        s.innerHTML = '✨';
        s.style.left = x + 'px';
        s.style.top = y + 'px';
        s.style.setProperty('--x', (Math.random() - 0.5) * 100 + 'px');
        s.style.setProperty('--y', (Math.random() - 0.5) * 100 + 'px');
        document.body.appendChild(s);
        setTimeout(() => s.remove(), 800);
    }
</script>
</body>
</html>
