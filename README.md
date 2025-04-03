# testing-
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Eliminate Useless Players</title>
    <style>
        body { margin: 0; overflow: hidden; background: #222; color: white; text-align: center; }
        canvas { display: block; background: #333; margin: auto; }
    </style>
</head>
<body>
    <h1>Eliminate Useless Players!</h1>
    <canvas id="gameCanvas"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        canvas.width = 800;
        canvas.height = 500;

        class Player {
            constructor() {
                this.x = 50;
                this.y = canvas.height / 2;
                this.width = 30;
                this.height = 30;
                this.speed = 5;
            }
            draw() {
                ctx.fillStyle = "blue";
                ctx.fillRect(this.x, this.y, this.width, this.height);
            }
        }

        class Enemy {
            constructor(x, y) {
                this.x = x;
                this.y = y;
                this.width = 30;
                this.height = 30;
            }
            draw() {
                ctx.fillStyle = "red";
                ctx.fillRect(this.x, this.y, this.width, this.height);
            }
        }

        class Bullet {
            constructor(x, y) {
                this.x = x;
                this.y = y;
                this.width = 10;
                this.height = 5;
                this.speed = 7;
            }
            move() {
                this.x += this.speed;
            }
            draw() {
                ctx.fillStyle = "yellow";
                ctx.fillRect(this.x, this.y, this.width, this.height);
            }
        }

        const player = new Player();
        const enemies = [new Enemy(600, 200), new Enemy(700, 300), new Enemy(650, 400)];
        const bullets = [];
        
        function update() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            player.draw();
            bullets.forEach((bullet, index) => {
                bullet.move();
                bullet.draw();
                enemies.forEach((enemy, enemyIndex) => {
                    if (bullet.x < enemy.x + enemy.width && bullet.x + bullet.width > enemy.x &&
                        bullet.y < enemy.y + enemy.height && bullet.y + bullet.height > enemy.y) {
                        enemies.splice(enemyIndex, 1);
                        bullets.splice(index, 1);
                    }
                });
            });
            enemies.forEach(enemy => enemy.draw());
            requestAnimationFrame(update);
        }
        
        document.addEventListener("keydown", (e) => {
            if (e.key === "ArrowUp" && player.y > 0) player.y -= player.speed;
            if (e.key === "ArrowDown" && player.y < canvas.height - player.height) player.y += player.speed;
            if (e.key === " ") bullets.push(new Bullet(player.x + player.width, player.y + player.height / 2));
        });
        
        update();
    </script>
</body>
</html>
