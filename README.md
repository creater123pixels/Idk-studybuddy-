<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Idk AI — studybuddy</title>
<style>
/* --- THEME --- */
:root{--cyan:#6ef0ff;--vio:#a77bff;--muted:#9aa8bd;--bg1:#020317;--card:rgba(255,255,255,0.03)}
*{box-sizing:border-box}
body{margin:0;font-family:Inter,system-ui,Segoe UI,Arial;background:radial-gradient(1200px 600px at 10% 10%, #08102a, #000018 60%);color:var(--cyan);height:100vh;overflow:hidden}
body::before{content:"";position:fixed;inset:0;background-image:radial-gradient(1px 1px at 20% 30%, rgba(255,255,255,0.9),transparent),radial-gradient(1px 1px at 40% 70%, rgba(255,255,255,0.8),transparent);opacity:0.08;pointer-events:none}
#sidebar{width:300px;height:100vh;background:linear-gradient(180deg, rgba(8,12,28,0.9), rgba(6,8,18,0.9));position:fixed;top:0;left:-300px;transition:left .35s ease;backdrop-filter:blur(6px);padding:18px;border-right:1px solid rgba(108,224,255,0.06);z-index:40}
#sidebar.open{left:0}
#sidebar .close{display:block;margin-bottom:10px;padding:8px;border-radius:8px;background:transparent;border:1px solid rgba(255,255,255,0.03);color:var(--cyan);cursor:pointer}
.sidebar-btn{width:100%;padding:10px;margin-top:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.03);background:linear-gradient(90deg, rgba(12,22,36,0.6), rgba(6,8,16,0.6));color:var(--cyan);cursor:pointer}
.sidebar-section{margin-top:14px}
#chatsList{margin-top:8px;max-height:40vh;overflow:auto}
.chat-item{padding:8px;border-radius:8px;margin-bottom:8px;background:rgba(255,255,255,0.02);display:flex;justify-content:space-between;align-items:center}
.chat-item .title{color:var(--cyan);font-size:14px}
.chat-item button{margin-left:6px;padding:6px;border-radius:6px;border:0;background:rgba(255,255,255,0.03);color:var(--muted);cursor:pointer}
#toggle-btn{position:fixed;top:18px;left:18px;padding:10px 12px;border-radius:10px;border:1px solid rgba(108,224,255,0.12);background:rgba(6,10,18,0.6);color:var(--cyan);cursor:pointer;z-index:50}
#main{position:absolute;inset:0;display:flex;align-items:stretch}
#center{flex:1;margin-left:0;padding:24px 40px 110px 40px;overflow:auto}
#header{display:flex;align-items:center;gap:12px;margin-bottom:12px}
#logoSmall{display:none;color:var(--vio);font-weight:700;font-size:18px}
#logoBig{height:320px;display:flex;align-items:center;justify-content:center;flex-direction:column;gap:12px}
#logoBig h1{font-size:56px;color:var(--cyan);margin:0;text-shadow:0 6px 24px rgba(108,224,255,0.06)}
#logoBig p{color:var(--muted);font-size:16px;margin:0}
.recs{display:flex;gap:10px;margin-top:12px;flex-wrap:wrap;justify-content:center}
.rec{padding:10px 12px;border-radius:999px;background:rgba(255,255,255,0.02);border:1px solid rgba(108,224,255,0.04);color:var(--muted);cursor:pointer}
#chatbox{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border-radius:12px;padding:18px;min-height:420px;max-height:60vh;overflow:auto;border:1px solid rgba(255,255,255,0.02)}
.msg{max-width:72%;padding:12px;border-radius:12px;margin:10px;position:relative;opacity:0;transform:translateY(6px);transition:all .32s ease}
.msg.show{opacity:1;transform:translateY(0)}
.msg.user{margin-left:auto;background:linear-gradient(90deg, rgba(167,123,255,0.08), rgba(108,224,255,0.06));color:#dfeaff;border:1px solid rgba(167,123,255,0.12)}
.msg.ai{margin-right:auto;background:linear-gradient(90deg, rgba(6,12,24,0.6), rgba(0,20,30,0.6));color:var(--cyan);border:1px solid rgba(108,224,255,0.06)}
.msg .meta{font-size:11px;color:var(--muted);position:absolute;bottom:-16px;right:10px}
.typing-indicator{display:flex;gap:6px;align-items:center;padding:10px 14px;border-radius:12px;background:linear-gradient(90deg, rgba(8,12,20,0.6), rgba(6,10,16,0.6));border:1px solid rgba(108,224,255,0.04);color:var(--muted)}
.typing-dot{width:8px;height:8px;border-radius:50%;background:rgba(255,255,255,0.18);animation:blink 1s infinite}
.typing-dot:nth-child(2){animation-delay:0.12s}
.typing-dot:nth-child(3){animation-delay:0.24s}
@keyframes blink{0%{opacity:0.15;transform:translateY(0)}50%{opacity:1;transform:translateY(-3px)}100%{opacity:0.15;transform:translateY(0)}}
.msg.mystery{box-shadow:0 0 18px rgba(167,123,255,0.12), 0 0 48px rgba(108,224,255,0.06) inset; border:1px solid rgba(167,123,255,0.18); color: #fff; background: linear-gradient(90deg, rgba(20,10,40,0.7), rgba(6,8,20,0.7));}
.mystery-text{font-weight:700;font-size:16px; letter-spacing:0.2px; text-shadow: 0 6px 18px rgba(108,224,255,0.08); animation:glow 2s ease-in-out infinite;}
@keyframes glow{0%{text-shadow:0 0 6px rgba(108,224,255,0.02)}50%{text-shadow:0 0 18px rgba(108,224,255,0.14)}100%{text-shadow:0 0 6px rgba(108,224,255,0.02)}}
#inputBar{position:fixed;left:50%;transform:translateX(-50%);bottom:18px;width:86%;display:flex;gap:10px;align-items:center;z-index:45}
#userInput{flex:1;padding:12px;border-radius:12px;border:1px solid rgba(255,255,255,0.03);background:rgba(0,0,0,0.4);color:var(--cyan);outline:none}
#btnSend{padding:12px 16px;border-radius:10px;background:linear-gradient(90deg,var(--cyan),var(--vio));border:none;color:black;cursor:pointer}
.smallBtn{padding:8px 10px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:transparent;color:var(--muted);cursor:pointer}
#quizControls{display:flex;gap:8px;margin-top:10px}
#explainBtn{display:none}
#creditsBtn{position:fixed;top:18px;right:18px;padding:10px 12px;border-radius:10px;border:1px solid rgba(108,224,255,0.12);background:rgba(6,10,18,0.6);color:var(--cyan);cursor:pointer;z-index:50}
#creditsSidebar{width:280px;height:100vh;position:fixed;top:0;right:-280px;background:linear-gradient(180deg, rgba(8,12,28,0.95), rgba(6,8,18,0.95));backdrop-filter:blur(6px);padding:18px;border-left:1px solid rgba(108,224,255,0.06);transition:right .35s ease;z-index:60;}
.score-board{font-size:13px;color:var(--muted);margin-top:8px}
@media (max-width:900px){#sidebar{width:260px}#logoBig h1{font-size:44px}}

  /* ========== MOBILE FIXES (≤600px) ========== */
@media (max-width: 600px) {

  body {
    overflow: hidden;
    height: 100dvh; /* fixes mobile viewport bug */
  }

  /* Sidebar fully mobile width */
  #sidebar {
    width: 85%;
    left: -85%;
    padding: 14px;
  }
  #sidebar.open {
    left: 0;
  }

  /* Main layout */
  #center {
    padding: 14px 16px 120px 16px;
  }

  /* Reduce logo */
  #logoBig h1 {
    font-size: 36px;
  }
  #logoBig p {
    font-size: 14px;
  }

  /* Chatbox resizes to viewport */
  #chatbox {
    min-height: 300px;
    max-height: 50dvh;
    padding: 14px;
  }

  /* Messages fit small screens */
  .msg {
    max-width: 90%;
    font-size: 14px;
    padding: 10px;
    margin: 8px;
  }

  /* Input bar pinned to bottom */
  #inputBar {
    width: 94%;
    bottom: 10px;
    gap: 8px;
  }
  #userInput {
    padding: 10px;
    font-size: 15px;
  }
  #btnSend {
    padding: 10px 12px;
    font-size: 14px;
  }

  /* Small header icon */
  #logoSmall {
    font-size: 16px;
  }

  /* Buttons */
  .sidebar-btn,
  .smallBtn {
    font-size: 14px;
    padding: 8px 10px;
  }

  /* Recommended chips */
  .rec {
    padding: 8px 12px;
    font-size: 13px;
  }

  /* Quiz modal width */
  #testModal > div {
    width: 92%;
    max-width: none;
  }

  /* Credits panel smaller */
  #creditsSidebar {
    width: 80%;
    right: -80%;
  }

  /* Make iframe (google fallback) scale */
  iframe {
    height: 280px !important;
  }
}

