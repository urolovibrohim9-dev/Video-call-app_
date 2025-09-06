# Video-call-app_
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Til tanlash</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h2>Tilni tanlang</h2>
    <select id="langSelect">
      <option value="uz">O'zbek</option>
      <option value="ru">Русский</option>
      <option value="en">English</option>
    </select>
    <button onclick="saveLang()">Davom etish</button>
  </div>
  <script src="script.js"></script>
</body>
</html><!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Ro‘yxatdan o‘tish</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h2>Ma’lumotlaringizni kiriting</h2>
    <input type="text" id="name" placeholder="Ismingiz">
    <input type="text" id="phone" placeholder="Telefon raqamingiz">
    <button onclick="saveUser()">Saqlash</button>
  </div>
  <script src="script.js"></script>
</body>
</html><!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Kontaktlar</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h2>Kontaktlar</h2>
    <ul id="contactList"></ul>
    <input type="text" id="newContactName" placeholder="Kontakt nomi">
    <input type="text" id="newContactPhone" placeholder="Telefon raqami">
    <button onclick="addContact()">Kontakt qo‘shish</button>
  </div>
  <script src="script.js"></script>
</body>
</html><!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Chat</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="chat-header">
    <h3 id="chatWith"></h3>
    <button onclick="startCall()">📹 Video qo‘ng‘iroq</button>
  </div>

  <div id="chatWindow" class="chat-window"></div>

  <div class="chat-input">
    <input type="text" id="messageInput" placeholder="Xabar yozing...">
    <button onclick="sendMessage()">Yubor</button>
  </div>

  <script src="script.js"></script>
</body>
</html><!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Video qo‘ng‘iroq</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="call-container">
    <!-- Qarshi taraf video -->
    <video id="remoteVideo" autoplay playsinline class="remote-video"></video>

    <!-- O'zimningbody {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f2f2f2;
}

.container {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
  text-align: center;
}

input, select, button {
  width: 90%;
  padding: 10px;
  margin: 10px 0;
  border-radius: 6px;
  border: 1px solid #ccc;
}

button {
  background: #007bff;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background: #0056b3;
}

.chat-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background: #007bff;
  color: white;
}

.chat-window {
  height: 300px;
  overflow-y: auto;
  background: #fafafa;
  padding: 10px;
  border: 1px solid #ccc;
}

.chat-input {
  display: flex;
  gap: 5px;
  padding: 10px;
}

.chat-input input {
  flex: 1;
}

.call-container {
  position: relative;
  width: 100%;
  height: 100vh;
  background: black;
  overflow: hidden;
}

.remote-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.local-video {
  position: absolute;
  bottom: 80px;
  right: 10px;
  width: 120px;
  height: 90px;
  border: 2px solid white;
  border// ===== Til tanlash =====
function saveLang() {
  const lang = document.getElementById("langSelect").value;
  localStorage.setItem("lang", lang);
  window.location.href = "register.html";
}

// ===== Ro‘yxatdan o‘tish =====
function saveUser() {
  const name = document.getElementById("name").value;
  const phone = document.getElementById("phone").value;
  if (name && phone) {
    localStorage.setItem("name", name);
    localStorage.setItem("phone", phone);
    window.location.href = "chats.html";
  } else {
    alert("Iltimos, ism va telefon raqam kiriting!");
  }
}

// ===== Kontaktlar =====
function loadContacts() {
  const contactList = document.getElementById("contactList");
  if (!contactList) return;

  const contacts = JSON.parse(localStorage.getItem("contacts")) || [];
  contactList.innerHTML = "";
  contacts.forEach((c, i) => {
    const li = document.createElement("li");
    li.textContent = `${c.name} (${c.phone})`;
    li.onclick = () => {
      localStorage.setItem("chatWith", JSON.stringify(c));
      window.location.href = "chat.html";
    };
    contactList.appendChild(li);
  });
}

function addContact() {
  const name = document.getElementById("newContactName").value;
  const phone = document.getElementById("newContactPhone").value;
  if (name && phone) {
    const contacts = JSON.parse(localStorage.getItem("contacts")) || [];
    contacts.push({ name, phone });
    localStorage.setItem("contacts", JSON.stringify(contacts));
    loadContacts();
  }
}

window.onload = loadContacts;

// ===== Chat =====
function loadChat() {
  const chatWith = JSON.parse(localStorage.getItem("chatWith"));
  if (!chatWith) return;
  document.getElementById("chatWith").textContent = chatWith.name;
}

function sendMessage() {
  const input = document.getElementById("messageInput");
  const text = input.value.trim();
  if (text === "") return;

  const chatWindow = document.getElementById("chatWindow");
  const div = document.createElement("div");
  div.textContent = "Siz: " + text;
  chatWindow.appendChild(div);

  input.value = "";
  chatWindow.scrollTop = chatWindow.scrollHeight;
}

function startCall() {
  window.location.href = "call.html";
}

window.onload = function () {
  loadContacts();
  loadChat();
};

// ===== Video qo‘ng‘iroq =====
let localStream;
let currentCamera = "user"; // old kamera

async function initCall() {
  try {
    localStream = await navigator.mediaDevices.getUserMedia({
      video: { facingMode: currentCamera },
      audio: true
    });

    const localVideo = document.getElementById("localVideo");
    if (localVideo) localVideo.srcObject = localStream;
  } catch (err) {
    alert("Kamera yoki mikrofonga ruxsat berilmadi: " + err);
  }
}

function switchCamera() {
  currentCamera = currentCamera === "user" ? "environment" : "user";
  initCall();
}

function toggleMic
