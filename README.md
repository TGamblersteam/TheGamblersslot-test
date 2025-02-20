<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TheGambler DAppSlot Machine</title>
    <style>
        body { 
            font-family: Arial, sans-serif; 
            text-align: center; 
            background-color: #654321; 
            color: white; 
        }
        .container {
            max-width: 500px;
            margin: auto;
            padding: 20px;
            background: url('https://www.transparenttextures.com/patterns/wood.png');
            border: 10px solid #8B4513;
            border-radius: 15px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);
        }
        .slot-machine {
            display: flex;
            justify-content: center;
            margin: 20px 0;
            background: #A0522D;
            padding: 15px;
            border-radius: 10px;
        }
        .reel {
            width: 60px;
            height: 60px;
            border: 2px solid gold;
            margin: 5px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
            background-color: black;
            transition: transform 1s ease-out;
            border-radius: 5px;
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
            background-color: #D2691E;
            color: white;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.5);
        }
        #spin {
            background-color: red;
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
        <h1>TheGambler DAppSlot Machine</h1>

        <div class="status">
            <p>tGt: <span id="playerPoints">1000</span></p>
            <p>tGt Pool: <span id="rewardPool">10000000</span></p>
            <p>Potential Win: <span id="potentialWin">0</span></p>
            <p>Bet Amount: <input type="number" id="betAmount" min="1" max="100" value="1"></p>
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

    <audio id="spinSound" src="https://www.soundjay.com/button/beep-07.wav"></audio>
    <audio id="winSound" src="https://www.soundjay.com/button/beep-08b.wav"></audio>
    <audio id="loseSound" src="https://www.soundjay.com/button/beep-05.wav"></audio>

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
            document.getElementById("spinSound").play();

            if (isNaN(bet) || bet < 1 || bet > 100 || bet > playerPoints) {
                document.getElementById("message").innerText = "Invalid bet amount!";
                return;
            }

            playerPoints -= bet;
            rewardPool += bet;
            document.getElementById("playerPoints").innerText = playerPoints;
            document.getElementById("rewardPool").innerText = rewardPool;

            reels.forEach((reel, index) => {
                setTimeout(() => {
                    let randomSymbol = weightedRandom();
                    reel.style.transform = "rotateX(360deg)";
                    setTimeout(() => {
                        reel.innerText = randomSymbol;
                        reel.style.transform = "rotateX(0deg)";
                        result.push(randomSymbol);
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
            let winAmount = maxMatch >= 3 ? bet * maxMatch : 0;
            potentialWinDisplay.innerText = winAmount;

            if (winAmount > 0) {
                playerPoints += winAmount;
                rewardPool -= winAmount;
                document.getElementById("playerPoints").innerText = playerPoints;
                document.getElementById("rewardPool").innerText = rewardPool;
                document.getElementById("winSound").play();
                message.innerText = `🎉 Congratulations! You won ${winAmount} tGt! 🎉`;
            } else {
                document.getElementById("loseSound").play();
                message.innerText = "Try again!";
            }
        }

        document.getElementById("spin").addEventListener("click", spinReels);
    </script>

</body>
</html>


