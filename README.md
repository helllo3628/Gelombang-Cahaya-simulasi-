<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulasi Gelombang Cahaya</title>

    <style>
        body {
            margin: 0;
            background: black;
            color: white;
            font-family: Arial, sans-serif;
            text-align: center;
        }

        canvas {
            display: block;
            margin: auto;
            background: #000;
        }

        .controls {
            margin: 20px;
        }

        label {
            display: block;
            margin: 10px;
        }
    </style>
</head>

<body>

<h2>Simulasi Gelombang Cahaya</h2>

<canvas id="waveCanvas" width="800" height="300"></canvas>

<div class="controls">
    <label>
        Amplitudo: <span id="ampVal">50</span>
        <input type="range" id="amplitude" min="10" max="100" value="50">
    </label>

    <label>
        Frekuensi: <span id="freqVal">0.02</span>
        <input type="range" id="frequency" min="0.01" max="0.1" step="0.01" value="0.02">
    </label>

    <label>
        Kecepatan: <span id="speedVal">0.05</span>
        <input type="range" id="speed" min="0.01" max="0.2" step="0.01" value="0.05">
    </label>
</div>

<script>
const canvas = document.getElementById("waveCanvas");
const ctx = canvas.getContext("2d");

let amplitude = 50;
let frequency = 0.02;
let speed = 0.05;
let phase = 0;

const ampSlider = document.getElementById("amplitude");
const freqSlider = document.getElementById("frequency");
const speedSlider = document.getElementById("speed");

const ampVal = document.getElementById("ampVal");
const freqVal = document.getElementById("freqVal");
const speedVal = document.getElementById("speedVal");

ampSlider.oninput = () => {
    amplitude = ampSlider.value;
    ampVal.textContent = amplitude;
};

freqSlider.oninput = () => {
    frequency = freqSlider.value;
    freqVal.textContent = frequency;
};

speedSlider.oninput = () => {
    speed = speedSlider.value;
    speedVal.textContent = speed;
};

function drawWave() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    ctx.beginPath();
    ctx.moveTo(0, canvas.height / 2);

    for (let x = 0; x < canvas.width; x++) {
        let y = canvas.height / 2 + amplitude * Math.sin(frequency * x + phase);
        ctx.lineTo(x, y);
    }

    ctx.strokeStyle = "cyan";
    ctx.lineWidth = 2;
    ctx.stroke();

    phase += speed;
    requestAnimationFrame(drawWave);
}

drawWave();
</script>

</body>
</html>
