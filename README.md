# New-project-
It is a new project  i have  done by my own idea and  it could  like  real life  project 
It will  be  working  so actively and  everything is  involving  by ai tools 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Integrated ALPR & Dynamic Traffic Signal Controller</title>
    <!-- TensorFlow.js & COCO-SSD -->
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/coco-ssd"></script>
    <style>
        :root {
            --bg-color: #0f172a;
            --card-bg: #1e293b;
            --accent-color: #38bdf8;
            --text-color: #f8fafc;
            --danger-color: #ef4444;
            --warning-color: #f59e0b;
            --success-color: #22c55e;
            --inactive-light: #334155;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        h1 {
            color: var(--accent-color);
            margin-bottom: 15px;
            text-align: center;
        }

        .status-bar {
            margin-bottom: 15px;
            font-weight: bold;
            color: var(--warning-color);
            font-size: 0.95rem;
        }

        .controls {
            background-color: var(--card-bg);
            padding: 15px 25px;
            border-radius: 10px;
            margin-bottom: 20px;
            display: flex;
            gap: 15px;
            align-items: center;
            flex-wrap: wrap;
            justify-content: center;
            width: 100%;
            max-width: 900px;
            border: 1px solid #334155;
        }

        .control-group {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        select, button {
            padding: 8px 14px;
            border-radius: 6px;
            border: 1px solid var(--inactive-light);
            font-size: 0.9rem;
            cursor: pointer;
            outline: none;
        }

        select {
            background-color: var(--bg-color);
            color: #fff;
            max-width: 250px;
        }

        button {
            background-color: #0284c7;
            color: #fff;
            font-weight: bold;
            transition: background 0.2s;
            border: none;
        }

        button:hover { background-color: #0369a1; }
        .btn-capture { background-color: #10b981; }
        .btn-capture:hover { background-color: #059669; }

        .main-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            width: 100%;
            max-width: 900px;
        }

        .video-container {
            position: relative;
            width: 100%;
            background: #000;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.5);
            margin-bottom: 20px;
        }

        video, canvas {
            width: 100%;
            height: auto;
            display: block;
        }

        canvas {
            position: absolute;
            top: 0;
            left: 0;
        }

        .dashboard {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            width: 100%;
        }

        @media (max-width: 768px) {
            .dashboard { grid-template-columns: 1fr; }
        }

        .card {
            background-color: var(--card-bg);
            padding: 15px;
            border-radius: 10px;
            border: 1px solid #334155;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
        }

        .card h3 {
            color: var(--accent-color);
            font-size: 1rem;
            margin-bottom: 10px;
            border-bottom: 1px solid #334155;
            padding-bottom: 5px;
        }

        .count-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 8px;
            margin-top: 10px;
        }

        .count-box {
            background-color: var(--bg-color);
            padding: 8px;
            border-radius: 6px;
            font-size: 0.8rem;
        }

        .count-box strong {
            color: var(--accent-color);
            font-size: 0.9rem;
        }

        .object-breakdown {
            margin-top: 4px;
            color: #cbd5e1;
            font-size: 0.72rem;
            line-height: 1.2;
        }

        #activeDirection {
            font-size: 1.1rem;
            font-weight: bold;
            text-align: center;
            margin-bottom: 5px;
        }

        .timer-display {
            font-size: 2.5rem;
            font-weight: bold;
            text-align: center;
            color: var(--warning-color);
            font-family: monospace;
        }

        .signals-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-top: 10px;
        }

        .signal-box {
            background-color: var(--bg-color);
            padding: 8px;
            border-radius: 6px;
            text-align: center;
            font-size: 0.85rem;
        }

        .signal {
            display: flex;
            justify-content: center;
            gap: 6px;
            margin-top: 5px;
        }

        .signal span {
            width: 14px;
            height: 14px;
            border-radius: 50%;
            background-color: var(--inactive-light);
            display: inline-block;
            opacity: 0.3;
        }

        .signal .red.active { background-color: var(--danger-color); box-shadow: 0 0 8px var(--danger-color); opacity: 1; }
        .signal .yellow.active { background-color: var(--warning-color); box-shadow: 0 0 8px var(--warning-color); opacity: 1; }
        .signal .green.active { background-color: var(--success-color); box-shadow: 0 0 8px var(--success-color); opacity: 1; }

        .info-field { margin-bottom: 8px; }
        .info-label { color: #94a3b8; font-size: 0.75rem; font-weight: 600; text-transform: uppercase; }
        .info-value { font-size: 0.95rem; font-weight: bold; color: var(--success-color); margin-top: 2px; }
    </style>
</head>
<body>

    <h1>Smart Traffic & ALPR Controller</h1>
    <div class="status-bar" id="statusText">WebSocket: Initializing...</div>

    <div class="controls">
        <div class="control-group">
            <label for="cameraSelect"><strong>Camera:</strong></label>
            <select id="cameraSelect">
                <option value="environment">Back Camera (Environment)</option>
                <option value="user">Front Camera (User)</option>
            </select>
        </div>

        <div class="control-group">
            <label for="laneSelect"><strong>Lane Mode:</strong></label>
            <select id="laneSelect">
                <option value="ALL">ALL LANES (Auto Cycle)</option>
                <option value="NORTH">NORTH LANE ONLY</option>
                <option value="SOUTH">SOUTH LANE ONLY</option>
                <option value="EAST">EAST LANE ONLY</option>
                <option value="WEST">WEST LANE ONLY</option>
            </select>
        </div>

        <button onclick="startWebcam()">Start Camera</button>
        <button class="btn-capture" onclick="captureAndSavePhoto()">📸 Save Photo to Gallery</button>
        <button onclick="startSignalSystem()">Start AI Signals</button>
    </div>

    <div class="main-container">
        <!-- Live Camera & Bounding Box Feed -->
        <div class="video-container">
            <video id="videoFeed" autoplay playsinline muted></video>
            <canvas id="detectionCanvas"></canvas>
        </div>

        <!-- System Dashboard -->
        <div class="dashboard">
            <!-- 1. Live Quad-Lane Object Counter -->
            <div class="card">
                <h3>Vehicle Counts</h3>
                <p style="font-size: 0.85rem;">Total Objects: <strong id="total" style="color: var(--accent-color); font-size: 1.1rem;">0</strong></p>
                <div class="count-grid">
                    <div class="count-box">
                        <strong>North: <span id="cntNorth">0</span></strong>
                        <div class="object-breakdown" id="detailsNorth">Cars: 0 | Buses: 0 | Trucks: 0 | Bikes: 0 | Persons: 0</div>
                    </div>
                    <div class="count-box">
                        <strong>South: <span id="cntSouth">0</span></strong>
                        <div class="object-breakdown" id="detailsSouth">Cars: 0 | Buses: 0 | Trucks: 0 | Bikes: 0 | Persons: 0</div>
                    </div>
                    <div class="count-box">
                        <strong>East: <span id="cntEast">0</span></strong>
                        <div class="object-breakdown" id="detailsEast">Cars: 0 | Buses: 0 | Trucks: 0 | Bikes: 0 | Persons: 0</div>
                    </div>
                    <div class="count-box">
                        <strong>West: <span id="cntWest">0</span></strong>
                        <div class="object-breakdown" id="detailsWest">Cars: 0 | Buses: 0 | Trucks: 0 | Bikes: 0 | Persons: 0</div>
                    </div>
                </div>
            </div>

            <!-- 2. Dynamic Signal Controller -->
            <div class="card">
                <h3>Intersection Signal</h3>
                <div id="activeDirection" style="color: var(--success-color);">READY</div>
                <div class="timer-display" id="signalTimer">00</div>

                <div class="signals-grid">
                    <div class="signal-box">
                        <strong>North</strong>
                        <div class="signal">
                            <span id="northRed" class="red"></span>
                            <span id="northYellow" class="yellow"></span>
                            <span id="northGreen" class="green"></span>
                        </div>
                    </div>
                    <div class="signal-box">
                        <strong>South</strong>
                        <div class="signal">
                            <span id="southRed" class="red"></span>
                            <span id="southYellow" class="yellow"></span>
                            <span id="southGreen" class="green"></span>
                        </div>
                    </div>
                    <div class="signal-box">
                        <strong>East</strong>
                        <div class="signal">
                            <span id="eastRed" class="red"></span>
                            <span id="eastYellow" class="yellow"></span>
                            <span id="eastGreen" class="green"></span>
                        </div>
                    </div>
                    <div class="signal-box">
                        <strong>West</strong>
                        <div class="signal">
                            <span id="westRed" class="red"></span>
                            <span id="westYellow" class="yellow"></span>
                            <span id="westGreen" class="green"></span>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 3. ALPR Backend Readout -->
            <div class="card">
                <h3>ALPR Plate Scan</h3>
                <div class="info-field">
                    <div class="info-label">Detected Plate</div>
                    <div class="info-value" id="dispPlate">---</div>
                </div>
                <div class="info-field">
                    <div class="info-label">Registered Owner</div>
                    <div class="info-value" style="color:#fff;" id="dispOwner">---</div>
                </div>
                <div class="info-field">
                    <div class="info-label">Registered Address</div>
                    <div class="info-value" style="color:var(--accent-color);" id="dispAddress">---</div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Global State Variables
        let model = null;
        let signalInterval = null;
        let currentStream = null;
        let animationFrameId = null;
        let ws = null;
        let isWsProcessing = false;

        let timer = 0;
        let direction = 0; // 0 = N/S, 1 = E/W for ALL mode
        let currentState = "GREEN"; // States: "GREEN", "YELLOW", "STOP"
        let backendDetections = [];

        const CLEARANCE_STOP_TIME = 2; // All-red clearance interval in seconds

        // Off-screen canvas for WebSocket frame streaming
        const captureCanvas = document.createElement('canvas');
        const captureCtx = captureCanvas.getContext('2d');

        // Target UI Elements
        const laneSelect = document.getElementById("laneSelect");
        const cameraSelect = document.getElementById("cameraSelect");
        const video = document.getElementById("videoFeed");
        const canvas = document.getElementById("detectionCanvas");
        const ctx = canvas.getContext("2d");
        const statusText = document.getElementById("statusText");

        const targetClasses = ["car", "bus", "truck", "motorcycle", "person"];

        // 1. Load TensorFlow COCO-SSD Model
        cocoSsd.load().then(loadedModel => {
            model = loadedModel;
            console.log("Client-side COCO-SSD Model Loaded successfully.");
        });

        // 2. Initialize WebSocket Connection to Python Backend
        function connectWebSocket() {
            if (ws && (ws.readyState === WebSocket.OPEN || ws.readyState === WebSocket.CONNECTING)) return;

            ws = new WebSocket("ws://127.0.0.1:8000/ws");

            ws.onopen = () => {
                statusText.style.color = "var(--success-color)";
                statusText.innerText = "WebSocket: Connected (ALPR Active)";
                sendControlConfig();
            };

            ws.onmessage = (event) => {
                const data = JSON.parse(event.data);
                const list = data.detections || data.results || [];

                if (list.length > 0) {
                    backendDetections = list;
                    list.forEach(det => {
                        const plateNum = det.plate || det.plate_number || det.text || '';
                        const ownerName = det.owner || det.details?.owner || det.registered_owner || '';
                        const addressVal = det.address || det.details?.address || det.registered_address || '';

                        if (plateNum) {
                            document.getElementById('dispPlate').innerText = plateNum;
                            document.getElementById('dispOwner').innerText = ownerName || 'Unknown Owner';
                            document.getElementById('dispAddress').innerText = addressVal || 'N/A';
                        }
                    });
                }
                isWsProcessing = false;
            };

            ws.onerror = () => {
                statusText.style.color = "var(--danger-color)";
                statusText.innerText = "WebSocket Error: ALPR backend offline";
            };

            ws.onclose = () => {
                statusText.style.color = "var(--warning-color)";
                statusText.innerText = "WebSocket Disconnected. Retrying in 3s...";
                setTimeout(connectWebSocket, 3000);
            };
        }

        function sendControlConfig() {
            if (ws && ws.readyState === WebSocket.OPEN) {
                const payload = {
                    type: "config",
                    lane_mode: laneSelect.value
                };
                ws.send(JSON.stringify(payload));
            }
        }

        // 3. Robust Multi-Device Camera Initialization
        async function loadCameraOptions() {
            try {
                const devices = await navigator.mediaDevices.enumerateDevices();
                const videoDevices = devices.filter(device => device.kind === 'videoinput');

                const selectedVal = cameraSelect.value || "environment";
                
                cameraSelect.innerHTML = `
                    <option value="environment">Back Camera (Environment)</option>
                    <option value="user">Front Camera (User)</option>
                `;

                videoDevices.forEach((device, index) => {
                    if (device.deviceId) {
                        const option = document.createElement("option");
                        option.value = device.deviceId;
                        option.text = device.label || `Hardware Camera ${index + 1}`;
                        cameraSelect.appendChild(option);
                    }
                });

                cameraSelect.value = selectedVal;
            } catch (error) {
                console.warn("Camera enumeration error:", error);
            }
        }

        async function startWebcam() {
            connectWebSocket();

            if (currentStream) {
                currentStream.getTracks().forEach(track => track.stop());
                currentStream = null;
            }
            if (animationFrameId) {
                cancelAnimationFrame(animationFrameId);
                animationFrameId = null;
            }

            const selectedValue = cameraSelect.value;
            let constraintWaterfall = [];

            if (selectedValue === "user" || selectedValue === "environment") {
                constraintWaterfall = [
                    { video: { facingMode: { exact: selectedValue } } },
                    { video: { facingMode: selectedValue } },
                    { video: true }
                ];
            } else if (selectedValue) {
                constraintWaterfall = [
                    { video: { deviceId: { exact: selectedValue } } },
                    { video: { deviceId: selectedValue } },
                    { video: true }
                ];
            } else {
                constraintWaterfall = [
                    { video: { facingMode: "environment" } },
                    { video: true }
                ];
            }

            let streamSuccess = false;

            for (const constraints of constraintWaterfall) {
                try {
                    currentStream = await navigator.mediaDevices.getUserMedia(constraints);
                    streamSuccess = true;
                    break;
                } catch (err) {
                    console.warn("Attempting next fallback constraint:", constraints, err);
                }
            }

            if (!streamSuccess || !currentStream) {
                alert("Camera Access Denied or Device Unavailable.");
                return;
            }

            video.srcObject = currentStream;

            video.onloadedmetadata = () => {
                video.play();
                canvas.width = video.videoWidth;
                canvas.height = video.videoHeight;
                captureCanvas.width = video.videoWidth;
                captureCanvas.height = video.videoHeight;
                detectFrame();
            };
        }

        // Snapshot Functionality
        async function captureAndSavePhoto() {
            if (!video || video.readyState !== 4) {
                alert("Camera is not active!");
                return;
            }

            const photoCanvas = document.createElement("canvas");
            photoCanvas.width = video.videoWidth;
            photoCanvas.height = video.videoHeight;
            const photoCtx = photoCanvas.getContext("2d");

            photoCtx.drawImage(video, 0, 0, photoCanvas.width, photoCanvas.height);
            photoCtx.drawImage(canvas, 0, 0, photoCanvas.width, photoCanvas.height);

            const timestamp = new Date().toISOString().replace(/[:.]/g, "-");
            const fileName = `Traffic_Snapshot_${timestamp}.png`;

            photoCanvas.toBlob(async (blob) => {
                if (!blob) {
                    alert("Failed to capture image.");
                    return;
                }

                const file = new File([blob], fileName, { type: "image/png" });

                if (navigator.canShare && navigator.canShare({ files: [file] })) {
                    try {
                        await navigator.share({
                            files: [file],
                            title: 'Traffic Camera Snapshot',
                            text: 'Captured ALPR & Traffic Signal Frame'
                        });
                        return;
                    } catch (err) {
                        if (err.name === 'AbortError') return;
                        console.warn("Share API failed, fallback to download:", err);
                    }
                }

                const downloadLink = document.createElement("a");
                downloadLink.download = fileName;
                downloadLink.href = URL.createObjectURL(blob);
                document.body.appendChild(downloadLink);
                downloadLink.click();
                
                document.body.removeChild(downloadLink);
                setTimeout(() => URL.revokeObjectURL(downloadLink.href), 1000);
            }, "image/png");
        }

        // 4. Object Detection Loop
        async function detectFrame() {
            if (ws && ws.readyState === WebSocket.OPEN && !isWsProcessing && video.readyState === 4) {
                isWsProcessing = true;
                captureCtx.drawImage(video, 0, 0);
                captureCanvas.toBlob((blob) => {
                    if (blob && ws.readyState === WebSocket.OPEN) {
                        blob.arrayBuffer().then(buffer => {
                            ws.send(buffer);
                        });
                    }
                }, 'image/jpeg', 0.6);
            }

            if (model && video.readyState === 4) {
                const predictions = await model.detect(video);
                ctx.clearRect(0, 0, canvas.width, canvas.height);

                const counts = {
                    north: { car: 0, bus: 0, truck: 0, motorcycle: 0, person: 0, total: 0 },
                    south: { car: 0, bus: 0, truck: 0, motorcycle: 0, person: 0, total: 0 },
                    east:  { car: 0, bus: 0, truck: 0, motorcycle: 0, person: 0, total: 0 },
                    west:  { car: 0, bus: 0, truck: 0, motorcycle: 0, person: 0, total: 0 }
                };

                const midX = canvas.width / 2;
                const midY = canvas.height / 2;

                // Draw Dividers
                ctx.strokeStyle = "rgba(255, 255, 255, 0.25)";
                ctx.setLineDash([5, 5]);
                ctx.beginPath();
                ctx.moveTo(midX, 0); ctx.lineTo(midX, canvas.height);
                ctx.moveTo(0, midY); ctx.lineTo(canvas.width, midY);
                ctx.stroke();
                ctx.setLineDash([]);

                // Bounding Boxes
                predictions.forEach(prediction => {
                    if (targetClasses.includes(prediction.class) && prediction.score > 0.5) {
                        const [x, y, width, height] = prediction.bbox;
                        const centerX = x + width / 2;
                        const centerY = y + height / 2;
                        const objType = prediction.class;

                        let targetLane = "";
                        if (centerY < midY && centerX < midX) targetLane = "north";
                        else if (centerY >= midY && centerX < midX) targetLane = "south";
                        else if (centerY < midY && centerX >= midX) targetLane = "east";
                        else targetLane = "west";

                        counts[targetLane][objType]++;
                        counts[targetLane].total++;

                        ctx.strokeStyle = objType === "person" ? "#f97316" : "#38bdf8";
                        ctx.lineWidth = 2;
                        ctx.strokeRect(x, y, width, height);

                        ctx.fillStyle = objType === "person" ? "#f97316" : "#38bdf8";
                        ctx.font = "12px Segoe UI";
                        ctx.fillText(`${prediction.class} (${Math.round(prediction.score * 100)}%)`, x, y > 10 ? y - 5 : 10);
                    }
                });

                // Render Plate Bounding Boxes
                backendDetections.forEach(det => {
                    const box = det.bbox || det.box;
                    if (box) {
                        const [bx, by, bw, bh] = box;
                        ctx.strokeStyle = "#22c55e";
                        ctx.lineWidth = 3;
                        ctx.strokeRect(bx, by, bw, bh);

                        const plateStr = det.plate || det.plate_number || det.text || '';
                        if (plateStr) {
                            ctx.fillStyle = "#22c55e";
                            ctx.font = "bold 14px Segoe UI";
                            ctx.fillText(`PLATE: ${plateStr}`, bx, by > 20 ? by - 8 : 20);
                        }
                    }
                });

                // Update UI Counters
                document.getElementById("cntNorth").innerText = counts.north.total;
                document.getElementById("cntSouth").innerText = counts.south.total;
                document.getElementById("cntEast").innerText = counts.east.total;
                document.getElementById("cntWest").innerText = counts.west.total;

                document.getElementById("detailsNorth").innerText = `Cars: ${counts.north.car} | Buses: ${counts.north.bus} | Trucks: ${counts.north.truck} | Bikes: ${counts.north.motorcycle} | Persons: ${counts.north.person}`;
                document.getElementById("detailsSouth").innerText = `Cars: ${counts.south.car} | Buses: ${counts.south.bus} | Trucks: ${counts.south.truck} | Bikes: ${counts.south.motorcycle} | Persons: ${counts.south.person}`;
                document.getElementById("detailsEast").innerText = `Cars: ${counts.east.car} | Buses: ${counts.east.bus} | Trucks: ${counts.east.truck} | Bikes: ${counts.east.motorcycle} | Persons: ${counts.east.person}`;
                document.getElementById("detailsWest").innerText = `Cars: ${counts.west.car} | Buses: ${counts.west.bus} | Trucks: ${counts.west.truck} | Bikes: ${counts.west.motorcycle} | Persons: ${counts.west.person}`;

                updateTotalCount();
            }

            animationFrameId = requestAnimationFrame(detectFrame);
        }

        function updateTotalCount() {
            const filterLane = laneSelect.value;
            const n = parseInt(document.getElementById("cntNorth").innerText) || 0;
            const s = parseInt(document.getElementById("cntSouth").innerText) || 0;
            const e = parseInt(document.getElementById("cntEast").innerText) || 0;
            const w = parseInt(document.getElementById("cntWest").innerText) || 0;

            if (filterLane === "NORTH") document.getElementById("total").innerText = n;
            else if (filterLane === "SOUTH") document.getElementById("total").innerText = s;
            else if (filterLane === "EAST") document.getElementById("total").innerText = e;
            else if (filterLane === "WEST") document.getElementById("total").innerText = w;
            else document.getElementById("total").innerText = n + s + e + w;
        }

        // 5. Dynamic Traffic Signal Controls with Explicit STOP Logic
        function calculateTimeForDirection(dir) {
            const filterLane = laneSelect.value;
            let objectCount = 0;

            const n = parseInt(document.getElementById("cntNorth").innerText) || 0;
            const s = parseInt(document.getElementById("cntSouth").innerText) || 0;
            const e = parseInt(document.getElementById("cntEast").innerText) || 0;
            const w = parseInt(document.getElementById("cntWest").innerText) || 0;

            if (filterLane === "NORTH") objectCount = n;
            else if (filterLane === "SOUTH") objectCount = s;
            else if (filterLane === "EAST") objectCount = e;
            else if (filterLane === "WEST") objectCount = w;
            else objectCount = (dir === 0) ? (n + s) : (e + w);

            if (objectCount >= 15) return 40;
            if (objectCount >= 10) return 30;
            if (objectCount >= 5) return 20;
            return 10;
        }

        function startSignalSystem() {
            clearInterval(signalInterval);
            direction = 0;
            currentState = "GREEN";
            timer = calculateTimeForDirection(direction);
            setDirection(direction);

            signalInterval = setInterval(() => {
                timer--;
                document.getElementById("signalTimer").innerText = String(timer).padStart(2, '0');

                if (currentState === "GREEN" && timer <= 3) {
                    currentState = "YELLOW";
                    setYellowDirection(direction);
                }

                if (timer <= 0) {
                    if (currentState === "YELLOW") {
                        // Switch into explicit Clearance STOP Mode
                        currentState = "STOP";
                        timer = CLEARANCE_STOP_TIME;
                        setStopState();
                    } else if (currentState === "STOP") {
                        // Transition to the Next Direction
                        currentState = "GREEN";
                        if (laneSelect.value === "ALL") {
                            direction = (direction === 0) ? 1 : 0;
                        } else {
                            direction = 0;
                        }
                        timer = calculateTimeForDirection(direction);
                        setDirection(direction);
                    }
                }
            }, 1000);
        }

        function resetAllLights() {
            document.querySelectorAll(".signal span").forEach(light => light.classList.remove("active"));
        }

        // All Red Lights ON - STOP Clearance State
        function setStopState() {
            resetAllLights();
            document.getElementById("northRed").classList.add("active");
            document.getElementById("southRed").classList.add("active");
            document.getElementById("eastRed").classList.add("active");
            document.getElementById("westRed").classList.add("active");

            const heading = document.getElementById("activeDirection");
            heading.innerText = "ALL LANES STOP";
            heading.style.color = "var(--danger-color)";
        }

        function setDirection(dir) {
            resetAllLights();
            const heading = document.getElementById("activeDirection");
            const filterLane = laneSelect.value;

            if (filterLane === "NORTH") {
                document.getElementById("northGreen").classList.add("active");
                ["southRed", "eastRed", "westRed"].forEach(id => document.getElementById(id).classList.add("active"));
                heading.innerText = "NORTH GREEN";
            } else if (filterLane === "SOUTH") {
                document.getElementById("southGreen").classList.add("active");
                ["northRed", "eastRed", "westRed"].forEach(id => document.getElementById(id).classList.add("active"));
                heading.innerText = "SOUTH GREEN";
            } else if (filterLane === "EAST") {
                document.getElementById("eastGreen").classList.add("active");
                ["northRed", "southRed", "westRed"].forEach(id => document.getElementById(id).classList.add("active"));
                heading.innerText = "EAST GREEN";
            } else if (filterLane === "WEST") {
                document.getElementById("westGreen").classList.add("active");
                ["northRed", "southRed", "eastRed"].forEach(id => document.getElementById(id).classList.add("active"));
                heading.innerText = "WEST GREEN";
            } else {
                if (dir === 0) {
                    document.getElementById("northGreen").classList.add("active");
                    document.getElementById("southGreen").classList.add("active");
                    document.getElementById("eastRed").classList.add("active");
                    document.getElementById("westRed").classList.add("active");
                    heading.innerText = "NORTH / SOUTH GREEN";
                } else {
                    document.getElementById("eastGreen").classList.add("active");
                    document.getElementById("westGreen").classList.add("active");
                    document.getElementById("northRed").classList.add("active");
                    document.getElementById("southRed").classList.add("active");
                    heading.innerText = "EAST / WEST GREEN";
                }
            }
            heading.style.color = "var(--success-color)";
        }

        function setYellowDirection(dir) {
            resetAllLights();
            const heading = document.getElementById("activeDirection");
            const filterLane = laneSelect.value;

            if (filterLane === "NORTH") {
                document.getElementById("northYellow").classList.add("active");
                ["southRed", "eastRed", "westRed"].forEach(id => document.getElementById(id).classList.add("active"));
            } else if (filterLane === "SOUTH") {
                document.getElementById("southYellow").classList.add("active");
                ["northRed", "eastRed", "westRed"].forEach(id => document.getElementById(id).classList.add("active"));
            } else if (filterLane === "EAST") {
                document.getElementById("eastYellow").classList.add("active");
                ["northRed", "southRed", "westRed"].forEach(id => document.getElementById(id).classList.add("active"));
            } else if (filterLane === "WEST") {
                document.getElementById("westYellow").classList.add("active");
                ["northRed", "southRed", "eastRed"].forEach(id => document.getElementById(id).classList.add("active"));
            } else {
                if (dir === 0) {
                    document.getElementById("northYellow").classList.add("active");
                    document.getElementById("southYellow").classList.add("active");
                    document.getElementById("eastRed").classList.add("active");
                    document.getElementById("westRed").classList.add("active");
                } else {
                    document.getElementById("eastYellow").classList.add("active");
                    document.getElementById("westYellow").classList.add("active");
                    document.getElementById("northRed").classList.add("active");
                    document.getElementById("southRed").classList.add("active");
                }
            }
            heading.innerText = "PREPARE TO STOP";
            heading.style.color = "var(--warning-color)";
        }

        // Event Listeners
        cameraSelect.addEventListener("change", startWebcam);
        laneSelect.addEventListener("change", () => {
            updateTotalCount();
            sendControlConfig();
            if (signalInterval) {
                startSignalSystem();
            }
        });

        window.addEventListener("DOMContentLoaded", async () => {
            await loadCameraOptions();
            startWebcam();
        });
    </script>
</body>
</html>