</style>
</head>
<body>
<button id="toggle-btn">☰ Menu</button>

<!-- left sidebar -->
<div id="sidebar" aria-hidden="true">
  <button class="close" onclick="closeSidebar()">‹ Close</button>
  <div class="sidebar-section">
    <button class="sidebar-btn" onclick="newChat()">New Chat</button>
    <button class="sidebar-btn" id="webBtn" onclick="toggleWebMode()">Web Search (Ask)</button>
    <!-- Play game button uses same class for consistent styling -->
    <button class="sidebar-btn" onclick="startRandomGame()">🎮 Play a Game</button>
    <button class="sidebar-btn" onclick="clearAndSaveChat()">Clear Chat (Save)</button>
  </div>
  <div class="sidebar-section">
    <h3 style="color:var(--vio);margin:8px 0 6px 0">Your Chats</h3>
    <div id="chatsList"></div>
  </div>
</div>

<!-- main -->
<div id="main">
  <div id="center">
    <div id="header"><div id="logoSmall">Idk</div></div>

    <div id="logoBig">
      <h1>Idk</h1>
      <p>Your futuristic study buddy</p>
      <div class="recs">
        <div class="rec" onclick="startRecommended('who is nelson mandela')">Who is Nelson Mandela</div>
        <div class="rec" onclick="startRecommended('solve 2x+3=7')">Solve 2x+3=7</div>
        <div class="rec" onclick="startRecommended('area of circle formula')">Area of circle formula</div>
        <div class="rec" onclick="startRecommended('what is gravity')">What is gravity</div>
        <div class="rec" onclick="startRecommended('why are you named idk')">Why are you named Idk?</div>
      </div>
    </div>

    <div id="chatbox" aria-live="polite"></div>

    <!-- Controls area under chatbox (Explain / End Quiz originally) -->
    <div id="quizControls" style="display:flex;gap:8px;margin-top:12px">
      <button class="smallBtn" id="endGameBtn" onclick="endGame()" style="display:none;">End Game</button>
      <button class="smallBtn" id="restartGameBtn" onclick="restartGame()" style="display:none;">Restart Game</button>
      <div id="scoreBoard" class="score-board" style="display:none;"></div>
    </div>
  </div>
