<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>7 Makaralı Slot Oyunu</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background: linear-gradient(135deg, #2c3e50, #34495e);
            color: white;
            margin: 0;
            padding: 20px;
            touch-action: manipulation;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .slot-machine {
            background: linear-gradient(145deg, #1e1e1e, #2c2c2c);
            border: 5px solid #ffcc00;
            border-radius: 15px;
            padding: 15px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.4);
            max-width: 800px;
            width: 100%;
        }

        .reels {
            display: flex;
            justify-content: center;
            gap: 5px;
            margin: 15px 0;
        }

        .reel {
            width: 80px;
            height: 120px;
            overflow: hidden;
            background: #000;
            border-radius: 5px;
            border: 2px solid #ffcc00;
            position: relative;
        }

        .strip {
            position: absolute;
            width: 100%;
            transition: transform 1s ease-out;
        }

        .strip div {
            height: 120px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
        }

        .buttons {
            margin: 15px 0;
            display: flex;
            gap: 10px;
            justify-content: center;
            flex-wrap: wrap;
        }

        button {
            padding: 10px 20px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.2s;
            min-width: 100px;
        }

        #spin {
            background: #ff4444;
            color: white;
        }

        .status {
            margin: 10px 0;
            font-size: 18px;
        }

        .winning {
            animation: blink 0.5s infinite alternate;
        }

        @keyframes blink {
            from { color: #ffcc00; }
            to { color: #ffffff; }
        }
    </style>
</head>
<body>
    <div class="slot-machine">
        <h1>7 Makaralı Slot Oyunu</h1>
        
        <div class="status">
            Puan: <span id="score">100</span> | Havuz: <span id="pool">500000</span>
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

        <div class="buttons">
            <button id="spin">SPIN</button>
        </div>

        <div id="message" class="status"></div>
    </div>

    <script>
        const symbols = ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"];
        let score = 100;
        let pool = 500000;
        let isSpinning = false;

        const elements = {
            score: document.getElementById('score'),
            pool: document.getElementById('pool'),
            message: document.getElementById('message'),
            reels: document.querySelectorAll('.reel .strip')
        };

        function initializeStrips() {
            elements.reels.forEach(strip => {
                let html = '';
                for (let i = 0; i < 15; i++) { 
                    html += `<div>${symbols[Math.floor(Math.random() * symbols.length)]}</div>`;
                }
                strip.innerHTML = html;
            });
        }

        function updateScore(points) {
            score += points;
            elements.score.textContent = score;
        }

        function updatePool(points) {
            pool += points;
            elements.pool.textContent = pool;
        }

        function spinReels() {
            if (isSpinning || score < 10) return;
            isSpinning = true;

            updateScore(-10);
            updatePool(10);

            let results = [];
            elements.reels.forEach((strip, index) => {
                const randomPos = -(Math.floor(Math.random() * 7 + 3) * 120);
                strip.style.transition = `transform ${1 + index * 0.2}s ease-out`;
                strip.style.transform = `translateY(${randomPos}px)`;

                const resultIndex = Math.abs(randomPos / 120) % symbols.length;
                results.push(symbols[resultIndex]);
            });

            setTimeout(() => {
                checkWin(results);
                isSpinning = false;
            }, 2500);
        }

        function checkWin(results) {
            const counts = {};
            results.forEach(symbol => counts[symbol] = (counts[symbol] || 0) + 1);
            
            const maxCount = Math.max(...Object.values(counts));
            const winAmount = {3: 10, 4: 50, 5: 100, 6: 1000, 7: 5000}[maxCount] || 0;

            if (winAmount > 0) {
                updateScore(winAmount);
                updatePool(-winAmount);
                elements.message.textContent = `🎉 ${maxCount}x KAZANDINIZ! +${winAmount} Puan 🎉`;
                elements.message.classList.add("winning");
            } else {
                elements.message.textContent = "Tekrar deneyin...";
                elements.message.classList.remove("winning");
            }
        }

        document.getElementById('spin').addEventListener('click', spinReels);
        initializeStrips();
    </script>
</body>
</html>
