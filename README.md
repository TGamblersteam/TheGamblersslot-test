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
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .slot-machine {
            background: linear-gradient(145deg, #1e1e1e, #2c2c2c);
            border: 10px solid #ffcc00;
            border-radius: 20px;
            padding: 20px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
            width: 600px;
        }
        .reels {
            display: flex;
            justify-content: center;
            margin: 20px 0;
        }
        .reel {
            width: 80px;
            height: 120px;
            border: 2px solid #ffcc00;
            margin: 5px;
            overflow: hidden;
            position: relative;
            background-color: #000;
            border-radius: 10px;
            box-shadow: inset 0 0 10px rgba(255, 204, 0, 0.5);
        }
        .reel .strip {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            transition: top 0.5s ease-out;
        }
        .reel .strip div {
            height: 120px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 40px;
        }
        .buttons {
            margin-top: 20px;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            margin: 5px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            background-color: #ff4444;
            color: white;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
            transition: background-color 0.3s, transform 0.2s;
        }
        button:hover {
            background-color: #cc0000;
            transform: scale(1.05);
        }
        button:active {
            transform: scale(0.95);
        }
        #autoSpin {
            background-color: #00cc66;
        }
        #autoSpin:hover {
            background-color: #00994d;
        }
        #shareButton {
            background-color: #1da1f2;
        }
        #shareButton:hover {
            background-color: #0d8bf0;
        }
        .message {
            margin-top: 15px;
            font-size: 18px;
            font-weight: bold;
            color: #ffcc00;
        }
        #score {
            font-size: 20px;
            color: #ffcc00;
            margin-bottom: 10px;
        }
        #pool {
            font-size: 20px;
            color: #ffcc00;
            margin-bottom: 10px;
        }
        #leaderboard {
            margin-top: 20px;
            background: rgba(0, 0, 0, 0.5);
            padding: 10px;
            border-radius: 10px;
        }
        #highScores {
            list-style-type: none;
            padding: 0;
        }
        #highScores li {
            margin: 5px 0;
            font-size: 16px;
        }
        @media (max-width: 600px) {
            .slot-machine {
                width: 90%;
            }
            .reel {
                width: 60px;
                height: 90px;
            }
            .reel .strip div {
                height: 90px;
                font-size: 30px;
            }
            button {
                padding: 8px 16px;
                font-size: 14px;
            }
        }
    </style>
