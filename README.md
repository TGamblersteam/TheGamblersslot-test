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
            transition: top 1s ease-out;
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
            strip.style.transition = "top 1s ease-out";
            strip.style.top = `-${position}px`;
            setTimeout(callback, 1000);
        }

        function checkWin(bet) {
            let results = Array.from(elements.reels).map(strip => {
                let topValue = parseInt(strip.style.top.replace("px", ""));
                let finalSymbolIndex = Math.abs(topValue / 100) % symbols.length;
                return symbols[finalSymbolIndex];
            });

            let count = results.reduce((acc, val) => (acc[val] = (acc[val] || 0) + 1, acc), {});
            let maxCount = Math.max(...Object.values(count));

            let winPercentage = 0;
            if (maxCount === 3) winPercentage = 0.0001 * bet;
            else if (maxCount === 4) winPercentage = 0.01 * bet;
            else if (maxCount === 5) winPercentage = 1 * bet;

            let winAmount = Math.floor((rewardPool * winPercentage) / 100);
            if (winAmount > 0 && winAmount <= rewardPool) {
                playerPoints += winAmount;
                rewardPool -= winAmount;
                elements.message.textContent = `🎉 You won ${winAmount} points! 🎉`;
            } else {
                elements.message.textContent = "💥 BOOM! You lost!";
            }
            updateUI();
        }

        function updateUI() {
            elements.playerPoints.textContent = playerPoints;
            elements.rewardPool.textContent = rewardPool;
        }

        elements.spinBtn.addEventListener("click", spinReels);
        initializeStrips();
    </script>

</body>
</html>
