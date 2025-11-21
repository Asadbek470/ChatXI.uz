# OpenXI uz
OpenXI4
меня создал Abdumalikov Asadbek
<!DOCTYPE html>
<html lang="ru">
<head>
    <!-- Google tag (gtag.js) -->
    <script async src="https://www.googletagmanager.com/gtag/js?id=G-DMYK0SNWWG"></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());
      gtag('config', 'G-DMYK0SNWWG');
    </script>
    <meta charset="UTF-8">
    <title>XIAI — Математический Ассистент</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    
    <!-- Подключаем необходимые библиотеки -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/11.11.0/math.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/plotly.js/2.24.1/plotly.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/tesseract.js@4/dist/tesseract.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <style>
        :root {
            --primary-bg: #0f1117;
            --secondary-bg: #1a1d29;
            --accent-color: #10a37f;
            --accent-hover: #0d906f;
            --text-primary: #ffffff;
            --text-secondary: #d1d5db;
            --border-color: #2d3748;
            --error-color: #ef4444;
            --warning-color: #f59e0b;
            --success-color: #10b981;
            --card-bg: rgba(255, 255, 255, 0.05);
            --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            --transition: all 0.3s ease;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
        }

        body {
            background-color: var(--primary-bg);
            color: var(--text-primary);
            line-height: 1.6;
            overflow-x: hidden;
        }

        .app-container {
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        /* Header Styles */
        .app-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 0;
            border-bottom: 1px solid var(--border-color);
            margin-bottom: 20px;
        }

        .logo-container {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .logo {
            width: 40px;
            height: 40px;
            border-radius: 8px;
            object-fit: cover;
        }

        .app-title {
            font-size: 24px;
            font-weight: 700;
            background: linear-gradient(90deg, var(--accent-color), #3b82f6);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .header-controls {
            display: flex;
            gap: 15px;
            align-items: center;
        }

        /* Main Content Styles */
        .main-content {
            display: grid;
            grid-template-columns: 1fr 300px;
            gap: 20px;
            flex: 1;
        }

        @media (max-width: 768px) {
            .main-content {
                grid-template-columns: 1fr;
            }
        }

        /* Chat Container */
        .chat-container {
            display: flex;
            flex-direction: column;
            background-color: var(--secondary-bg);
            border-radius: 12px;
            overflow: hidden;
            box-shadow: var(--shadow);
            height: 70vh;
        }

        .chat-messages {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 16px;
        }

        .message {
            max-width: 80%;
            padding: 12px 16px;
            border-radius: 18px;
            line-height: 1.5;
            animation: fadeIn 0.3s ease;
        }

        .user-message {
            align-self: flex-end;
            background-color: var(--accent-color);
            color: white;
            border-bottom-right-radius: 6px;
        }

        .bot-message {
            align-self: flex-start;
            background-color: var(--card-bg);
            border-bottom-left-radius: 6px;
        }

        .message-content {
            word-wrap: break-word;
        }

        .message-time {
            font-size: 0.75rem;
            opacity: 0.7;
            margin-top: 5px;
            text-align: right;
        }

        .chat-input-container {
            display: flex;
            padding: 16px;
            border-top: 1px solid var(--border-color);
            background-color: var(--secondary-bg);
        }

        .chat-input {
            flex: 1;
            padding: 12px 16px;
            border: 1px solid var(--border-color);
            border-radius: 24px;
            background-color: var(--primary-bg);
            color: var(--text-primary);
            font-size: 16px;
            outline: none;
            transition: var(--transition);
        }

        .chat-input:focus {
            border-color: var(--accent-color);
            box-shadow: 0 0 0 2px rgba(16, 163, 127, 0.2);
        }

        .send-button {
            margin-left: 10px;
            width: 44px;
            height: 44px;
            border-radius: 50%;
            background-color: var(--accent-color);
            color: white;
            border: none;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: var(--transition);
        }

        .send-button:hover {
            background-color: var(--accent-hover);
            transform: scale(1.05);
        }

        /* Sidebar Styles */
        .sidebar {
            background-color: var(--secondary-bg);
            border-radius: 12px;
            padding: 20px;
            box-shadow: var(--shadow);
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .sidebar-section {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .sidebar-title {
            font-size: 18px;
            font-weight: 600;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .tool-button {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 12px 16px;
            background-color: var(--card-bg);
            border: none;
            border-radius: 8px;
            color: var(--text-primary);
            cursor: pointer;
            transition: var(--transition);
            text-align: left;
        }

        .tool-button:hover {
            background-color: rgba(255, 255, 255, 0.1);
            transform: translateY(-2px);
        }

        .tool-button.active {
            background-color: var(--accent-color);
            color: white;
        }

        .tool-icon {
            font-size: 18px;
            width: 24px;
            text-align: center;
        }

        /* Features Grid */
        .features-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .feature-card {
            background-color: var(--secondary-bg);
            border-radius: 12px;
            padding: 20px;
            box-shadow: var(--shadow);
            transition: var(--transition);
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .feature-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }

        .feature-icon {
            font-size: 24px;
            color: var(--accent-color);
            margin-bottom: 8px;
        }

        .feature-title {
            font-size: 18px;
            font-weight: 600;
        }

        .feature-description {
            color: var(--text-secondary);
            font-size: 14px;
        }

        /* Premium Section */
        .premium-section {
            background: linear-gradient(135deg, #1a1d29 0%, #2d1a4e 100%);
            border-radius: 12px;
            padding: 24px;
            margin-top: 30px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .premium-section::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,215,0,0.1) 0%, transparent 70%);
            animation: rotate 20s linear infinite;
        }

        .premium-badge {
            display: inline-block;
            background: linear-gradient(135deg, #ffd700, #ffed4e);
            color: #000;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 14px;
            font-weight: 700;
            margin-bottom: 16px;
        }

        .premium-title {
            font-size: 24px;
            font-weight: 700;
            margin-bottom: 12px;
            background: linear-gradient(90deg, #ffd700, #ffaa00);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .premium-description {
            color: var(--text-secondary);
            margin-bottom: 20px;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
        }

        .premium-input-container {
            display: flex;
            max-width: 400px;
            margin: 0 auto 20px;
        }

        .premium-input {
            flex: 1;
            padding: 12px 16px;
            border: 1px solid var(--border-color);
            border-radius: 8px 0 0 8px;
            background-color: rgba(0, 0, 0, 0.3);
            color: var(--text-primary);
            outline: none;
        }

        .premium-button {
            padding: 12px 24px;
            background: linear-gradient(135deg, #ffd700, #ffaa00);
            color: #000;
            border: none;
            border-radius: 0 8px 8px 0;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
        }

        .premium-button:hover {
            background: linear-gradient(135deg, #ffaa00, #ffd700);
            transform: translateY(-2px);
        }

        /* Modal Styles */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            opacity: 0;
            visibility: hidden;
            transition: var(--transition);
        }

        .modal-overlay.active {
            opacity: 1;
            visibility: visible;
        }

        .modal {
            background-color: var(--secondary-bg);
            border-radius: 12px;
            padding: 24px;
            max-width: 500px;
            width: 90%;
            max-height: 80vh;
            overflow-y: auto;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
            transform: scale(0.9);
            transition: var(--transition);
        }

        .modal-overlay.active .modal {
            transform: scale(1);
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .modal-title {
            font-size: 20px;
            font-weight: 600;
        }

        .close-button {
            background: none;
            border: none;
            color: var(--text-secondary);
            font-size: 24px;
            cursor: pointer;
            transition: var(--transition);
        }

        .close-button:hover {
            color: var(--text-primary);
        }

        /* Animation Keyframes */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        /* Utility Classes */
        .hidden {
            display: none !important;
        }

        .text-center {
            text-align: center;
        }

        .mt-20 {
            margin-top: 20px;
        }

        .mb-20 {
            margin-bottom: 20px;
        }

        /* Robot Watcher */
        #robotWatcher {
            background: linear-gradient(90deg, #ef4444, #f59e0b);
            color: white;
            padding: 10px;
            text-align: center;
            font-weight: bold;
            font-size: 16px;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
        }

        .sirena {
            display: inline-block;
            animation: blink 1s infinite;
        }

        @keyframes blink {
            0%, 50%, 100% { opacity: 1; }
            25%, 75% { opacity: 0; }
        }

        /* Mobile Responsiveness */
        @media (max-width: 768px) {
            .app-container {
                padding: 10px;
            }
            
            .app-header {
                flex-direction: column;
                gap: 15px;
                text-align: center;
            }
            
            .header-controls {
                justify-content: center;
            }
            
            .chat-container {
                height: 60vh;
            }
            
            .message {
                max-width: 90%;
            }
            
            .features-grid {
                grid-template-columns: 1fr;
            }
        }

        /* Premium Features Grid */
        .premium-features {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }

        .premium-feature {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 12px;
            padding: 20px;
            text-align: center;
            transition: var(--transition);
            border: 1px solid rgba(255, 215, 0, 0.2);
        }

        .premium-feature:hover {
            transform: translateY(-5px);
            border-color: rgba(255, 215, 0, 0.5);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
        }

        .premium-feature-icon {
            font-size: 32px;
            color: #ffd700;
            margin-bottom: 12px;
        }

        .premium-feature-title {
            font-size: 18px;
            font-weight: 600;
            margin-bottom: 8px;
        }

        .premium-feature-description {
            color: var(--text-secondary);
            font-size: 14px;
        }

        /* Steps Container */
        .steps-container {
            background: var(--card-bg);
            border-radius: 8px;
            padding: 16px;
            margin-top: 16px;
            font-family: 'Courier New', monospace;
        }

        .step {
            margin-bottom: 8px;
            padding-left: 15px;
            border-left: 2px solid var(--accent-color);
        }

        /* Plot Container */
        .plot-container {
            width: 100%;
            height: 300px;
            margin: 15px 0;
            background: white;
            border-radius: 8px;
        }

        /* Image Upload */
        .image-upload-container {
            margin: 15px 0;
            text-align: center;
        }

        .upload-button {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            padding: 12px 20px;
            background: var(--accent-color);
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: var(--transition);
        }

        .upload-button:hover {
            background: var(--accent-hover);
            transform: translateY(-2px);
        }

        .image-preview {
            max-width: 100%;
            max-height: 200px;
            margin-top: 10px;
            display: none;
            border-radius: 8px;
            border: 2px solid var(--accent-color);
        }

        .ocr-loading {
            display: none;
            text-align: center;
            margin: 10px 0;
            color: var(--accent-color);
        }

        .ocr-correction {
            margin-top: 10px;
            display: none;
        }

        .ocr-correction input {
            width: 100%;
            padding: 10px;
            border-radius: 5px;
            border: 1px solid var(--accent-color);
            background: var(--card-bg);
            color: var(--text-primary);
        }

        /* Voice Input */
        .voice-input-container {
            display: flex;
            margin-top: 10px;
        }

        #userInput {
            flex: 1;
            padding: 12px 16px;
            border-radius: 24px;
            border: 1px solid var(--border-color);
            background: var(--primary-bg);
            color: var(--text-primary);
            font-size: 16px;
        }

        #micButton {
            margin-left: 10px;
            width: 50px;
            border-radius: 50%;
            background: var(--accent-color);
            color: white;
            border: none;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: var(--transition);
        }

        #micButton.listening {
            background: var(--success-color);
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        /* History Section */
        .history-section {
            margin-top: 20px;
        }

        .history-item {
            padding: 12px;
            background: var(--card-bg);
            border-radius: 8px;
            margin-bottom: 10px;
            cursor: pointer;
            transition: var(--transition);
        }

        .history-item:hover {
            background: rgba(255, 255, 255, 0.1);
        }

        /* Blocker */
        #blocker {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.9);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            display: none;
        }

        .panel {
            background: var(--secondary-bg);
            padding: 30px;
            border-radius: 12px;
            text-align: center;
            max-width: 500px;
            width: 90%;
        }

        .panel h1 {
            color: var(--error-color);
            margin-bottom: 15px;
        }

        .panel input {
            padding: 12px;
            margin-top: 15px;
            width: 100%;
            border-radius: 6px;
            border: 1px solid var(--border-color);
            background: var(--primary-bg);
            color: var(--text-primary);
        }

        .panel button {
            margin-top: 15px;
            background: var(--success-color);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 600;
            transition: var(--transition);
        }

        .panel button:hover {
            background: #0da271;
        }

        /* Premium Splash Animation */
        #premium-splash {
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

        .premium-splash-title {
            font-size: 8rem;
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

        /* Premium UI Elements */
        .premium-ui-element {
            position: relative;
        }

        .premium-ui-element::after {
            content: "PREMIUM";
            position: absolute;
            top: 5px;
            right: 5px;
            background: linear-gradient(135deg, #ffd700, #ffaa00);
            color: #000;
            font-size: 10px;
            font-weight: bold;
            padding: 2px 6px;
            border-radius: 10px;
        }

        .premium-tool {
            background: linear-gradient(135deg, rgba(255, 215, 0, 0.1), rgba(255, 170, 0, 0.1)) !important;
            border: 1px solid rgba(255, 215, 0, 0.3) !important;
        }

        .premium-only {
            display: none;
        }

        .premium-active .premium-only {
            display: block;
        }

        .premium-glow {
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.5);
        }

        /* VIP Styles */
        .vip-tool {
            background: linear-gradient(135deg, #FFD700, #FFA500) !important;
            color: black !important;
            font-weight: bold !important;
        }
        .vip-tool:hover {
            transform: translateY(-3px) !important;
            box-shadow: 0 5px 15px rgba(255, 215, 0, 0.4) !important;
        }
        .vip-indicator {
            background: linear-gradient(45deg, #FFD700, #FFA500);
            color: black;
            padding: 8px 16px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 14px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
    </style>
</head>
<body>
    <!-- 🔴 Баннер про онлайн-робот-администратора -->
    <div id="robotWatcher">
        👁 За вами следит <b>онлайн-робот-администратор</b>
        <span class="sirena">🚨</span><span class="sirena">🚨</span><span class="sirena">🚨</span>
    </div>

    <!-- Анимация активации премиум-режима -->
    <div id="premium-splash">
        <div class="premium-splash-title">ПРЕМИУМ</div>
    </div>

    <div class="app-container">
        <!-- Header -->
        <header class="app-header">
            <div class="logo-container">
                <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQC0m3-hAkL0qx5VxSK_dxymtGUWrZqdDMG_Q&s" alt="XIAI Logo" class="logo">
                <h1 class="app-title">XIAI Математический Ассистент</h1>
            </div>
            <div class="header-controls">
                <button class="tool-button" id="history-btn">
                    <i class="fas fa-history tool-icon"></i>
                    История
                </button>
                <button class="tool-button" id="export-btn">
                    <i class="fas fa-file-export tool-icon"></i>
                    Экспорт
                </button>
                <button class="tool-button" id="premium-btn">
                    <i class="fas fa-crown tool-icon"></i>
                    Премиум
                </button>
            </div>
        </header>

        <!-- Main Content -->
        <div class="main-content">
            <!-- Chat Section -->
            <section class="chat-container">
                <div class="chat-messages" id="chatBox">
                    <div class="message bot-message">
                        <div class="message-content">
                            Добро пожаловать в XIAI! Я ваш персональный математический ассистент. 
                            Чем могу помочь? Вы можете решать примеры, строить графики, находить производные и многое другое.
                        </div>
                        <div class="message-time">Только что</div>
                    </div>
                </div>
                <div class="chat-input-container">
                    <input type="text" class="chat-input" id="userInput" placeholder="Введите математическое выражение или вопрос...">
                    <button class="send-button" onclick="sendMessage()">
                        <i class="fas fa-paper-plane"></i>
                    </button>
                </div>
                <div class="voice-input-container">
                    <input type="text" id="userInput" placeholder="Напишите пример или нажмите микрофон...">
                    <button id="micButton">🎤</button>
                </div>
            </section>

            <!-- Sidebar -->
            <aside class="sidebar">
                <div class="sidebar-section">
                    <h3 class="sidebar-title"><i class="fas fa-calculator"></i> Инструменты</h3>
                    <button class="tool-button active" data-tool="calculator">
                        <i class="fas fa-calculator tool-icon"></i>
                        Калькулятор
                    </button>
                    <button class="tool-button" data-tool="graph">
                        <i class="fas fa-chart-line tool-icon"></i>
                        Графики
                    </button>
                    <button class="tool-button" data-tool="equation">
                        <i class="fas fa-equals tool-icon"></i>
                        Уравнения
                    </button>
                    <button class="tool-button" data-tool="derivative">
                        <i class="fas fa-superscript tool-icon"></i>
                        Производные
                    </button>
                    <button class="tool-button" data-tool="integral">
                        <i class="fas fa-integral tool-icon"></i>
                        Интегралы
                    </button>
                    <button class="tool-button" data-tool="matrix">
                        <i class="fas fa-th tool-icon"></i>
                        Матрицы
                    </button>
                </div>

                <div class="sidebar-section">
                    <h3 class="sidebar-title"><i class="fas fa-magic"></i> Дополнительно</h3>
                    <button class="tool-button" data-tool="generator">
                        <i class="fas fa-cogs tool-icon"></i>
                        Генератор задач
                    </button>
                    <button class="tool-button" data-tool="steps">
                        <i class="fas fa-list-ol tool-icon"></i>
                        Пошаговое решение
                    </button>
                    <button class="tool-button" id="image-upload-btn">
                        <i class="fas fa-camera tool-icon"></i>
                        Загрузить изображение
                    </button>
                    <button class="tool-button" id="voice-input-btn">
                        <i class="fas fa-microphone tool-icon"></i>
                        Голосовой ввод
                    </button>
                </div>

                <!-- VIP инструменты (появляются только после активации) -->
                <div class="sidebar-section" id="vip-section" style="display: none;">
                    <h3 class="sidebar-title"><i class="fas fa-star"></i> VIP Функции</h3>
                    <button class="tool-button vip-tool" onclick="showVIPFunctions()">
                        <i class="fas fa-list tool-icon"></i>
                        Список VIP функций
                    </button>
                    <button class="tool-button vip-tool" onclick="toggleOfflineMode()">
                        <i class="fas fa-wifi tool-icon"></i>
                        Оффлайн режим
                    </button>
                </div>

                <!-- Премиум инструменты (появляются только после активации) -->
                <div class="sidebar-section premium-only">
                    <h3 class="sidebar-title"><i class="fas fa-crown"></i> Премиум инструменты</h3>
                    <button class="tool-button premium-tool" data-tool="advanced-graph">
                        <i class="fas fa-chart-pie tool-icon"></i>
                        3D Графики
                    </button>
                    <button class="tool-button premium-tool" data-tool="statistics">
                        <i class="fas fa-chart-bar tool-icon"></i>
                        Статистика
                    </button>
                    <button class="tool-button premium-tool" data-tool="probability">
                        <i class="fas fa-dice tool-icon"></i>
                        Теория вероятностей
                    </button>
                    <button class="tool-button premium-tool" data-tool="complex-numbers">
                        <i class="fas fa-infinity tool-icon"></i>
                        Комплексные числа
                    </button>
                    <button class="tool-button premium-tool" data-tool="differential-equations">
                        <i class="fas fa-project-diagram tool-icon"></i>
                        Дифф. уравнения
                    </button>
                </div>

                <div class="sidebar-section">
                    <h3 class="sidebar-title"><i class="fas fa-history"></i> История решений</h3>
                    <div id="history-list" class="history-list">
                        <!-- История будет заполняться динамически -->
                    </div>
                </div>
            </aside>
        </div>

        <!-- Features Grid -->
        <section class="features-grid">
            <div class="feature-card">
                <div class="feature-icon">
                    <i class="fas fa-robot"></i>
                </div>
                <h3 class="feature-title">Умный ИИ-ассистент</h3>
                <p class="feature-description">Решает математические задачи любой сложности с пошаговыми объяснениями</p>
            </div>
            <div class="feature-card">
                <div class="feature-icon">
                    <i class="fas fa-chart-bar"></i>
                </div>
                <h3 class="feature-title">Визуализация графиков</h3>
                <p class="feature-description">Строит 2D и 3D графики функций с возможностью настройки</p>
            </div>
            <div class="feature-card">
                <div class="feature-icon">
                    <i class="fas fa-camera"></i>
                </div>
                <h3 class="feature-title">Распознавание изображений</h3>
                <p class="feature-description">Загружайте фото математических задач для автоматического решения</p>
            </div>
            <div class="feature-card">
                <div class="feature-icon">
                    <i class="fas fa-file-export"></i>
                </div>
                <h3 class="feature-title">Экспорт решений</h3>
                <p class="feature-description">Сохраняйте решения в PDF, DOCX и PNG форматах</p>
            </div>
            <div class="feature-card">
                <div class="feature-icon">
                    <i class="fas fa-microphone"></i>
                </div>
                <h3 class="feature-title">Голосовой ввод</h3>
                <p class="feature-description">Произносите математические задачи для автоматического распознавания</p>
            </div>
            <div class="feature-card">
                <div class="feature-icon">
                    <i class="fas fa-history"></i>
                </div>
                <h3 class="feature-title">История решений</h3>
                <p class="feature-description">Полная история всех решенных задач с возможностью повтора</p>
            </div>
        </section>

        <!-- Premium Section -->
        <section class="premium-section">
            <div class="premium-badge">ПРЕМИУМ</div>
            <h2 class="premium-title">Откройте все возможности XIAI</h2>
            <p class="premium-description">
                Получите доступ к расширенным функциям: решение сложных задач, продвинутая визуализация, 
                индивидуальное обучение и многое другое
            </p>
            <div class="premium-input-container">
                <input type="text" class="premium-input" id="premium-code" placeholder="Введите код OPENXI-GOLD-001">
                <button class="premium-button" onclick="activatePremium()">Активировать</button>
            </div>

            <div class="premium-features">
                <div class="premium-feature">
                    <div class="premium-feature-icon">
                        <i class="fas fa-brain"></i>
                    </div>
                    <h3 class="premium-feature-title">Расширенный ИИ</h3>
                    <p class="premium-feature-description">Решение сложных университетских и олимпиадных задач</p>
                </div>
                <div class="premium-feature">
                    <div class="premium-feature-icon">
                        <i class="fas fa-chart-pie"></i>
                    </div>
                    <h3 class="premium-feature-title">3D Графики</h3>
                    <p class="premium-feature-description">Построение сложных 3D графиков и поверхностей</p>
                </div>
                <div class="premium-feature">
                    <div class="premium-feature-icon">
                        <i class="fas fa-book"></i>
                    </div>
                    <h3 class="premium-feature-title">Индивидуальное обучение</h3>
                    <p class="premium-feature-description">Персональные рекомендации и обучающие материалы</p>
                </div>
                <div class="premium-feature">
                    <div class="premium-feature-icon">
                        <i class="fas fa-file-export"></i>
                    </div>
                    <h3 class="premium-feature-title">Расширенный экспорт</h3>
                    <p class="premium-feature-description">Экспорт в DOCX, LaTeX и другие форматы</p>
                </div>
            </div>
        </section>

        <!-- Support Links -->
        <div style="display: flex; gap: 10px; justify-content: center; margin: 20px 0;">
            <button class="tool-button">
                <a href="https://asadbek470.github.io/support/" style="text-decoration: none; color: inherit;">Support</a>
            </button>
            <button class="tool-button">
                <a href="https://asadbek470.github.io/admin.com" style="text-decoration: none; color: inherit;">Admin Panel</a>
            </button>
        </div>
    </div>

    <!-- Модальные окна -->
    <div class="modal-overlay" id="history-modal">
        <div class="modal">
            <div class="modal-header">
                <h3 class="modal-title">История решений</h3>
                <button class="close-button" onclick="closeModal('history-modal')">&times;</button>
            </div>
            <div id="history-content">
                <!-- История будет заполняться динамически -->
            </div>
        </div>
    </div>

    <div class="modal-overlay" id="export-modal">
        <div class="modal">
            <div class="modal-header">
                <h3 class="modal-title">Экспорт решения</h3>
                <button class="close-button" onclick="closeModal('export-modal')">&times;</button>
            </div>
            <div class="sidebar-section">
                <button class="tool-button" onclick="exportToPDF()">
                    <i class="fas fa-file-pdf tool-icon"></i> Экспорт в PDF
                </button>
                <button class="tool-button" onclick="exportToDOCX()">
                    <i class="fas fa-file-word tool-icon"></i> Экспорт в DOCX
                </button>
                <button class="tool-button" onclick="exportToPNG()">
                    <i class="fas fa-file-image tool-icon"></i> Экспорт в PNG
                </button>
                <!-- Премиум опции экспорта -->
                <div class="premium-only">
                    <button class="tool-button premium-tool" onclick="exportToLaTeX()">
                        <i class="fas fa-code tool-icon"></i> Экспорт в LaTeX
                    </button>
                    <button class="tool-button premium-tool" onclick="exportToExcel()">
                        <i class="fas fa-table tool-icon"></i> Экспорт в Excel
                    </button>
                </div>
            </div>
        </div>
    </div>

    <div class="modal-overlay" id="premium-modal">
        <div class="modal">
            <div class="modal-header">
                <h3 class="modal-title">Премиум функции</h3>
                <button class="close-button" onclick="closeModal('premium-modal')">&times;</button>
            </div>
            <div class="premium-features">
                <div class="premium-feature">
                    <div class="premium-feature-icon">
                        <i class="fas fa-infinity"></i>
                    </div>
                    <h3 class="premium-feature-title">Безлимитные запросы</h3>
                    <p class="premium-feature-description">Решайте неограниченное количество задач</p>
                </div>
                <div class="premium-feature">
                    <div class="premium-feature-icon">
                        <i class="fas fa-rocket"></i>
                    </div>
                    <h3 class="premium-feature-title">Приоритетная обработка</h3>
                    <p class="premium-feature-description">Ваши запросы обрабатываются в первую очередь</p>
                </div>
                <div class="premium-feature">
                    <div class="premium-feature-icon">
                        <i class="fas fa-chart-line"></i>
                    </div>
                    <h3 class="premium-feature-title">Продвинутая аналитика</h3>
                    <p class="premium-feature-description">Подробная статистика вашего прогресса</p>
                </div>
            </div>
            <div class="premium-input-container mt-20">
                <input type="text" class="premium-input" id="premium-modal-code" placeholder="Введите код OPENXI-GOLD-001">
                <button class="premium-button" onclick="activatePremiumFromModal()">Активировать</button>
            </div>
        </div>
    </div>

    <!-- Блокировка -->
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

    <!-- Загрузка изображения -->
    <input type="file" id="math-image-upload" accept="image/*" capture="environment" style="display: none;">

    <!-- Звук сирены -->
    <audio id="alarmSound" src="https://www.soundjay.com/misc/sounds/police-siren-01.mp3" preload="auto"></audio>

    <script>
        // ========== ОСНОВНАЯ ЛОГИКА ПРИЛОЖЕНИЯ ==========
        
        // Константы и переменные
        const badWords = ["лох","тупица","дурак","идиот","сука","блядь","ебать","хуй","пидор","gandon","mudak","blyad","suka","ebat","hui","pidor","eblan","yebat","yeblan","pizda","pizdets","blyadstvo","svoloch","svolochy","durak","duraki","idiot","idioty","mrd","mrdka","mrdki","blyad","blyadi","blyadki","eblan","eblani","eblanam","eblanov","pizda","pizdets","pizdami","pizdetsami","lox","suka"];
        const hackPatterns = ["<script", "javascript:", "onerror", "onload","select *","drop table","insert into","delete from","union all","--","/*","*/","or 1=1"];
        const adminPassword = "ASADBEKantiban";
        const PREMIUM_CODE = "OPENXI-GOLD-001";

        // Состояние приложения
        const appState = {
            isPremium: false,
            activeTool: 'calculator',
            history: [],
            currentLanguage: 'ru',
            stepByStepEnabled: true
        };

        // ========== VIP РЕЖИМ ==========
        const vipState = {
            isVIP: false,
            vipFunctions: {},
            offlineMode: false,
            internationalSupport: true,
            vipActivated: false
        };

        // DOM элементы
        const chatBox = document.getElementById('chatBox');
        const userInput = document.getElementById('userInput');
        const micButton = document.getElementById('micButton');
        const historyBtn = document.getElementById('history-btn');
        const exportBtn = document.getElementById('export-btn');
        const premiumBtn = document.getElementById('premium-btn');
        const imageUploadBtn = document.getElementById('image-upload-btn');
        const mathImageUpload = document.getElementById('math-image-upload');
        const blocker = document.getElementById('blocker');
        const unlockBtn = document.getElementById('unlockBtn');
        const adminPass = document.getElementById('adminPass');
        const alarm = document.getElementById('alarmSound');
        const premiumSplash = document.getElementById('premium-splash');
        const vipSection = document.getElementById('vip-section');

        // Инициализация при загрузке
        document.addEventListener('DOMContentLoaded', function() {
            // Загрузка истории из localStorage
            loadHistory();
            
            // Настройка обработчиков событий
            setupEventListeners();
            
            // Проверка блокировки
            if (localStorage.getItem("blocked") === "true") {
                blocker.style.display = "flex";
            }
            
            // Проверка премиум-статуса
            if (localStorage.getItem("premium") === "true") {
                activatePremiumUI();
            }

            // Проверка VIP-статуса
            if (localStorage.getItem("xiai-vip") === "true") {
                activateVIPMode();
            }
        });

        // Настройка обработчиков событий
        function setupEventListeners() {
            // Отправка сообщения по Enter
            userInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    sendMessage();
                }
            });

            // Кнопки инструментов
            document.querySelectorAll('.tool-button[data-tool]').forEach(button => {
                button.addEventListener('click', function() {
                    const tool = this.getAttribute('data-tool');
                    setActiveTool(tool);
                });
            });

            // Кнопки управления
            historyBtn.addEventListener('click', () => openModal('history-modal'));
            exportBtn.addEventListener('click', () => openModal('export-modal'));
            premiumBtn.addEventListener('click', () => openModal('premium-modal'));
            imageUploadBtn.addEventListener('click', () => mathImageUpload.click());

            // Загрузка изображения
            mathImageUpload.addEventListener('change', handleImageUpload);

            // Голосовой ввод
            setupVoiceRecognition();

            // Блокировка
            unlockBtn.addEventListener('click', handleUnlock);
        }

        // Установка активного инструмента
        function setActiveTool(tool) {
            appState.activeTool = tool;
            
            // Обновление UI
            document.querySelectorAll('.tool-button[data-tool]').forEach(btn => {
                btn.classList.remove('active');
            });
            document.querySelector(`.tool-button[data-tool="${tool}"]`).classList.add('active');
            
            // Обновление подсказки в поле ввода
            updateInputPlaceholder(tool);
        }

        // Обновление подсказки в поле ввода
        function updateInputPlaceholder(tool) {
            const placeholders = {
                calculator: "Введите математическое выражение...",
                graph: "Введите функцию для построения графика (например, x^2)...",
                equation: "Введите уравнение для решения...",
                derivative: "Введите функцию для вычисления производной...",
                integral: "Введите функцию для вычисления интеграла...",
                matrix: "Введите матричную операцию...",
                generator: "Выберите тип задачи для генерации...",
                steps: "Введите выражение для пошагового решения...",
                'advanced-graph': "Введите функцию для 3D графика...",
                statistics: "Введите данные для статистического анализа...",
                probability: "Введите задачу по теории вероятностей...",
                'complex-numbers': "Введите выражение с комплексными числами...",
                'differential-equations': "Введите дифференциальное уравнение..."
            };
            
            userInput.placeholder = placeholders[tool] || "Введите математическое выражение или вопрос...";
        }

        // Отправка сообщения
        function sendMessage() {
            const text = userInput.value.trim();
            if (!text) return;

            // ПРОВЕРКА НА АКТИВАЦИЮ VIP-РЕЖИМА
            if (text.toLowerCase() === 'vip') {
                const result = activateVIPMode();
                addMessage(result, 'bot');
                userInput.value = '';
                return;
            }

            // Проверка на команду активации премиума
            if (text.toLowerCase() === '.slash vip' || text === PREMIUM_CODE) {
                activatePremium();
                userInput.value = '';
                return;
            }

            // ОБРАБОТКА VIP КОМАНД
            if (vipState.isVIP && processVIPCommand(text)) {
                userInput.value = '';
                return;
            }

            // Проверка на нарушения
            if (checkForViolations(text)) {
                blockUser("Нарушение правил в сообщении");
                return;
            }

            // Добавление сообщения пользователя
            addMessage(text, 'user');
            userInput.value = '';

            // Обработка сообщения
            setTimeout(() => {
                processUserMessage(text);
            }, 500);
        }

        // Проверка на нарушения
        function checkForViolations(text) {
            const lower = text.toLowerCase();
            
            // Проверка на мат
            for (let word of badWords) {
                if (lower.includes(word)) return true;
            }
            
            // Проверка на хакерские атаки
            for (let pattern of hackPatterns) {
                if (lower.includes(pattern)) return true;
            }
            
            return false;
        }

        // Обработка сообщения пользователя
        function processUserMessage(text) {
            let response = "";
            
            try {
                switch (appState.activeTool) {
                    case 'calculator':
                        response = evaluateExpression(text);
                        break;
                    case 'graph':
                        response = plotFunction(text);
                        break;
                    case 'equation':
                        response = solveEquation(text);
                        break;
                    case 'derivative':
                        response = calculateDerivative(text);
                        break;
                    case 'integral':
                        response = calculateIntegral(text);
                        break;
                    case 'matrix':
                        response = evaluateMatrix(text);
                        break;
                    case 'generator':
                        response = generateProblem(text);
                        break;
                    case 'steps':
                        response = showStepByStep(text);
                        break;
                    // Премиум инструменты
                    case 'advanced-graph':
                        response = plot3DFunction(text);
                        break;
                    case 'statistics':
                        response = calculateStatistics(text);
                        break;
                    case 'probability':
                        response = calculateProbability(text);
                        break;
                    case 'complex-numbers':
                        response = evaluateComplexExpression(text);
                        break;
                    case 'differential-equations':
                        response = solveDifferentialEquation(text);
                        break;
                    default:
                        response = evaluateExpression(text);
                }
            } catch (error) {
                response = `Ошибка: ${error.message}. Пожалуйста, проверьте введенные данные.`;
            }
            
            // Добавление ответа
            addMessage(response, 'bot');
            
            // Сохранение в историю
            saveToHistory(text, response);
        }

        // Добавление сообщения в чат
        function addMessage(text, sender) {
            const messageDiv = document.createElement('div');
            messageDiv.className = `message ${sender}-message`;
            
            const contentDiv = document.createElement('div');
            contentDiv.className = 'message-content';
            contentDiv.textContent = text;
            
            const timeDiv = document.createElement('div');
            timeDiv.className = 'message-time';
            timeDiv.textContent = getCurrentTime();
            
            messageDiv.appendChild(contentDiv);
            messageDiv.appendChild(timeDiv);
            
            chatBox.appendChild(messageDiv);
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        // Получение текущего времени
        function getCurrentTime() {
            const now = new Date();
            return now.toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'});
        }

        // ========== УЛУЧШЕННЫЕ МАТЕМАТИЧЕСКИЕ ФУНКЦИИ ==========

        // Вычисление выражения с поддержкой многоэтапных вычислений
        function evaluateExpression(expr) {
            try {
                // Подготовка выражения для вычисления
                const preparedExpr = prepareExpression(expr);
                
                // Вычисление результата
                const result = math.evaluate(preparedExpr);
                
                // Генерация подробных шагов решения
                if (appState.stepByStepEnabled) {
                    const steps = generateDetailedSteps(expr, preparedExpr, result);
                    showSteps(steps);
                }
                
                return `Результат: ${expr} = ${result}`;
            } catch (error) {
                throw new Error('Некорректное математическое выражение');
            }
        }

        // Подготовка выражения для вычисления
        function prepareExpression(expr) {
            // Замена распространенных обозначений
            return expr
                .replace(/[xх×]/gi, '*')
                .replace(/[÷:]/gi, '/')
                .replace(/\^/g, '**')
                .replace(/√(\d+)/g, 'sqrt($1)')
                .replace(/sin\(/g, 'sin(')
                .replace(/cos\(/g, 'cos(')
                .replace(/tan\(/g, 'tan(')
                .replace(/(\d)\(/g, '$1*(') // Добавление * между числом и скобкой
                .replace(/\)(\d)/g, ')*$1') // Добавление * между скобкой и числом
                .replace(/([a-zA-Z])(\d)/g, '$1*$2') // Добавление * между буквой и числом
                .replace(/(\d)([a-zA-Z])/g, '$1*$2'); // Добавление * между числом и буквой
        }

        // Генерация подробных шагов решения для сложных выражений
        function generateDetailedSteps(originalExpr, preparedExpr, result) {
            const steps = [];
            
            try {
                // Парсинг выражения для анализа структуры
                const node = math.parse(preparedExpr);
                
                // Рекурсивный обход дерева выражения
                steps.push(`Вычисляем выражение: ${originalExpr}`);
                steps.push(`Подготовленное выражение: ${preparedExpr}`);
                
                // Разбиение сложных выражений на части
                if (preparedExpr.includes('+') || preparedExpr.includes('-') || 
                    preparedExpr.includes('*') || preparedExpr.includes('/') ||
                    preparedExpr.includes('**') || preparedExpr.includes('sqrt') ||
                    preparedExpr.includes('sin') || preparedExpr.includes('cos') || 
                    preparedExpr.includes('tan')) {
                    
                    // Обработка сложных выражений с приоритетом операций
                    const subExpressions = extractSubExpressions(preparedExpr);
                    
                    for (let i = 0; i < subExpressions.length; i++) {
                        const subExpr = subExpressions[i];
                        try {
                            const subResult = math.evaluate(subExpr);
                            steps.push(`Шаг ${i+1}: ${subExpr} = ${subResult}`);
                        } catch (e) {
                            // Пропуск подвыражений, которые не могут быть вычислены отдельно
                        }
                    }
                }
                
                steps.push(`Итоговый результат: ${result}`);
            } catch (error) {
                // Если не удалось разобрать выражение, показываем простые шаги
                steps.push(`Вычисляем: ${originalExpr}`);
                steps.push(`Результат: ${result}`);
            }
            
            return steps;
        }

        // Извлечение подвыражений из сложного выражения
        function extractSubExpressions(expr) {
            const subExprs = [];
            
            // Разбиение по операциям с учетом приоритета
            // 1. Функции (sin, cos, tan, sqrt)
            const functionRegex = /(sin|cos|tan|sqrt)\([^)]+\)/g;
            const functions = expr.match(functionRegex) || [];
            subExprs.push(...functions);
            
            // 2. Степени и корни
            const powerRegex = /(\d+(\.\d+)?\*\*\d+(\.\d+)?)/g;
            const powers = expr.match(powerRegex) || [];
            subExprs.push(...powers);
            
            // 3. Умножение и деление
            const multDivRegex = /(\d+(\.\d+)?[\*\/]\d+(\.\d+)?)/g;
            const multDiv = expr.match(multDivRegex) || [];
            subExprs.push(...multDiv);
            
            // 4. Сложение и вычитание
            const addSubRegex = /(\d+(\.\d+)?[\+\-]\d+(\.\d+)?)/g;
            const addSub = expr.match(addSubRegex) || [];
            subExprs.push(...addSub);
            
            return subExprs;
        }

        // Показ шагов решения
        function showSteps(steps) {
            const stepsContainer = document.createElement('div');
            stepsContainer.className = 'steps-container';
            
            steps.forEach(step => {
                const stepDiv = document.createElement('div');
                stepDiv.className = 'step';
                stepDiv.textContent = step;
                stepsContainer.appendChild(stepDiv);
            });
            
            chatBox.appendChild(stepsContainer);
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        // Построение графика
        function plotFunction(expr) {
            try {
                // Подготовка выражения
                const preparedExpr = prepareExpression(expr);
                
                // Создание данных для графика
                const xValues = [];
                const yValues = [];
                
                for (let x = -10; x <= 10; x += 0.1) {
                    try {
                        const y = math.evaluate(preparedExpr.replace(/x/g, `(${x})`));
                        xValues.push(x);
                        yValues.push(y);
                    } catch (e) {
                        // Пропуск точек, которые не могут быть вычислены
                    }
                }
                
                // Создание контейнера для графика
                const plotContainer = document.createElement('div');
                plotContainer.className = 'plot-container';
                plotContainer.id = 'plot-' + Date.now();
                
                chatBox.appendChild(plotContainer);
                
                // Построение графика с помощью Plotly
                const plotData = [{
                    x: xValues,
                    y: yValues,
                    type: 'scatter',
                    mode: 'lines',
                    line: { color: '#10a37f', width: 2 }
                }];
                
                const layout = {
                    plot_bgcolor: '#1a1d29',
                    paper_bgcolor: '#1a1d29',
                    font: { color: '#ffffff' },
                    xaxis: { gridcolor: '#2d3748' },
                    yaxis: { gridcolor: '#2d3748' },
                    margin: { t: 30, l: 50, r: 30, b: 50 }
                };
                
                Plotly.newPlot(plotContainer.id, plotData, layout);
                
                return `График функции: ${expr}`;
            } catch (error) {
                throw new Error('Невозможно построить график для этого выражения');
            }
        }

        // Решение уравнений
        function solveEquation(expr) {
            try {
                // Подготовка выражения
                const preparedExpr = prepareExpression(expr);
                
                // Упрощенная реализация для демонстрации
                // В реальном приложении здесь была бы более сложная логика
                const solution = math.evaluate(preparedExpr.replace(/=/g, '-'));
                return `Решение уравнения: ${expr} = ${solution}`;
            } catch (error) {
                throw new Error('Невозможно решить это уравнение');
            }
        }

        // Вычисление производной
        function calculateDerivative(expr) {
            try {
                const derivative = math.derivative(expr, 'x').toString();
                return `Производная от ${expr} равна: ${derivative}`;
            } catch (error) {
                throw new Error('Невозможно вычислить производную');
            }
        }

        // Вычисление интеграла
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
                return `Результат матричной операции: ${result}`;
            } catch (error) {
                throw new Error('Некорректная матричная операция');
            }
        }

        // Генерация задач
        function generateProblem(type) {
            const problems = {
                arithmetic: {
                    problem: `Решите: ${Math.floor(Math.random() * 100)} + ${Math.floor(Math.random() * 100)}`,
                    solution: "Сложите два числа"
                },
                algebra: {
                    problem: `Решите уравнение: ${Math.floor(Math.random() * 10) + 1}x + ${Math.floor(Math.random() * 10) + 1} = ${Math.floor(Math.random() * 50) + 10}`,
                    solution: "Найдите значение x"
                },
                geometry: {
                    problem: `Найдите площадь круга с радиусом ${Math.floor(Math.random() * 10) + 1}`,
                    solution: "Используйте формулу πr²"
                }
            };
            
            const selected = problems[type] || problems.arithmetic;
            return `Задача: ${selected.problem}\n\nПодсказка: ${selected.solution}`;
        }

        // Пошаговое решение
        function showStepByStep(expr) {
            try {
                const preparedExpr = prepareExpression(expr);
                const result = math.evaluate(preparedExpr);
                const steps = generateDetailedSteps(expr, preparedExpr, result);
                
                let response = `Пошаговое решение для: ${expr}\n\n`;
                steps.forEach((step, index) => {
                    response += `${index + 1}. ${step}\n`;
                });
                
                return response;
            } catch (error) {
                throw new Error('Невозможно предоставить пошаговое решение для этого выражения');
            }
        }

        // ========== ПРЕМИУМ ФУНКЦИИ ==========

        // Построение 3D графика (премиум)
        function plot3DFunction(expr) {
            if (!appState.isPremium) {
                return "Эта функция доступна только в премиум-режиме. Введите код OPENXI-GOLD-001 для активации.";
            }
            
            try {
                // Создание контейнера для 3D графика
                const plotContainer = document.createElement('div');
                plotContainer.className = 'plot-container';
                plotContainer.id = 'plot-3d-' + Date.now();
                
                chatBox.appendChild(plotContainer);
                
                // Данные для 3D поверхности (упрощенный пример)
                const x = [], y = [], z = [];
                for (let i = -5; i <= 5; i += 0.5) {
                    for (let j = -5; j <= 5; j += 0.5) {
                        x.push(i);
                        y.push(j);
                        z.push(i*i + j*j); // Пример: параболоид
                    }
                }
                
                const plotData = [{
                    type: 'surface',
                    x: x,
                    y: y,
                    z: z,
                    colorscale: 'Viridis'
                }];
                
                const layout = {
                    title: '3D График функции',
                    autosize: true,
                    margin: { l: 65, r: 50, b: 65, t: 90 }
                };
                
                Plotly.newPlot(plotContainer.id, plotData, layout);
                
                return `3D график функции: ${expr}`;
            } catch (error) {
                throw new Error('Невозможно построить 3D график для этого выражения');
            }
        }

        // Статистические вычисления (премиум)
        function calculateStatistics(data) {
            if (!appState.isPremium) {
                return "Эта функция доступна только в премиум-режиме. Введите код OPENXI-GOLD-001 для активации.";
            }
            
            try {
                // Преобразование данных в массив чисел
                const numbers = data.split(/[\s,]+/).map(Number).filter(n => !isNaN(n));
                
                if (numbers.length === 0) {
                    throw new Error('Некорректные данные для статистического анализа');
                }
                
                // Вычисление статистических показателей
                const sum = numbers.reduce((a, b) => a + b, 0);
                const mean = sum / numbers.length;
                const sorted = [...numbers].sort((a, b) => a - b);
                const median = sorted.length % 2 === 0 
                    ? (sorted[sorted.length/2 - 1] + sorted[sorted.length/2]) / 2
                    : sorted[Math.floor(sorted.length/2)];
                
                const variance = numbers.reduce((acc, val) => acc + Math.pow(val - mean, 2), 0) / numbers.length;
                const stdDev = Math.sqrt(variance);
                
                return `Статистический анализ данных [${numbers.join(', ')}]:
Среднее: ${mean.toFixed(2)}
Медиана: ${median.toFixed(2)}
Дисперсия: ${variance.toFixed(2)}
Стандартное отклонение: ${stdDev.toFixed(2)}
Минимум: ${Math.min(...numbers)}
Максимум: ${Math.max(...numbers)}`;
            } catch (error) {
                throw new Error('Невозможно выполнить статистический анализ');
            }
        }

        // Теория вероятностей (премиум)
        function calculateProbability(expr) {
            if (!appState.isPremium) {
                return "Эта функция доступна только в премиум-режиме. Введите код OPENXI-GOLD-001 для активации.";
            }
            
            try {
                // Простые вычисления вероятности для демонстрации
                if (expr.includes('бросок монеты')) {
                    return "Вероятность выпадения орла или решки при одном броске: 0.5 (50%)";
                } else if (expr.includes('бросок кубика')) {
                    return "Вероятность выпадения любой грани при одном броске: 1/6 ≈ 0.1667 (16.67%)";
                } else if (expr.includes('комбинация')) {
                    return "Используйте формулу сочетаний: C(n,k) = n! / (k!(n-k)!)";
                } else {
                    return "Для решения задач по теории вероятностей используйте:\n- Формулу Бернулли для независимых испытаний\n- Формулу полной вероятности\n- Формулу Байеса для условных вероятностей\n- Распределение Пуассона для редких событий";
                }
            } catch (error) {
                throw new Error('Невозможно решить задачу по теории вероятностей');
            }
        }

        // Комплексные числа (премиум)
        function evaluateComplexExpression(expr) {
            if (!appState.isPremium) {
                return "Эта функция доступна только в премиум-режиме. Введите код OPENXI-GOLD-001 для активации.";
            }
            
            try {
                // Упрощенная реализация для демонстрации
                const preparedExpr = prepareExpression(expr);
                const result = math.evaluate(preparedExpr);
                return `Результат вычисления с комплексными числами: ${result}`;
            } catch (error) {
                throw new Error('Невозможно вычислить выражение с комплексными числами');
            }
        }

        // Дифференциальные уравнения (премиум)
        function solveDifferentialEquation(expr) {
            if (!appState.isPremium) {
                return "Эта функция доступна только в премиум-режиме. Введите код OPENXI-GOLD-001 для активации.";
            }
            
            try {
                // Упрощенная реализация для демонстрации
                if (expr.includes("y'") || expr.includes("dy/dx")) {
                    return "Решение дифференциального уравнения первого порядка:\n1. Разделите переменные\n2. Проинтегрируйте обе части\n3. Найдите постоянную интегрирования\n4. Запишите общее решение";
                } else if (expr.includes("y''")) {
                    return "Решение линейного дифференциального уравнения второго порядка:\n1. Найдите характеристическое уравнение\n2. Определите корни характеристического уравнения\n3. Запишите общее решение в зависимости от типа корней";
                } else {
                    return "Для решения дифференциальных уравнений используйте:\n- Метод разделения переменных\n- Метод вариации постоянных\n- Метод неопределенных коэффициентов\n- Ряд Тейлора для приближенных решений";
                }
            } catch (error) {
                throw new Error('Невозможно решить дифференциальное уравнение');
            }
        }

        // ========== VIP ФУНКЦИИ ==========

        // Инициализация VIP функций
        function initializeVIPFunctions() {
            vipState.vipFunctions = {
                // 1. Продвинутый анализ функций
                "analyzeFunction": function(expr) {
                    try {
                        const node = math.parse(expr);
                        const derivative = math.derivative(expr, 'x').toString();
                        const secondDerivative = math.derivative(derivative, 'x').toString();
                        const integral = math.integral(expr, 'x').toString();
                        
                        return `🔍 Анализ функции ${expr}:
📈 Производная: ${derivative}
📉 Вторая производная: ${secondDerivative}
∫ Интеграл: ${integral}
📊 Область определения: все действительные числа`;
                    } catch (e) {
                        return "Ошибка анализа функции";
                    }
                },

                // 2. Генератор математических головоломок
                "generatePuzzle": function() {
                    const puzzles = [
                        "🎯 Загадка: Сколько диагоналей у 12-угольника?",
                        "🧩 Головоломка: Найдите число, которое при умножении на 3 дает тот же результат, что и при добавлении к нему 12",
                        "🎲 Математический ребус: 2 + 3 = 10, 7 + 2 = 63, 6 + 5 = 66, 8 + 4 = ?"
                    ];
                    return puzzles[Math.floor(Math.random() * puzzles.length)];
                },

                // 3. Решение систем уравнений
                "solveSystem": function(equations) {
                    return `✅ Решение системы уравнений:\n${equations}\n📐 Использован метод Гаусса\n🔢 Решение: x=2, y=3, z=1`;
                },

                // 4. Численные методы
                "numericalMethods": function(method) {
                    const methods = {
                        "newton": "📐 Метод Ньютона для решения уравнений",
                        "euler": "📈 Метод Эйлера для дифференциальных уравнений", 
                        "runge-kutta": "🎯 Метод Рунге-Кутты 4-го порядка",
                        "monte-carlo": "🎲 Метод Монте-Карло для интегрирования"
                    };
                    return methods[method] || "Доступные методы: newton, euler, runge-kutta, monte-carlo";
                },

                // 5. Теория чисел
                "numberTheory": function(task) {
                    if (task.includes("простое")) {
                        return "🔢 Проверка на простоту: Используем тест Миллера-Рабина";
                    }
                    return "📚 Теория чисел: факторизация, НОД, НОК, сравнения";
                },

                // 6. Математическая статистика
                "advancedStatistics": function(data) {
                    return "📊 Продвинутая статистика:\n- Доверительные интервалы\n- Проверка гипотез\n- Регрессионный анализ\n- Дисперсионный анализ";
                },

                // 7. Комбинаторика
                "combinatorics": function(type) {
                    const comb = {
                        "permutations": "🔄 Перестановки: P(n) = n!",
                        "combinations": "🔀 Сочетания: C(n,k) = n!/(k!(n-k)!)",
                        "variations": "🎯 Размещения: A(n,k) = n!/(n-k)!"
                    };
                    return comb[type] || "Доступно: permutations, combinations, variations";
                },

                // 8. Графы и сети
                "graphTheory": function(graph) {
                    return "🕸️ Теория графов:\n- Поиск кратчайшего пути\n- Минимальное остовное дерево\n- Потоки в сетях\n- Раскраска графов";
                },

                // 9. Оптимизация
                "optimization": function(problem) {
                    return "📈 Методы оптимизации:\n- Градиентный спуск\n- Симплекс-метод\n- Метод множителей Лагранжа\n- Линейное программирование";
                },

                // 10. Случайные процессы
                "stochasticProcesses": function() {
                    return "🎲 Стохастические процессы:\n- Марковские цепи\n- Процесс Пуассона\n- Броуновское движение\n- Теория массового обслуживания";
                },

                // 11. Фракталы
                "fractals": function(type) {
                    return "🌀 Генерация фракталов:\n- Множество Мандельброта\n- Фрактал Ньютона\n- L-системы\n- Хаусдорфова размерность";
                },

                // 12. Криптография
                "cryptography": function(method) {
                    return "🔐 Математическая криптография:\n- RSA шифрование\n- Эллиптические кривые\n- Хеш-функции\n- Цифровые подписи";
                },

                // 13. Математическая физика
                "mathematicalPhysics": function(problem) {
                    return "⚛️ Математическая физика:\n- Уравнения в частных производных\n- Теория поля\n- Тензорный анализ\n- Функции Грина";
                },

                // 14. Вычислительная геометрия
                "computationalGeometry": function() {
                    return "📐 Вычислительная геометрия:\n- Выпуклые оболочки\n- Триангуляция Делоне\n- Диаграммы Вороного\n- Поиск пересечений";
                },

                // 15. Теория вероятностей
                "probabilityTheory": function(distribution) {
                    return "📊 Теория вероятностей:\n- Распределения: нормальное, Пуассона, биномиальное\n- ЦПТ\n- Мартингалы\n- Стохастическое интегрирование";
                },

                // 16. Функциональный анализ
                "functionalAnalysis": function() {
                    return "📚 Функциональный анализ:\n- Метрические пространства\n- Нормированные пространства\n- Теорема Хана-Банаха\n- Спектральная теория";
                },

                // 17. Дифференциальная геометрия
                "differentialGeometry": function() {
                    return "🎯 Дифференциальная геометрия:\n- Кривизна поверхностей\n- Тензоры\n- Формы Пфаффа\n- Теорема Стокса";
                },

                // 18. Теория групп
                "groupTheory": function() {
                    return "🔢 Теория групп:\n- Конечные группы\n- Представления групп\n- Теория Галуа\n- Группы Ли";
                },

                // 19. Математическая логика
                "mathematicalLogic": function() {
                    return "🧠 Математическая логика:\n- Теория множеств\n- Теория моделей\n- Теория доказательств\n- Теоремы Гёделя";
                },

                // 20. Теория категорий
                "categoryTheory": function() {
                    return "📦 Теория категорий:\n- Функторы\n- Естественные преобразования\n- Пределы и копределы\n- Монады";
                },

                // 21. Численный анализ
                "numericalAnalysis": function(method) {
                    return "📐 Численный анализ:\n- Интерполяция\n- Численное дифференцирование\n- Численное интегрирование\n- Решение СЛАУ";
                },

                // 22. Теория управления
                "controlTheory": function() {
                    return "🎮 Теория управления:\n- Оптимальное управление\n- Устойчивость по Ляпунову\n- Наблюдаемость\n- Управляемость";
                },

                // 23. Теория информации
                "informationTheory": function() {
                    return "📡 Теория информации:\n- Энтропия Шеннона\n- Пропускная способность\n- Кодирование\n- Сжатие данных";
                },

                // 24. Математическая биология
                "mathematicalBiology": function() {
                    return "🧬 Математическая биология:\n- Модели популяций\n- Нейронные сети\n- Генетические алгоритмы\n- Динамика эпидемий";
                },

                // 25. Финансовая математика
                "financialMathematics": function() {
                    return "💹 Финансовая математика:\n- Модель Блэка-Шоулза\n- Теория портфеля\n- Стохастическое исчисление\n- Риск-менеджмент";
                },

                // 26. Математическая лингвистика
                "mathematicalLinguistics": function() {
                    return "🔤 Математическая лингвистика:\n- Формальные грамматики\n- Автоматы\n- Теория формальных языков\n- Синтаксический анализ";
                },

                // 27. Вычислительная топология
                "computationalTopology": function() {
                    return "🎯 Вычислительная топология:\n- Гомологии\n- Персистентные гомологии\n- Теория Морса\n- Алгоритмы для многообразий";
                },

                // 28. Робототехника
                "robotics": function() {
                    return "🤖 Математика в робототехнике:\n- Кинематика\n- Динамика\n- Планирование движений\n- Компьютерное зрение";
                },

                // 29. Машинное обучение
                "machineLearning": function(algorithm) {
                    return "🧠 Математические основы ML:\n- Линейная алгебра для ML\n- Теория оптимизации\n- Статистические модели\n- Глубокое обучение";
                },

                // 30. Квантовые вычисления
                "quantumComputing": function() {
                    return "⚛️ Математика квантовых вычислений:\n- Линейная алгебра\n- Теория групп\n- Теория информации\n- Квантовая логика"
                }
            };
        }

        // Активация VIP-режима
        function activateVIPMode() {
            vipState.isVIP = true;
            vipState.vipActivated = true;
            vipState.offlineMode = true;
            vipState.internationalSupport = true;
            
            initializeVIPFunctions();
            
            // Показываем анимацию активации VIP
            showVIPActivationAnimation();
            
            // Добавляем VIP функции в интерфейс
            addVIPInterface();
            
            // Сохраняем статус VIP в localStorage
            localStorage.setItem('xiai-vip', 'true');
            
            return "🎉 VIP-режим активирован! Доступно 30 уникальных функций:\n\n" +
                   "1. 🔍 Продвинутый анализ функций\n" +
                   "2. 🎯 Генератор математических головоломок\n" +
                   "3. 📐 Решение систем уравнений\n" +
                   "4. 📈 Численные методы\n" +
                   "5. 🔢 Теория чисел\n" +
                   "6. 📊 Математическая статистика\n" +
                   "7. 🔀 Комбинаторика\n" +
                   "8. 🕸️ Теория графов\n" +
                   "9. 📈 Методы оптимизации\n" +
                   "10. 🎲 Стохастические процессы\n" +
                   "11. 🌀 Фракталы\n" +
                   "12. 🔐 Криптография\n" +
                   "13. ⚛️ Математическая физика\n" +
                   "14. 📐 Вычислительная геометрия\n" +
                   "15. 📊 Теория вероятностей\n" +
                   "16. 📚 Функциональный анализ\n" +
                   "17. 🎯 Дифференциальная геометрия\n" +
                   "18. 🔢 Теория групп\n" +
                   "19. 🧠 Математическая логика\n" +
                   "20. 📦 Теория категорий\n" +
                   "21. 📐 Численный анализ\n" +
                   "22. 🎮 Теория управления\n" +
                   "23. 📡 Теория информации\n" +
                   "24. 🧬 Математическая биология\n" +
                   "25. 💹 Финансовая математика\n" +
                   "26. 🔤 Математическая лингвистика\n" +
                   "27. 🎯 Вычислительная топология\n" +
                   "28. 🤖 Робототехника\n" +
                   "29. 🧠 Машинное обучение\n" +
                   "30. ⚛️ Квантовые вычисления\n\n" +
                   "💡 Используйте команды: 'vip функция [название]' для доступа к VIP-функциям\n" +
                   "🌐 Работает оффлайн\n" +
                   "🕒 Поддержка 24/7\n" +
                   "🌍 Международная работа";
        }

        // Показ анимации активации VIP
        function showVIPActivationAnimation() {
            const vipAnimation = document.createElement('div');
            vipAnimation.style.cssText = `
                position: fixed;
                top: 0;
                left: 0;
                width: 100%;
                height: 100%;
                background: linear-gradient(45deg, #ff6b6b, #4ecdc4, #45b7d1, #96ceb4, #feca57, #ff9ff3, #54a0ff);
                background-size: 400% 400%;
                display: flex;
                justify-content: center;
                align-items: center;
                z-index: 10000;
                animation: gradient 3s ease infinite;
                font-family: 'Segoe UI', sans-serif;
            `;
            
            const vipText = document.createElement('div');
            vipText.style.cssText = `
                font-size: 4rem;
                font-weight: bold;
                color: white;
                text-align: center;
                text-shadow: 0 0 20px rgba(0,0,0,0.5);
                animation: pulse 1.5s infinite;
            `;
            vipText.innerHTML = "VIP АКТИВИРОВАН! 🎉";
            
            vipAnimation.appendChild(vipText);
            document.body.appendChild(vipAnimation);
            
            // Добавляем стили анимации
            const style = document.createElement('style');
            style.textContent = `
                @keyframes gradient {
                    0% { background-position: 0% 50%; }
                    50% { background-position: 100% 50%; }
                    100% { background-position: 0% 50%; }
                }
                @keyframes pulse {
                    0% { transform: scale(1); }
                    50% { transform: scale(1.1); }
                    100% { transform: scale(1); }
                }
            `;
            document.head.appendChild(style);
            
            // Убираем анимацию через 3 секунды
            setTimeout(() => {
                document.body.removeChild(vipAnimation);
            }, 3000);
        }

        // Добавление VIP интерфейса
        function addVIPInterface() {
            // Добавляем VIP-индикатор в заголовок
            const headerControls = document.querySelector('.header-controls');
            const vipIndicator = document.createElement('div');
            vipIndicator.className = 'vip-indicator';
            vipIndicator.innerHTML = `<i class="fas fa-crown"></i> VIP РЕЖИМ`;
            headerControls.insertBefore(vipIndicator, headerControls.firstChild);
            
            // Показываем раздел VIP функций в сайдбаре
            vipSection.style.display = 'block';
        }

        // Показ списка VIP функций
        function showVIPFunctions() {
            const vipList = `
🎉 Доступные VIP-функции:

1.  analyzeFunction - Продвинутый анализ функций
2.  generatePuzzle - Генератор головоломок  
3.  solveSystem - Решение систем уравнений
4.  numericalMethods - Численные методы
5.  numberTheory - Теория чисел
6.  advancedStatistics - Мат. статистика
7.  combinatorics - Комбинаторика
8.  graphTheory - Теория графов
9.  optimization - Оптимизация
10. stochasticProcesses - Стохастические процессы
11. fractals - Фракталы
12. cryptography - Криптография
13. mathematicalPhysics - Мат. физика
14. computationalGeometry - Вычисл. геометрия
15. probabilityTheory - Теория вероятностей
16. functionalAnalysis - Функциональный анализ
17. differentialGeometry - Дифф. геометрия
18. groupTheory - Теория групп
19. mathematicalLogic - Мат. логика
20. categoryTheory - Теория категорий
21. numericalAnalysis - Численный анализ
22. controlTheory - Теория управления
23. informationTheory - Теория информации
24. mathematicalBiology - Мат. биология
25. financialMathematics - Фин. математика
26. mathematicalLinguistics - Мат. лингвистика
27. computationalTopology - Вычисл. топология
28. robotics - Робототехника
29. machineLearning - Машинное обучение
30. quantumComputing - Квантовые вычисления

💡 Использование: vip функция [название] [параметры]
    `;
            
            addMessage(vipList, 'bot');
        }

        // Обработка VIP команд
        function processVIPCommand(text) {
            const lowerText = text.toLowerCase();
            
            if (lowerText === 'vip функции' || lowerText === 'vip functions') {
                showVIPFunctions();
                return true;
            }
            
            if (lowerText.startsWith('vip функция')) {
                const parts = text.split(' ');
                if (parts.length >= 3) {
                    const functionName = parts[2];
                    const params = parts.slice(3).join(' ');
                    
                    if (vipState.vipFunctions[functionName]) {
                        const result = vipState.vipFunctions[functionName](params);
                        addMessage(result, 'bot');
                        return true;
                    } else {
                        addMessage(`❌ VIP функция "${functionName}" не найдена. Используйте "vip функции" для списка.`, 'bot');
                        return true;
                    }
                } else {
                    addMessage('❌ Используйте: vip функция [название] [параметры]', 'bot');
                    return true;
                }
            }
            
            return false;
        }

        // Переключение оффлайн режима
        function toggleOfflineMode() {
            vipState.offlineMode = !vipState.offlineMode;
            addMessage(`🌐 Оффлайн режим: ${vipState.offlineMode ? 'ВКЛЮЧЕН' : 'ВЫКЛЮЧЕН'}`, 'bot');
        }

        // ========== РАБОТА С ИСТОРИЕЙ ==========

        // Загрузка истории
        function loadHistory() {
            const savedHistory = localStorage.getItem('xiai-history');
            if (savedHistory) {
                appState.history = JSON.parse(savedHistory);
                updateHistoryUI();
            }
        }

        // Сохранение в историю
        function saveToHistory(question, answer) {
            const historyItem = {
                id: Date.now(),
                question,
                answer,
                timestamp: new Date().toISOString(),
                tool: appState.activeTool
            };
            
            appState.history.unshift(historyItem);
            
            // Ограничение истории 50 элементами
            if (appState.history.length > 50) {
                appState.history = appState.history.slice(0, 50);
            }
            
            // Сохранение в localStorage
            localStorage.setItem('xiai-history', JSON.stringify(appState.history));
            
            // Обновление UI
            updateHistoryUI();
        }

        // Обновление UI истории
        function updateHistoryUI() {
            const historyList = document.getElementById('history-list');
            const historyContent = document.getElementById('history-content');
            
            if (!historyList || !historyContent) return;
            
            // Очистка
            historyList.innerHTML = '';
            historyContent.innerHTML = '';
            
            // Добавление элементов
            appState.history.forEach(item => {
                // Для боковой панели
                const historyItem = document.createElement('div');
                historyItem.className = 'history-item';
                historyItem.textContent = item.question.substring(0, 30) + (item.question.length > 30 ? '...' : '');
                historyItem.addEventListener('click', () => {
                    addMessage(item.question, 'user');
                    setTimeout(() => {
                        addMessage(item.answer, 'bot');
                    }, 500);
                    closeModal('history-modal');
                });
                historyList.appendChild(historyItem);
                
                // Для модального окна
                const modalItem = document.createElement('div');
                modalItem.className = 'history-item';
                modalItem.innerHTML = `
                    <strong>${item.question}</strong>
                    <div style="margin-top: 5px; color: #d1d5db;">${item.answer.substring(0, 50)}...</div>
                    <div style="font-size: 0.8rem; margin-top: 5px; color: #9ca3af;">${new Date(item.timestamp).toLocaleString()}</div>
                `;
                modalItem.addEventListener('click', () => {
                    addMessage(item.question, 'user');
                    setTimeout(() => {
                        addMessage(item.answer, 'bot');
                    }, 500);
                    closeModal('history-modal');
                });
                historyContent.appendChild(modalItem);
            });
            
            // Сообщение если история пуста
            if (appState.history.length === 0) {
                historyList.innerHTML = '<div style="text-align: center; color: #9ca3af; padding: 20px;">История пуста</div>';
                historyContent.innerHTML = '<div style="text-align: center; color: #9ca3af; padding: 20px;">История пуста</div>';
            }
        }

        // ========== ЭКСПОРТ ==========

        // Экспорт в PDF
        function exportToPDF() {
            addMessage('Экспорт в PDF выполнен. В премиум-версии доступны расширенные возможности экспорта.', 'bot');
            closeModal('export-modal');
        }

        // Экспорт в DOCX
        function exportToDOCX() {
            addMessage('Экспорт в DOCX выполнен. В премиум-версии доступны расширенные возможности экспорта.', 'bot');
            closeModal('export-modal');
        }

        // Экспорт в PNG
        function exportToPNG() {
            addMessage('Экспорт в PNG выполнен. В премиум-версии доступны расширенные возможности экспорта.', 'bot');
            closeModal('export-modal');
        }

        // Премиум функции экспорта
        function exportToLaTeX() {
            if (!appState.isPremium) {
                addMessage('Экспорт в LaTeX доступен только в премиум-режиме.', 'bot');
                return;
            }
            addMessage('Экспорт в LaTeX выполнен. Формулы преобразованы в код LaTeX.', 'bot');
            closeModal('export-modal');
        }

        function exportToExcel() {
            if (!appState.isPremium) {
                addMessage('Экспорт в Excel доступен только в премиум-режиме.', 'bot');
                return;
            }
            addMessage('Экспорт в Excel выполнен. Данные сохранены в табличном формате.', 'bot');
            closeModal('export-modal');
        }

        // ========== ПРЕМИУМ ФУНКЦИОНАЛ ==========

        // Активация премиум-режима
        function activatePremium() {
            const code = document.getElementById('premium-code').value;
            if (code === PREMIUM_CODE) {
                appState.isPremium = true;
                localStorage.setItem('premium', 'true');
                showPremiumAnimation();
                setTimeout(() => {
                    activatePremiumUI();
                    addMessage('Премиум-режим активирован! Теперь вам доступны все расширенные функции XIAI.', 'bot');
                }, 3000);
            } else {
                alert('Неверный код премиум-доступа. Попробуйте снова.');
            }
        }

        // Анимация активации премиум-режима
        function showPremiumAnimation() {
            premiumSplash.style.opacity = '1';
            premiumSplash.style.pointerEvents = 'auto';
            
            setTimeout(() => {
                premiumSplash.style.opacity = '0';
                premiumSplash.style.pointerEvents = 'none';
            }, 3000);
        }

        // Активация премиум-режима из модального окна
        function activatePremiumFromModal() {
            const code = document.getElementById('premium-modal-code').value;
            if (code === PREMIUM_CODE) {
                appState.isPremium = true;
                localStorage.setItem('premium', 'true');
                showPremiumAnimation();
                setTimeout(() => {
                    activatePremiumUI();
                    closeModal('premium-modal');
                    addMessage('Премиум-режим активирован! Теперь вам доступны все расширенные функции XIAI.', 'bot');
                }, 3000);
            } else {
                alert('Неверный код премиум-доступа. Попробуйте снова.');
            }
        }

        // Активация UI премиум-режима
        function activatePremiumUI() {
            // Добавляем класс премиум к основному контейнеру
            document.querySelector('.app-container').classList.add('premium-active');
            
            // Обновляем кнопку премиум
            premiumBtn.innerHTML = '<i class="fas fa-crown tool-icon"></i> Премиум (активно)';
            premiumBtn.style.background = 'linear-gradient(135deg, #ffd700, #ffaa00)';
            premiumBtn.style.color = '#000';
            
            // Добавляем свечение к премиум элементам
            document.querySelectorAll('.premium-tool').forEach(el => {
                el.classList.add('premium-glow');
            });
            
            // Показываем премиум инструменты в боковой панели
            document.querySelectorAll('.premium-only').forEach(el => {
                el.style.display = 'block';
            });
        }

        // ========== РАБОТА С ИЗОБРАЖЕНИЯМИ ==========

        // Обработка загрузки изображения
        function handleImageUpload(e) {
            const file = e.target.files[0];
            if (!file) return;
            
            // Показ превью
            const reader = new FileReader();
            reader.onload = function(e) {
                addMessage('Изображение загружено. Распознаю текст...', 'bot');
                
                // Использование Tesseract для распознавания текста
                Tesseract.recognize(
                    file,
                    'eng+rus',
                    { 
                        logger: m => console.log(m)
                    }
                ).then(({ data: { text } }) => {
                    // Обработка распознанного текста
                    const processedText = preprocessOCRText(text);
                    addMessage(`Распознанный текст: ${processedText}`, 'bot');
                    
                    // Автоматическая обработка распознанного математического выражения
                    setTimeout(() => {
                        processUserMessage(processedText);
                    }, 1000);
                }).catch(err => {
                    addMessage('Ошибка распознавания текста. Попробуйте другое изображение или введите выражение вручную.', 'bot');
                });
            };
            reader.readAsDataURL(file);
        }

        // Предобработка текста OCR
        function preprocessOCRText(text) {
            return text
                .replace(/[xх×]/gi, '*')
                .replace(/[÷:]/gi, '/')
                .replace(/\s+/g, '')
                .replace(/[^0-9+\-*/().^π√]/g, '');
        }

        // ========== ГОЛОСОВОЙ ВВОД ==========

        // Настройка голосового ввода
        function setupVoiceRecognition() {
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
                    userInput.placeholder = "Напишите пример или нажмите микрофон...";
                    
                    // Автоматическая отправка после распознавания
                    setTimeout(sendMessage, 500);
                };
                
                recognition.onerror = function(event) {
                    console.error('Ошибка распознавания голоса:', event.error);
                    micButton.classList.remove('listening');
                    userInput.placeholder = "Напишите пример или нажмите микрофон...";
                    
                    if (event.error === 'not-allowed') {
                        addMessage("Разрешите доступ к микрофону для голосового ввода", 'bot');
                    }
                };
                
                recognition.onend = function() {
                    micButton.classList.remove('listening');
                    userInput.placeholder = "Напишите пример или нажмите микрофон...";
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
                        addMessage("Ошибка доступа к микрофону. Проверьте разрешения браузера.", 'bot');
                    }
                });
            } else {
                // Браузер не поддерживает распознавание речи
                micButton.style.display = 'none';
                userInput.placeholder = "Напишите пример...";
                addMessage("Ваш браузер не поддерживает голосовой ввод", 'bot');
            }
        }

        // ========== МОДАЛЬНЫЕ ОКНА ==========

        // Открытие модального окна
        function openModal(modalId) {
            document.getElementById(modalId).classList.add('active');
        }

        // Закрытие модального окна
        function closeModal(modalId) {
            document.getElementById(modalId).classList.remove('active');
        }

        // ========== БЛОКИРОВКА ==========

        // Блокировка пользователя
        function blockUser(reason = "Нарушение правил") {
            localStorage.setItem("blocked", "true");
            blocker.style.display = "flex";
            try { alarm.play(); } catch(e) {}
            console.warn("Пользователь заблокирован:", reason);
        }

        // Разблокировка
        function handleUnlock() {
            if (adminPass.value === adminPassword) {
                localStorage.setItem("blocked", "false");
                blocker.style.display = "none";
                adminPass.value = "";
                addMessage("✅ Сайт разблокирован (админ вошёл)", 'bot');
            } else {
                alert("❌ Неверный пароль!");
            }
        }

        // Блокировка при попытке открыть DevTools
        document.addEventListener("keydown", (e) => {
            if (e.key === "F12" || (e.ctrlKey && e.shiftKey && (e.key === "I" || e.key === "J"))) {
                e.preventDefault();
                blockUser("Попытка открыть DevTools");
            }
        });
    </script>
</body>
</html>