</div>

<!-- input -->
<div id="inputBar">
  <input id="userInput" placeholder="Ask me anything... e.g. 5+3*2 or who is Newton" />
  <button id="btnSend" onclick="onSend()">Send</button>
</div>

<!-- Quiz modal -->
<div id="testModal" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,0.6);align-items:center;justify-content:center;z-index:60">
  <div style="background:linear-gradient(180deg,rgba(10,14,30,0.95),rgba(5,8,16,0.95));padding:18px;border-radius:10px;width:90%;max-width:640px;margin:40px auto;color:var(--muted)">
    <h3 style="color:var(--vio);margin-top:0">Take Quiz Test</h3>
    <div style="display:flex;gap:8px;margin-top:8px;flex-wrap:wrap">
      <select id="qDiff"><option>Easy</option><option>Medium</option><option>Hard</option></select>
      <select id="qNation"><option>India</option><option>USA</option><option>Japan</option><option>UK</option><option>Canada</option></select>
      <select id="qSubject"><option>General Knowledge</option><option>Math</option><option>Science</option><option>History</option></select>
      <select id="qCount"><option value="5">5 questions</option><option value="10">10 questions</option></select>
    </div>
    <input id="qKeywords" placeholder="Optional: topic or keywords (e.g. motion, algebra)" style="width:100%;margin-top:8px;padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:transparent;color:var(--cyan)" />
    <div style="display:flex;justify-content:flex-end;gap:8px;margin-top:10px">
      <button class="smallBtn" onclick="closeTestModal()">Cancel</button>
      <button class="smallBtn" onclick="startWebQuiz()">Start Quiz</button>
    </div>
    <p style="color:var(--muted);font-size:12px;margin-top:8px">Quiz will try to fetch questions from Google; if that fails, built-in questions are used as fallback.</p>
  </div>
