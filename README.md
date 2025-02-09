<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>7 Makaralı Slot Oyunu</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background: #1a1a1a;
            color: white;
            touch-action: manipulation;
            overflow-x: hidden;
        }

        .slot-machine {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background: #2a2a2a;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0,0,0,0.5);
        }

        .reels {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 5px;
            margin: 20px 0;
        }

        .reel {
            height: 100px;
            overflow: hidden;
            position: relative;
            background: #000;
            border-radius: 5px;
            border: 2px solid #444;
        }

        .strip {
            position: absolute;
            width: 100%;
            transition: transform 2.5s cubic-bezier(0.25, 0.1, 0.25, 1);
        }

        .symbol {
            height: 100px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
            text-shadow: 0 0 10px rgba(255,255,255,0.3);
        }

        .controls {
            text-align: center;
            margin: 20px 0;
        }

        button {
            padding: 12px 30px;
            font-size: 18px;
            background: #e74c3c;
            color: white;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s;
        }

        button:disabled {
            background: #7f8c8d;
            cursor: not-allowed;
        }

        .status {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin: 20px 0;
            font-size: 20px;
        }

        .firework {
            position: fixed;
            width: 10px;
            height: 10px;
            background: #ff0;
            border-radius: 50%;
            pointer-events: none;
            animation: explode 1.5s ease-out;
        }

        @keyframes explode {
            0% { transform: scale(0); opacity: 1; }
            100% { transform: scale(20); opacity: 0; }
        }

        .win-animation {
            animation: win-blink 0.5s infinite;
        }

        @keyframes win-blink {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.2); }
        }

        @media (max-width: 768px) {
            .reel { height: 70px; }
            .symbol { height: 70px; font-size: 24px; }
        }
    </style>
</head>
<body>
    <div class="slot-machine">
        <div class="status">
            <div>Puan: <span id="score">100</span></div>
            <div>Havuz: <span id="pool">500000</span></div>
        </div>

        <div class="reels">
            <div class="reel"><div class="strip"></div></div>
            <!-- 6 more reels -->
        </div>

        <div class="controls">
            <button id="spin">SPİN (10 Puan)</button>
        </div>
        <div id="message" class="status"></div>
    </div>

    <script>
        const symbols = ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"];
        const SYMBOL_HEIGHT = 100;
        let score = 100;
        let pool = 500000;
        let isSpinning = false;

        const elements = {
            score: document.getElementById('score'),
            pool: document.getElementById('pool'),
            message: document.getElementById('message'),
            spinBtn: document.getElementById('spin'),
            reels: document.querySelectorAll('.reel .strip')
        };

        // Optimize edilmiş strip oluşturma
        function initializeStrips() {
            elements.reels.forEach(strip => {
                const fragment = document.createDocumentFragment();
                for(let i = 0; i < 21; i++) {
                    const div = document.createElement('div');
                    div.className = 'symbol';
                    div.textContent = symbols[i % 7];
                    fragment.appendChild(div);
                }
                strip.appendChild(fragment);
            });
        }

        function updateScore(points) {
            score = Math.max(0, score + points);
            elements.score.textContent = score;
        }

        function updatePool(points) {
            pool = Math.max(0, pool + points);
            elements.pool.textContent = pool;
        }

        function createFirework(x, y) {
            const firework = document.createElement('div');
            firework.className = 'firework';
            firework.style.left = x + 'px';
            firework.style.top = y + 'px';
            document.body.appendChild(firework);
            setTimeout(() => firework.remove(), 1500);
        }

        function showWinAnimation() {
            for(let i = 0; i < 20; i++) {
                setTimeout(() => {
                    createFirework(
                        Math.random() * window.innerWidth,
                        Math.random() * window.innerHeight
                    );
                }, i * 50);
            }
        }

        function spinReels() {
            if(isSpinning || score < 10) return;
            
            isSpinning = true;
            elements.spinBtn.disabled = true;
            updateScore(-10);
            updatePool(10);

            const results = [];
            const startTime = Date.now();

            elements.reels.forEach((strip, index) => {
                const targetIndex = Math.floor(Math.random() * 7);
                const targetY = -(targetIndex * SYMBOL_HEIGHT + 3 * 7 * SYMBOL_HEIGHT);
                
                strip.style.transition = 'none';
                strip.style.transform = `translateY(${-14 * SYMBOL_HEIGHT}px)`;
                
                requestAnimationFrame(() => {
                    strip.style.transition = 'transform 2.5s cubic-bezier(0.25, 0.1, 0.25, 1)';
                    strip.style.transform = `translateY(${targetY}px)`;
                });

                results.push(symbols[targetIndex]);
            });

            setTimeout(() => {
                checkWin(results);
                isSpinning = false;
                elements.spinBtn.disabled = false;
            }, 2500);
        }

        function checkConsecutive(results) {
            let maxCount = 1;
            let current = 1;
            
            for(let i = 1; i < results.length; i++) {
                results[i] === results[i-1] ? current++ : current = 1;
                maxCount = Math.max(maxCount, current);
            }
            return maxCount;
        }

        function checkWin(results) {
            const count = checkConsecutive(results);
            const winAmount = {3:10, 4:50, 5:100, 6:1000, 7:5000}[count] || 0;

            if(winAmount > 0) {
                updateScore(winAmount);
                updatePool(-winAmount);
                elements.message.innerHTML = `🎉 ${count} ARDIŞIK KAZANÇ! +${winAmount} Puan 🎉`;
                showWinAnimation();
                elements.message.classList.add('win-animation');
            } else {
                elements.message.textContent = "Tekrar deneyin...";
                elements.message.classList.remove('win-animation');
            }
        }

        // Event Listeners
        elements.spinBtn.addEventListener('click', spinReels);
        
        // Başlangıç
        initializeStrips();
    </script>
</body>
</html>
