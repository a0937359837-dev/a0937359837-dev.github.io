<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<title>DVD 反彈畫面</title>
<style>
  html, body {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
    background: #000;
    overflow: hidden;
  }
  canvas {
    display: block;
    width: 100vw;
    height: 100vh;
    background: #000;
  }
</style>
</head>
<body>
<canvas id="tv"></canvas>
<script>
const canvas = document.getElementById('tv');
const ctx = canvas.getContext('2d');
 
function resize() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}
resize();
window.addEventListener('resize', resize);
 
// --- 反彈的長方形 logo 區塊 ---
const box = {
  x: 100,
  y: 100,
  width: 220,
  height: 130,
  speedX: 2.8,
  speedY: 2.2,
  color: randomColor()
};
 
function randomColor() {
  const colors = [
    '#ff3b30', '#ff9500', '#ffcc00', '#34c759',
    '#00c7be', '#30b0c7', '#007aff', '#5856d6',
    '#af52de', '#ff2d55', '#ffffff'
  ];
  return colors[Math.floor(Math.random() * colors.length)];
}
 
function updateBox() {
  box.x += box.speedX;
  box.y += box.speedY;
 
  let bounced = false;
 
  if (box.x <= 0) {
    box.x = 0;
    box.speedX *= -1;
    bounced = true;
  } else if (box.x + box.width >= canvas.width) {
    box.x = canvas.width - box.width;
    box.speedX *= -1;
    bounced = true;
  }
 
  if (box.y <= 0) {
    box.y = 0;
    box.speedY *= -1;
    bounced = true;
  } else if (box.y + box.height >= canvas.height) {
    box.y = canvas.height - box.height;
    box.speedY *= -1;
    bounced = true;
  }
 
  if (bounced) {
    box.color = randomColor();
  }
}
 
// 原創的「DVD 風格」標誌：斜體粗體文字 + 下方橢圓唱片圖形
function drawLogo() {
  const w = box.width;
  const h = box.height;
  ctx.save();
  ctx.translate(box.x + w / 2, box.y + h / 2);
 
  // 文字部分
  ctx.font = `italic 900 ${h * 0.5}px "Arial Black", Impact, sans-serif`;
  ctx.fillStyle = box.color;
  ctx.textAlign = 'center';
  ctx.textBaseline = 'middle';
  ctx.fillText('DVD', 0, -h * 0.15);
 
  // 下方橢圓唱片
  ctx.beginPath();
  ctx.ellipse(0, h * 0.28, w * 0.42, h * 0.13, 0, 0, Math.PI * 2);
  ctx.fillStyle = box.color;
  ctx.fill();
 
  // 唱片中心孔（背景本身是黑色，直接用黑色填滿即可）
  ctx.beginPath();
  ctx.ellipse(0, h * 0.28, w * 0.13, h * 0.045, 0, 0, Math.PI * 2);
  ctx.fillStyle = '#000000';
  ctx.fill();
 
  ctx.restore();
}
 
function loop() {
  ctx.fillStyle = '#000000';
  ctx.fillRect(0, 0, canvas.width, canvas.height);
 
  updateBox();
  drawLogo();
 
  requestAnimationFrame(loop);
}
 
loop();
</script>
</body>
</html>