</div>

<!-- Credits / About Me -->
<button id="creditsBtn">About Me</button>
<div id="creditsSidebar">
  <button style="display:block;margin-bottom:10px;padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:transparent;color:var(--cyan);cursor:pointer;" onclick="closeCredits()">‹ Close</button>
  <h3 style="color:var(--vio);margin-top:0;">Who am I?</h3>
  <p style="color:var(--muted);font-size:14px;line-height:1.5;">
    I am Idk, your study buddy!<br>
    I was created because, instead of searching all over the browser, you can get only what you need and I can help with your homework.<br>
    I can answer questions, perform quizzes, and provide instant information efficiently. I learn from what you teach me and remember little things like your name (if you tell me).<br>
  </p>
  <h4 style="color:var(--vio);margin:12px 0 4px 0;">Credits</h4>
  <p style="color:var(--muted);font-size:13px;line-height:1.4;">
    Frontend, Js & CSS by G.kavi and K.Kavin <br>
    <br>
    Naming and designing code by Rajasree and Ananya (TEAM 8ALPHA 2024 -2025 batch)<br>
    <br>
    stage 4 of factbot, stage 1 of smarXs(upcomming AI on 2026)<br>
    <br>
    Next updation on jan 2026 to feb 2026 <br>

  </p>
</div>

<script>
/* ================= CONFIG ================= */
const GOOGLE_API_KEY = 'AIzaSyBwrhsjojHF7MN5Fk3LqD5qvC119x8d3JU'; // replace if you want
const GOOGLE_CX = 'e536480adf3f94c71'; // replace if needed

/* ================= STATE ================= */
let userName = localStorage.getItem('idk_user_name') || null;
let webMode = 'ask';
let pendingQuery = null;
let awaitingPermission = false;
let currentChat = {id: Date.now(), title: 'Chat', messages: []};
let savedChats = JSON.parse(localStorage.getItem('idk_saved_chats_v1')||'[]');
let quizActive = null; // {questions:[], index:0, score:0, sourcedFromWeb:bool}
let lastBotAnswer = null;

/* game state container */
let gameActive = null; // when active: { secret, attempts, difficulty, prevHandler }

/* ================= ELEMENTS ================= */
const sidebar = document.getElementById('sidebar');
const toggleBtn = document.getElementById('toggle-btn');
const logoBig = document.getElementById('logoBig');
const logoSmall = document.getElementById('logoSmall');
const chatbox = document.getElementById('chatbox');
const chatsList = document.getElementById('chatsList');
const quizControls = document.getElementById('quizControls');
const explainBtn = document.getElementById('explainBtn');
const creditsBtn = document.getElementById('creditsBtn');
const creditsSidebar = document.getElementById('creditsSidebar');
const endGameBtn = document.getElementById('endGameBtn');
const restartGameBtn = document.getElementById('restartGameBtn');
const scoreBoardDiv = document.getElementById('scoreBoard');

function openSidebar(){ sidebar.classList.add('open'); sidebar.setAttribute('aria-hidden','false'); toggleBtn.style.display='none'; renderSavedChats(); }
function closeSidebar(){ sidebar.classList.remove('open'); sidebar.setAttribute('aria-hidden','true'); toggleBtn.style.display='block'; }
toggleBtn.onclick = openSidebar;
creditsBtn.onclick = ()=>openCredits();
function openCredits(){ creditsSidebar.style.right = '0'; }
function closeCredits(){ creditsSidebar.style.right = '-280px'; }

function openTestModal() { document.getElementById('testModal').style.display = 'flex'; }
function closeTestModal() { document.getElementById('testModal').style.display = 'none'; }

