<!DOCTYPE html>
<html lang="sw">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>E-Coin - Official App</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Roboto, Arial, sans-serif;
            -webkit-user-select: none;
            user-select: none;
        }
        body {
            background-color: #000000;
            color: #ffffff;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            overflow: hidden;
        }
        .app-container {
            width: 100%;
            max-width: 430px;
            height: 100vh;
            background: linear-gradient(180deg, #161001 0%, #000000 100%);
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 15px;
            position: relative;
        }

        /* --- TOP PROFILE SECTION --- */
        .top-profile {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: rgba(30, 30, 30, 0.5);
            padding: 12px;
            border-radius: 16px;
            border: 1px solid #2d2002;
        }
        .profile-left {
            display: flex;
            align-items: center;
            gap: 12px;
        }
        .avatar-container {
            position: relative;
            width: 45px;
            height: 45px;
        }
        .avatar-img {
            width: 100%;
            height: 100%;
            border-radius: 12px;
            object-fit: cover;
            border: 2px solid #f3ba2f;
            background-color: #222;
        }
        .file-input {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            opacity: 0;
            cursor: pointer;
        }
        .user-meta h4 {
            font-size: 14px;
            font-weight: bold;
            color: #fff;
        }
        .level-container {
            display: flex;
            align-items: center;
            gap: 6px;
            margin-top: 4px;
        }
        .level-title {
            font-size: 11px;
            color: #f3ba2f;
            font-weight: 600;
        }
        .level-bar-container {
            width: 120px;
            height: 8px;
            background: #252525;
            border-radius: 6px;
            overflow: hidden;
            border: 1px solid #444;
        }
        .level-bar-fill {
            width: 0%; /* Inasogea yenyewe kwa JavaScript */
            height: 100%;
            background: linear-gradient(90deg, #ffe066, #f3ba2f);
            transition: width 0.3s ease;
        }

        /* --- TABS CONTENT MANAGEMENT --- */
        .tab-content {
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            flex-grow: 1;
            width: 100%;
            padding: 20px 0;
        }
        .tab-content.active {
            display: flex;
        }

        /* --- TAB 1: EXCHANGE (MAIN CLICKER) --- */
        .balance-box {
            text-align: center;
            margin-bottom: 25px;
        }
        .coin-balance {
            font-size: 46px;
            font-weight: 800;
            color: #ffffff;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
        }
        .gold-coin-icon {
            width: 40px;
            height: 40px;
            background: radial-gradient(circle, #ffe066 0%, #f3ba2f 70%, #947116 100%);
            border-radius: 50%;
            box-shadow: 0 0 12px rgba(243, 186, 47, 0.6);
        }
        .clicker-container {
            position: relative;
            width: 250px;
            height: 250px;
            margin-bottom: 20px;
        }
        .clicker-circle {
            width: 100%;
            height: 100%;
            background: radial-gradient(circle, #2d2102 0%, #090600 100%);
            border-radius: 50%;
            border: 6px solid #f3ba2f;
            box-shadow: 0 0 35px rgba(243, 186, 47, 0.25);
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            transition: transform 0.05s ease;
        }
        .clicker-circle:active {
            transform: scale(0.96);
        }
        .character-emoji {
            font-size: 115px;
        }

        /* Floating +1 Text Anim */
        .floating-text {
            position: absolute;
            color: #ffffff;
            font-size: 30px;
            font-weight: bold;
            pointer-events: none;
            animation: floatUp 0.6s ease-out forwards;
            text-shadow: 0 3px 6px rgba(0,0,0,0.9);
        }
        @keyframes floatUp {
            0% { opacity: 1; transform: translateY(0) scale(1); }
            100% { opacity: 0; transform: translateY(-90px) scale(1.2); }
        }

        .energy-boost-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: 100%;
            padding: 0 15px;
        }
        .energy-box {
            font-size: 16px;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        .boost-link {
            background: none;
            border: none;
            color: #f3ba2f;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 5px;
        }

        /* --- INFO CARDS --- */
        .tab-title {
            font-size: 26px;
            font-weight: bold;
            color: #f3ba2f;
            margin-bottom: 10px;
            text-align: center;
        }
        .tab-subtitle {
            color: #aaa;
            font-size: 14px;
            text-align: center;
            margin-bottom: 25px;
            padding: 0 20px;
            line-height: 1.4;
        }
        .info-card {
            background: #141414;
            border: 1px solid #252525;
            border-radius: 16px;
            padding: 16px;
            width: 100%;
            margin-bottom: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .upgrade-btn {
            background: #f3ba2f;
            color: #000;
            border: none;
            padding: 10px 18px;
            border-radius: 12px;
            font-weight: bold;
            cursor: pointer;
        }

        /* --- AIRDROP TAB (BINANCE ONLY & LIVE COIN COUNTER) --- */
        .airdrop-stats {
            background: rgba(243, 186, 47, 0.05);
            border: 1px solid #3a2b03;
            border-radius: 16px;
            padding: 20px;
            width: 100%;
            text-align: center;
            margin-bottom: 20px;
        }
        .airdrop-coins {
            font-size: 32px;
            font-weight: 800;
            color: #fff;
            margin-bottom: 5px;
        }
        .airdrop-usd {
            font-size: 18px;
            color: #00ffcc;
            font-weight: bold;
        }
        .wallet-btn {
            width: 100%;
            background: #111;
            border: 2px solid #f3ba2f;
            border-radius: 16px;
            padding: 18px;
            color: #fff;
            font-size: 16px;
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: space-between;
            cursor: pointer;
        }
        .status-badge {
            font-size: 13px;
            background: #222;
            padding: 6px 14px;
            border-radius: 8px;
            color: #aaa;
        }
        .status-badge.connected {
            color: #000;
            background: #f3ba2f;
            font-weight: bold;
        }

        /* --- INTERFACE YA NDANI YA BOOST --- */
        #tab-boost-section .upgrade-btn {
            min-width: 80px;
        }

        /* --- BOTTOM NAVIGATION BAR --- */
        .bottom-nav {
            display: flex;
            justify-content: space-between;
            background: #121212;
            padding: 10px 6px;
            border-radius: 20px;
            border: 1px solid #222;
            width: 100%;
        }
        .nav-item {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
            background: none;
            border: none;
            color: #666666;
            font-size: 11px;
            font-weight: 600;
            cursor: pointer;
        }
        .nav-item .nav-icon {
            font-size: 22px;
            transition: transform 0.1s ease;
        }
        .nav-item.active {
            color: #ffffff;
        }
        .nav-item.active .nav-icon {
            transform: scale(1.15);
        }
    </style>
</head>
<body>

    <div class="app-container">
        
        <!-- TOP PROFILE AREA -->
        <div class="top-profile">
            <div class="profile-left">
                <div class="avatar-container">
                    <img id="avatar-preview" class="avatar-img" src="https://via.placeholder.com/150/222222/f3ba2f?text=CEO" alt="Profile">
                    <input type="file" id="image-loader" class="file-input" accept="image/*">
                </div>
                <div class="user-meta">
                    <h4 id="username-display">Esboni24 (CEO)</h4>
                    <div class="level-container">
                        <span id="level-title-text" class="level-title">Level 1</span>
                        <div class="level-bar-container">
                            <div id="level-bar-fill" class="level-bar-fill"></div>
                        </div>
                    </div>
                </div>
            </div>
            <div style="font-size: 20px; color: #f3ba2f; cursor: pointer;">⚙️</div>
        </div>

        <!-- ================= TABS SECTIONS ================= -->

        <!-- TAB 1: EXCHANGE (MAIN GAME) -->
        <div id="tab-exchange" class="tab-content active">
            <div class="balance-box">
                <div class="coin-balance">
                    <div class="gold-coin-icon"></div>
                    <span id="score-text">0</span>
                </div>
            </div>
            
            <div class="clicker-container">
                <div id="coin-clicker" class="clicker-circle">
                    <span class="character-emoji">🐹</span>
                </div>
            </div>

            <div class="energy-boost-row">
                <div class="energy-box">⚡ <span id="energy-text">1500</span>/1500</div>
                <button class="boost-link" onclick="openBoostSection()">🚀 Boost</button>
            </div>
        </div>

        <!-- TAB 2: MINE -->
        <div id="tab-mine" class="tab-content">
            <p class="tab-title">Mine Market</p>
            <p class="tab-subtitle">Nunua kadi ili kuongeza kipato cha passive kila saa.</p>
            <div class="info-card">
                <div>
                    <p style="font-weight: bold;">Uchimbaji wa Dhahabu</p>
                    <p style="font-size: 12px; color: #aaa;">Gharama: 500 E-Coin</p>
                </div>
                <p style="color: #f3ba2f; font-weight: bold;">+100/hr</p>
            </div>
        </div>

        <!-- TAB 3: FRIENDS (INVITE LINK) -->
        <div id="tab-mine-friends" class="tab-content">
            <p class="tab-title">👥 Invite Friends</p>
            <p class="tab-subtitle">Alika marafiki wajiunge na ujipatie pointi +5,000 kwa kila mtu.</p>
            
            <div class="info-card" style="flex-direction: column; gap: 15px; align-items: flex-start;">
                <p style="font-size: 12px; color: #f3ba2f; font-weight: bold;">LINK YAKO YA URUFIANI (REFERRAL LINK):</p>
                <p id="ref-link-text" style="font-size: 13px; color: #fff; word-break: break-all; background: #222; padding: 10px; border-radius: 8px; width: 100%;">https://t.me/EsboniBot?start=user123</p>
                <button onclick="copyInviteLink()" style="background: #f3ba2f; color: #000; border: none; padding: 12px; border-radius: 10px; font-weight: bold; width: 100%; cursor: pointer;">
                    📋 Copy Invite Link
                </button>
            </div>
        </div>

        <!-- TAB 4: EARN -->
        <div id="tab-earn" class="tab-content">
            <p class="tab-title">🪙 Earn Coins</p>
            <p class="tab-subtitle">Kamilisha kazi rahisi ili upate bonasi za papo hapo.</p>
            <div class="info-card">
                <p style="font-weight: bold;">Daily Reward</p>
                <button class="upgrade-btn">Claim</button>
            </div>
        </div>

        <!-- TAB 5: AIRDROP (BINANCE SEAMLESS & LIVE COIN-TO-USD VALUE) -->
        <div id="tab-airdrop" class="tab-content">
            <div style="font-size: 45px; margin-bottom: 10px;">💰</div>
            <p class="tab-title">Binance Airdrop</p>
            <p class="tab-subtitle">Angalia salio lako na unganisha Binance Web3 Wallet yako kupokea token zikizinduliwa blockchain.</p>

            <!-- Inaonyesha sarafu halisi na thamani ya Dola ($) -->
            <div class="airdrop-stats">
                <div class="airdrop-coins">🪙 <span id="airdrop-score-display">0</span></div>
                <div class="airdrop-usd">Value: $<span id="airdrop-usd-display">0.00</span></div>
            </div>

            <div style="width: 100%;">
                <div class="wallet-btn" onclick="connectBinanceWalletOnly()">
                    <div style="display: flex; align-items: center; gap: 12px;">
                        <span style="font-size: 24px;">🔶</span>
                        <span>Binance Web3 Wallet</span>
                    </div>
                    <span id="status-Binance" class="status-badge">Connect</span>
                </div>
            </div>
        </div>

        <!-- TAB EXTRA: INSIDE BOOST SECTION -->
        <div id="tab-boost-section" class="tab-content">
            <p class="tab-title">🚀 Multitap Boost</p>
            <p class="tab-subtitle">Upgrade Multitap yako ili kuongeza pointi kwa kila mbonyezo.</p>
            
            <div class="info-card">
                <div>
                    <p style="font-weight: bold; font-size: 16px;">👉 Multitap Upgrade</p>
                    <p id="multitap-desc" style="font-size: 12px; color: #aaa;">Level 1 (+1 per tap)</p>
                </div>
                <div>
                    <button id="btn-buy-boost" class="upgrade-btn" onclick="buyMultitapUpgrade()">
                        💰 <span id="boost-cost-text">100</span>
                    </button>
                </div>
            </div>
            
            <button style="margin-top: 20px; background: #222; color: #fff; border: 1px solid #444; padding: 10px 20px; border-radius: 10px; font-weight: bold; cursor: pointer;" onclick="goBackToExchange()">
                ◀ Back to Game
            </button>
        </div>

        <!-- ================= BOTTOM NAVIGATION BAR ================= -->
        <div class="bottom-nav" id="main-bottom-nav">
            <button id="nav-btn-exchange" class="nav-item active" onclick="switchTab('tab-exchange', this)">
                <span class="nav-icon">🐹</span>
                <span>Exchange</span>
            </button>
            <button class="nav-item" onclick="switchTab('tab-mine', this)">
                <span class="nav-icon">⛏️</span>
                <span>Mine</span>
            </button>
            <button class="nav-item" onclick="switchTab('tab-mine-friends', this)">
                <span class="nav-icon">👥</span>
                <span>Friends</span>
            </button>
            <button class="nav-item" onclick="switchTab('tab-earn', this)">
                <span class="nav-icon">🪙</span>
                <span>Earn</span>
            </button>
            <button class="nav-item" onclick="switchTab('tab-airdrop', this)">
                <span class="nav-icon">💰</span>
                <span>Airdrop</span>
            </button>
        </div>

    </div>

    <script>
        const tg = window.Telegram.WebApp;
        tg.expand();

        // --- CORE VARIABLES ---
        let score = parseInt(localStorage.getItem('adv_score')) || 0;
        let energy = parseInt(localStorage.getItem('adv_energy')) || 1500;
        let tapPower = parseInt(localStorage.getItem('adv_tappower')) || 1; 
        let boostCost = parseInt(localStorage.getItem('adv_boostcost')) || 100; 

        const scoreText = document.getElementById('score-text');
        const energyText = document.getElementById('energy-text');
        const clickerContainer = document.querySelector('.clicker-container');

        // Setup Telegram Username kama ipo
        if (tg.initDataUnsafe && tg.initDataUnsafe.user) {
            document.getElementById('username-display').textContent = tg.initDataUnsafe.user.first_name + " (CEO)";
            document.getElementById('ref-link-text').textContent = "https://t.me/EsboniBot?start=" + tg.initDataUnsafe.user.id;
        }

        // Kusafisha na kuweka data mwanzo
        refreshAllUI();

        // --- DYNAMIC LEVEL BAR ENGINE (MAHITAJI YAKO) ---
        function updateLevelProgressBar() {
            let currentLevel = 1;
            let maxCoinsForLevel = 10000; // Level 1 mwisho ni 10k
            let levelName = "Level 1 (Bronze)";

            if (score >= 100000) {
                currentLevel = 3;
                maxCoinsForLevel = 1000000;
                levelName = "Level 3 (Gold)";
            } else if (score >= 10000) {
                currentLevel = 2;
                maxCoinsForLevel = 100000;
                levelName = "Level 2 (Silver)";
            }

            document.getElementById('level-title-text').textContent = levelName;
            
            // Hesabu asilimia ya mstari kusogea
            let percentage = (score / maxCoinsForLevel) * 100;
            if (percentage > 100) percentage = 100;
            document.getElementById('level-bar-fill').style.width = percentage + "%";
        }

        // --- TICK ENGINE (TAP CONTROL) ---
        document.getElementById('coin-clicker').addEventListener('click', (e) => {
            if (energy >= tapPower) {
                score += tapPower;
                energy -= tapPower;
                
                createFloatingText(e, "+" + tapPower);
                saveData();
                refreshAllUI();
            }
        });

        function createFloatingText(e, text) {
            const el = document.createElement('div');
            el.className = 'floating-text';
            el.textContent = text;
            
            const rect = clickerContainer.getBoundingClientRect();
            let x = e.clientX ? (e.clientX - rect.left) : (rect.width / 2);
            let y = e.clientY ? (e.clientY - rect.top) : (rect.height / 2);
            
            el.style.left = x + "px";
            el.style.top = y + "px";
            
            clickerContainer.appendChild(el);
            setTimeout(() => { el.remove(); }, 600);
        }

        // --- REFRESH DATA KILA SEHEMU (PAMOJA NA AIRDROP DOLLAR) ---
        function refreshAllUI() {
            scoreText.textContent = score.toLocaleString();
            energyText.textContent = energy;
            
            // Sasisha data za Airdrop
            document.getElementById('airdrop-score-display').textContent = score.toLocaleString();
            // Mahesabu ya kubadili coin kuwa Dola halisi (Mfano: Coin 10,000 = $10.00)
            let usdValue = score * 0.001; 
            document.getElementById('airdrop-usd-display').textContent = usdValue.toFixed(2);

            updateLevelProgressBar();
            updateBoostTabUI();
        }

        // --- NAVIGATION TABS ---
        function switchTab(tabId, buttonElement) {
            const tabs = document.querySelectorAll('.tab-content');
            tabs.forEach(tab => tab.classList.remove('active'));

            const navItems = document.querySelectorAll('.nav-item');
            navItems.forEach(item => item.classList.remove('active'));

            document.getElementById(tabId).classList.add('active');
            buttonElement.classList.add('active');
            refreshAllUI();
        }

        function openBoostSection() {
            const tabs = document.querySelectorAll('.tab-content');
            tabs.forEach(tab => tab.classList.remove('active'));
            document.getElementById('tab-boost-section').classList.add('active');
        }

        function goBackToExchange() {
            const tabs = document.querySelectorAll('.tab-content');
            tabs.forEach(tab => tab.classList.remove('active'));
            document.getElementById('tab-exchange').classList.add('active');
            
            const navItems = document.querySelectorAll('.nav-item');
            navItems.forEach(item => item.classList.remove('active'));
            document.getElementById('nav-btn-exchange').classList.add('active');
        }

        // --- BOOST ENGINE ---
        function buyMultitapUpgrade() {
            if (score >= boostCost) {
                score -= boostCost;
                tapPower += 1; 
                boostCost = boostCost * 2; 
                
                saveData();
                refreshAllUI();
                alert("Upgrade Done! Sasa unapata +" + tapPower + " kila tap!");
            } else {
                alert("Huna coin za kutosha!");
            }
        }

        function updateBoostTabUI() {
            document.getElementById('multitap-desc').textContent = "Level " + tapPower + " (+" + tapPower + " per tap)";
            document.getElementById('boost-cost-text').textContent = boostCost.toLocaleString();
            
            const btn = document.getElementById('btn-buy-boost');
            if (score < boostCost) {
                btn.style.background = "#444"; btn.style.color = "#888";
            } else {
                btn.style.background = "#f3ba2f"; btn.style.color = "#000";
            }
        }

        // --- COPY LINK ENGINE ---
        function copyInviteLink() {
            const linkText = document.getElementById('ref-link-text').textContent;
            navigator.clipboard.writeText(linkText).then(() => {
                alert("Invite link copied successfully! 📋");
            }).catch(() => {
                alert("Imeshindwa ku-copy, tafadhali copy kwa mkono.");
            });
        }

        // --- BINANCE WALLET SIMULATOR (AIRDROP) ---
        function connectBinanceWalletOnly() {
            let keyName = "binance_wallet_status";
            const badge = document.getElementById('status-Binance');

            if (localStorage.getItem(keyName) === "connected") {
                localStorage.removeItem(keyName);
                badge.textContent = "Connect";
                badge.classList.remove('connected');
                alert("Binance Web3 Wallet disconnected.");
            } else {
                badge.textContent = "Connecting...";
                setTimeout(() => {
                    localStorage.setItem(keyName, "connected");
                    badge.textContent = "Connected ✔";
                    badge.classList.add('connected');
                    alert("Binance Web3 Wallet connected successfully! 🔶");
                }, 1000);
            }
        }

        // --- REGULAR ENERGY REGEN ---
        setInterval(() => {
            if (energy < 1500) {
                energy += 2;
                if (energy > 1500) energy = 1500;
                energyText.textContent = energy;
                saveData();
            }
        }, 1500);

        function saveData() {
            localStorage.setItem('adv_score', score);
            localStorage.setItem('adv_energy', energy);
            localStorage.setItem('adv_tappower', tapPower);
            localStorage.setItem('adv_boostcost', boostCost);
        }

        // --- IMAGE LOADER ---
        const imageLoader = document.getElementById('image-loader');
        const avatarPreview = document.getElementById('avatar-preview');
        if (localStorage.getItem('user_custom_avatar')) avatarPreview.src = localStorage.getItem('user_custom_avatar');

        imageLoader.addEventListener('change', function(e) {
            const reader = new FileReader();
            reader.onload = function(event) {
                avatarPreview.src = event.target.result;
                localStorage.setItem('user_custom_avatar', event.target.result);
            }
            reader.readAsDataURL(e.target.files[0]);
        });

        // Washa muonekano wa wallet mwanzo ukurasa ukifunguka
        window.addEventListener('DOMContentLoaded', () => {
            if(localStorage.getItem("binance_wallet_status") === "connected") {
                const b = document.getElementById('status-Binance');
                b.textContent = "Connected ✔";
                b.classList.add('connected');
            }
        });
    </script>
</body>
</html>
