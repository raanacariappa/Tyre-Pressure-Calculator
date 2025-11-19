<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Tyre Pressure">
    <title>Tyre Pressure Calculator - RGB Racing UMA</title>
    <link rel="apple-touch-icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><rect fill='%234CAF50' width='100' height='100'/><text y='70' x='50' text-anchor='middle' font-size='60' fill='white' font-family='Arial'>TP</text></svg>">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
            padding: 20px;
        }
        .header {
            background: white;
            border-radius: 20px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            text-align: center;
        }
        h1 {
            font-size: 24px;
            color: #333;
            margin-bottom: 5px;
        }
        .subtitle {
            font-size: 14px;
            color: #666;
        }
        .card {
            background: white;
            border-radius: 20px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }
        .section-title {
            font-size: 18px;
            font-weight: 600;
            color: #333;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            font-size: 14px;
            font-weight: 500;
            color: #555;
            margin-bottom: 8px;
        }
        input, select {
            width: 100%;
            padding: 12px 15px;
            font-size: 16px;
            border: 2px solid #e0e0e0;
            border-radius: 12px;
            background: #f8f9fa;
            transition: all 0.3s;
        }
        input:focus, select:focus {
            outline: none;
            border-color: #667eea;
            background: white;
        }
        .btn {
            width: 100%;
            padding: 16px;
            font-size: 16px;
            font-weight: 600;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s;
            margin-bottom: 10px;
        }
        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
        }
        .btn-primary:active {
            transform: scale(0.98);
        }
        .btn-success {
            background: linear-gradient(135deg, #4CAF50 0%, #45a049 100%);
            color: white;
        }
        .results {
            background: #f8f9fa;
            border-radius: 12px;
            padding: 15px;
            margin-top: 15px;
        }
        .pressure-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-top: 15px;
        }
        .pressure-card {
            background: white;
            border-radius: 12px;
            padding: 15px;
            text-align: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .pressure-label {
            font-size: 14px;
            color: #666;
            margin-bottom: 5px;
        }
        .pressure-value {
            font-size: 28px;
            font-weight: 700;
            color: #4CAF50;
        }
        .pressure-unit {
            font-size: 14px;
            color: #999;
        }
        .car-viz {
            position: relative;
            width: 200px;
            height: 300px;
            margin: 20px auto;
            background: #333;
            border-radius: 40px 40px 20px 20px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.2);
        }
        .tyre {
            position: absolute;
            width: 40px;
            height: 60px;
            background: #000;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 10px;
            font-weight: 700;
            color: white;
        }
        .tyre-fl { left: -20px; top: 40px; }
        .tyre-fr { right: -20px; top: 40px; }
        .tyre-rl { left: -20px; bottom: 40px; }
        .tyre-rr { right: -20px; bottom: 40px; }
        .tab-nav {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
        }
        .tab-btn {
            flex: 1;
            min-width: 120px;
            padding: 12px;
            font-size: 14px;
            font-weight: 600;
            background: white;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s;
        }
        .tab-btn.active {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }
        .tab-content {
            display: none;
        }
        .tab-content.active {
            display: block;
        }
        .info-box {
            background: #e3f2fd;
            border-left: 4px solid #2196F3;
            padding: 12px;
            border-radius: 8px;
            font-size: 13px;
            color: #555;
            margin-top: 10px;
        }
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🏁 Tyre Pressure Calculator</h1>
            <div class="subtitle">RGB Racing UMA • v4.6 PWA</div>
        </div>
        <div class="tab-nav">
            <button class="tab-btn active" onclick="showTab('setup')">Setup</button>
            <button class="tab-btn" onclick="showTab('calculate')">Calculate</button>
            <button class="tab-btn" onclick="showTab('adjust')">Adjust</button>
            <button class="tab-btn" onclick="showTab('hot')">Hot Analysis</button>
        </div>
        <!-- Setup Tab -->
        <div id="tab-setup" class="tab-content active">
            <div class="card">
                <div class="section-title">🏎️ Car Model</div>
                <div class="form-group">
                    <label>Select Porsche Model</label>
                    <select id="carModel">
                        <option value="991">Porsche 991 GT3 Cup</option>
                        <option value="992">Porsche 992 GT3 Cup</option>
                    </select>
                </div>
            </div>
            <div class="card">
                <div class="section-title">🛣️ Track Type</div>
                <div class="form-group">
                    <label>Track Characteristic</label>
                    <select id="trackType">
                        <option value="right">Right-Hander Track</option>
                        <option value="left" selected>Left-Hander Track</option>
                        <option value="neutral">Neutral/Balanced</option>
                    </select>
                </div>
                <div class="info-box">
                    Calibrated: FL=1.27, FR=1.29, RL=1.19, RR=1.23 bar
                </div>
            </div>
            <div class="card">
                <div class="section-title">⏱️ Session Duration</div>
                <div class="form-group">
                    <label>Session Type</label>
                    <select id="sessionType">
                        <option value="0.07">Qualifying (1-3 laps) +0.07</option>
                        <option value="0.04" selected>Short Practice (5-8) +0.04</option>
                        <option value="0.02">Medium Practice (10-15) +0.02</option>
                        <option value="0.00">Race (20+) 0.00</option>
                    </select>
                </div>
            </div>
        </div>
        <!-- Calculate Tab -->
        <div id="tab-calculate" class="tab-content">
            <div class="card">
                <div class="section-title">📊 Morning Setup</div>
                <div class="form-group">
                    <label>Baseline Pressure (bar)</label>
                    <input type="number" id="baselinePressure" value="1.245" step="0.01">
                </div>
                <div class="form-group">
                    <label>Ambient Temperature (°C)</label>
                    <input type="number" id="ambientTemp" value="12" step="1">
                </div>
                <div class="form-group">
                    <label>Track Temperature (°C)</label>
                    <input type="number" id="trackTemp" value="24" step="1">
                </div>
                <button class="btn btn-primary" onclick="calculateColdPressures()">Calculate Cold Pressures</button>  
                <div id="results" class="results hidden">
                    <div class="pressure-grid">
                        <div class="pressure-card">
                            <div class="pressure-label">FL</div>
                            <div class="pressure-value" id="result-fl">-</div>
                            <div class="pressure-unit">bar</div>
                        </div>
                        <div class="pressure-card">
                            <div class="pressure-label">FR</div>
                            <div class="pressure-value" id="result-fr">-</div>
                            <div class="pressure-unit">bar</div>
                        </div>
                        <div class="pressure-card">
                            <div class="pressure-label">RL</div>
                            <div class="pressure-value" id="result-rl">-</div>
                            <div class="pressure-unit">bar</div>
                        </div>
                        <div class="pressure-card">
                            <div class="pressure-label">RR</div>
                            <div class="pressure-value" id="result-rr">-</div>
                            <div class="pressure-unit">bar</div>
                        </div>
                    </div>
                    <div class="car-viz">
                        <div class="tyre tyre-fl" id="viz-fl">FL</div>
                        <div class="tyre tyre-fr" id="viz-fr">FR</div>
                        <div class="tyre tyre-rl" id="viz-rl">RL</div>
                        <div class="tyre tyre-rr" id="viz-rr">RR</div>
                    </div>
                </div>
            </div>
        </div>
        <!-- Adjust Tab -->
        <div id="tab-adjust" class="tab-content">
            <div class="card">
                <div class="section-title">🌡️ Temperature Adjustment</div>
                <div class="form-group">
                    <label>Current FL (bar)</label>
                    <input type="number" id="current-fl" value="1.27" step="0.01">
                </div>
                <div class="form-group">
                    <label>Current FR (bar)</label>
                    <input type="number" id="current-fr" value="1.29" step="0.01">
                </div>
                <div class="form-group">
                    <label>Current RL (bar)</label>
                    <input type="number" id="current-rl" value="1.19" step="0.01">
                </div>
                <div class="form-group">
                    <label>Current RR (bar)</label>
                    <input type="number" id="current-rr" value="1.23" step="0.01">
                </div>
                <div class="form-group">
                    <label>Previous Ambient (°C)</label>
                    <input type="number" id="prev-ambient" value="12" step="1">
                </div>
                <div class="form-group">
                    <label>Previous Track (°C)</label>
                    <input type="number" id="prev-track" value="24" step="1">
                </div>
                <div class="form-group">
                    <label>New Ambient (°C)</label>
                    <input type="number" id="new-ambient" value="12" step="1">
                </div>
                <div class="form-group">
                    <label>New Track (°C)</label>
                    <input type="number" id="new-track" value="28" step="1">
                </div>
                <button class="btn btn-success" onclick="calculateAdjustment()">Calculate Adjustment</button>  
                <div id="adjust-results" class="results hidden">
                    <div class="pressure-grid">
                        <div class="pressure-card">
                            <div class="pressure-label">FL New</div>
                            <div class="pressure-value" id="adjust-fl">-</div>
                            <div class="pressure-unit">bar</div>
                        </div>
                        <div class="pressure-card">
                            <div class="pressure-label">FR New</div>
                            <div class="pressure-value" id="adjust-fr">-</div>
                            <div class="pressure-unit">bar</div>
                        </div>
                        <div class="pressure-card">
                            <div class="pressure-label">RL New</div>
                            <div class="pressure-value" id="adjust-rl">-</div>
                            <div class="pressure-unit">bar</div>
                        </div>
                        <div class="pressure-card">
                            <div class="pressure-label">RR New</div>
                            <div class="pressure-value" id="adjust-rr">-</div>
                            <div class="pressure-unit">bar</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- Hot Analysis Tab -->
        <div id="tab-hot" class="tab-content">
            <div class="card">
                <div class="section-title">🔥 Hot Pressure Analysis</div>
                <div class="form-group">
                    <label>Cold FL SET (bar)</label>
                    <input type="number" id="cold-fl" value="1.27" step="0.01">
                </div>
                <div class="form-group">
                    <label>Cold FR SET (bar)</label>
                    <input type="number" id="cold-fr" value="1.29" step="0.01">
                </div>
                <div class="form-group">
                    <label>Cold RL SET (bar)</label>
                    <input type="number" id="cold-rl" value="1.19" step="0.01">
                </div>
                <div class="form-group">
                    <label>Cold RR SET (bar)</label>
                    <input type="number" id="cold-rr" value="1.23" step="0.01">
                </div>
                <div class="form-group">
                    <label>Target Hot Pressure (bar)</label>
                    <input type="number" id="target-hot" value="1.80" step="0.01">
                </div>
                <div class="form-group">
                    <label>Measured Hot FL (bar)</label>
                    <input type="number" id="hot-fl" value="1.87" step="0.01">
                </div>
                <div class="form-group">
                    <label>Measured Hot FR (bar)</label>
                    <input type="number" id="hot-fr" value="1.93" step="0.01">
                </div>
                <div class="form-group">
                    <label>Measured Hot RL (bar)</label>
                    <input type="number" id="hot-rl" value="1.85" step="0.01">
                </div>
                <div class="form-group">
                    <label>Measured Hot RR (bar)</label>
                    <input type="number" id="hot-rr" value="1.86" step="0.01">
                </div>
                <button class="btn btn-primary" onclick="analyzeHot()">Analyze & Calculate New Cold</button>  
                <div id="hot-results" class="results hidden">
                    <div class="pressure-grid">
                        <div class="pressure-card">
                            <div class="pressure-label">FL New Cold</div>
                            <div class="pressure-value" id="new-cold-fl">-</div>
                            <div class="pressure-unit">bar</div>
                        </div>
                        <div class="pressure-card">
                            <div class="pressure-label">FR New Cold</div>
                            <div class="pressure-value" id="new-cold-fr">-</div>
                            <div class="pressure-unit">bar</div>
                        </div>
                        <div class="pressure-card">
                            <div class="pressure-label">RL New Cold</div>
                            <div class="pressure-value" id="new-cold-rl">-</div>
                            <div class="pressure-unit">bar</div>
                        </div>
                        <div class="pressure-card">
                            <div class="pressure-label">RR New Cold</div>
                            <div class="pressure-value" id="new-cold-rr">-</div>
                            <div class="pressure-unit">bar</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <script>
        // Constants
        const T_REF_K = 293.15;
        const REF_COLD = 0.05;
        const REF_HOT = 0.08;
        const TEMP_CORRECTION = [
            [-10, 0.30], [-8, 0.25], [-5, 0.15], [-3, 0.10], [-1, 0.03],
            [0, 0.00], [1, -0.03], [3, -0.03], [5, -0.05], [8, -0.08],
            [10, -0.10], [13, -0.13], [15, -0.15], [18, -0.18], [20, -0.20],
            [25, -0.25], [30, -0.30]
        ];
        const OFFSETS = {
            frontToRear: 0.07,
            frontLR: 0.01,
            rearLR: 0.02
        };
        function showTab(tab) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
            document.getElementById(`tab-${tab}`).classList.add('active');
            event.target.classList.add('active');
        }
        function interpolate(x, xs, ys) {
            for (let i = 0; i < xs.length - 1; i++) {
                if (x >= xs[i] && x <= xs[i + 1]) {
                    const t = (x - xs[i]) / (xs[i + 1] - xs[i]);
                    return ys[i] + t * (ys[i + 1] - ys[i]);
                }
            }
            return x < xs[0] ? ys[0] : ys[ys.length - 1];
        }
        function calculatePressureDistribution(pBase) {
            const pFront = pBase + 0.035;
            const pRear = pBase - 0.035;   
            const trackType = document.getElementById('trackType').value;
            let fl, fr, rl, rr;
            if (trackType === 'right') {
                fl = pFront - OFFSETS.frontLR;
                fr = pFront + OFFSETS.frontLR;
                rl = pRear - OFFSETS.rearLR;
                rr = pRear + OFFSETS.rearLR;
            } else if (trackType === 'left') {
                fl = pFront - OFFSETS.frontLR;
                fr = pFront + OFFSETS.frontLR;
                rl = pRear - OFFSETS.rearLR;
                rr = pRear + OFFSETS.rearLR;
            } else {
                fl = fr = pFront;
                rl = rr = pRear;
            }
            return [fl, fr, rl, rr];
        }
        function calculateColdPressures() {
            const pRef = parseFloat(document.getElementById('baselinePressure').value);
            const tAmbient = parseFloat(document.getElementById('ambientTemp').value);
            const tTrack = parseFloat(document.getElementById('trackTemp').value);   
            const tAmbientK = tAmbient + 273.15;
            const baseRatio = tAmbientK / T_REF_K;
            const pBase = pRef * baseRatio;
            const pDistributed = calculatePressureDistribution(pBase);
            const deltaT = tTrack - tAmbient;
            const xs = TEMP_CORRECTION.map(row => row[0]);
            const ys = TEMP_CORRECTION.map(row => row[1]);
            const correction = interpolate(deltaT, xs, ys);
            const sessionAdjust = parseFloat(document.getElementById('sessionType').value);
            const pColdFinal = pDistributed.map(p => (p + correction + sessionAdjust).toFixed(2));
            document.getElementById('result-fl').textContent = pColdFinal[0];
            document.getElementById('result-fr').textContent = pColdFinal[1];
            document.getElementById('result-rl').textContent = pColdFinal[2];
            document.getElementById('result-rr').textContent = pColdFinal[3];
            document.getElementById('viz-fl').textContent = pColdFinal[0];
            document.getElementById('viz-fr').textContent = pColdFinal[1];
            document.getElementById('viz-rl').textContent = pColdFinal[2];
            document.getElementById('viz-rr').textContent = pColdFinal[3];
            document.getElementById('results').classList.remove('hidden');
        }
        function calculateAdjustment() {
            const pCurrent = [
                parseFloat(document.getElementById('current-fl').value),
                parseFloat(document.getElementById('current-fr').value),
                parseFloat(document.getElementById('current-rl').value),
                parseFloat(document.getElementById('current-rr').value)
            ];   
            const tAmbOld = parseFloat(document.getElementById('prev-ambient').value);
            const tTrkOld = parseFloat(document.getElementById('prev-track').value);
            const tAmbNew = parseFloat(document.getElementById('new-ambient').value);
            const tTrkNew = parseFloat(document.getElementById('new-track').value);
            const deltaTOld = tTrkOld - tAmbOld;
            const deltaTNew = tTrkNew - tAmbNew;
            const xs = TEMP_CORRECTION.map(row => row[0]);
            const ys = TEMP_CORRECTION.map(row => row[1]);
            const corrOld = interpolate(deltaTOld, xs, ys);
            const corrNew = interpolate(deltaTNew, xs, ys);
            const corrChange = corrNew - corrOld;
            const pAdjusted = pCurrent.map(p => (p + corrChange).toFixed(2));
            document.getElementById('adjust-fl').textContent = pAdjusted[0];
            document.getElementById('adjust-fr').textContent = pAdjusted[1];
            document.getElementById('adjust-rl').textContent = pAdjusted[2];
            document.getElementById('adjust-rr').textContent = pAdjusted[3];
            document.getElementById('adjust-results').classList.remove('hidden');
        }
        function analyzeHot() {
            const pColdSet = [
                parseFloat(document.getElementById('cold-fl').value),
                parseFloat(document.getElementById('cold-fr').value),
                parseFloat(document.getElementById('cold-rl').value),
                parseFloat(document.getElementById('cold-rr').value)
            ];   
            const pHotMeasured = [
                parseFloat(document.getElementById('hot-fl').value),
                parseFloat(document.getElementById('hot-fr').value),
                parseFloat(document.getElementById('hot-rl').value),
                parseFloat(document.getElementById('hot-rr').value)
            ];
            const targetHot = parseFloat(document.getElementById('target-hot').value);
            const bleed = pHotMeasured.map(p => p - targetHot);
            const bleedScaled = bleed.map(b => (REF_COLD * b) / REF_HOT);
            const pColdNew = pColdSet.map((p, i) => (p - bleedScaled[i]).toFixed(2));
            document.getElementById('new-cold-fl').textContent = pColdNew[0];
            document.getElementById('new-cold-fr').textContent = pColdNew[1];
            document.getElementById('new-cold-rl').textContent = pColdNew[2];
            document.getElementById('new-cold-rr').textContent = pColdNew[3];
            document.getElementById('hot-results').classList.remove('hidden');
        }
        // Install prompt
        let deferredPrompt;
        window.addEventListener('beforeinstallprompt', (e) => {
            e.preventDefault();
            deferredPrompt = e;
        });
    </script>
</body>
</html>