/* Chats UI */
function saveCurrentChat(){
  if(!currentChat.messages || currentChat.messages.length===0) return;
  const snapshot = {id: currentChat.id||Date.now(), title: currentChat.title||('Chat '+(savedChats.length+1)), messages: currentChat.messages.slice(), created: new Date().toISOString() };
  savedChats.unshift(snapshot);
  localStorage.setItem('idk_saved_chats_v1', JSON.stringify(savedChats));
  renderSavedChats();
}
function renderSavedChats(){
  chatsList.innerHTML='';
  savedChats.forEach((c,i)=>{
    const div = document.createElement('div'); div.className='chat-item';
    const t = document.createElement('div'); t.className='title'; t.innerText = c.title + ' ('+ new Date(c.created).toLocaleString()+')';
    const btns = document.createElement('div');
    const openBtn = document.createElement('button'); openBtn.innerText='Open'; openBtn.onclick = ()=>{ openSavedChat(i); closeSidebar(); };
    const renameBtn = document.createElement('button'); renameBtn.innerText='Rename'; renameBtn.onclick = ()=>{ const name = prompt('Rename chat', c.title); if (name){ c.title=name; savedChats[i]=c; localStorage.setItem('idk_saved_chats_v1', JSON.stringify(savedChats)); renderSavedChats(); } };
    const delBtn = document.createElement('button'); delBtn.innerText='Delete'; delBtn.onclick = ()=>{ if(confirm('Delete this saved chat?')){ savedChats.splice(i,1); localStorage.setItem('idk_saved_chats_v1', JSON.stringify(savedChats)); renderSavedChats(); } };
    btns.appendChild(openBtn); btns.appendChild(renameBtn); btns.appendChild(delBtn);
    div.appendChild(t); div.appendChild(btns); chatsList.appendChild(div);
  });
}
function openSavedChat(index){ const c = savedChats[index]; currentChat = {id:c.id,title:c.title,messages:c.messages.slice()}; renderChat(); showHome(false); }

function renderChat(){
  chatbox.innerHTML='';
  if (!currentChat.messages || currentChat.messages.length===0){ showHome(true); return; }
  showHome(false);
  currentChat.messages.forEach(m=>{
    const d = document.createElement('div');
    d.className = m.from==='user' ? 'msg user' : 'msg ai';
    if (m.html) d.innerHTML = m.text; else d.innerText = m.text;
    if (m.mystery) d.classList.add('mystery');
    const meta = document.createElement('div'); meta.className='meta'; meta.innerText = new Date(m.t||Date.now()).toLocaleTimeString();
    d.appendChild(meta);
    chatbox.appendChild(d);
    setTimeout(()=>d.classList.add('show'),20);
  });
  chatbox.scrollTop = chatbox.scrollHeight;
}
function showHome(show){ if (show){ logoBig.style.display='flex'; logoSmall.style.display='none'; } else { logoBig.style.display='none'; logoSmall.style.display='block'; } }

/* push message helper */
function pushMessage(from, text, html=false, opts={}) {
  if (from === 'ai') {
    const typing = document.createElement('div'); typing.className='msg ai typing-indicator';
    typing.innerHTML = '<div class="typing-dot"></div><div class="typing-dot"></div><div class="typing-dot"></div><div style="margin-left:8px;color:var(--muted);font-size:13px">Idk is thinking...</div>';
    chatbox.appendChild(typing);
    chatbox.scrollTop = chatbox.scrollHeight;
    const base = 350, perChar = 14, rnd = Math.floor(Math.random()*300);
    const delay = (opts.pause?opts.pause:0) + Math.min(3000, base + (String(text).length)*perChar) + rnd;
    setTimeout(()=>{
      if (typing.parentNode) typing.parentNode.removeChild(typing);
      const d = document.createElement('div'); d.className='msg ai';
      if (html) d.innerHTML = text; else d.innerText = text;
      if (opts.mystery) { d.classList.add('mystery'); if (!html){ const span = document.createElement('div'); span.className='mystery-text'; span.innerText = text; d.innerHTML = ''; d.appendChild(span); } }
      const meta = document.createElement('div'); meta.className='meta'; meta.innerText = new Date().toLocaleTimeString();
      d.appendChild(meta);
      chatbox.appendChild(d); setTimeout(()=>d.classList.add('show'),20); chatbox.scrollTop = chatbox.scrollHeight;
      currentChat.messages.push({from:'ai', text:text, html:html, mystery:!!opts.mystery, t:Date.now()});
      lastBotAnswer = text;
      if (lastBotAnswer) explainBtn.style.display = 'inline-block';
    }, delay);
  } else {
    const d = document.createElement('div'); d.className='msg user';
    d.innerText = text;
    const meta = document.createElement('div'); meta.className='meta'; meta.innerText = new Date().toLocaleTimeString();
    d.appendChild(meta);
    chatbox.appendChild(d); setTimeout(()=>d.classList.add('show'),20);
    chatbox.scrollTop = chatbox.scrollHeight;
    currentChat.messages.push({from:'user', text:text, html:html, mystery:false, t:Date.now()});
  }
}

