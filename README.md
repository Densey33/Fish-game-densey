<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Underwater Scene with Loader</title>
<!-- Tailwind CSS -->
<script src="https://cdn.tailwindcss.com"></script>
<style>
  body {
    margin: 0;
    height: 100vh;
    overflow: hidden;
    background: linear-gradient(to bottom, #0d3b66, #115c8b, #23a6d5);
    font-family: sans-serif;
  }

  /* Water plants */
  .plants {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 50%;
    background: linear-gradient(to top, #2e8b57, transparent);
    overflow: hidden;
    z-index: 0;
  }

  /* Plants sway animation */
  .plant {
    position: absolute;
    bottom: 0;
    width: 20px;
    height: 100px;
    background: linear-gradient(to top, #006400, #00ff7f);
    border-radius: 10px;
    opacity: 0.8;
    animation: sway 3s infinite ease-in-out;
  }
  /* különböző helyeken */
  .plant:nth-child(1) { left:10%; height:150px; }
  .plant:nth-child(2) { left:25%; height:100px; }
  .plant:nth-child(3) { left:40%; height:180px; }
  .plant:nth-child(4) { left:55%; height:130px; }
  .plant:nth-child(5) { left:70%; height:160px; }

  @keyframes sway {
    0% { transform: translateX(0); }
    50% { transform: translateX(5px); }
    100% { transform: translateX(0); }
  }

  /* Kövek */
  .rocks {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 20%;
    z-index: 1;
  }

  .rock {
    position: absolute;
    bottom: 0;
    width: 60px;
    height: 30px;
    background: #654321;
    border-radius: 50% / 40%;
    opacity: 0.8;
  }
  /* különböző helyeken */
  .rock:nth-child(1) { left:15%; width:80px; height:40px; }
  .rock:nth-child(2) { left:50%; width:60px; height:25px; }
  .rock:nth-child(3) { left:75%; width:100px; height:50px; }

  /* Buborékok */
  #bubbles {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 2;
  }
  .bubble {
    position: absolute;
    bottom: 0;
    border-radius: 50%;
    background: rgba(255,255,255,0.7);
    opacity: 0.8;
    animation: rise 10s linear infinite;
  }
  @keyframes rise {
    0% { transform: translateY(0); opacity: 0.8; }
    100% { transform: translateY(-100vh); opacity: 0; }
  }

  /* A start loader külön konténer, szétválasztva */
  #start-area {
    position: absolute;
    top: 50%;
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 50px;
    transform: translateY(-50%);
    z-index: 999; /* felül minden */
  }

  /* Loader bal oldalra */
  #loader-wrapper {
    display: flex;
    align-items: center;
  }

  /* Start gomb jobb oldalra */
  #startButton {
    font-size: 3rem;
    padding: 20px 40px;
    border-radius: 8px;
    background: linear-gradient(to bottom, #6366f1, #3b82f6);
    color: white;
    border: none;
    cursor: pointer;
    font-weight: bold;
  }

  /* Loader animáció */
  .loader {
    display: flex;
    flex-wrap: wrap;
    width: calc(3 * (52px + 2px));
    height: calc(3 * (52px + 2px));
  }
  .cell {
    width: 52px;
    height: 52px;
    margin: 1px;
    border-radius: 4px;
    background-color: transparent;
    animation: ripple 1.2s ease-in-out infinite;
  }
  /* Késleltetés a cellák között */
  .d-1 { animation-delay: 0.1s; }
  .d-2 { animation-delay: 0.2s; }
  .d-3 { animation-delay: 0.3s; }
  .d-4 { animation-delay: 0.4s; }

  /* Színek a cellákhoz */
  .cell:nth-child(1) { --color: #00FF87; }
  .cell:nth-child(2) { --color: #0CFD95; }
  .cell:nth-child(3) { --color: #17FBA2; }
  .cell:nth-child(4) { --color: #23F9B2; }
  .cell:nth-child(5) { --color: #30F7C3; }
  .cell:nth-child(6) { --color: #3DF5D4; }
  .cell:nth-child(7) { --color: #45F4DE; }
  .cell:nth-child(8) { --color: #53F1F0; }
  .cell:nth-child(9) { --color: #60EFFF; }

  @keyframes ripple {
    0% { background-color: transparent; }
    30% { background-color: var(--color); }
    60% { background-color: transparent; }
  }

  /* Játék pontszám, üzenet stb. */
  #scoreboard {
    position: absolute;
    top: 10px;
    left: 10px;
    color: white;
    font-size: 20px;
    z-index: 10;
  }
  #message {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: white;
    font-size: 24px;
    background: rgba(0,0,0,0.7);
    padding: 20px;
    display: none;
    z-index: 20;
  }
  #restartButton {
    position: absolute;
    top: 10px;
    right: 10px;
    padding: 8px 16px;
    background-color: #ffcc00;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
  }
  #restartButton:hover {
    background-color: #ff9900;
  }
  /* Extra icons/buttons next to restart */
  #extra-icons {
    position: absolute;
    top: 10px;
    right: 10px;
    display: flex;
    gap: 10px;
    z-index: 10;
  }
  .icon-button {
    background: transparent;
    border: none;
    cursor: pointer;
    font-size: 1.5rem;
    color: white;
    padding: 5px;
  }
  .icon-button:hover {
    color: #ffd700;
  }
</style>
</head>
<body>

<!-- Környezet -->
<div class="plants">
  <div class="plant" style="left:10%;"></div>
  <div class="plant" style="left:25%;"></div>
  <div class="plant" style="left:40%;"></div>
  <div class="plant" style="left:55%;"></div>
  <div class="plant" style="left:70%;"></div>
</div>
<div class="rocks">
  <div class="rock" style="left:15%;"></div>
  <div class="rock" style="left:50%;"></div>
  <div class="rock" style="left:75%;"></div>
</div>
<div class="bubbles" id="bubbles"></div>

<!-- Start és loader külön szekcióban, egymás mellett -->
<div id="start-area">
  <!-- Loader bal oldalon -->
  <div id="loader-wrapper">
    <div class="loader">
      <div class="cell d-1"></div>
      <div class="cell d-2"></div>
      <div class="cell d-2"></div>
      <div class="cell d-2"></div>
      <div class="cell d-3"></div>
      <div class="cell d-3"></div>
      <div class="cell d-3"></div>
      <div class="cell d-3"></div>
      <div class="cell d-4"></div>
    </div>
  </div>
  <!-- Start gomb jobb oldalon -->
  <button id="startButton">Start</button>
</div>

<!-- Játék canvas -->
<canvas id="gameCanvas"></canvas>
<!-- Extra ikonok -->
<div id="extra-icons">
  <!-- Zene kikapcsoló -->
  <button id="musicToggle" class="icon-button" title="Zene be/ki">🎵</button>
  <!-- Szünet/Folytatás -->
  <button id="pauseResume" class="icon-button" title="Szünet/Folytatás">▶️</button>
  <!-- Újrakezdés -->
  <button id="resetGame" class="icon-button" title="Újrakezdés">🔄</button>
</div>

<!-- Pontszám -->
<div id="scoreboard">Pontszám: 0 | Célpont: 30</div>
<!-- Üzenet -->
<div id="message"></div>

<!-- Háttérzene -->
<audio id="backgroundMusic" src="backgroundm.mp3" loop></audio>

<script>
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

let width = window.innerWidth;
let height = window.innerHeight;
canvas.width = width;
canvas.height = height;

window.addEventListener('resize', () => {
  width = window.innerWidth;
  height = window.innerHeight;
  canvas.width = width;
  canvas.height = height;
});

// Játék adatok
let score = 0;
const targetScore = 30;
let gameStarted = false;
let paused = false;
let animationFrameId = null;

// Fő játék objektumok
const fishes = [];
const rareFishes = [];
const players = [
  { x: 0, y: 0, size: 50, speed: 5, keys: {} },
  { x: 0, y: 0, size: 50, speed: 5, keys: {} }
];

function initializePlayers() {
  players[0].x = width/3;
  players[0].y = height/2;
  players[1].x = 2*width/3;
  players[1].y = height/2;
}
initializePlayers();

// Ritka halak
function createRareFish() {
  const rf = {
    x: Math.random() * (width - 40),
    y: Math.random() * (height - 40),
    size: 40,
    color: 'yellow',
    eyeColor: 'black',
    visible: true,
    directionX: Math.random() < 0.5 ? -1 : 1,
    directionY: Math.random() < 0.5 ? -1 : 1,
    move: function() {
      if (!this.visible) return;
      this.x += this.directionX * 1.5;
      this.y += this.directionY * 1.5;
      if (this.x < this.size/2 || this.x > width - this.size/2) this.directionX *= -1;
      if (this.y < this.size/2 || this.y > height - this.size/2) this.directionY *= -1;
    },
    draw: function() {
      if (!this.visible) return;
      ctx.fillStyle = this.color;
      ctx.beginPath();
      ctx.ellipse(this.x, this.y, this.size/2, this.size/2.4, 0, 0, Math.PI*2);
      ctx.fill();

      ctx.fillStyle = 'orange';
      ctx.beginPath();
      ctx.arc(this.x - 10, this.y - 5, 4, 0, Math.PI*2);
      ctx.fill();
      ctx.beginPath();
      ctx.arc(this.x + 10, this.y + 5, 4, 0, Math.PI*2);
      ctx.fill();

      ctx.fillStyle = this.eyeColor;
      ctx.beginPath();
      ctx.arc(this.x + this.size/4, this.y - this.size/8, 5, 0, Math.PI*2);
      ctx.fill();

      ctx.fillStyle = 'white';
      ctx.beginPath();
      ctx.arc(this.x + this.size/4, this.y - this.size/8, 2, 0, Math.PI*2);
      ctx.fill();

      ctx.fillStyle = this.color;
      ctx.beginPath();
      ctx.moveTo(this.x - this.size/2, this.y);
      ctx.lineTo(this.x - this.size/2 - 20, this.y - 10);
      ctx.lineTo(this.x - this.size/2 - 20, this.y + 10);
      ctx.closePath();
      ctx.fill();
    }
  };
  return rf;
}
const maxRareFishes = 10;
for (let i=0; i<3; i++) {
  rareFishes.push(createRareFish());
}
for (let i=0; i<5; i++) {
  fishes.push({
    size: 40,
    x: Math.random() * (width - 40),
    y: Math.random() * (height - 40),
    speed: 1 + Math.random() * 2,
    direction: ['left','right','up','down'][Math.floor(Math.random()*4)],
    move: function() {
      switch(this.direction) {
        case 'left': this.x -= this.speed; if (this.x < -this.size) this.x = width + this.size; break;
        case 'right': this.x += this.speed; if (this.x > width + this.size) this.x = -this.size; break;
        case 'up': this.y -= this.speed; if (this.y < -this.size) this.y = height + this.size; break;
        case 'down': this.y += this.speed; if (this.y > height + this.size) this.y = -this.size; break;
      }
    },
    draw: function() {
      ctx.fillStyle = '#0099ff';
      ctx.beginPath();
      ctx.ellipse(this.x, this.y, this.size/2, this.size/4, 0, 0, Math.PI*2);
      ctx.fill();

      ctx.fillStyle = '#0066cc';
      ctx.beginPath();
      ctx.moveTo(this.x + this.size/2, this.y);
      ctx.lineTo(this.x + this.size/2 + 10, this.y - 5);
      ctx.lineTo(this.x + this.size/2 + 10, this.y + 5);
      ctx.closePath();
      ctx.fill();
    }
  });
}

document.addEventListener('keydown', (e) => {
  if (['ArrowUp','ArrowDown','ArrowLeft','ArrowRight'].includes(e.key))
    players[0].keys[e.key] = true;
  if (['w','a','s','d','W','A','S','D'].includes(e.key))
    players[1].keys[e.key.toLowerCase()] = true;
});
document.addEventListener('keyup', (e) => {
  if (['ArrowUp','ArrowDown','ArrowLeft','ArrowRight'].includes(e.key))
    players[0].keys[e.key] = false;
  if (['w','a','s','d','W','A','S','D'].includes(e.key))
    players[1].keys[e.key.toLowerCase()] = false;
});

function movePlayers() {
  if (!gameStarted || paused) return;
  if (players[0].keys['ArrowUp']) players[0].y -= players[0].speed;
  if (players[0].keys['ArrowDown']) players[0].y += players[0].speed;
  if (players[0].keys['ArrowLeft']) players[0].x -= players[0].speed;
  if (players[0].keys['ArrowRight']) players[0].x += players[0].speed;
  if (players[0].x < players[0].size/2) players[0].x = players[0].size/2;
  if (players[0].x > width - players[0].size/2) players[0].x = width - players[0].size/2;
  if (players[0].y < players[0].size/2) players[0].y = players[0].size/2;
  if (players[0].y > height - players[0].size/2) players[0].y = height - players[0].size/2;

  if (players[1].keys['w']) players[1].y -= players[1].speed;
  if (players[1].keys['s']) players[1].y += players[1].speed;
  if (players[1].keys['a']) players[1].x -= players[1].speed;
  if (players[1].keys['d']) players[1].x += players[1].speed;
  if (players[1].x < players[1].size/2) players[1].x = players[1].size/2;
  if (players[1].x > width - players[1].size/2) players[1].x = width - players[1].size/2;
  if (players[1].y < players[1].size/2) players[1].y = players[1].size/2;
  if (players[1].y > height - players[1].size/2) players[1].y = height - players[1].size/2;
}

function drawPlayers() {
  ctx.strokeStyle = '#000000';
  ctx.lineWidth = 2;
  ctx.beginPath();
  ctx.arc(players[0].x, players[0].y - players[0].size/4, 10, 0, Math.PI*2);
  ctx.stroke();
  ctx.moveTo(players[0].x, players[0].y - players[0].size/4 + 10);
  ctx.lineTo(players[0].x, players[0].y + players[0].size/4);
  ctx.stroke();
  ctx.moveTo(players[0].x, players[0].y + players[0].size/4);
  ctx.lineTo(players[0].x - 10, players[0].y + players[0].size/2);
  ctx.stroke();
  ctx.moveTo(players[0].x, players[0].y + players[0].size/4);
  ctx.lineTo(players[0].x + 10, players[0].y + players[0].size/2);
  ctx.stroke();
  ctx.moveTo(players[0].x, players[0].y + players[0].size/4);
  ctx.lineTo(players[0].x - 10, players[0].y);
  ctx.stroke();
  ctx.moveTo(players[0].x, players[0].y + players[0].size/4);
  ctx.lineTo(players[0].x + 10, players[0].y);
  ctx.stroke();

  ctx.strokeStyle = '#ff0000';
  ctx.beginPath();
  ctx.arc(players[1].x, players[1].y - players[1].size/4, 10, 0, Math.PI*2);
  ctx.stroke();
  ctx.moveTo(players[1].x, players[1].y - players[1].size/4 + 10);
  ctx.lineTo(players[1].x, players[1].y + players[1].size/4);
  ctx.stroke();
  ctx.moveTo(players[1].x, players[1].y + players[1].size/4);
  ctx.lineTo(players[1].x - 10, players[1].y + players[1].size/2);
  ctx.stroke();
  ctx.moveTo(players[1].x, players[1].y + players[1].size/4);
  ctx.lineTo(players[1].x + 10, players[1].y + players[1].size/2);
  ctx.stroke();
  ctx.moveTo(players[1].x, players[1].y + players[1].size/4);
  ctx.lineTo(players[1].x - 10, players[1].y);
  ctx.stroke();
  ctx.moveTo(players[1].x, players[1].y + players[1].size/4);
  ctx.lineTo(players[1].x + 10, players[1].y);
  ctx.stroke();
}

function checkCollision() {
  for (let i=fishes.length-1; i>=0; i--) {
    const fish = fishes[i];
    for (let p of players) {
      const dist = Math.hypot(fish.x - p.x, fish.y - p.y);
      if (dist < fish.size/2 + p.size/2) {
        fish.x = Math.random() * (width - fish.size);
        fish.y = Math.random() * (height - fish.size);
        score++;
        updateScore();
        if (score >= targetScore) showMessage("Gratulálok! Nyertél!");
        break;
      }
    }
  }
  for (let rf of rareFishes) {
    if (rf.visible) {
      for (let p of players) {
        const dist = Math.hypot(rf.x - p.x, rf.y - p.y);
        if (dist < rf.size/2 + p.size/2) {
          rf.visible = false;
          score += 2;
          updateScore();
          if (score >= targetScore) showMessage("Gratulálok! Nyertél!");
          break;
        }
      }
    }
  }
}

function updateScore() {
  document.getElementById('scoreboard').textContent = `Pontszám: ${score} | Célpont: ${targetScore}`;
}

function showMessage(text) {
  const msgDiv = document.getElementById('message');
  msgDiv.textContent = text;
  msgDiv.style.display = 'block';
}

// Spawn ritka hal
function spawnRareFish() {
  if (rareFishes.length < 10 && Math.random() < 0.02) {
    const newFish = createRareFish();
    newFish.x = Math.random() * (width - newFish.size);
    newFish.y = Math.random() * (height - newFish.size);
    newFish.directionX = Math.random() < 0.5 ? -1 : 1;
    newFish.directionY = Math.random() < 0.5 ? -1 : 1;
    rareFishes.push(newFish);
  }
}

// Reset játék
function resetGame() {
  if (animationFrameId) {
    cancelAnimationFrame(animationFrameId);
    animationFrameId = null;
  }
  score = 0;
  updateScore();
  showMessage('');
  document.getElementById('message').style.display = 'none';

  // Halak pozíciók
  for (let fish of fishes) {
    fish.x = Math.random() * (width - fish.size);
    fish.y = Math.random() * (height - fish.size);
  }
  // Ritka halak
  rareFishes.length = 0;
  for (let i=0; i<3; i++) {
    rareFishes.push(createRareFish());
  }
  // Játékosok pozíciója
  players[0].x = width/3;
  players[0].y = height/2;
  players[1].x = 2*width/3;
  players[1].y = height/2;

  // Indítjuk a játékot
  gameLoop();
}

// Event a restart gombhoz
document.getElementById('resetGame').addEventListener('click', () => {
  if (gameStarted) {
    resetGame();
  }
});

// Start gomb
const loaderWrapper = document.getElementById('loader-wrapper');
const startButton = document.getElementById('startButton');

startButton.addEventListener('click', () => {
  // Elrejtjük a loader-t
  loaderWrapper.style.display = 'none';

  // Indítjuk a játékot
  if (!gameStarted) {
    gameStarted = true;
    startButton.style.display = 'none';
    gameLoop();

    // Indítjuk a zenét
    document.getElementById('backgroundMusic').play();
  }
});

// Buborékok
const bubblesContainer = document.getElementById('bubbles');
for(let i=0; i<20; i++) {
  const bubble = document.createElement('div');
  bubble.className = 'bubble';
  const size = Math.random() * 8 + 2;
  bubble.style.width = size + 'px';
  bubble.style.height = size + 'px';
  bubble.style.left = Math.random() * 100 + '%';
  bubble.style.animationDelay = Math.random() * 10 + 's';
  bubblesContainer.appendChild(bubble);
}

// Játék fő ciklus
function gameLoop() {
  if (!gameStarted || paused) return;
  animationFrameId = requestAnimationFrame(gameLoop);
  ctx.clearRect(0, 0, width, height);
  ctx.fillStyle = '#001f3f';
  ctx.fillRect(0, 0, width, height);

  // Halak mozgása
  for (let fish of fishes) {
    fish.move();
    fish.draw();
  }
  for (let rf of rareFishes) {
    if (rf.visible) {
      rf.move();
      rf.draw();
    }
  }

  spawnRareFish();
  movePlayers();
  drawPlayers();
  checkCollision();
  updateScore();

  if (score >= targetScore) {
    cancelAnimationFrame(animationFrameId);
    animationFrameId = null;
  }
}

// Háttérzene
const music = document.getElementById('backgroundMusic');

const musicToggleBtn = document.getElementById('musicToggle');
const pauseResumeBtn = document.getElementById('pauseResume');

let musicPlaying = true;
let gamePaused = false;

// Zene kikapcsolás és ikon váltás
musicToggleBtn.addEventListener('click', () => {
  if (musicPlaying) {
    music.pause();
    musicPlaying = false;
    // Ikon váltás: csendes ikon
    document.getElementById('musicToggle').textContent = '🔇';
  } else {
    music.play();
    musicPlaying = true;
    // Vissza az eredeti ikon
    document.getElementById('musicToggle').textContent = '🎵';
  }
});

// Szünet/Folytatás
pauseResumeBtn.addEventListener('click', () => {
  if (!gameStarted) return;
  if (!gamePaused) {
    paused = true;
    gamePaused = true;
    pauseResumeBtn.textContent = '▶️'; // Folytatás
    pauseResumeBtn.title = 'Folytatás';
  } else {
    paused = false;
    gamePaused = false;
    pauseResumeBtn.textContent = '⏸️'; // Szünet
    pauseResumeBtn.title = 'Szünet';
    gameLoop();
  }
});

// Indítjuk a zenét a start gombbal
document.getElementById('startButton').addEventListener('click', () => {
  music.play();
});
</script>
</body>
</html>

