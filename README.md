# OpenXI uz
OpenXI4
меня создал Abdumalikov Asadbek
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>XIAI — Математический Ассистент</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  
  <!-- Подключаем необходимые библиотеки -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/11.11.0/math.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/plotly.js/2.24.1/plotly.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/tesseract.js@4/dist/tesseract.min.js"></script>

  <style>
    /* СТИЛИ ОБЫЧНОГО РЕЖИМА */
    body { 
      font-family: Arial, sans-serif; 
      background:#0b0c10; 
      color:#e6e6e6; 
      margin:0; 
      padding:20px; 
      transition: all 0.5s ease;
    }
    textarea {
      width:100%; 
      height:120px; 
      padding:10px; 
      font-size:15px;
      border-radius:8px; 
      border:1px solid #444; 
      background:#111; 
      color:#fff;
    }
    button { 
      margin-top:10px; 
      padding:10px 15px; 
      border-radius:6px; 
      border:0; 
      cursor:pointer;
      background:#e63946; 
      color:white; 
      font-weight:bold;
    }
    #blocker {
      position:fixed; 
      inset:0; 
      z-index:99999;
      display:none; 
      align-items:center; 
      justify-content:center;
      background:rgba(0,0,0,0.9);
      backdrop-filter: blur(4px);
    }
    .panel {
      background:#111217; 
      padding:28px; 
      border-radius:12px; 
      box-shadow:0 10px 40px rgba(0,0,0,0.6);
      border:1px solid rgba(255,255,255,0.03); 
      text-align:center;
    }
    .panel h1 { 
      color:#e63946; 
      margin-bottom:10px; 
    }
    .panel input {
      padding:10px; 
      margin-top:15px; 
      width:200px;
      border-radius:6px; 
      border:1px solid #444; 
      background:#222; 
      color:white;
    }
    .panel button { 
      margin-top:15px; 
      background:#28a745; 
    }
    img.logo {
      display:block;
      margin:20px auto;
      max-width:200px;
      border-radius:12px;
    }
    #robotWatcher {
      background: #111;
      color: #e63946;
      padding: 10px;
      text-align: center;
      font-weight: bold;
      font-size: 18px;
      border-bottom: 2px solid #e63946;
      position: sticky;
      top: 0;
      z-index: 10000;
    }
    .sirena {
      display: inline-block;
      animation: blink 1s infinite;
    }
    @keyframes blink {
      0%, 50%, 100% { opacity: 1; }
      25%, 75% { opacity: 0; }
    }
    .container {
      max-width: 600px;
      margin: auto;
      padding: 30px;
      background-color: white;
      color: black;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
      border-radius: 12px;
      margin-top: 30px;
    }
    #chatBox {
      height: 250px;
      overflow-y: auto;
      border: 1px solid #ccc;
      padding: 10px;
      margin-bottom: 10px;
    }
    .user { color: #000099; margin-bottom: 10px; }
    .bot { color: #009900; margin-bottom: 10px; }
    .voice-input-container {
      display: flex;
      margin-top: 10px;
    }
    #userInput {
      flex: 1;
      padding: 10px;
      border-radius: 6px;
      border: 1px solid #ccc;
    }
    #micButton {
      margin-left: 10px;
      width: 50px;
      border-radius: 50%;
      background: #e63946;
      color: white;
      border: none;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    #micButton.listening {
      background: #28a745;
      animation: pulse 1.5s infinite;
    }
    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.1); }
      100% { transform: scale(1); }
    }
    
    /* СТИЛИ VIP-РЕЖИМА (изначально скрыты) */
    #vip-container {
      display: none;
      opacity: 0;
      transition: opacity 1s ease;
    }
    
    /* Анимация заставки VIP */
    #vip-splash {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: linear-gradient(135deg, #1a1c2b 0%, #2a2d43 100%);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 10000;
      opacity: 0;
      pointer-events: none;
      transition: opacity 1s ease;
    }
    
    .vip-title {
      font-size: 5rem;
      font-weight: bold;
      background: linear-gradient(45deg, #ff6b6b, #4ecdc4, #ffd700, #9370db, #ff6b6b);
      background-size: 400% 400%;
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      animation: rainbow 3s ease infinite, pulse 2s infinite;
      text-align: center;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    
    @keyframes rainbow {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }
    
    /* Стили VIP-интерфейса */
    :root {
      --primary: #2a2d43;
      --secondary: #ff6b6b;
      --accent: #4ecdc4;
      --light: #f7f9fc;
      --dark: #1a1c2b;
      --vip-gold: #ffd700;
      --vip-purple: #9370db;
    }
    
    .vip-container {
      max-width: 1200px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: 1fr 3fr;
      gap: 20px;
    }
    
    .vip-header {
      grid-column: 1 / -1;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 15px 0;
      border-bottom: 2px solid var(--accent);
      margin-bottom: 20px;
    }
    
    .vip-logo {
      display: flex;
      align-items: center;
      gap: 10px;
    }
    
    .vip-logo img {
      width: 50px;
      height: 50px;
      border-radius: 50%;
      object-fit: cover;
    }
    
    .vip-logo h1 {
      font-size: 24px;
      color: var(--accent);
    }
    
    .vip-user-controls {
      display: flex;
      gap: 15px;
      align-items: center;
    }
    
    .vip-btn {
      padding: 10px 15px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-weight: 600;
      transition: all 0.3s ease;
    }
    
    .vip-btn-primary {
      background: var(--accent);
      color: var(--dark);
    }
    
    .vip-btn-primary:hover {
      background: #3bbcb4;
      transform: translateY(-2px);
    }
    
    .vip-btn-vip {
      background: linear-gradient(135deg, var(--vip-gold) 0%, var(--vip-purple) 100%);
      color: var(--dark);
    }
    
    .vip-btn-vip:hover {
      transform: translateY(-2px);
      box-shadow: 0 5px 15px rgba(255, 215, 0, 0.4);
    }
    
    .vip-sidebar {
      background: rgba(42, 45, 67, 0.8);
      border-radius: 10px;
      padding: 20px;
      backdrop-filter: blur(10px);
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
    }
    
    .vip-sidebar h2 {
      color: var(--accent);
      margin-bottom: 15px;
      font-size: 20px;
    }
    
    .vip-sidebar-menu {
      list-style: none;
    }
    
    .vip-sidebar-menu li {
      margin-bottom: 10px;
    }
    
    .vip-sidebar-menu a {
      color: var(--light);
      text-decoration: none;
      display: block;
      padding: 10px;
      border-radius: 5px;
      transition: background 0.3s;
    }
    
    .vip-sidebar-menu a:hover {
      background: rgba(255, 255, 255, 0.1);
    }
    
    .vip-main-content {
      display: flex;
      flex-direction: column;
      gap: 20px;
    }
    
    .vip-card {
      background: rgba(42, 45, 67, 0.8);
      border-radius: 10px;
      padding: 20px;
      backdrop-filter: blur(10px);
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
    }
    
    .vip-card-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 15px;
    }
    
    .vip-card-header h2 {
      color: var(--accent);
      font-size: 22px;
    }
    
    .vip-input-group {
      display: flex;
      gap: 10px;
      margin-bottom: 15px;
    }
    
    .vip-input-group input {
      flex: 1;
      padding: 12px 15px;
      border: none;
      border-radius: 5px;
      background: rgba(255, 255, 255, 0.1);
      color: var(--light);
      font-size: 16px;
    }
    
    .vip-input-group input:focus {
      outline: 2px solid var(--accent);
    }
    
    .vip-chat-container {
      height: 300px;
      overflow-y: auto;
      padding: 15px;
      background: rgba(26, 28, 43, 0.5);
      border-radius: 8px;
      margin-bottom: 15px;
    }
    
    .vip-message {
      margin-bottom: 15px;
      padding: 10px 15px;
      border-radius: 8px;
      max-width: 80%;
    }
    
    .vip-user-message {
      background: rgba(78, 205, 196, 0.2);
      margin-left: auto;
      border-bottom-right-radius: 2px;
    }
    
    .vip-bot-message {
      background: rgba(42, 45, 67, 0.9);
      margin-right: auto;
      border-bottom-left-radius: 2px;
    }
    
    .vip-steps-container {
      background: rgba(26, 28, 43, 0.7);
      padding: 15px;
      border-radius: 8px;
      margin-top: 10px;
      font-family: 'Courier New', monospace;
    }
    
    .vip-step {
      margin-bottom: 8px;
      padding-left: 15px;
      border-left: 2px solid var(--accent);
    }
    
    .vip-plot-container {
      width: 100%;
      height: 300px;
      margin: 15px 0;
      background: white;
      border-radius: 8px;
    }
    
    .vip-toolbar {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
      margin: 15px 0;
    }
    
    .vip-tool-btn {
      padding: 8px 12px;
      background: rgba(255, 255, 255, 0.1);
      border: none;
      border-radius: 5px;
      color: var(--light);
      cursor: pointer;
      transition: background 0.3s;
    }
    
    .vip-tool-btn:hover {
      background: rgba(255, 255, 255, 0.2);
    }
    
    .vip-problem-generator {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 15px;
    }
    
    .vip-problem-card {
      background: rgba(26, 28, 43, 0.7);
      padding: 15px;
      border-radius: 8px;
      text-align: center;
    }
    
    .vip-badge {
      display: inline-block;
      padding: 3px 8px;
      background: linear-gradient(135deg, var(--vip-gold) 0%, var(--vip-purple) 100%);
      color: var(--dark);
      border-radius: 20px;
      font-size: 12px;
      font-weight: bold;
      margin-left: 10px;
    }
    
    /* Стили для загрузки изображений */
    .image-upload-container {
      margin: 15px 0;
      text-align: center;
    }
    
    .image-preview {
      max-width: 100%;
      max-height: 200px;
      margin-top: 10px;
      display: none;
      border-radius: 8px;
      border: 2px solid var(--accent);
    }
    
    .upload-btn {
      background: linear-gradient(135deg, var(--accent) 0%, #007bff 100%);
      color: white;
      padding: 10px 15px;
      border-radius: 5px;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 8px;
      font-weight: 600;
      transition: all 0.3s;
    }
    
    .upload-btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 5px 15px rgba(0, 123, 255, 0.4);
    }
    
    .ocr-loading {
      display: none;
      text-align: center;
      margin: 10px 0;
      color: var(--accent);
    }
    
    .ocr-correction {
      margin-top: 10px;
      display: none;
    }
    
    .ocr-correction input {
      width: 100%;
      padding: 10px;
      border-radius: 5px;
      border: 1px solid var(--accent);
      background: rgba(255, 255, 255, 0.1);
      color: var(--light);
    }
    
    /* Стили для языкового переключателя */
    .language-selector {
      position: relative;
      display: inline-block;
    }
    
    .language-btn {
      background: rgba(255, 255, 255, 0.1);
      border: 1px solid var(--accent);
      color: var(--light);
      padding: 8px 15px;
      border-radius: 5px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 8px;
      transition: all 0.3s;
    }
    
    .language-btn:hover {
      background: rgba(255, 255, 255, 0.2);
    }
    
    .language-dropdown {
      position: absolute;
      top: 100%;
      right: 0;
      background: rgba(42, 45, 67, 0.95);
      border-radius: 5px;
      padding: 10px 0;
      min-width: 150px;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
      display: none;
      z-index: 1000;
      backdrop-filter: blur(10px);
    }
    
    .language-dropdown.active {
      display: block;
    }
    
    .language-option {
      padding: 8px 15px;
      cursor: pointer;
      transition: background 0.3s;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    
    .language-option:hover {
      background: rgba(255, 255, 255, 0.1);
    }
    
    .language-flag {
      width: 20px;
      height: 15px;
      border-radius: 2px;
    }
    
    /* МОБИЛЬНАЯ АДАПТАЦИЯ */
    @media (max-width: 900px) {
      .vip-container {
        grid-template-columns: 1fr;
      }
      
      .vip-problem-generator {
        grid-template-columns: 1fr;
      }
    }
    
    /* Анимации */
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }
    
    .vip-message {
      animation: fadeIn 0.3s ease;
    }
    
    .vip-pulse {
      animation: pulse 2s infinite;
    }
    
    /* МОБИЛЬНАЯ АДАПТАЦИЯ - ДОПОЛНИТЕЛЬНЫЕ СТИЛИ */
    @media (max-width: 768px) {
      body {
        padding: 10px;
        font-size: 14px;
      }
      
      .container {
        padding: 15px;
        margin-top: 15px;
      }
      
      #chatBox {
        height: 200px;
      }
      
      textarea {
        height: 100px;
        font-size: 16px;
      }
      
      button {
        padding: 12px 18px;
        font-size: 16px;
      }
      
      .vip-header {
        flex-direction: column;
        gap: 15px;
        padding: 10px 0;
      }
      
      .vip-user-controls {
        flex-wrap: wrap;
        justify-content: center;
      }
      
      .vip-sidebar {
        padding: 15px;
      }
      
      .vip-sidebar-menu a {
        padding: 12px;
        font-size: 16px;
      }
      
      .vip-input-group {
        flex-direction: column;
      }
      
      .vip-input-group input {
        padding: 14px;
        font-size: 16px;
      }
      
      .vip-chat-container {
        height: 250px;
      }
      
      .vip-message {
        max-width: 90%;
        font-size: 15px;
        padding: 12px;
      }
      
      .vip-toolbar {
        justify-content: center;
      }
      
      .vip-tool-btn {
        padding: 10px 14px;
        font-size: 16px;
      }
      
      .vip-plot-container {
        height: 250px;
      }
      
      .vip-card {
        padding: 15px;
      }
      
      .panel {
        padding: 20px;
        width: 90%;
        max-width: 90%;
      }
      
      .panel input {
        width: 100%;
        box-sizing: border-box;
      }
      
      #robotWatcher {
        font-size: 16px;
        padding: 8px;
      }
      
      .payment-buttons {
        flex-direction: column;
        gap: 15px;
      }
      
      .payment-buttons a {
        width: 100%;
        text-align: center;
        box-sizing: border-box;
      }
      
      .mobile-menu-toggle {
        display: block;
        position: fixed;
        top: 15px;
        right: 15px;
        z-index: 1000;
        background: var(--accent);
        color: var(--dark);
        width: 50px;
        height: 50px;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 24px;
        box-shadow: 0 4px 10px rgba(0,0,0,0.3);
      }
      
      .vip-sidebar {
        position: fixed;
        top: 0;
        left: -280px;
        width: 280px;
        height: 100%;
        z-index: 999;
        transition: left 0.3s ease;
        overflow-y: auto;
      }
      
      .vip-sidebar.active {
        left: 0;
      }
      
      .sidebar-overlay {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0,0,0,0.5);
        z-index: 998;
        display: none;
      }
      
      .sidebar-overlay.active {
        display: block;
      }
      
      .language-dropdown {
        position: fixed;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        width: 90%;
        max-width: 300px;
      }
    }
    
    @media (min-width: 769px) {
      .mobile-menu-toggle,
      .sidebar-overlay {
        display: none;
      }
    }
    
    @media (max-width: 360px) {
      .vip-title {
        font-size: 3rem;
      }
      
      .vip-logo h1 {
        font-size: 20px;
      }
      
      .vip-btn {
        padding: 8px 12px;
        font-size: 14px;
      }
    }
  </style>
