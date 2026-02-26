<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Thế Giới Thú Cưng Lung Linh</title>
    <style>
        /* Phong cách tổng thể - Ấm cúng & Lung linh */
        :root {
            --bg-color: #fdfcf5; /* Màu kem nhẹ nhàng */
            --accent-pink: #ffdae9; /* Hồng pastel */
            --accent-gold: #fff3b0; /* Vàng kim nhạt */
            --text-color: #7a6e5f; /* Nâu ấm */
            --glow-shadow: 0 0 15px rgba(255, 243, 176, 0.8);
        }

        body, html {
            margin: 0; padding: 0;
            width: 100%; height: 100%;
            overflow: hidden; /* Không cho cuộn trang */
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            display: flex; justify-content: center; align-items: center;
            touch-action: none; /* Tối ưu cho cảm ứng */
        }

        /* --- CĂN PHÒNG HOẠT HÌNH TINH XẢO --- */
        #game-container {
            width: 100vw; height: 100vh;
            max-width: 600px; max-height: 900px; /* Giới hạn kích thước trên màn hình lớn */
            background-image: url('https://img.pikbest.com/origin/09/24/63/61ypIkbEsT49Q.jpg!w700wp'); /* Nền phòng hoạt hình chi tiết */
            background-size: cover; background-position: center;
            position: relative;
            border: 8px solid var(--accent-pink); border-radius: 40px;
            box-shadow: 0 15px 50px rgba(0,0,0,0.15);
            overflow: hidden;
        }

        /* Chiếc nệm êm ái cho Pet */
        #pet-bed {
            position: absolute; bottom: 15%; left: 50%;
            transform: translateX(-50%);
            width: 80%; height: 120px;
            background-image: url('https://img.pikbest.com/png-images/20240523/cartoon-style-dog-bed-soft-sleep-vector_10578687.png!bw700'); /* Hình ảnh nệm chi tiết */
            background-size: contain; background-repeat: no-repeat; background-position: center;
            z-index: 10;
        }

        /* Container chứa Pet */
        #pet-container {
            position: absolute; bottom: 18%; left: 50%;
            transform: translateX(-50%);
            width: 200px; height: 200px;
            z-index: 20;
            display: flex; justify-content: center; align-items: center;
        }

        /* Sticker Pet hoạt hình tinh xảo */
        #pet-sticker {
            width: 100%; height: 100%;
            background-size: contain; background-repeat: no-repeat; background-position: center;
            filter: drop-shadow(0 5px 10px rgba(0,0,0,0.2));
            transition: transform 0.3s ease;
        }

        /* Hiệu ứng lung linh tỏa sáng khi được chải lông */
        .pet-glow {
            filter: drop-shadow(0 0 20px var(--accent-gold)) !important;
            animation: breathe 2s infinite alternate;
        }

        @keyframes breathe {
            from { transform: scale(1); }
            to { transform: scale(1.03); }
        }

        /* --- VẬT PHẨM TƯƠNG TÁC (KÉO THẢ) --- */
        .draggable-item {
            position: absolute;
            width: 80px; height: 80px;
            background-size: contain; background-repeat: no-repeat; background-position: center;
            cursor: grab;
            z-index: 100;
            touch-action: none;
            filter: drop-shadow(0 5px 8px rgba(0,0,0,0.15));
        }

        .draggable-item:active { cursor: grabbing; }

        /* Bát thức ăn (cho Chó/Mèo) */
        #food-item { bottom: 5%; left: 15%; display: none; }
        /* Hạt dưa hấu (cho Vẹt) */
        #seed-item { bottom: 5%; left: 15%; display: none; }
        /* Cây lược */
        #comb-item { bottom: 5%; right: 15%; display: none;
            background-image: url('https://png.pngtree.com/png-vector/20220612/ourmid/pngtree-comb-hair-cartoon-vector-icon-png-image_5033729.png');
        }

        /* --- UI: MÀN HÌNH CHỌN & ĐẶT TÊN --- */
        #ui-layer {
            position: absolute; inset: 0;
            background: rgba(255,255,255,0.85);
            backdrop-filter: blur(10px);
            z-index: 200;
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            transition: opacity 0.8s ease, transform 0.8s ease;
            border-radius: 32px;
        }

        .ui-hide { opacity: 0; transform: translateY(-100%); pointer-events: none; }

        h1 { font-size: 1.8rem; margin-bottom: 30px; color: #ff8fab; }

        /* Vùng chọn thú cưng */
        #pet-selection { display: flex; gap: 20px; margin-bottom: 30px; }

        .select-option {
            width: 100px; height: 100px;
            background-size: contain; background-repeat: no-repeat; background-position: center;
            cursor: pointer;
            transition: transform 0.3s;
            filter: drop-shadow(0 4px 6px rgba(0,0,0,0.1));
        }

        .select-option:hover { transform: scale(1.1) rotate(5deg); }

        /* Vùng nhập tên */
        #name-input-container { display: none; text-align: center; }

        input {
            padding: 15px 20px; border-radius: 25px;
            border: 3px solid var(--accent-pink);
            font-size: 1.1rem; width: 250px;
            outline: none; color: var(--text-color);
            margin-bottom: 20px; text-align: center;
        }

        button {
            padding: 12px 30px; border-radius: 25px;
            background: #ff8fab; color: white;
            border: none; font-size: 1rem; font-weight: bold;
            cursor: pointer; transition: 0.3s;
        }
        button:hover { background: #fb6f92; transform: translateY(-3px); }

        /* Nhãn tên Pet trên màn hình chính */
        #pet-label {
            position: absolute; top: 12%; left: 50%;
            transform: translateX(-50%);
            background: rgba(255,255,255,0.6);
            padding: 8px 25px; border-radius: 20px;
            font-size: 1.4rem; font-weight: bold; color: #fb6f92;
            text-shadow: 1px 1px white;
            display: none; z-index: 50;
        }

        /* --- HIỆU ỨNG SPARKLE (LUNG LINH) --- */
        .sparkle {
            position: absolute; pointer-events: none;
            width: 20px; height: 20px;
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%23fff3b0"><path d="M12 2l2.4 7.2h7.6l-6 4.8 2.4 7.2-6-4.8-6 4.8 2.4-7.2-6-4.8h7.6z"/></svg>');
            background-size: contain;
            animation: sparkle-fly 1s ease-out forwards;
            z-index: 150;
        }

        @keyframes sparkle-fly {
            0% { transform: scale(0) rotate(0deg); opacity: 1; }
            100% { transform: translate(var(--dx), var(--dy)) scale(1.5) rotate(180deg); opacity: 0; }
        }

        .hidden { display: none !important; }

    </style>
</head>
<body>

<div id="game-container">
    <div id="pet-bed"></div>
    
    <div id="pet-label"></div>

    <div id="pet-container">
        <div id="pet-sticker"></div>
    </div>

    <div id="food-item" class="draggable-item" style="background-image: url('https://png.pngtree.com/png-vector/20231017/ourmid/pngtree-cartoon-dog-food-bowl-vector-png-image_10199042.png');"></div>
    <div id="seed-item" class="draggable-item" style="background-image: url('https://png.pngtree.com/png-vector/20220623/ourmid/pngtree-sunflower-seeds-grain-nut-cartoon-png-image_5316496.png');"></div>
    <div id="comb-item" class="draggable-item"></div>

    <div id="ui-layer">
        <h1 id="ui-title">Chào bạn, chọn một người bạn nhé!</h1>
        
        <div id="pet-selection">
            <div class="select-option" style="background-image: url('https://png.pngtree.com/png-vector/20231019/ourmid/pngtree-cartoon-golden-retriever-puppy-vector-png-image_10207184.png');" onclick="selectPet('dog')"></div>
            <div class="select-option" style="background-image: url('https://png.pngtree.com/png-vector/20240131/ourmid/pngtree-cute-calico-cat-sit-cartoon-png-image_11579728.png');" onclick="selectPet('cat')"></div>
            <div class="select-option" style="background-image: url('https://png.pngtree.com/png-vector/20230906/ourmid/pngtree-blue-macaw-parrot-animal-cartoon-vector-png-image_10011504.png');" onclick="selectPet('parrot')"></div>
        </div>

        <div id="name-input-container">
            <input type="text" id="pet-name-input" placeholder="Tên của bạn ấy là gì nhỉ?" maxlength="12">
            <br>
            <button onclick="confirmName()">Bắt đầu chơi</button>
        </div>
    </div>
</div>

<script>
    // Dữ liệu hình ảnh và vật phẩm cho từng loại pet (sử dụng link internet)
    const petData = {
        'dog': {
            sticker: 'https://png.pngtree.com/png-vector/20231019/ourmid/pngtree-cartoon-golden-retriever-puppy-vector-png-image_10207184.png',
            foodItem: 'food-item'
        },
        'cat': {
            sticker: 'https://png.pngtree.com/png-vector/20240131/ourmid/pngtree-cute-calico-cat-sit-cartoon-png-image_11579728.png',
            foodItem: 'food-item'
        },
        'parrot': {
            sticker: 'https://png.pngtree.com/png-vector/20230906/ourmid/pngtree-blue-macaw-parrot-animal-cartoon-vector-png-image_10011504.png',
            foodItem: 'seed-item'
        }
    };

    let currentPetType = '';
    let isDragging = false;
    let dragItem = null;
    let offsetX, offsetY;
    let feedCount = 0;
    let isFed = false;
    let groomSwipeCount = 0;
    let lastSwipeX = 0;

    // --- BƯỚC 1: CHỌN PET ---
    function selectPet(type) {
        currentPetType = type;
        // Ẩn vùng chọn, hiện vùng nhập tên
        document.getElementById('pet-selection').style.display = 'none';
        document.getElementById('ui-title').innerText = "Tên của người bạn mới là...";
        document.getElementById('name-input-container').style.display = 'block';
    }

    // --- BƯỚC 2: XÁC NHẬN TÊN & BẮT ĐẦU ---
    function confirmName() {
        const nameInput = document.getElementById('pet-name-input').value.trim();
        const petName = nameInput || "Bé Cưng"; // Tên mặc định nếu không nhập

        // Ẩn lớp UI
        document.getElementById('ui-layer').classList.add('ui-hide');
        
        // Hiện nhãn tên
        const label = document.getElementById('pet-label');
        label.innerText = petName;
        label.style.display = 'block';

        // Hiển thị Pet và Vật phẩm tương ứng
        initGame(petName);
    }

    // --- KHỞI TẠO TRÒ CHƠI CHÍNH ---
    function initGame(name) {
        const data = petData[currentPetType];
        
        // Đặt hình ảnh Pet
        const petSticker = document.getElementById('pet-sticker');
        petSticker.style.backgroundImage = `url('${data.sticker}')`;

        // Hiển thị đúng loại thức ăn
        const foodId = data.foodItem;
        const foodEl = document.getElementById(foodId);
        foodEl.style.display = 'block';
        addDragListeners(foodEl); // Thêm sự kiện kéo thả cho thức ăn

        // Thêm sự kiện kéo thả cho lược (nhưng ẩn lúc đầu)
        addDragListeners(document.getElementById('comb-item'));
    }

    // --- XỬ LÝ KÉO THẢ (NHẤN GIỮ & VUỐT) ---
    function addDragListeners(el) {
        // Cho chuột
        el.addEventListener('mousedown', dragStart);
        // Cho cảm ứng (điện thoại)
        el.addEventListener('touchstart', dragStart, { passive: false });
    }

    function dragStart(e) {
        if (isDragging) return;
        isDragging = true;
        dragItem = e.target;
        
        // Tạo hiệu ứng phóng to nhẹ khi cầm
        dragItem.style.transition = 'transform 0.1s ease';
        dragItem.style.transform = 'scale(1.1)';

        const rect = dragItem.getBoundingClientRect();
        
        if (e.type === 'touchstart') {
            offsetX = e.touches[0].clientX - rect.left;
            offsetY = e.touches[0].clientY - rect.top;
        } else {
            offsetX = e.clientX - rect.left;
            offsetY = e.clientY - rect.top;
        }

        document.addEventListener('mousemove', dragMove);
        document.addEventListener('touchmove', dragMove, { passive: false });
        document.addEventListener('mouseup', dragEnd);
        document.addEventListener('touchend', dragEnd);
    }

    function dragMove(e) {
        if (!isDragging || !dragItem) return;
        
        let x, y;
        if (e.type === 'touchmove') {
            e.preventDefault(); // Ngăn cuộn trang trên điện thoại
            x = e.touches[0].clientX;
            y = e.touches[0].clientY;
        } else {
            x = e.clientX;
            y = e.clientY;
        }

        // Cập nhật vị trí vật phẩm theo ngón tay
        const containerRect = document.getElementById('game-container').getBoundingClientRect();
        let left = x - containerRect.left - offsetX;
        let top = y - containerRect.top - offsetY;

        // Giới hạn trong container
        left = Math.max(0, Math.min(left, containerRect.width - dragItem.offsetWidth));
        top = Math.max(0, Math.min(top, containerRect.height - dragItem.offsetHeight));

        dragItem.style.left = left + 'px';
        dragItem.style.top = top + 'px';

        // Kiểm tra va chạm với Pet
        checkInteraction(x, y);
    }

    function dragEnd() {
        if (!isDragging || !dragItem) return;
        isDragging = false;

        // Trả lại kích thước bình thường
        dragItem.style.transform = 'scale(1)';
        dragItem.style.transition = 'left 0.5s ease, top 0.5s ease, transform 0.1s ease';

        // Logic sau khi thả vật phẩm
        if (dragItem.id === 'food-item' || dragItem.id === 'seed-item') {
            if (!isFed) {
                // Nếu chưa ăn no, trả bát về chỗ cũ (bạn có thể thêm logic này nếu muốn)
            }
        } else if (dragItem.id === 'comb-item') {
             // Reset bộ đếm vuốt nếu thả lược ra
             groomSwipeCount = 0;
        }

        dragItem = null;
        document.removeEventListener('mousemove', dragMove);
        document.removeEventListener('touchmove', dragMove);
        document.removeEventListener('mouseup', dragEnd);
        document.removeEventListener('touchend', dragEnd);
    }

    // --- KIỂM TRA TƯƠNG TÁC KHI VUỐT QUA PET ---
    function checkInteraction(x, y) {
        const petSticker = document.getElementById('pet-sticker');
        const petRect = petSticker.getBoundingClientRect();

        // Kiểm tra xem ngón tay/chuột có nằm trên Pet không
        if (x > petRect.left && x < petRect.right && y > petRect.top && y < petRect.bottom) {
            
            // 1. Logic cho ăn
            if ((dragItem.id === 'food-item' || dragItem.id === 'seed-item') && !isFed) {
                feedCount++;
                createSparkles(x, y, 2); // Tạo ít sao khi ăn
                
                // Hiệu ứng ăn (giật nhẹ)
                petSticker.style.transform = 'scale(1.05)';
                setTimeout(() => petSticker.style.transform = 'scale(1)', 100);

                if (feedCount > 30) { // Cần vuốt qua lại một lúc
                    isFed = true;
                    // Ẩn bát thức ăn, hiện lược
                    dragItem.style.display = 'none';
                    document.getElementById('comb-item').style.display = 'block';
                    createSparkles(petRect.left + petRect.width/2, petRect.top + petRect.height/2, 15); // Sao lớn khi no
                    petSticker.style.transition = 'transform 0.5s ease';
                }
            }
            
            // 2. Logic chải lông (vuốt đổi hướng 5 lần)
            else if (dragItem.id === 'comb-item' && isFed) {
                if (Math.abs(x - lastSwipeX) > 20) { // Vuốt đủ dài
                    const currentDirection = x > lastSwipeX ? 'right' : 'left';
                    
                    if (this.lastDirection && this.lastDirection !== currentDirection) {
                        groomSwipeCount++;
                        createSparkles(x, y, 3);
                        
                        // Hiệu ứng chải (nghiêng nhẹ)
                        petSticker.style.transform = `rotate(${currentDirection === 'right' ? 3 : -3}deg)`;

                        if (groomSwipeCount >= 5) {
                            // HOÀN THÀNH: Pet lung linh
                            petSticker.classList.add('pet-glow');
                            createSparkles(petRect.left + petRect.width/2, petRect.top + petRect.height/2, 25);
                            
                            // Ẩn lược
                            dragItem.style.display = 'none';
                            groomSwipeCount = -100; // Ngăn đếm tiếp
                        }
                    }
                    this.lastDirection = currentDirection;
                }
                lastSwipeX = x;
            }
        }
    }

    // --- TẠO HIỆU ỨNG SAO LUNG LINH ---
    function createSparkles(x, y, count) {
        const container = document.getElementById('game-container');
        const containerRect = container.getBoundingClientRect();

        for (let i = 0; i < count; i++) {
            const sparkle = document.createElement('div');
            sparkle.className = 'sparkle';
            
            // Vị trí xuất hiện (tương đối với container)
            sparkle.style.left = (x - containerRect.left - 10) + 'px';
            sparkle.style.top = (y - containerRect.top - 10) + 'px';
            
            // Hướng bay ngẫu nhiên
            const dx = (Math.random() - 0.5) * 150 + 'px';
            const dy = (Math.random() - 0.5) * 150 + 'px';
            sparkle.style.setProperty('--dx', dx);
            sparkle.style.setProperty('--dy', dy);
            
            container.appendChild(sparkle);
            
            // Xóa sao sau khi diễn xong animation
            setTimeout(() => sparkle.remove(), 1000);
        }
    }

</script>

</body>
</html>
