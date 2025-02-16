<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Basit Slot Oyunu</title>
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
    </style>
</head>
<body>

    <div class="container">
        <h1>Basit Slot Makinesi</h1>

        <div class="slot-machine">
            <div class="reel" id="reel1">🍒</div>
            <div class="reel" id="reel2">🍋</div>
            <div class="reel" id="reel3">🍊</div>
        </div>

        <div class="buttons">
            <button id="spin">SPIN</button>
        </div>

        <p class="message" id="message">Spin'e bas ve şansını dene!</p>
    </div>

    <script>
        // 7 farklı sembol tanımlandı
        const symbols = ["🍒", "🍋", "🍊", "🍉", "🍎", "🍇", "🍌"];

        function spinReels() {
            let reels = document.querySelectorAll(".reel");
            let result = [];

            // Her makara için rastgele sembol seçimi
            reels.forEach(reel => {
                let randomSymbol = symbols[Math.floor(Math.random() * symbols.length)];
                reel.innerText = randomSymbol;
                result.push(randomSymbol);
            });

            // Kazanç kontrolü
            checkWin(result);
        }

        function checkWin(result) {
            let message = document.getElementById("message");

            // Tüm semboller eşleşiyorsa
            if (result[0] === result[1] && result[1] === result[2]) {
                message.innerText = `Tebrikler! ${result[0]} ile kazandınız! 🎉`;
                message.style.color = "yellow";
            } else {
                message.innerText = "Şansını tekrar dene!";
                message.style.color = "white";
            }
        }

        document.getElementById("spin").addEventListener("click", spinReels);
    </script>

</body>
</html>
