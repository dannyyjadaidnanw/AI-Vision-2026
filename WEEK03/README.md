<img width="1080" height="810" alt="image" src="https://github.com/user-attachments/assets/d3a68b0f-72da-4406-9621-148bdd04aee5" />


Claude link:
https://claude.ai/share/474a3d59-b674-40ec-a8fe-113e627aab29

Prompt:can you make this game, make it cool and many thing to make it look awesome and fantastic physic and extra detail if you want to add based on the picture

Rumusan

Hari ini saya belajar tentang etika penggunaan AI. Apa yang paling saya faham, AI ni sebenarnya sekadar pembantu dan bukannya pengganti kita. Tapi saya masih tertanya-tanya macam mana AI boleh jadi sebijak ini dan bagaimana sistem di belakangnya berfungsi. Untuk bagi lebih faham, saya bercadang nak rajin membaca dan mengkaji fungsi dalaman AI selepas ini.

[neon-pong.html](https://github.com/user-attachments/files/30290786/neon-pong.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>NEON PONG — AI Game Jam Mission</title>
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@400;500;600;700&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
  :root{
    --accent: rgb(0,232,255);
    --accent2: rgb(0,232,255);
    --bg: #05020f;
    --panel: rgba(8,6,22,0.72);
    --panel-border: rgba(255,255,255,0.08);
    --danger: #ff3355;
    --font-display: 'Orbitron', sans-serif;
    --font-body: 'Rajdhani', sans-serif;
    --font-mono: 'Share Tech Mono', monospace;
  }
  *{ box-sizing:border-box; }
  html,body{
    margin:0; padding:0; width:100%; height:100%;
    background:var(--bg); overflow:hidden;
    font-family:var(--font-body);
    -webkit-user-select:none; user-select:none;
    overscroll-behavior:none;
  }
  canvas{
    display:block; position:fixed; inset:0;
    width:100%; height:100%;
    cursor:none; touch-action:none;
  }

  /* ---------- ambient CRT dressing ---------- */
  #vignette{
    position:fixed; inset:0; pointer-events:none; z-index:5;
    background: radial-gradient(ellipse at center, rgba(0,0,0,0) 45%, rgba(0,0,0,0.55) 100%);
  }
  #scanlines{
    position:fixed; inset:0; pointer-events:none; z-index:6;
    background: repeating-linear-gradient(
      to bottom,
      rgba(0,0,0,0) 0px,
      rgba(0,0,0,0.12) 1px,
      rgba(0,0,0,0) 3px
    );
    mix-blend-mode:overlay;
    opacity:0.55;
  }
  #flash{
    position:fixed; inset:0; pointer-events:none; z-index:7;
    background: var(--accent); opacity:0;
  }

  /* ---------- HUD ---------- */
  #hud{
    position:fixed; inset:0; z-index:10; pointer-events:none;
    padding: max(14px, env(safe-area-inset-top)) 18px 0 18px;
  }
  .hud-row{
    display:flex; align-items:flex-start; justify-content:space-between;
    gap:12px;
  }
  .stat{
    display:flex; flex-direction:column; align-items:flex-start;
    min-width:74px;
  }
  .stat.right{ align-items:flex-end; }
  .stat-label{
    font-family:var(--font-mono); font-size:11px; letter-spacing:3px;
    color:rgba(255,255,255,0.55); margin-bottom:2px;
  }
  .stat-value{
    font-family:var(--font-display); font-weight:700; font-size:clamp(22px,4vw,32px);
    color:#fff; text-shadow:0 0 6px var(--accent), 0 0 18px var(--accent);
    line-height:1;
  }
  .speed-tag{
    font-family:var(--font-mono); font-size:12px; color:var(--accent);
    text-shadow:0 0 8px var(--accent); margin-top:4px; opacity:0.9;
  }
  #progress{
    display:flex; gap:5px; justify-content:center; margin:10px auto 0; 
    max-width:420px; width:70%;
  }
  .tick{
    flex:1; height:6px; border-radius:3px;
    background:rgba(255,255,255,0.08);
    border:1px solid rgba(255,255,255,0.12);
    transition: background 200ms ease, box-shadow 200ms ease;
  }
  .tick.lit{
    background:var(--accent);
    box-shadow:0 0 8px var(--accent), 0 0 2px var(--accent);
    border-color:transparent;
  }
  #hint{
    position:fixed; bottom:16px; left:0; right:0; text-align:center; z-index:10;
    font-family:var(--font-mono); font-size:11px; letter-spacing:2px;
    color:rgba(255,255,255,0.35); pointer-events:none;
  }

  /* ---------- overlays ---------- */
  .overlay{
    position:fixed; inset:0; z-index:20;
    display:flex; align-items:center; justify-content:center;
    opacity:0; pointer-events:none; transition:opacity 350ms ease;
    padding:20px;
  }
  .overlay.visible{ opacity:1; pointer-events:auto; cursor:auto; }
  .card{
    background:var(--panel); border:1px solid var(--panel-border);
    backdrop-filter: blur(10px);
    border-radius:14px;
    padding:34px 40px;
    max-width:480px; width:100%;
    text-align:center;
    box-shadow: 0 0 0 1px rgba(255,255,255,0.03), 0 20px 60px rgba(0,0,0,0.6), 0 0 40px rgba(0,0,0,0.4);
  }
  .eyebrow{
    font-family:var(--font-mono); font-size:12px; letter-spacing:4px;
    color:var(--accent); text-shadow:0 0 8px var(--accent);
    margin-bottom:10px;
  }
  .glitch{
    position:relative;
    font-family:var(--font-display); font-weight:900;
    font-size:clamp(34px,8vw,58px);
    color:#fff; letter-spacing:2px;
    text-shadow:0 0 8px var(--accent), 0 0 30px var(--accent), 0 0 70px var(--accent);
    animation: flicker 4.5s infinite;
    margin:0 0 18px 0;
  }
  .glitch::before, .glitch::after{
    content: attr(data-text); position:absolute; left:0; top:0; width:100%;
  }
  .glitch::before{ color:#ff2bd6; clip-path: inset(0 0 58% 0); transform:translate(-2px,-1px); animation: glitchTop 2.6s infinite linear alternate-reverse; }
  .glitch::after{ color:#00e8ff; clip-path: inset(58% 0 0 0); transform:translate(2px,1px); animation: glitchBottom 2.1s infinite linear alternate-reverse; }
  @keyframes flicker{
    0%,19%,21%,23%,80%,100%{opacity:1;}
    20%,22%,24%{opacity:0.45;}
  }
  @keyframes glitchTop{ 0%{transform:translate(-2px,-1px);} 50%{transform:translate(1px,-2px);} 100%{transform:translate(-1px,1px);} }
  @keyframes glitchBottom{ 0%{transform:translate(2px,1px);} 50%{transform:translate(-1px,2px);} 100%{transform:translate(1px,-1px);} }
  @media (prefers-reduced-motion: reduce){ .glitch, .glitch::before, .glitch::after{ animation:none !important; } }

  .rules{
    list-style:none; margin:0 0 22px 0; padding:0;
    display:flex; flex-direction:column; gap:8px;
    font-family:var(--font-body); font-weight:600; font-size:15px;
    color:rgba(255,255,255,0.85); text-align:left;
  }
  .rules li{ display:flex; gap:10px; align-items:baseline; }
  .rules b{ color:var(--accent); font-family:var(--font-mono); font-size:12px; letter-spacing:1px; }
  .target-line{
    font-family:var(--font-mono); font-size:13px; color:rgba(255,255,255,0.5);
    margin-bottom:22px;
  }
  .target-line strong{ color:#fff; }

  button.neon-btn{
    font-family:var(--font-display); font-weight:700; font-size:15px; letter-spacing:2px;
    color:#fff; background:transparent;
    border:2px solid var(--accent); border-radius:8px;
    padding:14px 28px; cursor:pointer;
    box-shadow: 0 0 12px rgba(0,0,0,0.2) inset, 0 0 16px var(--accent);
    text-shadow:0 0 8px var(--accent);
    transition: transform 150ms ease, box-shadow 150ms ease, background 150ms ease;
  }
  button.neon-btn:hover{ transform:translateY(-2px) scale(1.03); box-shadow:0 0 26px var(--accent); background:rgba(255,255,255,0.05); }
  button.neon-btn:active{ transform:translateY(0) scale(0.98); }

  .credit{
    margin-top:18px; font-family:var(--font-mono); font-size:10px;
    letter-spacing:1.5px; color:rgba(255,255,255,0.28);
  }
  .score-line{
    font-family:var(--font-body); font-size:16px; color:rgba(255,255,255,0.8);
    margin-bottom:6px;
  }
  .score-line b{ color:#fff; font-family:var(--font-display); font-size:22px; }
  .best-line{
    font-family:var(--font-mono); font-size:12px; color:var(--accent);
    margin-bottom:22px; text-shadow:0 0 6px var(--accent);
  }
  #overlay-gameover .glitch{ text-shadow:0 0 8px var(--danger), 0 0 30px var(--danger), 0 0 70px var(--danger); }
  #overlay-gameover .glitch::before{ color:#ff2bd6; }
  #overlay-gameover .glitch::after{ color:#ff3355; }
  #overlay-pause .card{ padding:26px 34px; }
</style>
</head>
<body>

<canvas id="game"></canvas>
<div id="vignette"></div>
<div id="scanlines"></div>
<div id="flash"></div>

<div id="hud">
  <div class="hud-row">
    <div class="stat">
      <span class="stat-label">SCORE</span>
      <span class="stat-value" id="scoreVal">00</span>
    </div>
    <div class="stat right">
      <span class="stat-label">BEST</span>
      <span class="stat-value" id="bestVal">00</span>
      <span class="speed-tag" id="speedVal">x1.00</span>
    </div>
  </div>
  <div id="progress"></div>
</div>
<div id="hint">MOUSE / TOUCH TO AIM &nbsp;·&nbsp; ESC TO PAUSE</div>

<!-- TITLE -->
<div class="overlay visible" id="overlay-title">
  <div class="card">
    <div class="eyebrow">AI GAME JAM // MISSION FILE</div>
    <h1 class="glitch" data-text="NEON PONG">NEON PONG</h1>
    <ul class="rules">
      <li><b>PRECISION</b> the paddle tracks your cursor or finger exactly</li>
      <li><b>ACCELERATION</b> the ball gains +10% speed on every return</li>
      <li><b>FEEDBACK</b> the world shifts color every 5 points scored</li>
    </ul>
    <div class="target-line">Reach a score of <strong>15</strong> to complete the mission. Miss once and it's over.</div>
    <button class="neon-btn" id="btnStart">CLICK OR TAP TO BEGIN</button>
    <div class="credit">CAN YOU PROMPT YOUR WAY TO A HIGH SCORE?</div>
  </div>
</div>

<!-- GAME OVER -->
<div class="overlay" id="overlay-gameover">
  <div class="card">
    <div class="eyebrow">TRANSMISSION LOST</div>
    <h1 class="glitch" data-text="MISSION FAILED">MISSION FAILED</h1>
    <div class="score-line">Ball returned <b id="finalScore">0</b> times</div>
    <div class="best-line" id="bestLine">BEST: 00</div>
    <button class="neon-btn" id="btnRetry">RETRY MISSION</button>
  </div>
</div>

<!-- WIN -->
<div class="overlay" id="overlay-win">
  <div class="card">
    <div class="eyebrow">MISSION COMPLETE</div>
    <h1 class="glitch" data-text="HIGH SCORE" style="--accent:rgb(57,255,176);">HIGH SCORE</h1>
    <div class="score-line">You prompted your way to <b>15 / 15</b></div>
    <div class="best-line" id="bestLineWin">BEST: 00</div>
    <button class="neon-btn" id="btnAgain">PLAY AGAIN</button>
  </div>
</div>

<!-- PAUSE -->
<div class="overlay" id="overlay-pause">
  <div class="card">
    <div class="eyebrow">SIGNAL PAUSED</div>
    <h1 class="glitch" data-text="PAUSED" style="font-size:clamp(28px,6vw,42px);">PAUSED</h1>
    <button class="neon-btn" id="btnResume">RESUME</button>
  </div>
</div>

<script>
(function(){
  "use strict";

  /* ===================== helpers ===================== */
  const clamp = (v,a,b)=> Math.max(a, Math.min(b,v));
  const reducedMotion = window.matchMedia && window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  function hexToRgb(hex){
    const h = hex.replace('#','');
    const v = h.length===3 ? h.split('').map(c=>c+c).join('') : h;
    const num = parseInt(v,16);
    return {r:(num>>16)&255, g:(num>>8)&255, b:num&255};
  }
  function parseColor(c){
    if(typeof c !== 'string') return c;
    if(c[0]==='#') return hexToRgb(c);
    const m = c.match(/rgba?\(([\d.]+),\s*([\d.]+),\s*([\d.]+)/);
    if(m) return {r:+m[1], g:+m[2], b:+m[3]};
    return {r:255,g:255,b:255};
  }
  function lerpColor(a,b,t){
    const ca = parseColor(a), cb = parseColor(b);
    return {
      r: Math.round(ca.r+(cb.r-ca.r)*t),
      g: Math.round(ca.g+(cb.g-ca.g)*t),
      b: Math.round(ca.b+(cb.b-ca.b)*t)
    };
  }
  function rgba(o,a){ if(a===undefined)a=1; return `rgba(${o.r},${o.g},${o.b},${a})`; }
  function roundRect(ctx,x,y,w,h,r){
    ctx.beginPath();
    ctx.moveTo(x+r,y);
    ctx.arcTo(x+w,y,x+w,y+h,r);
    ctx.arcTo(x+w,y+h,x,y+h,r);
    ctx.arcTo(x,y+h,x,y,r);
    ctx.arcTo(x,y,x+w,y,r);
    ctx.closePath();
  }

  /* ===================== canvas / sizing ===================== */
  const canvas = document.getElementById('game');
  const ctx = canvas.getContext('2d');
  let W=0,H=0,DPR=1;
  const MARGIN = 46; // overpaint margin so screen-shake never reveals gaps

  function resize(){
    DPR = Math.min(window.devicePixelRatio||1, 2);
    W = window.innerWidth; H = window.innerHeight;
    canvas.width = Math.round(W*DPR);
    canvas.height = Math.round(H*DPR);
    canvas.style.width = W+'px';
    canvas.style.height = H+'px';
    ctx.setTransform(DPR,0,0,DPR,0,0);
    paddle.x = W*0.045;
    paddle.height = Math.max(70, H*0.15);
    if(paddle.y===undefined || paddle.y===null) paddle.y = H/2 - paddle.height/2;
    paddle.y = clamp(paddle.y, 0, H-paddle.height);
    buildSkyline();
  }
  window.addEventListener('resize', resize);

  /* ===================== themes ===================== */
  const THEMES = [
    { name:'CYAN CIRCUIT',   sky0:'#05021a', sky1:'#0a1a3d', grid:'#00e8ff', accent:'#00e8ff', building:'#081233' },
    { name:'MAGENTA PULSE',  sky0:'#180022', sky1:'#3d0a2e', grid:'#ff2bd6', accent:'#ff2bd6', building:'#220b28' },
    { name:'AMBER OVERDRIVE',sky0:'#1a0f00', sky1:'#3d1f00', grid:'#ff9d1f', accent:'#ff9d1f', building:'#2b1a05' },
    { name:'MISSION GREEN',  sky0:'#001a12', sky1:'#00402a', grid:'#39ffb0', accent:'#39ffb0', building:'#052b1d' }
  ];
  let themeIndex = 0;
  let themeFrom = THEMES[0];
  let themeTo = THEMES[0];
  let themeT = 1;
  let themeTransStart = 0;
  const THEME_MS = 650;
  let currentTheme = lerpTheme(THEMES[0],THEMES[0],1);

  function lerpTheme(a,b,t){
    return {
      sky0: lerpColor(a.sky0,b.sky0,t),
      sky1: lerpColor(a.sky1,b.sky1,t),
      grid: lerpColor(a.grid,b.grid,t),
      accent: lerpColor(a.accent,b.accent,t),
      building: lerpColor(a.building,b.building,t)
    };
  }
  function themeSnapshotNow(){
    if(themeT<1){
      const el = performance.now()-themeTransStart;
      themeT = clamp(el/THEME_MS,0,1);
    }
    return lerpTheme(themeFrom,themeTo,themeT);
  }
  function setThemeIndex(idx, silent){
    if(idx===themeIndex) return;
    themeFrom = themeSnapshotNow();
    themeTo = THEMES[idx];
    themeIndex = idx;
    themeT = 0;
    themeTransStart = performance.now();
    if(!silent){ pulseFlash(rgba(themeTo.accent?parseColor(themeTo.accent):parseColor(themeTo.accent),0.35)); audio.milestone(); }
  }
  function resetTheme(){
    themeIndex = 0; themeFrom = THEMES[0]; themeTo = THEMES[0]; themeT = 1;
  }

  const root = document.documentElement;
  function applyCSSVars(t){
    root.style.setProperty('--accent', rgba(t.accent,1));
  }

  /* ===================== flash / shake ===================== */
  const flashEl = document.getElementById('flash');
  function pulseFlash(color){
    flashEl.style.transition='none';
    flashEl.style.background = color || 'var(--accent)';
    flashEl.style.opacity = '0.5';
    requestAnimationFrame(()=>{
      flashEl.style.transition='opacity 550ms ease-out';
      flashEl.style.opacity='0';
    });
  }
  function flashDanger(){
    flashEl.style.transition='none';
    flashEl.style.background = '#ff2244';
    flashEl.style.opacity='0.6';
    requestAnimationFrame(()=>{
      flashEl.style.transition='opacity 700ms ease-out';
      flashEl.style.opacity='0';
    });
  }
  let shakeMag = 0;
  function addShake(m){ if(reducedMotion) m*=0.25; shakeMag = Math.min(shakeMag+m, 30); }
  function updateShake(dt){ shakeMag *= Math.pow(0.90, dt); if(shakeMag<0.05) shakeMag=0; }

  /* ===================== audio ===================== */
  let actx = null;
  function ensureAudio(){
    if(actx) return;
    try{ actx = new (window.AudioContext||window.webkitAudioContext)(); }catch(e){ actx=null; }
  }
  function tone(freq,dur,type,vol,glideTo){
    if(!actx) return;
    try{
      const osc = actx.createOscillator();
      const gain = actx.createGain();
      osc.type = type||'sine';
      osc.frequency.setValueAtTime(freq, actx.currentTime);
      if(glideTo) osc.frequency.exponentialRampToValueAtTime(Math.max(glideTo,1), actx.currentTime+dur);
      gain.gain.setValueAtTime(vol!==undefined?vol:0.16, actx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.0001, actx.currentTime+dur);
      osc.connect(gain); gain.connect(actx.destination);
      osc.start(); osc.stop(actx.currentTime+dur+0.03);
    }catch(e){}
  }
  const audio = {
    hit(ratio){ tone(220+ratio*440, 0.09, 'square', 0.14); },
    wall(){ tone(160, 0.05, 'triangle', 0.07); },
    miss(){ tone(320, 0.5, 'sawtooth', 0.18, 45); },
    milestone(){ [523,659,784].forEach((f,i)=> setTimeout(()=>tone(f,0.16,'sine',0.14), i*90)); },
    win(){ [523,659,784,1046,1318].forEach((f,i)=> setTimeout(()=>tone(f,0.24,'sine',0.17), i*110)); }
  };

  /* ===================== persistent best score ===================== */
  let best = 0;
  async function loadBest(){
    try{
      if(window.storage){
        const res = await window.storage.get('neon-pong-best', false);
        if(res && res.value) best = parseInt(res.value,10)||0;
      }
    }catch(e){ /* no saved value yet, or storage unavailable */ }
    updateHUD();
  }
  async function saveBest(v){
    try{ if(window.storage){ await window.storage.set('neon-pong-best', String(v), false); } }catch(e){}
  }
  loadBest();

  /* ===================== paddle ===================== */
  const paddle = { x:40, y:undefined, width:14, height:100, vy:0, prevY:0, hitFlash:0 };
  let inputY = null;
  function setInputY(y){ inputY = y; }

  canvas.addEventListener('mousemove', e=> setInputY(e.clientY));
  canvas.addEventListener('mouseleave', ()=> { /* keep last known position */ });
  canvas.addEventListener('touchmove', e=>{ if(e.touches[0]) setInputY(e.touches[0].clientY); e.preventDefault(); }, {passive:false});
  canvas.addEventListener('touchstart', e=>{
    if(e.touches[0]) setInputY(e.touches[0].clientY);
    e.preventDefault();
    if(state==='title' || state==='gameover' || state==='win') startGame();
  }, {passive:false});
  canvas.addEventListener('click', ()=>{
    if(state==='title' || state==='gameover' || state==='win') startGame();
  });

  let keyUp=false, keyDown=false;
  window.addEventListener('keydown', e=>{
    if(e.code==='ArrowUp'||e.code==='KeyW') keyUp=true;
    if(e.code==='ArrowDown'||e.code==='KeyS') keyDown=true;
    if(e.code==='Space'||e.code==='Enter'){
      e.preventDefault();
      if(state==='title'||state==='gameover'||state==='win') startGame();
      else if(state==='paused') togglePause();
    }
    if(e.code==='Escape'){ togglePause(); }
  });
  window.addEventListener('keyup', e=>{
    if(e.code==='ArrowUp'||e.code==='KeyW') keyUp=false;
    if(e.code==='ArrowDown'||e.code==='KeyS') keyDown=false;
  });

  function updatePaddle(dt){
    paddle.prevY = paddle.y;
    if(inputY!==null){
      paddle.y = clamp(inputY - paddle.height/2, 0, H-paddle.height);
    }else{
      const kSpeed = 10*dt;
      if(keyUp) paddle.y -= kSpeed;
      if(keyDown) paddle.y += kSpeed;
      paddle.y = clamp(paddle.y, 0, H-paddle.height);
    }
    paddle.vy = dt>0 ? (paddle.y-paddle.prevY)/dt : 0;
    paddle.hitFlash *= Math.pow(0.85, dt);
  }
  function updateDemoPaddle(dt){
    const targetY = ball.y - paddle.height/2;
    paddle.y += (targetY-paddle.y)*Math.min(0.05*dt,1);
    paddle.y = clamp(paddle.y, 0, H-paddle.height);
    paddle.hitFlash *= Math.pow(0.85, dt);
  }

  /* ===================== ball ===================== */
  const WIN_SCORE = 15;
  const MAX_SPEED = 28;
  const ball = { x:0,y:0, r:9, dx:3, dy:2, speed:4, trail:[] };

  function resetBall(){
    ball.x = W*0.55; ball.y = H*0.5;
    ball.speed = 6;
    const angle = (Math.random()*0.5-0.25);
    ball.dx = -Math.abs(ball.speed*Math.cos(angle)); // head toward the paddle
    ball.dy = ball.speed*Math.sin(angle) + (Math.random()<0.5?-1:1)*1.2;
    ball.trail = [];
  }
  function demoBall(){
    ball.x = W*0.5; ball.y = H*0.5; ball.speed=4;
    const a = Math.random()*Math.PI*2;
    ball.dx = Math.cos(a)*ball.speed; ball.dy = Math.sin(a)*ball.speed*0.6 - 1;
    ball.trail = [];
  }
  function gameSpeedFactor(){ return (ball.speed||6)/6; }

  function onWallBounce(x,y){
    spawnParticles(x,y, rgba(currentTheme.accent,1), 6);
    addShake(2);
    audio.wall();
  }
  function onPaddleHit(){
    const rel = clamp((ball.y-(paddle.y+paddle.height/2)) / (paddle.height/2), -1, 1);
    ball.speed = Math.min(ball.speed*1.1, MAX_SPEED);
    const spin = clamp(paddle.vy*0.12, -4.5, 4.5);
    const angle = rel*0.85;
    ball.dx = Math.abs(Math.cos(angle))*ball.speed;
    ball.dy = Math.sin(angle)*ball.speed + spin;
    paddle.hitFlash = 1;
    score++;
    spawnParticles(ball.x, ball.y, rgba(currentTheme.accent,1), 20);
    addShake(4+ball.speed*0.3);
    floatScorePop(ball.x, ball.y);
    audio.hit(ball.speed/MAX_SPEED);
    updateHUD();
    const newIdx = Math.min(Math.floor(score/5), THEMES.length-1);
    if(newIdx!==themeIndex) setThemeIndex(newIdx);
    if(score>=WIN_SCORE) triggerWin();
  }
  function onMiss(){
    audio.miss();
    addShake(16);
    flashDanger();
    triggerGameOver();
  }

  function updateBall(dt){
    const prevX = ball.x, prevY = ball.y;
    ball.trail.push({x:ball.x,y:ball.y});
    if(ball.trail.length>16) ball.trail.shift();

    ball.x += ball.dx*dt;
    ball.y += ball.dy*dt;

    if(ball.y-ball.r<=0){ ball.y=ball.r; ball.dy=Math.abs(ball.dy); onWallBounce(ball.x,ball.y); }
    else if(ball.y+ball.r>=H){ ball.y=H-ball.r; ball.dy=-Math.abs(ball.dy); onWallBounce(ball.x,ball.y); }
    if(ball.x+ball.r>=W){ ball.x=W-ball.r; ball.dx=-Math.abs(ball.dx); onWallBounce(ball.x,ball.y); }

    const paddleFront = paddle.x+paddle.width;
    if(ball.dx<0 && prevX-ball.r>=paddleFront && ball.x-ball.r<=paddleFront){
      const denom = (ball.x-ball.r)-(prevX-ball.r);
      const t = denom!==0 ? (paddleFront-(prevX-ball.r))/denom : 0;
      const crossY = prevY + (ball.y-prevY)*clamp(t,0,1);
      if(crossY+ball.r>=paddle.y && crossY-ball.r<=paddle.y+paddle.height){
        ball.x = paddleFront+ball.r;
        ball.y = crossY;
        onPaddleHit();
      }
    }
    if(ball.x+ball.r<0) onMiss();
  }
  function updateDemoBall(dt){
    ball.x += ball.dx*dt; ball.y += ball.dy*dt;
    if(ball.y-ball.r<=0){ ball.y=ball.r; ball.dy=Math.abs(ball.dy); }
    if(ball.y+ball.r>=H){ ball.y=H-ball.r; ball.dy=-Math.abs(ball.dy); }
    if(ball.x-ball.r<=0){ ball.x=ball.r; ball.dx=Math.abs(ball.dx); }
    if(ball.x+ball.r>=W){ ball.x=W-ball.r; ball.dx=-Math.abs(ball.dx); }
  }

  /* ===================== particles & floaters ===================== */
  let particles = [];
  function spawnParticles(x,y,color,count){
    if(reducedMotion) count = Math.ceil(count*0.4);
    for(let i=0;i<count;i++){
      const a = Math.random()*Math.PI*2;
      const sp = 1+Math.random()*4;
      particles.push({ x,y, vx:Math.cos(a)*sp, vy:Math.sin(a)*sp, life:1, decay:0.025+Math.random()*0.025, color, size:1.5+Math.random()*2.5, grav:0.05 });
    }
  }
  function spawnConfetti(){
    const colors = THEMES.map(t=>t.accent);
    const n = reducedMotion ? 40 : 130;
    for(let i=0;i<n;i++){
      particles.push({
        x: Math.random()*W, y:-20-Math.random()*140,
        vx:(Math.random()-0.5)*3, vy:2+Math.random()*3,
        life:1, decay:0.0035+Math.random()*0.004,
        color: colors[Math.floor(Math.random()*colors.length)],
        size:3+Math.random()*3, grav:0.03
      });
    }
  }
  function updateParticles(dt){
    for(let i=particles.length-1;i>=0;i--){
      const p = particles[i];
      p.x += p.vx*dt; p.y += p.vy*dt; p.vy += p.grav*dt; p.life -= p.decay*dt;
      if(p.life<=0) particles.splice(i,1);
    }
  }
  function drawParticles(){
    particles.forEach(p=>{
      ctx.globalAlpha = clamp(p.life,0,1);
      ctx.fillStyle = p.color;
      ctx.shadowColor = p.color; ctx.shadowBlur = 8;
      ctx.beginPath(); ctx.arc(p.x,p.y,p.size,0,Math.PI*2); ctx.fill();
    });
    ctx.globalAlpha=1; ctx.shadowBlur=0;
  }
  let floaters = [];
  function floatScorePop(x,y){ floaters.push({x,y,life:1,text:'+1'}); }
  function updateFloaters(dt){
    for(let i=floaters.length-1;i>=0;i--){
      floaters[i].y -= 0.6*dt; floaters[i].life -= 0.018*dt;
      if(floaters[i].life<=0) floaters.splice(i,1);
    }
  }
  function drawFloaters(){
    floaters.forEach(f=>{
      ctx.globalAlpha = clamp(f.life,0,1);
      ctx.fillStyle = '#fff';
      ctx.font = "700 22px 'Rajdhani', sans-serif";
      ctx.textAlign='center';
      ctx.shadowColor = rgba(currentTheme.accent,1); ctx.shadowBlur=12;
      ctx.fillText(f.text, f.x, f.y);
    });
    ctx.globalAlpha=1; ctx.shadowBlur=0; ctx.textAlign='left';
  }

  /* ===================== skyline ===================== */
  let buildings = [];
  function buildSkyline(){
    buildings = [];
    let x = 0;
    while(x < W){
      const bw = 28+Math.random()*54;
      const bh = H*0.10 + Math.random()*H*0.22;
      const windows = [];
      const cols = Math.max(1, Math.floor(bw/11));
      const rows = Math.max(1, Math.floor(bh/15));
      for(let cx=0; cx<cols; cx++){
        for(let ry=0; ry<rows; ry++){
          if(Math.random()<0.32) windows.push({x:cx*11+4, y:ry*15+6, seed:Math.random()*10});
        }
      }
      buildings.push({x,w:bw,h:bh,windows});
      x += bw + 4 + Math.random()*12;
    }
  }

  /* ===================== background ===================== */
  function drawBackground(theme, time){
    const g = ctx.createLinearGradient(0,-MARGIN,0,H+MARGIN);
    g.addColorStop(0, rgba(theme.sky0,1));
    g.addColorStop(1, rgba(theme.sky1,1));
    ctx.fillStyle = g;
    ctx.fillRect(-MARGIN,-MARGIN,W+MARGIN*2,H+MARGIN*2);

    const horizonY = H*0.6;

    const hg = ctx.createRadialGradient(W*0.5,horizonY,0, W*0.5,horizonY, W*0.68);
    hg.addColorStop(0, rgba(theme.accent,0.32));
    hg.addColorStop(1, rgba(theme.accent,0));
    ctx.fillStyle = hg;
    ctx.fillRect(0, horizonY-H*0.28, W, H*0.56);

    buildings.forEach(b=>{
      const by = horizonY-b.h;
      ctx.fillStyle = rgba(theme.building,0.92);
      ctx.fillRect(b.x, by, b.w, b.h+6);
      b.windows.forEach(w=>{
        const flicker = 0.35+0.65*Math.abs(Math.sin(time*0.001+w.seed));
        ctx.fillStyle = rgba(theme.accent, 0.55*flicker);
        ctx.fillRect(b.x+w.x, by+w.y, 3,5);
      });
    });

    ctx.strokeStyle = rgba(theme.accent,0.85);
    ctx.lineWidth = 2;
    ctx.shadowColor = rgba(theme.accent,1); ctx.shadowBlur = 14;
    ctx.beginPath(); ctx.moveTo(-MARGIN,horizonY); ctx.lineTo(W+MARGIN,horizonY); ctx.stroke();
    ctx.shadowBlur = 0;

    ctx.save();
    ctx.beginPath(); ctx.rect(0,horizonY,W,H-horizonY+MARGIN); ctx.clip();
    const rows = 14;
    const scroll = (time*0.05*(1+gameSpeedFactor()*0.4))%40;
    for(let i=0;i<rows;i++){
      const p = (i*40+scroll)/(rows*40);
      const y = horizonY + Math.pow(p,1.8)*(H-horizonY)*2.3;
      if(y>H+20) continue;
      const alpha = Math.max(0.5*(1-p*0.6), 0.04);
      ctx.strokeStyle = rgba(theme.grid, alpha);
      ctx.lineWidth = 1;
      ctx.beginPath(); ctx.moveTo(0,y); ctx.lineTo(W,y); ctx.stroke();
    }
    const vanishX = W*0.5;
    for(let i=-6;i<=6;i++){
      const xBottom = vanishX + i*(W/9);
      ctx.strokeStyle = rgba(theme.grid,0.16);
      ctx.beginPath(); ctx.moveTo(vanishX,horizonY); ctx.lineTo(xBottom,H); ctx.stroke();
    }
    ctx.restore();

    // classic center divider, nostalgic nod to the original game
    ctx.save();
    ctx.setLineDash([10,14]);
    ctx.strokeStyle = rgba(theme.accent,0.12);
    ctx.lineWidth = 2;
    ctx.beginPath(); ctx.moveTo(W*0.5,0); ctx.lineTo(W*0.5,H); ctx.stroke();
    ctx.restore();
  }
  function drawDangerZone(theme){
    const pulse = 0.06+0.06*Math.abs(Math.sin(performance.now()*0.002));
    ctx.fillStyle = `rgba(255,40,70,${pulse})`;
    ctx.fillRect(0,0,paddle.x,H);
  }

  /* ===================== paddle / ball rendering ===================== */
  function drawPaddle(theme){
    ctx.save();
    ctx.shadowColor = rgba(theme.accent,1);
    ctx.shadowBlur = 18+paddle.hitFlash*22;
    const grad = ctx.createLinearGradient(paddle.x,paddle.y,paddle.x,paddle.y+paddle.height);
    grad.addColorStop(0, rgba(theme.accent,0.95));
    grad.addColorStop(0.5, `rgba(255,255,255,${0.85+paddle.hitFlash*0.15})`);
    grad.addColorStop(1, rgba(theme.accent,0.95));
    ctx.fillStyle = grad;
    roundRect(ctx, paddle.x, paddle.y, paddle.width, paddle.height, 6);
    ctx.fill();
    ctx.restore();
  }
  function drawBallAndTrail(theme){
    const c = rgba(theme.accent,1);
    for(let i=0;i<ball.trail.length;i++){
      const p = ball.trail[i];
      const t = i/ball.trail.length;
      ctx.globalAlpha = t*0.45;
      ctx.fillStyle = c;
      ctx.beginPath(); ctx.arc(p.x,p.y, Math.max(ball.r*t,1), 0, Math.PI*2); ctx.fill();
    }
    ctx.globalAlpha = 1;
    ctx.save();
    ctx.shadowColor = c; ctx.shadowBlur = 26;
    ctx.fillStyle = '#ffffff';
    ctx.beginPath(); ctx.arc(ball.x,ball.y,ball.r,0,Math.PI*2); ctx.fill();
    ctx.restore();
  }

  /* ===================== HUD ===================== */
  const scoreVal = document.getElementById('scoreVal');
  const bestVal = document.getElementById('bestVal');
  const speedVal = document.getElementById('speedVal');
  const progressEl = document.getElementById('progress');
  for(let i=0;i<WIN_SCORE;i++){
    const d = document.createElement('div');
    d.className='tick';
    progressEl.appendChild(d);
  }
  const ticks = progressEl.querySelectorAll('.tick');
  function updateHUD(){
    scoreVal.textContent = String(score).padStart(2,'0');
    bestVal.textContent = String(best).padStart(2,'0');
    speedVal.textContent = 'x'+(ball.speed/6).toFixed(2);
    ticks.forEach((t,i)=> t.classList.toggle('lit', i<score));
  }

  /* ===================== overlays / state machine ===================== */
  let state = 'title'; // title | playing | paused | gameover | win
  let score = 0;

  const overlays = {
    title: document.getElementById('overlay-title'),
    gameover: document.getElementById('overlay-gameover'),
    win: document.getElementById('overlay-win'),
    pause: document.getElementById('overlay-pause')
  };
  function hideAllOverlays(){ Object.values(overlays).forEach(o=>o.classList.remove('visible')); }
  function showOverlay(name){ hideAllOverlays(); overlays[name].classList.add('visible'); }

  function startGame(){
    ensureAudio();
    score = 0;
    resetTheme();
    resetBall();
    state = 'playing';
    hideAllOverlays();
    updateHUD();
  }
  function togglePause(){
    if(state==='playing'){ state='paused'; showOverlay('pause'); }
    else if(state==='paused'){ state='playing'; hideAllOverlays(); }
  }
  document.addEventListener('visibilitychange', ()=>{ if(document.hidden && state==='playing') togglePause(); });

  function triggerGameOver(){
    state = 'gameover';
    if(score>best){ best=score; saveBest(best); }
    document.getElementById('finalScore').textContent = score;
    document.getElementById('bestLine').textContent = 'BEST: '+String(best).padStart(2,'0');
    updateHUD();
    showOverlay('gameover');
  }
  function triggerWin(){
    state = 'win';
    if(score>best){ best=score; saveBest(best); }
    document.getElementById('bestLineWin').textContent = 'BEST: '+String(best).padStart(2,'0');
    updateHUD();
    spawnConfetti();
    audio.win();
    showOverlay('win');
  }

  document.getElementById('btnStart').addEventListener('click', startGame);
  document.getElementById('btnRetry').addEventListener('click', startGame);
  document.getElementById('btnAgain').addEventListener('click', startGame);
  document.getElementById('btnResume').addEventListener('click', togglePause);

  /* ===================== main loop ===================== */
  let lastTime = performance.now();
  function loop(now){
    const dtMs = now-lastTime; lastTime = now;
    const dt = clamp(dtMs/16.6667, 0, 3);

    currentTheme = themeSnapshotNow();
    applyCSSVars(currentTheme);

    if(state==='playing'){
      updatePaddle(dt);
      updateBall(dt);
    } else if(state==='title'){
      updateDemoPaddle(dt);
      updateDemoBall(dt);
    }
    updateShake(dt);
    updateParticles(dt);
    updateFloaters(dt);

    ctx.save();
    const sx = (Math.random()-0.5)*shakeMag;
    const sy = (Math.random()-0.5)*shakeMag;
    ctx.translate(sx,sy);

    drawBackground(currentTheme, now);
    if(state==='playing' || state==='paused') drawDangerZone(currentTheme);
    drawPaddle(currentTheme);
    drawBallAndTrail(currentTheme);
    drawParticles();
    drawFloaters();

    ctx.restore();
    requestAnimationFrame(loop);
  }

  /* ===================== init ===================== */
  resize();
  demoBall();
  requestAnimationFrame(loop);

})();
</script>
</body>
</html>
