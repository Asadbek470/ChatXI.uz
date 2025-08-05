# OpenXI uz
OpenXI
меня создал Abdumalikov Asadbek
<script>
  // Показ форм
  function showForm(formId) {
    document.getElementById('loginForm').style.display = 'none';
    document.getElementById('registerForm').style.display = 'none';
    document.getElementById('chat').style.display = 'none';
    document.getElementById(formId + 'Form').style.display = 'block';
  }

  // Вход
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

  // Регистрация
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

  // Чат
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
      // Обработка корней и степеней
      text = text.toLowerCase()
                 .replace(/sqrt\(([^)]+)\)/g, 'Math.sqrt($1)')
                 .replace(/√(\d+)/g, 'Math.sqrt($1)')
                 .replace(/(\d+)\s*\^\s*(\d+)/g, 'Math.pow($1,$2)');

      // Простое уравнение вида "x + 5 = 10"
      if (text.includes('=') && text.includes('x')) {
        const parts = text.split('=');
        const left = parts[0].replace(/x/g, '0');
        const right = parseFloat(parts[1]);

        const leftVal = eval(left);
        const xValue = right - leftVal;
        return `Ответ: x = ${xValue}`;
      }

      // Проверка допустимых символов (чтобы не было кода)
      if (!/^[\d+\-*/().=\s^√mathpowqrtx]+$/.test(text)) {
        return "Пожалуйста, введите корректный математический пример.";
      }

      const result = eval(text);
      return `Ответ: ${result}`;
    } catch (e) {
      return "Ошибка в примере. Попробуйте ещё раз.";
    }
  }
</script>

    
    
