<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>tGt Slot Simulator</title>
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
        .status {
            font-size: 18px;
            margin-top: 15px;
            font-weight: bold;
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
            background: gold;
            color: black;
            box-shadow: 0 0 10px rgba(255, 215, 0, 0.8);
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>🎰 tGt Slot Simulator 🎰</h1>

        <div class="status">
            <p>Simulated Spins: <span id="spinCount">0</span></p>
            <p>Total Bets: <span id="totalBets">0</span></p>
            <p>Total Winnings: <span id="totalWinnings">0</span></p>
            <p>3-Match Wins: <span id="win3">0</span></p>
            <p>4-Match Wins: <span id="win4">0</span></p>
            <p>5-Match Wins (Jackpot): <span id="win5">0</span></p>
        </div>

        <div class="buttons">
            <button onclick="simulateSpins(10000)">Simulate 10,000 Spins</button>
        </div>
    </div>

    <script>
        const symbols = ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"];
        let spinCount = 0;
        let totalBets = 0;
        let totalWinnings = 0;
        let winCounts = { 3: 0, 4: 0, 5: 0 };

        function simulateSpins(numSpins) {
            for (let i = 0; i < numSpins; i++) {
                let bet = Math.floor(Math.random() * 50) + 1; // Random bet (1x - 50x)
                totalBets += bet;
                
                let result = [];
                for (let j = 0; j < 5; j++) {
                    result.push(symbols[Math.floor(Math.random() * symbols.length)]);
                }

                let counts = {};
                result.forEach(symbol => counts[symbol] = (counts[symbol] || 0) + 1);
                let maxMatch = Math.max(...Object.values(counts));

                let winAmount = 0;
                if (maxMatch === 3) {
                    winAmount = Math.floor((10000000 * 0.0001 * bet) / 100);
                    winCounts[3]++;
                } else if (maxMatch === 4) {
                    winAmount = Math.floor((10000000 * 0.01 * bet) / 100);
                    winCounts[4]++;
                } else if (maxMatch === 5) {
                    winAmount = Math.floor((10000000 * 1 * bet) / 100);
                    winCounts[5]++;
                }

                totalWinnings += winAmount;
                spinCount++;
            }

            updateUI();
        }

        function updateUI() {
            document.getElementById("spinCount").innerText = spinCount;
            document.getElementById("totalBets").innerText = totalBets;
            document.getElementById("totalWinnings").innerText = totalWinnings;
            document.getElementById("win3").innerText = winCounts[3];
            document.getElementById("win4").innerText = winCounts[4];
            document.getElementById("win5").innerText = winCounts[5];
        }
    </script>

</body>
</html>
