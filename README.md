# Good-morning-message-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Sunrise for Giselle</title>
    <style>
        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        body {
            margin: 0; padding: 0; overflow: hidden;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background: #1a1a2e; height: 100vh; width: 100vw;
            display: flex; justify-content: center; align-items: center;
            transition: background 3s ease;
        }

        /* Music & Start Overlay */
        #overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.85); z-index: 100;
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            color: white; text-align: center; padding: 20px;
        }
        #start-btn {
            background: #ff4d6d; color: white; border: none; padding: 18px 45px;
            font-size: 1.2rem; border-radius: 50px; cursor: pointer; margin-top: 25px;
            box-shadow: 0 0 25px rgba(255, 77, 109, 0.6); font-weight: bold;
        }

        #game-container { position: relative; width: 100%; height: 100%; max-width: 500px; overflow: hidden; display: flex; justify-content: center; align-items: center;}
        
        .ocean { position: absolute; bottom: 0; width: 100%; height: 25%; background: linear-gradient(to bottom, #1e3c72, #2a5298); z-index: 5; border-top: 2px solid rgba(255,255,255,0.1); }
        
        .sun { position: absolute; bottom: -80px; left: 50%; transform: translateX(-50%); width: 75px; height: 75px; background: radial-gradient(circle, #fff700, #ff8c00); border-radius: 50%; box-shadow: 0 0 50px #ff8c00; z-index: 2; transition: bottom 2.5s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
        
        #ui { position: absolute; top: 10%; width: 100%; text-align: center; z-index: 10; color: white; padding: 0 20px; text-shadow: 0 2px 10px rgba(0,0,0,0.7); }
        
        .score-box { background: rgba(255, 255, 255, 0.2); padding: 10px 20px; border-radius: 20px; backdrop-filter: blur(10px); display: inline-block; border: 1px solid rgba(255,255,255,0.3); margin-top: 15px; }

        .item { position: absolute; font-size: 40px; z-index: 15; user-select: none; cursor: pointer; animation: floatUp 2.8s linear forwards; }
        
        @keyframes floatUp { 
            from { opacity: 0; transform: translateY(20px) scale(0.5); } 
            15% { opacity: 1; transform: translateY(0) scale(1.2); }
            85% { opacity: 1; }
            to { opacity: 0; transform: translateY(-80px) scale(1); } 
        }

        #final-message { 
            display: none; position: relative; background: rgba(255, 255, 255, 0.95); 
            margin: 0 20px; padding: 30px 20px; border-radius: 30px; text-align: center; 
            z-index: 20; color: #d63384; box-shadow: 0 20px 50px rgba(0,0,0,0.3); 
            animation: popIn 1s cubic-bezier(0.175, 0.885, 0.32, 1.275); 
        }

        @keyframes popIn { from { transform: scale(0.8); opacity: 0; } to { transform: scale(1); opacity: 1; } }

        h1 { font-size: 1.6rem; margin-bottom: 15px; font-weight: 800; }
        p { font-size: 1.1rem; line-height: 1.5; color: #444; margin: 10px 0; }
    </style>
</head>
<body id="sky">

    <div id="overlay">
        <h1 style="margin:0;">Good Morning, Giselle ✨</h1>
        <p style="opacity: 0.9;">I made a little game for you to start your day.</p>
        <button id="start-btn" onclick="startGame()">Open Your Surprise</button>
    </div>

    <audio id="bg-music" loop>
        <source src="https://www.bensound.com/bensound-music/bensound-tenderness.mp3" type="audio/mpeg">
    </audio>

    <div id="game-container">
        <div class="sun" id="sun"></div>
        <div class="ocean"></div>

        <div id="ui">
            <h2 style="margin:0; font-size: 1.4rem;">Catch 5 Hearts to Start the Day! ❤️</h2>
            <div class="score-box">Hearts: <span id="score">0</span> / 5</div>
        </div>

        <div id="final-message">
            <h1>You're my sunshine! ☀️</h1>
            <p>Good morning, my love! I hope you have an amazing day at work today.</p>
            <p>Remember that you are <strong>absolutely gorgeous</strong> and <strong>completely perfect</strong> in every single way.</p>
            <p>I love you so much!</p>
            <div style="font-size: 2.5rem; margin-top: 15px;">❤️ 💋 ❤️</div>
        </div>
    </div>

    <script>
        let score = 0;
        const scoreEl = document.getElementById('score');
        const sun = document.getElementById('sun');
        const sky = document.body;
        const music = document.getElementById('bg-music');
        const overlay = document.getElementById('overlay');
        const finalMessage = document.getElementById('final-message');
        const ui = document.getElementById('ui');
        const container = document.getElementById('game-container');

        const emojis = { heart: '❤️', distractors: ['🌸', '⭐', '✨', '🌼'] };

        function startGame() {
            overlay.style.display = 'none';
            // Setting volume slightly lower for a gentle morning vibe
            music.volume = 0.6;
            music.play().catch(e => console.log("Audio play blocked. Tap the screen!"));
            setInterval(spawnItem, 900);
        }

        function spawnItem() {
            if (score >= 5) return;

            const isHeart = Math.random() > 0.45;
            const item = document.createElement('div');
            item.className = 'item';
            item.innerHTML = isHeart ? emojis.heart : emojis.distractors[Math.floor(Math.random() * emojis.distractors.length)];
            
            // Keeps items in the safe tapping zone of a phone (not too high, not too low)
            const x = (Math.random() * (container.offsetWidth - 80) + 40);
            const y = (Math.random() * (container.offsetHeight * 0.4) + (container.offsetHeight * 0.25));
            
            item.style.left = x + 'px';
            item.style.top = y + 'px';

            item.onclick = (e) => {
                e.stopPropagation();
                if (isHeart) {
                    score++;
                } else {
                    score = Math.max(0, score - 1);
                }
                updateSunrise();
                scoreEl.innerText = score;
                item.remove();
                if (score >= 5) winGame();
            };

            container.appendChild(item);
            // Auto-remove if not clicked to keep the screen clean
            setTimeout(() => { if(item.parentNode) item.remove(); }, 2800);
        }

        function updateSunrise() {
            const sunPositions = ['-80px', '-10px', '60px', '130px', '200px', '280px'];
            const skyColors = [
                'linear-gradient(to bottom, #1a1a2e, #16213e)',
                'linear-gradient(to bottom, #2c3e50, #4a4e69)',
                'linear-gradient(to bottom, #4a4e69, #9a8c98)',
                'linear-gradient(to bottom, #ff9a9e, #fad0c4)',
                'linear-gradient(to bottom, #ffecd2, #fcb69f)',
                'linear-gradient(to bottom, #87CEEB, #E0F7FA)'
            ];
            
            if(score <= 5) {
                sun.style.bottom = sunPositions[score];
                sky.style.background = skyColors[score];
            }
        }

        function winGame() {
            ui.style.display = 'none';
            finalMessage.style.display = 'block';
            // Cleanup remaining items
            document.querySelectorAll('.item').forEach(i => i.remove());
        }
    </script>
</body>
</html>