</head>
<body>

    <div class="slot-machine">
        <h1>TheGamblersslot-test</h1>

        <p>Puan: <span id="score">100</span></p>
        <p>Puan Havuzu: <span id="pool">500000</span></p>

        <div class="reels">
            <div class="reel" id="reel1">
                <div class="strip">
                    <div>🍒</div>
                    <div>🍋</div>
                    <div>🍊</div>
                    <div>🍉</div>
                    <div>🍎</div>
                    <div>🍇</div>
                    <div>🍌</div>
                </div>
            </div>
            <div class="reel" id="reel2">
                <div class="strip">
                    <div>🍒</div>
                    <div>🍋</div>
                    <div>🍊</div>
                    <div>🍉</div>
                    <div>🍎</div>
                    <div>🍇</div>
                    <div>🍌</div>
                </div>
            </div>
            <div class="reel" id="reel3">
                <div class="strip">
                    <div>🍒</div>
                    <div>🍋</div>
                    <div>🍊</div>
                    <div>🍉</div>
                    <div>🍎</div>
                    <div>🍇</div>
                    <div>🍌</div>
                </div>
            </div>
            <div class="reel" id="reel4">
                <div class="strip">
                    <div>🍒</div>
                    <div>🍋</div>
                    <div>🍊</div>
                    <div>🍉</div>
                    <div>🍎</div>
                    <div>🍇</div>
                    <div>🍌</div>
                </div>
            </div>
            <div class="reel" id="reel5">
                <div class="strip">
                    <div>🍒</div>
                    <div>🍋</div>
                    <div>🍊</div>
                    <div>🍉</div>
                    <div>🍎</div>
                    <div>🍇</div>
                    <div>🍌</div>
                </div>
            </div>
        </div>

        <div class="buttons">
            <button id="spin">SPIN</button>
            <button id="autoSpin">5 Spin Yap</button>
            <button id="shareButton">Sonucu Paylaş</button>
        </div>

        <p class="message" id="message">Spin'e bas ve şansını dene!</p>

        <div id="leaderboard">
            <h2>Lider Tablosu</h2>
            <ol id="highScores"></ol>
        </div>
    </div>

    <script>
        const symbols = ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"];
        let score = 100;
        let pool = 500000;
        let highScores = JSON.parse(localStorage.getItem("highScores")) || [];

        // Puanı güncelle
        function updateScore(points) {
            score += points;
            document.getElementById("score").innerText = score;
        }

        // Puan havuzunu güncelle
        function updatePool(points) {
            pool += points;
            document.getElementById("pool").innerText = pool;
        }

        // Reel'leri döndür
        function spinReels() {
            if (score < 10) {
                alert("Yeterli puanınız yok!");
                return;
            }

            updateScore(-10); // Her spin için 10 puan harca
            updatePool(10); // Kaybedilen puanlar havuzda birikir

            let reels = document.querySelectorAll(".reel .strip");
            let reelResults = [];

            reels.forEach((strip, index) => {
                let randomPosition = Math.floor(Math.random() * symbols.length) * -120; // Rastgele pozisyon
                strip.style.top = `${randomPosition}px`;

                // Durdurulan sembolü belirle
                let visibleIndex = Math.abs(randomPosition / 120) % symbols.length;
                reelResults.push(symbols[visibleIndex]);
            });

            // Kazanç kontrolü
            setTimeout(() => checkWin(reelResults), 1000); // Animasyon bittikten sonra kontrol et
        }

        // Kazanç kontrolü
        function checkWin(reelResults) {
            let message = document.getElementById("message");

            // Aynı sembollerin sayısını kontrol et
            let counts = {};
            reelResults.forEach(symbol => {
                counts[symbol] = (counts[symbol] || 0) + 1;
            });

            let maxCount = Math.max(...Object.values(counts));
            let winAmount = 0;

            if (maxCount >= 3) {
                switch (maxCount) {
                    case 3:
                        winAmount = 10;
                        break;
                    case 4:
                        winAmount = 100;
                        break;
                    case 5:
                        winAmount = 1000;
                        break;
                }
                updateScore(winAmount);
                updatePool(-winAmount); // Kazanılan puan havuzdan düşülür
                message.innerText = `Tebrikler! ${maxCount} aynı sembol ile ${winAmount} puan kazandınız! 🎉`;
                message.style.color = "yellow";
            } else {
                message.innerText = "Şansını tekrar dene!";
                message.style.color = "white";
            }
        }

        // Lider tablosunu güncelle
        function updateLeaderboard() {
            let highScoresList = document.getElementById("highScores");
            highScoresList.innerHTML = "";

            highScores.sort((a, b) => b - a).slice(0, 5).forEach((score, index) => {
                let li = document.createElement("li");
                li.innerText = `${index + 1}. ${score} puan`;
                highScoresList.appendChild(li);
            });
        }

        // Puanı kaydet
        function saveScore() {
            highScores.push(score);
            localStorage.setItem("highScores", JSON.stringify(highScores));
            updateLeaderboard();
        }

        // Oyun sonu
        function gameOver() {
            saveScore();
            alert("Oyun bitti! Puanınız lider tablosuna eklendi.");
        }

        // Sosyal medya paylaşımı
        document.getElementById("shareButton").addEventListener("click", function () {
            let shareText = `Slot oyununda ${score} puan kazandım! 🎉`;
            let shareUrl = window.location.href;
            window.open(`https://twitter.com/intent/tweet?text=${encodeURIComponent(shareText)}&url=${encodeURIComponent(shareUrl)}`, "_blank");
        });

        // Spin butonu
        document.getElementById("spin").addEventListener("click", spinReels);

        // Otomatik spin
        document.getElementById("autoSpin").addEventListener("click", function () {
            let spinsLeft = 5;
            let interval = setInterval(() => {
                if (spinsLeft > 0) {
                    spinReels();
                    spinsLeft--;
                } else {
                    clearInterval(interval);
                }
            }, 1500); // Her 1.5 saniyede bir spin
        });
    </script>

</body>
</html>
