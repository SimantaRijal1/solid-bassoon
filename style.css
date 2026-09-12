<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Flappy Bird Clone</title>
    <style>
        body { margin: 0; padding: 0; overflow: hidden; background: #333; font-family: 'Arial', sans-serif; touch-action: none; }
        canvas { display: block; margin: 0 auto; background: #70c5ce; cursor: pointer; }
        #ui { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; display: flex; flex-direction: column; justify-content: center; align-items: center; color: white; text-shadow: 2px 2px 4px #000; }
        .menu { pointer-events: auto; text-align: center; background: rgba(0,0,0,0.3); padding: 20px; border-radius: 15px; }
        button { padding: 15px 30px; font-size: 20px; margin-top: 15px; cursor: pointer; border: none; border-radius: 10px; background: #f8e71c; color: #d35400; font-weight: bold; }
    </style>
</head>
<body>

<div id="ui">
    <div id="start-menu" class="menu">
        <h1>Flappy Bird</h1>
        <p>Tap / Click / Space to Flap</p>
        <button onclick="startGame()">START GAME</button>
    </div>
    <div id="game-over" class="menu" style="display: none;">
        <h1>Game Over</h1>
        <p id="final-score"></p>
        <p id="best-score"></p>
        <button onclick="restartGame()">RESTART</button>
    </div>
</div>

<canvas id="gameCanvas"></canvas>

<script>
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
const ui = document.getElementById('ui');
const startMenu = document.getElementById('start-menu');
const gameOverMenu = document.getElementById('game-over');

// Game Constants & Variables
let gameRunning = false;
let score = 0;
let bestScore = localStorage.getItem('bestScore') || 0;
let bird = { x: 50, y: 150, v: 0, gravity: 0.25, jump: -4.5, size: 20, rotation: 0 };
let pipes = [];
let frame = 0;
let shake = 0;

function resize() {
    canvas.width = Math.min(window.innerWidth, 400);
    canvas.height = window.innerHeight;
}
window.addEventListener('resize', resize);
resize();

// Input Handling
const flap = () => {
    if (!gameRunning && startMenu.style.display !== 'none') startGame();
    else if (gameRunning) bird.v = bird.jump;
};
window.addEventListener('keydown', (e) => { if(e.code === 'Space' || e.code === 'Enter') flap(); });
canvas.addEventListener('mousedown', flap);
canvas.addEventListener('touchstart', (e) => { e.preventDefault(); flap(); }, {passive: false});

function startGame() {
    gameRunning = true;
    score = 0;
    pipes = [];
    bird = { x: 50, y: canvas.height/2, v: 0, gravity: 0.25, jump: -4.5, size: 20, rotation: 0 };
    startMenu.style.display = 'none';
    gameOverMenu.style.display = 'none';
    loop();
}

function gameOver() {
    gameRunning = false;
    if (score > bestScore) { bestScore = score; localStorage.setItem('bestScore', bestScore); }
    document.getElementById('final-score').innerText = `Score: ${score}`;
    document.getElementById('best-score').innerText = `Best: ${bestScore}`;
    gameOverMenu.style.display = 'block';
    shake = 10;
}

function restartGame() { startGame(); }

function update() {
    if (!gameRunning) return;
    
    bird.v += bird.gravity;
    bird.y += bird.v;
    bird.rotation = bird.v * 0.1;

    if (bird.y + bird.size > canvas.height || bird.y - bird.size < 0) gameOver();

    if (frame % 100 === 0) {
        let gap = 150;
        let pipeY = Math.random() * (canvas.height - 300) + 100;
        pipes.push({ x: canvas.width, y: pipeY, passed: false });
    }

    pipes.forEach((p, i) => {
        p.x -= 2;
        if (p.x + 50 < bird.x && !p.passed) { score++; p.passed = true; }
        
        // Collision
        if (bird.x + 10 > p.x && bird.x - 10 < p.x + 50 && 
           (bird.y - 10 < p.y - 75 || bird.y + 10 > p.y + 75)) gameOver();
        
        if (p.x < -50) pipes.splice(i, 1);
    });
    frame++;
}

function draw() {
    ctx.fillStyle = '#70c5ce';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    // Shake effect
    ctx.save();
    if (shake > 0) { ctx.translate(Math.random()*shake-shake/2, Math.random()*shake-shake/2); shake--; }

    // Draw Bird
    ctx.save();
    ctx.translate(bird.x, bird.y);
    ctx.rotate(bird.rotation);
    ctx.fillStyle = '#f8e71c';
    ctx.beginPath();
    ctx.arc(0, 0, 10, 0, Math.PI * 2);
    ctx.fill();
    ctx.fillStyle = 'orange'; // beak
    ctx.fillRect(5, -2, 10, 5);
    ctx.restore();

    // Draw Pipes
    pipes.forEach(p => {
        ctx.fillStyle = '#2ecc71';
        ctx.fillRect(p.x, 0, 50, p.y - 75); // Top
        ctx.fillRect(p.x, p.y + 75, 50, canvas.height); // Bottom
    });

    // Score
    ctx.fillStyle = 'white';
    ctx.font = '30px Arial';
    ctx.fillText(score, canvas.width/2 - 10, 50);

    ctx.restore();
}

function loop() {
    if (!gameRunning) return;
    update();
    draw();
    requestAnimationFrame(loop);
}
</script>
</body>
</html>