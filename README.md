<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>5 Reel Slot Game</title>
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
            max-width: 800px;
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
            height: 150px;
            overflow: hidden;
            background: black;
            border: 3px solid yellow;
            border-radius: 10px;
            position: relative;
        }

        .strip {
            position: absolute;
            width: 100%;
            transition: transform 1s ease-out;
        }

        .symbol {
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
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

        .win {
            animation: blink 0.5s infinite alternate;
        }

        @keyframes blink {
            0%, 100% { color: yellow; }
            50% { color: white; }
        }
    </style>
</head>
<body>

    <div class="slot-machine">
        <h1>5 Reel Slot Machine</h1>
        
        <div class="status">
            <p>Points: <span id="playerPoints">1000</span></p>
            <p>Reward Pool: <span id="rewardPool">10000000</span></p>
        </div>

        <div>
            <label for="betAmount">Bet (1-500): </label>
            <input type="number" id="betAmount" min="1" max="500" value="1">
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
        const SYMBOL_HEIGHT = 50;
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
                for (let i = 0; i < 9; i++) {
                    html += `<div class="symbol">${symbols[i % symbols.length]}</div>`;
                }
                strip.innerHTML = html;
            });
        }

        function updatePoints(amount) {
            playerPoints = Math.max(0, playerPoints + amount);
            elements.playerPoints.textContent = playerPoints;
        }

        function updateRewardPool(amount) {
            rewardPool = Math.max(0, rewardPool + amount);
            elements.rewardPool.textContent = rewardPool;
        }

        function spinReels() {
            if (isSpinning) return;

            let bet = parseInt(elements.betAmount.value);
            if (isNaN(bet) || bet < 1 || bet > 500 || bet > playerPoints) {
                elements.message.textContent = "Invalid bet!";
                return;
            }

            isSpinning = true;
            elements.spinBtn.disabled = true;
            updatePoints(-bet);
            updateRewardPool(bet);

            let results = [];
            elements.reels.forEach((strip, index) => {
                const randomIndex = Math.floor(Math.random() * symbols.length);
                const targetY = -(randomIndex * SYMBOL_HEIGHT);
                
                strip.style.transition = `transform ${1.5 + index * 0.1}s ease-out`;
                strip.style.transform = `translateY(${targetY}px)`;

                results.push(symbols[randomIndex]);
            });

            setTimeout(() => {
                calculateWin(results, bet);
                isSpinning = false;
                elements.spinBtn.disabled = false;
            }, 2000);
        }

        function calculateWin(results, bet) {
            const counts = results.reduce((acc, val) => {
                acc[val] = (acc[val] || 0) + 1;
                return acc;
            }, {});

            let maxCount = Math.max(...Object.values(counts));
            let winPercentage = 0;

            if (maxCount === 3) winPercentage = 0.0001 * bet;
            else if (maxCount === 4) winPercentage = 0.01 * bet;
            else if (maxCount === 5) winPercentage = 1 * bet;

            let winAmount = Math.floor((rewardPool * winPercentage) / 100);
            if (winAmount > 0) {
                updatePoints(winAmount);
                updateRewardPool(-winAmount);
                elements.message.innerHTML = `🎉 You won ${winAmount} points! 🎉`;
                elements.message.classList.add("win");
            } else {
                elements.message.textContent = "Try again!";
                elements.message.classList.remove("win");
            }
        }

        elements.spinBtn.addEventListener("click", spinReels);
        initializeStrips();
    </script>

</body>
</html>

