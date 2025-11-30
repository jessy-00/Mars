[index.html](https://github.com/user-attachments/files/23840602/index.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>多设备同步时长计算器</title>
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
            padding: 10px;
            min-height: 100vh;
        }
        
        .app-container {
            background: white;
            border-radius: 16px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
            overflow: hidden;
            margin-bottom: 15px;
            position: relative;
            max-width: 100%;
        }
        
        header {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: white;
            padding: 20px 15px;
            text-align: center;
            position: relative;
        }
        
        h1 {
            font-size: 1.4rem;
            margin-bottom: 8px;
            font-weight: 600;
        }
        
        .current-time {
            font-size: 1rem;
            opacity: 0.95;
            font-weight: 300;
        }
        
        .sync-status {
            font-size: 0.8rem;
            margin-top: 5px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }
        
        .sync-indicator {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background-color: #4CAF50;
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% { opacity: 0.5; }
            50% { opacity: 1; }
            100% { opacity: 0.5; }
        }
        
        .input-section {
            padding: 15px;
            background: #f8f9fa;
        }
        
        .input-group {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }
        
        .input-row {
            display: flex;
            gap: 10px;
            margin-bottom: 10px;
        }
        
        input {
            flex: 1;
            padding: 12px 15px;
            border: 2px solid #e9ecef;
            border-radius: 10px;
            font-size: 1rem;
            background: white;
            transition: border-color 0.3s;
            min-width: 0;
        }
        
        input:focus {
            outline: none;
            border-color: #2575fc;
        }
        
        .time-input {
            max-width: 120px;
        }
        
        button {
            background: linear-gradient(135deg, #4CAF50 0%, #45a049 100%);
            color: white;
            border: none;
            border-radius: 10px;
            padding: 12px 20px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 4px 12px rgba(76, 175, 80, 0.3);
            white-space: nowrap;
        }
        
        button:active {
            transform: translateY(2px);
            box-shadow: 0 2px 6px rgba(76, 175, 80, 0.3);
        }
        
        .btn-end {
            background: linear-gradient(135deg, #f44336 0%, #d32f2f 100%);
            box-shadow: 0 4px 12px rgba(244, 67, 54, 0.3);
        }
        
        .btn-end:active {
            box-shadow: 0 2px 6px rgba(244, 67, 54, 0.3);
        }
        
        .btn-delete {
            background: linear-gradient(135deg, #ff9800 0%, #f57c00 100%);
            padding: 8px 12px;
            font-size: 0.85rem;
            box-shadow: 0 4px 10px rgba(255, 152, 0, 0.3);
        }
        
        .btn-delete:active {
            box-shadow: 0 2px 5px rgba(255, 152, 0, 0.3);
        }
        
        .section {
            padding: 15px;
        }
        
        .section-title {
            font-size: 1.1rem;
            margin-bottom: 12px;
            color: #2c3e50;
            font-weight: 600;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        
        .section-title-text {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .section-title-text::before {
            content: "•";
            color: #2575fc;
            font-size: 1.3rem;
        }
        
        .task-count {
            font-size: 0.9rem;
            color: #666;
            background: #f1f3f4;
            padding: 2px 8px;
            border-radius: 10px;
        }
        
        .task-list {
            max-height: 35vh;
            overflow-y: auto;
        }
        
        .task-item {
            background: white;
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 12px;
            border-left: 4px solid #6a11cb;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
            transition: transform 0.2s;
        }
        
        .task-item:active {
            transform: scale(0.98);
        }
        
        .task-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }
        
        .seat-number {
            font-weight: bold;
            font-size: 1.1rem;
            color: #2c3e50;
        }
        
        .timer {
            font-size: 1.6rem;
            font-weight: bold;
            color: #2575fc;
            text-align: center;
            margin: 12px 0;
            font-family: 'Courier New', monospace;
        }
        
        .planned-time {
            font-size: 0.9rem;
            color: #666;
            text-align: center;
            margin-bottom: 5px;
        }
        
        .task-footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 12px;
        }
        
        .start-time {
            font-size: 0.85rem;
            color: #666;
        }
        
        .completed-tasks {
            background: white;
            border-radius: 16px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.1);
            padding: 15px;
            margin-top: 15px;
            max-width: 100%;
        }
        
        .result-item {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 10px;
            border-left: 4px solid #4CAF50;
            position: relative;
        }
        
        .result-header {
            font-weight: bold;
            margin-bottom: 8px;
            color: #2c3e50;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .result-details {
            font-size: 0.85rem;
            color: #666;
            line-height: 1.4;
        }
        
        .overtime {
            color: #f44336;
            font-weight: bold;
            margin-top: 5px;
        }
        
        .date-time {
            font-size: 0.8rem;
            color: #888;
            margin-top: 5px;
        }
        
        .empty-state {
            text-align: center;
            color: #999;
            padding: 30px 15px;
            font-size: 0.95rem;
        }
        
        .empty-state::before {
            content: "📝";
            font-size: 1.8rem;
            display: block;
            margin-bottom: 8px;
        }
        
        /* 滚动条样式 */
        .task-list::-webkit-scrollbar {
            width: 5px;
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
                padding: 8px;
            }
            
            .input-group {
                flex-direction: column;
            }
            
            .input-row {
                flex-direction: column;
            }
            
            .time-input {
                max-width: 100%;
            }
            
            button {
                width: 100%;
            }
            
            .task-footer {
                flex-direction: column;
                gap: 8px;
            }
            
            .task-footer button {
                width: 100%;
            }
            
            .result-header {
                flex-direction: column;
                align-items: flex-start;
                gap: 8px;
            }
            
            .timer {
                font-size: 1.4rem;
            }
            
            .section-title {
                font-size: 1rem;
            }
        }
        
        @media (max-width: 360px) {
            header {
                padding: 15px 10px;
            }
            
            h1 {
                font-size: 1.2rem;
            }
            
            .current-time {
                font-size: 0.9rem;
            }
            
            .input-section, .section {
                padding: 12px;
            }
            
            .task-item, .result-item {
                padding: 12px;
            }
            
            .timer {
                font-size: 1.3rem;
            }
        }

        /* 加载动画 */
        .loading {
            display: inline-block;
            width: 18px;
            height: 18px;
            border: 2px solid #f3f3f3;
            border-top: 2px solid #3498db;
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
            <h1>⏱️ 多设备同步时长计算器</h1>
            <div class="current-time" id="currentTime">--:--:--</div>
            <div class="sync-status">
                <div class="sync-indicator"></div>
                <span id="syncStatus">数据已同步</span>
            </div>
        </header>
        
        <div class="input-section">
            <div class="input-row">
                <input type="text" id="seatInput" placeholder="请输入座位号" autocomplete="off">
                <input type="number" id="plannedMinutes" class="time-input" placeholder="计划分钟数" min="1" max="480" autocomplete="off">
            </div>
            <div class="input-group">
                <button id="startBtn">开始计时</button>
            </div>
        </div>
        
        <div class="section">
            <h2 class="section-title">
                <span class="section-title-text">进行中的任务</span>
                <span class="task-count" id="activeTaskCount">0</span>
            </h2>
            <div class="task-list" id="activeTasks">
                <div class="empty-state">暂无进行中的任务<br>点击上方开始新任务</div>
            </div>
        </div>
    </div>
    
    <div class="completed-tasks">
        <h2 class="section-title">
            <span class="section-title-text">已完成的任务</span>
            <span class="task-count" id="completedTaskCount">0</span>
        </h2>
        <div id="completedTasks">
            <div class="empty-state">暂无已完成的任务</div>
        </div>
    </div>

    <script>
        // 存储所有任务
        let tasks = {};
        let completedTasks = [];
        let lastSyncTime = Date.now();
        let syncInterval;
        
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
            
            document.getElementById('plannedMinutes').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    startNewTask();
                }
            });
            
            // 每秒更新所有计时器
            setInterval(updateAllTimers, 1000);
            
            // 设置数据同步检查
            syncInterval = setInterval(checkDataSync, 5000);
            
            // 监听页面可见性变化，当页面重新激活时检查数据同步
            document.addEventListener('visibilitychange', function() {
                if (!document.hidden) {
                    checkDataSync();
                }
            });
            
            console.log('应用初始化完成');
        });
        
        // 更新当前时间
        function updateCurrentTime() {
            const now = new Date();
            const dateString = now.toLocaleDateString('zh-CN');
            const timeString = now.toLocaleTimeString('zh-CN');
            document.getElementById('currentTime').textContent = `${dateString} ${timeString}`;
        }
        
        // 开始新任务
        function startNewTask() {
            const seatInput = document.getElementById('seatInput');
            const plannedMinutesInput = document.getElementById('plannedMinutes');
            const seatNumber = seatInput.value.trim();
            const plannedMinutes = parseInt(plannedMinutesInput.value);
            
            if (!seatNumber) {
                showMessage('请输入座位号！', 'warning');
                return;
            }
            
            if (!plannedMinutes || plannedMinutes < 1) {
                showMessage('请输入有效的计划分钟数！', 'warning');
                return;
            }
            
            if (tasks[seatNumber]) {
                showMessage(`座位号 ${seatNumber} 已经在计时中！`, 'warning');
                return;
            }
            
            const startTime = new Date();
            tasks[seatNumber] = {
                startTime: startTime,
                plannedMinutes: plannedMinutes,
                element: null
            };
            
            // 清空输入框
            seatInput.value = '';
            plannedMinutesInput.value = '';
            
            // 更新任务显示
            updateTasksDisplay();
            
            // 保存数据
            saveData();
            
            showMessage(`座位 ${seatNumber} 开始计时，计划时长 ${plannedMinutes} 分钟`, 'success');
        }
        
        // 结束任务
        function endTask(seatNumber) {
            if (!tasks[seatNumber]) return;
            
            const task = tasks[seatNumber];
            const endTime = new Date();
            const timeElapsed = endTime - task.startTime;
            const plannedMilliseconds = task.plannedMinutes * 60 * 1000;
            const overtime = Math.max(0, timeElapsed - plannedMilliseconds);
            
            // 添加到已完成任务列表
            completedTasks.unshift({
                seatNumber: seatNumber,
                startTime: new Date(task.startTime),
                endTime: endTime,
                timeElapsed: timeElapsed,
                plannedMinutes: task.plannedMinutes,
                overtime: overtime
            });
            
            // 从进行中任务中移除
            delete tasks[seatNumber];
            
            // 更新显示
            updateTasksDisplay();
            updateCompletedTasksDisplay();
            
            // 保存数据
            saveData();
            
            if (overtime > 0) {
                showMessage(`座位 ${seatNumber} 计时完成，超时 ${formatTime(overtime)}`, 'info');
            } else {
                showMessage(`座位 ${seatNumber} 计时完成，未超时`, 'info');
            }
        }
        
        // 删除已完成任务
        function deleteCompletedTask(index) {
            if (index >= 0 && index < completedTasks.length) {
                completedTasks.splice(index, 1);
                updateCompletedTasksDisplay();
                saveData();
                showMessage('任务已删除', 'info');
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
                
                // 检查是否超时并更新颜色
                const plannedMilliseconds = task.plannedMinutes * 60 * 1000;
                if (elapsed > plannedMilliseconds) {
                    timerElement.style.color = '#f44336';
                } else {
                    timerElement.style.color = '#2575fc';
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
        
        // 格式化分钟数为可读格式
        function formatMinutes(minutes) {
            const hours = Math.floor(minutes / 60);
            const mins = minutes % 60;
            
            if (hours > 0) {
                return `${hours}小时${mins}分钟`;
            } else {
                return `${mins}分钟`;
            }
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
            const activeTaskCount = Object.keys(tasks).length;
            document.getElementById('activeTaskCount').textContent = activeTaskCount;
            
            if (activeTaskCount === 0) {
                activeTasksContainer.innerHTML = '<div class="empty-state">暂无进行中的任务<br>点击上方开始新任务</div>';
                return;
            }
            
            activeTasksContainer.innerHTML = '';
            
            for (const seatNumber in tasks) {
                const task = tasks[seatNumber];
                const elapsed = new Date() - task.startTime;
                const plannedMilliseconds = task.plannedMinutes * 60 * 1000;
                const isOvertime = elapsed > plannedMilliseconds;
                
                const taskElement = document.createElement('div');
                taskElement.className = 'task-item';
                taskElement.innerHTML = `
                    <div class="task-header">
                        <div class="seat-number">座位 ${seatNumber}</div>
                    </div>
                    <div class="planned-time">计划时长: ${formatMinutes(task.plannedMinutes)}</div>
                    <div class="timer" id="timer-${seatNumber}" style="color: ${isOvertime ? '#f44336' : '#2575fc'}">${formatTime(elapsed)}</div>
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
            const completedTaskCount = completedTasks.length;
            document.getElementById('completedTaskCount').textContent = completedTaskCount;
            
            if (completedTaskCount === 0) {
                completedTasksContainer.innerHTML = '<div class="empty-state">暂无已完成的任务</div>';
                return;
            }
            
            completedTasksContainer.innerHTML = '';
            
            // 只显示最近10个已完成任务
            const recentTasks = completedTasks.slice(0, 10);
            
            recentTasks.forEach((task, index) => {
                const resultElement = document.createElement('div');
                resultElement.className = 'result-item';
                
                let overtimeHtml = '';
                if (task.overtime > 0) {
                    overtimeHtml = `<div class="overtime">超出时长: ${formatTime(task.overtime)}</div>`;
                }
                
                resultElement.innerHTML = `
                    <div class="result-header">
                        <span>座位 ${task.seatNumber}</span>
                        <button class="btn-delete" onclick="deleteCompletedTask(${index})">删除</button>
                    </div>
                    <div class="result-details">
                        计划时长: ${formatMinutes(task.plannedMinutes)}<br>
                        开始: ${formatDateTime(task.startTime)}<br>
                        结束: ${formatDateTime(task.endTime)}<br>
                        用时: ${formatTime(task.timeElapsed)}
                        ${overtimeHtml}
                    </div>
                `;
                
                completedTasksContainer.appendChild(resultElement);
            });
        }
        
        // 检查数据同步
        function checkDataSync() {
            const lastSavedData = localStorage.getItem('timeTrackerLastSaved');
            
            if (lastSavedData && parseInt(lastSavedData) > lastSyncTime) {
                console.log('检测到数据更新，重新加载数据');
                loadData();
                updateSyncStatus('数据已同步', '#4CAF50');
            } else {
                updateSyncStatus('数据已同步', '#4CAF50');
            }
        }
        
        // 更新同步状态显示
        function updateSyncStatus(message, color) {
            const syncStatus = document.getElementById('syncStatus');
            const syncIndicator = document.querySelector('.sync-indicator');
            
            syncStatus.textContent = message;
            syncIndicator.style.backgroundColor = color;
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
                        startTime: tasks[seatNumber].startTime.getTime(),
                        plannedMinutes: tasks[seatNumber].plannedMinutes
                    };
                }
                
                localStorage.setItem('timeTrackerData', JSON.stringify(data));
                localStorage.setItem('timeTrackerLastSaved', Date.now().toString());
                lastSyncTime = Date.now();
                
                updateSyncStatus('数据已保存', '#4CAF50');
            } catch (error) {
                console.error('保存数据失败:', error);
                updateSyncStatus('保存失败', '#f44336');
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
                            startTime: new Date(data.tasks[seatNumber].startTime),
                            plannedMinutes: data.tasks[seatNumber].plannedMinutes || 60 // 默认60分钟
                        };
                    }
                    
                    // 加载已完成任务
                    if (data.completedTasks) {
                        completedTasks = data.completedTasks.map(task => ({
                            seatNumber: task.seatNumber,
                            startTime: new Date(task.startTime),
                            endTime: new Date(task.endTime),
                            timeElapsed: task.timeElapsed,
                            plannedMinutes: task.plannedMinutes || 60, // 默认60分钟
                            overtime: task.overtime || 0
                        }));
                    }
                    
                    // 更新显示
                    updateTasksDisplay();
                    updateCompletedTasksDisplay();
                    
                    updateSyncStatus('数据已加载', '#4CAF50');
                }
            } catch (error) {
                console.error('加载数据失败:', error);
                updateSyncStatus('加载失败', '#f44336');
            }
        }
        
        // 全局函数，供HTML调用
        window.endTask = endTask;
        window.deleteCompletedTask = deleteCompletedTask;
    </script>
</body>
</html>