</head>
<body>
  <!-- Кнопка мобильного меню -->
  <div class="mobile-menu-toggle" onclick="toggleMobileMenu()">☰</div>
  <div class="sidebar-overlay" onclick="toggleMobileMenu()"></div>

  <!-- ОБЫЧНЫЙ РЕЖИМ -->
  <div id="normal-app">
    <!-- 🔴 Баннер про онлайн-робота -->
    <div id="robotWatcher">
      👁 За вами следит <b>онлайн-робот-администратор</b>
      <span class="sirena">🚨</span><span class="sirena">🚨</span><span class="sirena">🚨</span>
    </div>

    <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQC0m3-hAkL0qx5VxSK_dxymtGUWrZqdDMG_Q&s" alt="OpenXi Logo" class="logo">

    <h2>📝 Напиши заметку</h2>
    <textarea id="note" placeholder="Введите текст..."></textarea><br>
    <button id="saveNote">Сохранить</button>

    <!-- Чат -->
    <div class="container">
      <h3>Чат XIAI</h3>
      <div id="chatBox"></div>
      <div class="voice-input-container">
        <input type="text" id="userInput" placeholder="Напиши пример или нажми микрофон...">
        <button id="micButton">🎤</button>
      </div>
      <button onclick="sendMessage()">Отправить</button>
    </div>

    <!-- 🔊 Сирена -->
    <audio id="alarmSound" src="https://www.soundjay.com/misc/sounds/police-siren-01.mp3" preload="auto"></audio>

    <!-- Блокировка -->
    <div id="blocker">
      <div class="panel">
        <h1>🚫 Доступ заблокирован</h1>
        <p>Вы нарули правила (мат, спам или хакерская атака).</p>
        <p><b>Только администратор может разблокировать сайт.</b></p>
        <input type="password" id="adminPass" placeholder="Введите пароль">
        <br>
        <button id="unlockBtn">Разблокировать</button>
      </div>
    </div>
  </div>

  <!-- VIP РЕЖИМ (изначально скрыт) -->
  <div id="vip-container">
    <div class="vip-container">
      <header class="vip-header">
        <div class="vip-logo">
          <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQC0m3-hAkL0qx5VxSK_dxymtGUWrZqdDMG_Q&s" alt="XIAI Logo">
          <h1>XIAI Pro <span class="vip-badge">VIP</span></h1>
        </div>
        <div class="vip-user-controls">
          <!-- Языковой переключатель -->
          <div class="language-selector">
            <div class="language-btn" onclick="toggleLanguageDropdown()">
              <span id="current-language">🌐 Русский</span>
              <span>▼</span>
            </div>
            <div class="language-dropdown" id="language-dropdown">
              <div class="language-option" onclick="setLanguage('ru')">
                <span class="language-flag" style="background: linear-gradient(to bottom, #0039a6 33%, #fff 33%, #fff 66%, #d52b1e 66%)"></span>
                Русский
              </div>
              <div class="language-option" onclick="setLanguage('uz')">
                <span class="language-flag" style="background: linear-gradient(to bottom, #1eb53a 25%, #0099b5 25%, #0099b5 50%, #ce1126 50%, #ce1126 75%, #fff 75%)"></span>
                O'zbek
              </div>
              <div class="language-option" onclick="setLanguage('en')">
                <span class="language-flag" style="background: url('data:image/svg+xml;utf8,<svg xmlns=\"http://www.w3.org/2000/svg\" viewBox=\"0 0 60 30\" width=\"20\" height=\"10\"><clipPath id=\"a\"><path d=\"M0 0v30h60V0z\"/></clipPath><clipPath id=\"b\"><path d=\"M30 15h30v15zv15H0zH0V0zV0h30z\"/></clipPath><g clip-path=\"url(#a)\"><path d=\"M0 0v30h60V0z\" fill=\"#012169\"/><path d=\"M0 0l60 30m0-30L0 30\" stroke=\"#fff\" stroke-width=\"6\"/><path d=\"M0 0l60 30m0-30L0 30\" clip-path=\"url(#b)\" stroke=\"#C8102E\" stroke-width=\"4\"/><path d=\"M30 0v30M0 15h60\" stroke=\"#fff\" stroke-width=\"10\"/><path d=\"M30 0v30M0 15h60\" stroke=\"#C8102E\" stroke-width=\"6\"/></g></svg>')"></span>
                English
              </div>
              <div class="language-option" onclick="setLanguage('es')">
                <span class="language-flag" style="background: linear-gradient(to right, #aa151b 25%, #f1bf00 25%, #f1bf00 75%, #aa151b 75%)"></span>
                Español
              </div>
              <div class="language-option" onclick="setLanguage('it')">
                <span class="language-flag" style="background: linear-gradient(to right, #009246 33%, #fff 33%, #fff 66%, #ce2b37 66%)"></span>
                Italiano
              </div>
              <div class="language-option" onclick="setLanguage('de')">
                <span class="language-flag" style="background: linear-gradient(to bottom, #000 33%, #dd0000 33%, #dd0000 66%, #ffce00 66%)"></span>
                Deutsch
              </div>
            </div>
          </div>
          
          <button class="vip-btn vip-btn-primary" onclick="exportToPDF()" id="export-pdf-btn">Экспорт в PDF</button>
          <button class="vip-btn vip-btn-vip" onclick="showPremiumModal()" id="premium-btn">Premium</button>
        </div>
      </header>
      
      <aside class="vip-sidebar">
        <h2 id="tools-title">Инструменты</h2>
        <ul class="vip-sidebar-menu">
          <li><a href="#" onclick="setActiveTool('calculator')" id="calculator-btn">Калькулятор</a></li>
          <li><a href="#" onclick="setActiveTool('graph')" id="graph-btn">Построитель графиков</a></li>
          <li><a href="#" onclick="setActiveTool('equation')" id="equation-btn">Решение уравнений</a></li>
          <li><a href="#" onclick="setActiveTool('derivative')" id="derivative-btn">Производные</a></li>
          <li><a href="#" onclick="setActiveTool('integral')" id="integral-btn">Интегралы</a></li>
          <li><a href="#" onclick="setActiveTool('matrix')" id="matrix-btn">Матрицы</a></li>
          <li><a href="#" onclick="setActiveTool('generator')" id="generator-btn">Генератор задач</a></li>
        </ul>
      </aside>
      
      <main class="vip-main-content">
        <div class="vip-card">
          <div class="vip-card-header">
            <h2 id="active-tool-title">Математический ассистент</h2>
            <div class="mode-indicator">Режим: <span id="current-mode">Расширенный</span></div>
          </div>
          
          <div class="vip-input-group">
            <input type="text" id="math-input" placeholder="Введите математическое выражение или вопрос..." onkeypress="handleKeyPress(event)">
            <button class="vip-btn vip-btn-primary" onclick="solveMath()" id="solve-btn">Решить</button>
          </div>
          
          <div class="vip-toolbar">
            <button class="vip-tool-btn" onclick="insertSymbol('√')">√</button>
            <button class="vip-tool-btn" onclick="insertSymbol('π')">π</button>
            <button class="vip-tool-btn" onclick="insertSymbol('∞')">∞</button>
            <button class="vip-tool-btn" onclick="insertSymbol('∫')">∫</button>
            <button class="vip-tool-btn" onclick="insertSymbol('∑')">∑</button>
            <button class="vip-tool-btn" onclick="insertSymbol('∂')">∂</button>
            <button class="vip-tool-btn" onclick="insertSymbol('²')">x²</button>
            <button class="vip-tool-btn" onclick="insertSymbol('³')">x³</button>
          </div>
          
          <!-- Контейнер для загрузки изображений -->
          <div class="image-upload-container">
            <label for="math-image-upload" class="upload-btn" id="upload-image-btn">
              <span>📷</span> Загрузить изображение с примером
            </label>
            <input type="file" id="math-image-upload" accept="image/*" capture="environment" style="display: none;">
            <div class="ocr-loading" id="ocr-loading">
              <p id="ocr-loading-text">Распознавание текста... <span class="vip-pulse">⏳</span></p>
            </div>
            <img id="image-preview" class="image-preview" alt="Предпросмотр загруженного изображения">
            
            <!-- Поле для ручной корректировки -->
            <div class="ocr-correction" id="ocr-correction">
              <p id="ocr-correction-text">Проверьте и откорректируйте распознанный текст:</p>
              <input type="text" id="ocr-corrected-text" placeholder="Исправьте распознанный текст здесь...">
              <button class="vip-btn vip-btn-primary" onclick="useCorrectedText()" id="use-corrected-btn">Использовать исправленный текст</button>
            </div>
          </div>
          
          <div class="vip-chat-container" id="vip-chat-container">
            <div class="vip-message vip-bot-message" id="welcome-message">
              Добро пожаловать в XIAI Pro! Я ваш персональный математический ассистент. 
              Чем могу помочь? Вы можете решать примеры, строить графики, находить производные и многое другое.
            </div>
          </div>
          
          <div id="vip-steps-container" class="vip-steps-container" style="display: none;"></div>
          <div id="vip-plot-container" class="vip-plot-container" style="display: none;"></div>
          
          <div id="vip-problem-generator" class="vip-problem-generator" style="display: none;">
            <div class="vip-problem-card">
              <h3 id="arithmetic-title">Арифметика</h3>
              <button class="vip-btn vip-btn-primary" onclick="generateProblem('arithmetic')" id="generate-arithmetic-btn">Сгенерировать задачу</button>
              <p id="arithmetic-problem"></p>
            </div>
            <div class="vip-problem-card">
              <h3 id="algebra-title">Алгебра</h3>
              <button class="vip-btn vip-btn-primary" onclick="generateProblem('algebra')" id="generate-algebra-btn">Сгенерировать задачу</button>
              <p id="algebra-problem"></p>
            </div>
            <div class="vip-problem-card">
              <h3 id="geometry-title">Геометрия</h3>
              <button class="vip-btn vip-btn-primary" onclick="generateProblem('geometry')" id="generate-geometry-btn">Сгенерировать задачу</button>
              <p id="geometry-problem"></p>
            </div>
            <div class="vip-problem-card">
              <h3 id="advanced-title">Высшая математика</h3>
              <button class="vip-btn vip-btn-primary" onclick="generateProblem('advanced')" id="generate-advanced-btn">Сгенерировать задачу</button>
              <p id="advanced-problem"></p>
            </div>
          </div>
        </div>
        
        <div class="vip-card">
          <div class="vip-card-header">
            <h2 id="verification-title">Проверка решений</h2>
          </div>
          <div class="vip-input-group">
            <input type="text" id="user-solution" placeholder="Введите ваше решение для проверки...">
            <button class="vip-btn vip-btn-primary" onclick="checkSolution()" id="check-solution-btn">Проверить</button>
          </div>
          <div id="verification-result"></div>
        </div>
      </main>
    </div>
  </div>

  <!-- Анимация заставки VIP -->
  <div id="vip-splash">
    <div class="vip-title">OpenXI.US</div>
  </div>

  <!-- Кнопки оплаты -->
  <div style="height:100vh; display:flex; align-items:center; justify-content:center; flex-direction:column; gap:20px; background:linear-gradient(135deg,#000000,#000000); font-family:Arial,sans-serif; padding: 20px; box-sizing: border-box;" class="payment-buttons">

    <!-- WhatsApp -->
    <a href="https://wa.me/+998999100097" target="_blank"
       style="display:inline-block; padding:16px 28px; font-size:18px; font-weight:bold; border-radius:14px; text-decoration:none; color:#04260f; background:#25D366; box-shadow:0 6px 16px rgba(0,0,0,0.4); transition:all .3s;">
      💬 Связаться в WhatsApp
    </a>

    <!-- Click -->
    <a href="https://my.click.uz/services/" target="_blank"
       style="display:inline-block; padding:16px 28px; font-size:18px; font-weight:bold; border-radius:14px; text-decoration:none; color:white; background:#007bff; box-shadow:0 6px 16px rgba(0,0,0,0.4); transition:all .3s;">
      💳 Оплатить через Click
    </a>

    <!-- PayMe -->
    <a href="https://payme.uz/home" target="_blank"
       style="display:inline-block; padding:16px 28px; font-size:18px; font-weight:bold; border-radius:14px; text-decoration:none; color:white; background:#673ab7; box-shadow:0 6px 16px rgba(0,0,0,0.4); transition:all .3s;">
      💳 Оплатить через PayMe
    </a>

  </div>

  <div style="display: flex; gap: 10px; justify-content: center; margin: 20px 0;">
    <button><a href="https://asadbek470.github.io/support/" style="text-decoration: none; color: white;">support</a></button>
    <button><a href="https://asadbek470.github.io/admin.com" style="text-decoration: none; color: white;">adminPass</a></button>
  </div>

  <script>
    // ========== ОБЫЧНЫЙ РЕЖИМ ==========
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
    const micButton = document.getElementById("micButton");

    // Функция переключения мобильного меню
    function toggleMobileMenu() {
      const sidebar = document.querySelector('.vip-sidebar');
      const overlay = document.querySelector('.sidebar-overlay');
      sidebar.classList.toggle('active');
      overlay.classList.toggle('active');
    }

    // 🚨 Блокировка
    function blockUser(reason="Нарушение правил") {
      localStorage.setItem("blocked","true");
      blocker.style.display = "flex";
      try { alarm.play(); } catch(e) {}
      console.warn("Пользователь заблокирован:", reason);
    }

    // Проверка при загрузке
    if (localStorage.getItem("blocked") === "true") {
      blocker.style.display = "flex";
    }

    // Восстановление заметки
    if (localStorage.getItem("savedNote")) {
      noteInput.value = localStorage.getItem("savedNote");
    }

    // Сохранение заметки
    saveBtn.addEventListener("click", () => {
      let text = noteInput.value.toLowerCase();
      for (let word of badWords) {
        if (text.includes(word)) {
          blockUser("Мат в заметке");
          return;
        }
      }
      for (let p of hackPatterns) {
        if (text.includes(p)) {
          blockUser("Хакерская атака");
          return;
        }
      }
      localStorage.setItem("savedNote", noteInput.value);
      alert("Заметка сохранена ✅");
    });

    // Разблокировка
    unlockBtn.addEventListener("click", () => {
      if (adminPass.value === adminPassword) {
        localStorage.setItem("blocked","false");
        blocker.style.display = "none";
        adminPass.value = "";
        alert("✅ Сайт разблокирован (админ вошёл)");
      } else {
        alert("❌ Неверный пароль!");
      }
    });

    // Чат
    let messageLog = [];
    function sendMessage() {
      const text = userInput.value.trim();
      if (!text) return;

      // Проверка на команду активации VIP-режима
      if (text.toLowerCase() === '.slash vip') {
        activateVIPMode();
        return;
      }

      // Проверка на мат и хак
      let lower = text.toLowerCase();
      for (let word of badWords) if (lower.includes(word)) return blockUser("Мат в чате");
      for (let p of hackPatterns) if (lower.includes(p)) return blockUser("Хакерская атака");

      // Спам: 5 сообщений за 10 сек → блок
      let now = Date.now();
      messageLog.push(now);
      messageLog = messageLog.filter(t => now - t < 10000);
      if (messageLog.length > 5) return blockUser("Спам атака");

      appendMessage("user", text);
      userInput.value = "";

      setTimeout(() => {
        appendMessage("bot", getBotReply(text));
      }, 400);
    }

    function appendMessage(sender, text) {
      const msg = document.createElement("div");
      msg.className = sender;
      msg.textContent = (sender === "user" ? "Вы: " : "XIAI: ") + text;
      chatBox.appendChild(msg);
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    function getBotReply(text) {
      try {
        // Заменяем все варианты обозначения умножения на *
        let cleanedText = text
          .replace(/[xх×]/gi, '*')  // заменяем x, х, × на *
          .replace(/[÷:]/gi, '/')   // заменяем ÷, : на /
          .replace(/\s+/g, '')      // удаляем все пробелы
          .replace(/[^0-9+\-*/().]/g, ''); // удаляем все лишние символы
        
        if (!cleanedText) return "Пожалуйста, введите математическое выражение";
        
        const result = math.evaluate(cleanedText);
        return `Ответ: ${text} = ${result}`;
      } catch(e) {
        return "Ошибка: Некорректный пример. Попробуйте что-то вроде '2+2' или '5*3'";
      }
    }

    // Блокировка при F12/DevTools
    document.addEventListener("keydown", (e) => {
      if (e.key === "F12" || (e.ctrlKey && e.shiftKey && (e.key === "I" || e.key === "J"))) {
        e.preventDefault();
        blockUser("Попытка открыть DevTools");
      }
    });

    // Голосовой ввод
    let recognition = null;
    if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
      recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
      recognition.continuous = false;
      recognition.interimResults = false;
      recognition.lang = 'ru-RU';

      recognition.onstart = function() {
        micButton.classList.add('listening');
        userInput.placeholder = "Говорите...";
      };

      recognition.onresult = function(event) {
        const transcript = event.results[0][0].transcript;
        userInput.value = transcript;
        micButton.classList.remove('listening');
        userInput.placeholder = "Напиши пример или нажми микрофон...";
        
        // Автоматически отправляем сообщение после распознавания
        setTimeout(sendMessage, 500);
      };

      recognition.onerror = function(event) {
        console.error('Ошибка распознавания голоса:', event.error);
        micButton.classList.remove('listening');
        userInput.placeholder = "Напиш пример или нажми микрофон...";
        
        if (event.error === 'not-allowed') {
          appendMessage("bot", "Разрешите доступ к микрофону для голосового ввода");
        }
      };

      recognition.onend = function() {
        micButton.classList.remove('listening');
        userInput.placeholder = "Напиш пример или нажми микрофон...";
      };

      micButton.addEventListener('click', () => {
        if (micButton.classList.contains('listening')) {
          recognition.stop();
          return;
        }
        
        try {
          recognition.start();
        } catch (error) {
          console.error('Ошибка запуска распознавания:', error);
          appendMessage("bot", "Ошибка доступа к микрофону. Проверьте разрешения браузера.");
        }
      });
    } else {
      // Браузер не поддерживает распознавание речи
      micButton.style.display = 'none';
      userInput.placeholder = "Напиш пример...";
      appendMessage("bot", "Ваш браузер не поддерживает голосовой ввод");
    }

    // Отправка сообщения по нажатию Enter
    userInput.addEventListener('keypress', function(e) {
      if (e.key === 'Enter') {
        sendMessage();
      }
    });

    // ========== VIP РЕЖИМ ==========
    // Функция активации VIP-режима
    function activateVIPMode() {
      // Показываем анимацию заставки
      const splash = document.getElementById('vip-splash');
      splash.style.opacity = '1';
      splash.style.pointerEvents = 'auto';
      
      // Добавляем сообщение в обычный чат
      appendMessage("bot", "Обнаружена VIP-команда! Активируем расширенный режим...");
      
      // Через 3 секунды скрываем заставку и показываем VIP-интерфейс
      setTimeout(() => {
        splash.style.opacity = '0';
        splash.style.pointerEvents = 'none';
        
        // Скрываем обычный интерфейс и показываем VIP
        document.getElementById('normal-app').style.display = 'none';
        document.getElementById('vip-container').style.display = 'block';
        
        // Плавное появление VIP-интерфейса
        setTimeout(() => {
          document.getElementById('vip-container').style.opacity = '1';
        }, 100);
        
        // Инициализируем VIP-функции
        initVIPMode();
      }, 3000);
    }

    // Инициализация VIP-режима
    function initVIPMode() {
      // Текущее состояние приложения
      const appState = {
        activeTool: 'calculator',
        isPremium: true,
        history: [],
        stepByStepSolutions: true,
        currentLanguage: 'ru'
      };
      
      // ========== МНОГОЯЗЫЧНАЯ ПОДДЕРЖКА ==========
      const translations = {
        ru: {
          // Интерфейс
          'current-language': '🌐 Русский',
          'active-tool-title': 'Математический ассистент',
          'tools-title': 'Инструменты',
          'calculator-btn': 'Калькулятор',
          'graph-btn': 'Построитель графиков',
          'equation-btn': 'Решение уравнений',
          'derivative-btn': 'Производные',
          'integral-btn': 'Интегралы',
          'matrix-btn': 'Матрицы',
          'generator-btn': 'Генератор задач',
          'solve-btn': 'Решить',
          'export-pdf-btn': 'Экспорт в PDF',
          'premium-btn': 'Premium',
          'upload-image-btn': '📷 Загрузить изображение с примером',
          'use-corrected-btn': 'Использовать исправленный текст',
          'check-solution-btn': 'Проверить',
          'verification-title': 'Проверка решений',
          'arithmetic-title': 'Арифметика',
          'algebra-title': 'Алгебра',
          'geometry-title': 'Геометрия',
          'advanced-title': 'Высшая математика',
          'generate-arithmetic-btn': 'Сгенерировать задачу',
          'generate-algebra-btn': 'Сгенерировать задачу',
          'generate-geometry-btn': 'Сгенерировать задачу',
          'generate-advanced-btn': 'Сгенерировать задачу',
          
          // Сообщения
          'welcome-message': 'Добро пожаловать в XIAI Pro! Я ваш персональный математический ассистент. Чем могу помочь?',
          'ocr-loading-text': 'Распознавание текста... ⏳',
          'ocr-correction-text': 'Проверьте и откорректируйте распознанный текст:',
          
          // Плейсхолдеры
          'math-input-placeholder': 'Введите математическое выражение или вопрос...',
          'user-solution-placeholder': 'Введите ваше решение для проверки...',
          'ocr-corrected-placeholder': 'Исправьте распознанный текст здесь...'
        },
        uz: {
          // Интерфейс
          'current-language': '🌐 O\'zbek',
          'active-tool-title': 'Matematik yordamchi',
          'tools-title': 'Vositalar',
          'calculator-btn': 'Kalkulyator',
          'graph-btn': 'Grafik qurish',
          'equation-btn': 'Tenglama yechish',
          'derivative-btn': 'Hosila',
          'integral-btn': 'Integral',
          'matrix-btn': 'Matritsalar',
          'generator-btn': 'Masala generatori',
          'solve-btn': 'Yechish',
          'export-pdf-btn': 'PDF ga eksport',
          'premium-btn': 'Premium',
          'upload-image-btn': '📷 Misol bilan rasm yuklash',
          'use-corrected-btn': 'Tuzatilgan matndan foydalanish',
          'check-solution-btn': 'Tekshirish',
          'verification-title': 'Yechimlarni tekshirish',
          'arithmetic-title': 'Arifmetika',
          'algebra-title': 'Algebra',
          'geometry-title': 'Geometriya',
          'advanced-title': 'Yuqori matematika',
          'generate-arithmetic-btn': 'Masala yaratish',
          'generate-algebra-btn': 'Masala yaratish',
          'generate-geometry-btn': 'Masala yaratish',
          'generate-advanced-btn': 'Masala yaratish',
          
          // Сообщения
          'welcome-message': 'XIAI Pro ga xush kelibsiz! Men sizning shaxsiy matematik yordamchingizman. Qanday yordam bera olaman?',
          'ocr-loading-text': 'Matnni tanib olish... ⏳',
          'ocr-correction-text': 'Tanib olingan matnni tekshiring va tuzating:',
          
          // Плейсхолдеры
          'math-input-placeholder': 'Matematik ifoda yoki savol kiriting...',
          'user-solution-placeholder': 'Tekshirish uchun yechimingizni kiriting...',
          'ocr-corrected-placeholder': 'Tanib olingan matnni bu yerda tuzating...'
        },
        en: {
          // Интерфейс
          'current-language': '🌐 English',
          'active-tool-title': 'Math Assistant',
          'tools-title': 'Tools',
          'calculator-btn': 'Calculator',
          'graph-btn': 'Graph Builder',
          'equation-btn': 'Equation Solver',
          'derivative-btn': 'Derivatives',
          'integral-btn': 'Integrals',
          'matrix-btn': 'Matrices',
          'generator-btn': 'Problem Generator',
          'solve-btn': 'Solve',
          'export-pdf-btn': 'Export to PDF',
          'premium-btn': 'Premium',
          'upload-image-btn': '📷 Upload image with example',
          'use-corrected-btn': 'Use corrected text',
          'check-solution-btn': 'Check',
          'verification-title': 'Solution Verification',
          'arithmetic-title': 'Arithmetic',
          'algebra-title': 'Algebra',
          'geometry-title': 'Geometry',
          'advanced-title': 'Advanced Math',
          'generate-arithmetic-btn': 'Generate Problem',
          'generate-algebra-btn': 'Generate Problem',
          'generate-geometry-btn': 'Generate Problem',
          'generate-advanced-btn': 'Generate Problem',
          
          // Сообщения
          'welcome-message': 'Welcome to XIAI Pro! I am your personal math assistant. How can I help you?',
          'ocr-loading-text': 'Recognizing text... ⏳',
          'ocr-correction-text': 'Check and correct the recognized text:',
          
          // Плейсхолдеры
          'math-input-placeholder': 'Enter a mathematical expression or question...',
          'user-solution-placeholder': 'Enter your solution for verification...',
          'ocr-corrected-placeholder': 'Correct the recognized text here...'
        },
        es: {
          // Интерфейс
          'current-language': '🌐 Español',
          'active-tool-title': 'Asistente matemático',
          'tools-title': 'Herramientas',
          'calculator-btn': 'Calculadora',
          'graph-btn': 'Constructor de gráficos',
          'equation-btn': 'Resolución de ecuaciones',
          'derivative-btn': 'Derivadas',
          'integral-btn': 'Integrales',
          'matrix-btn': 'Matrices',
          'generator-btn': 'Generador de problemas',
          'solve-btn': 'Resolver',
          'export-pdf-btn': 'Exportar a PDF',
          'premium-btn': 'Premium',
          'upload-image-btn': '📷 Subir imagen con ejemplo',
          'use-corrected-btn': 'Usar texto corregido',
          'check-solution-btn': 'Verificar',
          'verification-title': 'Verificación de soluciones',
          'arithmetic-title': 'Aritmética',
          'algebra-title': 'Álgebra',
          'geometry-title': 'Geometría',
          'advanced-title': 'Matemáticas avanzadas',
          'generate-arithmetic-btn': 'Generar problema',
          'generate-algebra-btn': 'Generar problema',
          'generate-geometry-btn': 'Generar problema',
          'generate-advanced-btn': 'Generar problema',
          
          // Сообщения
          'welcome-message': '¡Bienvenido a XIAI Pro! Soy tu asistente matemático personal. ¿Cómo puedo ayudarte?',
          'ocr-loading-text': 'Reconociendo texto... ⏳',
          'ocr-correction-text': 'Verifica y corrige el texto reconocido:',
          
          // Плейсхолдеры
          'math-input-placeholder': 'Ingresa una expresión matemática o pregunta...',
          'user-solution-placeholder': 'Ingresa tu solución para verificación...',
          'ocr-corrected-placeholder': 'Corrige el texto reconocido aquí...'
        },
        it: {
          // Интерфейс
          'current-language': '🌐 Italiano',
          'active-tool-title': 'Assistente matematico',
          'tools-title': 'Strumenti',
          'calculator-btn': 'Calcolatrice',
          'graph-btn': 'Costruttore di grafici',
          'equation-btn': 'Risoluzione equazioni',
          'derivative-btn': 'Derivate',
          'integral-btn': 'Integrali',
          'matrix-btn': 'Matrici',
          'generator-btn': 'Generatore di problemi',
          'solve-btn': 'Risolvi',
          'export-pdf-btn': 'Esporta in PDF',
          'premium-btn': 'Premium',
          'upload-image-btn': '📷 Carica immagine con esempio',
          'use-corrected-btn': 'Usa testo corretto',
          'check-solution-btn': 'Verifica',
          'verification-title': 'Verifica soluzioni',
          'arithmetic-title': 'Aritmetica',
          'algebra-title': 'Algebra',
          'geometry-title': 'Geometria',
          'advanced-title': 'Matematica avanzata',
          'generate-arithmetic-btn': 'Genera problema',
          'generate-algebra-btn': 'Genera problema',
          'generate-geometry-btn': 'Genera problema',
          'generate-advanced-btn': 'Genera problema',
          
          // Сообщения
          'welcome-message': 'Benvenuto in XIAI Pro! Sono il tuo assistente matematico personale. Come posso aiutarti?',
          'ocr-loading-text': 'Riconoscimento testo... ⏳',
          'ocr-correction-text': 'Controlla e correggi il testo riconosciuto:',
          
          // Плейсхолдеры
          'math-input-placeholder': 'Inserisci un\'espressione matematica o domanda...',
          'user-solution-placeholder': 'Inserisci la tua soluzione per la verifica...',
          'ocr-corrected-placeholder': 'Correggi il testo riconosciuto qui...'
        },
        de: {
          // Интерфейс
          'current-language': '🌐 Deutsch',
          'active-tool-title': 'Mathe-Assistent',
          'tools-title': 'Werkzeuge',
          'calculator-btn': 'Rechner',
          'graph-btn': 'Grafikersteller',
          'equation-btn': 'Gleichungslöser',
          'derivative-btn': 'Ableitungen',
          'integral-btn': 'Integrale',
          'matrix-btn': 'Matrizen',
          'generator-btn': 'Problemgenerator',
          'solve-btn': 'Lösen',
          'export-pdf-btn': 'Als PDF exportieren',
          'premium-btn': 'Premium',
          'upload-image-btn': '📷 Bild mit Beispiel hochladen',
          'use-corrected-btn': 'Korrigierten Text verwenden',
          'check-solution-btn': 'Überprüfen',
          'verification-title': 'Lösungsüberprüfung',
          'arithmetic-title': 'Arithmetik',
          'algebra-title': 'Algebra',
          'geometry-title': 'Geometrie',
          'advanced-title': 'Höhere Mathematik',
          'generate-arithmetic-btn': 'Problem generieren',
          'generate-algebra-btn': 'Problem generieren',
          'generate-geometry-btn': 'Problem generieren',
          'generate-advanced-btn': 'Problem generieren',
          
          // Сообщения
          'welcome-message': 'Willkommen bei XIAI Pro! Ich bin Ihr persönlicher Mathe-Assistent. Wie kann ich Ihnen helfen?',
          'ocr-loading-text': 'Texterkennung... ⏳',
          'ocr-correction-text': 'Überprüfen und korrigieren Sie den erkannten Text:',
          
          // Плейсхолдеры
          'math-input-placeholder': 'Geben Sie einen mathematischen Ausdruck oder eine Frage ein...',
          'user-solution-placeholder': 'Geben Sie Ihre Lösung zur Überprüfung ein...',
          'ocr-corrected-placeholder': 'Korrigieren Sie den erkannten Text hier...'
        }
      };

      // Функция установки языка
      window.setLanguage = function(lang) {
        appState.currentLanguage = lang;
        localStorage.setItem('vip-language', lang);
        
        // Обновляем интерфейс
        updateInterfaceLanguage(lang);
        
        // Закрываем выпадающее меню
        document.getElementById('language-dropdown').classList.remove('active');
      };

      // Функция обновления языка интерфейса
      function updateInterfaceLanguage(lang) {
        const langData = translations[lang] || translations.ru;
        
        // Обновляем все элементы с data-lang-key
        document.querySelectorAll('[id]').forEach(element => {
          const key = element.id;
          if (langData[key]) {
            if (element.tagName === 'INPUT' && element.type === 'text') {
              element.placeholder = langData[key];
            } else {
              element.textContent = langData[key];
            }
          }
        });
        
        // Обновляем текущий язык в переключателе
        document.getElementById('current-language').textContent = langData['current-language'];
      }

      // Функция переключения выпадающего меню языков
      window.toggleLanguageDropdown = function() {
        document.getElementById('language-dropdown').classList.toggle('active');
      };

      // Закрытие выпадающего меню при клике вне его
      document.addEventListener('click', function(event) {
        const dropdown = document.getElementById('language-dropdown');
        const button = document.querySelector('.language-btn');
        if (!button.contains(event.target) && !dropdown.contains(event.target)) {
          dropdown.classList.remove('active');
        }
      });

      // Загружаем сохраненный язык
      const savedLanguage = localStorage.getItem('vip-language') || 'ru';
      setLanguage(savedLanguage);

      // Установка активного инструмента
      window.setActiveTool = function(tool) {
        appState.activeTool = tool;
        document.getElementById('active-tool-title').textContent = getToolTitle(tool);
        
        // Скрываем все дополнительные контейнеры
        document.getElementById('vip-steps-container').style.display = 'none';
        document.getElementById('vip-plot-container').style.display = 'none';
        document.getElementById('vip-problem-generator').style.display = 'none';
        
        // Показываем нужный контейнер
        if (tool === 'graph') {
          document.getElementById('vip-plot-container').style.display = 'block';
        } else if (tool === 'generator') {
          document.getElementById('vip-problem-generator').style.display = 'grid';
        }

        // На мобильных устройствах закрываем меню после выбора инструмента
        if (window.innerWidth <= 768) {
          toggleMobileMenu();
        }
      }
      
      // Получение заголовка для инструмента
      function getToolTitle(tool) {
        const titles = {
          'calculator': 'Калькулятор',
          'graph': 'Построитель графиков',
          'equation': 'Решение уравнений',
          'derivative': 'Вычисление производных',
          'integral': 'Вычисление интегралов',
          'matrix': 'Работа с матрицами',
          'generator': 'Генератор задач'
        };
        return titles[tool] || 'Математический ассистент';
      }
      
      // Вставка специальных символов
      window.insertSymbol = function(symbol) {
        const input = document.getElementById('math-input');
        input.value += symbol;
        input.focus();
      }
      
      // Обработка нажатия Enter
      window.handleKeyPress = function(event) {
        if (event.key === 'Enter') {
          solveMath();
        }
      }
      
      // Основная функция решения математических выражений
      window.solveMath = function() {
        const input = document.getElementById('math-input');
        const expression = input.value.trim();
        
        if (!expression) return;
        
        // Добавляем сообщение пользователя в чат
        addVIPMessage(expression, 'user');
        
        // Очищаем поле ввода
        input.value = '';
        
        // Обрабатываем выражение в зависимости от активного инструмента
        try {
          let result;
          switch (appState.activeTool) {
            case 'calculator':
              result = evaluateExpression(expression);
              break;
            case 'graph':
              result = plotFunction(expression);
              break;
            case 'equation':
              result = solveEquation(expression);
              break;
            case 'derivative':
              result = calculateDerivative(expression);
              break;
            case 'integral':
              result = calculateIntegral(expression);
              break;
            case 'matrix':
              result = evaluateMatrix(expression);
              break;
            default:
              result = evaluateExpression(expression);
          }
          
          // Добавляем ответ в чат
          addVIPMessage(result, 'bot');
          
          // Сохраняем в историю
          appState.history.push({ expression, result });
        } catch (error) {
          addVIPMessage(`Ошибка: ${error.message}`, 'bot');
        }
      }
      
      // Вычисление математического выражения
      function evaluateExpression(expr) {
        try {
          // Показываем пошаговое решение, если включено
          if (appState.stepByStepSolutions) {
            const steps = generateSteps(expr);
            showSteps(steps);
          }
          
          const result = math.evaluate(expr);
          return `Результат: ${expr} = ${result}`;
        } catch (error) {
          throw new Error('Некорректное выражение');
        }
      }
      
      // Генерация пошагового решения
      function generateSteps(expr) {
        const steps = [];
        
        try {
          // Шаг 1: Исходное выражение
          steps.push(`Вычисляем выражение: ${expr}`);
          
          // Шаг 2: Вычисление
          const result = math.evaluate(expr);
          steps.push(`Результат: ${result}`);
          
          // Дополнительные шаги в зависимости от выражения
          if (expr.includes('+')) {
            steps.push('Сложение: складываем числа');
          }
          if (expr.includes('-')) {
            steps.push('Вычитание: вычитаем числа');
          }
          if (expr.includes('*')) {
            steps.push('Умножение: умножаем числа');
          }
          if (expr.includes('/')) {
            steps.push('Деление: делим числа');
          }
          if (expr.includes('^')) {
            steps.push('Возведение в степень: вычисляем степень');
          }
        } catch (error) {
          steps.push('Ошибка вычисления: некорректное выражение');
        }
        
        return steps;
      }
      
      // Показ пошагового решение
      function showSteps(steps) {
        const container = document.getElementById('vip-steps-container');
        container.innerHTML = '<h3>Пошаговое решение:</h3>';
        
        steps.forEach((step, index) => {
          const stepEl = document.createElement('div');
          stepEl.className = 'vip-step';
          stepEl.textContent = `${index + 1}. ${step}`;
          container.appendChild(stepEl);
        });
        
        container.style.display = 'block';
      }
      
      // Построение графика функции
      function plotFunction(expr) {
        try {
          // Подготовка данных для графика
          const xValues = [];
          const yValues = [];
          
          for (let x = -10; x <= 10; x += 0.1) {
            try {
              const y = math.evaluate(expr.replace(/x/g, `(${x})`));
              xValues.push(x);
              yValues.push(y);
            } catch (e) {
              // Пропускаем точки, которые не могут быть вычислены
            }
          }
          
          // Построение графика с помощью Plotly
          const plotData = [{
            x: xValues,
            y: yValues,
            type: 'scatter',
            mode: 'lines',
            line: { color: '#4ecdc4', width: 2 }
          }];
          
          const layout = {
            plot_bgcolor: '#1a1c2b',
            paper_bgcolor: '#1a1c2b',
            font: { color: '#f7f9fc' },
            xaxis: { gridcolor: '#2a2d43' },
            yaxis: { gridcolor: '#2a2d43' },
            margin: { t: 30 }
          };
          
          Plotly.newPlot('vip-plot-container', plotData, layout);
          
          return `График функции: ${expr}`;
        } catch (error) {
          throw new Error('Невозможно построить график для этого выражения');
        }
      }
      
      // Решение уравнений
      function solveEquation(expr) {
        try {
          // Упрощенная реализация для демонстрации
          const solution = math.evaluate(expr);
          return `Решение уравнения: ${expr} = ${solution}`;
        } catch (error) {
          throw new Error('Невозможно решить это уравнение');
        }
      }
      
      // Вычисление производных
      function calculateDerivative(expr) {
        try {
          // Упрощенная реализация для демонстрации
          const derivative = math.derivative(expr, 'x').toString();
          return `Производная от ${expr} равна: ${derivative}`;
        } catch (error) {
          throw new Error('Невозможно вычислить производную');
        }
      }
      
      // Вычисление интегралов
      function calculateIntegral(expr) {
        try {
          // Упрощенная реализация для демонстрации
          const integral = math.integral(expr, 'x').toString();
          return `Интеграл от ${expr} равен: ${integral}`;
        } catch (error) {
          throw new Error('Невозможно вычислить интеграл');
        }
      }
      
      // Работа с матрицами
      function evaluateMatrix(expr) {
        try {
          const result = math.evaluate(expr);
          return `Результат операции с матрицами: ${result}`;
        } catch (error) {
          throw new Error('Некорректная матричная операция');
        }
      }
      
      // Генерация задач
      window.generateProblem = function(type) {
        let problem = '';
        let solution = '';
        
        switch (type) {
          case 'arithmetic':
            const a = Math.floor(Math.random() * 100);
            const b = Math.floor(Math.random() * 100);
            const operators = ['+', '-', '*', '/'];
            const op = operators[Math.floor(Math.random() * operators.length)];
            
            problem = `Решите: ${a} ${op} ${b}`;
            solution = math.evaluate(`${a} ${op} ${b}`);
            break;
            
          case 'algebra':
            const x = Math.floor(Math.random() * 10) + 1;
            const c = Math.floor(Math.random() * 10) + 1;
            const d = Math.floor(Math.random() * 10) + 1;
            
            problem = `Решите уравнение: ${c}x + ${d} = ${c*x + d}`;
            solution = `x = ${x}`;
            break;
            
          case 'geometry':
            const radius = Math.floor(Math.random() * 10) + 1;
            problem = `Найдите площадь круга с радиусом ${radius}`;
            solution = Math.PI * radius * radius;
            break;
            
          case 'advanced':
            const funcs = ['sin', 'cos', 'tan'];
            const func = funcs[Math.floor(Math.random() * funcs.length)];
            const angle = Math.floor(Math.random() * 360);
            
            problem = `Вычислите ${func}(${angle}°)`;
            solution = math.evaluate(`${func}(${angle} deg)`);
            break;
        }
        
        document.getElementById(`${type}-problem`).textContent = problem;
        
        // Сохраняем решение для последующей проверки
        document.getElementById(`${type}-problem`).dataset.solution = solution;
        
        return problem;
      }
      
      // Проверка решения пользователя
      window.checkSolution = function() {
        const userSolution = document.getElementById('user-solution').value;
        const problemType = appState.activeTool === 'generator' ? 'arithmetic' : appState.activeTool;
        const correctSolution = document.getElementById(`${problemType}-problem`).dataset.solution;
        
        if (!userSolution) {
          document.getElementById('verification-result').innerHTML = '<p>Введите решение для проверки</p>';
          return;
        }
        
        let resultHTML = '';
        try {
          const userResult = math.evaluate(userSolution);
          const correctResult = math.evaluate(correctSolution);
          
          if (math.abs(userResult - correctResult) < 0.001) {
            resultHTML = `<p style="color: #4ecdc4;">✓ Верно! Ваше решение правильное.</p>`;
          } else {
            resultHTML = `<p style="color: #ff6b6b;">✗ Неверно. Правильный ответ: ${correctSolution}</p>`;
          }
        } catch (error) {
          resultHTML = `<p style="color: #ff6b6b;">Ошибка: Невозможно проверить это решение</p>`;
        }
        
        document.getElementById('verification-result').innerHTML = resultHTML;
      }
      
      // Добавление сообщения в VIP-чат
      function addVIPMessage(text, sender) {
        const container = document.getElementById('vip-chat-container');
        const messageEl = document.createElement('div');
        messageEl.className = `vip-message vip-${sender}-message`;
        messageEl.textContent = text;
        container.appendChild(messageEl);
        container.scrollTop = container.scrollHeight;
      }
      
      // Экспорт в PDF
      window.exportToPDF = function() {
        addVIPMessage('Функция экспорта в PDF активирована. В premium-версии эта функция доступна для всех ваших решений.', 'bot');
      }
      
      // Показ модального окна premium
      window.showPremiumModal = function() {
        addVIPMessage('XIAI Pro уже работает в premium-режиме! Все функции доступны без ограничений.', 'bot');
      }
      
      // ========== УЛУЧШЕННАЯ ФУНКЦИЯ РАСПОЗНАВАНИЯ ИЗОБРАЖЕНИЙ ==========
      // Обработчик для загрузки изображений
      const imageUpload = document.getElementById('math-image-upload');
      const ocrLoading = document.getElementById('ocr-loading');
      const imagePreview = document.getElementById('image-preview');
      const ocrCorrection = document.getElementById('ocr-correction');
      const ocrCorrectedText = document.getElementById('ocr-corrected-text');
      
      if (imageUpload) {
        imageUpload.addEventListener('change', function(e) {
          const file = e.target.files[0];
          if (!file) return;
          
          // Показываем превью изображения
          imagePreview.style.display = 'block';
          imagePreview.src = URL.createObjectURL(file);
          
          // Показываем индикатор загрузки
          ocrLoading.style.display = 'block';
          ocrCorrection.style.display = 'none';
          
          // Используем Tesseract.js для распознавания текста
          Tesseract.recognize(
            file,
            'eng', // Используем только английский для лучшего распознавания математики
            { 
              logger: m => console.log(m),
              // Специфичные настройки для математических выражений
              tessedit_pageseg_mode: Tesseract.PSM.SINGLE_BLOCK,
              tessedit_char_whitelist: '0123456789+-×÷=(){}[].,|/\\*^%$#@!?&abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
            }
          ).then(({ data: { text } }) => {
            // Обрабатываем распознанный текст
            let processedText = preprocessOCRText(text);
            
            // Скрываем индикатор загрузки
            ocrLoading.style.display = 'none';
            
            // Показываем поле для корректировки
            ocrCorrection.style.display = 'block';
            ocrCorrectedText.value = processedText;
            
            // Показываем пользователю что было распознано
            addVIPMessage(`Распознано: ${processedText}. Проверьте и откорректируйте при необходимости.`, 'bot');
          }).catch(err => {
            console.error('Ошибка распознавания:', err);
            ocrLoading.style.display = 'none';
            addVIPMessage('Не удалось распознать текст на изображении. Попробуйте другое изображение или введите выражение вручную.', 'bot');
          });
        });
      }
      
      // Функция использования исправленного текста
      window.useCorrectedText = function() {
        const correctedText = ocrCorrectedText.value.trim();
        if (!correctedText) return;
        
        // Вставляем исправленный текст в поле ввода
        document.getElementById('math-input').value = correctedText;
        
        // Скрываем поле корректировки
        ocrCorrection.style.display = 'none';
        
        // Автоматически решаем пример
        solveMath();
      }
      
      // Функция предварительной обработки распознанного текста
      function preprocessOCRText(text) {
        // Удаляем лишние пробелы и символы
        let processed = text.trim();
        
        // Заменяем commonly misrecognized characters
        processed = processed
          .replace(/[oO]/g, '0') // иногда '0' распознается как 'o' или 'O'
          .replace(/[lI]/g, '1') // '1' как 'I' или 'l'
          .replace(/[zZ]/g, '2') // '2' как 'Z'
          .replace(/[а-яА-Я]/g, '') // удаляем русские буквы
          .replace(/[xх×]/gi, '*')
          .replace(/[÷:]/gi, '/')
          .replace(/\s+/g, '') // удаляем все пробелы
          .replace(/[^0-9+\-*/().^π√]/g, ''); // оставляем только математические символы
        
        // Улучшаем распознавание сложных выражений
        processed = processed.replace(/(\d)([a-zA-Z])/g, '$1*$2'); // добавляем * между цифрами и буквами
        processed = processed.replace(/([a-zA-Z])(\d)/g, '$1*$2'); // добавляем * между буквами и цифрами
        processed = processed.replace(/(\))(\()/g, '$1*$2'); // добавляем * между скобками
        
        return processed;
      }
      
      // Инициализация VIP-режима при загрузке
      // Генерируем примеры задач при загрузке
      generateProblem('arithmetic');
      generateProblem('algebra');
      generateProblem('geometry');
      generateProblem('advanced');
      
      // Добавляем приветственное сообщение
      addVIPMessage('Готов к работе! Вы можете решать сложные математические задачи, строить графики и многое другое.', 'bot');
    }
  </script>
</body>
</html>
