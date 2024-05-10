HTML:
html
<!DOCTYPE html>
<html>
<head>
  <title>Jogo da Cobrinha</title>
  <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
  <canvas id="snakeCanvas" width="400" height="400"></canvas>

  <script src="script.js"></script>
</body>
</html>


CSS (style.css):
css
body {
  text-align: center;
}

canvas {
  border: 1px solid #333;
}


JavaScript (script.js):
javascript
// Configurações do jogo
const canvas = document.getElementById("snakeCanvas");
const context = canvas.getContext("2d");
const box = 20;
let snake = [];
snake[0] = { x: 10 * box, y: 10 * box };
let direction = "right";
let food = {
  x: Math.floor(Math.random() * 20) * box,
  y: Math.floor(Math.random() * 20) * box
};
let score = 0;

// Controles
document.addEventListener("keydown", changeDirection);

function changeDirection(event) {
  if (event.keyCode === 37 && direction !== "right") direction = "left";
  if (event.keyCode === 38 && direction !== "down") direction = "up";
  if (event.keyCode === 39 && direction !== "left") direction = "right";
  if (event.keyCode === 40 && direction !== "up") direction = "down";
}

// Função para desenhar elementos no canvas
function draw() {
  context.clearRect(0, 0, canvas.width, canvas.height);

  // Desenhar cobrinha
  for (let i = 0; i < snake.length; i++) {
    context.fillStyle = i === 0 ? "#00FF00" : "#FFFFFF";
    context.fillRect(snake[i].x, snake[i].y, box, box);
    context.strokeStyle = "#000000";
    context.strokeRect(snake[i].x, snake[i].y, box, box);
  }

  // Desenhar comida
  context.fillStyle = "#FF0000";
  context.fillRect(food.x, food.y, box, box);

  // Movimentação da cobrinha
  let snakeX = snake[0].x;
  let snakeY = snake[0].y;

  if (direction === "right") snakeX += box;
  if (direction === "left") snakeX -= box;
  if (direction === "up") snakeY -= box;
  if (direction === "down") snakeY += box;

  // Verificar colisão com a comida
  if (snakeX === food.x && snakeY === food.y) {
    score++;
    food = {
      x: Math.floor(Math.random() * 20) * box,
      y: Math.floor(Math.random() * 20) * box
    };
  } else {
    snake.pop();
  }

  // Verificar colisão com as paredes
  if (
    snakeX < 0 ||
    snakeX >= canvas.width ||
    snakeY < 0 ||
    snakeY >= canvas.height
  ) {
    clearInterval(game);
    alert("Game Over! Pontuação: " + score);
    window.location.reload();
  }

  // Verificar colisão com o próprio corpo
  for (let i = 1; i < snake.length; i++) {
    if (snakeX === snake[i].x && snakeY === snake[i].y) {
      clearInterval(game);
      alert("Game Over! Pontuação: " + score);
      window.location.reload();
    }
  }

  // Adicionar nova cabeça à cobrinha
  const newHead = { x: snakeX, y: snakeY };
  snake.unshift(newHead);

  // Mostrar pontuação
  context.fillStyle = "#000000";
  context.font = "20px Arial";
  context.fillText("Pontuação: " + score, box, 1.6 * box);
}

// Iniciar o jogo
const game = setInterval(draw, 200);
