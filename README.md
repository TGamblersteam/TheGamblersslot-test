<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TheGamblersslot-test</title>
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
            max-width: 600px;
            width: 100%;
        }

        .reels {
            display: flex;
            justify-content: center;
            gap: 5px;
            margin: 15px 0;
        }

        .reel {
            width: 18vw;
            height: 25vw;
            max-width: 80px;
            max-height: 120px;
            position: relative;
            overflow: hidden;
            background: #000;
            border-radius: 5px;
            border: 2px solid #ffcc00;
        }

        .strip {
            position: absolute;
            width: 100%;
            transition: transform 0.8s cubic-bezier(0.25, 0.1, 0.25, 1);
        }

        .strip div {
            height: 25vw;
            max-height: 120px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 6vw;
        }

        @media (min-width: 600px) {
            .strip div {
                font-size: 30px;
            }
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
            font-size: 14px;
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

        #autoSpin {
            background: #00cc66;
            color: white;
        }

        #shareButton {
            background: #1da1f2;
            color: white;
        }

        .status {
            margin: 10px 0;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <div class="slot-machine">
        <h1>TheGamblersslot-test</h1>
        
        <div class="status">
            Puan: <span id="score">100</span><br>
            Havuz: <span id="pool">500000</span>
        </div>

        <div class="reels">
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
        </div>

        <div class="buttons">
            <button id="spin">SPIN</button>
            <button id="autoSpin">5 SPIN</button>
            <button id="shareButton">PAYLAŞ</button>
        </div>

        <div id="message" class="status"></div>
    </div>

    <script>
        // Oyun verileri
        const symbols = ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"];
        const symbolHeight = 120;
        let score = 100;
        let pool = 500000;
        let isSpinning = false;

        // DOM elementleri
        const elements = {
            score: document.getElementById('score'),
            pool: document.getElementById('pool'),
            message: document.getElementById('message'),
            reels: document.querySelectorAll('.reel .strip')
        };

        // Strip'leri başlat
        function initializeStrips() {
            elements.reels.forEach(strip => {
                let html = '';
                for(let i = 0; i < 10; i++) { // Daha akıcı animasyon için fazla sembol
                    html += `<div>${symbols[Math.floor(Math.random()*symbols.length)]}</div>`;
                }
                strip.innerHTML = html;
            });
        }

        // Puan güncelleme
        function updateScore(points) {
            score += points;
            elements.score.textContent = score;
        }

        // Havuz güncelleme
        function updatePool(points) {
            pool += points;
            elements.pool.textContent = pool;
        }

        // Spin animasyonu
        function spinReels() {
            if(isSpinning || score < 10) return;
            isSpinning = true;
            
            updateScore(-10);
            updatePool(10);

            const results = [];
            
            elements.reels.forEach((strip, index) => {
                const randomPos = -(Math.floor(Math.random()*7 + 3) * symbolHeight);
                strip.style.transform = `translateY(${randomPos}px)`;
                
                const resultIndex = Math.abs(randomPos/symbolHeight) % symbols.length;
                results.push(symbols[resultIndex]);
            });

            setTimeout(() => {
                checkWin(results);
                isSpinning = false;
            }, 800);
        }

        // Kazanç kontrolü
        function checkWin(results) {
            const counts = {};
            results.forEach(symbol => counts[symbol] = (counts[symbol] || 0) + 1);
            
            const maxCount = Math.max(...Object.values(counts));
            const winAmount = {3:10, 4:100, 5:1000}[maxCount] || 0;

            if(winAmount > 0) {
                updateScore(winAmount);
                updatePool(-winAmount);
                elements.message.textContent = `${maxCount}x KAZANDINIZ! +${winAmount} Puan`;
                elements.message.style.color = '#ffcc00';
            } else {
                elements.message.textContent = "Tekrar deneyin...";
                elements.message.style.color = 'white';
            }
        }

        // Otomatik spin
        let autoSpinInterval;
        document.getElementById('autoSpin').addEventListener('click', () => {
            if(autoSpinInterval) {
                clearInterval(autoSpinInterval);
                autoSpinInterval = null;
            } else {
                let spinsLeft = 5;
                autoSpinInterval = setInterval(() => {
                    if(spinsLeft-- > 0) spinReels();
                    else clearInterval(autoSpinInterval);
                }, 1000);
            }
        });

        // İlk yükleme
        initializeStrips();
        document.getElementById('spin').addEventListener('click', spinReels);
    </script>
</body>
</html>
