
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>SCIENCE = WATER | প্রিমিয়াম শিক্ষা প্ল্যাটফর্ম</title>
   
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-auth-compat.js"></script>
    
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=Hind+Siliguri:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --primary: #4361ee;
            --primary-light: #4895ef;
            --primary-dark: #3f37c9;
            --secondary: #7209b7;
            --accent: #4cc9f0;
            --success: #4caf50;
            --warning: #ff9800;
            --danger: #f44336;
            --dark: #1a1a2e;
            --light: #f8f9fa;
            --gray: #6c757d;
            --light-gray: #e9ecef;
            --shadow-sm: 0 4px 6px rgba(0, 0, 0, 0.07);
            --shadow-md: 0 10px 25px rgba(0, 0, 0, 0.1);
            --shadow-lg: 0 20px 40px rgba(0, 0, 0, 0.15);
            --radius-sm: 12px;
            --radius-md: 16px;
            --radius-lg: 24px;
            --ssc-color: #0ea5e9;
            --hsc-color: #8b5cf6;
            --spoken-color: #10b981;
            --madrasah-color: #f59e0b;
            --xp-gold: #f59e0b;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Inter', 'Hind Siliguri', sans-serif; }

        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            color: var(--dark);
            min-height: 100vh;
            max-width: 600px;
            margin: 0 auto;
            position: relative;
            overflow-x: hidden;
            padding-bottom: 80px;
        }

        .login-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            z-index: 10000;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
            transition: all 0.3s ease;
        }
        .login-overlay.hidden { display: none; }
        .login-container {
            background: white;
            border-radius: var(--radius-lg);
            padding: 35px 25px;
            max-width: 400px;
            width: 100%;
            text-align: center;
            box-shadow: var(--shadow-lg);
            animation: fadeInUp 0.5s ease;
        }
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .login-logo {
            width: 80px;
            height: 80px;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            border-radius: 25px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 20px;
        }
        .login-logo i { font-size: 40px; color: white; }
        .login-container h2 { font-size: 24px; margin-bottom: 8px; }
        .login-container p { color: var(--gray); margin-bottom: 25px; font-size: 14px; }
        .login-input {
            width: 100%;
            padding: 14px 16px;
            margin: 10px 0;
            border: 2px solid var(--light-gray);
            border-radius: var(--radius-md);
            font-size: 14px;
        }
        .login-input:focus { outline: none; border-color: var(--primary); }
        .login-btn-primary {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: white;
            border: none;
            padding: 14px;
            border-radius: var(--radius-md);
            font-weight: 600;
            cursor: pointer;
            width: 100%;
            margin-top: 15px;
            font-size: 16px;
        }
        .toggle-auth-btn {
            background: none;
            border: none;
            color: var(--primary);
            cursor: pointer;
            margin-top: 15px;
            font-size: 14px;
        }
        .error-message { color: var(--danger); font-size: 12px; margin-top: 8px; display: none; }

        .app-header {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: white;
            padding: 20px 20px 30px;
            border-radius: 0 0 30px 30px;
            box-shadow: var(--shadow-lg);
            margin-bottom: 20px;
        }
        .header-top { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
        .logo { display: flex; align-items: center; gap: 12px; }
        .logo-icon { width: 40px; height: 40px; background: white; border-radius: 12px; display: flex; align-items: center; justify-content: center; color: var(--primary); font-size: 20px; }
        .logo-text h1 { font-size: 22px; font-weight: 700; }
        .logo-text span { font-size: 12px; opacity: 0.9; }
        .greeting { font-size: 18px; font-weight: 600; margin-bottom: 4px; }
        .date-display { font-size: 13px; opacity: 0.9; }

        .firebase-status {
            background: rgba(0,0,0,0.5);
            padding: 6px 10px;
            border-radius: 30px;
            display: flex;
            align-items: center;
            gap: 6px;
        }
        .status-dot {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background: #4caf50;
            animation: pulse 2s infinite;
        }
        @keyframes pulse {
            0% { opacity: 0.5; transform: scale(0.8); }
            100% { opacity: 1; transform: scale(1.2); }
        }

        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 100%;
            max-width: 600px;
            background: white;
            display: flex;
            justify-content: space-around;
            padding: 12px 0;
            box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.1);
            z-index: 100;
            border-top-left-radius: 25px;
            border-top-right-radius: 25px;
        }
        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            color: var(--gray);
            cursor: pointer;
            transition: all 0.3s;
            font-size: 12px;
        }
        .nav-item i { font-size: 22px; margin-bottom: 4px; }
        .nav-item.active { color: var(--primary); }

        .page-content { display: none; padding: 0 20px 20px; animation: fadeIn 0.3s ease; }
        .page-content.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        .page-header {
            display: flex; align-items: center; gap: 15px; margin-bottom: 25px; padding-bottom: 15px; border-bottom: 2px solid var(--light-gray);
        }
        .page-header i {
            font-size: 24px; color: var(--primary); background: rgba(67, 97, 238, 0.1); width: 50px; height: 50px; border-radius: 15px; display: flex; align-items: center; justify-content: center;
        }
        .page-header h2 { font-size: 22px; font-weight: 700; }

        .stats-grid {
            display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin-bottom: 25px; margin-top: -40px; padding: 0 20px; position: relative; z-index: 10;
        }
        .stat-card { background: white; padding: 15px; border-radius: var(--radius-md); text-align: center; box-shadow: var(--shadow-md); cursor: pointer; transition: 0.3s; }
        .stat-value { font-size: 22px; font-weight: 800; color: var(--primary); }
        .stat-label { font-size: 12px; color: var(--gray); font-weight: 500; }

        .message-box {
            background: white; border-radius: var(--radius-md); padding: 20px; margin-bottom: 15px; box-shadow: var(--shadow-sm); border-left: 5px solid var(--warning);
            position: relative;
        }
        .class-card {
            background: white; border-radius: var(--radius-md); padding: 20px; margin-bottom: 15px; box-shadow: var(--shadow-sm);
            position: relative;
        }
        
        .thumbnail-grid { display: grid; grid-template-columns: 1fr; gap: 15px; }
        .thumbnail-card { background: white; border-radius: var(--radius-md); overflow: hidden; box-shadow: var(--shadow-sm); position: relative; }
        .thumbnail-image-container { position: relative; padding-bottom: 56.25%; background: #f1f5f9; }
        .thumbnail-image-container img { position: absolute; top: 0; left: 0; width: 100%; height: 100%; object-fit: cover; }
        .thumbnail-content { padding: 15px; }
        
        .login-btn, .link-btn, .start-btn {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: white; border: none; padding: 12px 20px; border-radius: 30px; font-weight: 600; cursor: pointer; width: 100%; text-align: center; margin-top: 10px; font-size: 14px;
            display: inline-block;
            text-decoration: none;
        }
        .subject-btn {
            display: block; width: 100%; padding: 15px; background: white; border: 2px solid var(--light-gray); border-radius: var(--radius-md); margin-bottom: 10px; text-align: left; font-weight: 600; cursor: pointer;
        }
        
        .mcq-exam-card { background: white; padding: 20px; border-radius: var(--radius-md); margin-bottom: 15px; box-shadow: var(--shadow-sm); position: relative; }
        .mcq-section-filters { display: flex; gap: 10px; margin-bottom: 20px; flex-wrap: wrap; }
        .mcq-filter-btn { padding: 8px 18px; border-radius: 30px; border: 1px solid var(--light-gray); background: white; cursor: pointer; font-weight: 500; }
        .mcq-filter-btn.active { background: var(--primary); color: white; }
        .mcq-exam-interface { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: white; z-index: 2000; overflow-y: auto; padding: 20px; }
        .mcq-option { padding: 15px; border: 2px solid var(--light-gray); border-radius: 16px; margin-bottom: 12px; cursor: pointer; display: flex; gap: 12px; align-items: center; }
        .mcq-option.selected { border-color: var(--primary); background: rgba(67, 97, 238, 0.05); }
        
        .routine-day { margin-bottom: 20px; background: white; border-radius: var(--radius-md); padding: 15px; box-shadow: var(--shadow-sm); }
        .day-header { font-weight: 700; color: var(--primary); margin-bottom: 10px; border-bottom: 1px solid var(--light-gray); padding-bottom: 5px; font-size: 16px; }
        .class-slot { display: flex; justify-content: space-between; padding: 12px; background: var(--light); border-radius: var(--radius-sm); margin-bottom: 8px; }
        
        .loading-spinner { text-align: center; padding: 30px; color: var(--gray); }
        #studentSubjectSelection, .virtual-scroll-container { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: white; z-index: 2500; padding: 20px; overflow-y: auto; }
        .close-btn { font-size: 24px; background: none; border: none; cursor: pointer; }

        .profile-card {
            background: white;
            border-radius: var(--radius-lg);
            padding: 25px;
            margin-bottom: 20px;
            box-shadow: var(--shadow-md);
            text-align: center;
        }
        .profile-avatar {
            width: 100px;
            height: 100px;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 15px;
            font-size: 48px;
            color: white;
        }
        .profile-name { font-size: 22px; font-weight: 700; margin-bottom: 5px; }
        .profile-email { color: var(--gray); margin-bottom: 15px; font-size: 14px; }
        .join-date { font-size: 12px; color: var(--gray); margin-bottom: 20px; }
        
        .xp-section {
            background: linear-gradient(135deg, var(--xp-gold), #fbbf24);
            border-radius: var(--radius-md);
            padding: 20px;
            margin-bottom: 20px;
            color: white;
        }
        .xp-value { font-size: 32px; font-weight: 800; }
        .progress-bar-container {
            background: rgba(255,255,255,0.3);
            border-radius: 20px;
            height: 12px;
            margin: 15px 0;
            overflow: hidden;
        }
        .progress-bar-fill {
            background: white;
            height: 100%;
            border-radius: 20px;
            transition: width 0.3s ease;
        }
        
        .stats-grid-profile {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin-top: 20px;
        }
        .stat-item {
            background: var(--light);
            padding: 15px;
            border-radius: var(--radius-md);
            text-align: center;
        }
        .stat-item .value { font-size: 20px; font-weight: 800; color: var(--primary); }
        .stat-item .label { font-size: 12px; color: var(--gray); }
        
        .logout-btn { background: var(--danger); margin-top: 20px; }

        #mainAppContent { display: none; }
        #mainAppContent.visible { display: block; }

        .admin-panel-btn {
            position: fixed;
            bottom: 80px;
            right: 20px;
            background: var(--danger);
            color: white;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: var(--shadow-md);
            z-index: 200;
            font-size: 24px;
        }
        .admin-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.95);
            z-index: 5000;
            display: none;
            overflow-y: auto;
            padding: 20px;
        }
        .admin-container {
            background: white;
            border-radius: var(--radius-lg);
            padding: 25px;
            max-width: 500px;
            margin: 20px auto;
        }
        .admin-input, .admin-textarea, .admin-select {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border: 1px solid var(--light-gray);
            border-radius: var(--radius-sm);
        }
        .admin-textarea { min-height: 100px; }
        .json-input { font-family: monospace; font-size: 12px; }
        
        .tab-buttons { display: flex; gap: 10px; margin-bottom: 20px; flex-wrap: wrap; }
        .tab-btn {
            padding: 10px 20px;
            background: var(--light-gray);
            border: none;
            border-radius: 30px;
            cursor: pointer;
        }
        .tab-btn.active { background: var(--primary); color: white; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        
        .item-list { max-height: 300px; overflow-y: auto; margin-top: 15px; }
        .list-item {
            background: var(--light);
            padding: 10px;
            margin-bottom: 8px;
            border-radius: var(--radius-sm);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .list-item button { margin-left: 5px; padding: 5px 10px; font-size: 12px; border-radius: 5px; border: none; cursor: pointer; }
        .delete-btn-admin { background: var(--danger); color: white; }
        
        .madrasah-card {
            border-left: 5px solid var(--madrasah-color);
        }
        
        .ssc-option-card {
            background: white;
            border-radius: var(--radius-md);
            padding: 20px;
            margin-bottom: 15px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            border: 2px solid var(--light-gray);
        }
        .ssc-option-card:hover {
            border-color: var(--ssc-color);
            transform: translateY(-2px);
        }
        .ssc-option-card i {
            font-size: 40px;
            color: var(--ssc-color);
            margin-bottom: 10px;
        }
        .ssc-option-card h3 {
            font-size: 18px;
            margin-bottom: 5px;
        }
        .ssc-option-card p {
            color: var(--gray);
            font-size: 12px;
        }
    </style>
</head>
<body>

    <div id="loginOverlay" class="login-overlay">
        <div class="login-container">
            <div class="login-logo"><i class="fas fa-graduation-cap"></i></div>
            <h2>SCIENCE = WATER</h2>
            <p>প্রিমিয়াম শিক্ষা প্ল্যাটফর্মে স্বাগতম</p>
            
            <div id="loginForm">
                <input type="email" id="loginEmail" class="login-input" placeholder="ইমেইল">
                <input type="password" id="loginPassword" class="login-input" placeholder="পাসওয়ার্ড">
                <button class="login-btn-primary" onclick="handleLogin()">লগইন করুন</button>
                <button class="toggle-auth-btn" onclick="showRegisterForm()">নতুন ইউজার? রেজিস্টার করুন</button>
                <div id="loginError" class="error-message"></div>
            </div>
            
            <div id="registerForm" style="display: none;">
                <input type="email" id="registerEmail" class="login-input" placeholder="ইমেইল">
                <input type="password" id="registerPassword" class="login-input" placeholder="পাসওয়ার্ড (৬ অক্ষর)">
                <button class="login-btn-primary" onclick="handleRegister()">রেজিস্টার করুন</button>
                <button class="toggle-auth-btn" onclick="showLoginForm()">ইতিমধ্যে অ্যাকাউন্ট আছে? লগইন করুন</button>
                <div id="registerError" class="error-message"></div>
            </div>
        </div>
    </div>

    <div id="mainAppContent">
        <div id="spokenNotesModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.9); z-index:3000; justify-content:center; align-items:center;">
            <div style="background:white; padding:30px; border-radius:24px; width:90%; max-width:500px; max-height:80vh; overflow-y:auto;">
                <div style="display:flex; justify-content:space-between; margin-bottom:20px;"><h3>🗣️ ক্লাস নোট</h3><button onclick="closeSpokenNotesModal()" style="font-size:24px; background:none; border:none;">&times;</button></div>
                <div id="spokenNotesContent"></div>
            </div>
        </div>
        
        <div id="mcqExamInterface" class="mcq-exam-interface"><div id="mcqExamInterfaceContent"></div></div>

        <header class="app-header">
            <div class="header-top">
                <div class="logo"><div class="logo-icon"><i class="fas fa-graduation-cap"></i></div><div class="logo-text"><h1>SCIENCE = WATER</h1><span>প্রিমিয়াম শিক্ষা প্ল্যাটফর্ম</span></div></div>
                <div class="firebase-status"><div class="status-dot"></div><span id="authStatus">লগইন করুন</span></div>
            </div>
            <div class="greeting" id="greetingText">স্বাগতম! 🎓</div>
            <div class="date-display" id="dateDisplay"></div>
        </header>

        <section class="stats-grid">
            <div class="stat-card" onclick="navigateToPage('routinePage')"><div class="stat-value" id="totalClasses">0</div><div class="stat-label">ক্লাস</div></div>
            <div class="stat-card" onclick="navigateToPage('quizPage')"><div class="stat-value" id="totalExams">0</div><div class="stat-label">কুইজ</div></div>
            <div class="stat-card" onclick="navigateToPage('noticePage')"><div class="stat-value" id="totalNotices">0</div><div class="stat-label">নোটিশ</div></div>
        </section>

        <div class="page-content active" id="dashboardPage">
            <div class="page-header"><i class="fas fa-home"></i><h2>ড্যাশবোর্ড</h2></div>
            <div class="thumbnail-section">
                <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px;">
                    <h3 style="font-size:16px; font-weight:700;">ফিচার্ড লেসন</h3>
                    <div class="mcq-section-filters">
                        <button class="mcq-filter-btn active" onclick="filterThumbnails('featured')">সব</button>
                        <button class="mcq-filter-btn" onclick="filterThumbnails('ssc')">এসএসসি</button>
                        <button class="mcq-filter-btn" onclick="filterThumbnails('hsc')">এইচএসসি</button>
                        <button class="mcq-filter-btn" onclick="filterThumbnails('madrasah')">মাদ্রাসা</button>
                    </div>
                </div>
                <div id="thumbnailGalleryContent"><div class="loading-spinner">লোড হচ্ছে...</div></div>
            </div>
            <div style="margin-top:25px;"><h3 style="margin-bottom:15px;">কুইক এক্সেস</h3>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:15px;">
                <div class="class-card" style="margin:0; border-left-color:var(--ssc-color); cursor:pointer;" onclick="showSSCOptions()"><div style="font-size:24px; color:var(--ssc-color); margin-bottom:10px;"><i class="fas fa-book-open"></i></div><h3>SSC ক্লাস</h3><p>সকল বিষয়</p></div>
                <div class="class-card" style="margin:0; border-left-color:var(--hsc-color); cursor:pointer;" onclick="navigateToPage('hscPage')"><div style="font-size:24px; color:var(--hsc-color); margin-bottom:10px;"><i class="fas fa-university"></i></div><h3>HSC ক্লাস</h3><p>সকল বিষয়</p></div>
                <div class="class-card" style="margin:0; border-left-color:var(--spoken-color); cursor:pointer;" onclick="navigateToPage('spokenPage')"><div style="font-size:24px; color:var(--spoken-color); margin-bottom:10px;"><i class="fas fa-language"></i></div><h3>স্পোকেন ইংলিশ</h3><p>কথা বলার দক্ষতা</p></div>
                <div class="class-card" style="margin:0; border-left-color:var(--madrasah-color); cursor:pointer;" onclick="navigateToPage('madrasahPage')"><div style="font-size:24px; color:var(--madrasah-color); margin-bottom:10px;"><i class="fas fa-mosque"></i></div><h3>মাদ্রাসা বোর্ড</h3><p>দাখিল ও আলিম</p></div>
            </div></div>
        </div>

        <div class="page-content" id="routinePage"><div class="page-header"><i class="fas fa-calendar-alt"></i><h2>ক্লাস রুটিন</h2></div><div id="studentRoutine"><div class="loading-spinner">লোড হচ্ছে...</div></div></div>
        <div class="page-content" id="noticePage"><div class="page-header"><i class="fas fa-bullhorn"></i><h2>নোটিশ বোর্ড</h2></div><div id="studentMessages"></div></div>
        
        <div class="page-content" id="sscPage">
            <div class="page-header"><i class="fas fa-book-open"></i><h2>এসএসসি জোন</h2></div>
            <div id="sscOptionsContainer"></div>
            <div style="margin-top:20px;"><h3>ক্লাস নোট</h3><button class="link-btn" onclick="openSpokenNotes('ssc')" style="background: var(--secondary);">এসএসসি ক্লাস নোট</button></div>
        </div>
        
        <div class="page-content" id="hscPage">
            <div class="page-header"><i class="fas fa-university"></i><h2>এইচএসসি জোন</h2></div>
            <div class="message-box" style="text-align:center;"><i class="fas fa-lock" style="font-size:24px; color:var(--secondary); margin-bottom:10px;"></i><h3>ক্লাস অ্যাক্সেস</h3><p style="margin-bottom:15px;">ক্লাস দেখতে পিন কোড দিন</p><button class="login-btn" onclick="verifyStudentAccess('hsc')" style="background:linear-gradient(135deg,var(--secondary),#5a189a);">এইচএসসি ক্লাস দেখুন</button></div>
            <div style="margin-top:20px;"><h3>ক্লাস নোট</h3><button class="link-btn" onclick="openSpokenNotes('hsc')" style="background: var(--primary);">এইচএসসি ক্লাস নোট</button></div>
        </div>
        
        <div class="page-content" id="spokenPage">
            <div class="page-header"><i class="fas fa-language"></i><h2>স্পোকেন ইংলিশ</h2></div>
            <div class="message-box" style="text-align:center;"><i class="fas fa-lock" style="font-size:24px; color:var(--spoken-color); margin-bottom:10px;"></i><h3>ক্লাস অ্যাক্সেস</h3><p style="margin-bottom:15px;">ক্লাস দেখতে পিন কোড দিন</p><button class="login-btn" onclick="verifySpokenEnglishAccess()" style="background:linear-gradient(135deg,var(--spoken-color),#059669);">স্পোকেন ইংলিশ ক্লাস দেখুন</button></div>
            <div style="margin-top:20px;"><h3>ক্লাস নোট ও লেকচার শীট</h3><button class="link-btn" onclick="openSpokenNotes('spoken')" style="background: var(--spoken-color);">📘 ক্লাস নোট দেখুন</button></div>
        </div>
        
        <div class="page-content" id="madrasahPage">
            <div class="page-header"><i class="fas fa-mosque"></i><h2>মাদ্রাসা বোর্ড</h2></div>
            <div class="message-box" style="text-align:center;"><i class="fas fa-lock" style="font-size:24px; color:var(--madrasah-color); margin-bottom:10px;"></i><h3>ক্লাস অ্যাক্সেস</h3><p style="margin-bottom:15px;">মাদ্রাসা ক্লাস দেখতে পিন কোড দিন</p><button class="login-btn" onclick="verifyStudentAccess('madrasah')" style="background:linear-gradient(135deg,var(--madrasah-color),#d97706);">মাদ্রাসা ক্লাস দেখুন</button></div>
            <div style="margin-top:20px;"><h3>ক্লাস নোট</h3><button class="link-btn" onclick="openSpokenNotes('madrasah')" style="background: var(--madrasah-color);">📘 মাদ্রাসা ক্লাস নোট</button></div>
        </div>
        
        <div class="page-content" id="quizPage">
            <div class="page-header"><i class="fas fa-question-circle"></i><h2>অনলাইন এক্সাম</h2></div>
            <div class="mcq-section">
                <div class="mcq-section-filters">
                    <button class="mcq-filter-btn active" onclick="filterMCQExams('all')">সব</button>
                    <button class="mcq-filter-btn" onclick="filterMCQExams('ssc')">এসএসসি</button>
                    <button class="mcq-filter-btn" onclick="filterMCQExams('hsc')">এইচএসসি</button>
                    <button class="mcq-filter-btn" onclick="filterMCQExams('madrasah')">মাদ্রাসা</button>
                </div>
                <div id="mcqExamContent"><div class="loading-spinner">লোড হচ্ছে...</div></div>
            </div>
        </div>
        
        <div class="page-content" id="profilePage">
            <div class="page-header"><i class="fas fa-user-circle"></i><h2>আমার প্রোফাইল</h2></div>
            <div id="profileContent"></div>
        </div>

        <div id="studentSubjectSelection">
            <div style="display:flex; justify-content:space-between; margin-bottom:20px;"><h3 id="studentCategoryTitle">বিষয়</h3><button class="close-btn" onclick="document.getElementById('studentSubjectSelection').style.display='none'">&times;</button></div>
            <div id="studentSubjectButtons"></div>
        </div>
        <div class="virtual-scroll-container">
            <div style="margin-bottom:20px;"><button onclick="document.querySelector('.virtual-scroll-container').style.display='none'" style="border:none; background:none; font-size:16px;"><i class="fas fa-arrow-left"></i> পেছনে</button></div>
            <div id="studentClassesContent"></div>
        </div>

        <nav class="bottom-nav">
            <a class="nav-item active" data-page="dashboardPage"><i class="fas fa-home"></i><span>হোম</span></a>
            <a class="nav-item" data-page="sscPage"><i class="fas fa-book-open"></i><span>এসএসসি</span></a>
            <a class="nav-item" data-page="hscPage"><i class="fas fa-university"></i><span>এইচএসসি</span></a>
            <a class="nav-item" data-page="madrasahPage"><i class="fas fa-mosque"></i><span>মাদ্রাসা</span></a>
            <a class="nav-item" data-page="spokenPage"><i class="fas fa-language"></i><span>স্পোকেন</span></a>
            <a class="nav-item" data-page="quizPage"><i class="fas fa-question-circle"></i><span>কুইজ</span></a>
            <a class="nav-item" data-page="profilePage"><i class="fas fa-user"></i><span>প্রোফাইল</span></a>
        </nav>
    </div>

    <div id="adminPanelBtn" class="admin-panel-btn" style="display: none;" onclick="showAdminModal()">
        <i class="fas fa-cog"></i>
    </div>

    <div id="adminModal" class="admin-modal">
        <div class="admin-container">
            <div style="display: flex; justify-content: space-between; margin-bottom: 20px;">
                <h3><i class="fas fa-shield-alt"></i> অ্যাডমিন প্যানেল</h3>
                <button onclick="closeAdminModal()" style="background: none; border: none; font-size: 24px; cursor: pointer;">&times;</button>
            </div>
            
            <div class="tab-buttons">
                <button class="tab-btn active" onclick="switchTab('classes')">📚 ক্লাস</button>
                <button class="tab-btn" onclick="switchTab('notices')">📢 নোটিশ</button>
                <button class="tab-btn" onclick="switchTab('mcqs')">📝 MCQ</button>
                <button class="tab-btn" onclick="switchTab('routine')">📅 রুটিন</button>
                <button class="tab-btn" onclick="switchTab('thumbnails')">🖼️ থাম্বনেইল</button>
                <button class="tab-btn" onclick="switchTab('bulk')">📦 Bulk</button>
            </div>
            
            <div id="classesTab" class="tab-content active">
                <h4>➕ নতুন ক্লাস যোগ করুন</h4>
                <input type="text" id="adminClassName" class="admin-input" placeholder="ক্লাসের নাম">
                <input type="text" id="adminClassDesc" class="admin-input" placeholder="বিবরণ">
                <input type="text" id="adminClassVideoId" class="admin-input" placeholder="YouTube ভিডিও আইডি বা লিংক">
                <select id="adminClassCategory" class="admin-select" onchange="updateSubjectOptions()">
                    <option value="ssc">এসএসসি</option>
                    <option value="hsc">এইচএসসি</option>
                    <option value="spoken">স্পোকেন ইংলিশ</option>
                    <option value="madrasah">মাদ্রাসা বোর্ড</option>
                </select>
                <select id="adminClassSubject" class="admin-select"></select>
                <button class="login-btn" onclick="addClass()">➕ ক্লাস যোগ করুন</button>
                <h4 style="margin-top: 20px;">📋 বিদ্যমান ক্লাসসমূহ</h4>
                <div id="classesList" class="item-list"></div>
            </div>
            
            <div id="noticesTab" class="tab-content">
                <h4>➕ নতুন নোটিশ যোগ করুন</h4>
                <input type="text" id="adminNoticeTitle" class="admin-input" placeholder="নোটিশের শিরোনাম">
                <textarea id="adminNoticeContent" class="admin-textarea" placeholder="নোটিশের বিবরণ"></textarea>
                <button class="login-btn" onclick="addNotice()">➕ নোটিশ যোগ করুন</button>
                <h4 style="margin-top: 20px;">📋 বিদ্যমান নোটিশসমূহ</h4>
                <div id="noticesList" class="item-list"></div>
            </div>
            
            <div id="mcqsTab" class="tab-content">
                <h4>➕ নতুন MCQ পরীক্ষা যোগ করুন</h4>
                <input type="text" id="mcqTitle" class="admin-input" placeholder="পরীক্ষার নাম">
                <input type="text" id="mcqSubject" class="admin-input" placeholder="বিষয়">
                <select id="mcqCategory" class="admin-select">
                    <option value="ssc">এসএসসি</option>
                    <option value="hsc">এইচএসসি</option>
                    <option value="madrasah">মাদ্রাসা</option>
                </select>
                <input type="number" id="mcqTimeLimit" class="admin-input" placeholder="সময় (মিনিট)">
                <textarea id="mcqQuestionsJson" class="admin-textarea json-input" placeholder='প্রশ্নসমূহ (JSON):
[{"text":"প্রশ্ন?","options":{"A":"অপশন 1","B":"অপশন 2","C":"অপশন 3","D":"অপশন 4"},"correctAnswer":"A"}]'></textarea>
                <button class="login-btn" onclick="addMCQ()">➕ MCQ যোগ করুন</button>
                <h4 style="margin-top: 20px;">📋 বিদ্যমান MCQ পরীক্ষাসমূহ</h4>
                <div id="mcqsList" class="item-list"></div>
            </div>
            
            <div id="routineTab" class="tab-content">
                <h4>➕ নতুন রুটিন যোগ করুন</h4>
                <select id="routineDay" class="admin-select">
                    <option value="saturday">শনিবার</option>
                    <option value="sunday">রবিবার</option>
                    <option value="monday">সোমবার</option>
                    <option value="tuesday">মঙ্গলবার</option>
                    <option value="wednesday">বুধবার</option>
                    <option value="thursday">বৃহস্পতিবার</option>
                    <option value="friday">শুক্রবার</option>
                </select>
                <input type="text" id="routineTime" class="admin-input" placeholder="সময়">
                <input type="text" id="routineClassName" class="admin-input" placeholder="ক্লাসের নাম">
                <button class="login-btn" onclick="addRoutine()">➕ রুটিন যোগ করুন</button>
                <h4 style="margin-top: 20px;">📋 বিদ্যমান রুটিন</h4>
                <div id="routineList" class="item-list"></div>
            </div>
            
            <div id="thumbnailsTab" class="tab-content">
                <h4>➕ নতুন থাম্বনেইল যোগ করুন</h4>
                <input type="text" id="thumbTitle" class="admin-input" placeholder="শিরোনাম">
                <input type="text" id="thumbImageUrl" class="admin-input" placeholder="ইমেজ URL">
                <input type="text" id="thumbVideoLink" class="admin-input" placeholder="ভিডিও লিংক">
                <select id="thumbCategory" class="admin-select">
                    <option value="featured">ফিচার্ড</option>
                    <option value="ssc">এসএসসি</option>
                    <option value="hsc">এইচএসসি</option>
                    <option value="madrasah">মাদ্রাসা</option>
                </select>
                <button class="login-btn" onclick="addThumbnail()">➕ থাম্বনেইল যোগ করুন</button>
                <h4 style="margin-top: 20px;">📋 বিদ্যমান থাম্বনেইল</h4>
                <div id="thumbnailsList" class="item-list"></div>
            </div>
            
            <div id="bulkTab" class="tab-content">
                <h4>📦 Bulk JSON আপলোড</h4>
                <textarea id="bulkJsonData" class="admin-input json-input" rows="10" placeholder='{"classes":[],"notices":[],"mcqs":[],"routine":[],"thumbnails":[]}'></textarea>
                <button class="login-btn" onclick="bulkUpload()">🚀 Bulk আপলোড করুন</button>
            </div>
        </div>
    </div>

    <script>
        const FIREBASE_CONFIG = {
            apiKey: "AIzaSyDEuohqZKdyxAPOCpEaiW1rinvNGHyDYkE",
            authDomain: "batch-26-93675.firebaseapp.com",
            projectId: "batch-26-93675",
            storageBucket: "batch-26-93675.firebasestorage.app",
            messagingSenderId: "338211475047",
            appId: "1:338211475047:web:b5edfe4dd63fcb285e8ec2"
        };
        
        let db, auth;
        let currentUser = null;
        let userProfile = null;
        let classes = [], messages = [], routine = [], spokenNotes = [], thumbnails = [], mcqExams = [];
        const CLASS_PASSWORDS = { hsc: "science1232", spoken: "Spoken12345", madrasah: "madrasah2024" };
        
        const SSC_OPTIONS_PASSWORDS = {
            science: "science123",
            arts: "arts123",
            commerce: "commerce123"
        };
        
        const SPOKEN_SUBJECTS = [
            {id:'basics', name:'স্পোকেন ইংলিশ বেসিক'},
            {id:'grammar', name:'গ্রামার'},
            {id:'vocabulary', name:'ভোকাবুলারি'}
        ];
        
        const SSC_SUBJECTS = [
            {id:'physics', name:'পদার্থ'},
            {id:'chemistry', name:'রসায়ন'},
            {id:'biology', name:'জীববিজ্ঞান'},
            {id:'math', name:'সাধারণ গণিত'},
            {id:'bangla1', name:'বাংলা প্রথম'},
            {id:'bangla2', name:'বাংলা দ্বিতীয়'},
            {id:'english1', name:'ইংরেজি প্রথম'},
            {id:'english2', name:'ইংরেজি দ্বিতীয়'}
        ];
        
        const ARTS_SUBJECTS = [
            {id:'bangla1', name:'বাংলা প্রথম'},
            {id:'bangla2', name:'বাংলা দ্বিতীয়'},
            {id:'english1', name:'ইংরেজি প্রথম'},
            {id:'english2', name:'ইংরেজি দ্বিতীয়'},
            {id:'math', name:'সাধারণ গণিত'},
            {id:'science', name:'বিজ্ঞান'},
            {id:'ict', name:'আইসিটি'}
        ];
        
        const COMMERCE_SUBJECTS = [
            {id:'bangla1', name:'বাংলা প্রথম'},
            {id:'bangla2', name:'বাংলা দ্বিতীয়'},
            {id:'english1', name:'ইংরেজি প্রথম'},
            {id:'english2', name:'ইংরেজি দ্বিতীয়'},
            {id:'math', name:'সাধারণ গণিত'},
            {id:'science', name:'বিজ্ঞান'},
            {id:'ict', name:'আইসিটি'},
            {id:'accounting', name:'হিসাব বিজ্ঞান'},
            {id:'finance', name:'ফিন্যান্স'},
            {id:'business', name:'ব্যবসায় উদ্যোগ'}
        ];
        
        const MADRASAH_SUBJECTS = [
            {id:'quran', name:'কোরআন'},
            {id:'hadith', name:'হাদিস'},
            {id:'aqaid', name:'আকাইদ'},
            {id:'arabic1', name:'আরবী প্রথম'},
            {id:'arabic2', name:'আরবি দ্বিতীয়'},
            {id:'bangla1', name:'বাংলা প্রথম'},
            {id:'bangla2', name:'বাংলা দ্বিতীয়'},
            {id:'english1', name:'ইংরেজি প্রথম'},
            {id:'english2', name:'ইংরেজি দ্বিতীয়'},
            {id:'math', name:'সাধারণ গণিত'}
        ];
        
        const HSC_SUBJECTS = [
            {id:'physics1', name:'পদার্থ ১ম পত্র'},
            {id:'physics2', name:'পদার্থ ২য় পত্র'},
            {id:'chemistry1', name:'রসায়ন ১ম পত্র'},
            {id:'chemistry2', name:'রসায়ন ২য় পত্র'},
            {id:'biology1', name:'জীব বিজ্ঞান ১ম পত্র'},
            {id:'biology2', name:'জীব বিজ্ঞান ২য় পত্র'}
        ];
        
        const SUBJECTS = {
            ssc: SSC_SUBJECTS,
            hsc: HSC_SUBJECTS,
            spoken: SPOKEN_SUBJECTS,
            madrasah: MADRASAH_SUBJECTS,
            arts: ARTS_SUBJECTS,
            commerce: COMMERCE_SUBJECTS
        };
        
        function updateSubjectOptions() {
            const category = document.getElementById('adminClassCategory').value;
            const subjectSelect = document.getElementById('adminClassSubject');
            let subjects = SUBJECTS[category] || SSC_SUBJECTS;
            
            subjectSelect.innerHTML = '';
            subjects.forEach(sub => {
                const option = document.createElement('option');
                option.value = sub.id;
                option.textContent = sub.name;
                subjectSelect.appendChild(option);
            });
        }
        
        let currentMCQCategory = 'all', currentExam = null, currentQuestionIndex = 0, userAnswers = [], examTimer = null, timeRemaining = 0, currentThumbnailCategory = 'featured';
        
        const XP_CONFIG = { CORRECT: 10, WRONG: 2, EXAM_COMPLETE_BONUS: 50 };
        const ADMIN_EMAILS = ["admin@gmail.com", "admin@sciencewater.com"];
        
        function getYouTubeUrl(videoId) {
            if (!videoId) return '';
            if (videoId.includes('youtube.com/watch?v=')) return videoId;
            if (videoId.includes('youtu.be/')) return videoId;
            if (videoId.length === 11) return `https://www.youtube.com/watch?v=${videoId}`;
            return `https://www.youtube.com/watch?v=${videoId}`;
        }
        
        function getLevelFromXP(xp) {
            if (xp < 100) return 1;
            if (xp < 250) return 2;
            if (xp < 500) return 3;
            if (xp < 800) return 4;
            if (xp < 1200) return 5;
            if (xp < 1700) return 6;
            if (xp < 2300) return 7;
            if (xp < 3000) return 8;
            if (xp < 3800) return 9;
            return 10 + Math.floor((xp - 3800) / 1000);
        }
        
        function getXPForNextLevel(currentXP) {
            let level = getLevelFromXP(currentXP);
            if (level === 1) return 100;
            if (level === 2) return 250;
            if (level === 3) return 500;
            if (level === 4) return 800;
            if (level === 5) return 1200;
            if (level === 6) return 1700;
            if (level === 7) return 2300;
            if (level === 8) return 3000;
            if (level === 9) return 3800;
            return 3800 + (level - 9) * 1000;
        }
        
        async function initializeUserProfile(user) {
            const userRef = db.collection('users').doc(user.uid);
            const doc = await userRef.get();
            if (!doc.exists) {
                const newProfile = {
                    name: user.email.split('@')[0],
                    email: user.email,
                    avatar: '👨‍🎓',
                    joinDate: new Date().toISOString(),
                    xp: 0,
                    level: 1,
                    stats: { totalAttempted: 0, correctAnswers: 0, wrongAnswers: 0, examsCompleted: 0 }
                };
                await userRef.set(newProfile);
                userProfile = newProfile;
            } else {
                userProfile = doc.data();
            }
            updateUIWithUserData();
            
            if (ADMIN_EMAILS.includes(user.email)) {
                document.getElementById('adminPanelBtn').style.display = 'flex';
            } else {
                document.getElementById('adminPanelBtn').style.display = 'none';
            }
        }
        
        async function updateUserXP(questionCorrect) {
            if (!currentUser) return;
            let xpGain = questionCorrect ? XP_CONFIG.CORRECT : XP_CONFIG.WRONG;
            let newXP = userProfile.xp + xpGain;
            let newLevel = getLevelFromXP(newXP);
            userProfile.xp = newXP;
            userProfile.level = newLevel;
            userProfile.stats.totalAttempted++;
            if (questionCorrect) userProfile.stats.correctAnswers++;
            else userProfile.stats.wrongAnswers++;
            const userRef = db.collection('users').doc(currentUser.uid);
            await userRef.update({ xp: newXP, level: newLevel, stats: userProfile.stats });
            updateUIWithUserData();
        }
        
        async function addExamCompletionBonus() {
            if (!currentUser) return;
            let newXP = userProfile.xp + XP_CONFIG.EXAM_COMPLETE_BONUS;
            let newLevel = getLevelFromXP(newXP);
            userProfile.xp = newXP;
            userProfile.level = newLevel;
            userProfile.stats.examsCompleted++;
            const userRef = db.collection('users').doc(currentUser.uid);
            await userRef.update({ xp: newXP, level: newLevel, stats: userProfile.stats });
            updateUIWithUserData();
        }
        
        function updateUIWithUserData() {
            if (userProfile) {
                document.getElementById('greetingText').innerHTML = `স্বাগতম, ${userProfile.name}! 🎓`;
                document.getElementById('authStatus').innerHTML = `<i class="fas fa-user-check"></i> ${userProfile.name}`;
            }
            renderProfilePage();
        }
        
        function renderProfilePage() {
            if (!userProfile) return;
            let currentLevelXP = getXPForNextLevel(userProfile.xp);
            let previousLevelXP = 0;
            let level = userProfile.level;
            if (level === 1) previousLevelXP = 0;
            else if (level === 2) previousLevelXP = 100;
            else if (level === 3) previousLevelXP = 250;
            else if (level === 4) previousLevelXP = 500;
            else if (level === 5) previousLevelXP = 800;
            else if (level === 6) previousLevelXP = 1200;
            else if (level === 7) previousLevelXP = 1700;
            else if (level === 8) previousLevelXP = 2300;
            else if (level === 9) previousLevelXP = 3000;
            else previousLevelXP = 3800 + (level - 10) * 1000;
            
            let xpProgress = userProfile.xp - previousLevelXP;
            let xpNeeded = currentLevelXP - previousLevelXP;
            let progressPercent = (xpProgress / xpNeeded) * 100;
            let accuracy = userProfile.stats.totalAttempted > 0 ? ((userProfile.stats.correctAnswers / userProfile.stats.totalAttempted) * 100).toFixed(1) : 0;
            
            document.getElementById('profileContent').innerHTML = `
                <div class="profile-card">
                    <div class="profile-avatar">${userProfile.avatar || '👨‍🎓'}</div>
                    <div class="profile-name">${userProfile.name}</div>
                    <div class="profile-email">${userProfile.email}</div>
                    <div class="join-date">জয়েন করেছেন: ${new Date(userProfile.joinDate).toLocaleDateString('bn-BD')}</div>
                </div>
                <div class="xp-section">
                    <div style="display: flex; justify-content: space-between; margin-bottom: 10px;">
                        <span><i class="fas fa-star"></i> মোট XP</span>
                        <span class="xp-value">${userProfile.xp}</span>
                    </div>
                    <div style="display: flex; justify-content: space-between; margin-bottom: 5px;">
                        <span><i class="fas fa-chart-line"></i> লেভেল ${userProfile.level}</span>
                        <span>${xpProgress}/${xpNeeded} XP</span>
                    </div>
                    <div class="progress-bar-container"><div class="progress-bar-fill" style="width: ${progressPercent}%"></div></div>
                </div>
                <div class="stats-grid-profile">
                    <div class="stat-item"><div class="value">${userProfile.stats.totalAttempted}</div><div class="label">মোট MCQ</div></div>
                    <div class="stat-item"><div class="value">${userProfile.stats.correctAnswers}</div><div class="label">সঠিক উত্তর</div></div>
                    <div class="stat-item"><div class="value">${accuracy}%</div><div class="label">নির্ভুলতা</div></div>
                    <div class="stat-item"><div class="value">${userProfile.stats.examsCompleted}</div><div class="label">পরীক্ষা সম্পন্ন</div></div>
                </div>
                <button class="login-btn logout-btn" onclick="logoutUser()"><i class="fas fa-sign-out-alt"></i> লগআউট</button>
            `;
        }
        
        function showLoginForm() {
            document.getElementById('loginForm').style.display = 'block';
            document.getElementById('registerForm').style.display = 'none';
            document.getElementById('loginError').style.display = 'none';
            document.getElementById('registerError').style.display = 'none';
        }
        
        function showRegisterForm() {
            document.getElementById('loginForm').style.display = 'none';
            document.getElementById('registerForm').style.display = 'block';
            document.getElementById('loginError').style.display = 'none';
            document.getElementById('registerError').style.display = 'none';
        }
        
        async function handleLogin() {
            const email = document.getElementById('loginEmail').value;
            const password = document.getElementById('loginPassword').value;
            const errorDiv = document.getElementById('loginError');
            if (!email || !password) {
                errorDiv.innerText = 'ইমেইল এবং পাসওয়ার্ড দিন';
                errorDiv.style.display = 'block';
                return;
            }
            try {
                await auth.signInWithEmailAndPassword(email, password);
            } catch(e) {
                errorDiv.innerText = 'লগইন ব্যর্থ: ' + e.message;
                errorDiv.style.display = 'block';
            }
        }
        
        async function handleRegister() {
            const email = document.getElementById('registerEmail').value;
            const password = document.getElementById('registerPassword').value;
            const errorDiv = document.getElementById('registerError');
            if (!email || !password) {
                errorDiv.innerText = 'ইমেইল এবং পাসওয়ার্ড দিন';
                errorDiv.style.display = 'block';
                return;
            }
            if (password.length < 6) {
                errorDiv.innerText = 'পাসওয়ার্ড কমপক্ষে ৬ অক্ষরের হতে হবে';
                errorDiv.style.display = 'block';
                return;
            }
            try {
                await auth.createUserWithEmailAndPassword(email, password);
            } catch(e) {
                errorDiv.innerText = 'রেজিস্টার ব্যর্থ: ' + e.message;
                errorDiv.style.display = 'block';
            }
        }
        
        async function logoutUser() { await auth.signOut(); }
        
        function showAdminModal() { 
            document.getElementById('adminModal').style.display = 'block'; 
            renderAdminLists(); 
        }
        function closeAdminModal() { document.getElementById('adminModal').style.display = 'none'; }
        
        function switchTab(tab) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
            document.getElementById(tab + 'Tab').classList.add('active');
            event.target.classList.add('active');
            renderAdminLists();
        }
        
        async function renderAdminLists() {
            try {
                const classesSnap = await db.collection('science_water_classes').get();
                const allClasses = classesSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                const classesDiv = document.getElementById('classesList');
                if (allClasses.length === 0) {
                    classesDiv.innerHTML = '<div class="list-item">কোন ক্লাস নেই</div>';
                } else {
                    classesDiv.innerHTML = allClasses.map(c => `
                        <div class="list-item">
                            <span><strong>${c.name || 'নাম নেই'}</strong><br><small>${c.category || '?'} | ${c.subject || '?'}</small></span>
                            <div><button class="delete-btn-admin" onclick="deleteClass('${c.id}')">🗑️</button></div>
                        </div>
                    `).join('');
                }
                
                const messagesSnap = await db.collection('science_water_messages').get();
                const allMessages = messagesSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                const noticesDiv = document.getElementById('noticesList');
                if (allMessages.length === 0) {
                    noticesDiv.innerHTML = '<div class="list-item">কোন নোটিশ নেই</div>';
                } else {
                    noticesDiv.innerHTML = allMessages.map(m => `
                        <div class="list-item">
                            <span><strong>${m.title || 'শিরোনাম নেই'}</strong><br><small>${m.date ? new Date(m.date).toLocaleDateString() : 'তারিখ নেই'}</small></span>
                            <div><button class="delete-btn-admin" onclick="deleteNotice('${m.id}')">🗑️</button></div>
                        </div>
                    `).join('');
                }
                
                const mcqSnap = await db.collection('science_water_mcq_exams').get();
                const allMcqs = mcqSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                const mcqsDiv = document.getElementById('mcqsList');
                if (allMcqs.length === 0) {
                    mcqsDiv.innerHTML = '<div class="list-item">কোন MCQ নেই</div>';
                } else {
                    mcqsDiv.innerHTML = allMcqs.map(e => `
                        <div class="list-item">
                            <span><strong>${e.title || 'নাম নেই'}</strong><br><small>${e.category || '?'} | ${e.totalQuestions || 0} প্রশ্ন</small></span>
                            <div><button class="delete-btn-admin" onclick="deleteMCQ('${e.id}')">🗑️</button></div>
                        </div>
                    `).join('');
                }
                
                const routineSnap = await db.collection('science_water_routine').get();
                const allRoutine = routineSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                const routineDiv = document.getElementById('routineList');
                if (allRoutine.length === 0) {
                    routineDiv.innerHTML = '<div class="list-item">কোন রুটিন নেই</div>';
                } else {
                    routineDiv.innerHTML = allRoutine.map(r => `
                        <div class="list-item">
                            <span><strong>${r.day || '?'} : ${r.time || '?'}</strong><br><small>${r.className || 'ক্লাসের নাম নেই'}</small></span>
                            <div><button class="delete-btn-admin" onclick="deleteRoutine('${r.id}')">🗑️</button></div>
                        </div>
                    `).join('');
                }
                
                const thumbSnap = await db.collection('science_water_thumbnails').get();
                const allThumbs = thumbSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                const thumbsDiv = document.getElementById('thumbnailsList');
                if (allThumbs.length === 0) {
                    thumbsDiv.innerHTML = '<div class="list-item">কোন থাম্বনেইল নেই</div>';
                } else {
                    thumbsDiv.innerHTML = allThumbs.map(t => `
                        <div class="list-item">
                            <span><strong>${t.title || 'নাম নেই'}</strong><br><small>${t.category || '?'}</small></span>
                            <div><button class="delete-btn-admin" onclick="deleteThumbnail('${t.id}')">🗑️</button></div>
                        </div>
                    `).join('');
                }
            } catch(e) { console.error("Error rendering admin lists:", e); }
        }
        
        async function addClass() {
            const name = document.getElementById('adminClassName').value;
            const description = document.getElementById('adminClassDesc').value;
            let videoId = document.getElementById('adminClassVideoId').value;
            const category = document.getElementById('adminClassCategory').value;
            const subject = document.getElementById('adminClassSubject').value;
            
            if (!name || !videoId) { 
                alert('ক্লাসের নাম এবং ভিডিও আইডি/লিংক দিন'); 
                return; 
            }
            
            try {
                await db.collection('science_water_classes').add({ 
                    name, description, videoId, category, subject, 
                    createdAt: new Date().toISOString() 
                });
                alert('✅ ক্লাস যোগ হয়েছে!');
                document.getElementById('adminClassName').value = '';
                document.getElementById('adminClassDesc').value = '';
                document.getElementById('adminClassVideoId').value = '';
                await loadAllData();
                await renderAdminLists();
            } catch(e) { 
                alert('ক্লাস যোগ করতে সমস্যা: ' + e.message); 
            }
        }
        
        async function deleteClass(id) {
            if (confirm('এই ক্লাসটি মুছে ফেলতে চান?')) {
                try {
                    await db.collection('science_water_classes').doc(id).delete();
                    alert('✅ ক্লাস মুছে ফেলা হয়েছে');
                    await loadAllData();
                    await renderAdminLists();
                } catch(e) { alert('ডিলিট করতে সমস্যা: ' + e.message); }
            }
        }
        
        async function addNotice() {
            const title = document.getElementById('adminNoticeTitle').value;
            const content = document.getElementById('adminNoticeContent').value;
            if (!title || !content) { alert('শিরোনাম এবং বিবরণ দিন'); return; }
            await db.collection('science_water_messages').add({ title, content, date: new Date().toISOString() });
            alert('✅ নোটিশ যোগ হয়েছে');
            document.getElementById('adminNoticeTitle').value = '';
            document.getElementById('adminNoticeContent').value = '';
            await loadAllData();
            await renderAdminLists();
        }
        
        async function deleteNotice(id) {
            if (confirm('নোটিশ মুছে ফেলতে চান?')) {
                await db.collection('science_water_messages').doc(id).delete();
                alert('✅ নোটিশ মুছে ফেলা হয়েছে');
                await loadAllData();
                await renderAdminLists();
            }
        }
        
        async function addMCQ() {
            const title = document.getElementById('mcqTitle').value;
            const subjectName = document.getElementById('mcqSubject').value;
            const category = document.getElementById('mcqCategory').value;
            const timeLimit = parseInt(document.getElementById('mcqTimeLimit').value);
            const questionsJson = document.getElementById('mcqQuestionsJson').value;
            if (!title || !questionsJson) { alert('শিরোনাম এবং প্রশ্ন দিন'); return; }
            try {
                const questions = JSON.parse(questionsJson);
                await db.collection('science_water_mcq_exams').add({ title, subjectName, category, timeLimit, totalQuestions: questions.length, questions });
                alert('✅ MCQ যোগ হয়েছে');
                document.getElementById('mcqTitle').value = '';
                document.getElementById('mcqSubject').value = '';
                document.getElementById('mcqTimeLimit').value = '';
                document.getElementById('mcqQuestionsJson').value = '';
                await loadAllData();
                await renderAdminLists();
            } catch(e) { alert('JSON ফরম্যাট ঠিক করুন: ' + e.message); }
        }
        
        async function deleteMCQ(id) {
            if (confirm('MCQ পরীক্ষা মুছে ফেলতে চান?')) {
                await db.collection('science_water_mcq_exams').doc(id).delete();
                alert('✅ MCQ মুছে ফেলা হয়েছে');
                await loadAllData();
                await renderAdminLists();
            }
        }
        
        async function addRoutine() {
            const day = document.getElementById('routineDay').value;
            const time = document.getElementById('routineTime').value;
            const className = document.getElementById('routineClassName').value;
            if (!time || !className) { alert('সময় এবং ক্লাসের নাম দিন'); return; }
            await db.collection('science_water_routine').add({ day, time, className });
            alert('✅ রুটিন যোগ হয়েছে');
            document.getElementById('routineTime').value = '';
            document.getElementById('routineClassName').value = '';
            await loadAllData();
            await renderAdminLists();
        }
        
        async function deleteRoutine(id) {
            if (confirm('রুটিন মুছে ফেলতে চান?')) {
                await db.collection('science_water_routine').doc(id).delete();
                alert('✅ রুটিন মুছে ফেলা হয়েছে');
                await loadAllData();
                await renderAdminLists();
            }
        }
        
        async function addThumbnail() {
            const title = document.getElementById('thumbTitle').value;
            const imageUrl = document.getElementById('thumbImageUrl').value;
            const videoLink = document.getElementById('thumbVideoLink').value;
            const category = document.getElementById('thumbCategory').value;
            if (!title || !imageUrl) { alert('শিরোনাম এবং ইমেজ URL দিন'); return; }
            await db.collection('science_water_thumbnails').add({ title, imageUrl, videoLink, category });
            alert('✅ থাম্বনেইল যোগ হয়েছে');
            document.getElementById('thumbTitle').value = '';
            document.getElementById('thumbImageUrl').value = '';
            document.getElementById('thumbVideoLink').value = '';
            await loadAllData();
            await renderAdminLists();
        }
        
        async function deleteThumbnail(id) {
            if (confirm('থাম্বনেইল মুছে ফেলতে চান?')) {
                await db.collection('science_water_thumbnails').doc(id).delete();
                alert('✅ থাম্বনেইল মুছে ফেলা হয়েছে');
                await loadAllData();
                await renderAdminLists();
            }
        }
        
        async function bulkUpload() {
            const jsonData = document.getElementById('bulkJsonData').value;
            if (!jsonData) { alert('JSON ডেটা দিন'); return; }
            try {
                const data = JSON.parse(jsonData);
                if (data.classes) for (let cls of data.classes) await db.collection('science_water_classes').add(cls);
                if (data.notices) for (let notice of data.notices) await db.collection('science_water_messages').add(notice);
                if (data.mcqs) for (let mcq of data.mcqs) await db.collection('science_water_mcq_exams').add(mcq);
                if (data.routine) for (let r of data.routine) await db.collection('science_water_routine').add(r);
                if (data.thumbnails) for (let t of data.thumbnails) await db.collection('science_water_thumbnails').add(t);
                alert('✅ Bulk আপলোড সফল!');
                document.getElementById('bulkJsonData').value = '';
                await loadAllData();
                await renderAdminLists();
            } catch(e) { alert('JSON পার্স করতে সমস্যা: ' + e.message); }
        }
        
        function initFirebase() {
            try {
                firebase.initializeApp(FIREBASE_CONFIG);
                db = firebase.firestore();
                auth = firebase.auth();
                console.log("Firebase Connected");
                auth.onAuthStateChanged(async (user) => {
                    const overlay = document.getElementById('loginOverlay');
                    const mainApp = document.getElementById('mainAppContent');
                    if (user) {
                        currentUser = user;
                        await initializeUserProfile(user);
                        overlay.classList.add('hidden');
                        mainApp.classList.add('visible');
                        loadAllData();
                        renderSSCOptions();
                    } else {
                        currentUser = null;
                        userProfile = null;
                        overlay.classList.remove('hidden');
                        mainApp.classList.remove('visible');
                        document.getElementById('adminPanelBtn').style.display = 'none';
                        document.getElementById('loginEmail').value = '';
                        document.getElementById('loginPassword').value = '';
                        showLoginForm();
                    }
                });
            } catch(e) { console.error("Firebase error:", e); alert('Firebase সংযোগ সমস্যা!'); }
        }
        
        async function loadAllData() {
            if(!db) return;
            try {
                const classesSnap = await db.collection('science_water_classes').get();
                classes = classesSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                
                const messagesSnap = await db.collection('science_water_messages').get();
                messages = messagesSnap.docs.map(d => ({ id: d.id, ...d.data() })).sort((a,b)=>new Date(b.date)-new Date(a.date));
                
                const routineSnap = await db.collection('science_water_routine').get();
                routine = routineSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                
                const notesSnap = await db.collection('science_water_spoken_notes').get();
                spokenNotes = notesSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                
                const thumbSnap = await db.collection('science_water_thumbnails').get();
                thumbnails = thumbSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                
                const mcqSnap = await db.collection('science_water_mcq_exams').get();
                mcqExams = mcqSnap.docs.map(d => ({ id: d.id, ...d.data() }));
                
                updateStats();
                loadStudentMessages();
                loadStudentRoutine();
                renderThumbnailGallery();
                renderMCQExamGallery();
            } catch(e) { console.error(e); }
        }
        
        function updateStats() {
            document.getElementById('totalClasses').innerText = classes.length;
            document.getElementById('totalExams').innerText = mcqExams.length;
            document.getElementById('totalNotices').innerText = messages.length;
        }
        
        function navigateToPage(pageId) {
            document.querySelectorAll('.page-content').forEach(p => p.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            [...document.querySelectorAll('.nav-item')].find(n => n.dataset.page === pageId)?.classList.add('active');
            if(pageId === 'sscPage') renderSSCOptions();
        }
        
        function loadStudentMessages() {
            let con = document.getElementById('studentMessages');
            con.innerHTML = messages.length ? messages.map(m => `<div class="message-box"><div class="message-title">📢 ${m.title}</div><p>${m.content}</p><small>${new Date(m.date).toLocaleDateString()}</small></div>`).join('') : '<div class="loading-spinner">কোন নোটিশ নেই</div>';
        }
        
        function loadStudentRoutine() {
            let con = document.getElementById('studentRoutine');
            con.innerHTML = '';
            let days = {saturday:'শনিবার',sunday:'রবিবার',monday:'সোমবার',tuesday:'মঙ্গলবার',wednesday:'বুধবার',thursday:'বৃহস্পতিবার',friday:'শুক্রবার'};
            Object.keys(days).forEach(day=>{let filtered = routine.filter(r=>r.day===day); if(filtered.length){let html=`<div class="routine-day"><div class="day-header">${days[day]}</div>`; filtered.forEach(c=>{html+=`<div class="class-slot"><span>${c.time}</span> <span>${c.className}</span></div>`;}); html+=`</div>`; con.innerHTML+=html;}});
            if(!routine.length) con.innerHTML='<div class="loading-spinner">কোন রুটিন নেই</div>';
        }
        
        function showSSCOptions() {
            navigateToPage('sscPage');
        }
        
        function renderSSCOptions() {
            let container = document.getElementById('sscOptionsContainer');
            if(!container) return;
            
            container.innerHTML = `
                <div class="ssc-option-card" onclick="verifySSCOption('science')">
                    <i class="fas fa-flask"></i>
                    <h3>বিজ্ঞান বিভাগ</h3>
                    <p>পদার্থ, রসায়ন, জীববিজ্ঞান, সাধারণ গণিত, বাংলা, ইংরেজি</p>
                </div>
                <div class="ssc-option-card" onclick="verifySSCOption('arts')">
                    <i class="fas fa-palette"></i>
                    <h3>মানবিক বিভাগ</h3>
                    <p>বাংলা, ইংরেজি, সাধারণ গণিত, বিজ্ঞান, আইসিটি</p>
                </div>
                <div class="ssc-option-card" onclick="verifySSCOption('commerce')">
                    <i class="fas fa-chart-line"></i>
                    <h3>বাণিজ্য বিভাগ</h3>
                    <p>বাংলা, ইংরেজি, সাধারণ গণিত, বিজ্ঞান, আইসিটি, হিসাব বিজ্ঞান, ফিন্যান্স, ব্যবসায় উদ্যোগ</p>
                </div>
            `;
        }
        
        function verifySSCOption(option) {
            let password = prompt(`${option === 'science' ? 'বিজ্ঞান' : option === 'arts' ? 'মানবিক' : 'বাণিজ্য'} বিভাগের পিন কোড দিন:`);
            if(password === SSC_OPTIONS_PASSWORDS[option]) {
                showSubjectSelectionForSSC(option);
            } else if(password) {
                alert("ভুল পিন!");
            }
        }
        
        function showSubjectSelectionForSSC(option) {
            let container = document.getElementById('studentSubjectSelection');
            let btns = document.getElementById('studentSubjectButtons');
            let subjectsList = option === 'science' ? SSC_SUBJECTS : (option === 'arts' ? ARTS_SUBJECTS : COMMERCE_SUBJECTS);
            document.getElementById('studentCategoryTitle').innerHTML = `এসএসসি - ${option === 'science' ? 'বিজ্ঞান' : option === 'arts' ? 'মানবিক' : 'বাণিজ্য'} বিভাগ`;
            btns.innerHTML = '';
            
            subjectsList.forEach(sub=>{
                let btn = document.createElement('button'); 
                btn.className='subject-btn'; 
                btn.innerText=sub.name; 
                btn.onclick=()=>showClasses('ssc', sub.id); 
                btns.appendChild(btn);
            });
            container.style.display='block';
        }
        
        function verifyStudentAccess(cat) { 
            let p = prompt(`${cat.toUpperCase()} পিন:`); 
            if(p === CLASS_PASSWORDS[cat]) showSubjectSelection(cat); 
            else if(p) alert("ভুল পিন!"); 
        }
        
        function verifySpokenEnglishAccess() { 
            let p = prompt(`স্পোকেন ইংলিশ পিন:`); 
            if(p === CLASS_PASSWORDS.spoken) showSubjectSelection('spoken'); 
            else if(p) alert("ভুল পিন!"); 
        }
        
        function showSubjectSelection(cat) {
            let container = document.getElementById('studentSubjectSelection');
            let btns = document.getElementById('studentSubjectButtons');
            let displayName = cat === 'madrasah' ? 'মাদ্রাসা বোর্ড' : cat.toUpperCase();
            document.getElementById('studentCategoryTitle').innerText = displayName + " বিষয়";
            btns.innerHTML = '';
            let subjectsToShow = cat === 'madrasah' ? MADRASAH_SUBJECTS : (cat === 'hsc' ? HSC_SUBJECTS : (cat === 'spoken' ? SPOKEN_SUBJECTS : []));
            if (subjectsToShow.length) {
                subjectsToShow.forEach(sub=>{
                    let btn = document.createElement('button'); 
                    btn.className='subject-btn'; 
                    btn.innerText=sub.name; 
                    btn.onclick=()=>showClasses(cat,sub.id); 
                    btns.appendChild(btn);
                });
            }
            container.style.display='block';
        }
        
        function showClasses(cat,sub) {
            document.querySelector('.virtual-scroll-container').style.display='block';
            let content = document.getElementById('studentClassesContent');
            let filtered = classes.filter(c=>c.category===cat && c.subject===sub);
            
            let borderColor = cat === 'ssc' ? 'var(--ssc-color)' : (cat === 'hsc' ? 'var(--hsc-color)' : (cat === 'spoken' ? 'var(--spoken-color)' : 'var(--madrasah-color)'));
            
            let subjectsList = cat === 'ssc' ? SSC_SUBJECTS : (cat === 'arts' ? ARTS_SUBJECTS : (cat === 'commerce' ? COMMERCE_SUBJECTS : (cat === 'madrasah' ? MADRASAH_SUBJECTS : (cat === 'hsc' ? HSC_SUBJECTS : SPOKEN_SUBJECTS))));
            content.innerHTML = `<h3>${subjectsList?.find(s=>s.id===sub)?.name||sub}</h3>` + 
                (filtered.length ? filtered.map(c=>{
                    let videoUrl = getYouTubeUrl(c.videoId);
                    return `<div class="class-card" style="border-left: 5px solid ${borderColor};">
                        <h3>${c.name || 'ক্লাসের নাম নেই'}</h3>
                        <p>${c.description || 'কোনো বিবরণ নেই'}</p>
                        ${videoUrl ? `<a href="${videoUrl}" target="_blank" class="link-btn" style="display:inline-block; text-decoration:none;">🎬 ভিডিও দেখুন</a>` : '<p style="color:red;">ভিডিও লিংক নেই</p>'}
                    </div>`;
                }).join('') : '<p>কোন ক্লাস নেই</p>');
        }
        
        function openSpokenNotes(category) {
            let filtered = spokenNotes.filter(n=>n.category===category);
            if(!filtered.length) return alert("কোন ক্লাস নোট নেই");
            document.getElementById('spokenNotesContent').innerHTML = filtered.map(n=>`<div class="class-card"><h4>📄 ${n.title}</h4><p>${n.description}</p><a href="${n.link}" target="_blank" class="link-btn">ডাউনলোড/খুলুন</a></div>`).join('');
            document.getElementById('spokenNotesModal').style.display='flex';
        }
        
        function closeSpokenNotesModal() { document.getElementById('spokenNotesModal').style.display='none'; }
        
        function renderThumbnailGallery() {
            let container = document.getElementById('thumbnailGalleryContent');
            let filtered = thumbnails.filter(t=>currentThumbnailCategory==='featured' || t.category===currentThumbnailCategory);
            container.innerHTML = filtered.length ? `<div class="thumbnail-grid">${filtered.map(t=>`<div class="thumbnail-card"><div class="thumbnail-image-container"><img src="${t.imageUrl}" onerror="this.src='https://via.placeholder.com/300'"></div><div class="thumbnail-content"><h4>${t.title}</h4>${t.videoLink?`<a href="${t.videoLink}" target="_blank" class="link-btn">ভিডিও দেখুন</a>`:''}</div></div>`).join('')}</div>` : '<div class="loading-spinner">কোনো থাম্বনেইল নেই</div>';
        }
        
        function filterThumbnails(cat) { currentThumbnailCategory = cat; renderThumbnailGallery(); }
        
        function renderMCQExamGallery() {
            let container = document.getElementById('mcqExamContent');
            let filtered = mcqExams.filter(e=>currentMCQCategory==='all' || e.category===currentMCQCategory);
            container.innerHTML = filtered.length ? filtered.map(ex=>`<div class="mcq-exam-card"><h4>${ex.title}</h4><p>${ex.subjectName} | ${ex.totalQuestions} প্রশ্ন | ${ex.timeLimit} মিনিট</p><button class="start-btn" onclick="startMCQExam('${ex.id}')">পরীক্ষা শুরু</button></div>`).join('') : '<div class="loading-spinner">কোন পরীক্ষা নেই</div>';
        }
        
        function filterMCQExams(category) { currentMCQCategory = category; renderMCQExamGallery(); }
        
        async function startMCQExam(examId) {
            if (!currentUser) return;
            currentExam = mcqExams.find(e=>e.id===examId);
            if(!currentExam) return;
            currentQuestionIndex=0;
            userAnswers=[];
            timeRemaining=currentExam.timeLimit*60;
            currentExam.questions.forEach((q,i)=>userAnswers[i]={questionId:q.id,selectedAnswer:null});
            showMCQInterface();
            if(examTimer) clearInterval(examTimer);
            examTimer = setInterval(()=>{timeRemaining--; let timerDiv=document.getElementById('mcqTimer'); if(timerDiv)timerDiv.innerText=`${Math.floor(timeRemaining/60)}:${(timeRemaining%60).toString().padStart(2,'0')}`; if(timeRemaining<=0){clearInterval(examTimer); submitMCQExam();}},1000);
        }
        
        function showMCQInterface() { document.getElementById('mcqExamInterface').style.display='block'; loadMCQQuestion(); }
        
        function loadMCQQuestion() {
            let q = currentExam.questions[currentQuestionIndex];
            let ua = userAnswers[currentQuestionIndex];
            document.getElementById('mcqExamInterfaceContent').innerHTML = `<div style="display:flex; justify-content:space-between; margin-bottom:20px;"><h3>${currentExam.title}</h3><div id="mcqTimer">${Math.floor(timeRemaining/60)}:${(timeRemaining%60).toString().padStart(2,'0')}</div></div><div style="margin-bottom:20px;"><strong>প্রশ্ন ${currentQuestionIndex+1}:</strong> ${q.text}</div><div>${Object.entries(q.options).map(([l,t])=>`<div class="mcq-option ${ua.selectedAnswer===l?'selected':''}" onclick="selectMCQAnswer('${l}')"><span style="width:30px;height:30px;background:#eee;border-radius:50%;display:flex;align-items:center;justify-content:center;">${l}</span><span>${t}</span></div>`).join('')}</div><div style="margin-top:20px; display:flex; justify-content:space-between;"><button class="link-btn" style="width:auto;" onclick="prevMCQQuestion()" ${currentQuestionIndex===0?'disabled':''}>পূর্ববর্তী</button>${currentQuestionIndex<currentExam.questions.length-1?`<button class="link-btn" style="width:auto;" onclick="nextMCQQuestion()">পরবর্তী</button>`:`<button class="login-btn" style="width:auto;" onclick="submitMCQExam()">জমা দিন</button>`}</div><button onclick="closeMCQExam()" style="margin-top:20px; background:none; border:none; color:gray;">বন্ধ করুন</button>`;
        }
        
        function selectMCQAnswer(letter) { userAnswers[currentQuestionIndex].selectedAnswer = letter; loadMCQQuestion(); }
        function nextMCQQuestion() { if(currentQuestionIndex < currentExam.questions.length-1){ currentQuestionIndex++; loadMCQQuestion(); } }
        function prevMCQQuestion() { if(currentQuestionIndex > 0){ currentQuestionIndex--; loadMCQQuestion(); } }
        
        async function submitMCQExam() {
            clearInterval(examTimer);
            let correct=0;
            for(let i=0; i<currentExam.questions.length; i++) {
                if(userAnswers[i].selectedAnswer===currentExam.questions[i].correctAnswer) {
                    correct++;
                    await updateUserXP(true);
                } else if(userAnswers[i].selectedAnswer) {
                    await updateUserXP(false);
                }
            }
            await addExamCompletionBonus();
            let score = Math.round((correct/currentExam.questions.length)*100);
            document.getElementById('mcqExamInterfaceContent').innerHTML = `<div style="text-align:center; padding:20px;"><i class="fas fa-trophy" style="font-size:48px; color:gold;"></i><h2>ফলাফল</h2><h1 style="color:var(--primary); font-size:40px;">${score}%</h1><p>সঠিক: ${correct}/${currentExam.questions.length}</p><button class="login-btn" onclick="closeMCQExam()">বন্ধ করুন</button></div>`;
        }
        
        function closeMCQExam() { document.getElementById('mcqExamInterface').style.display='none'; if(examTimer) clearInterval(examTimer); }
        
        document.addEventListener('DOMContentLoaded', () => {
            initFirebase();
            document.querySelectorAll('.nav-item').forEach(item => { item.addEventListener('click', function(e) { e.preventDefault(); document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active')); this.classList.add('active'); document.querySelectorAll('.page-content').forEach(p=>p.classList.remove('active')); document.getElementById(this.dataset.page).classList.add('active'); if(this.dataset.page === 'sscPage') renderSSCOptions(); }); });
            document.getElementById('dateDisplay').innerText = new Date().toLocaleDateString('bn-BD', { year:'numeric', month:'long', day:'numeric' });
            updateSubjectOptions();
        });
    </script>
</body>
</html>
