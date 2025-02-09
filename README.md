<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Optimize Slot Oyunu</title>
    <style>
        /* Mobile-first CSS optimizations */
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background: linear-gradient(135deg, #2c3e50, #34495e);
            color: white;
            margin: 0;
            padding: 0;
            touch-action: manipulation; /* Mobile touch optimizasyonu */
        }

        .slot-machine {
            max-width: 100%;
            padding: 10px;
            box-sizing: border-box;
        }

        .reels {
            display: flex;
            justify-content: center;
            gap: 5px;
            padding: 10px;
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
            will-change: transform; /* GPU optimizasyonu */
        }

        .strip div {
            height: 25vw;
            max-height: 120px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 5vw;
        }

        @media (min-width: 600px) {
            .strip div {
                font-size: 30px;
            }
        }

        /* Diğer stiller aynı kalabilir... */
    </style>
</head>
<body>

    <!-- HTML yapısı aynı kalıyor... -->

    <script>
        // Performans optimizasyonları
        const symbols = ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"];
        const reelCount = 5;
        const symbolHeight = 120; // Sabit yükseklik değeri
        let animationFrameId;

        // Önbelleğe alınmış DOM elementleri
        const elements = {
            score: document.getElementById('score'),
            pool: document.getElementById('pool'),
            message: document.getElementById('message'),
            reels: document.querySelectorAll('.reel .strip')
        };

        // GPU accelerated animasyon
        function spinReels() {
            if (score < 10) return alert("Yeterli puanınız yok!");
            
            // Performans için sınıf ekleme
            document.body.classList.add('spinning');
            
            updateScore(-10);
            updatePool(10);

            const results = [];
            
            elements.reels.forEach((strip, index) => {
                const randomPos = -(Math.floor(Math.random() * 7) * symbolHeight);
                strip.style.transform = `translateY(${randomPos}px)`;
                results.push(symbols[Math.abs(randomPos/symbolHeight) % 7]);
            });

            // RAF ile animasyon kontrolü
            function checkAnimation() {
                if (document.body.classList.contains('spinning')) {
                    animationFrameId = requestAnimationFrame(checkAnimation);
                } else {
                    checkWin(results);
                }
            }
            
            setTimeout(() => {
                document.body.classList.remove('spinning');
            }, 1000);

            checkAnimation();
        }

        // Optimize edilmiş kazanç kontrolü
        function checkWin(results) {
            const frequencyMap = {};
            let maxCount = 1;
            
            // Daha hızlı döngü için for kullanımı
            for (let i = 0; i < results.length; i++) {
                const sym = results[i];
                frequencyMap[sym] = (frequencyMap[sym] || 0) + 1;
                if (frequencyMap[sym] > maxCount) maxCount = frequencyMap[sym];
            }

            const winAmounts = {3:10, 4:100, 5:1000};
            const win = winAmounts[maxCount] || 0;
            
            if (win) {
                updateScore(win);
                updatePool(-win);
                elements.message.textContent = `${maxCount} eşleşme! ${win} puan kazandınız! 🎉`;
            } else {
                elements.message.textContent = "Tekrar deneyin!";
            }
        }

        // Diğer fonksiyonlar aynı kalabilir...
        
        // Temizlik fonksiyonu
        function cancelSpin() {
            cancelAnimationFrame(animationFrameId);
            document.body.classList.remove('spinning');
        }

        window.addEventListener('beforeunload', cancelSpin);
    </script>
</body>
</html>
