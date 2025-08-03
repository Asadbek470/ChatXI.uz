# OpenXI uz
OpenXI
меня создал Abdumalikov Asadbek
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>XIAI — Математический помощник</title>
  <style>
    body {
      background: linear-gradient(to right, #a1c4fd, #c2e9fb);
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 0;
      padding: 0;
    }

    .container {
      max-width: 600px;
      margin: auto;
      padding: 40px 20px;
      background: white;
      margin-top: 40px;
      border-radius: 15px;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
    }

    h1 {
      color: #333;
    }

    .form-section {
      margin-bottom: 30px;
    }

    input[type="text"], input[type="password"] {
      padding: 10px;
      margin: 5px;
      width: 80%;
      border-radius: 8px;
      border: 1px solid #ccc;
    }

    button {
      padding: 10px 20px;
      margin: 10px;
      border: none;
      background-color: #4285f4;
      color: white;
      border-radius: 8px;
      cursor: pointer;
    }

    button:hover {
      background-color: #3367d6;
    }

    .chat-container {
      display: none;
      text-align: left;
    }

    .messages {
      height: 300px;
      overflow-y: auto;
      border: 1px solid #ccc;
      padding: 10px;
      background: #f8f8f8;
      border-radius: 10px;
      margin-bottom: 10px;
    }

    .user { color: #000099; }
    .bot  { color: #009900; }
  </style>
</head>
<body>

  <div class="container">
    <h1>Добро пожаловать в XIAI</h1>

    <!-- Регистрация -->
    <div id="authSection">
      <div class="form-section">
        <h3>Регистрация</h3>
        <input type="text" id="regName" placeholder="Введите имя"><br>
        <input type="password" id="regPass" placeholder="Придумайте пароль"><br>
        <button onclick="register()">Зарегистрироваться</button>
      </div>

      <!-- Вход -->
      <div class="form-section">
        <h3>Вход</h3>
        <input type="text" id="loginName" placeholder="Ваше имя"><br>
        <input type="password" id="loginPass" placeholder="Ваш пароль"><br>
        <button onclick="login()">Войти</button>
      </div>
    </div>

    <!-- Чат XIAI -->
    <div class="chat-container" id="chatContainer">
      <h2>Чат с XIAI</h2>
      <div class="messages" id="chatBox"></div>
      <input type="text" id="userInput" placeholder="Введите математический пример..." />
      <button onclick="sendMessage()">Отправить</button>
    </div>
  </div>

  <script>
    // Простейшее хранилище пользователя
    let registeredUser = { name: '', password: '' };

    function register() {
      const name = document.getElementById('regName').value.trim();
      const pass = document.getElementById('regPass').value.trim();
      if (!name || !pass) return alert("Введите имя и пароль!");
      registeredUser.name = name;
      registeredUser.password = pass;
      alert("Вы успешно зарегистрированы!");
    }

    function login() {
      const name = document.getElementById('loginName').value.trim();
      const pass = document.getElementById('loginPass').value.trim();
      if (name === registeredUser.name && pass === registeredUser.password) {
        document.getElementById('authSection').style.display = 'none';
        document.getElementById('chatContainer').style.display = 'block';
      } else {
        alert("Неверное имя или пароль!");
      }
    }

    function sendMessage() {
      const input = document.getElementById('userInput');
      const userText = input.value.trim();
      if (!userText) return;

      appendMessage('user', userText);
      input.value = '';

      // Обработка математических выражений
      try {
        const expression = userText.replace(/√/g, 'Math.sqrt'); // заменяем √ на Math.sqrt
        const result = eval(expression);
        appendMessage('bot', `${userText} = ${result}`);
      } catch {
        appendMessage('bot', "Не удалось понять выражение 😓");
      }
    }

    function appendMessage(sender, text) {
      const box = document.getElementById('chatBox');
      const msg = document.createElement('div');
      msg.className = sender;
      msg.textContent = (sender === 'user' ? "Вы: " : "XIAI: ") + text;
      box.appendChild(msg);
      box.scrollTop = box.scrollHeight;
    }
  </script>

</body>
</html>

