<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>天气骰子飞行棋</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Comic Sans MS', '微软雅黑', sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #a1c4fd 0%, #c2e9fb 100%);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            color: #333;
            overflow-x: hidden;
        }
        
        .container {
            max-width: 1200px;
            width: 100%;
            text-align: center;
        }
        
        h1 {
            color: #ff6b6b;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
            font-size: 2.8rem;
        }
        
        .subtitle {
            color: #5a67d8;
            margin-bottom: 30px;
            font-size: 1.5rem;
        }
        
        .setup-screen {
            background: white;
            border-radius: 25px;
            padding: 30px;
            margin-bottom: 30px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            text-align: center;
            width: 90%;
            max-width: 600px;
            margin: 0 auto 30px;
        }
        
        .setup-options {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 15px;
            margin: 25px 0;
        }
        
        .player-count-btn {
            background: #5a67d8;
            color: white;
            border: none;
            border-radius: 50px;
            padding: 18px 30px;
            font-size: 1.3rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            min-width: 120px;
        }
        
        .player-count-btn:hover {
            background: #4c51bf;
            transform: translateY(-5px);
        }
        
        .player-count-btn.active {
            background: #ff6b6b;
            transform: scale(1.1);
        }
        
        .game-area {
            display: none;
            flex-wrap: wrap;
            justify-content: center;
            gap: 30px;
            margin-bottom: 30px;
            width: 100%;
        }
        
        .board-container {
            flex: 1;
            min-width: 500px;
            max-width: 600px;
            position: relative;
        }
        
        .board {
            width: 100%;
            aspect-ratio: 1;
            background: #ffd166;
            border-radius: 50%;
            position: relative;
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2);
            overflow: hidden;
            border: 15px solid #8b4513;
        }
        
        .center-rainbow {
            position: absolute;
            width: 30%;
            height: 30%;
            top: 35%;
            left: 35%;
            border-radius: 50%;
            background: linear-gradient(45deg, #ff6b6b, #ffd166, #4ecdc4, #5a67d8, #6a0572);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            font-size: 1.5rem;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
            z-index: 5;
        }
        
        .path {
            position: absolute;
            width: 80%;
            height: 80%;
            top: 10%;
            left: 10%;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.7);
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .path-inner {
            width: 90%;
            height: 90%;
            border-radius: 50%;
            background: linear-gradient(45deg, #a8edea 0%, #fed6e3 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
        }
        
        .player {
            width: 35px;
            height: 35px;
            border-radius: 50%;
            position: absolute;
            transition: all 0.8s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: white;
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.3);
            z-index: 10;
            font-size: 1.2rem;
            border: 3px solid white;
        }
        
        .player-1 {
            background: #ff6b6b;
        }
        
        .player-2 {
            background: #4ecdc4;
        }
        
        .player-3 {
            background: #ffd166;
        }
        
        .player-4 {
            background: #5a67d8;
        }
        
        .player-5 {
            background: #6a0572;
        }
        
        .player-6 {
            background: #ff9a9e;
        }
        
        .player-7 {
            background: #a8edea;
            color: #333;
        }
        
        .player-8 {
            background: #fad0c4;
            color: #333;
        }
        
        .controls {
            flex: 1;
            min-width: 300px;
            max-width: 500px;
            background: white;
            border-radius: 25px;
            padding: 25px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 25px;
        }
        
        .dice-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
            width: 100%;
        }
        
        .dice {
            width: 150px;
            height: 150px;
            background: white;
            border-radius: 25px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 5rem;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
            cursor: pointer;
            transition: transform 0.3s;
            border: 5px solid #ffd166;
        }
        
        .dice:hover {
            transform: scale(1.05);
        }
        
        .dice.rolling {
            animation: roll 0.5s ease-in-out infinite;
        }
        
        @keyframes roll {
            0% { transform: rotate(0deg); }
            25% { transform: rotate(90deg); }
            50% { transform: rotate(180deg); }
            75% { transform: rotate(270deg); }
            100% { transform: rotate(360deg); }
        }
        
        .action-area {
            background: #f8f9fa;
            border-radius: 20px;
            padding: 20px;
            width: 100%;
            min-height: 150px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 15px;
            border: 3px dashed #5a67d8;
        }
        
        .action-icon {
            font-size: 4rem;
            margin-bottom: 15px;
        }
        
        .action-text {
            font-size: 1.8rem;
            font-weight: bold;
            color: #5a67d8;
        }
        
        .action-description {
            font-size: 1.3rem;
            color: #666;
            text-align: center;
        }
        
        .players-info {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            justify-content: center;
            width: 100%;
        }
        
        .player-info {
            background: #f1f3f4;
            border-radius: 15px;
            padding: 15px;
            min-width: 140px;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        
        .player-info.active {
            background: #e3f2fd;
            box-shadow: 0 0 0 3px #5a67d8;
            transform: scale(1.05);
        }
        
        .player-color {
            width: 25px;
            height: 25px;
            border-radius: 50%;
        }
        
        .player-name {
            font-weight: bold;
            font-size: 1.2rem;
        }
        
        .player-position {
            font-size: 1rem;
            color: #666;
        }
        
        button {
            background: #5a67d8;
            color: white;
            border: none;
            border-radius: 50px;
            padding: 18px 35px;
            font-size: 1.5rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
        }
        
        button:hover {
            background: #4c51bf;
            transform: translateY(-5px);
        }
        
        button:active {
            transform: translateY(0);
        }
        
        .big-button {
            padding: 20px 40px;
            font-size: 1.8rem;
        }
        
        .rules {
            background: white;
            border-radius: 25px;
            padding: 25px;
            margin-top: 30px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            text-align: left;
            width: 90%;
            max-width: 800px;
            margin: 30px auto;
        }
        
        .rules h2 {
            color: #ff6b6b;
            margin-bottom: 20px;
            text-align: center;
            font-size: 2.2rem;
        }
        
        .rules ul {
            list-style-type: none;
            padding-left: 0;
        }
        
        .rules li {
            margin-bottom: 15px;
            padding-left: 35px;
            position: relative;
            font-size: 1.2rem;
        }
        
        .rules li:before {
            content: "•";
            color: #5a67d8;
            font-size: 2rem;
            position: absolute;
            left: 0;
            top: -5px;
        }
        
        .win-message {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 100;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.5s;
        }
        
        .win-message.show {
            opacity: 1;
            pointer-events: all;
        }
        
        .win-content {
            background: white;
            border-radius: 30px;
            padding: 40px;
            text-align: center;
            max-width: 500px;
            width: 90%;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
            position: relative;
            overflow: hidden;
        }
        
        .rainbow-effect {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(45deg, 
                rgba(255, 107, 107, 0.3) 0%, 
                rgba(255, 209, 102, 0.3) 20%, 
                rgba(78, 205, 196, 0.3) 40%, 
                rgba(90, 103, 216, 0.3) 60%, 
                rgba(106, 5, 114, 0.3) 80%);
            z-index: -1;
            animation: rainbowPulse 2s infinite alternate;
        }
        
        @keyframes rainbowPulse {
            0% { opacity: 0.5; }
            100% { opacity: 1; }
        }
        
        .win-content h2 {
            color: #ff6b6b;
            margin-bottom: 20px;
            font-size: 2.5rem;
        }
        
        .win-content p {
            margin-bottom: 30px;
            font-size: 1.8rem;
        }
        
        .move-animation {
            position: absolute;
            font-size: 2rem;
            font-weight: bold;
            color: #ff6b6b;
            z-index: 20;
            animation: moveText 1.5s ease-out forwards;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        
        @keyframes moveText {
            0% { transform: translateY(0); opacity: 1; }
            100% { transform: translateY(-80px); opacity: 0; }
        }
        
        .action-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.85);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 200;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.5s;
        }
        
        .action-modal.show {
            opacity: 1;
            pointer-events: all;
        }
        
        .action-modal-content {
            background: white;
            border-radius: 30px;
            padding: 40px;
            text-align: center;
            max-width: 600px;
            width: 90%;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
        }
        
        .action-modal-icon {
            font-size: 8rem;
            margin-bottom: 25px;
        }
        
        .action-modal-text {
            font-size: 2.5rem;
            font-weight: bold;
            color: #5a67d8;
            margin-bottom: 20px;
        }
        
        .action-modal-description {
            font-size: 1.8rem;
            color: #666;
            margin-bottom: 25px;
            line-height: 1.5;
        }
        
        .action-modal-steps {
            font-size: 2rem;
            color: #ff6b6b;
            font-weight: bold;
            margin-bottom: 30px;
        }
        
        @media (max-width: 768px) {
            .game-area {
                flex-direction: column;
            }
            
            .board-container, .controls {
                max-width: 100%;
                min-width: 300px;
            }
            
            h1 {
                font-size: 2.2rem;
            }
            
            .subtitle {
                font-size: 1.2rem;
            }
            
            .dice {
                width: 120px;
                height: 120px;
                font-size: 4rem;
            }
            
            button {
                padding: 15px 25px;
                font-size: 1.3rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🌈 天气骰子飞行棋 🌈</h1>
        <p class="subtitle">掷骰子，根据天气前进并做出有趣的动作！</p>
        
        <div class="setup-screen" id="setup-screen">
            <h2>选择游戏人数</h2>
            <p>请选择玩家数量：</p>
            <div class="setup-options">
                <button class="player-count-btn" data-players="2">2人游戏</button>
                <button class="player-count-btn" data-players="3">3人游戏</button>
                <button class="player-count-btn" data-players="4">4人游戏</button>
                <button class="player-count-btn" data-players="5">5人游戏</button>
                <button class="player-count-btn" data-players="6">6人游戏</button>
                <button class="player-count-btn" data-players="7">7人游戏</button>
                <button class="player-count-btn" data-players="8">8人游戏</button>
            </div>
            <button id="start-game-btn" class="big-button">开始游戏</button>
        </div>
        
        <div class="game-area" id="game-area">
            <div class="board-container">
                <div class="board" id="game-board">
                    <div class="center-rainbow">彩虹终点</div>
                    <div class="path">
                        <div class="path-inner">
                            <!-- 玩家棋子将通过JS动态生成 -->
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="controls">
                <div class="dice-container">
                    <h2>天气骰子</h2>
                    <div class="dice" id="dice">🎲</div>
                    <button id="roll-btn" class="big-button">掷骰子</button>
                </div>
                
                <div class="action-area" id="action-area">
                    <div class="action-icon">☀️</div>
                    <div class="action-text">晴天</div>
                    <div class="action-description">等待掷骰子...</div>
                </div>
                
                <div class="players-info" id="players-info">
                    <!-- 玩家信息将通过JS动态生成 -->
                </div>
            </div>
        </div>
    </div>
    
    <div class="win-message" id="win-message">
        <div class="win-content">
            <div class="rainbow-effect"></div>
            <h2>🎉 恭喜！ 🎉</h2>
            <p id="winner-text">玩家1获胜！</p>
            <button id="play-again-btn" class="big-button">再玩一次</button>
        </div>
    </div>
    
    <div class="action-modal" id="action-modal">
        <div class="action-modal-content">
            <div class="action-modal-icon" id="action-modal-icon">☀️</div>
            <div class="action-modal-text" id="action-modal-text">晴天</div>
            <div class="action-modal-steps" id="action-modal-steps">前进3步</div>
            <div class="action-modal-description" id="action-modal-description">小手围圆做"太阳"动作，喊"太阳出来啦~"</div>
            <button id="action-complete-btn" class="big-button">我完成啦！</button>
        </div>
    </div>

    <div class="rules">
        <h2>游戏规则</h2>
        <ul>
            <li><strong>晴天 (太阳图标)</strong>: 走3步，小手围圆做"太阳"动作，喊"太阳出来啦~"</li>
            <li><strong>下雨 (粉色哭脸云)</strong>: 走2步，做一个鬼脸表情~</li>
            <li><strong>多云 (叠叠云)</strong>: 走2步，找旁边好朋友轻轻抱一抱~</li>
            <li><strong>刮风 (歪脸飘云)</strong>: 走1步，原地慢慢转1个小圈圈~</li>
            <li><strong>下雪 (围巾云)</strong>: 走1步，小手张开模仿"雪花飘"晃动手臂~</li>
            <li><strong>雷雨 (闪电紫云)</strong>: 走1步，身体比S型模拟"闪电"</li>
        </ul>
        <p style="text-align: center; margin-top: 20px; font-weight: bold; font-size: 1.5rem;">最先走到中心彩虹终点的小朋友获胜！</p>
    </div>

    <script>
        // 游戏数据
        const weatherTypes = [
            {
                name: "晴天",
                emoji: "☀️",
                steps: 3,
                description: "小手围圆做\"太阳\"动作，喊\"太阳出来啦~\"",
                sound: "sunny",
                speech: "晴天！前进三步，小手围圆做太阳动作，喊太阳出来啦"
            },
            {
                name: "下雨",
                emoji: "🌧️",
                steps: 2,
                description: "做一个鬼脸表情~",
                sound: "rainy",
                speech: "下雨！前进两步，做一个鬼脸表情"
            },
            {
                name: "多云",
                emoji: "☁️",
                steps: 2,
                description: "找旁边好朋友轻轻抱一抱~",
                sound: "cloudy",
                speech: "多云！前进两步，找旁边好朋友轻轻抱一抱"
            },
            {
                name: "刮风",
                emoji: "💨",
                steps: 1,
                description: "原地慢慢转1个小圈圈~",
                sound: "windy",
                speech: "刮风！前进一步，原地慢慢转一个小圈圈"
            },
            {
                name: "下雪",
                emoji: "❄️",
                steps: 1,
                description: "小手张开模仿\"雪花飘\"晃动手臂~",
                sound: "snowy",
                speech: "下雪！前进一步，小手张开模仿雪花飘晃动手臂"
            },
            {
                name: "雷雨",
                emoji: "⛈️",
                steps: 1,
                description: "身体比S型模拟\"闪电\"",
                sound: "thunder",
                speech: "雷雨！前进一步，身体比S型模拟闪电"
            }
        ];

        // 游戏状态
        let gameState = {
            players: [],
            currentPlayer: 0,
            gameOver: false,
            waitingForAction: false,
            playerCount: 2,
            totalPositions: 15  // 从20步减少到15步
        };

        // DOM元素
        const setupScreen = document.getElementById('setup-screen');
        const gameArea = document.getElementById('game-area');
        const gameBoard = document.getElementById('game-board');
        const pathInner = document.querySelector('.path-inner');
        const diceElement = document.getElementById('dice');
        const rollButton = document.getElementById('roll-btn');
        const actionArea = document.getElementById('action-area');
        const playersInfo = document.getElementById('players-info');
        const winMessage = document.getElementById('win-message');
        const winnerText = document.getElementById('winner-text');
        const playAgainButton = document.getElementById('play-again-btn');
        const actionModal = document.getElementById('action-modal');
        const actionModalIcon = document.getElementById('action-modal-icon');
        const actionModalText = document.getElementById('action-modal-text');
        const actionModalSteps = document.getElementById('action-modal-steps');
        const actionModalDescription = document.getElementById('action-modal-description');
        const actionCompleteBtn = document.getElementById('action-complete-btn');
        const playerCountButtons = document.querySelectorAll('.player-count-btn');
        const startGameButton = document.getElementById('start-game-btn');

        // 语音合成
        function speak(text) {
            if ('speechSynthesis' in window) {
                // 停止任何正在播放的语音
                speechSynthesis.cancel();
                
                const utterance = new SpeechSynthesisUtterance(text);
                utterance.lang = 'zh-CN';
                utterance.rate = 0.8;
                utterance.pitch = 1.3;
                utterance.volume = 1;
                
                // 设置儿童友好的声音（如果可用）
                const voices = speechSynthesis.getVoices();
                const childVoice = voices.find(voice => 
                    voice.lang.includes('zh') && (voice.name.includes('Child') || voice.name.includes('Kids'))
                );
                
                if (childVoice) {
                    utterance.voice = childVoice;
                }
                
                speechSynthesis.speak(utterance);
            }
        }

        // 音效
        const audioContext = new (window.AudioContext || window.webkitAudioContext)();
        
        function playSound(type) {
            try {
                const oscillator = audioContext.createOscillator();
                const gainNode = audioContext.createGain();
                
                oscillator.connect(gainNode);
                gainNode.connect(audioContext.destination);
                
                // 根据天气类型设置不同的音调
                switch(type) {
                    case "sunny":
                        oscillator.frequency.setValueAtTime(523.25, audioContext.currentTime); // C5
                        break;
                    case "rainy":
                        oscillator.frequency.setValueAtTime(493.88, audioContext.currentTime); // B4
                        break;
                    case "cloudy":
                        oscillator.frequency.setValueAtTime(440.00, audioContext.currentTime); // A4
                        break;
                    case "windy":
                        oscillator.frequency.setValueAtTime(392.00, audioContext.currentTime); // G4
                        break;
                    case "snowy":
                        oscillator.frequency.setValueAtTime(349.23, audioContext.currentTime); // F4
                        break;
                    case "thunder":
                        oscillator.frequency.setValueAtTime(293.66, audioContext.currentTime); // D4
                        break;
                    default:
                        oscillator.frequency.setValueAtTime(440.00, audioContext.currentTime); // A4
                }
                
                gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
                gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.5);
                
                oscillator.start(audioContext.currentTime);
                oscillator.stop(audioContext.currentTime + 0.5);
            } catch (e) {
                console.log("音频播放出错:", e);
            }
        }
        
        function playStepSound(steps) {
            try {
                for (let i = 0; i < steps; i++) {
                    setTimeout(() => {
                        const oscillator = audioContext.createOscillator();
                        const gainNode = audioContext.createGain();
                        
                        oscillator.connect(gainNode);
                        gainNode.connect(audioContext.destination);
                        
                        oscillator.frequency.setValueAtTime(261.63, audioContext.currentTime); // C4
                        gainNode.gain.setValueAtTime(0.2, audioContext.currentTime);
                        gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.2);
                        
                        oscillator.start(audioContext.currentTime);
                        oscillator.stop(audioContext.currentTime + 0.2);
                    }, i * 400);
                }
            } catch (e) {
                console.log("步数音效播放出错:", e);
            }
        }

        // 初始化游戏
        function initGame() {
            // 事件监听
            rollButton.addEventListener('click', rollDice);
            playAgainButton.addEventListener('click', resetGame);
            actionCompleteBtn.addEventListener('click', completeAction);
            
            // 玩家数量选择
            playerCountButtons.forEach(button => {
                button.addEventListener('click', function() {
                    playerCountButtons.forEach(btn => btn.classList.remove('active'));
                    this.classList.add('active');
                    gameState.playerCount = parseInt(this.dataset.players);
                });
            });
            
            // 开始游戏按钮
            startGameButton.addEventListener('click', startGame);
            
            // 默认选择两人游戏
            playerCountButtons[0].classList.add('active');
        }

        // 开始游戏
        function startGame() {
            setupScreen.style.display = 'none';
            gameArea.style.display = 'flex';
            
            // 初始化玩家
            initializePlayers();
            
            // 创建玩家棋子
            createPlayerPieces();
            
            // 渲染玩家信息
            renderPlayersInfo();
            
            // 更新玩家位置
            updatePlayerPositions();
            
            // 欢迎语音
            setTimeout(() => {
                speak(`游戏开始！${gameState.playerCount}人游戏，请${gameState.players[0].name}掷骰子`);
            }, 1000);
        }

        // 初始化玩家
        function initializePlayers() {
            gameState.players = [];
            const playerColors = [
                { id: 1, name: "玩家1", color: "#ff6b6b" },
                { id: 2, name: "玩家2", color: "#4ecdc4" },
                { id: 3, name: "玩家3", color: "#ffd166" },
                { id: 4, name: "玩家4", color: "#5a67d8" },
                { id: 5, name: "玩家5", color: "#6a0572" },
                { id: 6, name: "玩家6", color: "#ff9a9e" },
                { id: 7, name: "玩家7", color: "#a8edea" },
                { id: 8, name: "玩家8", color: "#fad0c4" }
            ];
            
            for (let i = 0; i < gameState.playerCount; i++) {
                gameState.players.push({
                    ...playerColors[i],
                    position: 0
                });
            }
            
            gameState.currentPlayer = 0;
            gameState.gameOver = false;
            gameState.waitingForAction = false;
        }

        // 创建玩家棋子
        function createPlayerPieces() {
            // 清除现有的玩家棋子
            const existingPlayers = document.querySelectorAll('.player');
            existingPlayers.forEach(player => player.remove());
            
            // 创建新的玩家棋子
            gameState.players.forEach(player => {
                const playerElement = document.createElement('div');
                playerElement.className = `player player-${player.id}`;
                playerElement.textContent = player.id;
                playerElement.id = `player-${player.id}`;
                pathInner.appendChild(playerElement);
            });
        }

        // 渲染玩家信息
        function renderPlayersInfo() {
            playersInfo.innerHTML = '';
            
            gameState.players.forEach((player, index) => {
                const playerElement = document.createElement('div');
                playerElement.className = `player-info ${index === gameState.currentPlayer ? 'active' : ''}`;
                
                playerElement.innerHTML = `
                    <div class="player-color" style="background: ${player.color}"></div>
                    <div class="player-name">${player.name}</div>
                    <div class="player-position">位置: ${player.position + 1}/${gameState.totalPositions}</div>
                `;
                
                playersInfo.appendChild(playerElement);
            });
        }

        // 掷骰子
        function rollDice() {
            if (gameState.gameOver || gameState.waitingForAction) return;
            
            // 禁用按钮
            rollButton.disabled = true;
            
            // 骰子滚动动画
            diceElement.classList.add('rolling');
            
            // 模拟骰子滚动
            let rolls = 0;
            const rollInterval = setInterval(() => {
                const randomWeather = weatherTypes[Math.floor(Math.random() * weatherTypes.length)];
                diceElement.textContent = randomWeather.emoji;
                rolls++;
                
                if (rolls > 10) {
                    clearInterval(rollInterval);
                    diceElement.classList.remove('rolling');
                    
                    // 确定最终结果
                    const finalWeather = weatherTypes[Math.floor(Math.random() * weatherTypes.length)];
                    diceElement.textContent = finalWeather.emoji;
                    
                    // 播放对应音效
                    playSound(finalWeather.sound);
                    
                    // 更新动作区域
                    updateActionArea(finalWeather);
                    
                    // 显示动作模态框
                    showActionModal(finalWeather);
                    
                    // 语音播报
                    speak(`${gameState.players[gameState.currentPlayer].name}掷到了${finalWeather.speech}`);
                }
            }, 100);
        }

        // 更新动作区域
        function updateActionArea(weather) {
            actionArea.innerHTML = `
                <div class="action-icon">${weather.emoji}</div>
                <div class="action-text">${weather.name}</div>
                <div class="action-description">前进${weather.steps}步</div>
            `;
        }

        // 显示动作模态框
        function showActionModal(weather) {
            actionModalIcon.textContent = weather.emoji;
            actionModalText.textContent = weather.name;
            actionModalSteps.textContent = `前进${weather.steps}步`;
            actionModalDescription.textContent = weather.description;
            
            actionModal.classList.add('show');
            gameState.waitingForAction = true;
        }

        // 完成动作
        function completeAction() {
            actionModal.classList.remove('show');
            gameState.waitingForAction = false;
            
            // 播放鼓励语音
            speak("真棒！做得很好！");
            
            // 移动玩家
            const currentWeather = getCurrentWeather();
            movePlayer(currentWeather.steps);
        }

        // 获取当前天气
        function getCurrentWeather() {
            const diceEmoji = diceElement.textContent;
            return weatherTypes.find(weather => weather.emoji === diceEmoji) || weatherTypes[0];
        }

        // 移动玩家
        function movePlayer(steps) {
            const currentPlayer = gameState.players[gameState.currentPlayer];
            const startPosition = currentPlayer.position;
            
            // 播放步数音效
            playStepSound(steps);
            
            // 显示移动动画
            showMoveAnimation(steps);
            
            // 逐步移动玩家
            let step = 0;
            const moveInterval = setInterval(() => {
                currentPlayer.position = Math.min(startPosition + step, gameState.totalPositions - 1);
                updatePlayerPositions();
                step++;
                
                if (step > steps || currentPlayer.position >= gameState.totalPositions - 1) {
                    clearInterval(moveInterval);
                    
                    // 检查是否获胜
                    if (currentPlayer.position >= gameState.totalPositions - 1) {
                        currentPlayer.position = gameState.totalPositions - 1;
                        gameState.gameOver = true;
                        setTimeout(() => showWinMessage(currentPlayer), 1000);
                    }
                    
                    // 切换到下一个玩家
                    if (!gameState.gameOver) {
                        setTimeout(() => {
                            gameState.currentPlayer = (gameState.currentPlayer + 1) % gameState.players.length;
                            renderPlayersInfo();
                            rollButton.disabled = false;
                            
                            // 提示下一个玩家
                            speak(`轮到${gameState.players[gameState.currentPlayer].name}掷骰子了`);
                        }, 1500);
                    }
                }
            }, 500);
        }

        // 显示移动动画
        function showMoveAnimation(steps) {
            const moveText = document.createElement('div');
            moveText.className = 'move-animation';
            moveText.textContent = `前进${steps}步`;
            moveText.style.left = '50%';
            moveText.style.top = '50%';
            moveText.style.transform = 'translate(-50%, -50%)';
            
            gameBoard.appendChild(moveText);
            
            setTimeout(() => {
                gameBoard.removeChild(moveText);
            }, 1500);
        }

        // 更新玩家在棋盘上的位置
        function updatePlayerPositions() {
            gameState.players.forEach(player => {
                const playerElement = document.getElementById(`player-${player.id}`);
                const angle = (player.position / gameState.totalPositions) * 2 * Math.PI;
                const radius = 35; // 百分比
                
                const x = 50 + radius * Math.cos(angle - Math.PI / 2);
                const y = 50 + radius * Math.sin(angle - Math.PI / 2);
                
                playerElement.style.left = `${x}%`;
                playerElement.style.top = `${y}%`;
            });
        }

        // 显示获胜消息
        function showWinMessage(player) {
            winnerText.textContent = `${player.name}获胜！`;
            winMessage.classList.add('show');
            
            // 随机选择获胜祝贺语
            const congratsMessages = [
                `恭喜${player.name}获胜！太厉害了！`,
                `${player.name}太棒了！赢得了游戏！`,
                `哇！${player.name}是今天的冠军！`,
                `恭喜${player.name}！你表现得非常出色！`,
                `${player.name}赢得了胜利！为你鼓掌！`
            ];
            
            const randomMessage = congratsMessages[Math.floor(Math.random() * congratsMessages.length)];
            speak(randomMessage);
        }

        // 重置游戏
        function resetGame() {
            winMessage.classList.remove('show');
            actionModal.classList.remove('show');
            
            // 返回设置界面
            setupScreen.style.display = 'block';
            gameArea.style.display = 'none';
            
            // 重置骰子
            diceElement.textContent = "🎲";
            
            // 启用按钮
            rollButton.disabled = false;
        }

        // 初始化游戏
        window.onload = initGame;
    </script>
</body>
</html># -
