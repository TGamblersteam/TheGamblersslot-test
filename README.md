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
        elements.message.textContent = "Geçersiz bahis!";
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
    const symbolCount = strip.children.length;
    const randomPosition = Math.floor(Math.random() * (symbolCount - 5)) * 100;
    
    strip.style.transition = "none";
    strip.style.top = "-2000px";

    setTimeout(() => {
        strip.style.transition = "top 1s ease-out";
        strip.style.top = `-${randomPosition}px`;
        setTimeout(callback, 1000);
    }, 50);
}

function checkWin(bet) {
    // Kazanan sembolleri hesapla
    let results = Array.from(elements.reels).map(strip => {
        const position = Math.abs(parseInt(strip.style.top) / 100);
        return strip.children[position % symbols.length].textContent;
    });

    // Eşleşme sayısını bul
    let count = results.reduce((acc, val) => (acc[val] = (acc[val] || 0) + 1, acc), {});
    let maxCount = Math.max(...Object.values(count));

    // Yeni ödül sistemi
    let winAmount = 0;
    if (maxCount >= 3) {
        const poolPercentage = {
            3: 0.0001, // %0.01
            4: 0.01,   // %1
            5: 1       // %100
        }[maxCount] || 0;
        
        winAmount = Math.floor(bet * (rewardPool * poolPercentage / 100));
    }

    if (winAmount > 0 && winAmount <= rewardPool) {
        playerPoints += winAmount;
        rewardPool -= winAmount;
        elements.message.textContent = `🎉 ${maxCount}x KAZANDINIZ! ${winAmount} puan kazandınız! 🎉`;
    } else {
        elements.message.textContent = "💥 KAYBETTİNİZ!";
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
