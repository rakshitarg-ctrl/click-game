<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Click Speed Reflex Game</title>

<style>
    body {
        background: #020617;
        color: white;
        font-family: Arial, sans-serif;
        text-align: center;
        padding-top: 40px;
    }

    h1 {
        font-size: 34px;
        margin-bottom: 20px;
    }

    p {
        font-size: 24px;
    }

    input {
        width: 80%;
        max-width: 350px;
        padding: 18px;
        font-size: 24px;
        border-radius: 12px;
        border: none;
        text-align: center;
    }

    button {
        font-size: 26px;
        padding: 20px 40px;
        border: none;
        border-radius: 16px;
        margin-top: 20px;
        cursor: pointer;
    }

    #startBtn {
        background: #38bdf8;
        color: black;
        width: 80%;
        max-width: 350px;
    }

    #clickBtn {
        background: #22c55e;
        color: black;
        display: none;
        width: 85%;
        max-width: 400px;
        font-size: 32px;
        padding: 30px;
        margin-top: 30px;
    }

    #countdown {
        font-size: 80px;
        margin: 30px;
        font-weight: bold;
    }

    #score, #timer {
        font-size: 30px;
        margin: 15px;
    }

    #records {
        margin-top: 35px;
        font-size: 20px;
    }
</style>
</head>

<body>

<h1>⚡ Click Speed Reflex Test ⚡</h1>

<div id="nameBox">
    <p>Enter Your Name</p>
    <input id="playerName" placeholder="Your name">
    <br>
    <button id="startBtn">START GAME</button>
</div>

<div id="countdown"></div>

<div id="game">
    <div id="timer"></div>
    <div id="score"></div>
    <button id="clickBtn">CLICK FAST!</button>
</div>

<div id="records"></div>

<script>
let score = 0;
let timeLeft = 10;
let timer;
let player = "";

const startBtn = document.getElementById("startBtn");
const clickBtn = document.getElementById("clickBtn");
const countdown = document.getElementById("countdown");
const scoreText = document.getElementById("score");
const timerText = document.getElementById("timer");
const recordsDiv = document.getElementById("records");

startBtn.onclick = function () {
    player = document.getElementById("playerName").value.trim();
    if (player === "") {
        alert("Please enter your name");
        return;
    }

    document.getElementById("nameBox").style.display = "none";
    startCountdown();
};

function startCountdown() {
    let count = 3;
    countdown.innerHTML = count;

    let cd = setInterval(() => {
        count--;
        if (count === 0) {
            clearInterval(cd);
            countdown.innerHTML = "GO!";
            setTimeout(() => {
                countdown.innerHTML = "";
                startGame();
            }, 600);
        } else {
            countdown.innerHTML = count;
        }
    }, 1000);
}

function startGame() {
    score = 0;
    timeLeft = 10;

    clickBtn.style.display = "inline-block";
    scoreText.innerHTML = "Score: 0";
    timerText.innerHTML = "Time: 10 sec";

    timer = setInterval(() => {
        timeLeft--;
        timerText.innerHTML = "Time: " + timeLeft + " sec";

        if (timeLeft === 0) {
            clearInterval(timer);
            endGame();
        }
    }, 1000);
}

clickBtn.onclick = function () {
    score++;
    scoreText.innerHTML = "Score: " + score;
};

function endGame() {
    clickBtn.style.display = "none";

    let reflex = (score / 10).toFixed(2);

    alert(
        "Player: " + player +
        "\nTotal Clicks: " + score +
        "\nReflex Speed: " + reflex + " clicks/sec"
    );

    saveRecord(player, score, reflex);
    showRecords();
}

function saveRecord(name, score, reflex) {
    let records = JSON.parse(localStorage.getItem("clickRecords")) || [];
    records.push({ name, score, reflex });
    localStorage.setItem("clickRecords", JSON.stringify(records));
}

function showRecords() {
    let records = JSON.parse(localStorage.getItem("clickRecords")) || [];
    let html = "<h3>🏆 Recent Records</h3>";

    records.slice(-5).reverse().forEach(r => {
        html += `<p>${r.name} → ${r.score} clicks | ${r.reflex} cps</p>`;
    });

    recordsDiv.innerHTML = html;
}

showRecords();
</script>

</body>
</html>
