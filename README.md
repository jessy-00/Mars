[Uploading index.html…]()
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>增强版时长计算器</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: "PingFang SC", "Helvetica Neue", Arial, sans-serif;
        }
        
        body {
            background-color: #f5f5f5;
            color: #333;
            line-height: 1.6;
            padding: 10px;
            max-width: 500px;
            margin: 0 auto;
        }
        
        .container {
            background-color: white;
            border-radius: 12px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            margin-bottom: 20px;
        }
        
        header {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: white;
            padding: 20px;
            text-align: center;
        }
        
        h1 {
            font-size: 1.5rem;
            margin-bottom: 5px;
        }
        
        .current-time {
            font-size: 0.9rem;
            opacity: 0.9;
        }
        
        .input-section {
            padding: 15px;
            border-bottom: 1px solid #eee;
        }
        
        .input-group {
            display: flex;
            margin-bottom: 10px;
            width: 100%;
        }
        
        input {
            flex: 1;
            padding: 12px 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1rem;
            width: 100%;
        }
        
        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 8px;
            padding: 12px 20px;
            font-size: 1rem;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        
        button:hover {
            background-color: #45a049;
        }
        
        button:disabled {
            background-color: #cccccc;
            cursor: not-allowed;
        }
        
        .btn-end {
            background-color: #f44336;
        }
        
        .btn-end:hover {
            background-color: #d32f2f;
        }
        
        .btn-delete {
            background-color: #ff9800;
            padding: 8px 15px;
            font-size: 0.9rem;
        }
        
        .btn-delete:hover {
            background-color: #f57c00;
        }
        
        .tasks-section {
            padding: 15px;
        }
        
        .section-title {
            font-size: 1.2rem;
            margin-bottom: 15px;
            color: #333;
            font-weight: 600;
        }
        
        .task-list {
            max-height: 300px;
            overflow-y: auto;
        }
        
        .task-item {
            background-color: #f9f9f9;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 10px;
            border-left: 4px solid #6a11cb;
        }
        
        .task-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
        }
        
        .seat-number {
            font-weight: bold;
            font-size: 1.1rem;
        }
        
        .timer {
            font-size: 1.3rem;
            font-weight: bold;
            color: #2575fc;
            text-align: center;
            margin: 10px 0;
        }
        
        .task-footer {
            display: flex;
            justify-content: space-between;
            margin-top: 10px;
        }
        
        .start-time {
            font-size: 0.9rem;
            color: #666;
        }
        
        .completed-tasks {
            background-color: white;
            border-radius: 12px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            padding: 15px;
            margin-top: 20px;
            max-width: 500px;
            margin-left: auto;
            margin-right: auto;
        }
        
        .result-item {
            background-color: #f9f9f9;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 10px;
            border-left: 4px solid #4CAF50;
            position: relative;
        }
        
        .result-header {
            font-weight: bold;
            margin-bottom: 5px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .result-details {
            font-size: 0.9rem;
            color: #666;
        }
        
        .date-time {
            font-size: 0.85rem;
            color: #888;
            margin-top: 5px;
        }
        
        .empty-state {
            text-align: center;
            color: #999;
            padding: 20px;
        }
        
        /* 统一宽度设置 - 以已完成任务栏宽度为准 */
        .input-section,
        .tasks-section {
            width: calc(100% - 30px);
            margin: 0 auto;
        }
        
        .input-group,
        .task-item {
            width: 100%;
        }
        
        /* 响应式调整 */
        @media (max-width: 480px) {
            body {
                padding: 5px;
            }
            
            .task-header, .task-footer {
                flex-direction: column;
            }
            
            .task-footer button {
                margin-top: 10px;
            }
            
            .input-group {
                flex-direction: column;
            }
            
            #startBtn {
                margin-left: 0;
                margin-top: 10px;
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>增强版时长计算器</h1>
            <div class="current-time" id="currentTime">--:--:--</div>
        </header>
        
        <div class="input-section">
            <div class="input-group">
                <input type="text" id="seatInput" placeholder="请输入座位号">
                <button id="startBtn">开始计时</button>
            </div>
        </div>
        
        <div class="tasks-section">
            <h2 class="section-title">进行中的任务</h2>
            <div class="task-list" id="activeTasks">
                <!-- 动态添加的任务将显示在这里 -->
                <div class="empty-state">暂无进行中的任务</div>
            </div>
        </div>
    </div>
    
    <div class="completed-tasks">
        <h2 class="section-title">已完成的任务</h2>
        <div id="completedTasks">
            <!-- 动态添加的已完成任务将显示在这里 -->
            <div class="empty-state">暂无已完成的任务</div>
        </div>
    </div>

    <script>
        // 存储所有任务
        let tasks = {};
        let completedTasks = [];
        
        // 更新当前时间
        function updateCurrentTime() {
            const now = new Date();
            const dateString = now.toLocaleDateString('zh-CN');
            const timeString = now.toLocaleTimeString('zh-CN');
            document.getElementById('currentTime').textContent = `${dateString} ${timeString}`;
        }
        
        // 初始化
        function init() {
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
        }
        
        // 开始新任务
        function startNewTask() {
            const seatInput = document.getElementById('seatInput');
            const seatNumber = seatInput.value.trim();
            
            if (!seatNumber) {
                alert('请输入座位号！');
                return;
            }
            
            if (tasks[seatNumber]) {
                alert(`座位号 ${seatNumber} 已经在计时中！`);
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
                startTime: task.startTime,
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
        }
        
        // 删除已完成任务
        function deleteCompletedTask(index) {
            if (index >= 0 && index < completedTasks.length) {
                completedTasks.splice(index, 1);
                updateCompletedTasksDisplay();
                saveData();
            }
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
        
        // 格式化日期时间为完整格式
        function formatDateTime(date) {
            const year = date.getFullYear();
            const month = (date.getMonth() + 1).toString().padStart(2, '0');
            const day = date.getDate().toString().padStart(2, '0');
            const hours = date.getHours().toString().padStart(2, '0');
            const minutes = date.getMinutes().toString().padStart(2, '0');
            const seconds = date.getSeconds().toString().padStart(2, '0');
            
            return `${year}-${month}-${day} ${hours}:${minutes}:${seconds}`;
        }
        
        // 更新进行中任务显示
        function updateTasksDisplay() {
            const activeTasksContainer = document.getElementById('activeTasks');
            
            if (Object.keys(tasks).length === 0) {
                activeTasksContainer.innerHTML = '<div class="empty-state">暂无进行中的任务</div>';
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
                    <div class="date-time">开始时间: ${formatDateTime(task.startTime)}</div>
                    <div class="task-footer">
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
            
            completedTasks.forEach((task, index) => {
                const resultElement = document.createElement('div');
                resultElement.className = 'result-item';
                resultElement.innerHTML = `
                    <div class="result-header">
                        <span>座位 ${task.seatNumber}</span>
                        <button class="btn-delete" onclick="deleteCompletedTask(${index})">删除</button>
                    </div>
                    <div class="result-details">
                        开始: ${formatDateTime(task.startTime)}<br>
                        结束: ${formatDateTime(task.endTime)}<br>
                        用时: ${formatTime(task.timeElapsed)}
                    </div>
                `;
                
                completedTasksContainer.appendChild(resultElement);
            });
        }
        
        // 保存数据到本地存储
        function saveData() {
            const data = {
                tasks: {},
                completedTasks: completedTasks
            };
            
            // 只保存必要的数据，不能保存函数或DOM元素
            for (const seatNumber in tasks) {
                data.tasks[seatNumber] = {
                    startTime: tasks[seatNumber].startTime.getTime() // 保存时间戳
                };
            }
            
            localStorage.setItem('timeTrackerData', JSON.stringify(data));
        }
        
        // 从本地存储加载数据
        function loadData() {
            const savedData = localStorage.getItem('timeTrackerData');
            
            if (savedData) {
                const data = JSON.parse(savedData);
                
                // 加载进行中任务
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
        }
        
        // 页面加载完成后初始化
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
