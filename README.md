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
            text-align: center;
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
            display: flex;
            justify-content: center;
            gap: 5px;
            margin: 20px 0;
        }

        .reel {
            width: 80px;
            height: 120px;
            overflow: hidden;
            position: relative;
            background: #000;
            border-radius: 5px;
            border: 2px solid #444;
        }

        .strip {
            position: absolute;
            width: 100%;
            transition: transform 2s ease-out;
        }

        .symbol {
            height: 120px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
        }

        .controls {
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
            font-size: 20px;
            margin-top: 10px;
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
            animation: win-blink 0.5s infinite alternate;
        }

        @keyframes win-blink {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.2); }
        }
    </style>
</head>
<body>
    <div class="slot-machine">
        <div class="status">
            <span>Puan: <span id="score">100</span></span> | <span>Havuz: <span id="pool">500000</span></span>
        </div>

        <div class="reels">
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
        </div>

        <div class="controls">
            <button id="spin">SPİN (10 Puan)</button>
        </div>

        <div id="message" class="status"></div>
    </div>

    <script>
        const symbols = ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"];
        const SYMBOL_HEIGHT = 120;
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

        function initializeStrips() {
            elements.reels.forEach(strip => {
                let symbolsHTML = "";
                for(let i = 0; i < 21; i++) {
                    symbolsHTML += `<div class="symbol">${symbols[i % symbols.length]}</div>`;
                }
                strip.innerHTML = symbolsHTML;
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
            firework.style.left = `${x}px`;
            firework.style.top = `${y}px`;
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
            elements.reels.forEach((strip, index) => {
                const randomIndex = Math.floor(Math.random() * symbols.length);
                const targetY = -(randomIndex * SYMBOL_HEIGHT);
                
                strip.style.transition = `transform ${1.5 + index * 0.2}s ease-out`;
                strip.style.transform = `translateY(${targetY}px)`;

                results.push(symbols[randomIndex]);
            });

            setTimeout(() => {
                checkWin(results);
                isSpinning = false;
                elements.spinBtn.disabled = false;
            }, 2500);
        }

        function checkWin(results) {
            const count = results.reduce((acc, val) => (acc[val] = (acc[val] || 0) + 1, acc), {});
            const maxCount = Math.max(...Object.values(count));
            const winAmount = {3: 10, 4: 50, 5: 100, 6: 1000, 7: 5000}[maxCount] || 0;

            if(winAmount > 0) {
                updateScore(winAmount);
                updatePool(-winAmount);
                elements.message.innerHTML = `🎉 ${maxCount} aynı sembol ile kazandınız! +${winAmount} Puan 🎉`;
                showWinAnimation();
                elements.message.classList.add('win-animation');
            } else {
                elements.message.textContent = "Tekrar deneyin...";
                elements.message.classList.remove('win-animation');
            }
        }

        elements.spinBtn.addEventListener('click', spinReels);
        initializeStrips();
    </script>
</body>
</html>
