<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>tGt Slot Machine</title>
    <style>
        body { 
            font-family: Arial, sans-serif; 
            text-align: center; 
            background: linear-gradient(to bottom, #FFD700, #FFA500); 
            color: black; 
        }
        .container {
            max-width: 600px;
            margin: auto;
            padding: 20px;
            background: #333;
            border-radius: 15px;
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.8);
            color: white;
        }
        h1 {
            color: #FFD700;
            text-shadow: 2px 2px 10px rgba(255, 255, 0, 0.8);
        }
        .slot-machine {
            display: flex;
            justify-content: center;
            margin: 20px 0;
            background: #222;
            padding: 10px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(255, 215, 0, 0.5);
        }
        .reel {
            width: 80px;
            height: 80px;
            border: 3px solid gold;
            margin: 5px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 40px;
            background-color: black;
            color: white;
            transition: transform 1s ease-out, box-shadow 0.5s ease-in-out;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(255, 215, 0, 0.6);
        }
        .reel.spinning {
            animation: spinEffect 0.6s infinite;
        }
        @keyframes spinEffect {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }
        .buttons {
            margin-top: 20px;
        }
        button {
            padding: 12px 30px;
            font-size: 18px;
            margin: 5px;
            cursor: pointer;
            border: none;
            border-radius: 10px;
            font-weight: bold;
        }
        #spin {
            background: gold;
            color: black;
            box-shadow: 0 0 10px rgba(255, 215, 0, 0.8);
        }
        .message {
            margin-top: 15px;
            font-size: 20px;
            font-weight: bold;
            color: white;
            text-shadow: 2px 2px 8px gold;
        }
        .status {
            font-size: 18px;
            margin-top: 15px;
            font-weight: bold;
        }
        .glow {
            animation: glowEffect 1s infinite alternate;
        }
        @keyframes glowEffect {
            0% { box-shadow: 0 0 10px rgba(255, 215, 0, 0.6); }
            100% { box-shadow: 0 0 20px rgba(255, 215, 0, 1); }
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>🎰 tGt Slot 🎰</h1>

        <div class="status">
            <p>Player Points: <span id="playerPoints">1000</span></p>
            <p>Reward Pool: <span id="rewardPool">10000000</span></p>
            <p>Potential Win: <span id="potentialWin">0</span></p>
            <p>Bet Amount: <input type="number" id="betAmount" min="1" max="50" value="1" oninput="updatePotentialWin()"></p>
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

        function updatePotentialWin() {
            let bet = parseInt(document.getElementById("betAmount").value);
            if (isNaN(bet) || bet < 1 || bet > 50) {
                document.getElementById("potentialWin").innerText = "Invalid bet!";
                return;
            }

            let potentialWin3 = Math.floor((rewardPool * 0.0001 * bet) / 100);
            let potentialWin4 = Math.floor((rewardPool * 0.01 * bet) / 100);
            let potentialWin5 = Math.floor((rewardPool * 1 * bet) / 100);

            document.getElementById("potentialWin").innerText = 
                `3 Match: ${potentialWin3} | 4 Match: ${potentialWin4} | 5 Match: ${potentialWin5}`;
        }

        function spinReels() {
            let reels = document.querySelectorAll(".reel");
            let result = [];
            let bet = parseInt(document.getElementById("betAmount").value);

            if (isNaN(bet) || bet < 1 || bet > 50 || bet > playerPoints) {
                document.getElementById("message").innerText = "Invalid bet amount!";
                return;
            }

            playerPoints -= bet;
            rewardPool += bet;
            document.getElementById("playerPoints").innerText = playerPoints;
            document.getElementById("rewardPool").innerText = rewardPool;

            reels.forEach((reel, index) => {
                reel.classList.add("spinning");
                setTimeout(() => {
                    let randomSymbol = weightedRandom();
                    reel.innerText = randomSymbol;
                    reel.classList.remove("spinning");
                    result.push(randomSymbol);
                    if (index === reels.length - 1) checkWin(result, bet);
                }, index * 500);
            });
        }

        document.getElementById("spin").addEventListener("click", spinReels);
        updatePotentialWin();
    </script>

</body>
</html>