/* ================= INPUT & NLU ================= */
function isGreeting(s){
  if(!s) return false;
  return /\b(hi|hello|hey|hiya|good morning|good afternoon|good evening|how are you|howdy|yo|sup|bye|goodbye|see you|good night|night|ciao|hola|bonjour)\b/i.test(s);
}
function respondGreeting(msg){
  let greetText = `Hello! I am <strong>Idk</strong>.`;
  if (userName) greetText += ` Nice to see you again, <strong>${userName}</strong>!`;
  greetText += ' How can I help you today?';
  pushMessage('ai', greetText, true);
}
function onSend(){
  const raw = document.getElementById('userInput').value.trim();
  if (!raw) return;
  document.getElementById('userInput').value = '';
  pushMessage('user', raw);
  // call whichever handler is currently active (normal or game override)
  (window.handleUserMessage || handleUserMessage)(raw);
}

/* handle the message with proper permission flow */
function handleUserMessage(msg){
  // permission handling
  if (awaitingPermission){
    const m = msg.trim().toLowerCase();
    if (/^(yes|y|sure|ok|okay|please do)$/i.test(m)){
      awaitingPermission = false;
      const q = pendingQuery;
      pendingQuery = null;
      performSearch(q);
      return;
    }
    if (/^(no|n|dont|do not|nah|nope)$/i.test(m)){
      awaitingPermission = false;
      const q = pendingQuery;
      pendingQuery = null;
      pushMessage('ai','Okay — I will try to answer from memory (or you can teach me).');
      fallbackAnswer(q);
      return;
    }
    // if not yes/no, reset and continue
    awaitingPermission = false;
    pendingQuery = null;
  }

  // name detection
  const nameMatch = msg.match(/\bmy name is\s+([A-Za-z][A-Za-z0-9 ]{0,40})/i) || msg.match(/\bi am\s+([A-Za-z][A-Za-z0-9 ]{0,40})/i);
  if (nameMatch){
    const name = nameMatch[1].trim().split(' ')[0];
    userName = name;
    localStorage.setItem('idk_user_name', userName);
    pushMessage('ai', `Nice to meet you, <strong>${userName}</strong>! I am here to help you learn and explore.`, true);
    return;
  }

  if (isGreeting(msg)){
    respondGreeting(msg);
    return;
  }

  if (/why (are|r) you named idk|why.*named idk/i.test(msg)){
    showMysteriousAnswer();
    return;
  }

  if (webMode === 'auto'){
    performSearch(msg);
    return;
  }

  // ask permission
  pendingQuery = msg;
  awaitingPermission = true;
  pushMessage('ai','Do you want me to search the web for: <strong>'+escapeHtml(msg)+'</strong>? (reply: yes / no)', true);
}

/* ================= SEARCH (Google Custom Search) ================= */
async function performSearch(q) {
    pushMessage('ai', 'Searching the web for: <b>' + escapeHtml(q) + '</b> ...', true);

    // --- AUTO MODE ---
    if (typeof webMode === "undefined") webMode = "auto";

    // -------------------------
    //  WIKIPEDIA FIRST
    // -------------------------
    try {
        const wikiURL = `https://lingering-heart-a5f1.ksmlrs123.workers.dev/?url=${encodeURIComponent(
            `https://en.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(q)}`
        )}`;

        const w = await fetch(wikiURL);
        if (!w.ok) throw new Error("wiki_fail");

        const wiki = await w.json();
        if (wiki.extract) {
            const answer = `<b>${wiki.title}</b><br><br>${wiki.extract}`;
            lastBotAnswer = answer;
            pushMessage('ai', answer, true);
            explainBtn.style.display = 'inline-block';
            return;
        }
        throw new Error("wiki_blank");
    } catch (err) {
        console.warn("Wikipedia failed, switching to Google:", err);
    }

    // -------------------------
    //  GOOGLE CUSTOM SEARCH
    // -------------------------
    try {
        const googleURL =
            `https://www.googleapis.com/customsearch/v1?key=${GOOGLE_API_KEY}&cx=${GOOGLE_CX}&q=${encodeURIComponent(q)}`;
        const proxiedGoogleURL =
            `https://lingering-heart-a5f1.ksmlrs123.workers.dev/?url=${encodeURIComponent(googleURL)}`;

        const r = await fetch(proxiedGoogleURL);
        if (!r.ok) throw new Error("google_fail");

        const data = await r.json();
        if (data.items && data.items.length > 0) {
            const items = data.items.slice(0, 3); // top 3 results
            const longAnswer = items.map(it => (it.snippet || it.title)).join("<br><br>");
            lastBotAnswer = longAnswer;
            pushMessage('ai', longAnswer, true);
            explainBtn.style.display = 'inline-block';
            return;
        }
        throw new Error("no_results");
    } catch (err) {
        console.warn("Google failed, showing iframe:", err);
    }

    // -------------------------
    //  FINAL FALLBACK (iframe)
    // -------------------------
    pushMessage('ai', "Couldn't fetch instant details — here are full Google results:", true);
    showIframeGoogle(q);
}

