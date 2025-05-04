<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Endless Car Game</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body { margin: 0; font-family: Arial, sans-serif; background: #222; color: #fff; }
    canvas { display: block; margin: auto; background: #444; border: 5px solid #000; }

    #menu {
      text-align: center;
      padding-top: 50px;
    }

    .car-option {
      margin: 10px;
      cursor: pointer;
      display: inline-block;
    }

    .car-option img {
      width: 80px;
      height: 160px;
      border: 3px solid white;
      border-radius: 10px;
    }

    .car-option.selected img {
      border-color: limegreen;
    }

    #startButton {
      margin-top: 30px;
      padding: 10px 30px;
      font-size: 18px;
      cursor: pointer;
    }

    #gameOver {
      position: absolute;
      top: 40%;
      left: 50%;
      transform: translate(-50%, -50%);
      color: white;
      text-align: center;
      font-size: 2em;
      display: none;
    }
  </style>
</head>
<body>

<div id="menu">
  <h1>Select Your Car</h1>
  <div id="carSelection">
    <div class="car-option selected" data-car="lambo">
      <img src="https://i.imgur.com/Jz5VpBM.png" alt="Lambo">
    </div>
    <div class="car-option" data-car="porsche">
      <img src="https://i.imgur.com/gLZ1Ao6.png" alt="Porsche">
    </div>
    <div class="car-option" data-car="ferrari">
      <img src="https://i.imgur.com/oCPvu9n.png" alt="Ferrari">
    </div>
  </div>
  <button id="startButton">Start Game</button>
</div>

<canvas id="gameCanvas" width="400" height="600" style="display:none;"></canvas>
<div id="gameOver">Game Over!<br>Score: <span id="finalScore"></span><br><button onclick="location.reload()">Restart</button></div>

<audio id="engineSound" src="https://www.soundjay.com/transportation/sounds/car-engine-01.mp3" loop></audio>
<audio id="crashSound" src="https://www.soundjay.com/transportation/sounds/car-crash-1.mp3"></audio>

<script>
  const canvas = document.getElementById('gameCanvas');
  const ctx = canvas.getContext('2d');
  const carSelection = document.getElementById('carSelection');
  const startButton = document.getElementById('startButton');
  const gameOverScreen = document.getElementById('gameOver');
  const scoreDisplay = document.getElementById('finalScore');

  const engineSound = document.getElementById('engineSound');
  const crashSound = document.getElementById('crashSound');

  let selectedCar = 'lambo';
  const carImages = {
    lambo: 'https://i.imgur.com/Jz5VpBM.png',
    porsche: 'https://i.imgur.com/gLZ1Ao6.png',
    ferrari: 'https://i.imgur.com/oCPvu9n.png'
  };

  let carImg = new Image();

  document.querySelectorAll('.car-option').forEach(option => {
    option.addEventListener('click', () => {
      document.querySelectorAll('.car-option').forEach(opt => opt.classList.remove('selected'));
      option.classList.add('selected');
      selectedCar = option.dataset.car;
    });
  });

  startButton.addEventListener('click', () => {
    document.getElementById('menu').style.display = 'none';
    canvas.style.display = 'block';
    carImg.src = carImages[selectedCar];
    engineSound.play();
    loop();
  });

  const carWidth = 40;
  const carHeight = 80;
  const player = { x: 180, y: 500 };
  const roadX = 100;
  const roadWidth = 200;
  let keys = {};
  let speed = 5;
  let obstacles = [];
  let lines = [];
  let score = 0;
  let gameRunning = true;

  document.addEventListener('keydown', e => keys[e.key] = true);
  document.addEventListener('

