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
            flex-wrap: wrap;
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
            transition: transform 2s cubic-bezier(0.25, 0.1, 0.25, 1);
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
            <!-- 7 Makara -->
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
        const SYMBOL_HEIGHT = 120;
        let score = 100;
        let pool = 500000;
        let isSpinning = false;

        const elements = {
            score: document.getElementById('score'),
            pool: document.getElementById('pool'),
            message: document.getElementById('message'),
            reels: document.querySelectorAll('.reel .strip')
        };

        // Makaraları başlat
        function initializeStrips() {
            elements.reels.forEach(strip => {
                let html = '';
                for(let i = 0; i < 21; i++) { // 3 tam döngü
                    html += `<div>${symbols[i % symbols.length]}</div>`;
                }
                strip.innerHTML = html;
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

        function spinReels() {
            if (isSpinning || score < 10) return;
            isSpinning = true;
            
            updateScore(-10);
            updatePool(10);

            let results = [];
            const animationTime = 2000;

            elements.reels.forEach((strip, index) => {
                const targetIndex = Math.floor(Math.random() * symbols.length);
                const randomPos = -(targetIndex * SYMBOL_HEIGHT + 3 * SYMBOL_HEIGHT * symbols.length);
                
                strip.style.transition = `transform ${animationTime}ms cubic-bezier(0.25, 0.1, 0.25, 1)`;
                strip.style.transform = `translateY(${randomPos}px)`;

                results.push(symbols[targetIndex]);
            });

            setTimeout(() => {
                checkWin(results);
                isSpinning = false;
            }, animationTime);
        }

        function checkConsecutive(results) {
            let maxConsecutive = 1;
            let current = 1;
            
            for(let i = 1; i < results.length; i++) {
                if(results[i] === results[i-1]) {
                    current++;
                    maxConsecutive = Math.max(maxConsecutive, current);
                } else {
                    current = 1;
                }
            }
            return maxConsecutive;
        }

        function checkWin(results) {
            const consecutiveCount = checkConsecutive(results);
            const winAmounts = {3:10, 4:50, 5:100, 6:1000, 7:5000};
            const winAmount = winAmounts[consecutiveCount] || 0;

            if(winAmount > 0) {
                updateScore(winAmount);
                updatePool(-winAmount);
                elements.message.textContent = `🎉 ${consecutiveCount} ARDIŞIK KAZANÇ! +${winAmount} Puan 🎉`;
                elements.message.classList.add("winning");
            } else {
                elements.message.textContent = "Tekrar deneyin...";
                elements.message.classList.remove("winning");
            }
        }

        // Event listener'lar
        document.getElementById('spin').addEventListener('click', spinReels);
        
        // Oyunu başlat
        initializeStrips();
    </script>
</body>
</html>