function showIframeGoogle(q) {
    const ifr = document.createElement('iframe');
    ifr.src = `https://www.google.com/search?q=${encodeURIComponent(q)}`;
    ifr.style.width = '100%';
    ifr.style.height = '420px';
    ifr.style.border = '0';
    chatbox.appendChild(ifr);
    chatbox.scrollTop = chatbox.scrollHeight;
}



/* ================= RANDOM GAME ================= */

/* Helper: display or hide end/restart/scoreboard controls */
function showGameControls(show) {
  endGameBtn.style.display = show ? 'inline-block' : 'none';
  restartGameBtn.style.display = show ? 'inline-block' : 'none';
  scoreBoardDiv.style.display = show ? 'block' : 'none';
  if (show) updateScoreBoardUI(); // refresh scoreboard when showing
}

/* Get difficulty bounds from string */
function difficultyToRange(diff) {
  diff = (diff || 'medium').toLowerCase();
  if (diff === 'easy') return {min:1, max:20};
  if (diff === 'hard') return {min:1, max:100};
  return {min:1, max:50}; // medium default
}

/* Start a new random game; asks difficulty with a simple prompt (keeps UI light) */
function startRandomGame() {
  if (gameActive) {
    pushMessage('ai', 'A game is already running. Use End Game or Restart.');
    return;
  }

  // Ask difficulty quickly
  const diffPrompt = prompt('Pick difficulty: easy / medium / hard', 'medium');
  const diff = (diffPrompt || 'medium').trim().toLowerCase();
  const range = difficultyToRange(diff);
  const secret = Math.floor(Math.random() * (range.max - range.min + 1)) + range.min;

  // store previous handler so we can restore it when game ends
  const prevHandler = window.handleUserMessage || handleUserMessage;

  gameActive = { secret, attempts: 0, difficulty: diff, prevHandler };

  pushMessage('ai', `🎲 Welcome to <strong>Guess the Number</strong>! I'm thinking of a number between <strong>${range.min}</strong> and <strong>${range.max}</strong>. Type your guess. (Or press <strong>End Game</strong>)`, true);

  showGameControls(true);

  // override the global handler to capture guesses
  window.handleUserMessage = function(msg) {
    // Note: onSend already pushed the user's message into the chat.
    // This handler should *not* push it again.

    const trimmed = msg.trim().toLowerCase();

    if (trimmed === 'end game') {
      endGame();
      return;
    }
    if (trimmed === 'restart') {
      restartGame();
      return;
    }

    const num = parseInt(trimmed, 10);
    if (isNaN(num)) {
      pushMessage('ai', '❗ Please enter a valid number.');
      return;
    }

    gameActive.attempts++;

    if (num === gameActive.secret) {
      pushMessage('ai', `🎉 Correct! The number was <strong>${gameActive.secret}</strong>.<br>You guessed it in <strong>${gameActive.attempts}</strong> tries.`, true);

      // record score
      saveGameScore({date: new Date().toISOString(), attempts: gameActive.attempts, difficulty: gameActive.difficulty});

      // finish
      const restore = gameActive.prevHandler || handleUserMessage;
      gameActive = null;
      window.handleUserMessage = restore;
      showGameControls(false);
      updateScoreBoardUI(true); // briefly show scoreboard with latest
      return;
    }

    if (num < gameActive.secret) {
      pushMessage('ai', '⬆️ Too low! Try a higher number.');
    } else {
      pushMessage('ai', '⬇️ Too high! Try a lower number.');
    }
  };
}

