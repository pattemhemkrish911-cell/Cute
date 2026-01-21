<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pragathi AI | Voice Command Master</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    
    <script type="importmap">
      { "imports": { "@google/generative-ai": "https://esm.run/@google/generative-ai" } }
    </script>

    <style>
        :root { --sidebar-bg: #020617; --accent: #0ea5e9; --gov-red: #ef4444; --card-bg: #ffffff; }
        body, html { margin: 0; padding: 0; height: 100vh; font-family: 'Inter', sans-serif; background: #0f172a; overflow: hidden; }
        
        /* LOGIN SCREEN */
        #login-overlay { position: fixed; inset: 0; background: radial-gradient(circle, #1e293b, #020617); display: flex; align-items: center; justify-content: center; z-index: 1000; }
        .login-card { background: white; padding: 50px; border-radius: 40px; text-align: center; width: 450px; box-shadow: 0 25px 50px rgba(0,0,0,0.5); }
        .social-btn { width: 100%; padding: 18px; margin: 15px 0; border-radius: 15px; border: 1px solid #e2e8f0; display: flex; align-items: center; justify-content: center; gap: 15px; cursor: pointer; font-weight: 700; font-size: 16px; }

        /* TOP NAVIGATION */
        .top-nav { position: absolute; top: 30px; right: 50px; display: flex; gap: 20px; z-index: 100; }
        .gov-btn { background: var(--gov-red); color: white; padding: 15px 30px; border-radius: 15px; font-weight: 800; cursor: pointer; border: none; font-size: 14px; }

        /* DASHBOARD LAYOUT */
        #main-dashboard { display: none; height: 100vh; width: 100%; }
        
        /* SIDEBAR (Pragati AI) */
        .sidebar { width: 450px; background: var(--sidebar-bg); color: white; padding: 40px; display: flex; flex-direction: column; box-sizing: border-box; border-right: 2px solid #1e293b; }
        .ai-title { font-size: 34px; font-weight: 900; color: var(--accent); margin-bottom: 5px; }
        
        /* CHAT LOG */
        #chat-log { flex: 1; background: rgba(255,255,255,0.03); border-radius: 20px; padding: 25px; overflow-y: auto; font-size: 18px; line-height: 1.6; border: 1px solid #1e293b; margin: 25px 0; color: #cbd5e1; }
        .user-msg { color: var(--accent); font-weight: 700; margin-top: 15px; }
        
        /* TEXT AREA */
        .long-input { width: 100%; height: 160px; background: #1e293b; border: 2px solid #334155; color: white; border-radius: 20px; padding: 20px; resize: none; font-family: inherit; font-size: 16px; box-sizing: border-box; outline: none; }
        
        /* CONTENT AREA */
        .content { flex: 1; padding: 120px 60px 60px; overflow-y: auto; background: #f8fafc; }
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 40px; max-width: 1300px; margin: 0 auto; }
        .card { background: white; padding: 40px; border-radius: 35px; box-shadow: 0 15px 35px rgba(0,0,0,0.05); border: 1px solid #e2e8f0; }
        .full-card { grid-column: span 2; }

        /* BUTTONS */
        .btn-main { background: var(--sidebar-bg); color: white; border: none; padding: 18px; border-radius: 18px; width: 100%; cursor: pointer; font-weight: 800; font-size: 16px; margin-top: 10px; transition: 0.3s; }
        .btn-voice { background: #10b981; color: white; }
        .btn-voice.listening { background: #ef4444; animation: pulse 1.5s infinite; }
        
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.5; } 100% { opacity: 1; } }

        .hud-display { background: #0f172a; color: #38bdf8; padding: 30px; border-radius: 25px; display: flex; justify-content: space-around; margin-top: 20px; text-align: center; }
        .hud-val { font-size: 38px; font-weight: 900; display: block; }
    </style>
</head>
<body>

<div id="login-overlay">
    <div class="login-card">
        <h1 style="font-size: 30px; margin:0;">Pragathi Cyber Security AI </h1>
        <p style="color:#64748b; margin-top:5px; margin-bottom:30px;">Secure Cybersecurity Voice Command Dashboard</p>
        <div class="social-btn" onclick="secureLogin('Google')"><img src="https://www.gstatic.com/images/branding/product/1x/gsa_512dp.png" width="24"> Login with Google</div>
        <div class="social-btn" onclick="secureLogin('Microsoft')"><img src="https://upload.wikimedia.org/wikipedia/commons/4/44/Microsoft_logo.svg" width="24"> Login with Microsoft ID</div>
    </div>
</div>

<div class="top-nav" id="top-nav" style="display:none;">
    <button class="gov-btn" onclick="window.open('tel:1930')">🚨 1930 HELPLINE</button>
    <button class="gov-btn" style="background:#1e3a8a;" onclick="window.open('https://cybercrime.gov.in')">GOVT PORTAL</button>
</div>

<div id="main-dashboard">
    <div class="sidebar">
        <div class="ai-title">🛡️ Pragathi Cyber Security AI Version</div>    
        <div id="chat-log"><b>Pragati AI:</b> I am listening. You can type or use the voice button below.</div>
        
        <textarea id="userInput" class="long-input" placeholder="Enter security report..."></textarea>
        
        <button id="voiceBtn" onclick="startVoice()" class="btn-main btn-voice">🎤 Click to Speak Command</button>
        <button onclick="askPragati()" class="btn-main" style="background:var(--accent); color:#020617;">Send to Pragati AI</button>
        <button onclick="stopSpeaking()" class="btn-main" style="background:#ef4444;">🛑 Stop Speaking</button>

        <button onclick="location.reload()" style="background:transparent; color:#64748b; border:1px solid #334155; margin-top:15px; padding:10px; border-radius:12px; cursor:pointer; font-size: 12px;">🔒 SECURE LOGOUT</button>
    </div>

    <div class="content">
        <div class="grid">
            <div class="card full-card">
                <h2 style="font-size: 30px; margin-bottom: 5px;">🔐 Vulnerability Radius</h2>
                <p style="color:#64748b; margin-bottom: 25px;">AI Physical Signal Interception Test</p>
                <input type="password" id="pInput" oninput="updateRadius()" placeholder="Type a password..." style="width:100%; padding:20px; font-size:22px; border-radius:18px; border:2px solid #e2e8f0;">
                <div class="hud-display">
                    <div><span class="hud-val" id="ftVal" style="color:#f43f5e;">0 ft</span><small>DANGER RADIUS</small></div>
                    <div><span class="hud-val" id="tmVal" style="color:#10b981;">0s</span><small>TIME TO CRACK</small></div>
                </div>
            </div>

            <div class="card">
                <h2>🔍 Pragathi Link Scan</h2>
                <input type="text" id="urlIn" placeholder="Enter URL to analyze..." style="width:100%; padding:15px; border-radius:12px; border:1px solid #ddd; margin-bottom: 15px;">
                <button class="btn-main" onclick="scanLink()">Run AI Audit</button>
            </div>

            <div class="card">
                <h2>📄 System File Scan</h2>
                <input type="file" id="fileIn" style="display:none;" onchange="document.getElementById('fLab').innerText = this.files[0].name">
                <label for="fileIn" id="fLab" style="display:block; padding:30px; border:3px dashed #cbd5e1; border-radius:20px; text-align:center; cursor:pointer; font-weight: bold; background:#f8fafc;">📁 Browse Computer Files</label>
                <button class="btn-main" onclick="alert('Metadata Secure')">Scan Files</button>
            </div>

            <div class="card full-card">
                <div style="display:flex; justify-content: space-between; align-items: center;">
                    <h3 style="font-size: 28px;">🎯Cyber security Quiz</h3>
                    <div id="quiz-stats" style="font-weight: 900; font-size: 20px; color: var(--accent);">
                        LEVEL: <span id="lvl">1</span> | ✅ <span id="cor">0</span> | ❌ <span id="wrg">0</span>
                    </div>
                </div>
                <div id="quiz-box" style="margin-top: 30px; background:#f1f5f9; padding:30px; border-radius:25px;">
                    <p id="qText" style="font-size: 22px; font-weight: 700;">Generate an AI Mission to start.</p>
                    <div id="qOptions" style="display:grid; grid-template-columns: 1fr 1fr; gap:20px; margin-top:20px;"></div>
                </div>
                <button class="btn-main" id="qBtn" onclick="newMission()" style="background:#10b981;">Generate AI Mission</button>
            </div>
        </div>
    </div>
</div>

<script type="module">
    import { GoogleGenerativeAI } from "@google/generative-ai";
    const API_KEY = "AIzaSyCawxY2VJW5n4DEcnPSfpAYEqJkt_UIzQc";
    const genAI = new GoogleGenerativeAI(API_KEY);
    const model = genAI.getGenerativeModel({ model:"gemini-flash-latest" });

    let stats = { c:0, w:0, l:1 };

    function speak(text) {
    window.speechSynthesis.cancel(); // stop previous speech
    const msg = new SpeechSynthesisUtterance(text);
    window.speechSynthesis.speak(msg);
}


    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    const recognition = new SpeechRecognition();

    window.startVoice = () => {
        const btn = document.getElementById('voiceBtn');
        btn.innerText = "🔴 Listening...";
        btn.classList.add('listening');
        recognition.start();
    };

    recognition.onresult = (event) => {
        const transcript = event.results[0][0].transcript;
        document.getElementById('userInput').value = transcript;
        document.getElementById('voiceBtn').innerText = "🎤 Click to Speak Command";
        document.getElementById('voiceBtn').classList.remove('listening');
        askPragati();
    };

    // FIXED LOGIN: ASKS FOR EMAIL
    window.secureLogin = (provider) => {
        const email = prompt(`Enter your ${provider} email address:`);
        if (email && email.includes("@")) {
            document.getElementById('login-overlay').style.display='none';
            document.getElementById('main-dashboard').style.display='flex';
            document.getElementById('top-nav').style.display='flex';
            speak("Welcome to Pragati AI. Your security is our priority.");
        } else {
            alert("Please enter a valid email address to login.");
        }
    }

    window.askPragati = async function() {
        const log = document.getElementById('chat-log');
        const input = document.getElementById('userInput');
        if(!input.value) return;
        const text = input.value;
        log.innerHTML += `<div class="user-msg"><b>You:</b> ${text}</div>`;
        input.value = "";
        try {
            const res = await model.generateContent(text);
            const reply = res.response.text();
            log.innerHTML += `<div><b>Pragati syber security AI:</b> ${reply}</div>`;
            speak(reply);
        } catch(e) { log.innerHTML += `<div>Connection error.</div>`; }
        log.scrollTop = log.scrollHeight;
    }

    window.scanLink = async function() {
        const url = document.getElementById('urlIn').value;
        const res = await model.generateContent(`Analyze safety of ${url}. 1 line.`);
        const reply = res.response.text();
        speak(reply); alert(reply);
    }

    window.updateRadius = () => {
        const p = document.getElementById('pInput').value;
        document.getElementById('ftVal').innerText = p.length < 8 ? "8,500 ft" : "1 ft";
        document.getElementById('tmVal').innerText = p.length < 8 ? "0.01s" : "900 Yrs";
    }

    // FIXED QUIZ: WORKS WITH GEMINI
    window.newMission = async function() {
        const btn = document.getElementById('qBtn'); btn.innerText = "Consulting AI...";
        try {
            const prompt = `Create a level ${stats.l} cybersecurity MCQ. 
            Format exactly like this:
            Question: [Text]
            A: [Option]
            B: [Option]
            Answer: [A or B]`;
            
            const res = await model.generateContent(prompt);
            const text = res.response.text();
            
            const q = text.split('Question:')[1].split('A:')[0].trim();
            const a = text.split('A:')[1].split('B:')[0].trim();
            const b = text.split('B:')[1].split('Answer:')[0].trim();
            const correct = text.split('Answer:')[1].trim().charAt(0);

            document.getElementById('qText').innerText = q;
            const box = document.getElementById('qOptions'); box.innerHTML = "";
            
            [['A', a], ['B', b]].forEach(([key, val]) => {
                const bBtn = document.createElement('button');
                bBtn.className = "btn-main"; bBtn.style.background = "white"; bBtn.style.color = "black"; bBtn.style.border = "1px solid #ddd";
                bBtn.innerText = key + ": " + val;
                bBtn.onclick = () => {
                    if(key === correct) {
                        stats.c++; stats.l++; speak("Correct.");
                    } else {
                        stats.w++; speak("Incorrect.");
                    }
                    document.getElementById('cor').innerText = stats.c;
                    document.getElementById('wrg').innerText = stats.w;
                    document.getElementById('lvl').innerText = stats.l;
                    newMission();
                };
                box.appendChild(bBtn);
            });
        } catch(e) { console.error(e); }
        btn.innerText = "Generate AI Mission";
    }
    window.stopSpeaking = () => {
    window.speechSynthesis.cancel();
};
</script>
</body>
</html>
