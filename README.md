<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Balanced Slot Machine</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #222;
            color: white;
        }
        .container {
            max-width: 400px;
            margin: auto;
            padding: 20px;
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
        .message {
            margin-top: 15px;
            font-size: 18px;
            font-weight: bold;
        }
        .status {
            font-size: 18px;
            margin-top: 15px;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Balanced Slot Machine</h1>

        <div class="status">
            <p>Player Points: <span id="playerPoints">1000</span></p>
            <p>Reward Pool: <span id="rewardPool">10000000</span></p>
            <p>Potential Win: <span id="potentialWin">0</span></p>
        </div>

        <div class="slot-machine">
            <div class="reel" id="reel1">🍒</div>
            <div class="reel" id="reel2">🍋</div>
            <div class="reel" id="reel3">🍊</div>
        </div>

        <div class="buttons">
            <button id="spin">SPIN</button>
        </div>

        <p class="message" id="message">Press spin and test your luck!</p>
    </div>

    <script>
        const symbols = ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"];
        let playerPoints = 1000;
        let rewardPool = 10000000;

        function weightedRandom() {
            return symbols[Math.floor(Math.random() * symbols.length)];
        }

        function spinReels() {
            let reels = document.querySelectorAll(".reel");
            let result = [];

            // Select random symbols ensuring independent probability
            reels.forEach(reel => {
                let randomSymbol = weightedRandom();
                reel.innerText = randomSymbol;
                result.push(randomSymbol);
            });

            // Check for wins
            checkWin(result);
        }

        function checkWin(result) {
            let message = document.getElementById("message");
            let potentialWinDisplay = document.getElementById("potentialWin");

            if (result[0] === result[1] && result[1] === result[2]) {
                let matchCount = 3;
                let probability = (1 / (7 ** matchCount)) * 100;

                // Calculate dynamic win based on reward pool
                let winPercentage = matchCount === 3 ? 0.0001 : matchCount === 4 ? 0.01 : 1;
                let winAmount = Math.floor((rewardPool * winPercentage) / 100);

                if (winAmount > 0 && winAmount <= rewardPool) {
                    playerPoints += winAmount;
                    rewardPool -= winAmount;
                    document.getElementById("playerPoints").innerText = playerPoints;
                    document.getElementById("rewardPool").innerText = rewardPool;
                    potentialWinDisplay.innerText = winAmount;
                } else {
                    potentialWinDisplay.innerText = "0";
                }

                message.innerText = `Congratulations! You won with ${result[0]}! 🎉 (Win chance: ${probability.toFixed(5)}%)`;
                message.style.color = "yellow";
            } else {
                message.innerText = "Try again!";
                message.style.color = "white";
                potentialWinDisplay.innerText = "0";
            }
        }

        document.getElementById("spin").addEventListener("click", spinReels);
    </script>

</body>
</html>
