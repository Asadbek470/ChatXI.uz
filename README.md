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
    textarea {
      width:100%; height:120px; padding:10px; font-size:15px;
      border-radius:8px; border:1px solid #444; background:#111; color:#fff;
    }
    button { margin-top:10px; padding:10px 15px; border-radius:6px; border:0; cursor:pointer;
      background:#e63946; color:white; font-weight:bold;
    }
    #blocker {
      position:fixed; inset:0; z-index:99999;
      display:none; align-items:center; justify-content:center;
      background:rgba(0,0,0,0.85);
      backdrop-filter: blur(4px);
    }
    .panel {
      background:#111217; padding:28px; border-radius:12px; box-shadow:0 10px 40px rgba(0,0,0,0.6);
      border:1px solid rgba(255,255,255,0.03); text-align:center;
    }
    .panel h1 { color:#e63946; margin-bottom:10px; }
    .panel input {
      padding:10px; margin-top:15px; width:200px;
      border-radius:6px; border:1px solid #444; background:#222; color:white;
    }
    .panel button { margin-top:15px; background:#28a745; }
  </style>
</head>
<body>
  <h2>📝 Напиши заметку</h2>
  <textarea id="note" placeholder="Введите текст..."></textarea><br>
  <button id="saveNote">Сохранить</button>

  <!-- Блокировка -->
  <div id="blocker">
    <div class="panel">
      <h1>🚫 Доступ временно ограничен</h1>
      <p>Вы нарушили правила: запрещены оскорбления и плохие слова.</p>
      <p><b>Только администратор может разблокировать сайт.</b></p>
      <input type="password" id="adminPass" placeholder="Введите пароль">
      <br>
      <button id="unlockBtn">Разблокировать</button>
    </div>
  </div>

  <script>
    const badWords = ["лох","тупица","плохой","дурак","идиот","асадбек плохой","асадбек лох","мат"]; 
    const adminPassword = "ASADBEKantiban";

    const noteInput = document.getElementById("note");
    const saveBtn = document.getElementById("saveNote");
    const blocker = document.getElementById("blocker");
    const unlockBtn = document.getElementById("unlockBtn");
    const adminPass = document.getElementById("adminPass");

    // Проверяем блокировку при загрузке
    if (localStorage.getItem("blocked") === "true") {
      blocker.style.display = "flex";
    }

    // Загружаем сохранённую заметку при входе
    if (localStorage.getItem("savedNote")) {
      noteInput.value = localStorage.getItem("savedNote");
    }

    // Сохраняем заметку по кнопке
    saveBtn.addEventListener("click", () => {
      let text = noteInput.value.toLowerCase();

      for (let word of badWords) {
        if (text.includes(word)) {
          localStorage.setItem("blocked", "true");
          blocker.style.display = "flex";
          return;
        }
      }

      localStorage.setItem("savedNote", noteInput.value); // сохраняем в браузере
      alert("Заметка сохранена ✅ (она останется даже после перезагрузки)");
    });

    // Разблокировка
    unlockBtn.addEventListener("click", () => {
      if (adminPass.value === adminPassword) {
        localStorage.setItem("blocked", "false");
        blocker.style.display = "none";
        adminPass.value = "";
        alert("✅ Сайт успешно разблокирован (администратор вошёл)");
      } else {
        alert("❌ Неверный пароль!");
      }
    });
  </script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/11.11.0/math.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(to bottom right, #dbe9f4, #f7f9fc);
      margin: 0;
      padding: 0;
    }

    .container {
      max-width: 600px;
      margin: auto;
      padding: 30px;
      background-color: white;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
      border-radius: 12px;
      margin-top: 50px;
    }

    h2 {
      text-align: center;
      color: #333;
    }

    form {
      display: none;
      margin-top: 20px;
    }

    label {
      display: block;
      margin: 10px 0 5px;
    }

    input[type="text"], input[type="password"] {
      width: 100%;
      padding: 10px;
      margin-bottom: 15px;
      border-radius: 6px;
      border: 1px solid #ccc;
    }

    button {
      padding: 10px 20px;
      background-color: #0099cc;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 16px;
    }

    button:hover {
      background-color: #007799;
    }

    .links {
      text-align: center;
      margin-top: 20px;
    }

    .links a {
      margin: 0 10px;
      color: #0099cc;
      cursor: pointer;
      text-decoration: underline;
    }

    #chatBox {
      height: 300px;
      overflow-y: auto;
      border: 1px solid #ccc;
      padding: 10px;
      margin-bottom: 10px;
    }

    .user { color: #000099; margin-bottom: 10px; }
    .bot { color: #009900; margin-bottom: 10px; }

    #chat {
      display: none;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>Добро пожаловать в XIAI</h2>

    <div class="links">
      <a onclick="showForm('login')">Вход</a> |
      <a onclick="showForm('register')">Регистрация</a>
    </div>

    <!-- Вход -->
    <form id="loginForm">
      <label>Имя:</label>
      <input type="text" id="loginName" required>
      <label>Пароль:</label>
      <input type="password" id="loginPass" required>
      <button type="button" onclick="login()">Войти</button>
    </form>

    <!-- Регистрация -->
    <form id="registerForm">
      <label>Имя:</label>
      <input type="text" id="regName" required>
      <label>Пароль:</label>
      <input type="password" id="regPass" required>
      <label><input type="checkbox" id="agree"> Я согласен с условиями</label><br><br>
      <button type="button" onclick="register()">Зарегистрироваться</button>
    </form>

    <!-- Чат -->
    <div id="chat">
      <h3>Чат XIAI</h3>
      <div id="chatBox"></div>
      <input type="text" id="userInput" placeholder="Напиши математический вопрос...">
      <button onclick="sendMessage()">Отправить</button>
    </div>
  </div>

  <script>
    function showForm(formId) {
      document.getElementById('loginForm').style.display = 'none';
      document.getElementById('registerForm').style.display = 'none';
      document.getElementById('chat').style.display = 'none';
      document.getElementById(formId + 'Form').style.display = 'block';
    }

    function login() {
      const name = document.getElementById('loginName').value;
      const pass = document.getElementById('loginPass').value;
      if (name && pass) {
        alert("Добро пожаловать, " + name + "!");
        document.getElementById('loginForm').style.display = 'none';
        document.getElementById('chat').style.display = 'block';
      } else {
        alert("Введите имя и пароль.");
      }
    }

    function register() {
      const name = document.getElementById('regName').value;
      const pass = document.getElementById('regPass').value;
      const agree = document.getElementById('agree').checked;
      if (name && pass && agree) {
        alert("Успешная регистрация! Теперь войдите.");
        showForm('login');
      } else {
        alert("Заполните все поля и согласитесь с условиями.");
      }
    }

    const chatBox = document.getElementById('chatBox');

    function sendMessage() {
      const input = document.getElementById('userInput');
      const userText = input.value.trim();
      if (userText === '') return;

      appendMessage('user', userText);
      input.value = '';

      setTimeout(() => {
        const botReply = getBotReply(userText);
        appendMessage('bot', botReply);
      }, 500);
    }

    function appendMessage(sender, text) {
      const msg = document.createElement('div');
      msg.className = sender;
      msg.textContent = sender === 'user' ? "Вы: " + text : "XIAI: " + text;
      chatBox.appendChild(msg);
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    function getBotReply(text) {
      try {
        // Решение уравнений
        if (text.includes('=')) {
          const eq = math.parse(text);
          const simplified = math.simplify(eq);
          const solution = math.solve(text, 'x');
          return `Решение: x = ${solution}`;
        }

        // Просто вычислить выражение
        const result = math.evaluate(text);
        return `Ответ: ${result}`;
      } catch (e) {
        return "Ошибка: Некорректный пример или уравнение.";
      }
    }
  </script>
</body>
</html>

