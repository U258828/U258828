<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Catch the Falling Items Game</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            font-family: Arial, sans-serif;
            background-color: #87CEEB;
        }

        #gameCanvas {
            display: block;
            background-color: #fff;
        }

        #score {
            position: absolute;
            top: 10px;
            left: 10px;
            font-size: 24px;
            color: #333;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas"></canvas>
    <div id="score">Score: 0</div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const scoreDisplay = document.getElementById('score');
        
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
        
        const player = {
            x: canvas.width / 2 - 25,
            y: canvas.height - 50,
            width: 50,
            height: 50,
            dx: 0,
            speed: 5,
            color: 'red'
        };

        const items = [];
        let score = 0;
        let gameInterval;
        let itemInterval;

        function drawPlayer() {
            ctx.fillStyle = player.color;
            ctx.fillRect(player.x, player.y, player.width, player.height);
        }

        function movePlayer() {
            player.x += player.dx;
            if (player.x < 0) player.x = 0;
            if (player.x + player.width > canvas.width) player.x = canvas.width - player.width;
        }

        function createItem() {
            const item = {
                x: Math.random() * (canvas.width - 20),
                y: 0,
                width: 20,
                height: 20,
                speed: Math.random() * 2 + 2,
                color: 'green'
            };
            items.push(item);
        }

        function drawItems() {
            items.forEach(item => {
                ctx.fillStyle = item.color;
                ctx.fillRect(item.x, item.y, item.width, item.height);
            });
        }

        function moveItems() {
            items.forEach(item => {
                item.y += item.speed;
            });
        }

        function checkCollision() {
            items.forEach((item, index) => {
                if (item.y + item.height > player.y &&
                    item.y < player.y + player.height &&
                    item.x + item.width > player.x &&
                    item.x < player.x + player.width) {
                    items.splice(index, 1);
                    score += 1;
                    scoreDisplay.innerText = `Score: ${score}`;
                }
            });
        }

        function endGame() {
            clearInterval(gameInterval);
            clearInterval(itemInterval);
            alert(`Game Over! Your final score is ${score}`);
        }

        function update() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            movePlayer();
            drawPlayer();
            moveItems();
            drawItems();
            checkCollision();

            items.forEach((item, index) => {
                if (item.y > canvas.height) {
                    endGame();
                }
            });
        }

        function startGame() {
            score = 0;
            items.length = 0;
            player.x = canvas.width / 2 - 25;
            scoreDisplay.innerText = 'Score: 0';
            gameInterval = setInterval(update, 20);
            itemInterval = setInterval(createItem, 1000);
        }

        document.addEventListener('keydown', (e) => {
            if (e.key === 'ArrowLeft') {
                player.dx = -player.speed;
            } else if (e.key === 'ArrowRight') {
                player.dx = player.speed;
            }
        });

        document.addEventListener('keyup', (e) => {
            if (e.key === 'ArrowLeft' || e.key === 'ArrowRight') {
                player.dx = 0;
            }
        });

        startGame();
    </script>
</body>
</html>