/* Restart current active game (keeps same difficulty) */
function restartGame() {
  if (!gameActive) { pushMessage('ai','No game to restart. Click Play a Game to start one.'); return; }
  const range = difficultyToRange(gameActive.difficulty);
  const secret = Math.floor(Math.random() * (range.max - range.min + 1)) + range.min;
  gameActive.secret = secret;
  gameActive.attempts = 0;
  pushMessage('ai', `🔁 Game restarted (difficulty: ${gameActive.difficulty}). I'm thinking of a new number between ${range.min} and ${range.max}. Start guessing!`);
}

/* End game and restore chat handler */
function endGame() {
  if (!gameActive) {
    pushMessage('ai','No active game to end.');
    return;
  }

  const restore = gameActive.prevHandler || handleUserMessage;
  gameActive = null;
  window.handleUserMessage = restore;
  showGameControls(false);
  pushMessage('ai', '🛑 Game ended. You can start a new one from the sidebar.');
}

/* ================= Scoreboard storage & UI ================= */
const SCORE_KEY = 'idk_game_scores_v1';
function loadScores() {
  try {
    return JSON.parse(localStorage.getItem(SCORE_KEY) || '[]');
  } catch(e) { return []; }
}
function saveGameScore(scoreObj) {
  const arr = loadScores();
  arr.unshift(scoreObj);
  // keep last 20 scores
  localStorage.setItem(SCORE_KEY, JSON.stringify(arr.slice(0,20)));
}

/* render scoreboard small UI (under controls) */
function updateScoreBoardUI(forceShowLatest=false) {
  const scores = loadScores();
  if (!scores || scores.length === 0) {
    scoreBoardDiv.innerHTML = '<em>No scores yet — win a game to create one.</em>';
    return;
  }
  const rows = scores.slice(0,5).map(s => {
    const d = new Date(s.date);
    return `<div>• ${d.toLocaleDateString()} ${d.toLocaleTimeString()} — ${s.attempts} tries (${s.difficulty})</div>`;
  }).join('');
  scoreBoardDiv.innerHTML = `<strong>Recent scores</strong>${rows}`;
  if (forceShowLatest) {
    // briefly highlight
    scoreBoardDiv.style.transition = 'none';
    scoreBoardDiv.style.opacity = '1';
  }
}

/* ================= HELPERS ================= */
function escapeHtml(s){ return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/\n/g,'<br>'); }
function startRecommended(q){ showHome(false); if (/why (are|r) you named idk|why.*named idk/i.test(q)){ showMysteriousAnswer(); return; } pushMessage('user', q); handleUserMessage(q); }
function toggleWebMode(){ webMode = webMode==='ask' ? 'auto' : 'ask'; document.getElementById('webBtn').innerText = webMode==='ask' ? 'Web Search (Ask)' : 'Web Search (Auto)'; pushMessage('ai','Web mode set to: '+webMode); }
function showMysteriousAnswer(){ const mysterious = "People say great discoveries begin with three words— 'I don\\'t know.' So that became my name."; pushMessage('ai','',false,{pause:700}); setTimeout(()=>{ pushMessage('ai', mysterious, false, {mystery:true, pause:0}); }, 900); }

/* ================= NEW CHAT & CLEAR ================= */
function newChat(){
  if (currentChat.messages && currentChat.messages.length>0) saveCurrentChat();
  currentChat = {id: Date.now(), title: 'Chat '+(savedChats.length+1), messages: []};
  renderChat();
  showHome(true);
}
function clearAndSaveChat(){
  if (currentChat.messages && currentChat.messages.length>0) saveCurrentChat();
  currentChat = {id: Date.now(), title:'Chat '+(savedChats.length+1), messages:[]};
  renderChat();
  showHome(true);
}

/* save on unload */
window.addEventListener('beforeunload', ()=>{ if (currentChat.messages && currentChat.messages.length) saveCurrentChat(); });

/* initial render */
renderSavedChats(); renderChat(); showHome(true);

/* expose global handler for quiz override */
window.handleUserMessage = handleUserMessage;

</script>
</body>
</html>
