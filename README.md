<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>5-Reel Slot Machine</title>
    <style>
        body { 
            font-family: Arial, sans-serif; 
            text-align: center; 
            background-color: #222; 
            color: white; 
        }
        .container {
            max-width: 500px;
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
            transition: transform 1s ease-out;
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
        <h1>5-Reel Slot Machine</h1>

        <div class="status">
            <p>Player Points: <span id="playerPoints">1000</span></p>
            <p>Reward Pool: <span id="rewardPool">10000000</span></p>
            <p>Potential Win: <span id="potentialWin">0</span></p>
            <p>Bet Amount: <input type="number" id="betAmount" min="1" max="50" value="1"></p>
        </div>

        <div class="slot-machine">
            <div class="reel" id="reel1">🍒</div>
            <div class="reel" id="reel2">🍋</div>
            <div class="reel" id="reel3">🍊</div>
            <div class="reel" id="reel4">🍉</div>
            <div class="reel" id="reel5">🍎</div>
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
            let bet = parseInt(document.getElementById("betAmount").value);

            if (isNaN(bet) || bet < 1 || bet > 50 || bet > playerPoints) {
                document.getElementById("message").innerText = "Invalid bet amount!";
                return;
            }

            // Deduct bet from player and add to reward pool
            playerPoints -= bet;
            rewardPool += bet;
            document.getElementById("playerPoints").innerText = playerPoints;
            document.getElementById("rewardPool").innerText = rewardPool;

            // Realistic reel spin effect with staggered stopping
            reels.forEach((reel, index) => {
                setTimeout(() => {
                    let randomSymbol = weightedRandom();
                    reel.style.transform = "rotateX(360deg)";
                    setTimeout(() => {
                        reel.innerText = randomSymbol;
                        reel.style.transform = "rotateX(0deg)";
                        result.push(randomSymbol);

                        // Check win only after the last reel stops
                        if (index === reels.length - 1) {
                            checkWin(result, bet);
                        }
                    }, 500);
                }, index * 400);
            });
        }

        function checkWin(result, bet) {
            let message = document.getElementById("message");
            let potentialWinDisplay = document.getElementById("potentialWin");

            let counts = {};
            result.forEach(symbol => {
                counts[symbol] = (counts[symbol] || 0) + 1;
            });

            let maxMatch = Math.max(...Object.values(counts));
            let winPercentage = 0;

            if (maxMatch === 3) {
                winPercentage = 0.0001 * bet;
            } else if (maxMatch === 4) {
                winPercentage = 0.01 * bet;
            } else if (maxMatch === 5) {
                winPercentage = 1 * bet;
            }

            let winAmount = Math.floor((rewardPool * winPercentage) / 100);

            if (winAmount > 0 && winAmount <= rewardPool) {
                playerPoints += winAmount;
                rewardPool -= winAmount;
                document.getElementById("playerPoints").innerText = playerPoints;
                document.getElementById("rewardPool").innerText = rewardPool;
                potentialWinDisplay.innerText = winAmount;
                message.innerText = `🎉 Congratulations! You won ${winAmount} points! 🎉`;
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
