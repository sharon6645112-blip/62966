<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>❤️ 超級大考驗：妳能順利通關嗎？ ❤️</title>
    <style>
        /* --- 視覺效果 & 遊戲風設定 --- */
        @import url('https://fonts.googleapis.com/css2?family=ZCOOL+KuaiLe&display=swap');

        * {
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: 'ZCOOL KuaiLe', 'Microsoft JhengHei', cursive;
            background-color: #ffe0e9; /* 甜美粉色背景 */
            color: #4a4a4a;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            overflow: hidden;
            transition: background-color 0.5s, transform 0.1s;
        }

        #game-container {
            background-color: rgba(255, 255, 255, 0.95);
            padding: 25px;
            border-radius: 25px;
            box-shadow: 0 10px 30px rgba(231, 76, 60, 0.2);
            width: 92%;
            max-width: 450px;
            text-align: center;
            position: relative;
            border: 4px solid #ffb6c1;
            transition: all 0.3s ease;
        }

        /* --- HP 血條 (心型) --- */
        #hp-container {
            font-size: 26px;
            color: #e74c3c;
            margin-bottom: 15px;
            min-height: 35px;
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 5px;
        }
        .hp-text {
            font-size: 20px;
            margin-left: 8px;
            font-weight: bold;
        }

        #question-text {
            font-size: 22px;
            margin-bottom: 20px;
            font-weight: bold;
            line-height: 1.4;
        }

        /* 圖片容器 */
        #image-wrapper {
            width: 100%;
            height: 200px;
            margin-bottom: 15px;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            border-radius: 15px;
            background-color: #f9f9f9;
            border: 2px dashed #ffb6c1;
        }

        #q-image {
            max-width: 100%;
            max-height: 100%;
            object-fit: cover;
        }

        /* 選擇題按鈕 */
        #options-container {
            display: grid;
            grid-template-columns: 1fr;
            gap: 12px;
        }

        .option-btn {
            background-color: #ffb6c1;
            color: white;
            border: none;
            padding: 14px 10px;
            font-size: 18px;
            border-radius: 12px;
            cursor: pointer;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.1);
            transition: transform 0.1s, background-color 0.2s;
            font-family: inherit;
            box-shadow: 0 4px 0 #e6a1ad;
        }

        .option-btn:hover {
            background-color: #ff99aa;
        }

        .option-btn:active {
            transform: translateY(4px);
            box-shadow: none;
        }

        /* 簡答題輸入區 */
        #text-answer-container {
            display: none;
            flex-direction: column;
            gap: 15px;
            align-items: center;
        }

        #answer-input {
            width: 90%;
            padding: 12px;
            font-size: 18px;
            border-radius: 12px;
            border: 3px solid #ffb6c1;
            text-align: center;
            font-family: inherit;
            outline: none;
        }

        #submit-btn {
            background-color: #6fdcff;
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 12px;
            cursor: pointer;
            font-size: 18px;
            font-family: inherit;
            box-shadow: 0 4px 0 #52b8dc;
        }
        #submit-btn:active {
            transform: translateY(4px);
            box-shadow: none;
        }

        /* --- 遊戲特效 --- */
        /* 震動效果 */
        .vibrate {
            animation: vibrateAnm 0.3s linear;
        }
        @keyframes vibrateAnm {
            0% { transform: translate(0); }
            20% { transform: translate(-8px, 5px); }
            40% { transform: translate(-8px, -5px); }
            60% { transform: translate(8px, 5px); }
            80% { transform: translate(8px, -5px); }
            100% { transform: translate(0); }
        }

        /* 飛起來效果 */
        .flying {
            animation: flyAnm 0.8s cubic-bezier(0.25, 1, 0.5, 1);
        }
        @keyframes flyAnm {
            0% { transform: translateY(0) scale(1); }
            50% { transform: translateY(-150px) scale(1.1); }
            100% { transform: translateY(0) scale(1); }
        }

        /* 愛心掉落 */
        .falling-heart {
            position: fixed;
            color: #e74c3c;
            font-size: 24px;
            user-select: none;
            pointer-events: none;
            z-index: 999;
            animation: fall linear forwards;
        }
        @keyframes fall {
            0% { transform: translateY(-20px) rotate(0deg); opacity: 1; }
            100% { transform: translateY(105vh) rotate(360deg); opacity: 0; }
        }

        /* 王冠落下動畫 */
        #crown-anim {
            position: absolute;
            top: -60px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 60px;
            display: none;
            z-index: 10;
        }
        .crown-drop {
            display: block !important;
            animation: crownDropAnm 1s forwards;
        }
        @keyframes crownDropAnm {
            0% { top: -150px; opacity: 0; }
            60% { top: -40px; opacity: 1; }
            100% { top: -40px; opacity: 1; }
        }

        /* 大魔王出場蓋板 */
        #boss-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background-color: rgba(0, 0, 0, 0.85);
            display: flex;
            justify-content: center;
            align-items: center;
            color: #ff3366;
            font-size: 45px;
            font-weight: bold;
            flex-direction: column;
            z-index: 1000;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.8s ease;
        }
        #boss-overlay.active {
            opacity: 1;
            pointer-events: auto;
        }
        #boss-img-intro {
            max-width: 250px;
            max-height: 250px;
            border: 6px solid #ff3366;
            border-radius: 50%;
            margin-top: 25px;
            object-fit: cover;
        }

        /* 吐槽彈窗 */
        #tucao-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background-color: rgba(0,0,0,0.75);
            display: none;
            justify-content: center;
            align-items: center;
            color: #fff;
            font-size: 28px;
            z-index: 2000;
            text-align: center;
            padding: 30px;
            line-height: 1.5;
        }

        /* 音效與功能按鈕 */
        #sound-toggle {
            position: absolute;
            top: 15px;
            right: 15px;
            font-size: 22px;
            cursor: pointer;
            background: none;
            border: none;
            z-index: 10;
        }

        /* --- 結局與寶箱 --- */
        #final-container {
            display: none;
        }
        #chest-btn {
            background-color: #f1c40f;
            color: #ba135d;
            border: 4px solid #ba135d;
            padding: 20px 40px;
            font-size: 26px;
            border-radius: 20px;
            cursor: pointer;
            font-family: inherit;
            font-weight: bold;
            box-shadow: 0 6px 0 #b7950b;
            margin: 20px auto;
            display: block;
        }
        #chest-btn:active {
            transform: translateY(6px);
            box-shadow: none;
        }
        #final-text-container {
            display: none;
            background-color: #fff0f5;
            padding: 20px;
            border-radius: 15px;
            border: 2px dashed #ff69b4;
            margin-top: 20px;
            font-size: 20px;
            line-height: 1.6;
            text-align: left;
            white-space: pre-wrap;
        }
    </style>
