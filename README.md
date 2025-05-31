<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="theme-color" content="#667eea">
    
    <title>Afaneh AI Pro - النسخة النهائية</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    
    <style>
        :root {
            --primary-color: #667eea;
            --secondary-color: #764ba2;
            --accent-color: #28a745;
            --ai-color: #ff6b6b;
            --success-color: #20c997;
            --warning-color: #ffc107;
            --purple-color: #6f42c1;
            --pink-color: #e91e63;
            --text-color: #333;
            --border-radius: 15px;
            --transition: all 0.3s ease;
        }
        
        * { 
            margin: 0; 
            padding: 0; 
            box-sizing: border-box; 
            font-family: 'Roboto', Arial, sans-serif; 
            -webkit-tap-highlight-color: transparent;
        }
        
        body { 
            background: linear-gradient(135deg, var(--primary-color), var(--secondary-color)); 
            min-height: 100vh; 
            position: fixed;
            width: 100%;
            height: 100%;
            overflow: hidden;
        }
        
        .status-bar {
            height: 24px;
            background: rgba(0,0,0,0.3);
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 15px;
            color: white;
            font-size: 12px;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            z-index: 10000;
        }
        
        .app-container {
            position: fixed;
            top: 24px;
            left: 0;
            right: 0;
            bottom: 0;
            background: white;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
        }
        
        .mobile-header {
            background: linear-gradient(135deg, var(--ai-color), var(--primary-color));
            color: white;
            padding: 20px 15px;
            text-align: center;
            position: sticky;
            top: 0;
            z-index: 1000;
            box-shadow: 0 4px 20px rgba(0,0,0,0.15);
        }
        
        .header-title {
            font-size: 1.4rem;
            font-weight: bold;
            margin-bottom: 8px;
        }
        
        .header-subtitle {
            font-size: 0.9rem;
            opacity: 0.9;
            margin-bottom: 15px;
        }
        
        .mobile-language-selector {
            display: flex;
            justify-content: center;
            gap: 8px;
            padding: 12px;
            background: rgba(255,255,255,0.15);
            margin: 0 10px;
            border-radius: 25px;
        }
        
        .mobile-lang-btn {
            padding: 8px 16px;
            border: none;
            border-radius: 20px;
            background: rgba(255,255,255,0.2);
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: var(--transition);
            min-width: 50px;
        }
        
        .mobile-lang-btn.active {
            background: white;
            color: var(--ai-color);
            transform: scale(1.05);
        }
        
        .ai-assistant-panel {
            margin: 15px;
            padding: 20px;
            background: linear-gradient(135deg, #f8f9ff, #e8f0ff);
            border-radius: var(--border-radius);
            border: 2px solid var(--purple-color);
            position: relative;
            overflow: hidden;
        }
        
        .ai-assistant-panel::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(45deg, transparent, rgba(255,255,255,0.1), transparent);
            animation: shimmer 3s ease-in-out infinite;
            pointer-events: none;
        }
        
        @keyframes shimmer {
            0% { transform: translateX(-100%) translateY(-100%) rotate(45deg); }
            100% { transform: translateX(100%) translateY(100%) rotate(45deg); }
        }
        
        .ai-chat-header {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin-bottom: 15px;
            color: var(--purple-color);
            font-weight: bold;
            font-size: 1.1rem;
            position: relative;
            z-index: 1;
        }
        
        .ai-avatar {
            width: 45px;
            height: 45px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--purple-color), var(--pink-color));
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 22px;
            animation: aiPulse 2s ease-in-out infinite;
        }
        
        @keyframes aiPulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.1); }
        }
        
        .ai-chat-container {
            background: white;
            border-radius: 15px;
            padding: 15px;
            min-height: 120px;
            max-height: 200px;
            overflow-y: auto;
            box-shadow: inset 0 2px 10px rgba(0,0,0,0.1);
            margin-bottom: 15px;
            position: relative;
            z-index: 1;
        }
        
        .ai-message {
            margin-bottom: 12px;
            padding: 10px 15px;
            border-radius: 15px;
            background: #f8f9fa;
            border-left: 4px solid var(--purple-color);
            font-size: 0.9rem;
            line-height: 1.5;
            animation: messageSlide 0.3s ease-out;
        }
        
        .ai-message.user {
            background: #e3f2fd;
            border-left: 4px solid #2196f3;
            margin-left: 20px;
        }
        
        @keyframes messageSlide {
            from { transform: translateY(20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        
        .ai-input-container {
            display: flex;
            gap: 10px;
            position: relative;
            z-index: 1;
        }
        
        .ai-input {
            flex: 1;
            padding: 12px 15px;
            border: 2px solid #e9ecef;
            border-radius: 25px;
            font-size: 0.9rem;
            outline: none;
            transition: var(--transition);
        }
        
        .ai-input:focus {
            border-color: var(--purple-color);
            box-shadow: 0 0 0 3px rgba(111, 66, 193, 0.1);
        }
        
        .ai-send-btn {
            width: 45px;
            height: 45px;
            border: none;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--purple-color), var(--pink-color));
            color: white;
            font-size: 18px;
            cursor: pointer;
            transition: var(--transition);
        }
        
        .ai-send-btn:active {
            transform: scale(0.95);
        }
        
        .room-scanner-section {
            margin: 15px;
            padding: 20px;
            background: linear-gradient(135deg, #fff3e0, #ffe0b3);
            border-radius: var(--border-radius);
            border: 2px solid var(--warning-color);
        }
        
        .scanner-header {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin-bottom: 20px;
            color: #e65100;
            font-weight: bold;
            font-size: 1.2rem;
        }
        
        .scanner-controls {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
            margin-bottom: 20px;
        }
        
        .scanner-btn {
            padding: 15px 10px;
            border: 2px solid #ffb74d;
            border-radius: 15px;
            background: white;
            color: #e65100;
            font-weight: bold;
            cursor: pointer;
            transition: var(--transition);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            font-size: 0.85rem;
        }
        
        .scanner-btn:active {
            transform: scale(0.95);
            background: #fff3e0;
        }
        
        .room-dimensions {
            background: white;
            padding: 20px;
            border-radius: 15px;
        }
        
        .dimension-input {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .dimension-input input {
            width: 90px;
            padding: 10px;
            border: 2px solid #ddd;
            border-radius: 10px;
            text-align: center;
            font-weight: bold;
        }
        
        .mobile-contact {
            margin: 15px;
            padding: 20px;
            background: linear-gradient(135deg, #e8f5e8, #f0fff0);
            border-radius: var(--border-radius);
            border: 2px solid var(--accent-color);
        }
        
        .contact-header {
            text-align: center;
            color: var(--accent-color);
            font-weight: bold;
            margin-bottom: 20px;
            font-size: 1.2rem;
        }
        
        .contact-grid { display: grid; gap: 12px; }
        
        .contact-item {
            background: white;
            padding: 15px;
            border-radius: 12px;
            display: flex;
            align-items: center;
            gap: 15px;
            text-decoration: none;
            color: var(--text-color);
            transition: var(--transition);
        }
        
        .contact-item:active {
            transform: scale(0.98);
            background: #f8f9fa;
        }
        
        .contact-icon {
            width: 45px;
            height: 45px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            color: white;
            flex-shrink: 0;
        }
        
        .contact-text { 
            flex: 1; 
            font-weight: 600;
        }
        
        .contact-arrow { 
            color: #ccc; 
            font-size: 20px;
        }
        
        .business-section {
            margin: 15px;
            padding: 20px;
            background: linear-gradient(135deg, #f8f9fa, #e9ecef);
            border-radius: var(--border-radius);
            border: 2px solid var(--ai-color);
        }
        
        .section-title {
            text-align: center;
            color: var(--ai-color);
            font-size: 1.3rem;
            font-weight: bold;
            margin-bottom: 20px;
        }
        
        .business-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }
        
        .business-card {
            padding: 20px;
            border: 2px solid #ddd;
            border-radius: var(--border-radius);
            text-align: center;
            cursor: pointer;
            transition: var(--transition);
            background: white;
            position: relative;
        }
        
        .business-card:active { 
            transform: scale(0.95); 
        }
        
        .business-card.selected {
            border-color: var(--ai-color);
            background: linear-gradient(145deg, #ffe6e6, #fff0f0);
            transform: scale(1.02);
        }
        
        .business-card.selected::after {
            content: '✨';
            position: absolute;
            top: 8px;
            right: 8px;
            font-size: 18px;
            animation: sparkle 1s ease-in-out infinite;
        }
        
        @keyframes sparkle {
            0%, 100% { opacity: 1; transform: scale(1); }
            50% { opacity: 0.7; transform: scale(1.3); }
        }
        
        .business-icon { 
            font-size: 2.2rem; 
            margin-bottom: 12px;
        }
        
        .business-name { 
            font-weight: bold; 
            margin-bottom: 8px; 
            color: var(--text-color);
        }
        
        .business-desc { 
            font-size: 0.8rem; 
            color: #666;
        }
        
        .cost-section {
            margin: 15px;
            padding: 20px;
            background: linear-gradient(135deg, #e8f5e8, #f0fff0);
            border-radius: var(--border-radius);
            border: 2px solid var(--success-color);
        }
        
        .cost-breakdown {
            background: white;
            padding: 20px;
            border-radius: 15px;
            margin-top: 15px;
        }
        
        .cost-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid #eee;
        }
        
        .cost-item:last-child {
            border-bottom: none;
            font-weight: bold;
            color: var(--success-color);
            background: rgba(32, 201, 151, 0.1);
            padding: 15px;
            border-radius: 10px;
            margin-top: 10px;
        }
        
        .cost-label {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 0.95rem;
        }
        
        .cost-value { 
            font-weight: bold; 
            font-size: 0.95rem;
        }
        
        .generate-section {
            margin: 15px;
            padding: 20px;
            text-align: center;
        }
        
        .mobile-generate-btn {
            width: 100%;
            padding: 20px;
            background: linear-gradient(135deg, var(--ai-color), #ff5252);
            color: white;
            border: none;
            border-radius: var(--border-radius);
            font-size: 1.2rem;
            font-weight: bold;
            cursor: pointer;
            transition: var(--transition);
        }
        
        .mobile-generate-btn:active { 
            transform: scale(0.98); 
        }
        
        .mobile-generate-btn.loading {
            background: linear-gradient(135deg, #999, #777);
            cursor: not-allowed;
        }
        
        .mobile-3d-viewer {
            margin: 15px;
            height: 450px;
            background: #1a1a1a;
            border-radius: var(--border-radius);
            position: relative;
            overflow: hidden;
            border: 2px solid #333;
        }
        
        .mobile-3d-viewer.fullscreen {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            margin: 0;
            border-radius: 0;
            z-index: 9999;
            height: 100vh;
        }
        
        #mobile-canvas {
            width: 100%;
            height: 100%;
            display: block;
            border-radius: 13px;
        }
        
        .mobile-controls {
            position: absolute;
            top: 15px;
            right: 15px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            z-index: 10;
        }
        
        .mobile-control-btn {
            width: 50px;
            height: 50px;
            background: rgba(255,255,255,0.95);
            border: 2px solid var(--ai-color);
            border-radius: 15px;
            cursor: pointer;
            font-size: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: var(--transition);
            color: var(--text-color);
        }
        
        .mobile-control-btn:active {
            transform: scale(0.9);
            background: var(--ai-color);
            color: white;
        }
        
        .mobile-loading {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
            color: white;
            z-index: 5;
        }
        
        .mobile-spinner {
            width: 70px;
            height: 70px;
            border: 5px solid rgba(255,255,255,0.3);
            border-top: 5px solid var(--ai-color);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 25px;
        }
        
        @keyframes spin {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }
        
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: white;
            border-top: 1px solid #eee;
            display: flex;
            z-index: 1000;
        }
        
        .nav-item {
            flex: 1;
            padding: 15px 8px;
            text-align: center;
            cursor: pointer;
            transition: var(--transition);
            border: none;
            background: white;
            color: #666;
            position: relative;
        }
        
        .nav-item.active {
            color: var(--ai-color);
            background: rgba(255, 107, 107, 0.1);
        }
        
        .nav-item:active { 
            background: #f8f9fa; 
        }
        
        .nav-icon { 
            font-size: 22px; 
            margin-bottom: 5px;
        }
        
        .nav-label { 
            font-size: 0.75rem; 
            font-weight: 600;
        }
        
        .mobile-footer {
            margin: 15px;
            margin-bottom: 90px;
            padding: 25px;
            background: rgba(255,255,255,0.95);
            border-radius: var(--border-radius);
            text-align: center;
        }
        
        .footer-logo { 
            font-size: 2.5rem; 
            margin-bottom: 15px;
        }
        
        .footer-text { 
            color: #666; 
            font-size: 0.9rem; 
            line-height: 1.6;
        }
        
        .hidden { display: none !important; }
        
        .toast {
            position: fixed;
            top: 60px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0,0,0,0.9);
            color: white;
            padding: 15px 25px;
            border-radius: 30px;
            z-index: 10000;
            font-size: 0.9rem;
            max-width: 85%;
            text-align: center;
            animation: toastSlide 0.3s ease-out;
        }
        
        @keyframes toastSlide {
            from { transform: translateX(-50%) translateY(-20px); opacity: 0; }
            to { transform: translateX(-50%) translateY(0); opacity: 1; }
        }
    </style>
</head>
<body>
    <!-- Status Bar -->
    <div class="status-bar">
        <div id="statusTime">12:34</div>
        <div>📶 📶 🔋85%</div>
    </div>

    <!-- App Container -->
    <div class="app-container">
        <!-- Header -->
        <div class="mobile-header">
            <div class="header-title" id="headerTitle">🤖 Afaneh AI Pro</div>
            <div class="header-subtitle" id="headerSubtitle">نظام التصميم الذكي المتطور</div>
            
            <div class="mobile-language-selector">
                <button class="mobile-lang-btn active" onclick="changeLanguage('ar')" id="btn-ar">عربي</button>
                <button class="mobile-lang-btn" onclick="changeLanguage('en')" id="btn-en">EN</button>
                <button class="mobile-lang-btn" onclick="changeLanguage('es')" id="btn-es">ES</button>
                <button class="mobile-lang-btn" onclick="changeLanguage('fr')" id="btn-fr">FR</button>
            </div>
        </div>

        <!-- AI Assistant -->
        <div class="ai-assistant-panel">
            <div class="ai-chat-header">
                <div class="ai-avatar">🧠</div>
                <span id="aiAssistantTitle">مساعد الذكاء الاصطناعي</span>
            </div>
            <div class="ai-chat-container" id="aiChatContainer">
            </div>
            <div class="ai-input-container">
                <input type="text" class="ai-input" id="aiInput" placeholder="اكتب سؤالك هنا...">
                <button class="ai-send-btn" onclick="sendAIMessage()">📤</button>
            </div>
        </div>

        <!-- Room Scanner -->
        <div class="room-scanner-section">
            <div class="scanner-header">
                <span>📐</span>
                <span id="scannerTitle">مسح الغرفة الذكي</span>
            </div>
            <div class="scanner-controls">
                <button class="scanner-btn" onclick="startCamera()">
                    📷 <span id="cameraBtn">تشغيل الكاميرا</span>
                </button>
                <button class="scanner-btn" onclick="measureRoom()">
                    📏 <span id="measureBtn">قياس المساحة</span>
                </button>
                <button class="scanner-btn" onclick="detectObjects()">
                    🔍 <span id="detectBtn">اكتشاف الأثاث</span>
                </button>
                <button class="scanner-btn" onclick="generateFloorPlan()">
                    🗺️ <span id="floorPlanBtn">مخطط الأرضية</span>
                </button>
            </div>
            <div class="room-dimensions">
                <div class="dimension-input">
                    <label>العرض (متر):</label>
                    <input type="number" id="roomWidth" value="4.5" step="0.1">
                </div>
                <div class="dimension-input">
                    <label>الطول (متر):</label>
                    <input type="number" id="roomLength" value="5.2" step="0.1">
                </div>
                <div class="dimension-input">
                    <label>الارتفاع (متر):</label>
                    <input type="number" id="roomHeight" value="3.0" step="0.1">
                </div>
            </div>
        </div>

        <!-- Contact -->
        <div class="mobile-contact">
            <div class="contact-header" id="contactHeader">📞 معلومات الاتصال</div>
            <div class="contact-grid">
                <a href="tel:+962780888088" class="contact-item">
                    <div class="contact-icon" style="background: var(--accent-color);">📱</div>
                    <div class="contact-text">+962 78 088 8088</div>
                    <div class="contact-arrow">›</div>
                </a>
                <a href="mailto:Manager@afanehgroup.com" class="contact-item">
                    <div class="contact-icon" style="background: #17a2b8;">✉️</div>
                    <div class="contact-text">Manager@afanehgroup.com</div>
                    <div class="contact-arrow">›</div>
                </a>
                <a href="https://afanehgroup.com" class="contact-item">
                    <div class="contact-icon" style="background: var(--ai-color);">🌐</div>
                    <div class="contact-text" id="websiteText">مجموعة عفانة للتطوير</div>
                    <div class="contact-arrow">›</div>
                </a>
            </div>
        </div>

        <!-- Business Types -->
        <div class="business-section">
            <div class="section-title" id="businessTitle">🏪 اختر نوع مشروعك</div>
            <div class="business-grid">
                <div class="business-card" data-type="restaurant" onclick="selectBusiness('restaurant', this)">
                    <div class="business-icon">🍽️</div>
                    <div class="business-name" id="restaurantName">مطعم</div>
                    <div class="business-desc" id="restaurantDesc">تصميم مطاعم احترافي</div>
                </div>
                <div class="business-card" data-type="cafe" onclick="selectBusiness('cafe', this)">
                    <div class="business-icon">☕</div>
                    <div class="business-name" id="cafeName">كافيه</div>
                    <div class="business-desc" id="cafeDesc">أجواء عصرية مريحة</div>
                </div>
                <div class="business-card" data-type="retail" onclick="selectBusiness('retail', this)">
                    <div class="business-icon">🛍️</div>
                    <div class="business-name" id="retailName">متجر</div>
                    <div class="business-desc" id="retailDesc">عرض منتجات جذاب</div>
                </div>
                <div class="business-card" data-type="office" onclick="selectBusiness('office', this)">
                    <div class="business-icon">🏢</div>
                    <div class="business-name" id="officeName">مكتب</div>
                    <div class="business-desc" id="officeDesc">بيئة عمل منتجة</div>
                </div>
            </div>
        </div>

        <!-- Cost Calculator -->
        <div class="cost-section">
            <div class="section-title" id="costTitle">💰 حاسبة التكاليف الذكية</div>
            <div class="cost-breakdown">
                <div class="cost-item">
                    <div class="cost-label">🪑 <span id="furnitureLabel">الأثاث</span></div>
                    <div class="cost-value" id="furnitureCost">15,000 د.أ</div>
                </div>
                <div class="cost-item">
                    <div class="cost-label">🎨 <span id="decorLabel">الديكور</span></div>
                    <div class="cost-value" id="decorCost">10,000 د.أ</div>
                </div>
                <div class="cost-item">
                    <div class="cost-label">💡 <span id="lightLabel">الإضاءة</span></div>
                    <div class="cost-value" id="lightCost">5,000 د.أ</div>
                </div>
                <div class="cost-item">
                    <div class="cost-label">🔧 <span id="laborLabel">العمالة</span></div>
                    <div class="cost-value" id="laborCost">8,000 د.أ</div>
                </div>
                <div class="cost-item">
                    <div class="cost-label">📋 <span id="totalLabel">المجموع</span></div>
                    <div class="cost-value" id="totalCost">38,000 د.أ</div>
                </div>
            </div>
        </div>

        <!-- Generate Button -->
        <div class="generate-section">
            <button class="mobile-generate-btn" id="generateBtn" onclick="generate3DDesign()">
                🎮 <span id="generateText">إنشاء التصميم ثلاثي الأبعاد</span>
            </button>
        </div>

        <!-- 3D Viewer -->
        <div class="mobile-3d-viewer hidden" id="mobile3DViewer">
            <canvas id="mobile-canvas"></canvas>
            
            <div class="mobile-controls">
                <button class="mobile-control-btn" onclick="resetCamera()" title="إعادة تعيين">🔄</button>
                <button class="mobile-control-btn" onclick="toggleFullscreen()" title="ملء الشاشة">🔍</button>
                <button class="mobile-control-btn" onclick="takeScreenshot()" title="صورة">📷</button>
            </div>
            
            <div class="mobile-loading" id="mobileLoading">
                <div class="mobile-spinner"></div>
                <div style="font-weight: bold;" id="loadingText">جاري تحميل المجسمات...</div>
                <div style="font-size: 0.9rem; margin-top: 10px;" id="loadingDesc">تطبيق الخامات والإضاءة</div>
            </div>
        </div>

        <!-- Footer -->
        <div class="mobile-footer">
            <div class="footer-logo">🏢</div>
            <div class="footer-text">
                <strong>© 2024 Afaneh AI Pro</strong><br>
                مجموعة عفانة للتطوير والاستثمار<br>
                🎮 محرك ثلاثي الأبعاد • 🧠 ذكاء اصطناعي متطور
            </div>
        </div>
    </div>

    <!-- Bottom Navigation -->
    <div class="bottom-nav">
        <button class="nav-item active" onclick="showTab('design')" id="navDesign">
            <div class="nav-icon">🎨</div>
            <div class="nav-label" id="navDesignLabel">تصميم</div>
        </button>
        <button class="nav-item" onclick="showTab('3d')" id="nav3D">
            <div class="nav-icon">🎮</div>
            <div class="nav-label" id="nav3DLabel">ثلاثي الأبعاد</div>
        </button>
        <button class="nav-item" onclick="showTab('ai')" id="navAI">
            <div class="nav-icon">🧠</div>
            <div class="nav-label" id="navAILabel">الذكاء الاصطناعي</div>
        </button>
        <button class="nav-item" onclick="showTab('contact')" id="navContact">
            <div class="nav-icon">📞</div>
            <div class="nav-label" id="navContactLabel">اتصال</div>
        </button>
    </div>

    <script>
        // Global Variables
        let currentLanguage = 'ar';
        let selectedBusinessType = '';
        let scene, camera, renderer;
        let isGenerating = false;

        // Initialize App
        document.addEventListener('DOMContentLoaded', function() {
            console.log('🚀 Afaneh AI Pro - تهيئة التطبيق...');
            
            updateStatusBar();
            setInterval(updateStatusBar, 1000);
            updateCosts('cafe');
            
            // Initialize AI chat
            setTimeout(() => {
                addAIMessage('🤖 مرحباً بك في Afaneh AI Pro! أنا مساعدك الذكي لتصميم المساحات الداخلية. كيف يمكنني مساعدتك اليوم؟', 'ai');
            }, 1000);
            
            // Add Enter key support
            document.getElementById('aiInput').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    sendAIMessage();
                }
            });
            
            console.log('✅ التطبيق جاهز للاستخدام');
        });

        // Status Bar
        function updateStatusBar() {
            const now = new Date();
            const time = now.toLocaleTimeString('en-US', { 
                hour12: false, 
                hour: '2-digit', 
                minute: '2-digit' 
            });
            document.getElementById('statusTime').textContent = time;
        }

        // Translations
        const translations = {
            ar: {
                headerTitle: "🤖 Afaneh AI Pro",
                headerSubtitle: "نظام التصميم الذكي المتطور",
                aiAssistantTitle: "مساعد الذكاء الاصطناعي",
                scannerTitle: "مسح الغرفة الذكي",
                cameraBtn: "تشغيل الكاميرا",
                measureBtn: "قياس المساحة",
                detectBtn: "اكتشاف الأثاث",
                floorPlanBtn: "مخطط الأرضية",
                contactHeader: "📞 معلومات الاتصال",
                websiteText: "مجموعة عفانة للتطوير",
                businessTitle: "🏪 اختر نوع مشروعك",
                restaurantName: "مطعم",
                restaurantDesc: "تصميم مطاعم احترافي",
                cafeName: "كافيه",
                cafeDesc: "أجواء عصرية مريحة",
                retailName: "متجر",
                retailDesc: "عرض منتجات جذاب",
                officeName: "مكتب",
                officeDesc: "بيئة عمل منتجة",
                costTitle: "💰 حاسبة التكاليف الذكية",
                furnitureLabel: "الأثاث",
                decorLabel: "الديكور",
                lightLabel: "الإضاءة",
                laborLabel: "العمالة",
                totalLabel: "المجموع",
                generateText: "إنشاء التصميم ثلاثي الأبعاد",
                loadingText: "جاري تحميل المجسمات...",
                loadingDesc: "تطبيق الخامات والإضاءة",
                navDesignLabel: "تصميم",
                nav3DLabel: "ثلاثي الأبعاد",
                navAILabel: "الذكاء الاصطناعي",
                navContactLabel: "اتصال"
            },
            en: {
                headerTitle: "🤖 Afaneh AI Pro",
                headerSubtitle: "Advanced Smart Design System",
                aiAssistantTitle: "AI Assistant",
                scannerTitle: "Smart Room Scanner",
                cameraBtn: "Start Camera",
                measureBtn: "Measure Space",
                detectBtn: "Detect Furniture",
                floorPlanBtn: "Floor Plan",
                contactHeader: "📞 Contact Information",
                websiteText: "Afaneh Development Group",
                businessTitle: "🏪 Choose Your Business Type",
                restaurantName: "Restaurant",
                restaurantDesc: "Professional restaurant design",
                cafeName: "Cafe",
                cafeDesc: "Modern comfortable atmosphere",
                retailName: "Store",
                retailDesc: "Attractive product display",
                officeName: "Office",
                officeDesc: "Productive work environment",
                costTitle: "💰 Smart Cost Calculator",
                furnitureLabel: "Furniture",
                decorLabel: "Decoration",
                lightLabel: "Lighting",
                laborLabel: "Labor",
                totalLabel: "Total",
                generateText: "Generate 3D Design",
                loadingText: "Loading 3D models...",
                loadingDesc: "Applying materials and lighting",
                navDesignLabel: "Design",
                nav3DLabel: "3D",
                navAILabel: "AI",
                navContactLabel: "Contact"
            }
        };

        // Language Change
        function changeLanguage(lang) {
            currentLanguage = lang;
            
            document.querySelectorAll('.mobile-lang-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            document.getElementById(`btn-${lang}`).classList.add('active');
            
            if (lang === 'ar') {
                document.documentElement.setAttribute('dir', 'rtl');
                document.documentElement.setAttribute('lang', 'ar');
            } else {
                document.documentElement.setAttribute('dir', 'ltr');
                document.documentElement.setAttribute('lang', lang);
            }
            
            const t = translations[lang] || translations.ar;
            Object.keys(t).forEach(key => {
                const element = document.getElementById(key);
                if (element) {
                    element.textContent = t[key];
                }
            });
            
            const placeholders = {
                ar: 'اكتب سؤالك هنا...',
                en: 'Type your question here...'
            };
            document.getElementById('aiInput').placeholder = placeholders[lang] || placeholders.ar;
            
            showToast(`✅ تم تغيير اللغة إلى ${lang.toUpperCase()}`);
        }

        // AI Functions
        function sendAIMessage() {
            const input = document.getElementById('aiInput');
            const message = input.value.trim();
            
            if (!message) return;
            
            addAIMessage(`👤 ${message}`, 'user');
            input.value = '';
            
            const typingMsg = currentLanguage === 'ar' ? '🤔 جاري التفكير...' : '🤔 Thinking...';
            const typingElement = addAIMessage(typingMsg, 'ai');
            
            setTimeout(() => {
                if (typingElement && typingElement.parentNode) {
                    typingElement.remove();
                }
                
                const response = generateAIResponse(message);
                addAIMessage(`🤖 ${response}`, 'ai');
            }, 1500);
        }

        function addAIMessage(message, type) {
            const container = document.getElementById('aiChatContainer');
            const messageDiv = document.createElement('div');
            messageDiv.className = `ai-message ${type}`;
            messageDiv.textContent = message;
            
            container.appendChild(messageDiv);
            container.scrollTop = container.scrollHeight;
            
            return messageDiv;
        }

        function generateAIResponse(userMessage) {
            const responses = {
                ar: [
                    "ممتاز! أنصحك بالبدء باختيار نوع المشروع أولاً لأتمكن من تقديم نصائح مخصصة.",
                    "فكرة رائعة! الألوان والإضاءة تلعب دوراً مهماً في خلق الأجواء المناسبة.",
                    "يمكنني مساعدتك في اختيار التصميم المثالي. ما نوع المساحة التي تريد تصميمها؟",
                    "أقترح إضافة لمسات طبيعية مثل النباتات والخشب لجعل المكان أكثر دفئاً.",
                    "الاستفادة القصوى من المساحة مهمة جداً. دعني أقترح بعض الحلول الذكية."
                ],
                en: [
                    "Excellent! I recommend starting by selecting your project type first so I can provide customized advice.",
                    "Great idea! Colors and lighting play a crucial role in creating the right atmosphere.",
                    "I can help you choose the perfect design. What type of space would you like to design?",
                    "I suggest adding natural touches like plants and wood to make the space warmer.",
                    "Maximizing space efficiency is very important. Let me suggest some smart solutions."
                ]
            };
            
            const langResponses = responses[currentLanguage] || responses.ar;
            return langResponses[Math.floor(Math.random() * langResponses.length)];
        }

        // Room Scanner Functions
        function startCamera() {
            showToast('📷 تم تشغيل الكاميرا - وضع المسح الذكي');
        }

        function measureRoom() {
            const width = document.getElementById('roomWidth').value;
            const length = document.getElementById('roomLength').value;
            const height = document.getElementById('roomHeight').value;
            
            const area = (width * length).toFixed(1);
            const volume = (width * length * height).toFixed(1);
            
            showToast(`📏 المساحة: ${area} م² | الحجم: ${volume} م³`);
        }

        function detectObjects() {
            showToast('🔍 جاري اكتشاف الأثاث الموجود...');
            setTimeout(() => {
                showToast('✅ تم اكتشاف: طاولة، 4 كراسي، خزانة، نبات');
            }, 2000);
        }

        function generateFloorPlan() {
            showToast('🗺️ جاري إنشاء مخطط الأرضية...');
            setTimeout(() => {
                showToast('✅ تم إنشاء المخطط بنجاح!');
            }, 3000);
        }

        // Business Selection
        function selectBusiness(type, element) {
            selectedBusinessType = type;
            
            document.querySelectorAll('.business-card').forEach(card => {
                card.classList.remove('selected');
            });
            
            element.classList.add('selected');
            updateCosts(type);
            
            if (navigator.vibrate) {
                navigator.vibrate(50);
            }
            
            const businessNames = {
                ar: { restaurant: 'مطعم', cafe: 'كافيه', retail: 'متجر', office: 'مكتب' },
                en: { restaurant: 'Restaurant', cafe: 'Cafe', retail: 'Store', office: 'Office' }
            };
            
            const name = businessNames[currentLanguage][type] || type;
            showToast(`✨ تم اختيار: ${name}`);
        }

        function updateCosts(businessType) {
            const costs = {
                restaurant: { furniture: 18000, decor: 12000, light: 6000, labor: 9000 },
                cafe: { furniture: 15000, decor: 10000, light: 5000, labor: 8000 },
                retail: { furniture: 12000, decor: 8000, light: 4500, labor: 6500 },
                office: { furniture: 16000, decor: 11000, light: 5500, labor: 7500 }
            };
            
            const cost = costs[businessType] || costs.cafe;
            const total = cost.furniture + cost.decor + cost.light + cost.labor;
            
            document.getElementById('furnitureCost').textContent = `${cost.furniture.toLocaleString()} د.أ`;
            document.getElementById('decorCost').textContent = `${cost.decor.toLocaleString()} د.أ`;
            document.getElementById('lightCost').textContent = `${cost.light.toLocaleString()} د.أ`;
            document.getElementById('laborCost').textContent = `${cost.labor.toLocaleString()} د.أ`;
            document.getElementById('totalCost').textContent = `${total.toLocaleString()} د.أ`;
        }

        // 3D Generation
        function generate3DDesign() {
            if (!selectedBusinessType) {
                showToast('⚠️ يرجى اختيار نوع المشروع أولاً');
                return;
            }
            
            if (isGenerating) return;
            isGenerating = true;
            
            const btn = document.getElementById('generateBtn');
            const viewer = document.getElementById('mobile3DViewer');
            const loading = document.getElementById('mobileLoading');
            
            btn.classList.add('loading');
            btn.textContent = 'جاري الإنشاء...';
            
            viewer.classList.remove('hidden');
            loading.style.display = 'block';
            
            if (!scene) {
                initThreeJS();
            }
            
            setTimeout(() => {
                addFurnitureToScene();
                loading.style.display = 'none';
                btn.classList.remove('loading');
                btn.textContent = translations[currentLanguage]?.generateText || 'إنشاء التصميم ثلاثي الأبعاد';
                
                isGenerating = false;
                showToast('🎉 تم إنشاء التصميم بنجاح!');
                showTab('3d');
            }, 3000);
        }

        // Three.js
        function initThreeJS() {
            try {
                const canvas = document.getElementById('mobile-canvas');
                const container = document.getElementById('mobile3DViewer');
                
                scene = new THREE.Scene();
                scene.background = new THREE.Color(0x1a1a1a);
                
                camera = new THREE.PerspectiveCamera(75, container.offsetWidth / container.offsetHeight, 0.1, 1000);
                camera.position.set(8, 6, 8);
                camera.lookAt(0, 0, 0);
                
                renderer = new THREE.WebGLRenderer({ canvas: canvas, antialias: true });
                renderer.setSize(container.offsetWidth, container.offsetHeight);
                renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
                renderer.shadowMap.enabled = true;
                
                // Lights
                const ambientLight = new THREE.AmbientLight(0x404040, 0.6);
                scene.add(ambientLight);
                
                const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
                directionalLight.position.set(10, 10, 5);
                directionalLight.castShadow = true;
                scene.add(directionalLight);
                
                // Floor
                const floorGeometry = new THREE.PlaneGeometry(20, 20);
                const floorMaterial = new THREE.MeshLambertMaterial({ color: 0x808080 });
                const floor = new THREE.Mesh(floorGeometry, floorMaterial);
                floor.rotation.x = -Math.PI / 2;
                floor.receiveShadow = true;
                scene.add(floor);
                
                addTouchControls();
                animate();
                
            } catch (error) {
                console.error('❌ Three.js error:', error);
                showToast('❌ خطأ في تهيئة المحرك ثلاثي الأبعاد');
            }
        }

        function addTouchControls() {
            const canvas = document.getElementById('mobile-canvas');
            let isRotating = false;
            let previousTouch = { x: 0, y: 0 };
            
            canvas.addEventListener('touchstart', (e) => {
                e.preventDefault();
                isRotating = true;
                const touch = e.touches[0];
                previousTouch.x = touch.clientX;
                previousTouch.y = touch.clientY;
            });
            
            canvas.addEventListener('touchmove', (e) => {
                e.preventDefault();
                if (!isRotating) return;
                
                const touch = e.touches[0];
                const deltaX = touch.clientX - previousTouch.x;
                const deltaY = touch.clientY - previousTouch.y;
                
                const spherical = new THREE.Spherical();
                spherical.setFromVector3(camera.position);
                spherical.theta -= deltaX * 0.01;
                spherical.phi += deltaY * 0.01;
                spherical.phi = Math.max(0.1, Math.min(Math.PI - 0.1, spherical.phi));
                
                camera.position.setFromSpherical(spherical);
                camera.lookAt(0, 0, 0);
                
                previousTouch.x = touch.clientX;
                previousTouch.y = touch.clientY;
            });
            
            canvas.addEventListener('touchend', () => {
                isRotating = false;
            });
        }

        function createFurniture(type) {
            const group = new THREE.Group();
            
            try {
                switch(type) {
                    case 'table':
                        const tableTop = new THREE.Mesh(
                            new THREE.BoxGeometry(2, 0.1, 1),
                            new THREE.MeshPhongMaterial({ color: 0x8B4513 })
                        );
                        tableTop.position.y = 1;
                        tableTop.castShadow = true;
                        group.add(tableTop);
                        
                        for(let i = 0; i < 4; i++) {
                            const leg = new THREE.Mesh(
                                new THREE.CylinderGeometry(0.03, 0.03, 1),
                                new THREE.MeshPhongMaterial({ color: 0x444444 })
                            );
                            const x = (i % 2) * 1.6 - 0.8;
                            const z = Math.floor(i / 2) * 0.6 - 0.3;
                            leg.position.set(x, 0.5, z);
                            leg.castShadow = true;
                            group.add(leg);
                        }
                        break;
                        
                    case 'chair':
                        const seat = new THREE.Mesh(
                            new THREE.BoxGeometry(0.5, 0.05, 0.5),
                            new THREE.MeshPhongMaterial({ color: 0x8B4513 })
                        );
                        seat.position.y = 0.5;
                        seat.castShadow = true;
                        group.add(seat);
                        
                        const back = new THREE.Mesh(
                            new THREE.BoxGeometry(0.5, 0.6, 0.05),
                            new THREE.MeshPhongMaterial({ color: 0x8B4513 })
                        );
                        back.position.set(0, 0.8, -0.225);
                        back.castShadow = true;
                        group.add(back);
                        
                        for(let i = 0; i < 4; i++) {
                            const leg = new THREE.Mesh(
                                new THREE.CylinderGeometry(0.02, 0.02, 0.5),
                                new THREE.MeshPhongMaterial({ color: 0x444444 })
                            );
                            const x = (i % 2) * 0.4 - 0.2;
                            const z = Math.floor(i / 2) * 0.4 - 0.2;
                            leg.position.set(x, 0.25, z);
                            leg.castShadow = true;
                            group.add(leg);
                        }
                        break;
                }
            } catch (error) {
                console.error('❌ Furniture creation error:', error);
            }
            
            return group;
        }

        function addFurnitureToScene() {
            if (!scene) return;
            
            const objectsToRemove = [];
            scene.traverse((child) => {
                if (child.userData.furniture) {
                    objectsToRemove.push(child);
                }
            });
            objectsToRemove.forEach(obj => scene.remove(obj));
            
            const layouts = {
                restaurant: [
                    { type: 'table', position: [0, 0, 0] },
                    { type: 'chair', position: [-1.5, 0, 0] },
                    { type: 'chair', position: [1.5, 0, 0] },
                    { type: 'chair', position: [0, 0, -1] },
                    { type: 'chair', position: [0, 0, 1] }
                ],
                cafe: [
                    { type: 'table', position: [0, 0, 0] },
                    { type: 'chair', position: [-1.2, 0, 0] },
                    { type: 'chair', position: [1.2, 0, 0] },
                    { type: 'table', position: [3, 0, 2] },
                    { type: 'chair', position: [1.8, 0, 2] },
                    { type: 'chair', position: [4.2, 0, 2] }
                ],
                retail: [
                    { type: 'table', position: [0, 0, 0] },
                    { type: 'table', position: [3, 0, 0] },
                    { type: 'table', position: [-3, 0, 0] }
                ],
                office: [
                    { type: 'table', position: [0, 0, 0] },
                    { type: 'chair', position: [0, 0, -1] },
                    { type: 'table', position: [3, 0, 0] },
                    { type: 'chair', position: [3, 0, -1] }
                ]
            };
            
            const layout = layouts[selectedBusinessType] || layouts.cafe;
            
            layout.forEach(item => {
                try {
                    const furniture = createFurniture(item.type);
                    furniture.position.set(...item.position);
                    furniture.userData.furniture = true;
                    scene.add(furniture);
                } catch (error) {
                    console.error('❌ Add furniture error:', error);
                }
            });
        }

        function animate() {
            requestAnimationFrame(animate);
            if (renderer && scene && camera) {
                try {
                    renderer.render(scene, camera);
                } catch (error) {
                    console.error('❌ Render error:', error);
                }
            }
        }

        // Control Functions
        function resetCamera() {
            if (camera) {
                camera.position.set(8, 6, 8);
                camera.lookAt(0, 0, 0);
                showToast('🔄 تم إعادة تعيين الكاميرا');
            }
        }

        function toggleFullscreen() {
            const viewer = document.getElementById('mobile3DViewer');
            viewer.classList.toggle('fullscreen');
            
            setTimeout(() => {
                if (renderer && camera) {
                    const container = document.getElementById('mobile3DViewer');
                    camera.aspect = container.offsetWidth / container.offsetHeight;
                    camera.updateProjectionMatrix();
                    renderer.setSize(container.offsetWidth, container.offsetHeight);
                }
            }, 100);
            
            const isFullscreen = viewer.classList.contains('fullscreen');
            showToast(isFullscreen ? '🔍 وضع ملء الشاشة' : '📱 الوضع العادي');
        }

        function takeScreenshot() {
            if (renderer) {
                try {
                    const link = document.createElement('a');
                    link.download = 'afaneh-ai-design.png';
                    link.href = renderer.domElement.toDataURL();
                    link.click();
                    showToast('📷 تم حفظ لقطة الشاشة');
                } catch (error) {
                    console.error('❌ Screenshot error:', error);
                    showToast('❌ خطأ في حفظ الصورة');
                }
            }
        }

        // Navigation
        function showTab(tab) {
            document.querySelectorAll('.nav-item').forEach(item => {
                item.classList.remove('active');
            });
            
            switch(tab) {
                case 'design':
                    document.getElementById('navDesign').classList.add('active');
                    scrollToSection('.business-section');
                    break;
                case '3d':
                    document.getElementById('nav3D').classList.add('active');
                    if (!document.getElementById('mobile3DViewer').classList.contains('hidden')) {
                        scrollToSection('.mobile-3d-viewer');
                    } else {
                        showToast('⚠️ قم بإنشاء تصميم ثلاثي الأبعاد أولاً');
                    }
                    break;
                case 'ai':
                    document.getElementById('navAI').classList.add('active');
                    scrollToSection('.ai-assistant-panel');
                    break;
                case 'contact':
                    document.getElementById('navContact').classList.add('active');
                    scrollToSection('.mobile-contact');
                    break;
            }
        }

        function scrollToSection(selector) {
            const element = document.querySelector(selector);
            if (element) {
                element.scrollIntoView({ behavior: 'smooth', block: 'start' });
            }
        }

        // Toast Notification
        function showToast(message) {
            const toast = document.createElement('div');
            toast.className = 'toast';
            toast.textContent = message;
            document.body.appendChild(toast);
            
            setTimeout(() => {
                if (toast.parentNode) {
                    toast.remove();
                }
            }, 3000);
        }

        // Error Handling
        window.addEventListener('error', function(e) {
            console.error('❌ Global error:', e.error);
            showToast('⚠️ حدث خطأ في التطبيق');
        });

        // Prevent zoom on double tap
        let lastTouchEnd = 0;
        document.addEventListener('touchend', function (event) {
            const now = (new Date()).getTime();
            if (now - lastTouchEnd <= 300) {
                event.preventDefault();
            }
            lastTouchEnd = now;
        }, false);

        // Handle orientation change
        window.addEventListener('orientationchange', function() {
            setTimeout(() => {
                if (renderer && camera) {
                    const container = document.getElementById('mobile3DViewer');
                    camera.aspect = container.offsetWidth / container.offsetHeight;
                    camera.updateProjectionMatrix();
                    renderer.setSize(container.offsetWidth, container.offsetHeight);
                }
            }, 500);
        });
    </script>
</body>
</html>
