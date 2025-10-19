<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Три в Ряд: Слабо Собрать? (Жуткий Пранк)</title>
    <style>
        body {
            background-color: #111;
            color: #fff;
            font-family: 'Courier New', monospace;
            text-align: center;
            margin: 0;
            padding: 0;
            overflow: hidden;
            background-image: url('https://i.imgur.com/4Z3kX9m.jpeg');
            background-size: cover;
        }
        #game-container {
            position: relative;
            width: 400px;
            height: 400px;
            margin: 50px auto;
            background-color: rgba(0, 0, 0, 0.8);
            border: 2px solid #ff0000;
            box-shadow: 0 0 20px rgba(255, 0, 0, 0.5);
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 2px;
        }
        .tile {
            width: 78px;
            height: 78px;
            background-color: #333;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 40px;
            cursor: pointer;
            transition: transform 0.2s;
        }
        .tile:hover {
            transform: scale(1.05);
        }
        #instructions {
            margin: 20px;
            font-size: 18px;
            color: #ff5555;
            text-shadow: 0 0 5px #ff0000;
        }
        #scare {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: #000;
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        #scare img {
            max-width: 80%;
            max-height: 80%;
            animation: shake 0.1s infinite;
        }
        #scare-text {
            position: absolute;
            bottom: 20px;
            color: #ff5555;
            font-size: 24px;
            text-shadow: 0 0 5px #ff0000;
            animation: fadeIn 1s;
        }
        #apology {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: #000;
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        #apology img {
            max-width: 60%;
            max-height: 60%;
            opacity: 0.8;
            animation: fadeIn 1s;
        }
        #apology-text {
            position: absolute;
            bottom: 20px;
            color: #ff0000;
            font-size: 30px;
            font-family: 'Chiller', cursive;
            text-shadow: 0 0 10px #ff0000;
            animation: pulse 1.5s infinite;
        }
        #reaction-form {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0,0,0,0.9);
            padding: 20px;
            border: 2px solid #ff0000;
            z-index: 1001;
        }
        #reaction-form input, #reaction-form textarea {
            width: 100%;
            margin: 10px 0;
            padding: 10px;
            background: #333;
            color: #fff;
            border: 1px solid #ff5555;
        }
        #reactions-gallery {
            margin: 50px auto;
            max-width: 600px;
            text-align: left;
        }
        .reaction-item {
            background: rgba(0,0,0,0.7);
            margin: 10px;
            padding: 15px;
            border-radius: 5px;
            border: 1px solid #ff5555;
        }
        .share-button {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin: 5px;
            color: white;
            font-size: 14px;
        }
        #share-x { background: #1da1f2; }
        #share-telegram { background: #0088cc; }
        #share-whatsapp { background: #25d366; }
        #share-facebook { background: #3b5998; }
        #share-vk { background: #4c75a3; }
        @keyframes shake {
            0% { transform: translate(0, 0); }
            50% { transform: translate(5px, 5px); }
            100% { transform: translate(0, 0); }
        }
        @keyframes fadeIn {
            0% { opacity: 0; }
            100% { opacity: 1; }
        }
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }
        #share {
            margin: 20px;
            font-size: 16px;
            color: #ff5555;
        }
    </style>
