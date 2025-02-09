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
            width: 400px;
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
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 40px;
            background-color: #000;
            border-radius: 10px;
            box-shadow: inset 0 0 10px rgba(255, 204, 0, 0.5);
            animation: spin 0.5s linear infinite;
            animation-play-state: paused;
        }
        @keyframes spin {
            0% { transform: rotateX(0deg); }
            100% { transform: rotateX(360deg); }
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

        <div class="reels">
            <div class="reel" id="reel1">🍒</div>
            <div class="reel" id="reel2">🍋</div>
            <div class="reel" id="reel3">🍊</div>
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
        const symbols = ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌", "🌟"]; // Wild sembolü eklendi
        let score = 100;
        let highScores = JSON.parse(localStorage.getItem("highScores")) || [];

        // Puanı güncelle
        function updateScore(points) {
            score += points;
            document.getElementById("score").innerText = score;
        }

        // Spin işlemi
        function spinReels() {
            if (score < 10) {
                alert("Yeterli puanınız yok!");
                return;
            }

            updateScore(-10); // Her spin için 10 puan harca

            let reels = document.querySelectorAll(".reel");
            let reelResults = [];

            // Animasyonu başlat
            reels.forEach(reel => reel.style.animationPlayState = "running");

            setTimeout(() => {
                reels.forEach((reel, index) => {
                    let randomSymbol = symbols[Math.floor(Math.random() * symbols.length)];
                    reel.innerText = randomSymbol;
                    reelResults.push(randomSymbol);
                    reel.style.animationPlayState = "paused"; // Animasyonu durdur
                });

                checkWin(reelResults);
            }, 1000); // 1 saniye sonra sonuçları göster
        }

        // Kazanç kontrolü
        function checkWin(reelResults) {
            let winAmount = 0;
            let message = document.getElementById("message");

            // Wild sembolü kontrolü
            if (reelResults.includes("🌟")) {
                let nonWildSymbols = reelResults.filter(symbol => symbol !== "🌟");
                if (nonWildSymbols.length >= 2 && nonWildSymbols[0] === nonWildSymbols[1]) {
                    winAmount = getWinAmount(nonWildSymbols[0]);
                }
            }
            // Yatay satır kontrolü
            else if (reelResults[0] === reelResults[1] && reelResults[1] === reelResults[2]) {
                winAmount = getWinAmount(reelResults[0]);

                // Bonus tur tetikleme
                if (reelResults[0] === "🍉") {
                    startBonusRound();
                    return;
                }
            }

            if (winAmount > 0) {
                updateScore(winAmount);
                message.innerText = `Tebrikler! ${winAmount} puan kazandınız! 🎉`;
                message.style.color = "yellow";
            } else {
                message.innerText = "Şansını tekrar dene!";
                message.style.color = "white";
            }
        }

        // Kazanç miktarını belirle
        function getWinAmount(symbol) {
            switch (symbol) {
                case "🍒": return 50;
                case "🍋": return 30;
                case "🍊": return 20;
                default: return 10;
            }
        }

        // Bonus tur
        function startBonusRound() {
            alert("Bonus Turu Kazandınız! Ekstra 100 puan kazandınız.");
            updateScore(100);
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
