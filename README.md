<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>For Eldar 🤍</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #E1C6FF;
        }
        h1 {
            color: #794DAF;
        }
        @keyframes heartbeat {
            0% { transform: scale(1); }
            100% { transform: scale(1.2); }
        }
        .btn {
            background-color: #794DAF;
            color: white;
            padding: 10px 20px;
            font-size: 18px;
            border: none;
            cursor: pointer;
            margin-top: 20px;
            border-radius: 5px;
        }
        /* Game styles */
        #gameContainer {
            position: relative;
            width: 400px;
            height: 600px;
            background-color: white;
            border-radius: 15px;
            border: 5px solid #794DAF;
            margin: 20px auto;
            overflow: hidden;
        }
        .heart {
            position: absolute;
            font-size: 30px;
            color: rgb(158, 84, 255);
        }
        #player {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 50px;
            width: 50px;
            height: 50px;
        }
        #score {
            font-size: 20px;
            margin-top: 10px;
        }
        .btn-back {
            background-color: #794DAF;
            color: white;
            padding: 10px 20px;
            font-size: 18px;
            border: none;
            cursor: pointer;
            border-radius: 5px;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <!-- First Page -->
    <div id="firstPage">
        <h1>С Днем святого Валентина, любимый!🤍</h1>
        <p> Eldar, You are the most special person in my life!</p>
        
        <button class="btn" onclick="showMessage()">Click Me</button>
        <p id="message"></p>
        <button class="btn" onclick="startGame()">Play Me </button>
    </div>

    <!-- Game Page (Initially hidden) -->
    <div id="gamePage" style="display: none;">
        <h1>paymay serdechku 🤍</h1>
        <p id="score">Score: 0 | Lives: 5</p>

        <div id="gameContainer">
            <div id="player">🫶</div>
        </div>

        <button class="btn-back" onclick="goBack()">Go Back 💖</button>
    </div>

    <script>
        let score = 0;
        let lives = 5;
        let playerSpeed = 40;

        // First page 
        function showMessage() {
            const messages = [
                "I love you more than words can say! 🤍",
                "You are my sunshine on a cloudy day ☀️",
                "I'm so lucky to have you in my life! 🌹",
                "Every moment with you is special 💞",
                "Every day, I love you more than the last! 💖",
                "Your arms are my favorite place in the world! 🤗💓",
            ];
            document.getElementById("message").innerText = messages[Math.floor(Math.random() * messages.length)];
        }

        // Switch to game 
        function startGame() {
            document.getElementById("firstPage").style.display = "none";
            document.getElementById("gamePage").style.display = "block";
            startGameLogic();
        }

        // Back to first 
        function goBack() {
            document.getElementById("gamePage").style.display = "none";
            document.getElementById("firstPage").style.display = "block";
        }

        // Game Logic
        function startGameLogic() {
            const gameContainer = document.getElementById("gameContainer");
            const player = document.getElementById("player");
            const scoreDisplay = document.getElementById("score");

            // upravleniya igrakom
            document.addEventListener("keydown", function(event) {
                let playerLeft = player.offsetLeft;

                if (event.key === "ArrowLeft" && playerLeft > 10) {
                    player.style.left = playerLeft - playerSpeed + "px";
                } else if (event.key === "ArrowRight" && playerLeft < gameContainer.clientWidth - 60) {
                    player.style.left = playerLeft + playerSpeed + "px";
                }
            });

            // function heart game
            function createHeart() {
                let heart = document.createElement("div");
                heart.classList.add("heart");
                heart.innerHTML = "💜";
                heart.style.left = Math.random() * (gameContainer.clientWidth - 30) + "px"; // Рандомная позиция
                heart.style.top = "-30px"; // Начальное положение
                gameContainer.appendChild(heart);

                let fallInterval = setInterval(() => {
                    let heartTop = heart.offsetTop;
                    heart.style.top = heartTop + 5 + "px";

                    if (heartTop > gameContainer.clientHeight - 70) { // Проверяем столкновение
                        let playerRect = player.getBoundingClientRect();
                        let heartRect = heart.getBoundingClientRect();

                        if (
                            heartRect.left < playerRect.right &&
                            heartRect.right > playerRect.left &&
                            heartRect.bottom > playerRect.top
                        ) {
                            score++;
                            gameContainer.removeChild(heart);
                            clearInterval(fallInterval);
                        } else {
                            lives--;
                            gameContainer.removeChild(heart);
                            clearInterval(fallInterval);
                        }

                        scoreDisplay.innerHTML = `Score: ${score} | Lives: ${lives}`;

                        if (lives <= 0) {
                            alert("Game Over prasti((! Your final score: " + score);
                            location.reload();
                        }
                    }
                }, 30);
            }

            // Запускаем игру (новое сердечко каждые 1 секунду)
            setInterval(createHeart, 1500);
        }
    </script>
</body>
</html>
