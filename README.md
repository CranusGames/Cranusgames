<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Cranus Games</title>
<style>
html, body {
  margin: 0;
  padding: 0;
  background: #000;
  overflow: hidden;
}
canvas {
  display: block;
}
@font-face {
  font-family: 'Pixel';
  src: local('Courier New');
}
</style>
</head>
<body>
<canvas id="c"></canvas>
<script>
const canvas = document.getElementById('c');
const ctx = canvas.getContext('2d');

function resize() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}
window.addEventListener('resize', resize);
resize();

// Snow particles
const snow = [];
for (let i = 0; i < 120; i++) {
  snow.push({
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    s: Math.random() * 2 + 1,
    v: Math.random() * 0.5 + 0.2
  });
}

// Walker
let walkX = canvas.width * 0.4;
let frame = 0;

// Text timing
let t = 0;

function drawSnow() {
  ctx.fillStyle = '#fff';
  snow.forEach(p => {
    ctx.fillRect(p.x, p.y, p.s, p.s);
    p.y += p.v;
    if (p.y > canvas.height) {
      p.y = -5;
      p.x = Math.random() * canvas.width;
    }
  });
}

function drawWalker() {
  ctx.fillStyle = '#fff';
  const y = canvas.height * 0.75;

  // body
  ctx.fillRect(walkX, y, 8, 12);

  // legs (simple walk animation)
  if (frame % 20 < 10) {
    ctx.fillRect(walkX - 2, y + 12, 3, 6);
    ctx.fillRect(walkX + 7, y + 12, 3, 6);
  } else {
    ctx.fillRect(walkX + 1, y + 12, 3, 6);
    ctx.fillRect(walkX + 5, y + 12, 3, 6);
  }
}

function drawIdentity() {
  ctx.font = '20px Pixel';
  ctx.textAlign = 'center';

  const phase = (t / 200) % 3;
  let text = '';

  if (phase < 1) text = 'EMIRHAN';
  else if (phase < 2) text = 'CRANUS';
  else text = 'AYCIBIN';

  const alpha = Math.abs(Math.sin(t * 0.01));
  ctx.fillStyle = `rgba(255,255,255,${alpha})`;
  ctx.fillText(text, canvas.width / 2, canvas.height * 0.3);
}

function drawLink() {
  ctx.font = '14px Pixel';
  ctx.textAlign = 'left';

  const speed = 0.5;
  const x = canvas.width - ((t * speed) % (canvas.width + 400));
  ctx.fillStyle = 'rgba(200,200,200,0.8)';
  ctx.fillText('cranusgames.github.io/Cranusgames', x, canvas.height * 0.8);
}

function loop() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  drawSnow();
  drawIdentity();
  drawWalker();
  drawLink();

  frame++;
  t++;
  requestAnimationFrame(loop);
}

loop();
</script>
</body>
</html>
