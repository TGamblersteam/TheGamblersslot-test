<!DOCTYPE html><html lang="en">
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
<body><div class="container">
    <h1>TheGambler DAppSlot Machine</h1>

    <div class="status">
        <p>tGt: <span id="playerPoints">1000</span></p>
        <p>tGt Pool: <span id="rewardPool">10000000</span></p>
        <p>Potential Win: <span id="potentialWin">0</span></p>
        <p>Bet Amount: <input type="number" id="betAmount" min="1" max="100" value="1" oninput="calculatePotentialWin()"></p>
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

    function calculatePotentialWin() {
        let bet = parseInt(document.getElementById("betAmount").value);
        let potentialWinDisplay = document.getElementById("potentialWin");
        if (isNaN(bet) || bet < 1 || bet > 100) {
            potentialWinDisplay.innerText = 0;
            return;
        }
        let win3 = Math.floor(rewardPool * 0.00005 * bet);
        let win4 = Math.floor(rewardPool * 0.005 * bet);
        let win5 = Math.floor(rewardPool * 0.5 * bet);
        potentialWinDisplay.innerText = `3-match: ${win3} tGt, 4-match: ${win4} tGt, 5-match: ${win5} tGt`;
    }

    function checkWin(result, bet) {
        let message = document.getElementById("message");
        let counts = {};
        result.forEach(symbol => counts[symbol] = (counts[symbol] || 0) + 1);
        let maxMatch = Math.max(...Object.values(counts));
        let winAmount = maxMatch === 3 ? Math.floor(rewardPool * 0.00005 * bet) : maxMatch === 4 ? Math.floor(rewardPool * 0.005 * bet) : maxMatch === 5 ? Math.floor(rewardPool * 0.5 * bet) : 0;

        if (winAmount > 0) {
            playerPoints += winAmount;
            rewardPool -= winAmount;
            document.getElementById("playerPoints").innerText = playerPoints;
            document.getElementById("rewardPool").innerText = rewardPool;
            message.innerText = `🎉 Congratulations! You won ${winAmount} tGt! 🎉`;
        } else {
            message.innerText = "Try again!";
        }
    }
</script>

</body>
</html>
