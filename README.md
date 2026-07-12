<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Benim Oyun Motorum</title>
    <style>
        /* CSS TASARIM KODLARIN */
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background-color: #1e1e1e;
            color: white;
        }
        #editor-container {
            display: flex;
            height: 100vh;
        }
        #sidebar {
            width: 200px;
            background-color: #252526;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        button {
            background-color: #0e639c;
            color: white;
            border: none;
            padding: 10px;
            cursor: pointer;
            border-radius: 4px;
        }
        button:hover {
            background-color: #1177bb;
        }
        #canvas-area {
            flex: 1;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        canvas {
            background-color: #333;
            border: 2px solid #555;
        }
    </style>
</head>
<body>

    <div id="editor-container">
        <!-- SOL MENÜ -->
        <div id="sidebar">
            <h3>Araçlar</h3>
            <button onclick="setTool('player')">Karakter Ekle</button>
            <button onclick="setTool('block')">Blok Ekle</button>
            <button onclick="runGame()">Oyunu Başlat</button>
        </div>
        <!-- OYUN ALANI -->
        <div id="canvas-area">
            <canvas id="gameCanvas" width="800" height="600"></canvas>
        </div>
    </div>

    <script>
        /* JAVASCRIPT OYUN MANTIĞI KODLARIN */
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        let currentTool = 'block';
        let gameObjects = [];
        let isRunning = false;

        // Tıklanan yere nesne ekleme
        canvas.addEventListener('click', (e) => {
            if (isRunning) return;
            const rect = canvas.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;

            // Nesneyi tam farenin ucuna ortalamak için 20 piksel çıkartıyoruz
            gameObjects.push({ x: x - 20, y: y - 20, type: currentTool, width: 40, height: 40 });
            draw();
        });

        function setTool(tool) {
            currentTool = tool;
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            gameObjects.forEach(obj => {
                if (obj.type === 'player') {
                    ctx.fillStyle = '#ff4757'; // Kırmızı karakter
                } else {
                    ctx.fillStyle = '#2ed573'; // Yeşil blok
                }
                ctx.fillRect(obj.x, obj.y, obj.width, obj.height);
            });
        }

        function runGame() {
            if (isRunning) return; // Çift tıklamayı önler
            isRunning = true;
            
            function gameLoop() {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                gameObjects.forEach(obj => {
                    if (obj.type === 'player' && obj.y < 560) {
                        obj.y += 2; // Basit yerçekimi düşüşü
                    }
                    if (obj.type === 'player') ctx.fillStyle = '#ff4757';
                    else ctx.fillStyle = '#2ed573';
                    ctx.fillRect(obj.x, obj.y, obj.width, obj.height);
                });
                if (isRunning) requestAnimationFrame(gameLoop);
            }
            gameLoop();
        }

        // İlk çizim
        draw();
    </script>
</body>
</html>
# Leyla
Selana
