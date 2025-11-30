[index.html](https://github.com/user-attachments/files/23840095/index.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>多任务时长计算器</title>
    <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>⏱️</text></svg>">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: "PingFang SC", "Helvetica Neue", Arial, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
            line-height: 1.6;
            padding: 15px;
            min-height: 100vh;
        }
        
        .app-container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            overflow: hidden;
            margin-bottom: 20px;
            position: relative;
        }
        
        header {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: white;
            padding: 25px 20px;
            text-align: center;
            position: relative;
        }
        
        h1 {
            font-size: 1.5rem;
            margin-bottom: 8px;
            font-weight: 600;
        }
        
        .current-time {
            font-size: 1.1rem;
            opacity: 0.95;
            font-weight: 300;
        }
        
        .input-section {
            padding: 20px;
            background: #f8f9fa;
        }
        
        .input-group {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }
        
        input {
            flex: 1;
            padding: 15px;
            border: 2px solid #e9ecef;
            border-radius: 12px;
            font-size: 1rem;
            background: white;
            transition: border-color 0.3s;
        }
        
        input:focus {
            outline: none;
            border-color: #2575fc;
        }
        
        button {
            background: linear-gradient(135deg, #4CAF50 0%, #45a049 100%);
            color: white;
            border: none;
            border-radius: 12px;
            padding: 15px 25px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 4px 15px rgba(76, 175, 80, 0.3);
        }
        
        button:active {
            transform: translateY(2px);
            box-shadow: 0 2px 8px rgba(76, 175, 80, 0.3);
        }
        
        .btn-end {
            background: linear-gradient(135deg, #f44336 0%, #d32f2f 100%);
            box-shadow: 0 4px 15px rgba(244, 67, 54, 0.3);
        }
        
        .btn-end:active {
            box-shadow: 0 2px 8px rgba(244, 67, 54, 0.3);
        }
        
        .section {
            padding: 20px;
        }
        
        .section-title {
            font-size: 1.2rem;
            margin-bottom: 15px;
            color: #2c3e50;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .section-title::before {
            content: "•";
            color: #2575fc;
            font-size: 1.5rem;
        }
        
        .task-list {
            max-height: 40vh;
            overflow-y: auto;
        }
        
        .task-item {
            background: white;
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 15px;
            border-left: 5px solid #6a11cb;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            transition: transform 0.2s;
        }
        
        .task-item:active {
            transform: scale(0.98);
        }
        
        .task-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
        }
        
        .seat-number {
            font-weight: bold;
            font-size: 1.2rem;
            color: #2c3e50;
        }
        
        .timer {
            font-size: 2rem;
            font-weight: bold;
            color: #2575fc;
            text-align: center;
            margin: 15px 0;
            font-family: 'Courier New', monospace;
        }
        
        .task-footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 15px;
        }
        
        .start-time {
            font-size: 0.9rem;
            color: #666;
        }
        
        .completed-tasks {
            background: white;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            padding: 20px;
            margin-top: 20px;
        }
        
        .result-item {
            background: #f8f9fa;
            border-radius: 12px;
            padding: 18px;
            margin-bottom: 12px;
            border-left: 4px solid #4CAF50;
        }
        
        .result-header {
            font-weight: bold;
            margin-bottom: 8px;
            color: #2c3e50;
        }
        
        .result-details {
            font-size: 0.9rem;
            color: #666;
            line-height: 1.5;
        }
        
        .empty-state {
            text-align: center;
            color: #999;
            padding: 40px 20px;
            font-size: 1rem;
        }
        
        .empty-state::before {
            content: "📝";
            font-size: 2rem;
            display: block;
            margin-bottom: 10px;
        }
        
        /* 滚动条样式 */
        .task-list::-webkit-scrollbar {
            width: 6px;
        }
        
        .task-list::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 3px;
        }
        
        .task-list::-webkit-scrollbar-thumb {
            background: #c1c1c1;
            border-radius: 3px;
        }
        
        .task-list::-webkit-scrollbar-thumb:hover {
            background: #a8a8a8;
        }
        
        /* 响应式调整 */
        @media (max-width: 480px) {
            body {
                padding: 10px;
            }
            
            .input-group {
                flex-direction: column;
            }
            
            button {
                width: 100%;
            }
            
            .task-footer {
                flex-direction: column;
                gap: 10px;
            }
            
            .task-footer button {
                width: 100%;
            }
        }

        /* 加载动画 */
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid #f3f3f3;
            border-top: 3px solid #3498db;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body>
    <div class="app-container">
        <header>
            <h1>⏱️ 多任务时长计算器</h1>
            <div class="current-time" id="currentTime">--:--:--</div>
        </header>
        
        <div class="input-section">
            <div class="input-group">
                <input type="text" id="seatInput" placeholder="请输入座位号" autocomplete="off">
                <button id="startBtn">开始计时</button>
            </div>
        </div>
        
        <div class="section">
            <h2 class="section-title">进行中的任务</h2>
            <div class="task-list" id="activeTasks">
                <div class="empty-state">暂无进行中的任务<br>点击上方开始新任务</div>
            </div>
        </div>
    </div>
    
    <div class="completed-tasks">
        <h2 class="section-title">已完成的任务</h2>
        <div id="completedTasks">
            <div class="empty-state">暂无已完成的任务</div>
        </div>
    </div>

    <script>
        // 存储所有任务
        let tasks = {};
        let completedTasks = [];
        
        // DOM 加载完成后初始化
        document.addEventListener('DOMContentLoaded', function() {
            console.log('应用初始化中...');
            
            // 更新当前时间
            updateCurrentTime();
            setInterval(updateCurrentTime, 1000);
            
            // 加载保存的数据
            loadData();
            
            // 绑定事件
            document.getElementById('startBtn').addEventListener('click', startNewTask);
            document.getElementById('seatInput').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    startNewTask();
                }
            });
            
            // 每秒更新所有计时器
            setInterval(updateAllTimers, 1000);
            
            console.log('应用初始化完成');
        });
        
        // 更新当前时间
        function updateCurrentTime() {
            const now = new Date();
            const timeString = now.toLocaleTimeString('zh-CN');
            document.getElementById('currentTime').textContent = timeString;
        }
        
        // 开始新任务
        function startNewTask() {
            const seatInput = document.getElementById('seatInput');
            const seatNumber = seatInput.value.trim();
            
            if (!seatNumber) {
                showMessage('请输入座位号！', 'warning');
                return;
            }
            
            if (tasks[seatNumber]) {
                showMessage(`座位号 ${seatNumber} 已经在计时中！`, 'warning');
                return;
            }
            
            const startTime = new Date();
            tasks[seatNumber] = {
                startTime: startTime,
                element: null
            };
            
            // 清空输入框
            seatInput.value = '';
            
            // 更新任务显示
            updateTasksDisplay();
            
            // 保存数据
            saveData();
            
            showMessage(`座位 ${seatNumber} 开始计时`, 'success');
        }
        
        // 结束任务
        function endTask(seatNumber) {
            if (!tasks[seatNumber]) return;
            
            const task = tasks[seatNumber];
            const endTime = new Date();
            const timeElapsed = endTime - task.startTime;
            
            // 添加到已完成任务列表
            completedTasks.unshift({
                seatNumber: seatNumber,
                startTime: new Date(task.startTime),
                endTime: endTime,
                timeElapsed: timeElapsed
            });
            
            // 从进行中任务中移除
            delete tasks[seatNumber];
            
            // 更新显示
            updateTasksDisplay();
            updateCompletedTasksDisplay();
            
            // 保存数据
            saveData();
            
            showMessage(`座位 ${seatNumber} 计时完成`, 'info');
        }
        
        // 更新所有计时器显示
        function updateAllTimers() {
            const now = new Date();
            
            for (const seatNumber in tasks) {
                const task = tasks[seatNumber];
                const elapsed = now - task.startTime;
                const timerElement = document.getElementById(`timer-${seatNumber}`);
                
                if (timerElement) {
                    timerElement.textContent = formatTime(elapsed);
                }
            }
        }
        
        // 格式化时间为 HH:MM:SS
        function formatTime(milliseconds) {
            const totalSeconds = Math.floor(milliseconds / 1000);
            const hours = Math.floor(totalSeconds / 3600);
            const minutes = Math.floor((totalSeconds % 3600) / 60);
            const seconds = totalSeconds % 60;
            
            return `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }
        
        // 更新进行中任务显示
        function updateTasksDisplay() {
            const activeTasksContainer = document.getElementById('activeTasks');
            
            if (Object.keys(tasks).length === 0) {
                activeTasksContainer.innerHTML = '<div class="empty-state">暂无进行中的任务<br>点击上方开始新任务</div>';
                return;
            }
            
            activeTasksContainer.innerHTML = '';
            
            for (const seatNumber in tasks) {
                const task = tasks[seatNumber];
                const elapsed = new Date() - task.startTime;
                
                const taskElement = document.createElement('div');
                taskElement.className = 'task-item';
                taskElement.innerHTML = `
                    <div class="task-header">
                        <div class="seat-number">座位 ${seatNumber}</div>
                    </div>
                    <div class="timer" id="timer-${seatNumber}">${formatTime(elapsed)}</div>
                    <div class="task-footer">
                        <div class="start-time">开始: ${task.startTime.toLocaleTimeString('zh-CN')}</div>
                        <button class="btn-end" onclick="endTask('${seatNumber}')">结束计时</button>
                    </div>
                `;
                
                activeTasksContainer.appendChild(taskElement);
                tasks[seatNumber].element = taskElement;
            }
        }
        
        // 更新已完成任务显示
        function updateCompletedTasksDisplay() {
            const completedTasksContainer = document.getElementById('completedTasks');
            
            if (completedTasks.length === 0) {
                completedTasksContainer.innerHTML = '<div class="empty-state">暂无已完成的任务</div>';
                return;
            }
            
            completedTasksContainer.innerHTML = '';
            
            // 只显示最近10个已完成任务
            const recentTasks = completedTasks.slice(0, 10);
            
            recentTasks.forEach(task => {
                const resultElement = document.createElement('div');
                resultElement.className = 'result-item';
                resultElement.innerHTML = `
                    <div class="result-header">座位 ${task.seatNumber}</div>
                    <div class="result-details">
                        开始: ${task.startTime.toLocaleTimeString('zh-CN')}<br>
                        结束: ${task.endTime.toLocaleTimeString('zh-CN')}<br>
                        用时: ${formatTime(task.timeElapsed)}
                    </div>
                `;
                
                completedTasksContainer.appendChild(resultElement);
            });
        }
        
        // 显示消息提示
        function showMessage(message, type = 'info') {
            // 简单的消息提示实现
            console.log(`${type}: ${message}`);
        }
        
        // 保存数据到本地存储
        function saveData() {
            try {
                const data = {
                    tasks: {},
                    completedTasks: completedTasks.map(task => ({
                        ...task,
                        startTime: task.startTime.getTime(),
                        endTime: task.endTime.getTime()
                    }))
                };
                
                for (const seatNumber in tasks) {
                    data.tasks[seatNumber] = {
                        startTime: tasks[seatNumber].startTime.getTime()
                    };
                }
                
                localStorage.setItem('timeTrackerData', JSON.stringify(data));
            } catch (error) {
                console.error('保存数据失败:', error);
            }
        }
        
        // 从本地存储加载数据
        function loadData() {
            try {
                const savedData = localStorage.getItem('timeTrackerData');
                
                if (savedData) {
                    const data = JSON.parse(savedData);
                    
                    // 加载进行中任务
                    tasks = {};
                    for (const seatNumber in data.tasks) {
                        tasks[seatNumber] = {
                            startTime: new Date(data.tasks[seatNumber].startTime)
                        };
                    }
                    
                    // 加载已完成任务
                    if (data.completedTasks) {
                        completedTasks = data.completedTasks.map(task => ({
                            seatNumber: task.seatNumber,
                            startTime: new Date(task.startTime),
                            endTime: new Date(task.endTime),
                            timeElapsed: task.timeElapsed
                        }));
                    }
                    
                    // 更新显示
                    updateTasksDisplay();
                    updateCompletedTasksDisplay();
                }
            } catch (error) {
                console.error('加载数据失败:', error);
            }
        }
        
        // 全局函数，供HTML调用
        window.endTask = endTask;
    </script>
</body>
</html>
