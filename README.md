<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kalkulyator</title>
<link rel="stylesheet" href="style.css">
</head>
<body>
<div class="calc" id="calc">
  <input type="text" id="display" readonly>
  <div class="buttons">
    <button onclick="add('7')">7</button>
    <button onclick="add('8')">8</button>
    <button onclick="add('9')">9</button>
    <button onclick="add('/')">÷</button>
    <button onclick="add('4')">4</button>
    <button onclick="add('5')">5</button>
    <button onclick="add('6')">6</button>
    <button onclick="add('*')">×</button>
    <button onclick="add('1')">1</button>
    <button onclick="add('2')">2</button>
    <button onclick="add('3')">3</button>
    <button onclick="add('-')">−</button>
    <button onclick="add('0')">0</button>
    <button onclick="add('.')">.</button>
    <button onclick="calculate()">=</button>
    <button onclick="add('+')">+</button>
    <button class="clear" onclick="clearDisplay()">Tozalash</button>
  </div>
</div>

<div id="recorder" class="hidden">
  <h2>Diktofon</h2>
  <div id="status">Tayyor 🎤</div>
  <button id="recordBtn">Yozishni boshlash</button>
  <button id="stopBtn" disabled>To‘xtatish</button>
  <ul id="recordingsList"></ul>
  <button class="clear" onclick="backToCalc()">🔙 Kalkulyatorga qaytish</button>
</div>

<script src="script.js"></script>
</body>
</html>body {
  background: #000;
  color: #fff;
  font-family: sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  margin: 0;
}

.calc {
  background: #111;
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 0 20px #000;
  width: 260px;
}

input {
  width: 100%;
  height: 50px;
  font-size: 22px;
  margin-bottom: 10px;
  text-align: right;
  background: #222;
  color: #0f0;
  border: none;
  border-radius: 8px;
  padding: 10px;
}

button {
  width: 55px;
  height: 55px;
  font-size: 20px;
  margin: 4px;
  border: none;
  border-radius: 10px;
  background: #333;
  color: #fff;
  cursor: pointer;
}

button:hover {
  background: #444;
}

button.clear {
  width: 100%;
  background: #900;
}

.hidden {
  display: none;
}

#recorder {
  text-align: center;
}

#recordBtn, #stopBtn {
  margin: 10px;
  padding: 10px 20px;
  font-size: 18px;
  border-radius: 10px;
}

#status {
  margin: 10px;
  font-size: 18px;
}

.recording-dot {
  width: 10px;
  height: 10px;
  background: red;
  border-radius: 50%;
  display: inline-block;
  animation: blink 1s infinite;
}

@keyframes blink {
  0% {opacity: 1;}
  50% {opacity: 0;}
  100% {opacity: 1;}
}let display = document.getElementById('display');
let calc = document.getElementById('calc');
let recorderDiv = document.getElementById('recorder');
let recordingsList = document.getElementById('recordingsList');
let sirliKod = localStorage.getItem('sirliKod');

if (!sirliKod) {
  sirliKod = prompt("Sirli kodni o'rnating:");
  localStorage.setItem('sirliKod', sirliKod);
}

function add(value) {
  display.value += value;
}

function clearDisplay() {
  display.value = '';
}

function calculate() {
  if (display.value === sirliKod) {
    openRecorder();
    return;
  }
  try {
    display.value = eval(display.value);
  } catch {
    display.value = "Xato!";
  }
}

function openRecorder() {
  calc.classList.add('hidden');
  recorderDiv.classList.remove('hidden');
}

function backToCalc() {
  recorderDiv.classList.add('hidden');
  calc.classList.remove('hidden');
}

let mediaRecorder;
let audioChunks = [];

const recordBtn = document.getElementById('recordBtn');
const stopBtn = document.getElementById('stopBtn');
const statusDiv = document.getElementById('status');

recordBtn.onclick = async () => {
  const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
  mediaRecorder = new MediaRecorder(stream);
  audioChunks = [];
  mediaRecorder.ondataavailable = e => audioChunks.push(e.data);
  mediaRecorder.onstop = saveRecording;
  mediaRecorder.start();
  statusDiv.innerHTML = 'Yozilmoqda <span class="recording-dot"></span>';
  recordBtn.disabled = true;
  stopBtn.disabled = false;
};

stopBtn.onclick = () => {
  mediaRecorder.stop();
  recordBtn.disabled = false;
  stopBtn.disabled = true;
  statusDiv.textContent = 'Tayyor 🎤';
};

function saveRecording() {
  const blob = new Blob(audioChunks, { type: 'audio/webm' });
  const url = URL.createObjectURL(blob);
  const dateName = new Date().toISOString().replace(/[:.]/g, '-');
  const li = document.createElement('li');
  const audio = document.createElement('audio');
  const delBtn = document.createElement('button');

  audio.controls = true;
  audio.src = url;
  delBtn.textContent = '🗑️';
  delBtn.onclick = () => {
    recordingsList.removeChild(li);
    saveAll();
  };

  li.textContent = `${dateName}: `;
  li.appendChild(audio);
  li.appendChild(delBtn);
  recordingsList.appendChild(li);

  saveAll();
}

function saveAll() {
  const data = [];
  recordingsList.querySelectorAll('audio').forEach(a => {
    data.push(a.src);
  });
  localStorage.setItem('audioList', JSON.stringify(data));
}

function loadAll() {
  const data = JSON.parse(localStorage.getItem('audioList') || '[]');
  data.forEach(url => {
    const li = document.createElement('li');
    const audio = document.createElement('audio');
    const delBtn = document.createElement('button');
    audio.controls = true;
    audio.src = url;
    delBtn.textContent = '🗑️';
    delBtn.onclick = () => {
      recordingsList.removeChild(li);
      saveAll();
    };
    li.appendChild(audio);
    li.appendChild(delBtn);
    recordingsList.appendChild(li);
  });
}

loadAll();
