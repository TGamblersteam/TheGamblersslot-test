<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TheGambler DAppSlot</title>
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
            transition: transform 1s ease-out;
            border-radius: 10px;
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
    </style>
</head>
<body>
    <div class="container">
        <h1>🎰 TheGambler DAppSlot 🎰</h1>

        <p>tGt: <span id="tGtBalance">1000</span></p>
        <p>tGt Pool: <span id="rewardPool">10000000</span></p>
        <p>Potential Win: <span id="potentialWin">0</span></p>
        <p>Bet Amount: <input type="number" id="betAmount" min="1" max="100" value="1" oninput="updatePotentialWin()"></p>

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
        function updatePotentialWin() {
    let bet = parseInt(document.getElementById("betAmount").value);
    let rewardPool = parseInt(document.getElementById("rewardPool").innerText);

    if (isNaN(bet) || bet < 1 || bet > 100) {
        document.getElementById("potentialWin").innerText = "Invalid Bet!";
        return;
    }

    let potentialWin3 = (rewardPool * 0.00005 * bet) / 100;
    let potentialWin4 = (rewardPool * 0.005 * bet) / 100;
    let potentialWin5 = (rewardPool * 0.5 * bet) / 100;

    document.getElementById("potentialWin").innerText = `3 Match: ${potentialWin3.toFixed(2)} | 4 Match: ${potentialWin4.toFixed(2)} | 5 Match: ${potentialWin5.toFixed(2)}`;
} | 4 Match: ${potentialWin4} | 5 Match: ${potentialWin5}`;
        }

        document.getElementById("spin").addEventListener("click", () => { 
    spinReels();
});

function spinReels() {
    let bet = parseInt(document.getElementById("betAmount").value);
    let rewardPool = parseInt(document.getElementById("rewardPool").innerText);
    let balance = parseInt(document.getElementById("tGtBalance").innerText);
    
    if (isNaN(bet) || bet < 1 || bet > 100 || bet > balance) {
        document.getElementById("message").innerText = "Invalid bet amount!";
        return;
    }
    
    balance -= bet;
    rewardPool += bet;
    document.getElementById("tGtBalance").innerText = balance;
    document.getElementById("rewardPool").innerText = rewardPool;
    
    const symbols = ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"];
    let reels = document.querySelectorAll(".reel");
    let results = [];
    
    reels.forEach((reel, index) => {
        setTimeout(() => {
            reel.style.transition = "transform 1s ease-out";
            reel.style.transform = "translateY(100px)";
            
            setTimeout(() => {
                let randomSymbol = symbols[Math.floor(Math.random() * symbols.length)];
                reel.innerText = randomSymbol;
                reel.style.transform = "translateY(0px)";
                results.push(randomSymbol);
                
                if (results.length === reels.length) {
                    checkWin(results, bet);
                }
            }, 1000);
        }, index * 800);
    });
}

function updatePotentialWin() {
    let bet = parseInt(document.getElementById("betAmount").value);
    let rewardPool = parseInt(document.getElementById("rewardPool").innerText);
    
    if (isNaN(bet) || bet < 1 || bet > 100) {
        document.getElementById("potentialWin").innerText = "Invalid Bet!";
        return;
    }

    let potentialWin3 = Math.floor(rewardPool * 0.000005 * bet);
    let potentialWin4 = Math.floor(rewardPool * 0.0005 * bet);
    let potentialWin5 = Math.floor(rewardPool * 0.05 * bet);

    document.getElementById("potentialWin").innerText = `3 Match: ${potentialWin3} | 4 Match: ${potentialWin4} | 5 Match: ${potentialWin5}`;
}
    
    balance -= bet;
    rewardPool += bet;
    document.getElementById("tGtBalance").innerText = balance;
    document.getElementById("rewardPool").innerText = rewardPool;
    
    let reels = document.querySelectorAll(".reel");
    let results = [];
    
    reels.forEach((reel, index) => {
        setTimeout(() => {
            reel.style.transition = "transform 0.8s ease-out";
            reel.style.transform = "translateY(100px)";
            
            setTimeout(() => {
                let randomSymbol = symbols[Math.floor(Math.random() * symbols.length)];
                reel.innerText = randomSymbol;
                reel.style.transform = "translateY(0px)";
                results.push(randomSymbol);
                
                if (index === reels.length - 1) {
                    checkWin(results, bet);
                }
            }, 800);
        }, index * 600);
    });
}

function updatePotentialWin() {
    let bet = parseInt(document.getElementById("betAmount").value);
    let rewardPool = parseInt(document.getElementById("rewardPool").innerText);
    
    if (isNaN(bet) || bet < 1 || bet > 100) {
        document.getElementById("potentialWin").innerText = "Invalid Bet!";
        return;
    }

    let potentialWin3 = Math.floor(rewardPool * 0.000005 * bet);
    let potentialWin4 = Math.floor(rewardPool * 0.0005 * bet);
    let potentialWin5 = Math.floor(rewardPool * 0.05 * bet);

    document.getElementById("potentialWin").innerText = `3 Match: ${potentialWin3} | 4 Match: ${potentialWin4} | 5 Match: ${potentialWin5}`;
}
    
    balance -= bet;
    rewardPool += bet;
    document.getElementById("tGtBalance").innerText = balance;
    document.getElementById("rewardPool").innerText = rewardPool;
    
    let reels = document.querySelectorAll(".reel");
    let results = [];
    
    reels.forEach((reel, index) => {
        setTimeout(() => {
            reel.style.transition = "transform 0.5s ease-out";
            reel.style.transform = "rotateX(360deg)";
            
            setTimeout(() => {
                let randomSymbol = symbols[Math.floor(Math.random() * symbols.length)];
                reel.innerText = randomSymbol;
                reel.style.transform = "rotateX(0deg)";
                results.push(randomSymbol);
                
                if (index === reels.length - 1) {
                    checkWin(results, bet);
                }
            }, 500);
        }, index * 500);
    });
}

function updatePotentialWin() {
    let bet = parseInt(document.getElementById("betAmount").value);
    let rewardPool = parseInt(document.getElementById("rewardPool").innerText);
    
    if (isNaN(bet) || bet < 1 || bet > 100) {
        document.getElementById("potentialWin").innerText = "Invalid Bet!";
        return;
    }

    let potentialWin3 = (rewardPool * 0.000005 * bet);
    let potentialWin4 = (rewardPool * 0.0005 * bet);
    let potentialWin5 = (rewardPool * 0.05 * bet);

    document.getElementById("potentialWin").innerText = `3 Match: ${potentialWin3.toFixed(2)} | 4 Match: ${potentialWin4.toFixed(2)} | 5 Match: ${potentialWin5.toFixed(2)}`;
}
    
    balance -= bet;
    rewardPool += bet;
    document.getElementById("tGtBalance").innerText = balance;
    document.getElementById("rewardPool").innerText = rewardPool;
    
    let reels = document.querySelectorAll(".reel");
    let results = [];
    
    reels.forEach((reel, index) => {
        setTimeout(() => {
            reel.style.transition = "transform 0.5s ease-out";
            reel.style.transform = "rotateX(360deg)";
            
            setTimeout(() => {
                let randomSymbol = symbols[Math.floor(Math.random() * symbols.length)];
                reel.innerText = randomSymbol;
                reel.style.transform = "rotateX(0deg)";
                results.push(randomSymbol);
                
                if (index === reels.length - 1) {
                    checkWin(results, bet);
                }
            }, 500);
        }, index * 500);
    });
}

function updatePotentialWin() {
    let bet = parseInt(document.getElementById("betAmount").value);
    let rewardPool = parseInt(document.getElementById("rewardPool").innerText);

    if (isNaN(bet) || bet < 1 || bet > 100) {
        document.getElementById("potentialWin").innerText = "Invalid Bet!";
        return;
    }

    let potentialWin3 = (rewardPool * 0.00005 * bet);
    let potentialWin4 = (rewardPool * 0.005 * bet);
    let potentialWin5 = (rewardPool * 0.5 * bet);

    document.getElementById("potentialWin").innerText = `3 Match: ${potentialWin3.toFixed(2)} | 4 Match: ${potentialWin4.toFixed(2)} | 5 Match: ${potentialWin5.toFixed(2)}`;
}

    let potentialWin3 = (rewardPool * 0.00005 * bet);
    let potentialWin4 = (rewardPool * 0.005 * bet);
    let potentialWin5 = (rewardPool * 0.5 * bet);

    document.getElementById("potentialWin").innerText = `3 Match: ${potentialWin3.toFixed(2)} | 4 Match: ${potentialWin4.toFixed(2)} | 5 Match: ${potentialWin5.toFixed(2)}`;
}
    
    balance -= bet;
    rewardPool += bet;
    document.getElementById("tGtBalance").innerText = balance;
    document.getElementById("rewardPool").innerText = rewardPool;
    
    let reels = document.querySelectorAll(".reel");
    let results = [];
    
    reels.forEach((reel, index) => {
        setTimeout(() => {
            let randomSymbol = symbols[Math.floor(Math.random() * symbols.length)];
            reel.innerText = randomSymbol;
            results.push(randomSymbol);
            
            if (index === reels.length - 1) {
                checkWin(results, bet);
            }
        }, index * 500);
    });
}
            spinReels();
        });
    </script>
</body>
</html>