</head>
<body>
    <h1>Три в Ряд: Слабо Собрать?</h1>
    <p id="instructions">Кликай, чтобы менять местами плитки и собрать 3+ в ряд. Простая головоломка с сюрпризом! 😱</p>
    <div id="game-container"></div>
    <p id="share">Попался? Поделись с друзьями: 
        <button class="share-button" id="share-x" onclick="shareToX()">X</button>
        <button class="share-button" id="share-telegram" onclick="shareToTelegram()">Telegram</button>
        <button class="share-button" id="share-whatsapp" onclick="shareToWhatsApp()">WhatsApp</button>
        <button class="share-button" id="share-facebook" onclick="shareToFacebook()">Facebook</button>
        <button class="share-button" id="share-vk" onclick="shareToVK()">VK</button>
    </p>
    
    <div id="reactions-gallery">
        <h2>Реакции выживших:</h2>
        <div id="reactions-list"></div>
    </div>
    
    <div id="scare">
        <img src="https://pics.craiyon.com/2023-11-05/5f0b4a7a8c6b4e7b9e1a2b3c4d5e6f7g.webp" alt="Жуткий скример">
        <div id="scare-text">ХА, ПОПАЛСЯ!</div>
    </div>
    
    <div id="apology">
        <img id="apology-img" src="" alt="Прости за пранк">
        <div id="apology-text">ПРОСТИ, ЭТО БЫЛ ПРАНК! 😈</div>
    </div>
    
    <div id="reaction-form">
        <h3>Твоя реакция на скример:</h3>
        <input type="text" id="name-input" placeholder="Твоё имя (анонимно ок)">
        <textarea id="comment-input" placeholder="Что ты почувствовал? (Орёж? Смех? Месть?)" rows="3"></textarea>
        <button onclick="saveReaction()">Сохранить и закрыть</button>
        <button onclick="closeForm()">Закрыть без сохранения</button>
    </div>
    
    <audio id="scream" src="https://soundbible.com/mp3/Scream-Wilhelm.mp3"></audio>
    <audio id="scary-bg" src="https://soundbible.com/mp3/Scary_Ambience.mp3" loop></audio>

    <script>
        const gameContainer = document.getElementById('game-container');
        const scare = document.getElementById('scare');
        const apology = document.getElementById('apology');
        const apologyImg = document.getElementById('apology-img');
        const scream = document.getElementById('scream');
        const scaryBg = document.getElementById('scary-bg');
        const reactionForm = document.getElementById('reaction-form');
        const reactionsList = document.getElementById('reactions-list');

        let board = [];
        let selectedTile = null;
        let moveCount = 0;
        let gameActive = true;
        let reactions = JSON.parse(localStorage.getItem('screamerReactions')) || [];
        const colors = ['🔴', '🔵', '🟢', '🟡', '🟣'];
        const apologyImages = [
            'https://pics.craiyon.com/2023-10-15/8a9b7c6d2f4a4e3b9c1d2e3f4a5b6c7d.webp', // Призрак
            'https://pixabay.com/get/gf3b7a9c2e4f4e3b9c1d2e3f4a5b6c7d.jpg', // Череп
            'https://pics.craiyon.com/2023-12-10/7b8c9d0e1f2a4e3b9c1d2e3f4a5b6c7d.webp' // Силуэт
        ];
        let currentImageIndex = 0;

        // Загрузка реакций
        function loadReactions() {
            reactionsList.innerHTML = '';
            reactions.forEach(reaction => {
                const div = document.createElement('div');
                div.className = 'reaction-item';
                div.innerHTML = `<strong>${reaction.name || 'Аноним'}:</strong> ${reaction.comment} <small>(${reaction.date})</small>`;
                reactionsList.appendChild(div);
            });
        }
        loadReactions();

        // Создаём игровое поле
        function createBoard() {
            gameContainer.innerHTML = '';
            board = [];
            for (let i = 0; i < 5; i++) {
                board[i] = [];
                for (let j = 0; j < 5; j++) {
                    const tile = document.createElement('div');
                    tile.className = 'tile';
                    tile.dataset.row = i;
                    tile.dataset.col = j;
                    tile.innerText = colors[Math.floor(Math.random() * colors.length)];
                    board[i][j] = tile.innerText;
                    tile.addEventListener('click', () => handleTileClick(i, j));
                    gameContainer.appendChild(tile);
                }
            }
            // Таймер для случайного скримера
            setTimeout(() => {
                if (gameActive) triggerScare();
            }, 15000 + Math.random() * 5000);
        }

        function handleTileClick(row, col) {
            if (!gameActive) return;
            if (!selectedTile) {
                selectedTile = { row, col };
                gameContainer.children[row * 5 + col].style.backgroundColor = '#ff5555';
            } else {
                const prevTile = selectedTile;
                selectedTile = null;
                gameContainer.children[prevTile.row * 5 + prevTile.col].style.backgroundColor = '#333';
                
                // Проверяем, соседние ли плитки
                if (Math.abs(prevTile.row - row) + Math.abs(prevTile.col - col) === 1) {
                    // Меняем местами
                    const temp = board[prevTile.row][prevTile.col];
                    board[prevTile.row][prevTile.col] = board[row][col];
                    board[row][col] = temp;
                    updateBoard();
                    moveCount++;
                    if (moveCount >= 5) {
                        triggerScare();
                    }
                }
            }
        }

        function updateBoard() {
            for (let i = 0; i < 5; i++) {
                for (let j = 0; j < 5; j++) {
                    gameContainer.children[i * 5 + j].innerText = board[i][j];
                }
            }
        }

        createBoard();

        function triggerScare() {
            gameActive = false;
            scare.style.display = 'flex';
            scream.play();
            setTimeout(() => {
                scare.style.display = 'none';
                showApology();
            }, 4000);
        }

        function showApology() {
            apology.style.display = 'flex';
            scaryBg.play();
            currentImageIndex = 0;
            apologyImg.src = apologyImages[currentImageIndex];
            
            const interval = setInterval(() => {
                currentImageIndex++;
                if (currentImageIndex < apologyImages.length) {
                    apologyImg.src = apologyImages[currentImageIndex];
                } else {
                    clearInterval(interval);
                    apology.style.display = 'none';
                    scaryBg.pause();
                    scaryBg.currentTime = 0;
                    reactionForm.style.display = 'block';
                }
            }, 2000);
        }

        function saveReaction() {
            const name = document.getElementById('name-input').value || 'Аноним';
            const comment = document.getElementById('comment-input').value;
            if (comment) {
                reactions.push({ name, comment, date: new Date().toLocaleString() });
                localStorage.setItem('screamerReactions', JSON.stringify(reactions));
                loadReactions();
                closeForm();
            }
        }

        function closeForm() {
            reactionForm.style.display = 'none';
            document.getElementById('name-input').value = '';
            document.getElementById('comment-input').value = '';
            createBoard(); // Перезапускаем игру
            gameActive = true;
            moveCount = 0;
        }

        function shareToX() {
            const text = encodeURIComponent('Слабо собрать три в ряд? Я чуть не умер от скримера! 😱 #ScreamerPuzzle');
            const url = encodeURIComponent(window.location.href);
            window.open(`https://twitter.com/intent/tweet?text=${text}&url=${url}`, '_blank');
        }

        function shareToTelegram() {
            const text = encodeURIComponent('Слабо собрать три в ряд? Осторожно, жуткий сюрприз! 😱 #ScreamerPuzzle');
            const url = encodeURIComponent(window.location.href);
            window.open(`https://t.me/share/url?url=${url}&text=${text}`, '_blank');
        }

        function shareToWhatsApp() {
            const text = encodeURIComponent('Слабо собрать три в ряд? Я чуть не умер от скримера! 😱 #ScreamerPuzzle ' + window.location.href);
            window.open(`https://api.whatsapp.com/send?text=${text}`, '_blank');
        }

        function shareToFacebook() {
            const url = encodeURIComponent(window.location.href);
            window.open(`https://www.facebook.com/sharer/sharer.php?u=${url}`, '_blank');
        }

        function shareToVK() {
            const url = encodeURIComponent(window.location.href);
            const text = encodeURIComponent('Слабо собрать три в ряд? Жуткий сюрприз ждёт! 😱 #ScreamerPuzzle');
            window.open(`https://vk.com/share.php?url=${url}&title=${text}`, '_blank');
        }
    </script>
</body>
</html>
