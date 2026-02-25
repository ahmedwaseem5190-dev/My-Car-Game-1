<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Super Car Pro</title>
    <style>
        body { margin: 0; background: #333; overflow: hidden; font-family: 'Arial', sans-serif; touch-action: manipulation; }
        /* Road ka design */
        #road { 
            width: 100vw; height: 100vh; background: #444; position: relative; 
            border-left: 10px solid #ffd700; border-right: 10px solid #ffd700;
            background-image: linear-gradient(#555 50%, transparent 50%);
            background-size: 100% 50px; animation: moveRoad 0.5s linear infinite;
        }
        @keyframes moveRoad { from { background-position: 0 0; } to { background-position: 0 50px; } }
        
        .car { width: 50px; height: 90px; position: absolute; display: flex; align-items: center; justify-content: center; font-size: 40px; }
        #player { bottom: 130px; left: 50%; transform: translateX(-50%); z-index: 5; transition: left 0.1s; }
        .enemy { top: -100px; }
        
        #score { position: absolute; top: 20px; left: 20px; color: white; font-size: 24px; z-index: 10; background: rgba(0,0,0,0.5); padding: 5px 15px; border-radius: 20px; }
        
        .controls { position: absolute; bottom: 30px; width: 100%; display: flex; justify-content: space-around; z-index: 20; }
        .btn { width: 80px; height: 80px; background: rgba(255,255,255,0.3); color: white; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 40px; border: 3px solid #fff; box-shadow: 0 5px 15px rgba(0,0,0,0.3); }
    </style>
</head>
<body>
    <div id="score">Score: 0</div>
    <div id="road">
        <div id="player" class="car">🏎️</div>
    </div>
    <div class="controls">
        <div class="btn" onclick="move(-50)">◀️</div>
        <div class="btn" onclick="move(50)">▶️</div>
    </div>

    <script>
        const player = document.getElementById('player');
        const road = document.getElementById('road');
        let score = 0;
        let pPos = window.innerWidth / 2 - 25;

        function move(val) {
            pPos += val;
            if(pPos < 20) pPos = 20;
            if(pPos > window.innerWidth - 70) pPos = window.innerWidth - 70;
            player.style.left = pPos + 'px';
        }

        function createEnemy() {
            const enemy = document.createElement('div');
            enemy.classList.add('car', 'enemy');
            // Dushman car ki types
            const enemyCars = ['🚔', '🚘', '🚖', '🚍'];
            enemy.innerHTML = enemyCars[Math.floor(Math.random() * enemyCars.length)];
            enemy.style.left = Math.random() * (window.innerWidth - 70) + 'px';
            road.appendChild(enemy);

            let top = -100;
            let speed = 5 + (score / 5); // Score barhne se speed barhegi
            
            let timer = setInterval(() => {
                top += speed;
                enemy.style.top = top + 'px';

                let p = player.getBoundingClientRect();
                let e = enemy.getBoundingClientRect();

                // Takrane ka logic
                if (!(p.right < e.left || p.left > e.right || p.bottom < e.top || p.top > e.bottom)) {
                    alert("CRASHED! Final Score: " + score);
                    location.reload();
                }

                if (top > window.innerHeight) {
                    enemy.remove();
                    score++;
                    document.getElementById('score').innerText = "Score: " + score;
                    clearInterval(timer);
                }
            }, 20);
        }

        setInterval(createEnemy, 1200);
    </script>
</body>
</html>
