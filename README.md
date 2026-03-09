# division
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>食物分配數學遊戲</title>
    <style>
        :root {
            --primary: #ff7675;
            --secondary: #74b9ff;
            --accent: #55efc4;
            --light: #fffdf9;
            --dark: #2d3436;
            --gray: #636e72;
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: 'Segoe UI', 'Microsoft JhengHei', sans-serif;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            color: var(--dark);
        }
        
        .container {
            width: 100%;
            max-width: 1200px;
            background: white;
            border-radius: 24px;
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.12);
            padding: 30px;
            margin: 20px 0;
        }
        
        header {
            text-align: center;
            margin-bottom: 30px;
        }
        
        h1 {
            color: var(--primary);
            font-size: 2.5rem;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
        }
        
        .subtitle {
            color: var(--gray);
            font-size: 1.1rem;
            max-width: 700px;
            margin: 0 auto;
            line-height: 1.6;
        }
        
        .game-section {
            display: flex;
            flex-direction: column;
            gap: 30px;
        }
        
        .problem-section {
            background: #f8f9fa;
            border-radius: 20px;
            padding: 25px;
            text-align: center;
        }
        
        .problem-text {
            font-size: 1.8rem;
            font-weight: bold;
            color: var(--dark);
            margin-bottom: 20px;
        }
        
        .problem-equation {
            font-size: 2.2rem;
            color: var(--primary);
            background: white;
            padding: 15px 30px;
            border-radius: 15px;
            display: inline-block;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
            margin-bottom: 15px;
        }
        
        .problem-hint {
            color: var(--gray);
            font-size: 1rem;
        }
        
        .controls-section {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 25px;
        }
        
        @media (max-width: 768px) {
            .controls-section {
                grid-template-columns: 1fr;
            }
        }
        
        .control-panel {
            background: white;
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.08);
            border: 2px solid #f1f2f6;
        }
        
        .control-title {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            font-size: 1.3rem;
            margin-bottom: 20px;
            color: var(--dark);
        }
        
        .slider-container {
            margin: 20px 0;
        }
        
        .slider-label {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            font-size: 1.1rem;
        }
        
        .slider-value {
            background: var(--primary);
            color: white;
            padding: 5px 15px;
            border-radius: 20px;
            font-weight: bold;
            min-width: 50px;
            text-align: center;
        }
        
        input[type=range] {
            width: 100%;
            height: 12px;
            -webkit-appearance: none;
            appearance: none;
            background: linear-gradient(to right, #dfe6e9, var(--primary));
            border-radius: 10px;
            outline: none;
        }
        
        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 28px;
            height: 28px;
            border-radius: 50%;
            background: white;
            border: 3px solid var(--primary);
            cursor: pointer;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
            transition: all 0.2s;
        }
        
        input[type=range]::-webkit-slider-thumb:hover {
            transform: scale(1.1);
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.25);
        }
        
        #game-area {
            width: 100%;
            height: 600px;
            background: #f8f9fa;
            border-radius: 20px;
            overflow: hidden;
            position: relative;
            margin: 20px 0;
            border: 3px dashed #e9ecef;
            display: flex;
            flex-direction: column;
        }
        
        .food-area {
            flex: 1.2;
            padding: 20px;
            display: flex;
            flex-direction: column;
            border-bottom: 3px dashed #dfe6e9;
        }
        
        .food-area-title {
            text-align: center;
            font-weight: bold;
            color: var(--dark);
            margin-bottom: 15px;
            font-size: 1.2rem;
        }
        
        .food-grid {
            flex: 1;
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            grid-template-rows: repeat(4, 1fr);
            gap: 15px;
            padding: 15px;
            justify-items: center;
            align-items: center;
        }
        
        .plate-area {
            flex: 1.8;
            padding: 20px;
            display: flex;
            flex-direction: column;
            overflow-y: auto;
        }
        
        .plate-area-title {
            text-align: center;
            font-weight: bold;
            color: var(--dark);
            margin-bottom: 15px;
            font-size: 1.2rem;
        }
        
        .plate-grid {
            flex: 1;
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            grid-template-rows: repeat(4, 1fr);
            gap: 20px;
            padding: 15px;
            justify-items: center;
            align-items: start;
            min-height: 400px;
        }
        
        .food-item {
            font-size: 2.2rem;
            cursor: grab;
            transition: transform 0.2s, opacity 0.2s;
            user-select: none;
            width: 70px;
            height: 70px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .food-item.dragging {
            cursor: grabbing;
            z-index: 100;
            transform: scale(1.3);
        }
        
        .food-item.in-plate {
            opacity: 1;
            transform: none;
            font-size: 1.8rem;
            width: 45px;
            height: 45px;
        }
        
        .plate {
            background: white;
            border-radius: 15px;
            padding: 15px;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
            border: 3px solid var(--secondary);
            width: 130px;
            height: 130px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
        }
        
        .plate-icon {
            font-size: 2.2rem;
            margin-bottom: 8px;
        }
        
        .plate-label {
            font-weight: bold;
            color: var(--dark);
            margin-bottom: 5px;
            font-size: 0.9rem;
        }
        
        .plate-count {
            color: var(--primary);
            font-size: 1.1rem;
            font-weight: bold;
        }
        
        .plate-food-items {
            display: flex;
            flex-wrap: wrap;
            gap: 3px;
            justify-content: center;
            margin-top: 5px;
            min-height: 30px;
            max-width: 100%;
        }
        
        .answer-section {
            background: white;
            border-radius: 20px;
            padding: 25px;
            text-align: center;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.08);
            margin-top: 20px;
        }
        
        .answer-form {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
        }
        
        .answer-label {
            font-size: 1.3rem;
            font-weight: bold;
            color: var(--dark);
        }
        
        .answer-input-container {
            display: flex;
            align-items: center;
            gap: 15px;
            flex-wrap: wrap;
            justify-content: center;
        }
        
        .answer-input {
            font-size: 2rem;
            width: 120px;
            text-align: center;
            padding: 10px;
            border: 3px solid var(--secondary);
            border-radius: 12px;
            outline: none;
            transition: all 0.3s;
            font-weight: bold;
            color: var(--dark);
        }
        
        .answer-input:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(255, 118, 117, 0.2);
        }
        
        .answer-unit {
            font-size: 1.5rem;
            color: var(--gray);
        }
        
        .buttons-section {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 30px;
            flex-wrap: wrap;
        }
        
        .btn {
            padding: 15px 30px;
            border: none;
            border-radius: 12px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 10px;
            min-width: 180px;
            justify-content: center;
        }
        
        .btn-primary {
            background: var(--primary);
            color: white;
        }
        
        .btn-primary:hover {
            background: #ff5252;
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(255, 118, 117, 0.3);
        }
        
        .btn-secondary {
            background: var(--secondary);
            color: white;
        }
        
        .btn-secondary:hover {
            background: #0984e3;
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(116, 185, 255, 0.3);
        }
        
        .btn-success {
            background: var(--accent);
            color: var(--dark);
        }
        
        .btn-success:hover {
            background: #00b894;
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(85, 239, 196, 0.3);
        }
        
        .feedback {
            text-align: center;
            margin-top: 20px;
            padding: 15px;
            border-radius: 12px;
            font-weight: bold;
            font-size: 1.2rem;
            opacity: 0;
            transition: opacity 0.3s;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .feedback.correct {
            background: rgba(85, 239, 196, 0.2);
            color: #00b894;
            opacity: 1;
        }
        
        .feedback.incorrect {
            background: rgba(255, 118, 117, 0.2);
            color: #ff5252;
            opacity: 1;
        }
        
        .instructions {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 20px;
            margin-top: 30px;
        }
        
        .instructions h3 {
            color: var(--primary);
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .instructions ul {
            padding-left: 20px;
            color: var(--gray);
            line-height: 1.8;
        }
        
        .instructions li {
            margin-bottom: 8px;
        }
        
        .stats-section {
            display: flex;
            justify-content: space-around;
            flex-wrap: wrap;
            gap: 20px;
            margin-top: 20px;
        }
        
        .stat-box {
            background: white;
            border-radius: 15px;
            padding: 20px;
            text-align: center;
            min-width: 150px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
        }
        
        .stat-value {
            font-size: 2.5rem;
            font-weight: bold;
            color: var(--primary);
        }
        
        .stat-label {
            color: var(--gray);
            margin-top: 5px;
        }
        
        .current-problem-info {
            background: #f0f7ff;
            border-radius: 15px;
            padding: 15px;
            margin-bottom: 20px;
            text-align: center;
            border-left: 5px solid var(--secondary);
        }
        
        .current-problem-info h3 {
            color: var(--secondary);
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        
        .problem-details {
            display: flex;
            justify-content: center;
            gap: 30px;
            flex-wrap: wrap;
            margin-top: 10px;
        }
        
        .problem-detail {
            background: white;
            padding: 10px 20px;
            border-radius: 10px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.08);
        }
        
        .problem-detail-value {
            font-size: 1.8rem;
            font-weight: bold;
            color: var(--primary);
        }
        
        .problem-detail-label {
            font-size: 0.9rem;
            color: var(--gray);
            margin-top: 5px;
        }
        
        @media (max-width: 768px) {
            .container {
                padding: 20px;
            }
            
            h1 {
                font-size: 2rem;
            }
            
            .problem-equation {
                font-size: 1.8rem;
                padding: 12px 20px;
            }
            
            .btn {
                min-width: 100%;
            }
            
            #game-area {
                height: 700px;
            }
            
            .food-grid {
                grid-template-columns: repeat(4, 1fr);
                grid-template-rows: repeat(5, 1fr);
            }
            
            .plate-grid {
                grid-template-columns: repeat(3, 1fr);
                grid-template-rows: repeat(4, 1fr);
            }
            
            .plate {
                width: 110px;
                height: 110px;
                padding: 10px;
            }
            
            .food-item {
                width: 60px;
                height: 60px;
                font-size: 1.8rem;
            }
        }
        
        .empty-cell {
            width: 70px;
            height: 70px;
            opacity: 0.3;
            border: 2px dashed #dfe6e9;
            border-radius: 10px;
        }
        
        .empty-plate-cell {
            width: 130px;
            height: 130px;
            opacity: 0;
        }
        
        .scroll-hint {
            text-align: center;
            color: var(--gray);
            font-size: 0.9rem;
            margin-top: 10px;
            font-style: italic;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1><span role="img" aria-label="食物">🍽️</span> 食物分配數學遊戲</h1>
            <p class="subtitle">隨機生成數學問題，拖動食物到盤子上，食物會從食物區消失。計算每個盤子應該有多少食物並輸入答案！</p>
        </header>
        
        <div class="game-section">
            <!-- 當前問題資訊 -->
            <div class="current-problem-info">
                <h3><span role="img" aria-hidden="true">🎯</span> 當前問題資訊</h3>
                <div class="problem-details">
                    <div class="problem-detail">
                        <div class="problem-detail-value" id="problem-dividend">12</div>
                        <div class="problem-detail-label">食物總數</div>
                    </div>
                    <div class="problem-detail">
                        <div class="problem-detail-value" id="problem-divisor">4</div>
                        <div class="problem-detail-label">盤子數量</div>
                    </div>
                </div>
            </div>
            
            <!-- 問題顯示區域 -->
            <div class="problem-section">
                <div class="problem-text">請解決以下問題：</div>
                <div class="problem-equation" id="problem-equation"></div>
                <div class="problem-hint" id="problem-hint"></div>
            </div>
            
            <!-- 控制區域 -->
            <div class="controls-section">
                <div class="control-panel">
                    <div class="control-title">
                        <span role="img" aria-label="食物">🍕</span>
                        <span>練習食物數量</span>
                    </div>
                    <div class="slider-container">
                        <div class="slider-label">
                            <span>食物總數：</span>
                            <span class="slider-value" id="food-count-value">12</span>
                        </div>
                        <input type="range" id="food-slider" min="2" max="20" value="12" step="1">
                    </div>
                    <p class="problem-hint">自由調整練習用的食物數量 (最多20個)</p>
                </div>
                
                <div class="control-panel">
                    <div class="control-title">
                        <span role="img" aria-label="盤子">🍽️</span>
                        <span>練習盤子數量</span>
                    </div>
                    <div class="slider-container">
                        <div class="slider-label">
                            <span>盤子數量：</span>
                            <span class="slider-value" id="plate-count-value">4</span>
                        </div>
                        <input type="range" id="plate-slider" min="2" max="10" value="4" step="1">
                    </div>
                    <p class="problem-hint">自由調整練習用的盤子數量 (2-10個)</p>
                </div>
            </div>
            
            <!-- 統計區域 -->
            <div class="stats-section">
                <div class="stat-box">
                    <div class="stat-value" id="total-food">12</div>
                    <div class="stat-label">練習食物總數</div>
                </div>
                <div class="stat-box">
                    <div class="stat-value" id="total-plates">4</div>
                    <div class="stat-label">練習盤子數量</div>
                </div>
                <div class="stat-box">
                    <div class="stat-value" id="remaining-food">12</div>
                    <div class="stat-label">剩餘食物</div>
                </div>
                <div class="stat-box">
                    <div class="stat-value" id="food-on-plates">0</div>
                    <div class="stat-label">已分配食物</div>
                </div>
            </div>
            
            <!-- 遊戲區域 -->
            <div id="game-area">
                <!-- 食物區域 -->
                <div class="food-area">
                    <div class="food-area-title">食物區 (拖動食物到盤子上)</div>
                    <div class="food-grid" id="food-grid">
                        <!-- 食物將在這裡動態生成 -->
                    </div>
                </div>
                
                <!-- 盤子區域 -->
                <div class="plate-area">
                    <div class="plate-area-title">盤子區 (將食物拖到這裡)</div>
                    <div class="plate-grid" id="plate-grid">
                        <!-- 盤子將在這裡動態生成 -->
                    </div>
                    <div class="scroll-hint">如果盤子太多看不見，可以向下滾動</div>
                </div>
            </div>
            
            <!-- 答案輸入區域 -->
            <div class="answer-section">
                <div class="answer-form">
                    <div class="answer-label">你的答案：</div>
                    <div class="answer-input-container">
                        <span>每個盤子有</span>
                        <input type="number" class="answer-input" id="answer-input" min="1" max="20" step="1" placeholder="?">
                        <span class="answer-unit">個食物</span>
                    </div>
                </div>
            </div>
            
            <!-- 反饋區域 -->
            <div class="feedback" id="feedback"></div>
            
            <!-- 按鈕區域 -->
            <div class="buttons-section">
                <button class="btn btn-primary" id="check-btn">
                    <span role="img" aria-hidden="true">✅</span> 檢查答案
                </button>
                <button class="btn btn-secondary" id="reset-btn">
                    <span role="img" aria-hidden="true">🔄</span> 重新開始
                </button>
                <button class="btn btn-success" id="new-problem-btn">
                    <span role="img" aria-hidden="true">🎯</span> 新問題
                </button>
            </div>
            
            <!-- 說明區域 -->
            <div class="instructions">
                <h3><span role="img" aria-hidden="true">📚</span> 遊戲說明</h3>
                <ul>
                    <li><strong>步驟1：</strong> 隨機生成一個數學問題（上方藍色區域）</li>
                    <li><strong>步驟2：</strong> 自由調整食物和盤子的數量進行練習（滑竿區域）</li>
                    <li><strong>步驟3：</strong> 從食物區拖動食物到盤子上（食物會從食物區消失）</li>
                    <li><strong>步驟4：</strong> 根據隨機問題計算每個盤子應該有多少食物</li>
                    <li><strong>步驟5：</strong> 在輸入框中輸入你的答案</li>
                    <li><strong>步驟6：</strong> 點擊「檢查答案」查看是否正確</li>
                    <li><strong>注意：</strong> 如果答案錯誤，請重新思考並再試一次！</li>
                    <li><strong>重要：</strong> 食物拖到盤子後會從食物區消失，代表已被分配！</li>
                </ul>
            </div>
        </div>
    </div>

    <script>
        // 遊戲狀態變數
        let currentProblem = null;
        let totalFood = 12;  // 當前練習用的食物數量（由滑竿控制）
        let totalPlates = 4; // 當前練習用的盤子數量（由滑竿控制）
        let foodItems = [];
        let plates = [];
        let foodOnPlates = {}; // 記錄每個盤子上的食物ID
        let allFoodItems = {}; // 記錄所有食物物件
        
        // DOM 元素
        const gameArea = document.getElementById('game-area');
        const foodSlider = document.getElementById('food-slider');
        const plateSlider = document.getElementById('plate-slider');
        const foodCountValue = document.getElementById('food-count-value');
        const plateCountValue = document.getElementById('plate-count-value');
        const problemEquation = document.getElementById('problem-equation');
        const problemHint = document.getElementById('problem-hint');
        const totalFoodEl = document.getElementById('total-food');
        const totalPlatesEl = document.getElementById('total-plates');
        const remainingFoodEl = document.getElementById('remaining-food');
        const foodOnPlatesEl = document.getElementById('food-on-plates');
        const feedbackEl = document.getElementById('feedback');
        const checkBtn = document.getElementById('check-btn');
        const resetBtn = document.getElementById('reset-btn');
        const newProblemBtn = document.getElementById('new-problem-btn');
        const answerInput = document.getElementById('answer-input');
        const foodGrid = document.getElementById('food-grid');
        const plateGrid = document.getElementById('plate-grid');
        const problemDividendEl = document.getElementById('problem-dividend');
        const problemDivisorEl = document.getElementById('problem-divisor');
        
        // 食物表情符號
        const foodEmojis = ['🍕', '🍔', '🌭', '🥪', '🍟', '🍗', '🥨', '🍦', '🍩', '🍪', '🍎', '🍌', '🍇', '🍓', '🥑', '🥦', '🥕', '🌽', '🧀', '🥚'];
        
        // 盤子表情符號
        const plateEmojis = ['🍽️', '🥘', '🍛', '🍜', '🍲', '🥣', '🍴', '🥄', '🔸', '🔹'];
        
        // 初始化遊戲
        function initGame() {
            // 生成第一個隨機問題
            generateRandomProblem();
            
            // 添加事件監聽器
            foodSlider.addEventListener('input', handleFoodSliderChange);
            plateSlider.addEventListener('input', handlePlateSliderChange);
            checkBtn.addEventListener('click', checkAnswer);
            resetBtn.addEventListener('click', resetGame);
            newProblemBtn.addEventListener('click', generateRandomProblem);
            answerInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    checkAnswer();
                }
            });
            
            // 初始化遊戲元素
            createGameElements();
            updateStats();
        }
        
        // 生成隨機問題
        function generateRandomProblem() {
            let divisor, dividend, multiplier, maxMultiplier;
            let attempts = 0;
            const maxAttempts = 100;
            
            do {
                // 隨機生成除數 (2-9，避免除數為1)
                divisor = Math.floor(Math.random() * 8) + 2; // 2-9
                
                // 生成能被除數整除的被除數 (1-20以內)
                maxMultiplier = Math.floor(20 / divisor);
                
                // 確保有至少一個倍數可用
                if (maxMultiplier < 1) {
                    continue;
                }
                
                multiplier = Math.floor(Math.random() * maxMultiplier) + 1;
                dividend = divisor * multiplier;
                
                attempts++;
                
                // 避免被除數和除數相同，且確保被除數大於除數
            } while ((dividend === divisor || dividend <= divisor) && attempts < maxAttempts);
            
            // 如果經過多次嘗試仍然不符合條件，使用默認值
            if (attempts >= maxAttempts || dividend === divisor) {
                // 使用一些預設的好問題
                const defaultProblems = [
                    {dividend: 12, divisor: 4, answer: 3},
                    {dividend: 15, divisor: 5, answer: 3},
                    {dividend: 18, divisor: 6, answer: 3},
                    {dividend: 20, divisor: 5, answer: 4},
                    {dividend: 16, divisor: 8, answer: 2},
                    {dividend: 14, divisor: 7, answer: 2},
                    {dividend: 9, divisor: 3, answer: 3},
                    {dividend: 10, divisor: 2, answer: 5},
                    {dividend: 8, divisor: 4, answer: 2},
                    {dividend: 6, divisor: 2, answer: 3}
                ];
                
                const randomIndex = Math.floor(Math.random() * defaultProblems.length);
                currentProblem = defaultProblems[randomIndex];
            } else {
                // 計算答案
                const answer = dividend / divisor;
                
                // 更新問題
                currentProblem = { dividend, divisor, answer };
            }
            
            // 更新問題顯示
            updateProblemDisplay();
            
            // 重置答案輸入
            answerInput.value = '';
            
            // 重置遊戲
            resetDistribution();
            
            clearFeedback();
        }
        
        // 更新問題顯示
        function updateProblemDisplay() {
            problemEquation.textContent = `${currentProblem.dividend} ÷ ${currentProblem.divisor} = ?`;
            problemHint.textContent = `將 ${currentProblem.dividend} 個食物平均分配到 ${currentProblem.divisor} 個盤子中，每個盤子應該有幾個食物？`;
            
            // 更新問題資訊區域
            problemDividendEl.textContent = currentProblem.dividend;
            problemDivisorEl.textContent = currentProblem.divisor;
        }
        
        // 處理食物滑竿變化
        function handleFoodSliderChange() {
            totalFood = parseInt(foodSlider.value);
            foodCountValue.textContent = totalFood;
            
            // 重新創建遊戲元素
            createGameElements();
            updateStats();
            clearFeedback();
        }
        
        // 處理盤子滑竿變化
        function handlePlateSliderChange() {
            totalPlates = parseInt(plateSlider.value);
            plateCountValue.textContent = totalPlates;
            
            // 初始化盤子食物計數
            foodOnPlates = {};
            for (let i = 0; i < totalPlates; i++) {
                foodOnPlates[i] = [];
            }
            
            // 重新創建遊戲元素
            createGameElements();
            updateStats();
            clearFeedback();
        }
        
        // 創建遊戲元素（食物和盤子）
        function createGameElements() {
            // 清除容器
            foodGrid.innerHTML = '';
            plateGrid.innerHTML = '';
            foodItems = [];
            plates = [];
            allFoodItems = {};
            
            // 創建食物網格 (5x4 = 20個位置)
            for (let i = 0; i < 20; i++) {
                const cell = document.createElement('div');
                
                if (i < totalFood) {
                    // 創建食物
                    const food = document.createElement('div');
                    food.className = 'food-item';
                    food.innerHTML = foodEmojis[i % foodEmojis.length];
                    food.dataset.foodId = i;
                    food.dataset.inPlate = 'false';
                    
                    // 添加拖動功能
                    makeDraggable(food);
                    
                    cell.appendChild(food);
                    
                    const foodObj = {
                        element: food,
                        id: i,
                        plateId: null,
                        inPlate: false
                    };
                    
                    foodItems.push(foodObj);
                    allFoodItems[i] = foodObj;
                } else {
                    // 創建空位置
                    cell.className = 'empty-cell';
                }
                
                foodGrid.appendChild(cell);
            }
            
            // 創建盤子網格 (5x4 = 20個位置，足夠顯示10個盤子)
            for (let i = 0; i < 20; i++) {
                const cell = document.createElement('div');
                
                if (i < totalPlates) {
                    // 創建盤子
                    const plate = document.createElement('div');
                    plate.className = 'plate';
                    plate.dataset.plateId = i;
                    
                    plate.innerHTML = `
                        <div class="plate-icon">${plateEmojis[i % plateEmojis.length]}</div>
                        <div class="plate-label">盤子 ${i + 1}</div>
                        <div class="plate-count" id="plate-count-${i}">0</div>
                        <div class="plate-food-items" id="plate-food-items-${i}"></div>
                    `;
                    
                    // 設置盤子為可放置區域
                    plate.addEventListener('dragover', (e) => {
                        e.preventDefault();
                        plate.style.borderColor = '#55efc4';
                        plate.style.boxShadow = '0 0 15px rgba(85, 239, 196, 0.5)';
                    });
                    
                    plate.addEventListener('dragleave', () => {
                        plate.style.borderColor = 'var(--secondary)';
                        plate.style.boxShadow = '0 8px 20px rgba(0, 0, 0, 0.1)';
                    });
                    
                    plate.addEventListener('drop', (e) => {
                        e.preventDefault();
                        plate.style.borderColor = 'var(--secondary)';
                        plate.style.boxShadow = '0 8px 20px rgba(0, 0, 0, 0.1)';
                        
                        const foodId = e.dataTransfer.getData('text/plain');
                        if (foodId) {
                            placeFoodOnPlate(parseInt(foodId), i);
                        }
                    });
                    
                    cell.appendChild(plate);
                    
                    plates.push({
                        element: plate,
                        id: i
                    });
                    
                    // 初始化盤子食物計數
                    if (!foodOnPlates[i]) {
                        foodOnPlates[i] = [];
                    }
                } else {
                    // 創建空位置
                    cell.className = 'empty-plate-cell';
                }
                
                plateGrid.appendChild(cell);
            }
        }
        
        // 使元素可拖動
        function makeDraggable(element) {
            element.setAttribute('draggable', 'true');
            
            element.addEventListener('dragstart', (e) => {
                // 只允許在食物區的食物被拖動
                if (element.dataset.inPlate === 'true') {
                    e.preventDefault();
                    return;
                }
                
                element.classList.add('dragging');
                e.dataTransfer.setData('text/plain', element.dataset.foodId);
                e.dataTransfer.effectAllowed = 'move';
            });
            
            element.addEventListener('dragend', () => {
                element.classList.remove('dragging');
            });
        }
        
        // 將食物放在盤子上
        function placeFoodOnPlate(foodId, plateId) {
            const foodItem = allFoodItems[foodId];
            if (!foodItem || foodItem.inPlate) return;
            
            const foodElement = foodItem.element;
            const plateFoodItemsContainer = document.getElementById(`plate-food-items-${plateId}`);
            
            // 將食物添加到盤子
            foodItem.plateId = plateId;
            foodItem.inPlate = true;
            foodOnPlates[plateId].push(foodId);
            
            // 更新食物元素屬性
            foodElement.dataset.inPlate = 'true';
            foodElement.dataset.plateId = plateId;
            
            // 從食物容器中移除食物（視覺上消失）
            foodElement.style.display = 'none';
            
            // 創建盤子上的食物顯示
            const plateFoodElement = document.createElement('div');
            plateFoodElement.className = 'food-item in-plate';
            plateFoodElement.innerHTML = foodElement.innerHTML;
            plateFoodElement.dataset.foodId = foodId;
            plateFoodElement.dataset.inPlate = 'true';
            
            // 添加返回功能（雙擊返回食物區）
            plateFoodElement.addEventListener('dblclick', () => {
                returnFoodToContainer(foodId);
            });
            
            plateFoodItemsContainer.appendChild(plateFoodElement);
            
            // 更新盤子計數和統計
            updatePlateCount(plateId);
            updateStats();
        }
        
        // 將食物返回食物容器
        function returnFoodToContainer(foodId) {
            const foodItem = allFoodItems[foodId];
            if (!foodItem || !foodItem.inPlate) return;
            
            const plateId = foodItem.plateId;
            const plateFoodItemsContainer = document.getElementById(`plate-food-items-${plateId}`);
            
            // 從盤子中移除
            const index = foodOnPlates[plateId].indexOf(foodId);
            if (index > -1) {
                foodOnPlates[plateId].splice(index, 1);
            }
            
            // 移除盤子上的食物顯示
            const plateFoodElement = plateFoodItemsContainer.querySelector(`[data-food-id="${foodId}"]`);
            if (plateFoodElement) {
                plateFoodElement.remove();
            }
            
            // 更新食物狀態
            foodItem.plateId = null;
            foodItem.inPlate = false;
            foodItem.element.dataset.inPlate = 'false';
            delete foodItem.element.dataset.plateId;
            
            // 重新顯示食物在食物容器中
            foodItem.element.style.display = 'block';
            
            // 更新盤子計數和統計
            updatePlateCount(plateId);
            updateStats();
        }
        
        // 更新盤子計數顯示
        function updatePlateCount(plateId) {
            const count = foodOnPlates[plateId] ? foodOnPlates[plateId].length : 0;
            const plateCountEl = document.getElementById(`plate-count-${plateId}`);
            if (plateCountEl) {
                plateCountEl.textContent = count;
            }
        }
        
        // 更新統計資訊
        function updateStats() {
            totalFoodEl.textContent = totalFood;
            totalPlatesEl.textContent = totalPlates;
            
            // 計算已分配的食物總數
            let totalFoodOnPlates = 0;
            for (let i = 0; i < totalPlates; i++) {
                if (foodOnPlates[i]) {
                    totalFoodOnPlates += foodOnPlates[i].length;
                }
            }
            
            foodOnPlatesEl.textContent = totalFoodOnPlates;
            remainingFoodEl.textContent = totalFood - totalFoodOnPlates;
        }
        
        // 檢查答案
        function checkAnswer() {
            const userAnswer = parseInt(answerInput.value);
            
            if (isNaN(userAnswer)) {
                showFeedback('請輸入一個數字答案！', 'incorrect');
                return;
            }
            
            if (userAnswer === currentProblem.answer) {
                showFeedback('太棒了！答案正確！', 'correct');
            } else {
                showFeedback('答案不對，再試試看！', 'incorrect');
                // 不再提示正確答案
            }
        }
        
        // 重置分配（只重置食物分配，不改變問題和滑竿）
        function resetDistribution() {
            // 將所有食物返回食物容器
            for (const foodId in allFoodItems) {
                const foodItem = allFoodItems[foodId];
                if (foodItem.inPlate) {
                    returnFoodToContainer(parseInt(foodId));
                }
            }
            
            // 重置盤子食物計數
            foodOnPlates = {};
            for (let i = 0; i < totalPlates; i++) {
                foodOnPlates[i] = [];
                updatePlateCount(i);
            }
            
            // 重置答案輸入
            answerInput.value = '';
            
            updateStats();
            clearFeedback();
        }
        
        // 重置整個遊戲（包括滑竿）
        function resetGame() {
            // 重置滑竿到問題的值
            totalFood = currentProblem.dividend;
            totalPlates = currentProblem.divisor;
            
            foodSlider.value = totalFood;
            plateSlider.value = totalPlates;
            foodCountValue.textContent = totalFood;
            plateCountValue.textContent = totalPlates;
            
            // 重新創建遊戲元素
            createGameElements();
            updateStats();
            clearFeedback();
            
            // 視覺反饋
            resetBtn.style.transform = 'scale(0.95)';
            setTimeout(() => {
                resetBtn.style.transform = 'scale(1)';
            }, 150);
        }
        
        // 顯示反饋訊息
        function showFeedback(message, type) {
            feedbackEl.textContent = message;
            feedbackEl.className = `feedback ${type}`;
            
            // 5秒後清除反饋
            setTimeout(clearFeedback, 5000);
        }
        
        // 清除反饋訊息
        function clearFeedback() {
            feedbackEl.textContent = '';
            feedbackEl.className = 'feedback';
        }
        
        // 初始化遊戲
        window.addEventListener('load', initGame);
    </script>
</body>
</html>
