<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>7 Makaralı Slot Oyunu</title>
    <style>
        /* Değişmeyen CSS kısmı */
    </style>
</head>
<body>
    <div class="slot-machine">
        <!-- Aynı HTML yapısı -->
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

        function initializeStrips() {
            elements.reels.forEach(strip => {
                strip.innerHTML = '';
                for(let i = 0; i < 21; i++) {
                    const symbol = symbols[i % symbols.length];
                    strip.innerHTML += `<div>${symbol}</div>`;
                }
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
            const animationTime = 1500;

            elements.reels.forEach((strip, index) => {
                const targetPosition = Math.floor(Math.random() * 15) * SYMBOL_HEIGHT;
                const randomPos = -(targetPosition % (symbols.length * SYMBOL_HEIGHT));
                
                strip.style.transition = `transform ${animationTime}ms cubic-bezier(0.1, 0.4, 0.2, 1)`;
                strip.style.transform = `translateY(${randomPos}px)`;

                const resultIndex = Math.abs(randomPos / SYMBOL_HEIGHT) % symbols.length;
                results.push(symbols[resultIndex]);
            });

            setTimeout(() => {
                checkWin(results);
                isSpinning = false;
            }, animationTime + 500);
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

        document.getElementById('spin').addEventListener('click', spinReels);
        initializeStrips();
    </script>
</body>
</html>
