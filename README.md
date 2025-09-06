# OpenXI uz
OpenXI4
меня создал Abdumalikov Asadbek
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>XIAI — Математический Ассистент</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body { font-family: Arial, sans-serif; background:#0b0c10; color:#e6e6e6; margin:0; padding:20px; }
    textarea { width:100%; height:120px; padding:10px; font-size:15px; border-radius:8px; border:1px solid #444; background:#111; color:#fff; }
    button { margin-top:10px; padding:10px 15px; border-radius:6px; border:0; cursor:pointer; background:#e63946; color:white; font-weight:bold; }
    #blocker { position:fixed; inset:0; z-index:99999; display:none; align-items:center; justify-content:center; background:rgba(0,0,0,0.9); backdrop-filter: blur(4px); }
    .panel { background:#111217; padding:28px; border-radius:12px; box-shadow:0 10px 40px rgba(0,0,0,0.6); border:1px solid rgba(255,255,255,0.03); text-align:center; }
    .panel h1 { color:#e63946; margin-bottom:10px; }
    .panel input { padding:10px; margin-top:15px; width:200px; border-radius:6px; border:1px solid #444; background:#222; color:white; }
    .panel button { margin-top:15px; background:#28a745; }
    img.logo { display:block; margin:20px auto; max-width:200px; border-radius:12px; }
    #robotWatcher { background: #111; color: #e63946; padding: 10px; text-align: center; font-weight: bold; font-size: 18px; border-bottom: 2px solid #e63946; position: sticky; top: 0; z-index: 10000; }
    .sirena { display: inline-block; animation: blink 1s infinite; }
    @keyframes blink { 0%,50%,100%{opacity:1;} 25%,75%{opacity:0;} }
    .container { max-width: 600px; margin: auto; padding: 30px; background-color: white; color: black; box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1); border-radius: 12px; margin-top: 30px; }
    #chatBox { height: 250px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; margin-bottom: 10px; white-space: pre-line; }
    .user { color: #000099; margin-bottom: 10px; }
    .bot { color: #009900; margin-bottom: 10px; }
  </style>
</head>
<body>
  <div id="robotWatcher">
    👁 За вами следит <b>онлайн-робот-администратор</b>
    <span class="sirena">🚨</span><span class="sirena">🚨</span><span class="sirena">🚨</span>
  </div>

  <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQC0m3-hAkL0qx5VxSK_dxymtGUWrZqdDMG_Q&s" alt="OpenXi Logo" class="logo">

  <h2>📝 Напиши заметку</h2>
  <textarea id="note" placeholder="Введите текст..."></textarea><br>
  <button id="saveNote">Сохранить</button>

  <div class="container">
    <h3>Чат XIAI</h3>
    <div id="chatBox"></div>
    <input type="text" id="userInput" placeholder="Напиши пример...">
    <button onclick="sendMessage()">Отправить</button>
    <button onclick="startVoice()">🎤</button>
  </div>

  <audio id="alarmSound" src="https://www.soundjay.com/misc/sounds/police-siren-01.mp3" preload="auto"></audio>

  <div id="blocker">
    <div class="panel">
      <h1>🚫 Доступ заблокирован</h1>
      <p>Вы нарушили правила (мат, спам или хакерская атака).</p>
      <p><b>Только администратор может разблокировать сайт.</b></p>
      <input type="password" id="adminPass" placeholder="Введите пароль">
      <br>
      <button id="unlockBtn">Разблокировать</button>
    </div>
  </div>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/11.11.0/math.min.js"></script>
  <script>
    const badWords = ["лох","тупица","дурак","идиот","сука","блядь","ебать","хуй","пидор","gandon","mudak","blyad","suka","ebat","hui","pidor","eblan","yebat","yeblan","pizda","pizdets","blyadstvo","svoloch","svolochy","durak","duraki","idiot","idioty","mrd","mrdka","mrdki","blyad","blyadi","blyadki","eblan","eblani","eblanam","eblanov","pizda","pizdets","pizdami","pizdetsami","lox","suka"];
    const hackPatterns = ["<script", "javascript:", "onerror", "onload","select *","drop table","insert into","delete from","union all","--","/*","*/","or 1=1"];
    const adminPassword = "ASADBEKantiban";

    const noteInput = document.getElementById("note");
    const saveBtn = document.getElementById("saveNote");
    const blocker = document.getElementById("blocker");
    const unlockBtn = document.getElementById("unlockBtn");
    const adminPass = document.getElementById("adminPass");
    const alarm = document.getElementById("alarmSound");
    const chatBox = document.getElementById("chatBox");
    const userInput = document.getElementById("userInput");
    const synth = window.speechSynthesis;

    function blockUser(reason="Нарушение правил") {
      localStorage.setItem("blocked","true");
      blocker.style.display = "flex";
      try { alarm.play(); } catch(e) {}
      console.warn("Пользователь заблокирован:", reason);
    }

    if (localStorage.getItem("blocked") === "true") blocker.style.display = "flex";
    if (localStorage.getItem("savedNote")) noteInput.value = localStorage.getItem("savedNote");

    saveBtn.addEventListener("click", () => {
      let text = noteInput.value.toLowerCase();
      for (let word of badWords) if (text.includes(word)) return blockUser("Мат в заметке");
      for (let p of hackPatterns) if (text.includes(p)) return blockUser("Хакерская атака");
      localStorage.setItem("savedNote", noteInput.value);
      alert("Заметка сохранена ✅");
    });

    unlockBtn.addEventListener("click", () => {
      if (adminPass.value === adminPassword) {
        localStorage.setItem("blocked","false");
        blocker.style.display = "none";
        adminPass.value = "";
        alert("✅ Сайт разблокирован (админ вошёл)");
      } else alert("❌ Неверный пароль!");
    });

    let messageLog = [];
    function sendMessage() {
      const text = userInput.value.trim();
      if (!text) return;

      let lower = text.toLowerCase();
      for (let word of badWords) if (lower.includes(word)) return blockUser("Мат в чате");
      for (let p of hackPatterns) if (lower.includes(p)) return blockUser("Хакерская атака");

      let now = Date.now();
      messageLog.push(now);
      messageLog = messageLog.filter(t => now - t < 10000);
      if (messageLog.length > 5) return blockUser("Спам атака");

      appendMessage("user", text);
      userInput.value = "";

      setTimeout(() => {
        const reply = getBotReply(text);
        appendMessage("bot", reply);
        speak(reply);
      }, 400);
    }

    function appendMessage(sender, text) {
      const msg = document.createElement("div");
      msg.className = sender;
      msg.textContent = (sender === "user" ? "Вы: " : "XIAI: ") + text;
      chatBox.appendChild(msg);
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    function multiplicationTable(n) {
      n = parseInt(n);
      if (isNaN(n)) return "Ошибка: некорректное число.";
      let table = "";
      for (let i = 1; i <= 100; i++) {
        table += `${n} × ${i} = ${n*i}\n`;
      }
      return table;
    }

    function evaluateExpression(expr) {
      try {
        // BigInt для целых чисел
        let exprBig = expr.replace(/\d+/g, m => BigInt(m));
        const result = math.evaluate(exprBig.toString());
        return "Ответ: " + result.toString();
      } catch(e) {
        try {
          return "Ответ: " + math.evaluate(expr);
        } catch(err) {
          return solveTextTask(expr);
        }
      }
    }

    function getBotReply(text) {
      text = text.trim();

      let match = text.match(/таблица умножения (\d+)/i);
      if (match) return multiplicationTable(match[1]);

      return evaluateExpression(text);
    }

    function solveTextTask(text) {
      text = text.toLowerCase();
      const words = text.replace(/\d+/g, "").trim();
      const numbers = text.match(/\d+/g);

      if (words.length > 3 && numbers && numbers.length >= 1) {
        const total = numbers.reduce((a,b) => a + parseInt(b), 0);
        return `Ответ (текстовая задача): ${total}`;
      }
      return "Ошибка: некорректный пример.";
    }

    function speak(text) {
      if (!synth) return;
      const utter = new SpeechSynthesisUtterance(text);
      utter.lang = "ru-RU";
      synth.speak(utter);
    }

    document.addEventListener("keydown", (e) => {
      if (e.key === "F12" || (e.ctrlKey && e.shiftKey && (e.key === "I" || e.key === "J"))) {
        e.preventDefault();
        blockUser("Попытка открыть DevTools");
      }
    });

    const Recognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    let recognizer;
    if (Recognition) { recognizer = new Recognition(); recognizer.lang = "ru-RU"; }

    function startVoice() {
      if (!recognizer) { alert("Ваш браузер не поддерживает голосовой ввод."); return; }
      recognizer.start();
      recognizer.onresult = (event) => {
        const text = event.results[0][0].transcript;
        userInput.value = text;
        sendMessage();
      };
    }
  </script>
</body>
</html>

