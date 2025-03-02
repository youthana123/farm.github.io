<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Farm Dashboard</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', sans-serif;
        }
        body {
            background: linear-gradient(to bottom, #a8e6cf, #dcedc1);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            text-align: center;
            padding: 20px;
        }
        .container {
            width: 100%;
            max-width: 500px;
            background: white;
            padding: 25px;
            border-radius: 20px;
            box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.2);
            transition: 0.3s;
        }
        h1 {
            color: #2e7d32;
            margin-bottom: 15px;
            font-weight: 700;
            font-size: 24px;
        }
        .status {
            padding: 20px 10px;
            display: flex;
            justify-content: space-around;
            align-items: center;
            gap: 15px;
        }
        .status-card {
            padding: 20px;
            border-radius: 15px;
            box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.15);
            width: 50%;
            text-align: center;
            transition: 0.3s;
        }
        .temperature-card {
            background: #ffebee; 
            color: #d32f2f; 
        }
        .humidity-card {
            background: #e3f2fd; 
            color: #1976d2; 
        }
        .status-card h2 {
            font-size: 16px;
            font-weight: 500;
            margin-bottom: 5px;
        }
        .status-card p {
            font-size: 28px;
            font-weight: 700;
        }
        .button-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            margin-top: 20px;
            gap: 10px;
        }
        button {
            width: 48%;
            padding: 14px;
            border: none;
            border-radius: 10px;
            font-size: 18px;
            cursor: pointer;
            transition: 0.3s;
            font-weight: 600;
            background: #eeeeee;
            color: black;
        }
        .auto-btn { background: #4caf50; color: white; }
        .manual-btn { background: #ff9800; color: white; }
        .extra-btn { background: #673ab7; color: white; }
        .extra-btn2 { background: #009688; color: white; }
        button:hover {
            transform: scale(1.05);
            opacity: 0.9;
        }
        button:active {
            transform: scale(0.95);
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Smart Farm Dashboard</h1>
        <img src="https://www.kasetnumchok.com/wp-content/uploads/2018/12/RECO0080-e1479978229938-1000x576-1.jpg" alt="Smart Farm" style="width:100%; border-radius:15px; margin-bottom:20px;">
        <div class="status">
            <div class="status-card temperature-card">
                <h2>🌡 Temperature</h2>
                <p id="temperature-value">--°C</p>
            </div>
            <div class="status-card humidity-card">
                <h2>💧 Humidity</h2>
                <p id="humidity-value">--%</p>
            </div>
        </div>
        <div class="button-container">
            <button class="auto-btn" id="autoBtn" onclick="toggleMode('auto')">Auto Mode</button>
            <button class="manual-btn" id="manualBtn" onclick="toggleMode('manual')">Manual Mode</button>
            <button class="extra-btn" id="system1Btn" onclick="controlSystem('system1')">System 1</button>
            <button class="extra-btn2" id="system2Btn" onclick="controlSystem('system2')">System 2</button>
        </div>
    </div>

    <script>
        function updateData() {
            fetch('https://api.thinger.io/v3/users/YOUR_USER/devices/YOUR_DEVICE')
                .then(response => response.json())
                .then(data => {
                    document.getElementById('temperature-value').textContent = data.temperature ? data.temperature + '°C' : '--°C';
                    document.getElementById('humidity-value').textContent = data.humidity ? data.humidity + '%' : '--%';
                })
                .catch(error => console.error('Error fetching data:', error));
        }

        function toggleMode(mode) {
            fetch(`https://api.thinger.io/v3/users/YOUR_USER/devices/YOUR_DEVICE/mode`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ mode })
            })
            .then(() => {
                let button = document.getElementById(mode === 'auto' ? 'autoBtn' : 'manualBtn');
                button.textContent = button.textContent.includes('On') ? button.textContent.replace(' On', '') : button.textContent + ' On';
            })
            .catch(error => console.error('Error setting mode:', error));
        }

        function controlSystem(system) {
            fetch(`https://api.thinger.io/v3/users/YOUR_USER/devices/YOUR_DEVICE/${system}`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ action: 'toggle' })
            })
            .then(() => {
                let button = document.getElementById(system === 'system1' ? 'system1Btn' : 'system2Btn');
                button.textContent = button.textContent.includes('On') ? button.textContent.replace(' On', '') : button.textContent + ' On';
            })
            .catch(error => console.error('Error controlling system:', error));
        }

        setInterval(updateData, 5000);
    </script>
</body>
</html>
