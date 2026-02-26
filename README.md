<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Pet Love - Thế Giới Lung Linh</title>
    <style>
        :root {
            --pink: #ffafcc;
            --blue: #a2d2ff;
            --gold: #ffc300;
        }

        body, html {
            margin: 0; padding: 0; width: 100%; height: 100%;
            display: flex; justify-content: center; align-items: center;
            background: #fdf0d5; font-family: 'Segoe UI', sans-serif;
            overflow: hidden; touch-action: none;
        }

        /* Background phòng chi tiết */
        #game-container {
            width: 100vw; height: 100vh; max-width: 500px;
            background: linear-gradient(180deg, #bde0fe 0%, #ffffff 100%);
            position: relative; border: 10px solid #fff; border-radius: 40px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.1); overflow: hidden;
        }

        /* Chi tiết phòng */
        .window { position: absolute; top: 50px; left: 40px; width: 80px; height: 100px; background: #fff; border-radius: 10px; border: 5px solid #ffafcc; box-shadow: 0 0 20px rgba(255,255,255,0.8); }
        .bed { position: absolute; bottom: 15%; left: 50%; transform: translateX(-50%); width: 300px; height: 100px; background: #ffc8dd; border-radius: 50%; border: 6px solid #ffafcc; box-shadow: 0 10px 0 #fb6f92; z-index: 5; }

        /* Sticker Pet tinh xảo bằng SVG */
        #pet-container {
            position: absolute; bottom: 18%; left: 50%; transform: translateX(-50%);
            width: 220px; height: 220px; z-index: 10; display: flex; justify-content: center; align-items: center;
        }

        /* Lớp UI Chọn & Đặt tên */
        #ui-layer {
            position: absolute; inset: 0; z-index: 100; background: rgba(255,255,255,0.9);
            backdrop-filter: blur(10px); display: flex; flex-direction: column; align-items: center; justify-content: center;
            transition: 0.6s ease;
        }

        .select-group { display: flex; gap: 20px; margin: 30px 0; }
        .pet-opt { width: 80px; height: 80px; cursor: pointer; transition: 0.3s; }
        .pet-opt:hover { transform: scale(1.2) rotate(5deg); }

        /* Nhãn tên */
        #pet-label { position: absolute; top: 10%; font-size: 1.8rem; font-weight: bold; color: #fb6f92; text-shadow: 2px 2px #fff; z-index: 50; }

        /* Item Kéo thả */
        .tool {
            position: absolute; width: 90px; height: 90px; z-index: 60;
            filter: drop-shadow(0 10px 10px rgba(0,0,0,0.1)); cursor: grab;
        }

        /* Hiệu ứng Lung Linh */
        .sparkle { position: absolute; pointer-events: none; animation: fly 0.8s forwards; font-size: 25px; z-index: 200; }
        @keyframes fly { 0% { transform: scale(0); opacity: 1; } 100% { transform: translate(var(--x), var(--y)) scale(1.5); opacity: 0; } }

        .glow { filter: drop-shadow(0 0 20px #ffd700) drop-shadow(0 0 40px #fff700) !important; animation: pulse 2s infinite !important; }
        @keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.05); } 100% { transform: scale(1); } }

        .hidden { display: none !important; }
        input { padding: 15px; border-radius: 30px; border: 3px solid #ffafcc; text-align: center; font-size: 1.1rem; outline: none; }
        button { background: #fb6f92; color: white; border: none; padding: 10px 30px; border-radius: 20px; margin-top: 15px; font-weight: bold; cursor: pointer; }
    </style>
</head>
<body>

<div id="game-container">
    <div class="window"></div>
    <div class="bed"></div>
    <div id="pet-label"></div>
    
    <div id="pet-container"></div>

    <div id="food-tool" class="tool hidden" style="left: 40px; bottom: 40px;">
        <svg id="food-svg" viewBox="0 0 100 100"></svg>
    </div>

    <div id="comb-tool" class="tool hidden" style="right: 40px; bottom: 40px;">
        <svg viewBox="0 0 100 100">
            <rect x="20" y="30" width="60" height="20" rx="5" fill="#a2d2ff"/>
            <rect x="25" y="50" width="5" height="30" fill="#a2d2ff"/>
            <rect x="35" y="50" width="5" height="30" fill="#a2d2ff"/>
            <rect x="45" y="50" width="5" height="30" fill="#a2d2ff"/>
            <rect x="55" y="50" width="5" height="30" fill="#a2d2ff"/>
            <rect x="65" y="50" width="5" height="30" fill="#a2d2ff"/>
            <rect x="75" y="50" width="5" height="30" fill="#a2d2ff"/>
        </svg>
    </div>

    <div id="ui-layer">
        <h2 style="color: #fb6f92;">Chọn bạn nhỏ nhé bạn ơi!</h2>
        <div class="select-group">
            <div class="pet-opt" onclick="preSelect('dog')"><svg viewBox="0 0 100 100"><circle cx="50" cy="50" r="45" fill="#fec89a"/><circle cx="35" cy="40" r="5" fill="#333"/><circle cx="65" cy="40" r="5" fill="#333"/><path d="M40 65 Q50 75 60 65" stroke="#333" fill="none" stroke-width="3"/></svg></div>
            <div class="pet-opt" onclick="preSelect('cat')"><svg viewBox="0 0 100 100"><circle cx="50" cy="50" r="45" fill="#e5e5e5"/><circle cx="35" cy="45" r="4" fill="#333"/><circle cx="65" cy="45" r="4" fill="#333"/><path d="M45 60 Q50 65 55 60" stroke="#333" fill="none" stroke-width="2"/></svg></div>
            <div class="pet-opt" onclick="preSelect('bird')"><svg viewBox="0 0 100 100"><circle cx="50" cy="50" r="45" fill="#80ed99"/><circle cx="65" cy="40" r="4" fill="#333"/><path d="M75 45 L95 50 L75 55" fill="#ffb703"/></svg></div>
        </div>
        <div id="name-form" class="hidden">
            <input type="text" id="name-in" placeholder="Tên của bạn ấy là...">
            <br>
            <button onclick="startGame()">Vào chơi thôi!</button>
        </div>
    </div>
</div>

<script>
    let selected = '';
    let isDrag = false;
    let dragItem = null;
    let swipeCount = 0;
    let lastX = 0;
    let feedScore = 0;

    const svgs = {
        dog: `<svg viewBox="0 0 100 100"><defs><radialGradient id="g1"><stop offset="0%" stop-color="#fff"/><stop offset="100%" stop-color="#fec89a"/></radialGradient></defs><circle cx="50" cy="50" r="45" fill="url(#g1)"/><path d="M15 30 Q5 50 15 70" fill="#fec89a"/><path d="M85 30 Q95 50 85 70" fill="#fec89a"/><circle cx="35" cy="45" r="6" fill="#333"/><circle cx="65" cy="45" r="6" fill="#333"/><circle cx="37" cy="43" r="2" fill="#fff"/><circle cx="67" cy="43" r="2" fill="#fff"/><path d="M42 65 Q50 75 58 65" stroke="#fb6f92" fill="none" stroke-width="4" stroke-linecap="round"/></svg>`,
        cat: `<svg viewBox="0 0 100 100"><circle cx="50" cy="50" r="45" fill="#f8f9fa"/><path d="M20 20 L40 35 L25 50 Z" fill="#f8f9fa"/><path d="M80 20 L60 35 L75 50 Z" fill="#f8f9fa"/><circle cx="35" cy="50" r="6" fill="#333"/><circle cx="65" cy="50" r="6" fill="#333"/><circle cx="37" cy="48" r="2" fill="#fff"/><circle cx="67" cy="48" r="2" fill="#fff"/><path d="M45 65 Q50 70 55 65" stroke="#fb6f92" fill="none" stroke-width="3"/></svg>`,
        bird: `<svg viewBox="0 0 100 100"><circle cx="50" cy="50" r="45" fill="#95d5b2"/><path d="M70 40 L95 50 L70 60" fill="#ffb703"/><circle cx="55" cy="40" r="6" fill="#333"/><circle cx="57" cy="38" r="2" fill="#fff"/><path d="M20 50 Q10 70 20 90" fill="#52b788"/></svg>`,
        bowl: `<circle cx="50" cy="70" r="30" fill="#ffafcc"/><path d="M30 70 L70 70 L60 85 L40 85 Z" fill="#fb6f92"/><circle cx="50" cy="55" r="10" fill="#a67c52"/>`, // Đồ ăn chó mèo
        seed: `<ellipse cx="50" cy="50" rx="15" ry="25" fill="#333" stroke="#fff" stroke-width="2"/><path d="M45 35 L55 35" stroke="#fff" stroke-width="2"/>` // Hạt dưa
    };

    function preSelect(t) { selected = t; document.getElementById('name-form').classList.remove('hidden'); }

    function startGame() {
        const n = document.getElementById('name-in').value || "Bé cưng";
        document.getElementById('pet-label').innerText = n;
        document.getElementById('ui-layer').style.opacity = '0';
        setTimeout(() => document.getElementById('ui-layer').classList.add('hidden'), 600);
        
        document.getElementById('pet-container').innerHTML = svgs[selected];
        document.getElementById('food-tool').classList.remove('hidden');
        document.getElementById('food-svg').innerHTML = (selected === 'bird') ? svgs.seed : svgs.bowl;
        
        makeDraggable(document.getElementById('food-tool'));
        makeDraggable(document.getElementById('comb-tool'));
    }

    function makeDraggable(el) {
        el.onmousedown = (e) => start(e.clientX, e.clientY);
        el.ontouchstart = (e) => start(e.touches[0].clientX, e.touches[0].clientY);

        function start(x, y) {
            isDrag = true; dragItem = el;
            el.style.transition = 'none';
        }
    }

    window.onmousemove = (e) => move(e.clientX, e.clientY);
    window.ontouchmove = (e) => move(e.touches[0].clientX, e.touches[0].clientY);
    window.onmouseup = window.ontouchend = () => {
        if (isDrag && dragItem) {
            if (feedScore > 30 && dragItem.id === 'food-tool') {
                dragItem.classList.add('hidden');
                document.getElementById('comb-tool').classList.remove('hidden');
                feedScore = 0;
            }
            if (swipeCount >= 10 && dragItem.id === 'comb-tool') {
                document.getElementById('pet-container').querySelector('svg').classList.add('glow');
            }
            dragItem.style.transition = '0.5s';
        }
        isDrag = false; dragItem = null;
    };

    function move(x, y) {
        if (!isDrag || !dragItem) return;
        dragItem.style.left = (x - 45) + 'px';
        dragItem.style.top = (y - 45) + 'px';

        const p = document.getElementById('pet-container').getBoundingClientRect();
        if (x > p.left && x < p.right && y > p.top && y < p.bottom) {
            createSparkle(x, y);
            if (dragItem.id === 'food-tool') feedScore++;
            if (dragItem.id === 'comb-tool') {
                if (Math.abs(x - lastX) > 20) { swipeCount++; lastX = x; }
            }
        }
    }

    function createSparkle(x, y) {
        const s = document.createElement('div');
        s.className = 'sparkle'; s.innerHTML = '✨';
        s.style.left = x + 'px'; s.style.top = y + 'px';
        s.style.setProperty('--x', (Math.random()-0.5)*100+'px');
        s.style.setProperty('--y', (Math.random()-0.5)*100+'px');
        document.body.appendChild(s);
        setTimeout(() => s.remove(), 800);
    }
</script>
</body>
</html>
