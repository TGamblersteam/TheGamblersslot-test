<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gelişmiş Slot Makinesi</title>
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
        .message {
            margin-top: 15px;
            font-size: 18px;
            font-weight: bold;
        }
        #score {
            font-size: 20px;
            color: yellow;
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
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Gelişmiş Slot Makinesi</h1>

        <p>Puan: <span id="score">100</span></p>

        <div class="slot-machine">
            <div class="reel" id="reel1">🍒</div>
            <div class="reel" id="reel2">🍋</div>
            <div class="reel" id="reel3">🍊</div>
        </div>

        <div class="buttons">
            <button id="spin">SPIN</button>
            <button id="autoSpin">5 Spin Yap</button>
        </div>

        <select id="difficulty">
            <option value="easy">Kolay</option>
            <option value="medium">Orta</option>
            <option value="hard">Zor</option>
        </select>

        <p class="message" id="message">Spin'e bas ve şansını dene!</p>

        <!-- Ses Efektleri -->
        <audio id="winSound" src="win-sound.mp3"></audio>
        <audio id="spinSound" src="spin-sound.mp3"></audio>
    </div>

    <script>
        const symbols = ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"];
        let score = 100;

        function updateScore(points) {
            score += points;
            document.getElementById("score").innerText = score;
        }

        function playSound(soundId) {
            let sound = document.getElementById(soundId);
            sound.currentTime = 0; // Sesi başa sar
            sound.play();
        }

        function getSymbolsByDifficulty() {
            let difficulty = document.getElementById("difficulty").value;
            if (difficulty === "easy") {
                return ["🍒", "🍋", "🍊", "🍒", "🍋", "🍊", "🍒"]; // Daha fazla kazanma şansı
            } else if (difficulty === "hard") {
                return ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"]; // Daha az kazanma şansı
            } else {
                return symbols; // Varsayılan
            }
        }

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

        function checkWin(reelResults) {
            let message = document.getElementById("message");

            if (reelResults[0] === "🍎" && reelResults[1] === "🍎" && reelResults[2] === "🍎") {
                updateScore(1000); // Jackpot!
                playSound("winSound");
                message.innerText = "JACKPOT! 1000 puan kazandınız! 🎉🎉🎉";
                message.style.color = "yellow";
            } else if (reelResults[0] === reelResults[1] && reelResults[1] === reelResults[2]) {
                let winAmount = 0;
                switch (reelResults[0]) {
                    case "🍒":
                        winAmount = 50;
                        break;
                    case "🍋":
                        winAmount = 30;
                        break;
                    case "🍊":
                        winAmount = 20;
                        break;
                    default:
                        winAmount = 10;
                }
                updateScore(winAmount);
                playSound("winSound");
                message.innerText = `Tebrikler! ${reelResults[0]} ile ${winAmount} puan kazandınız! 🎉`;
                message.style.color = "yellow";
            } else if (reelResults[0] === reelResults[1] || reelResults[1] === reelResults[2] || reelResults[0] === reelResults[2]) {
                updateScore(10); // 2 sembol eşleşirse 10 puan
                message.innerText = "İki sembol eşleşti! 10 puan kazandınız!";
                message.style.color = "white";
            } else {
                message.innerText = "Şansını tekrar dene!";
                message.style.color = "white";
            }
        }

        document.getElementById("spin").addEventListener("click", spinReels);

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
