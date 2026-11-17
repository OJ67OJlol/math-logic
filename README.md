# math-logic
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Math Logic</title>
<style>
  body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #17134a, #176b87);
    color: white;
    text-align: center;
    min-height: 100vh;
  }
  header { padding: 25px 10px; }
  h1 { font-size: 42px; margin-bottom: 5px; }
  .panel {
    background: rgba(255,255,255,.12);
    max-width: 650px;
    margin: 20px auto;
    padding: 22px;
    border-radius: 18px;
  }
  input, button {
    font-size: 18px;
    padding: 12px;
    margin: 6px;
    border: 0;
    border-radius: 9px;
  }
  input { max-width: 85%; }
  button {
    background: #ffcf40;
    color: #17134a;
    font-weight: bold;
    cursor: pointer;
  }
  button:hover { background: #ffe58a; }
  .game {
    background: rgba(255,255,255,.13);
    margin: 10px 0;
    padding: 13px;
    border-radius: 12px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 8px;
  }
  .game button { font-size: 14px; }
  #games { display: none; }
  #message { min-height: 24px; }
  #display {
    width: 80%;
    box-sizing: border-box;
    text-align: right;
    font-size: 26px;
  }
  .calc-buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 6px;
    max-width: 360px;
    margin: auto;
  }
  .calc-buttons button { margin: 0; }
</style>
</head>
<body>

<header>
  <h1>🧠 Math Logic</h1>
  <p>Think, learn, calculate, and play!</p>
</header>

<section class="panel">
  <h2>🧮 Calculator</h2>
  <input id="display" type="text" readonly value="0">
  <div class="calc-buttons">
    <button onclick="clearCalc()">C</button>
    <button onclick="add('/')">÷</button>
    <button onclick="add('*')">×</button>
    <button onclick="add('-')">−</button>
    <button onclick="add('7')">7</button>
    <button onclick="add('8')">8</button>
    <button onclick="add('9')">9</button>
    <button onclick="add('+')">+</button>
    <button onclick="add('4')">4</button>
    <button onclick="add('5')">5</button>
    <button onclick="add('6')">6</button>
    <button onclick="calculate()">=</button>
    <button onclick="add('1')">1</button>
    <button onclick="add('2')">2</button>
    <button onclick="add('3')">3</button>
    <button onclick="add('0')">0</button>
    <button onclick="add('.')">.</button>
  </div>
</section>

<section class="panel">
  <h2>🔐 Game Code</h2>
  <p>Enter the secret four-digit code to unlock the games.</p>
  <input id="secret" type="password" maxlength="4"
         inputmode="numeric" placeholder="Enter code">
  <button onclick="unlock()">Unlock</button>
  <p id="message"></p>
</section>

<section class="panel" id="games">
  <h2>🎮 Game Library</h2>
  <input id="search" type="text" placeholder="Search games..."
         oninput="showGames()">
  <div id="gameList"></div>
</section>

<script>
let expression = "";

function add(value) {
  expression += value;
  document.getElementById("display").value = expression;
}

function clearCalc() {
  expression = "";
  document.getElementById("display").value = "0";
}

function calculate() {
  try {
    if (!/^[0-9+\-*/.() ]+$/.test(expression)) {
      throw new Error("Invalid calculation");
    }
    const answer = Function('"use strict"; return (' + expression + ')')();
    if (!Number.isFinite(answer)) throw new Error("Invalid answer");
    expression = String(answer);
    document.getElementById("display").value = expression;
  } catch {
    document.getElementById("display").value = "Error";
    expression = "";
  }
}

const games = [
  { name: "Ski Frenzy", code: "2034" },
  { name: "Kour.io", code: "2036" },
  { name: "Basketball Legends", code: "2040" },
  { name: "Krunker", code: "2060" },
  { name: "Shell Shockers", code: "2062" },
  { name: "Drive Mad", code: "4018" },
  { name: "Geometry Dash", code: "4024" },
  { name: "Slither.io", code: "4049" },
  { name: "Subway Surfers", code: "4056" },
  { name: "Retro Bowl", code: "4042" },
  { name: "Minecraft", code: "3011" },
  { name: "Granny", code: "3003" }
];

function unlock() {
  const code = document.getElementById("secret").value;
  const message = document.getElementById("message");

  if (code === "6767") {
    document.getElementById("games").style.display = "block";
    message.textContent = "✅ Games unlocked!";
    showGames();
  } else {
    message.textContent = "❌ Incorrect code. Try again.";
  }
}

function showGames() {
  const search = document.getElementById("search").value.toLowerCase();
  const list = document.getElementById("gameList");
  list.innerHTML = "";

  games
    .filter(game =>
      game.name.toLowerCase().includes(search) ||
      game.code.includes(search)
    )
    .forEach(game => {
      const item = document.createElement("div");
      item.className = "game";

      const label = document.createElement("span");
      label.textContent = game.name + " — Code: " + game.code;

      const button = document.createElement("button");
      button.textContent = "Find game";
      button.onclick = () => {
        const url = "https://www.google.com/search?q=" +
          encodeURIComponent(game.name + " play online");
        window.open(url, "_blank", "noopener");
      };

      item.append(label, button);
      list.appendChild(item);
    });
}
</script>

</body>
</html>
