# ai
OpenXI


<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OpenXI - Ваш AI-помощник</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #1a1a1a;
            color: #f0f0f0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }

        .chat-container {
            width: 450px;
            background-color: #2c2c2c;
            border-radius: 15px;
            box-shadow: 0 8px 30px rgba(0, 0, 0, 0.7);
            padding: 25px;
            display: flex;
            flex-direction: column;
        }

        .chat-log {
            height: 350px;
            overflow-y: auto;
            border-bottom: 2px solid #444;
            padding-bottom: 15px;
            margin-bottom: 15px;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .chat-log::-webkit-scrollbar {
            width: 8px;
        }
        .chat-log::-webkit-scrollbar-track {
            background: #3a3a3a;
            border-radius: 10px;
        }
        .chat-log::-webkit-scrollbar-thumb {
            background-color: #555;
            border-radius: 10px;
            border: 2px solid #3a3a3a;
        }

        .message {
            max-width: 85%;
            padding: 12px 18px;
            border-radius: 20px;
            line-height: 1.5;
            word-wrap: break-word;
        }

        .user-message {
            background-color: #007bff;
            align-self: flex-end;
            border-bottom-right-radius: 5px;
        }

        .ai-message {
            background-color: #444;
            align-self: flex-start;
            border-bottom-left-radius: 5px;
            display: flex;
            align-items: center;
        }

        .ai-message .emotion {
            font-size: 24px;
            margin-right: 10px;
            animation: pulse 1.5s infinite;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        .input-container {
            display: flex;
            align-items: center;
        }

        .input-container input {
            flex-grow: 1;
            border: none;
            outline: none;
            padding: 12px 15px;
            border-radius: 25px;
            background-color: #333;
            color: #f0f0f0;
            font-size: 16px;
        }

        .input-container button {
            border: none;
            background-color: #00aaff;
            color: white;
            padding: 12px 20px;
            margin-left: 10px;
            border-radius: 25px;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        .input-container button:hover {
            background-color: #0077b6;
        }

        h3 {
            color: #00aaff;
            text-align: center;
            margin-bottom: 20px;
        }
    </style>
</head>
<body>

    <div class="chat-container">
        <h3>OpenXI - Ваш AI-помощник</h3>
        <div class="chat-log" id="chat-log">
            <div class="message ai-message"><span class="emotion">😊</span> <span>Привет! Я OpenXI, ваш многозадачный помощник. Чем могу помочь?</span></div>
        </div>
        <div class="input-container">
            <input type="text" id="userInput" placeholder="Введите ваш вопрос или пример...">
            <button onclick="sendMessage()">Отправить</button>
        </div>
    </div>

    <script>
        const chatLog = document.getElementById('chat-log');
        const userInput = document.getElementById('userInput');

        function sendMessage() {
            const userText = userInput.value.trim();
            if (userText === '') return;

            addMessage(userText, 'user');
            userInput.value = '';

            setTimeout(() => {
                const { response, emotion } = getAiResponse(userText.toLowerCase());
                addMessage(response, 'ai', emotion);
            }, 1000);
        }

        function getAiResponse(text) {
            let emotion = '🙂';
            let response = 'Извините, я пока не могу ответить на этот вопрос.';

            // Приветствие и простые вопросы
            if (text.includes('привет') || text.includes('здравствуй')) {
                emotion = '😊';
                response = 'Приветствую! Рад вас видеть.';
            } else if (text.includes('как тебя зовут')) {
                emotion = '🙂';
                response = 'Меня зовут OpenXI, я ваш виртуальный помощник.';
            } else if (text.includes('что ты умеешь')) {
                emotion = '🤔';
                response = 'Я могу отвечать на общие вопросы, решать математические примеры, а также рассказывать интересные факты.';
            } else if (text.includes('как дела')) {
                emotion = '😄';
                response = 'У меня всё отлично, спасибо, что спросили!';
            } else if (text.includes('динозавры')) {
                emotion = '🧐';
                response = 'Большинство динозавров вымерло примерно 66 миллионов лет назад.';
            } else if (text.includes('столица франции')) {
                emotion = '🌍';
                response = 'Столица Франции — Париж.';
            } else if (text.includes('сколько будет') || text.includes('+') || text.includes('-') || text.includes('*') || text.includes('/') || text.includes('х')) {
                // Сложный математический калькулятор
                const mathPart = text.replace('сколько будет', '').trim();
                try {
                    const cleanMath = mathPart.replace(/х/g, '*').replace(/×/g, '*').replace(/,/g, '.');
                    const result = math.evaluate(cleanMath);
                    emotion = '💡';
                    response = `Результат: ${result}`;
                } catch (e) {
                    emotion = '😕';
                    response = 'Я не смог решить этот пример. Попробуйте ввести его в формате "25.5 * 1804".';
                }
            } else {
                emotion = '🤷‍♂️';
                response = 'Извините, я пока не могу ответить на этот вопрос. Попробуйте спросить о динозаврах или попросить решить пример, например "сколько будет 538 * 1804".';
            }

            return { response, emotion };
        }

        function addMessage(text, sender, emotion = null) {
            const messageDiv = document.createElement('div');
            messageDiv.classList.add('message');
            messageDiv.classList.add(sender === 'user' ? 'user-message' : 'ai-message');
            
            if (sender === 'ai' && emotion) {
                const emotionSpan = document.createElement('span');
                emotionSpan.classList.add('emotion');
                emotionSpan.textContent = emotion;
                messageDiv.appendChild(emotionSpan);
            }

            const textSpan = document.createElement('span');
            textSpan.textContent = text;
            messageDiv.appendChild(textSpan);
            
            chatLog.appendChild(messageDiv);
            chatLog.scrollTop = chatLog.scrollHeight;
        }

        // Подключаем библиотеку math.js для решения сложных примеров
        const script = document.createElement('script');
        script.src = 'https://cdnjs.cloudflare.com/ajax/libs/mathjs/10.6.4/math.min.js';
        document.head.appendChild(script);

        userInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                sendMessage();
            }
        });
    </script>
</body>
</html>
