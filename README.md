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
            background-color: #222;
            color: white;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 400px;
            margin: auto;
            padding: 20px;
        }
        h1 {
            margin-bottom: 20px;
        }
        .slot-machine {
            display: flex;
            justify-content: center;
            margin: 20px 0;
        }
        .reel {
            width: 60px;
            height: 60px;
            border: 2px solid yellow;
            margin: 5px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
            background-color: black;
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
        }
        #spin {
            background-color: red;
            color: white;
        }
        #autoSpin {
            background-color: green;
            color: white;
        }
        #shareButton {
            background-color: #1da1f2;
            color: white;
        }
        .message {
            margin-top: 15px;
            font-size: 18px;
            font-weight: bold;
        }
        #score {
            font-size: 20px;
            color: yellow;
        }
        #leaderboard {
            margin-top: 20px;
        }
        #highScores {
            list-style-type: none;
            padding: 0;
        }
        @media (max-width: 600px) {
            .reel {
                width: 50px;
                height: 50px;
                font-size: 24px;
            }
            button {
                padding: 8px 16px;
                font-size: 14px;
            }
            h1 {
                font-size: 24px;
            }
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>TheGamblersslot-test</h1>

        <p>Puan: <span id="score">100</span></p>

        <div class="slot-machine">
            <div class="reel" id="reel1">🍒</div>
            <div class="reel" id="reel2">🍋</div>
            <div class="reel" id="reel3">🍊</div>
        </div>

        <div class="buttons">
            <button id="spin">SPIN</button>
            <button id="autoSpin">5 Spin Yap</button>
            <button id="shareButton">Sonucu Paylaş</button>
        </div>

        <select id="difficulty">
            <option value="easy">Kolay</option>
            <option value="medium">Orta</option>
            <option value="hard">Zor</option>
        </select>

        <select id="theme">
            <option value="dark">Karanlık Tema</option>
            <option value="retro">Retro Tema</option>
        </select>

        <p class="message" id="message">Spin'e bas ve şansını dene!</p>

        <div id="leaderboard">
            <h2>Lider Tablosu</h2>
            <ol id="highScores"></ol>
        </div>

        <!-- Ses Efektleri -->
        <audio id="backgroundMusic" src="background-music.mp3" loop></audio>
        <audio id="winSound" src="win-sound.mp3"></audio>
        <audio id="spinSound" src="spin-sound.mp3"></audio>
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

        // Ses çal
        function playSound(soundId) {
            let sound = document.getElementById(soundId);
            sound.currentTime = 0; // Sesi başa sar
            sound.play();
        }

        // Zorluk seviyesine göre sembolleri belirle
        function getSymbolsByDifficulty() {
            let difficulty = document.getElementById("difficulty").value;
            if (difficulty === "easy") {
                return ["🍒", "🍋", "🍊", "🍒", "🍋", "🍊", "🍒"];
            } else if (difficulty === "hard") {
                return ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"];
            } else {
                return symbols;
            }
        }

        // Spin işlemi
        function spinReels() {
            if (score < 10) {
                alert("Yeterli puanınız yok!");
                return;
            }

            updateScore(-10); // Her spin için 10 puan harca
            playSound("spinSound");

            let currentSymbols = getSymbolsByDifficulty();
            let reels = document.querySelectorAll(".reel");
            let reelResults = [];

            // Animasyonu başlat
            reels.forEach(reel => reel.style.animationPlayState = "running");

            setTimeout(() => {
                reels.forEach((reel, index) => {
                    let randomSymbol = currentSymbols[Math.floor(Math.random() * currentSymbols.length)];
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
                playSound("winSound");
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

        // Temayı değiştir
        document.getElementById("theme").addEventListener("change", function () {
            let theme = this.value;
            document.body.className = theme;

            if (theme === "retro") {
                document.body.style.backgroundColor = "#f0e68c";
                document.body.style.color = "#000";
            } else {
                document.body.style.backgroundColor = "#222";
                document.body.style.color = "#fff";
            }
        });

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

        // Arka plan müziği
        function playBackgroundMusic() {
            let music = document.getElementById("backgroundMusic");
            music.play();
        }

        playBackgroundMusic(); // Oyun başladığında müziği çal
    </script>

</body>
</html>
