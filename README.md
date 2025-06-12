# jumbled-words-game
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Jumbled Word Game</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      background-color: #f9f9f9;
      padding: 20px;
    }
    #game {
      margin-top: 20px;
    }
    .word-box {
      font-size: 24px;
      margin: 20px 0;
    }
    input[type="text"] {
      padding: 10px;
      font-size: 18px;
    }
    button {
      padding: 10px 20px;
      margin-top: 20px;
      font-size: 16px;
    }
    #scoreBox {
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>Jumbled Word Game</h1>
  <div>
    <label for="playerName">Enter your name:</label>
    <input type="text" id="playerName">
    <button onclick="startGame()">Start Game</button>
  </div>

  <div id="game" style="display:none;">
    <div class="word-box" id="jumbledWord"></div>
    <input type="text" id="userInput" placeholder="Your guess">
    <button onclick="submitAnswer()">Submit</button>
    <div id="scoreBox"></div>
  </div>

  <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-database-compat.js"></script>
  <script>
    const firebaseConfig = {
  apiKey: "AIzaSyCUMehDMWYoGSK4P4acHoz13I_rZ_k8Dlk",
  authDomain: "jumbledwords-eb483.firebaseapp.com",
  databaseURL: "https://jumbledwords-eb483-default-rtdb.firebaseio.com",
  projectId: "jumbledwords-eb483",
  storageBucket: "jumbledwords-eb483.firebasestorage.app",
  messagingSenderId: "950250611095",
  appId: "1:950250611095:web:2247e57f754f9d2fc4e4d2",
  measurementId: "G-SFT9D3Z9NL"
};
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    const words = [
      "apple", "banana", "orange", "grape", "lemon",
      "mango", "peach", "cherry", "strawberry", "melon",
      "papaya", "kiwi", "plum", "pear", "fig"
    ];

    let score = 0;
    let currentIndex = 0;
    let startTime;

    function shuffle(word) {
      return word.split('').sort(() => 0.5 - Math.random()).join('');
    }

    function startGame() {
      const name = document.getElementById("playerName").value.trim();
      if (!name) return alert("Please enter your name");
      document.getElementById("game").style.display = "block";
      document.querySelector("input[type='text']").focus();
      score = 0;
      currentIndex = 0;
      startTime = new Date();
      showNextWord();
    }

    function showNextWord() {
      if (currentIndex >= words.length) return endGame();
      const word = words[currentIndex];
      document.getElementById("jumbledWord").textContent = shuffle(word);
      document.getElementById("userInput").value = "";
    }

    function submitAnswer() {
      const input = document.getElementById("userInput").value.trim().toLowerCase();
      if (input === words[currentIndex]) {
        score++;
      }
      currentIndex++;
      showNextWord();
    }

    function endGame() {
      const endTime = new Date();
      const timeTaken = Math.round((endTime - startTime) / 1000);
      const name = document.getElementById("playerName").value.trim();

      document.getElementById("game").innerHTML = `<h2>Game Over!</h2>
        <p><strong>${name}</strong>, you scored <strong>${score}/15</strong></p>
        <p>Time taken: ${timeTaken} seconds</p>`;

      db.ref("scores").push({
        name: name,
        score: score,
        time: timeTaken,
        date: new Date().toLocaleString()
      });
    }
  </script>
</body>
</html>
