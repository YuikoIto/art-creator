<template>
  <canvas ref="canvas" width="480" height="320"></canvas>
</template>
<script>
export default {
  data() {
    return {
      ballRadius: 10,
      x: 240,
      y: 290,
      dx: 2,
      dy: -2,
      paddleHeight: 10,
      paddleWidth: 75,
      paddleX: (480 - 75) / 2,
      rightPressed: false,
      leftPressed: false,
      brickRowCount: 5,
      brickColumnCount: 3,
      brickWidth: 75,
      brickHeight: 20,
      brickPadding: 10,
      brickOffsetTop: 30,
      brickOffsetLeft: 30,
      score: 0,
      lives: 3,
    };
  },
  mounted() {
    this.canvas = this.$refs.canvas;
    this.ctx = this.$refs.canvas.getContext("2d");
    this.draw();
  },
  methods: {
    collisionDetection() {
      for (let c = 0; c < this.brickColumnCount; c++) {
        for (let r = 0; r < this.brickRowCount; r++) {
          let b = this.bricks[c][r];
          if (b.status == 1) {
            if (
              this.x > b.x &&
              this.x < b.x + this.brickWidth &&
              this.y > b.y &&
              this.y < b.y + this.brickHeight
            ) {
              this.dy = -this.dy;
              b.status = 0;
              this.score++;
              if (this.score == this.brickRowCount * this.brickColumnCount) {
                alert("YOU WIN, CONGRATS!");
                document.location.reload();
              }
            }
          }
        }
      }
    },
    drawBall() {
      this.ctx.beginPath();
      this.ctx.arc(this.x, this.y, this.ballRadius, 0, Math.PI * 2);
      this.ctx.fillStyle = "#0095DD";
      this.ctx.fill();
      this.ctx.closePath();
    },
    drawPaddle() {
      this.ctx.beginPath();
      this.ctx.rect(
        this.paddleX,
        this.canvas.height - this.paddleHeight,
        this.paddleWidth,
        this.paddleHeight
      );
      this.ctx.fillStyle = "#0095DD";
      this.ctx.fill();
      this.ctx.closePath();
    },
    drawBricks() {
      for (let c = 0; c < this.brickColumnCount; c++) {
        for (let r = 0; r < this.brickRowCount; r++) {
          if (this.bricks[c][r].status == 1) {
            let brickX =
              r * (this.brickWidth + this.brickPadding) + this.brickOffsetLeft;
            let brickY = c * (brickHeight + brickPadding) + brickOffsetTop;
            this.bricks[c][r].x = brickX;
            this.bricks[c][r].y = brickY;
            this.ctx.beginPath();
            this.ctx.rect(brickX, brickY, this.brickWidth, this.brickHeight);
            this.ctx.fillStyle = "#0095DD";
            this.ctx.fill();
            this.ctx.closePath();
          }
        }
      }
    },
    drawScore() {
      this.ctx.font = "16px Arial";
      this.ctx.fillStyle = "#0095DD";
      this.ctx.fillText("Score: " + this.score, 8, 20);
    },
    drawLives() {
      this.ctx.font = "16px Arial";
      this.ctx.fillStyle = "#0095DD";
      this.ctx.fillText("Lives: " + this.lives, this.canvas.width - 65, 20);
    },
    draw() {
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
      this.drawBricks();
      this.drawBall();
      this.drawPaddle();
      this.drawScore();
      this.drawLives();
      this.collisionDetection();

      if (
        this.x + this.dx > this.canvas.width - this.ballRadius ||
        this.x + this.dx < this.ballRadius
      ) {
        this.dx = -this.dx;
      }
      if (this.y + this.dy < this.ballRadius) {
        this.dy = -this.dy;
      } else if (this.y + this.dy > this.canvas.height - this.ballRadius) {
        if (this.x > this.paddleX && x < this.paddleX + this.paddleWidth) {
          this.dy = -this.dy;
        } else {
          this.lives--;
          if (!this.lives) {
            alert("GAME OVER");
            document.location.reload();
          } else {
            this.x = this.canvas.width / 2;
            this.y = this.canvas.height - 30;
            this.dx = 3;
            this.dy = -3;
            this.paddleX = (this.canvas.width - this.paddleWidth) / 2;
          }
        }
      }

      if (
        this.rightPressed &&
        this.paddleX < this.canvas.width - this.paddleWidth
      ) {
        this.paddleX += 7;
      } else if (this.leftPressed && this.paddleX > 0) {
        this.paddleX -= 7;
      }

      this.x += this.dx;
      this.y += this.dy;
      this.requestAnimationFrame(draw);
    },
  },
};
</script>
