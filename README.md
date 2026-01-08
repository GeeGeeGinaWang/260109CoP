<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>纸条之谜 - 校园解密游戏</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Microsoft YaHei', sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1a237e 0%, #311b92 100%);
            color: #e0e0e0;
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: rgba(30, 30, 46, 0.9);
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            overflow: hidden;
            border: 1px solid #3949ab;
        }

        /* 头部样式 */
        header {
            background: linear-gradient(to right, #3949ab, #5c6bc0);
            padding: 20px;
            text-align: center;
            border-bottom: 3px solid #ff9800;
            position: relative;
        }

        .header-content {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            flex-wrap: wrap;
        }

        .note-icon {
            font-size: 2.5rem;
            animation: float 3s ease-in-out infinite;
        }

        h1 {
            font-size: 2.2rem;
            color: white;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }

        .subtitle {
            color: #c5cae9;
            font-size: 1.1rem;
            margin-top: 5px;
        }

        .timer {
            position: absolute;
            right: 20px;
            top: 20px;
            background: rgba(0, 0, 0, 0.3);
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 1.2rem;
            font-weight: bold;
            color: #ffeb3b;
        }

        /* 主内容区域 */
        .main-content {
            display: flex;
            min-height: 600px;
            flex-wrap: wrap;
        }

        /* 左栏 - 线索纸条 */
        .clue-panel {
            flex: 1;
            min-width: 300px;
            padding: 25px;
            background: rgba(40, 40, 60, 0.8);
            border-right: 2px dashed #5c6bc0;
        }

        .clue-note {
            background: #fff8e1;
            border: 2px solid #ffb74d;
            border-radius: 10px;
            padding: 25px;
            margin-bottom: 25px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            color: #5d4037;
            font-size: 1.3rem;
            line-height: 1.6;
            position: relative;
        }

        .clue-note::before {
            content: "✉️";
            position: absolute;
            top: -15px;
            left: -15px;
            font-size: 2rem;
            background: #3949ab;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .blank {
            display: inline-block;
            min-width: 60px;
            padding: 2px 8px;
            margin: 0 5px;
            background: #ffecb3;
            border: 2px dashed #ff9800;
            border-radius: 4px;
            color: #d84315;
            font-weight: bold;
            text-align: center;
        }

        .filled-blank {
            background: #c8e6c9;
            border: 2px solid #4caf50;
            color: #2e7d32;
            animation: pop 0.3s ease-out;
        }

        .clues-collected {
            margin-top: 30px;
        }

        .clues-collected h3 {
            color: #ff9800;
            margin-bottom: 15px;
            padding-bottom: 5px;
            border-bottom: 2px solid #ff9800;
        }

        .clue-item {
            background: rgba(76, 175, 80, 0.1);
            border-left: 4px solid #4caf50;
            padding: 10px;
            margin: 8px 0;
            border-radius: 0 5px 5px 0;
            transition: all 0.3s;
        }

        /* 中栏 - 谜题区 */
        .puzzle-panel {
            flex: 2;
            min-width: 400px;
            padding: 25px;
            background: rgba(50, 50, 70, 0.8);
        }

        .story-text {
            background: rgba(30, 30, 50, 0.9);
            border-left: 4px solid #7986cb;
            padding: 15px;
            margin-bottom: 25px;
            border-radius: 0 10px 10px 0;
            line-height: 1.6;
        }

        .puzzle-card {
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid #5c6bc0;
            border-radius: 10px;
            padding: 25px;
            margin-bottom: 20px;
            transition: all 0.3s;
        }

        .puzzle-card.active {
            border-color: #ff9800;
            box-shadow: 0 0 15px rgba(255, 152, 0, 0.3);
            background: rgba(255, 152, 0, 0.05);
        }

        .puzzle-card.solved {
            border-color: #4caf50;
            background: rgba(76, 175, 80, 0.05);
        }

        .puzzle-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .puzzle-title {
            color: #90caf9;
            font-size: 1.2rem;
        }

        .status-badge {
            padding: 4px 12px;
            border-radius: 15px;
            font-size: 0.9rem;
            font-weight: bold;
        }

        .status-pending {
            background: #ff9800;
            color: white;
        }

        .status-solved {
            background: #4caf50;
            color: white;
        }

        .puzzle-content {
            margin: 20px 0;
        }

        .answer-input {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            background: rgba(255, 255, 255, 0.1);
            border: 2px solid #5c6bc0;
            border-radius: 5px;
            color: white;
            font-size: 1rem;
        }

        .answer-input:focus {
            outline: none;
            border-color: #ff9800;
            box-shadow: 0 0 10px rgba(255, 152, 0, 0.3);
        }

        .btn {
            padding: 12px 25px;
            background: linear-gradient(to right, #3949ab, #5c6bc0);
            color: white;
            border: none;
            border-radius: 25px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(57, 73, 171, 0.4);
        }

        .btn-check {
            background: linear-gradient(to right, #ff9800, #ffb74d);
        }

        .btn-check:hover {
            box-shadow: 0 5px 15px rgba(255, 152, 0, 0.4);
        }

        .btn-next {
            background: linear-gradient(to right, #4caf50, #66bb6a);
        }

        .btn-next:hover {
            box-shadow: 0 5px 15px rgba(76, 175, 80, 0.4);
        }

        .hint {
            background: rgba(25, 118, 210, 0.1);
            border-left: 3px solid #1976d2;
            padding: 10px;
            margin-top: 15px;
            font-size: 0.9rem;
            color: #90caf9;
            border-radius: 0 5px 5px 0;
            display: none;
        }

        .show-hint {
            color: #90caf9;
            cursor: pointer;
            font-size: 0.9rem;
            margin-top: 10px;
            display: inline-block;
        }

        /* 右栏 - 调查进度 */
        .progress-panel {
            flex: 1;
            min-width: 300px;
            padding: 25px;
            background: rgba(40, 40, 60, 0.8);
            border-left: 2px dashed #5c6bc0;
        }

        .suspects-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin: 20px 0;
        }

        .suspect-card {
            background: rgba(255, 255, 255, 0.05);
            border: 2px solid #5c6bc0;
            border-radius: 10px;
            padding: 15px;
            text-align: center;
            transition: all 0.3s;
            cursor: pointer;
        }

        .suspect-card:hover {
            transform: translateY(-5px);
            border-color: #ff9800;
        }

        .suspect-card.revealed {
            border-color: #4caf50;
            background: rgba(76, 175, 80, 0.1);
        }

        .suspect-avatar {
            font-size: 2.5rem;
            margin-bottom: 10px;
        }

        .suspect-name {
            font-weight: bold;
            margin: 5px 0;
        }

        .suspect-role {
            color: #b0bec5;
            font-size: 0.9rem;
        }

        .campus-map {
            background: rgba(30, 30, 50, 0.9);
            border-radius: 10px;
            padding: 15px;
            margin-top: 25px;
            text-align: center;
        }

        .map-placeholder {
            color: #b0bec5;
            font-style: italic;
            padding: 40px 20px;
        }

        .map-location {
            color: #4caf50;
            font-weight: bold;
            margin-top: 10px;
            display: none;
        }

        /* 模态窗口 */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }

        .modal-content {
            background: linear-gradient(135deg, #1a237e 0%, #311b92 100%);
            padding: 40px;
            border-radius: 20px;
            max-width: 500px;
            text-align: center;
            animation: modalIn 0.5s ease-out;
            border: 3px solid #ff9800;
        }

        .modal h2 {
            color: #ff9800;
            margin-bottom: 20px;
            font-size: 2rem;
        }

        .modal p {
            margin: 15px 0;
            line-height: 1.6;
        }

        /* 动画 */
        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        @keyframes pop {
            0% { transform: scale(0.8); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }

        @keyframes modalIn {
            from {
                opacity: 0;
                transform: translateY(-50px) scale(0.9);
            }
            to {
                opacity: 1;
                transform: translateY(0) scale(1);
            }
        }

        @keyframes celebrate {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        /* 响应式设计 */
        @media (max-width: 768px) {
            .main-content {
                flex-direction: column;
            }
            
            .clue-panel, .puzzle-panel, .progress-panel {
                border: none;
                border-bottom: 2px dashed #5c6bc0;
            }
            
            h1 {
                font-size: 1.8rem;
            }
            
            .timer {
                position: static;
                margin-top: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- 头部 -->
        <header>
            <div class="header-content">
                <div class="note-icon">🔍</div>
                <div>
                    <h1>纸条之谜：校园盗窃案</h1>
                    <div class="subtitle">一封密信，五个嫌疑人，等待你的推理</div>
                </div>
            </div>
            <div class="timer">⏱️ 剩余时间: <span id="countdown">15:00</span></div>
        </header>

        <!-- 主内容区 -->
        <div class="main-content">
            <!-- 左栏：线索纸条 -->
            <div class="clue-panel">
                <div class="clue-note">
                    <p>星期<span class="blank" id="blank-A">___A___</span>上午
                       <span class="blank" id="blank-B">___B___</span>点半，
                       你<span class="blank" id="blank-C">___C___</span>去
                       <span class="blank" id="blank-D">___D___</span>号楼
                       <span class="blank" id="blank-E">___E___</span>房间找
                       <span class="blank" id="blank-F">___F___</span>。
                    </p>
                </div>
                
                <div class="clues-collected">
                    <h3>📋 已收集线索</h3>
                    <div id="clues-list">
                        <div class="clue-item" id="clue-1">线索1：时间密码已破解</div>
                        <div class="clue-item" id="clue-2">线索2：超市消费记录分析中...</div>
                        <div class="clue-item" id="clue-3">线索3：教师通勤方式调查中...</div>
                        <div class="clue-item" id="clue-4">线索4：书店收据解密中...</div>
                        <div class="clue-item" id="clue-5">线索5：嫌疑人证词分析中...</div>
                    </div>
                </div>
            </div>

            <!-- 中栏：谜题区 -->
            <div class="puzzle-panel">
                <div class="story-text">
                    📱 <strong>匿名短信：</strong> "如果你能补全纸条上的信息，就能找到阻止校园盗窃案的关键线索。时间不多了，小偷可能就在纸条提到的地方！"
                </div>

                <!-- 谜题1 -->
                <div class="puzzle-card active" id="puzzle-1">
                    <div class="puzzle-header">
                        <div class="puzzle-title">🕐 谜题1：时间密码</div>
                        <span class="status-badge status-pending">待解决</span>
                    </div>
                    <div class="puzzle-content">
                        <p>"小偷留下了一个逻辑谜语：<strong>如果昨天是明天，那么今天就是星期五。</strong>"</p>
                        <p>请问实际上<strong>今天星期几</strong>？</p>
                        <input type="text" class="answer-input" id="answer-1" placeholder="请输入星期几（如：星期一）">
                        <div class="show-hint" onclick="showHint(1)">💡 显示提示</div>
                        <div class="hint" id="hint-1">提示：想一想"昨天是明天"这句话意味着什么？如果今天是X，那么昨天是X-1，明天是X+1。</div>
                    </div>
                    <button class="btn btn-check" onclick="checkAnswer(1)">✅ 验证答案</button>
                </div>

                <!-- 谜题2 -->
                <div class="puzzle-card" id="puzzle-2">
                    <div class="puzzle-header">
                        <div class="puzzle-title">💰 谜题2：消费记录</div>
                        <span class="status-badge status-pending">待解决</span>
                    </div>
                    <div class="puzzle-content">
                        <p>"监控显示小偷在行动前买了2斤苹果和1斤香蕉。"</p>
                        <p>苹果3.5元一斤，香蕉2元一斤。请问一共多少钱？</p>
                        <input type="number" class="answer-input" id="answer-2" placeholder="请输入总金额（元）" step="0.1">
                        <div class="show-hint" onclick="showHint(2)">💡 显示提示</div>
                        <div class="hint" id="hint-2">提示：总价 = (苹果单价 × 斤数) + (香蕉单价 × 斤数)</div>
                    </div>
                    <button class="btn btn-check" onclick="checkAnswer(2)">✅ 验证答案</button>
                </div>

                <!-- 谜题3 -->
                <div class="puzzle-card" id="puzzle-3">
                    <div class="puzzle-header">
                        <div class="puzzle-title">🏢 谜题3：教师信息</div>
                        <span class="status-badge status-pending">待解决</span>
                    </div>
                    <div class="puzzle-content">
                        <p>通过对话碎片推理老师的信息：</p>
                        <ul>
                            <li>医生说：我住102房间。</li>
                            <li>学生说：我不住102房间，我住1102房间。</li>
                            <li>老师说：我住202房间。</li>
                            <li>服务员说：我不住202房间，我住1202房间。</li>
                        </ul>
                        <p>问题1：老师怎么来学校？ 
                            <select class="answer-input" id="answer-3a">
                                <option value="">请选择</option>
                                <option value="走路">走路</option>
                                <option value="骑电动车">骑电动车</option>
                                <option value="骑自行车">骑自行车</option>
                                <option value="坐公共汽车">坐公共汽车</option>
                            </select>
                        </p>
                        <p>问题2：老师住哪个房间号？ 
                            <input type="text" class="answer-input" id="answer-3b" placeholder="请输入房间号">
                        </p>
                        <div class="show-hint" onclick="showHint(3)">💡 显示提示</div>
                        <div class="hint" id="hint-3">提示：对照已知的房间号与通勤方式：102-走路，1102-电动车，202-自行车，1202-公交车。</div>
                    </div>
                    <button class="btn btn-check" onclick="checkAnswer(3)">✅ 验证答案</button>
                </div>

                <!-- 谜题4 -->
                <div class="puzzle-card" id="puzzle-4">
                    <div class="puzzle-header">
                        <div class="puzzle-title">🧾 谜题4：书店收据</div>
                        <span class="status-badge status-pending">待解决</span>
                    </div>
                    <div class="puzzle-content">
                        <p>"在老师房间找到一张被涂改的书店收据："</p>
                        <ul>
                            <li>买一本汉语书，服务员找我5块钱</li>
                            <li>买一个本子，服务员找我8块钱</li>
                            <li>买一本汉语书和一个本子，服务员找我3块钱</li>
                        </ul>
                        <p>问：我每次付给服务员多少钱？（假设每次付的钱一样多）</p>
                        <input type="number" class="answer-input" id="answer-4" placeholder="请输入金额（元）">
                        <div class="show-hint" onclick="showHint(4)">💡 显示提示</div>
                        <div class="hint" id="hint-4">提示：设付的钱为X，书的价格为X-5，本子的价格为X-8。</div>
                    </div>
                    <button class="btn btn-check" onclick="checkAnswer(4)">✅ 验证答案</button>
                </div>

                <!-- 谜题5 -->
                <div class="puzzle-card" id="puzzle-5">
                    <div class="puzzle-header">
                        <div class="puzzle-title">🕵️ 谜题5：嫌疑人指证</div>
                        <span class="status-badge status-pending">待解决</span>
                    </div>
                    <div class="puzzle-content">
                        <p>"五个嫌疑人中只有一个人说真话，其他四人都说谎："</p>
                        <ul>
                            <li>英语老师：汉语老师是小偷。</li>
                            <li>汉语老师：英语老师是小偷。</li>
                            <li>服务员：医生不是小偷。</li>
                            <li>学生：医生是小偷。</li>
                            <li>医生：我不是小偷。</li>
                        </ul>
                        <p>问：谁是小偷？</p>
                        <select class="answer-input" id="answer-5">
                            <option value="">请选择</option>
                            <option value="英语老师">英语老师</option>
                            <option value="汉语老师">汉语老师</option>
                            <option value="服务员">服务员</option>
                            <option value="学生">学生</option>
                            <option value="医生">医生</option>
                        </select>
                        <div class="show-hint" onclick="showHint(5)">💡 显示提示</div>
                        <div class="hint" id="hint-5">提示：如果只有一个人说真话，那么其他人说的都是假话。逐一假设每个人说真话，看是否矛盾。</div>
                    </div>
                    <button class="btn btn-check" onclick="checkAnswer(5)">✅ 验证答案</button>
                </div>

                <!-- 最终按钮 -->
                <button class="btn btn-next" id="final-btn" onclick="showFinalModal()" style="display: none; width: 100%; margin-top: 20px;">
                    🔓 锁定目标，前往结局
                </button>
            </div>

            <!-- 右栏：调查进度 -->
            <div class="progress-panel">
                <h3>🕵️ 嫌疑人档案</h3>
                <div class="suspects-grid" id="suspects-grid">
                    <!-- 嫌疑人卡片将通过JS动态生成 -->
                </div>

                <div class="campus-map">
                    <h3>🗺️ 校园地图</h3>
                    <div class="map-placeholder" id="map-placeholder">
                        等待线索揭示具体位置...
                    </div>
                    <div class="map-location" id="map-location">
                        📍 目标位置：<span id="target-location">等待确定</span>
                    </div>
                </div>

                <div style="margin-top: 30px; text-align: center;">
                    <h3>🎯 调查进度</h3>
                    <div style="background: rgba(255, 255, 255, 0.1); height: 20px; border-radius: 10px; margin: 15px 0;">
                        <div id="progress-bar" style="width: 0%; height: 100%; background: linear-gradient(to right, #4caf50, #8bc34a); border-radius: 10px; transition: width 0.5s;"></div>
                    </div>
                    <div id="progress-text">0/5 谜题已解决</div>
                </div>
            </div>
        </div>
    </div>

    <!-- 结局模态窗口 -->
    <div class="modal" id="ending-modal">
        <div class="modal-content">
            <h2 id="ending-title">🎉 案件解决！</h2>
            <div id="ending-content">
                <!-- 结局内容将通过JS动态生成 -->
            </div>
            <button class="btn" onclick="restartGame()" style="margin-top: 20px;">🔄 重新挑战</button>
        </div>
    </div>

    <script>
        // 游戏状态
        const gameState = {
            currentPuzzle: 1,
            solvedPuzzles: new Set(),
            answers: {
                1: null, 2: null, 3: null, 4: null, 5: null
            },
            blanks: {
                A: null, B: null, C: null, D: null, E: null, F: null
            },
            startTime: Date.now(),
            timeLimit: 15 * 60 * 1000 // 15分钟
        };

        // 正确答案
        const correctAnswers = {
            1: "星期五",
            2: "9",
            3: { C: "骑自行车", E: "202" },
            4: "10",
            5: "医生"
        };

        // 嫌疑人数据
        const suspects = [
            { id: 1, name: "英语老师", role: "英语教师", avatar: "👨‍🏫", revealed: false },
            { id: 2, name: "汉语老师", role: "汉语教师", avatar: "👩‍🏫", revealed: false },
            { id: 3, name: "服务员", role: "校园服务员", avatar: "👨‍💼", revealed: false },
            { id: 4, name: "学生", role: "大学生", avatar: "👨‍🎓", revealed: false },
            { id: 5, name: "医生", role: "校医", avatar: "👨‍⚕️", revealed: false }
        ];

        // 初始化游戏
        function initGame() {
            renderSuspects();
            updateProgress();
            startTimer();
            activatePuzzle(1);
        }

        // 渲染嫌疑人网格
        function renderSuspects() {
            const grid = document.getElementById('suspects-grid');
            grid.innerHTML = suspects.map(suspect => `
                <div class="suspect-card ${suspect.revealed ? 'revealed' : ''}" 
                     onclick="showSuspectInfo(${suspect.id})">
                    <div class="suspect-avatar">${suspect.revealed ? suspect.avatar : '❓'}</div>
                    <div class="suspect-name">${suspect.revealed ? suspect.name : '？？？'}</div>
                    <div class="suspect-role">${suspect.revealed ? suspect.role : '身份未知'}</div>
                </div>
            `).join('');
        }

        // 显示嫌疑人信息
        function showSuspectInfo(id) {
            const suspect = suspects.find(s => s.id === id);
            if (suspect.revealed) {
                alert(`${suspect.name}（${suspect.role}）\n\n相关线索：${getSuspectClue(id)}`);
            }
        }

        function getSuspectClue(id) {
            const clues = [
                "声称汉语老师是小偷",
                "声称英语老师是小偷",
                "声称医生不是小偷",
                "声称医生是小偷",
                "声称自己不是小偷"
            ];
            return clues[id - 1];
        }

        // 激活当前谜题
        function activatePuzzle(number) {
            // 重置所有谜题卡片的激活状态
            document.querySelectorAll('.puzzle-card').forEach(card => {
                card.classList.remove('active');
            });
            
            // 激活当前谜题
            const currentCard = document.getElementById(`puzzle-${number}`);
            if (currentCard) {
                currentCard.classList.add('active');
                currentCard.scrollIntoView({ behavior: 'smooth', block: 'center' });
            }
        }

        // 显示提示
        function showHint(puzzleNumber) {
            const hint = document.getElementById(`hint-${puzzleNumber}`);
            hint.style.display = 'block';
        }

        // 检查答案
        function checkAnswer(puzzleNumber) {
            let userAnswer, isCorrect = false;
            
            switch(puzzleNumber) {
                case 1:
                    userAnswer = document.getElementById('answer-1').value.trim();
                    isCorrect = userAnswer === correctAnswers[1];
                    break;
                    
                case 2:
                    userAnswer = document.getElementById('answer-2').value;
                    isCorrect = Math.abs(parseFloat(userAnswer) - parseFloat(correctAnswers[2])) < 0.01;
                    break;
                    
                case 3:
                    const answer3a = document.getElementById('answer-3a').value;
                    const answer3b = document.getElementById('answer-3b').value.trim();
                    isCorrect = answer3a === correctAnswers[3].C && answer3b === correctAnswers[3].E;
                    userAnswer = { C: answer3a, E: answer3b };
                    break;
                    
                case 4:
                    userAnswer = document.getElementById('answer-4').value;
                    isCorrect = userAnswer === correctAnswers[4];
                    break;
                    
                case 5:
                    userAnswer = document.getElementById('answer-5').value;
                    isCorrect = userAnswer === correctAnswers[5];
                    break;
            }
            
            if (isCorrect) {
                handleCorrectAnswer(puzzleNumber, userAnswer);
            } else {
                alert('❌ 答案不正确，请再思考一下！');
            }
        }

        // 处理正确答案
        function handleCorrectAnswer(puzzleNumber, userAnswer) {
            // 标记谜题为已解决
            gameState.solvedPuzzles.add(puzzleNumber);
            const puzzleCard = document.getElementById(`puzzle-${puzzleNumber}`);
            const statusBadge = puzzleCard.querySelector('.status-badge');
            
            puzzleCard.classList.remove('active');
            puzzleCard.classList.add('solved');
            statusBadge.textContent = '已解决';
            statusBadge.className = 'status-badge status-solved';
            
            // 更新线索
            updateClue(puzzleNumber);
            
            // 更新答案
            gameState.answers[puzzleNumber] = userAnswer;
            
            // 更新纸条空白处
            updateBlanks(puzzleNumber, userAnswer);
            
            // 更新进度
            updateProgress();
            
            // 显示成功消息
            showSuccessMessage(puzzleNumber);
            
            // 如果还有未解决的谜题，激活下一个
            if (puzzleNumber < 5) {
                setTimeout(() => {
                    activatePuzzle(puzzleNumber + 1);
                }, 1000);
            }
        }

        // 更新线索
        function updateClue(puzzleNumber) {
            const clueTexts = [
                "✅ 时间密码已破解：星期五",
                "✅ 消费记录已分析：9元",
                "✅ 教师信息已查明：骑自行车，住202房间",
                "✅ 书店收据已解密：每次付10元",
                "✅ 嫌疑人分析完成：医生是小偷"
            ];
            
            const clueItem = document.getElementById(`clue-${puzzleNumber}`);
            if (clueItem) {
                clueItem.textContent = clueTexts[puzzleNumber - 1];
                clueItem.style.background = 'rgba(76, 175, 80, 0.2)';
                clueItem.style.borderLeft = '4px solid #4caf50';
            }
        }

        // 更新纸条空白处
        function updateBlanks(puzzleNumber, answer) {
            switch(puzzleNumber) {
                case 1:
                    gameState.blanks.A = answer;
                    updateBlank('A', answer);
                    break;
                case 2:
                    gameState.blanks.B = answer;
                    updateBlank('B', answer + '');
                    break;
                case 3:
                    gameState.blanks.C = answer.C;
                    gameState.blanks.E = answer.E;
                    updateBlank('C', answer.C);
                    updateBlank('E', answer.E);
                    break;
                case 4:
                    gameState.blanks.D = answer;
                    updateBlank('D', answer);
                    break;
                case 5:
                    gameState.blanks.F = answer;
                    updateBlank('F', answer);
                    // 揭示医生嫌疑人
                    const doctor = suspects.find(s => s.name === answer);
                    if (doctor) {
                        doctor.revealed = true;
                        renderSuspects();
                    }
                    break;
            }
            
            // 更新地图位置
            updateMapLocation();
        }

        function updateBlank(letter, value) {
            const blank = document.getElementById(`blank-${letter}`);
            if (blank) {
                blank.textContent = value;
                blank.classList.add('filled-blank');
            }
        }

        // 更新地图位置
        function updateMapLocation() {
            const mapPlaceholder = document.getElementById('map-placeholder');
            const mapLocation = document.getElementById('map-location');
            const targetLocation = document.getElementById('target-location');
            
            if (gameState.blanks.D && gameState.blanks.E) {
                mapPlaceholder.style.display = 'none';
                mapLocation.style.display = 'block';
                targetLocation.textContent = `${gameState.blanks.D}号楼 ${gameState.blanks.E}房间`;
            }
        }

        // 更新进度
        function updateProgress() {
            const solvedCount = gameState.solvedPuzzles.size;
            const progressBar = document.getElementById('progress-bar');
            const progressText = document.getElementById('progress-text');
            const finalBtn = document.getElementById('final-btn');
            
            const progress = (solvedCount / 5) * 100;
            progressBar.style.width = `${progress}%`;
            progressText.textContent = `${solvedCount}/5 谜题已解决`;
            
            // 如果所有谜题都解决了，显示最终按钮
            if (solvedCount === 5) {
                finalBtn.style.display = 'block';
                finalBtn.scrollIntoView({ behavior: 'smooth' });
            }
        }

        // 显示成功消息
        function showSuccessMessage(puzzleNumber) {
            const messages = [
                "🎯 时间密码破解成功！",
                "💰 消费记录分析正确！",
                "🏢 教师信息查明完成！",
                "🧾 书店收据解密成功！",
                "🕵️ 嫌疑人锁定完成！"
            ];
            
            alert(`✅ ${messages[puzzleNumber - 1]}\n\n线索已更新到调查板！`);
        }

        // 计时器
        function startTimer() {
            const countdownElement = document.getElementById('countdown');
            
            function updateTimer() {
                const elapsed = Date.now() - gameState.startTime;
                const remaining = gameState.timeLimit - elapsed;
                
                if (remaining <= 0) {
                    countdownElement.textContent = "00:00";
                    alert("⏰ 时间
