Enter<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Discipline Trader</title>
<style>
body {
    background: #0d1117;
    color: white;
    font-family: Arial;
    padding: 20px;
}
h2 { text-align: center; }

input, button {
    width: 100%;
    padding: 12px;
    margin: 6px 0;
    border-radius: 8px;
    border: none;
}

input { background: #161b22; color: white; }

button {
    background: #238636;
    color: white;
    font-weight: bold;
}

.result {
    padding: 20px;
    margin-top: 15px;
    border-radius: 12px;
    text-align: center;
    font-size: 20px;
}

.green { background: #033a16; }
.red { background: #3a0d0d; }

.progress-container {
    background: #161b22;
    border-radius: 20px;
    overflow: hidden;
    margin-top: 15px;
}

.progress-bar {
    height: 20px;
    background: #238636;
    width: 0%;
}

.streak {
    text-align: center;
    margin-top: 10px;
    font-size: 18px;
}
</style>
</head>

<body>

<h2>🔥 Discipline Trader</h2>

<input type="number" id="balance" placeholder="Account Balance">
<input type="number" id="risk" placeholder="Risk % per Trade">
<input type="number" id="entry" placeholder="Entry Price">
<input type="number" id="stop" placeholder="Stop Loss Price">
<input type="number" id="dailyLimit" placeholder="Max Daily Risk %">

<button onclick="calculate()">Calculate Position</button>

<div id="resultBox" class="result"></div>

<div class="progress-container">
    <div id="progressBar" class="progress-bar"></div>
</div>

<div class="streak" id="streak">Discipline Streak: 0 🔥</div>

<script>
let streak = 0;

function calculate() {

    let balance = parseFloat(document.getElementById("balance").value);
    let riskPercent = parseFloat(document.getElementById("risk").value);
    let entry = parseFloat(document.getElementById("entry").value);
    let stop = parseFloat(document.getElementById("stop").value);
    let dailyLimit = parseFloat(document.getElementById("dailyLimit").value);

    if (!balance || !riskPercent || !entry || !stop) {
        alert("Fill all fields properly.");
        return;
    }

    let riskAmount = balance * (riskPercent / 100);
    let stopDistance = Math.abs(entry - stop);
    let positionSize = riskAmount / stopDistance;

    let resultBox = document.getElementById("resultBox");
    resultBox.innerHTML = `
        Position Size: ${positionSize.toFixed(4)} <br>
        Risk Amount: ${riskAmount.toFixed(2)}
    `;

    if (riskPercent <= dailyLimit) {
        resultBox.className = "result green";
        streak++;
    } else {
        resultBox.className = "result red";
        alert("⚠ You are breaking your daily risk rule!");
        streak = 0;
    }

    document.getElementById("streak").innerText =
        "Discipline Streak: " + streak + " 🔥";

    let progress = (riskPercent / dailyLimit) * 100;
    if (progress > 100) progress = 100;

    document.getElementById("progressBar").style.width = progress + "%";
}
</script>

</body>
</html>
