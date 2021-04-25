<template>
  <div class="mx-auto text-center">
    <canvas
      class="mx-auto border-double border-4 border-gray-600"
      id="game-canvas"
      width="300"
      height="320"
    ></canvas>
    <div class="flex justify-center items-center">
      <img
        id="left"
        class="transform rotate-180"
        src="~/assets/img/array.svg"
        alt="array"
        width="100px"
      />
      <img id="right" src="~/assets/img/array.svg" alt="array" width="100px" />
    </div>
  </div>
</template>
<script>
export default {
  mounted() {
    let canvas = document.getElementById("game-canvas");
    let ctx = canvas.getContext("2d");
    let ballRadius = 10;
    let x = canvas.width / 2;
    let y = canvas.height - 30;
    let dx = 2;
    let dy = -2;
    let paddleHeight = 10;
    let paddleWidth = 75;
    let paddleX = (canvas.width - paddleWidth) / 2;
    let rightPressed = false;
    let leftPressed = false;
    let brickRowCount = 6;
    let brickColumnCount = 4;
    let brickWidth = 32;
    let brickHeight = 20;
    let brickPadding = 10;
    let brickOffsetTop = 30;
    let brickOffsetLeft = 30;
    let score = 0;
    let lives = 3;

    let bricks = [];
    for (let c = 0; c < brickColumnCount; c++) {
      bricks[c] = [];
      for (let r = 0; r < brickRowCount; r++) {
        bricks[c][r] = { x: 0, y: 0, status: 1 };
      }
    }
    const left = document.getElementById("left");
    const right = document.getElementById("right");
    left.addEventListener("click", LeftkeyDownHandler, false);
    left.addEventListener("keyup", LeftkeyUpHandler, false);
    // document.addEventListener("mousemove", mouseMoveHandler, false);
    function LeftkeyDownHandler(e) {
      if (e.key == "Right" || e.key == "ArrowRight") {
        rightPressed = true;
      } else if (e.key == "Left" || e.key == "ArrowLeft") {
        leftPressed = true;
      }
    }

    function LeftkeyUpHandler(e) {
      if (e.key == "Right" || e.key == "ArrowRight") {
        rightPressed = false;
      } else if (e.key == "Left" || e.key == "ArrowLeft") {
        leftPressed = false;
      }
    }

    // function mouseMoveHandler(e) {
    //   let relativeX = e.clientX - canvas.offsetLeft;
    //   if (relativeX > 0 && relativeX < canvas.width) {
    //     paddleX = relativeX - paddleWidth / 2;
    //   }
    // }
    function collisionDetection() {
      for (let c = 0; c < brickColumnCount; c++) {
        for (let r = 0; r < brickRowCount; r++) {
          let b = bricks[c][r];
          if (b.status == 1) {
            if (
              x > b.x &&
              x < b.x + brickWidth &&
              y > b.y &&
              y < b.y + brickHeight
            ) {
              dy = -dy;
              b.status = 0;
              score++;
              //   if (score == brickRowCount * brickColumnCount) {
              //     // alert("YOU WIN, CONGRATS!");
              //     break;
              //   }
            }
          }
        }
      }
    }

    function drawBall() {
      ctx.beginPath();
      ctx.arc(x, y, ballRadius, 0, Math.PI * 2);
      ctx.fillStyle = "#455A64";
      ctx.fill();
      ctx.closePath();
    }
    function drawPaddle() {
      ctx.beginPath();
      ctx.rect(
        paddleX,
        canvas.height - paddleHeight,
        paddleWidth,
        paddleHeight
      );
      ctx.fillStyle = "#455A64";
      ctx.fill();
      ctx.closePath();
    }
    function drawBricks() {
      for (let c = 0; c < brickColumnCount; c++) {
        for (let r = 0; r < brickRowCount; r++) {
          if (bricks[c][r].status == 1) {
            let brickX = r * (brickWidth + brickPadding) + brickOffsetLeft;
            let brickY = c * (brickHeight + brickPadding) + brickOffsetTop;
            bricks[c][r].x = brickX;
            bricks[c][r].y = brickY;
            ctx.beginPath();
            ctx.rect(brickX, brickY, brickWidth, brickHeight);
            if (brickY / 30 === 1) {
              ctx.fillStyle = "#B39DDB";
            } else if (brickY / 30 === 2) {
              ctx.fillStyle = "#5E35B1";
            } else if (brickY / 30 === 3) {
              ctx.fillStyle = "#03A9F4";
            } else {
              ctx.fillStyle = "#FFD180";
            }
            ctx.fill();
            ctx.closePath();
          }
        }
      }
    }
    function drawScore() {
      ctx.font = "16px Arial";
      ctx.fillStyle = "#455A64";
      ctx.fillText("点数: " + score, 10, 20);
    }
    function drawLives() {
      ctx.font = "16px Arial";
      ctx.fillStyle = "#455A64";
      ctx.fillText("残り: " + lives + "回", canvas.width - 80, 20);
    }

    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawBricks();
      drawBall();
      drawPaddle();
      drawScore();
      drawLives();
      collisionDetection();

      if (x + dx > canvas.width - ballRadius || x + dx < ballRadius) {
        dx = -dx;
      }
      if (score == brickRowCount * brickColumnCount) {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        drawBricks();
        drawBall();
        drawPaddle();
        drawScore();
        drawLives();
        collisionDetection();
        return;
      }
      if (y + dy < ballRadius) {
        dy = -dy;
      } else if (y + dy > canvas.height - ballRadius) {
        if (x > paddleX && x < paddleX + paddleWidth) {
          dy = -dy;
        } else {
          lives--;
          if (!lives) {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawBricks();
            drawBall();
            drawPaddle();
            drawScore();
            drawLives();
            collisionDetection();
            return;
          } else {
            x = canvas.width / 2;
            y = canvas.height - 30;
            dx = 3;
            dy = -3;
            paddleX = (canvas.width - paddleWidth) / 2;
          }
        }
      }

      if (rightPressed && paddleX < canvas.width - paddleWidth) {
        paddleX += 7;
      } else if (leftPressed && paddleX > 0) {
        paddleX -= 7;
      }

      x += dx;
      y += dy;
      requestAnimationFrame(draw);
    }

    draw();
  },
};
</script>
