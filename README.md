<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.7.2/css/all.min.css" rel="stylesheet">
    <style>
        body {
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: white;
            position: relative;
        }

        #main-menu {
            text-align: center;
        }

        #game-name-input {
            padding: 10px;
            margin-bottom: 10px;
        }

        #start-game-btn {
            background-color: #4CAF50;
            color: white;
            padding: 15px 32px;
            text-align: center;
            text-decoration: none;
            display: none;
            font-size: 16px;
            margin: 4px 2px;
            cursor: pointer;
        }

        #game-container {
            display: none;
            width: 800px;
            height: 600px;
            border: 1px solid #ccc;
            position: relative;
        }

        #number-five {
            width: 50px;
            height: 50px;
            font-size: 40px;
            text-align: center;
            position: absolute;
            bottom: 100px;
            left: 50px;
        }

        #pickup-btn {
            position: absolute;
            bottom: 200px;
            left: 50%;
            transform: translateX(-50%);
            display: none;
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
        }

        #game-over {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            display: none;
            font-size: 48px;
            color: red;
        }

        #number-one {
            width: 50px;
            height: 50px;
            font-size: 40px;
            text-align: center;
            position: absolute;
            bottom: 100px;
            right: 100px;
        }

        #congrats {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            display: none;
            font-size: 48px;
            color: green;
        }

        #announcement {
            position: absolute;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 10px;
            border-radius: 5px;
            display: none;
        }
    </style>
</head>

<body>
    <div id="main-menu">
        <input type="text" id="game-name-input" placeholder="请输入游戏名">
        <button id="start-game-btn">开始游戏</button>
    </div>
    <div id="game-container">
        <div id="number-five">5</div>
        <div id="number-one">1</div>
        <button id="pickup-btn">拾取</button>
        <div id="game-over">游戏失败</div>
        <div id="congrats"></div>
        <div id="announcement">a 向后走，d 向前走，w 跳起，跳一次速度慢一点，最多跳三次。</div>
    </div>

    <script>
        const mainMenu = document.getElementById('main-menu');
        const gameNameInput = document.getElementById('game-name-input');
        const startGameBtn = document.getElementById('start-game-btn');
        const gameContainer = document.getElementById('game-container');
        const numberFive = document.getElementById('number-five');
        const pickupBtn = document.getElementById('pickup-btn');
        const gameOver = document.getElementById('game-over');
        const numberOne = document.getElementById('number-one');
        const congrats = document.getElementById('congrats');
        const announcement = document.getElementById('announcement');

        let fiveX = 50;
        let fiveY = 100;
        let legCount = 0;
        let speed = 5;
        let gameName = '';

        gameNameInput.addEventListener('keydown', function (e) {
            if (e.key === 'Enter') {
                gameName = this.value;
                startGameBtn.style.display = 'block';
            }
        });

        startGameBtn.addEventListener('click', () => {
            mainMenu.style.display = 'none';
            gameContainer.style.display = 'block';
            announcement.style.display = 'block';
            setTimeout(() => {
                announcement.style.display = 'none';
            }, 4000);
        });

        document.addEventListener('keydown', (e) => {
            if (e.key === 'a') {
                fiveX -= speed;
            } else if (e.key === 'd') {
                fiveX += speed;
            } else if (e.key === 'w') {
                legCount++;
                if (legCount === 1) {
                    speed = 3;
                } else if (legCount === 2) {
                    speed = 1;
                } else if (legCount === 3) {
                    gameOver.style.display = 'block';
                    setTimeout(() => {
                        location.reload();
                    }, 3000);
                }
            }
            numberFive.style.left = fiveX + 'px';
            checkProximity();
        });

        function checkProximity() {
            const oneRect = numberOne.getBoundingClientRect();
            const fiveRect = numberFive.getBoundingClientRect();
            const distance = Math.sqrt(
                (oneRect.left - fiveRect.left) ** 2 + (oneRect.top - fiveRect.top) ** 2
            );
            if (distance < 100) {
                pickupBtn.style.display = 'block';
            } else {
                pickupBtn.style.display = 'none';
            }
        }

        pickupBtn.addEventListener('click', () => {
            congrats.textContent = `恭喜 ${gameName} 过关`;
            congrats.style.display = 'block';
            setTimeout(() => {
                mainMenu.style.display = 'block';
                gameContainer.style.display = 'none';
                legCount = 0;
                speed = 5;
                fiveX = 50;
                numberFive.style.left = fiveX + 'px';
                congrats.style.display = 'none';
                gameNameInput.value = '';
                startGameBtn.style.display = 'none';
            }, 2000);
        });
    </script>
</body>

</html>
    