</head>
<body>

    <button id="sound-toggle">🔊</button>

    <div id="boss-overlay">
        <div>⚠️ BOSS 降臨 ⚠️</div>
        <img src="" alt="Boss" id="boss-img-intro">
    </div>

    <div id="tucao-overlay"></div>

    <div id="game-container">
        <div id="crown-anim">👑</div>
        <div id="hp-container"></div>
        
        <div id="quiz-section">
            <div id="image-wrapper">
                <img src="" alt="題目照片" id="q-image">
            </div>
            <div id="question-text">載入中...</div>
            
            <div id="options-container"></div>
            
            <div id="text-answer-container">
                <input type="text" id="answer-input" placeholder="請輸入答案...">
                <button id="submit-btn">送出答案</button>
            </div>
        </div>

        <div id="final-container">
            <h2 style="color: #ff4477;">🎉 闖關結束囉！ 🎉</h2>
            <p id="ending-hint-msg">不管你有沒有擊敗大魔王，都可以直接領取你的專屬獎勵！</p>
            <button id="chest-btn">打開寶箱 🎁</button>
            <div id="final-text-container">
                <p id="final-text"></p>
            </div>
        </div>
    </div>

    <audio id="snd-correct" src="https://actions.google.com/sounds/v1/cartoon/clang_and_wobble.ogg"></audio>
    <audio id="snd-wrong" src="https://actions.google.com/sounds/v1/cartoon/boing.ogg"></audio>
    <audio id="snd-boss" src="https://actions.google.com/sounds/v1/horror/ambient_hum_fast.ogg"></audio>
    <audio id="snd-chest" src="https://actions.google.com/sounds/v1/cartoon/metallic_clank.ogg"></audio>

    <script>
        // ==========================================
        // ⚙️ 核心設定區：之後妳改這裡就好囉！
        // ==========================================
        const CONFIG = {
            // 1. 照片網址替換區（把引號內換成妳上傳後的網址）
            bossImg: "https://images.unsplash.com/photo-1517841905240-472988babdf9?w=500",      // BOSS 出場照 (可換妳的搞怪照)
            footImg: "https://images.unsplash.com/photo-1562183241-b937e95585b6?w=500",      // 足控題會顯示的腳照片
            
            // 隨機輪流出現在一般題目的照片們（可以放入妳傳給我的那些合照、生活照，數量不限）
            defaultQImgs: [
                "https://images.unsplash.com/photo-1516450360452-9312f5e86fc7?w=500",
                "https://images.unsplash.com/photo-1511671782779-c97d3d27a1d4?w=500",
                "https://images.unsplash.com/photo-1492684223066-81342ee5ff30?w=500"
            ],

            // 2. 溫馨結局文字（寫好後把下面這段換掉，要換行直接換行即可）
            endingText: "親愛的：\n\n恭喜你順利玩到最後啦！這是一段預設的溫馨文字 ❤️\n謝謝你包容我的小脾氣、我的白目還有我天天滑手機。\n未來的日子裡，我們還要繼續一起吃壽司、一起原地起飛！\n\n永遠愛你喲 (๑´ㅂ`๑) ✨"
        };

        // ==========================================
        // 題庫設定 (最後 10 題強制留給大 BOSS 關卡)
        // ==========================================
        const rawQuestions = [
            // 前 15 題（會自動打亂順序）
            { q: "我最常生氣的原因是？", o: ["A你太白目", "B太餓", "C單純脾氣不好"], a: 0 },
            { q: "我們第一次牽手是在", o: ["A電影院", "B公園", "C學校斜坡"], a: 2 },
            { q: "我最常說的暱稱是？", o: ["A寶貝", "B豬頭", "C徐張帝"], a: 1 },
            { q: "我最喜歡的食物是？", o: ["A巧克力", "B壽司", "C想吃什麼就吃什麼乾妳屁事"], a: 1 },
            { q: "我最常熬夜做什麼？", o: ["A看漫畫", "B講電話", "C滑手機"], a: 2 },
            { q: "我最喜歡的顏色是？", o: ["A藍色", "B彩色", "C綠色"], a: 2 },
            { q: "第一次打耳洞是什麼時候", o: ["A國中一年級", "B國中二年級", "C國中三年級"], a: -1, note: "全錯 因為我忘記了 😆" },
            { q: "我最想成為什麼", o: ["A超級大便人", "B農夫", "C有錢人的小三"], a: -1, note: "全錯 是我都想當 🤪" },
            { q: "如果世界末日我會做什麼", o: ["A跟你見面", "B等死", "C揍爆全世界"], a: -1, note: "全錯 我會在家裡滑手機 📱" },
            { q: "我近期最喜歡的一部漫畫", o: ["A衝鋒衣", "B初戀標籤", "C枯萎的花淚"], a: 1 },
            { q: "我最喜歡的休閒活動", o: ["A滑手機", "B睡覺", "C當農夫"], a: 0 },
            { q: "我媽什麼星座（簡答題）", type: "text", a: "摩羯座" },
            { q: "我上課想睡覺時我會怎麼做", o: ["A硬撐", "B喝水，讓自己清醒", "C滑手機"], a: -1, note: "全錯 我會直接睡覺 💤" },
            { q: "如果我和你媽掉進水裡妳救誰", o: ["救我媽", "救妳"], a: "special_water" },
            { q: "我走路都先邁左腳還是右腳", o: ["A左", "B右"], a: "special_fly" },

            // 👑 預留給大 BOSS 的最後 10 題 👑
            { q: "你是足控嗎", o: ["A是", "B不是"], a: "special_foot" },
            { q: "如果我看到老奶奶要過馬路我會做什麼", o: ["A扶他過馬路", "B揍他一拳", "C冷眼旁觀"], a: "special_grandma" },
            { q: "我們第一次約會在哪裡", o: ["A電影院", "B妳家", "C公園"], a: 0 },
            { q: "我是一個怎麼樣的人", o: ["A性感", "B可愛", "C67"], a: -1, note: "都錯 我是一個87的人 🤡" },
            { q: "請說出跟我玩在一起的四個朋友的名字（簡答題，免符號空一格即可）", type: "text", a: "special_friends" },
            { q: "我哥什麼星座（簡答題）", type: "text", a: "雙魚座" },
            { q: "我爸什麼時候生日（簡答題，例如：1/1）", type: "text", a: "5/5" },
            { q: "我寫作業的時候會做什麼", o: ["A聽音樂", "B唱歌", "C滑手機"], a: -1, note: "都錯，我不想寫作業 ✏️" },
            { q: "我最想做的一件事", o: ["A揍你一拳", "B成為有錢人", "C當香蕉大王"], a: 2, trigger: "banana" },
            { q: "如果荒野亂鬥的排位開始了，但是我媽叫我，我會做什麼", o: ["A立刻過去找我媽", "B繼續打", "C站起來跳舞"], a: -1, note: "都錯 我會直接變成愛德加 🦇" }
        ];

        // ==========================================
        // 遊戲系統驅動引擎 (核心邏輯請勿改動)
        // ==========================================
        let currentHp = 100;
        let isSoundOn = true;
        let isBossStage = false;
        let bossCorrectStreak = 0; 
        let bossTotalCorrect = 0;   
        let normalQuizList = [];    
        let bossQuizList = [];      
        let currentIdx = 0;

        // 綁定 DOM
        const hpBox = document.getElementById('hp-container');
        const qTextBox = document.getElementById('question-text');
        const optBox = document.getElementById('options-container');
        const textContainer = document.getElementById('text-answer-container');
        const textInput = document.getElementById('answer-input');
        const submitBtn = document.getElementById('submit-btn');
        const quizSection = document.getElementById('quiz-section');
        const finalSection = document.getElementById('final-container');
        const chestBtn = document.getElementById('chest-btn');
        const finalText = document.getElementById('final-text');
        const soundBtn = document.getElementById('sound-toggle');
        const tcOverlay = document.getElementById('tucao-overlay');
        const qImgEl = document.getElementById('q-image');
        const bossOverlay = document.getElementById('boss-overlay');
        const bossImgIntro = document.getElementById('boss-img-intro');

        function init() {
            // 切割題庫：前 15 題打亂，後 10 題留作 Boss
            let pool = [...rawQuestions];
            bossQuizList = pool.splice(-10);
            normalQuizList = shuffle(pool);

            bossImgIntro.src = CONFIG.bossImg;
            finalText.innerText = CONFIG.endingText;

            updateHp();
            renderQuestion();

            soundBtn.onclick = () => {
                isSoundOn = !isSoundOn;
                soundBtn.innerText = isSoundOn ? '🔊' : '🔇';
            };
            submitBtn.onclick = checkTextAnswer;
            chestBtn.onclick = openChest;
        }

        function shuffle(arr) {
            for (let i = arr.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [arr[i], arr[j]] = [arr[j], arr[i]];
            }
            return arr;
        }

        function updateHp() {
            let heartCount = Math.ceil(currentHp / 20);
            if (currentHp <= 0) {
                hpBox.innerHTML = `💔 <span class="hp-text">${currentHp} HP</span>`;
            } else {
                let hearts = '';
                for(let i=0; i<Math.max(0, heartCount); i++) hearts += '❤️';
                hpBox.innerHTML = `${hearts} <span class="hp-text">${currentHp} HP</span>`;
            }
        }

        function renderQuestion() {
            let q;
            if (!isBossStage) {
                if (currentIdx < normalQuizList.length) {
                    q = normalQuizList[currentIdx];
                } else {
                    enterBossStage();
                    return;
                }
            } else {
                // Boss 關卡過關檢測
                if (bossCorrectStreak >= 3 || bossTotalCorrect >= 5 || currentIdx >= bossQuizList.length) {
                    finishGame();
                    return;
                }
                q = bossQuizList[currentIdx];
            }

            // 畫面重置
            optBox.innerHTML = '';
            textContainer.style.display = 'none';
            optBox.style.display = 'grid';
            
            // 隨機或指定照片輪播
            if (q.a === "special_foot") {
                qImgEl.src = CONFIG.footImg;
            } else {
                let randomImg = CONFIG.defaultQImgs[Math.floor(Math.random() * CONFIG.defaultQImgs.length)];
                qImgEl.src = randomImg;
            }

            qTextBox.innerText = (isBossStage ? `👹【BOSS關卡】` : `✨`) + q.q;

            if (q.type === "text") {
                optBox.style.display = 'none';
                textContainer.style.display = 'flex';
                textInput.value = '';
                textInput.focus();
            } else {
                q.o.forEach((opt, idx) => {
                    const btn = document.createElement('button');
                    btn.innerText = opt;
                    btn.classList.add('option-btn');
                    btn.onclick = () => checkOptionAnswer(idx);
                    optBox.appendChild(btn);
                });
            }
        }

        function checkOptionAnswer(idx) {
            let q = isBossStage ? bossQuizList[currentIdx] : normalQuizList[currentIdx];
            let isCorrect = false;

            if (q.a === -1) {
                isCorrect = false; 
                triggerPopup(q.note || "都錯啦！直接扣分！");
            } else if (typeof q.a === 'string' && q.a.startsWith('special_')) {
                isCorrect = false;
                handleSpecialLogic(q.a, idx);
            } else if (idx === q.a) {
                isCorrect = true;
            }

            // 特殊王冠動畫觸發
            if (q.trigger === "banana" && idx === 2) {
                document.getElementById('crown-anim').classList.add('crown-drop');
                setTimeout(() => document.getElementById('crown-anim').classList.remove('crown-drop'), 1500);
            }

            judge(isCorrect);
        }

        function checkTextAnswer() {
            let q = isBossStage ? bossQuizList[currentIdx] : normalQuizList[currentIdx];
            let val = textInput.value.trim();
            let isCorrect = false;

            if (q.a === "special_friends") {
                // 四個朋友名字檢測
                let names = ["王盈穎", "陳琳綾", "劉千瑜", "吳柏儒"];
                let count = 0;
                names.forEach(n => { if(val.includes(n)) count++; });
                isCorrect = (count >= 4);
                if(!isCorrect) triggerPopup("名字打錯或不齊全喔！齁氣氣氣！");
            } else {
                isCorrect = (val === q.a);
            }

            judge(isCorrect);
        }

        function handleSpecialLogic(type, idx) {
            if (type === "special_water") {
                triggerPopup(idx === 0 ? "你不愛我了 😭" : "不肖子 😠");
            } else if (type === "special_fly") {
                triggerPopup("都錯！我會原地起飛！✈️");
                document.body.classList.add('flying');
                setTimeout(() => document.body.classList.remove('flying'), 800);
            } else if (type === "special_foot") {
                triggerPopup("👣 判定：不管你是不是，都給你看腳腳照片！");
            } else if (type === "special_grandma") {
                triggerPopup("老奶奶：少年仔，揍我幹嘛？👵\n（都錯！我就是那個老奶奶！）");
            }
        }

        function triggerPopup(msg) {
            tcOverlay.innerText = msg;
            tcOverlay.style.display = 'flex';
            setTimeout(() => tcOverlay.style.display = 'none', 2200);
        }

        function judge(isCorrect) {
            if (isCorrect) {
                playSnd('snd-correct');
                dropHearts();
                if (isBossStage) {
                    bossCorrectStreak++;
                    bossTotalCorrect++;
                }
            } else {
                playSnd('snd-wrong');
                currentHp -= 5;
                updateHp();
                triggerVibrate();
                if (isBossStage) bossCorrectStreak = 0;
            }

            currentIdx++;
            setTimeout(renderQuestion, 600);
        }

        function enterBossStage() {
            isBossStage = true;
            currentIdx = 0;
            bossCorrectStreak = 0;
            bossTotalCorrect = 0;
            
            playSnd('snd-boss');
            bossOverlay.classList.add('active');
            
            setTimeout(() => {
                bossOverlay.classList.remove('active');
                renderQuestion();
            }, 2500);
        }

        function finishGame() {
            quizSection.style.display = 'none';
            finalSection.style.display = 'block';
        }

        function openChest() {
            playSnd('snd-chest');
            chestBtn.style.display = 'none';
            document.getElementById('ending-hint-msg').style.display = 'none';
            document.getElementById('final-text-container').style.display = 'block';
            setInterval(dropHearts, 400); // 瘋狂噴發愛心
        }

        // --- 小特效函數群 ---
        function dropHearts() {
            for (let i = 0; i < 12; i++) {
                const h = document.createElement('div');
                h.classList.add('falling-heart');
                h.innerText = ['❤️','💖','💝','🫶'][Math.floor(Math.random()*4)];
                h.style.left = Math.random() * 100 + 'vw';
                h.style.animationDuration = (Math.random() * 1.5 + 1.5) + 's';
                h.style.fontSize = (Math.random() * 15 + 15) + 'px';
                document.body.appendChild(h);
                setTimeout(() => h.remove(), 3000);
            }
        }

        function triggerVibrate() {
            if ("vibrate" in navigator) {
                navigator.vibrate(200);
            } else {
                document.getElementById('game-container').classList.add('vibrate');
                setTimeout(() => document.getElementById('game-container').classList.remove('vibrate'), 300);
            }
        }

        function playSnd(id) {
            if (isSoundOn) {
                const s = document.getElementById(id);
                if(s) { s.currentTime = 0; s.play().catch(()=>{}); }
            }
        }

        window.onload = init;
    </script>
</body>
</html>
