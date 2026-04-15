
<html>
<head>
<meta charset="UTF-8">
<title>Gelombang + Suara (Fix)</title>

<style>
body {
    margin: 0;
    background: black;
    color: white;
    text-align: center;
}
canvas {
    display: block;
    margin: auto;
}
.controls {
    margin: 20px;
}
</style>
</head>

<body>

<h2>Simulasi Gelombang + Suara</h2>

<canvas id="waveCanvas" width="800" height="300"></canvas>

<div class="controls">
    <button id="soundBtn">NYALAKAN SUARA</button>
    <br><br>

    Amplitudo:
    <input type="range" id="amplitude" min="10" max="100" value="50">

    <br>

    Frekuensi (Hz):
    <input type="range" id="frequency" min="100" max="1000" value="440">

    <br>

    Kecepatan:
    <input type="range" id="speed" min="0.01" max="0.2" step="0.01" value="0.05">
</div>

<script>
const canvas = document.getElementById("waveCanvas");
const ctx = canvas.getContext("2d");

let amplitude = 50;
let frequencyVisual = 0.02;
let speed = 0.05;
let phase = 0;

// AUDIO
let audioCtx = null;
let oscillator = null;
let gainNode = null;
let isPlaying = false;

const btn = document.getElementById("soundBtn");

btn.onclick = async () => {
    if (!audioCtx) {
        audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    }

    if (audioCtx.state === "suspended") {
        await audioCtx.resume();
    }

    if (!isPlaying) {
        oscillator = audioCtx.createOscillator();
        gainNode = audioCtx.createGain();

        oscillator.type = "sine";
        oscillator.frequency.value = 440;

        gainNode.gain.value = 0.1;

        oscillator.connect(gainNode);
        gainNode.connect(audioCtx.destination);

        oscillator.start();

        isPlaying = true;
        btn.textContent = "MATIKAN SUARA";
    } else {
        oscillator.stop();
        isPlaying = false;
        btn.textContent = "NYALAKAN SUARA";
    }
};

// SLIDER
const ampSlider = document.getElementById("amplitude");
const freqSlider = document.getElementById("frequency");
const speedSlider = document.getElementById("speed");

ampSlider.oninput = () => {
    amplitude = ampSlider.value;
    if (gainNode) gainNode.gain.value = amplitude / 500;
};

freqSlider.oninput = () => {
    let f = freqSlider.value;

    frequencyVisual = f / 10000;

    if (oscillator) oscillator.frequency.value = f;
};

speedSlider.oninput = () => {
    speed = speedSlider.value;
};

// ANIMASI
function drawWave() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    ctx.beginPath();
    ctx.moveTo(0, canvas.height / 2);

    for (let x = 0; x < canvas.width; x++) {
        let y = canvas.height / 2 + amplitude * Math.sin(frequencyVisual * x + phase);
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
