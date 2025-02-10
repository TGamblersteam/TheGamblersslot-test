<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fixed Slot Machine</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background: #1a1a1a;
            color: white;
            margin: 0;
            padding: 20px;
        }

        .slot-machine {
            max-width: 600px;
            margin: auto;
            padding: 20px;
            background: #222;
            border-radius: 15px;
            box-shadow: 0 0 15px rgba(255, 255, 0, 0.5);
        }

        .status {
            font-size: 18px;
            margin-bottom: 10px;
        }

        .reels {
            display: flex;
            justify-content: center;
            gap: 10px;
            position: relative;
        }

        .reel {
            width: 80px;
            height: 100px;
            overflow: hidden;
            background: black;
            border: 3px solid yellow;
            border-radius: 10px;
            position: relative;
        }

        .strip {
            position: absolute;
            width: 100%;
        }

        .symbol {
            height: 100px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 40px;
        }

        .controls {
            margin-top: 20px;
        }

        button {
            padding: 12px 25px;
            font-size: 18px;
            background: red;
            color: white;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s;
        }

        button:disabled {
            background: gray;
            cursor: not-allowed;
        }

        input {
            width: 80px;
            padding: 5px;
            font-size: 16px;
            text-align: center;
            border-radius: 10px;
            border: none;
        }

        @keyframes fireworks {
            0% { transform: scale(0); opacity: 1; }
            100% { transform: scale(20); opacity: 0; }
        }

        .firework {
            position: fixed;
            width: 10px;
            height: 10px;
            background: yellow;
            border-radius: 50%;
            animation: fireworks 1.5s ease-out;
        }

        @keyframes explosion {
            0% { transform: scale(1); opacity: 1; }
            100% { transform: scale(10); opacity: 0; }
        }

        .explosion {
            position: fixed;
            width: 20px;
            height: 20px;
            background: red;
            border-radius: 50%;
            animation: explosion 1s ease-out;
        }
    </style>
</head>
<body>

    <div class="slot-machine">
        <h1>Fixed Slot Machine</h1>

        <div class="status">
            <p>Points: <span id="playerPoints">1000</span></p>
            <p>Reward Pool: <span id="rewardPool">10000000</span></p>
        </div>

        <div>
            <label for="betAmount">Bet (1-50x): </label>
            <input type="number" id="betAmount" min="1" max="50" value="1">
        </div>

        <div class="reels">
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
            <div class="reel"><div class="strip"></div></div>
        </div>

        <div class="controls">
            <button id="spin">SPIN</button>
        </div>

        <div id="message" class="status"></div>
    </div>

    <script>
        const symbols = ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"];
        let playerPoints = 1000;
        let rewardPool = 10000000;
        let isSpinning = false;

        const elements = {
            playerPoints: document.getElementById("playerPoints"),
            rewardPool: document.getElementById("rewardPool"),
            message: document.getElementById("message"),
            spinBtn: document.getElementById("spin"),
            betAmount: document.getElementById("betAmount"),
            reels: document.querySelectorAll(".reel .strip")
        };

        function initializeStrips() {
            elements.reels.forEach(strip => {
                let html = "";
                for (let i = 0; i < 30; i++) {
                    html += `<div class="symbol">${symbols[i % symbols.length]}</div>`;
                }
                strip.innerHTML = html;
                strip.style.top = `-${Math.floor(Math.random() * 20) * 100}px`;
            });
        }

        function spinReels() {
            if (isSpinning) return;

            let bet = parseInt(elements.betAmount.value);
            if (isNaN(bet) || bet < 1 || bet > 50 || bet > playerPoints) {
                elements.message.textContent = "Invalid bet!";
                return;
            }

            isSpinning = true;
            elements.spinBtn.disabled = true;
            playerPoints -= bet;
            rewardPool += bet;
            updateUI();

            elements.reels.forEach((strip, index) => {
                setTimeout(() => {
                    spinAnimation(strip, () => {
                        if (index === elements.reels.length - 1) {
                            checkWin(bet);
                            isSpinning = false;
                            elements.spinBtn.disabled = false;
                        }
                    });
                }, index * 600);
            });
        }

        function spinAnimation(strip, callback) {
            let position = Math.floor(Math.random() * 20) * 100;
            strip.style.transition = "none";
            strip.style.top = "-2000px";

            setTimeout(() => {
                strip.style.transition = "top 1s ease-out";
                strip.style.top = `-${position}px`;
                setTimeout(callback, 1000);
            }, 50);
        }

        function checkWin(bet) {
            let results = Array.from(elements.reels).map(strip => strip.children[3].textContent);
            let count = results.reduce((acc, val) => (acc[val] = (acc[val] || 0) + 1, acc), {});
            let maxCount = Math.max(...Object.values(count));
            let winPercentage = maxCount === 3 ? 0.0001 * bet : maxCount === 4 ? 0.01 * bet : maxCount === 5 ? 1 * bet : 0;
            let winAmount = Math.floor(rewardPool * winPercentage);

            if (winAmount > 0) {
                playerPoints += winAmount;
                rewardPool -= winAmount;
                elements.message.textContent = `🎉 You won ${winAmount} points! 🎉`;
                showEffect("firework");
            } else {
                elements.message.textContent = "💥 BOOM! You lost!";
                showEffect("explosion");
            }
            updateUI();
        }

        function updateUI() {
            elements.playerPoints.textContent = playerPoints;
            elements.rewardPool.textContent = rewardPool;
        }

        function showEffect(type) {
            let effect = document.createElement("div");
            effect.className = type;
            effect.style.left = `${Math.random() * window.innerWidth}px`;
            effect.style.top = `${Math.random() * window.innerHeight}px`;
            document.body.appendChild(effect);
            setTimeout(() => effect.remove(), 1500);
        }

        elements.spinBtn.addEventListener("click", spinReels);
        initializeStrips();
    </script>

</body>
</html>


