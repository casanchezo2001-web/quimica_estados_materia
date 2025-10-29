# quimica_estados_materia
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Estados de la Materia — Juega y Aprende</title>
  <meta name="description" content="Actividad interactiva para niños: sólido, líquido y gaseoso con una llama que derrite y evapora. Narración por voz, sonidos y modo automático." />
  <style>
    :root{
      --bg:#0e1320;           /* fondo */
      --card:#111936;          /* tarjeta */
      --accent:#ffd166;        /* amarillo cálido */
      --accent-2:#ef476f;      /* rosa/rojo para llama */
      --text:#f8fafc;          /* texto */
      --muted:#94a3b8;         /* texto suave */
      --liquid:#3ab0ff;        /* azul líquido */
      --solid:#cfefff;         /* hielo */
      --ok:#06d6a0;            /* verde */
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0; font-family: ui-rounded, system-ui, -apple-system, Segoe UI, Roboto, Noto Sans, Ubuntu, Cantarell, Helvetica Neue, Arial, "Apple Color Emoji","Segoe UI Emoji";
      background: radial-gradient(60% 60% at 50% 20%, #1a2240, var(--bg));
      color:var(--text); display:flex; align-items:center; justify-content:center; padding:16px;
    }
    .app{
      width:min(980px, 100%); background:linear-gradient(180deg, #121a33 0%, #0d1326 100%);
      border-radius:24px; box-shadow:0 10px 40px rgba(0,0,0,.35), inset 0 0 0 1px rgba(255,255,255,.06);
      padding:clamp(16px, 3vw, 28px);
    }
    header{ display:flex; gap:12px; align-items:center; justify-content:center; text-align:center; flex-wrap:wrap; }
    header h1{ font-size:clamp(22px, 3.2vw, 36px); margin:0; letter-spacing:.2px }
    header p{ margin:6px 0 0; color:var(--muted); font-size:clamp(14px, 2.3vw, 18px) }

    .stage{ display:grid; grid-template-columns: 1fr 180px; gap:clamp(12px, 2.4vw, 20px); margin-top:clamp(14px, 2vw, 20px); }
    @media (max-width: 860px){ .stage{ grid-template-columns: 1fr } }

    /* LAB */
    .lab{ position:relative; background:var(--card); border-radius:18px; padding:18px; overflow:hidden; min-height:420px; display:flex; flex-direction:column; align-items:center; justify-content:flex-end; box-shadow:inset 0 0 0 1px rgba(255,255,255,.05) }
    .sky-sparkles{ position:absolute; inset:0; pointer-events:none; background:
        radial-gradient(6px 6px at 20% 10%, rgba(255,255,255,.12), transparent 60%),
        radial-gradient(5px 5px at 80% 25%, rgba(255,255,255,.09), transparent 60%),
        radial-gradient(4px 4px at 60% 65%, rgba(255,255,255,.08), transparent 60%),
        radial-gradient(3px 3px at 35% 80%, rgba(255,255,255,.07), transparent 60%) }

    .beaker{ width:min(480px, 92%); height: clamp(280px, 42vw, 380px); border-radius: 22px 22px 10px 10px; background: linear-gradient(180deg, rgba(255,255,255,0.06), rgba(255,255,255,0.02)); outline: 2px solid rgba(255,255,255,.18); box-shadow: inset 0 6px 10px rgba(0,0,0,.25), 0 10px 20px rgba(0,0,0,.25); position: relative; overflow: hidden; backdrop-filter: blur(1px) }
    .beaker::after{ content:""; position:absolute; inset:0; background:
        radial-gradient(40% 25% at 40% 0%, rgba(255,255,255,.35), transparent 65%),
        radial-gradient(30% 20% at 70% 2%, rgba(255,255,255,.25), transparent 70%); pointer-events:none }

    .solid, .liquid, .gas{ position:absolute; inset:0 }

    /* Sólido */
    .ice-cube{ position:absolute; width:48px; height:48px; border-radius:8px; background: linear-gradient(180deg, #ffffff, var(--solid)); box-shadow: inset 0 -6px 10px rgba(0,0,0,.12), 0 6px 12px rgba(0,0,0,.2); opacity:.95; animation: float 4s ease-in-out infinite }
    .ice-cube:nth-child(2){ left:26%; bottom:24%; animation-duration:4.6s }
    .ice-cube:nth-child(3){ left:52%; bottom:22%; animation-duration:5.2s }
    .ice-cube:nth-child(4){ left:70%; bottom:26%; animation-duration:4.2s }
    .ice-cube:nth-child(5){ left:15%; bottom:18%; animation-duration:4.8s }
    @keyframes float{ 0%,100%{ transform:translateY(0)} 50%{ transform:translateY(-6px)} }

    /* Líquido */
    .liquid{ --level: 45%; display:block; bottom:0; height:var(--level); background:none; overflow:visible }
    .liquid .water{ position:absolute; inset:auto 0 0 0; height:100%; background: linear-gradient(180deg, rgba(58,176,255,0.85) 0%, rgba(58,176,255,0.9) 60%, rgba(58,176,255,0.95) 100%); filter:drop-shadow(0 -6px 12px rgba(0,0,0,.25)) }
    .liquid .wave{ position:absolute; left:-25%; right:-25%; top:-14px; height:28px; opacity:.9; background: radial-gradient(50% 130% at 50% 120%, rgba(255,255,255,.85), rgba(255,255,255,0) 70%); animation: wave 3.4s ease-in-out infinite }
    .liquid .wave:nth-child(2){ top:-8px; opacity:.7; animation-duration:4.2s }
    @keyframes wave{ 0%,100%{ transform: translateX(0)} 50%{ transform: translateX(8%) } }

    /* Gas */
    .bubble{ position:absolute; bottom:6%; left:50%; width:14px; height:14px; border-radius:50%; background: radial-gradient(circle at 30% 30%, #fff, rgba(255,255,255,.25) 40%, rgba(255,255,255,0) 70%); filter: blur(.2px); opacity:0; animation: rise 3.2s linear infinite }
    .bubble:nth-child(2){ left:35%; animation-duration:3.8s }
    .bubble:nth-child(3){ left:60%; animation-duration:3.1s }
    .bubble:nth-child(4){ left:42%; animation-duration:2.9s }
    .bubble:nth-child(5){ left:70%; animation-duration:3.6s }
    @keyframes rise{ 0%{ transform:translate(-50%,0) scale(.6); opacity:0 } 15%{ opacity:.9 } 100%{ transform:translate(-50%,-300px) scale(1); opacity:0 } }
    .steam{ position:absolute; bottom:70%; left:50%; width:200px; height:150px; pointer-events:none; opacity:0; background:
        radial-gradient(80px 60px at 30% 50%, rgba(255,255,255,.18), transparent 70%),
        radial-gradient(70px 50px at 60% 70%, rgba(255,255,255,.15), transparent 70%),
        radial-gradient(60px 40px at 45% 30%, rgba(255,255,255,.12), transparent 70%); transform:translateX(-50%); animation: steamUp 4s ease-in-out infinite }
    @keyframes steamUp{ 0%{ opacity:.05; transform:translate(-50%, 0) } 50%{ opacity:.22; transform:translate(-50%, -10px)} 100%{ opacity:.05; transform:translate(-50%, 0) } }

    /* Nieve cuando hace mucho frío */
    .snow{ position:absolute; inset:0; pointer-events:none; opacity:0; transition:opacity .3s }
    .flake{ position:absolute; top:-10px; width:6px; height:6px; border-radius:50%; background:rgba(255,255,255,.9); filter:blur(.3px); animation: fall 6s linear infinite }
    @keyframes fall{ to{ transform: translateY(120%) rotate(360deg) } }

    /* Controles */
    .controls{ background:var(--card); border-radius:18px; padding:16px; box-shadow:inset 0 0 0 1px rgba(255,255,255,.05); display:flex; flex-direction:column; gap:14px; align-items:center; justify-content:center; min-height: 240px }
    .big-label{ font-size:clamp(16px, 2.6vw, 22px); text-align:center }
    .status{ text-align:center; background:rgba(255,255,255,.04); border-radius:14px; padding:12px 14px; min-width:100% }
    .status .chip{ display:inline-block; padding:6px 12px; border-radius:999px; font-weight:700; letter-spacing:.3px; margin-top:6px }
    .row{ display:flex; gap:10px; align-items:center; justify-content:center; flex-wrap:wrap }
    button{ border:none; border-radius:14px; padding:12px 16px; font-size:16px; font-weight:700; cursor:pointer; color:#0b1222; background:var(--accent); box-shadow:0 6px 14px rgba(0,0,0,.25), inset 0 -2px 0 rgba(0,0,0,.15); transition:transform .08s ease, box-shadow .08s ease; min-width:120px }
    button:active{ transform:translateY(1px); box-shadow:0 4px 10px rgba(0,0,0,.25) }
    .ghost{ background:transparent; color:var(--text); outline:1px solid rgba(255,255,255,.18) }
    .ok{ background:var(--ok) }

    .flame-wrap{ display:flex; flex-direction:column; align-items:center; gap:10px }
    .flame-emoji{ font-size:clamp(46px, 8vw, 64px); line-height:1; filter: drop-shadow(0 6px 12px rgba(239,71,111,.45)) }
    .range{ -webkit-appearance: none; width: 180px; height: 26px; background:linear-gradient(90deg, #184a66, #ef476f 60%, #ffd166); border-radius: 999px; outline:none; overflow:hidden; box-shadow: inset 0 0 0 2px rgba(255,255,255,.12) }
    .range::-webkit-slider-thumb{ -webkit-appearance:none; appearance:none; width:28px; height:28px; border-radius:50%; background:#fff; box-shadow:0 2px 6px rgba(0,0,0,.35); border:2px solid #111 }
    .range::-moz-range-thumb{ width:28px; height:28px; border-radius:50%; background:#fff; border:2px solid #111 }
    .legend{ font-size:13px; color:var(--muted) }

    /* Termómetro lateral */
    .thermo{ display:flex; flex-direction:column; align-items:center; gap:4px; margin-top:4px }
    .ticks{ display:grid; grid-template-rows: repeat(10, 10px); width:10px; border-radius:999px; overflow:hidden; outline:1px solid rgba(255,255,255,.15); position:relative }
    .tick{ background:rgba(255,255,255,.08) }
    .mercury{ position:absolute; width:10px; background:linear-gradient(180deg, #ef476f, #ffd166); bottom:0; border-radius:999px; height:0; transition:height .2s ease }

    footer{ margin-top:10px; text-align:center; color:var(--muted); font-size:13px }
    a{ color:#7cd4ff }

    /* Vapor que sale del vaso */
    .steam-out{ position:absolute; width:240px; height:180px; pointer-events:none; opacity:0; background:
      radial-gradient(90px 70px at 30% 60%, rgba(255,255,255,.16), transparent 70%),
      radial-gradient(70px 55px at 65% 65%, rgba(255,255,255,.14), transparent 70%),
      radial-gradient(60px 40px at 45% 25%, rgba(255,255,255,.12), transparent 70%);
      animation: steamOut 5s ease-in-out infinite; filter: blur(0.2px) }
    @keyframes steamOut{ 0%{ opacity:.05; transform:translate(-50%, 0) } 50%{ opacity:.25; transform:translate(-50%, -12px)} 100%{ opacity:.05; transform:translate(-50%, 0) } }

    .sound-tip{ font-size:12px; color:var(--muted); text-align:center }
  </style>
</head>
<body>
  <div class="app" role="application" aria-label="Juego interactivo: estados de la materia">
    <header>
      <h1>🌟 Estados de la Materia</h1>
      <p>Sube la <strong>llama</strong> para derretir el hielo (sólido), formar agua (líquido) y crear vapor (gas). Ahora con <strong>modo automático</strong>, <strong>narración</strong> y <strong>sonidos</strong>.</p>
    </header>

    <section class="stage" aria-live="polite">
      <div class="lab">
        <!-- Vapor que sale del vaso (posicionado por JS en la boca del vaso) -->
        <div class="steam-out" id="steamOut" aria-hidden="true" style="left:50%; top:20%; transform:translateX(-50%)"></div>
        <div class="sky-sparkles" aria-hidden="true"></div>
        <div class="beaker" aria-label="Vaso con la sustancia" role="img">
          <!-- Sólido -->
          <div class="solid" id="solidLayer" aria-hidden="false">
            <div class="ice-cube" style="left:8%; bottom:20%"></div>
            <div class="ice-cube"></div>
            <div class="ice-cube"></div>
            <div class="ice-cube"></div>
            <div class="ice-cube"></div>
          </div>

          <!-- Líquido -->
          <div class="liquid" id="liquidLayer" style="--level: 42%; opacity:0" aria-hidden="true">
            <div class="water"></div>
            <div class="wave"></div>
            <div class="wave"></div>
            <div class="bubble"></div>
            <div class="bubble"></div>
            <div class="bubble"></div>
            <div class="bubble"></div>
            <div class="bubble"></div>
          </div>

          <!-- Gas -->
          <div class="gas" id="gasLayer" style="opacity:0" aria-hidden="true">
            <div class="steam"></div>
          </div>

          <!-- Nieve para frío extremo -->
          <div class="snow" id="snow"></div>
        </div>
      </div>

      <!-- CONTROLES -->
      <aside class="controls">
        <div class="big-label">🔥 Alza la llama</div>
        <div class="flame-wrap">
          <div class="flame-emoji" id="flameEmoji" aria-hidden="true">🔥</div>
          <input id="heat" class="range" type="range" min="0" max="100" value="0" aria-label="Temperatura" />
          <div class="legend">0 = frío ❄️ · 100 = muy caliente ♨️</div>
        </div>

        <div class="status" id="status">
          <div>Ahora es:</div>
          <div class="chip" id="stateChip" style="background:#cfefff; color:#0b1222">SÓLIDO 🧊</div>
          <div id="helperText" style="margin-top:6px; font-size:14px; color:var(--muted)">El hielo está durito. ¡Súbele a la llama para derretirlo!</div>
        </div>

        <div class="row" style="margin-top:6px">
          <button id="heatUpBtn" aria-label="Calentar">Calentar</button>
          <button id="coolDownBtn" class="ghost" aria-label="Enfriar">Enfriar</button>
          <button id="resetBtn" class="ghost" aria-label="Reiniciar">Reiniciar</button>
          <button id="autoBtn" class="ok" aria-label="Modo automático">Auto</button>
        </div>
        <div class="row">
          <button id="speakBtn" class="ghost" aria-label="Narración por voz">Narrar</button>
          <button id="muteBtn" class="ghost" aria-label="Silenciar narración">Silencio</button>
          <button id="soundBtn" class="ghost" aria-label="Sonidos">Sonidos ON</button>
        </div>

        <!-- Termómetro visual -->
        <div class="thermo" aria-hidden="true">
          <div style="position:relative">
            <div class="ticks">
              <div class="tick"></div><div class="tick"></div><div class="tick"></div><div class="tick"></div><div class="tick"></div>
              <div class="tick"></div><div class="tick"></div><div class="tick"></div><div class="tick"></div><div class="tick"></div>
              <div class="mercury" id="mercury"></div>
            </div>
          </div>
          <small class="legend">Termómetro</small>
        </div>
      </aside>
    </section>

    <footer>
      Consejo: explica que al <em>calentar</em> las partículas se mueven más. Al <em>enfriar</em>, se mueven menos.
    </footer>
  </div>

  <script>
    // ====== CONSTANTES ======
    const THRESHOLDS = { solidMax: 28, liquidMax: 68, gasMax: 100 };

    // ====== UTILIDADES ======
    function clamp(n, min, max){ return Math.max(min, Math.min(n, max)); }

    // ====== REFERENCIAS DOM ======
    const heat = document.getElementById('heat');
    const solidLayer = document.getElementById('solidLayer');
    const liquidLayer = document.getElementById('liquidLayer');
    const gasLayer = document.getElementById('gasLayer');
    const stateChip = document.getElementById('stateChip');
    const helperText = document.getElementById('helperText');
    const flameEmoji = document.getElementById('flameEmoji');
    const snow = document.getElementById('snow');
    const mercury = document.getElementById('mercury');
    const beaker = document.querySelector('.beaker');
    const steamOut = document.getElementById('steamOut');

    // Botones
    const heatUpBtn = document.getElementById('heatUpBtn');
    const coolDownBtn = document.getElementById('coolDownBtn');
    const resetBtn = document.getElementById('resetBtn');
    const autoBtn = document.getElementById('autoBtn');
    const speakBtn = document.getElementById('speakBtn');
    const muteBtn = document.getElementById('muteBtn');
    const soundBtn = document.getElementById('soundBtn');

    // ====== ESTADO ======
    let autoInterval = null;
    let autoDir = 1; // 1 sube, -1 baja
    let speaking = false;
    let audioCtx = null, steamNode = null, steamGain = null; let soundOn = true;

    // ====== AUDIO ======
    function ensureAudio(){
      if(audioCtx) return;
      try{ audioCtx = new (window.AudioContext||window.webkitAudioContext)(); }catch(e){ audioCtx = null; }
    }
    function popBubble(){
      if(!audioCtx || !soundOn) return;
      const now = audioCtx.currentTime;
      const o = audioCtx.createOscillator();
      const g = audioCtx.createGain();
      o.type = 'sine'; o.frequency.value = 680 + Math.random()*220; // tono agudo
      g.gain.setValueAtTime(0, now);
      g.gain.linearRampToValueAtTime(0.12, now + 0.01);
      g.gain.exponentialRampToValueAtTime(0.0001, now + 0.18);
      o.connect(g).connect(audioCtx.destination); o.start(now); o.stop(now + 0.2);
    }
    function sizzle(){
      if(!audioCtx || !soundOn) return;
      const now = audioCtx.currentTime;
      const o = audioCtx.createOscillator(); const g = audioCtx.createGain();
      o.type='triangle'; o.frequency.value = 220;
      g.gain.setValueAtTime(0, now);
      g.gain.linearRampToValueAtTime(0.1, now + 0.03);
      g.gain.exponentialRampToValueAtTime(0.0001, now + 0.25);
      o.connect(g).connect(audioCtx.destination); o.start(now); o.stop(now + 0.26);
    }
    function startSteam(){
      if(!audioCtx || !soundOn || steamNode) return;
      const bufferSize = 2 * audioCtx.sampleRate;
      const noiseBuffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
      const output = noiseBuffer.getChannelData(0);
      for (let i = 0; i < bufferSize; i++) output[i] = Math.random() * 2 - 1;
      const noise = audioCtx.createBufferSource(); noise.buffer = noiseBuffer; noise.loop = true;
      const filter = audioCtx.createBiquadFilter(); filter.type='lowpass'; filter.frequency.value = 1800;
      steamGain = audioCtx.createGain(); steamGain.gain.value = 0.0;
      noise.connect(filter).connect(steamGain).connect(audioCtx.destination);
      noise.start(); steamNode = noise;
      const now = audioCtx.currentTime; steamGain.gain.linearRampToValueAtTime(0.06, now + 0.4);
    }
    function stopSteam(){
      if(!audioCtx || !steamNode || !steamGain) return;
      const now = audioCtx.currentTime; steamGain.gain.linearRampToValueAtTime(0.0001, now + 0.3);
      setTimeout(()=>{ try{ steamNode.stop(); }catch(e){} steamNode = null; }, 350);
    }
    function maybePlayStateSFX(value){
      if(!audioCtx || !soundOn) return;
      if(value <= THRESHOLDS.solidMax){ stopSteam(); }
      if(value > THRESHOLDS.solidMax && value < THRESHOLDS.liquidMax){ if(Math.random()<0.25) popBubble(); stopSteam(); }
      if(value >= THRESHOLDS.liquidMax){ if(Math.random()<0.35) popBubble(); startSteam(); }
    }

    // ====== NARRACIÓN ======
    function narrate(text){
      if(!speaking) return;
      try{ window.speechSynthesis.cancel(); const u = new SpeechSynthesisUtterance(text); u.lang = 'es-ES'; u.rate = 1; u.pitch = 1.05; speechSynthesis.speak(u); }catch(e){}
    }

    // ====== HAPTICS ======
    function vib(ms=20){ if(navigator.vibrate) navigator.vibrate(ms); }

    // ====== NIEVE ======
    function makeSnowflakes(count=16){
      if(!snow) return;
      snow.innerHTML = '';
      for(let i=0;i<count;i++){
        const f = document.createElement('div');
        f.className = 'flake';
        f.style.left = Math.random()*100 + '%';
        f.style.animationDuration = (4+Math.random()*4) + 's';
        f.style.animationDelay = (Math.random()*2) + 's';
        snow.appendChild(f);
      }
    }

    // ====== TERMÓMETRO ======
    function updateThermo(value){ if(mercury){ mercury.style.height = value + '%'; } }

    // ====== ESCENA ======
    function updateScene(value){
      // llama
      const s = 1 + (value/100)*0.3;
      flameEmoji.style.transform = `scale(${s})`;
      flameEmoji.style.filter = `drop-shadow(0 6px 12px rgba(239,71,111,${0.25 + value/300}))`;
      updateThermo(value);

      if(value <= THRESHOLDS.solidMax){
        // SÓLIDO
        solidLayer.style.opacity = 1; solidLayer.setAttribute('aria-hidden','false');
        liquidLayer.style.opacity = 0; liquidLayer.setAttribute('aria-hidden','true');
        gasLayer.style.opacity = 0; gasLayer.setAttribute('aria-hidden','true');
        stateChip.textContent = 'SÓLIDO 🧊'; stateChip.style.background = '#cfefff'; stateChip.style.color = '#0b1222';
        helperText.textContent = 'El hielo está durito. ¡Súbele a la llama para derretirlo!';
        if(snow){ snow.style.opacity = 0.6; makeSnowflakes(); }
        if(steamOut) steamOut.style.opacity = 0;
      } else if(value <= THRESHOLDS.liquidMax){
        // LÍQUIDO
        const level = 32 + (value-THRESHOLDS.solidMax) * 0.9;
        liquidLayer.style.setProperty('--level', `${clamp(level, 30, 70)}%`);
        solidLayer.style.opacity = Math.max(0, 1 - (value-THRESHOLDS.solidMax)/10);
        solidLayer.setAttribute('aria-hidden', solidLayer.style.opacity <= 0 ? 'true':'false');
        liquidLayer.style.opacity = 1; liquidLayer.setAttribute('aria-hidden','false');
        gasLayer.style.opacity = 0; gasLayer.setAttribute('aria-hidden','true');
        stateChip.textContent = 'LÍQUIDO 💧'; stateChip.style.background = '#a8e1ff'; stateChip.style.color = '#0b1222';
        helperText.textContent = '¡Es agua! Se mueve y hace olitas. Si calientas más, ¡aparecerá vapor!';
        if(snow) snow.style.opacity = 0;
        if(steamOut) steamOut.style.opacity = 0;
      } else {
        // GAS
        solidLayer.style.opacity = 0; solidLayer.setAttribute('aria-hidden','true');
        liquidLayer.style.opacity = 0.12; liquidLayer.setAttribute('aria-hidden','false');
        gasLayer.style.opacity = 1; gasLayer.setAttribute('aria-hidden','false');
        stateChip.textContent = 'GASEOSO ☁️'; stateChip.style.background = '#ffd166'; stateChip.style.color = '#0b1222';
        helperText.textContent = '¡Vapor! Las partículas vuelan y se separan. Si bajas la llama, volverá a líquido.';
        if(snow) snow.style.opacity = 0;
        if(steamOut) steamOut.style.opacity = 1; // mostrar vapor externo
      }

      // Narración contextual
      if(value === 0) narrate('Muy frío. Es un sólido, como el hielo.');
      if(value === 30) narrate('Se derrite y se vuelve líquido.');
      if(value === 70) narrate('Se evapora y forma vapor.');

      // Audio contextual
      maybePlayStateSFX(value);
    }

    // ====== POSICIONAR VAPOR EXTERNO ======
    function positionSteamOut(){
      if(!beaker || !steamOut) return;
      const r = beaker.getBoundingClientRect();
      const lab = document.querySelector('.lab').getBoundingClientRect();
      const rimX = r.left + r.width/2 - lab.left; // centro
      const rimY = r.top + 16 - lab.top;         // cerca del borde superior
      steamOut.style.left = rimX + 'px';
      steamOut.style.top = rimY + 'px';
      steamOut.style.transform = 'translate(-50%, -8px)';
    }

    // ====== CONTROLES ======
    function setTemp(v){ v = clamp(v, 0, 100); heat.value = v; updateScene(v); }

    heat.addEventListener('input', (e)=> updateScene(parseInt(e.target.value,10)));
    heat.addEventListener('change', ()=> vib(10));

    heatUpBtn.addEventListener('click', ()=> setTemp(parseInt(heat.value,10) + 8));
    coolDownBtn.addEventListener('click', ()=> setTemp(parseInt(heat.value,10) - 8));
    resetBtn.addEventListener('click', ()=> { setTemp(0); narrate('Reiniciamos. ¡Otra vez!'); vib(20); });

    // Auto
    autoBtn.addEventListener('click', ()=>{
      if(autoInterval){ clearInterval(autoInterval); autoInterval = null; autoBtn.textContent = 'Auto'; vib(10); return; }
      autoBtn.textContent = 'Auto ▶'; vib(10);
      autoInterval = setInterval(()=>{
        let v = parseInt(heat.value,10) + autoDir*2;
        if(v >= 100){ v = 100; autoDir = -1; narrate('¡Mira! Ahora es gas.'); }
        if(v <= 0){ v = 0; autoDir = 1; narrate('Muy frío. ¡Hielo!'); }
        setTemp(v);
      }, 90);
    });

    // Narración
    speakBtn.addEventListener('click', ()=>{ speaking = true; narrate('Hola. Jugaremos con los estados de la materia.'); vib(10); });
    muteBtn.addEventListener('click', ()=>{ speaking = false; window.speechSynthesis.cancel(); vib(10); });

    // Habilitar audio tras primera interacción (evita errores de autoplay)
    const userInteractEvents = ['click','touchstart','pointerdown','keydown','input'];
    if(Array.isArray(userInteractEvents)){
      userInteractEvents.forEach(ev=>{
        document.addEventListener(ev, ()=>{ if(!audioCtx) ensureAudio(); }, {once:true});
      });
    }

    // Botón sonidos
    soundBtn.addEventListener('click', ()=>{
      ensureAudio();
      soundOn = !soundOn;
      soundBtn.textContent = soundOn ? 'Sonidos ON' : 'Sonidos OFF';
      vib(10);
      if(!soundOn) stopSteam();
    });

    // Posicionar vapor en resize y load
    window.addEventListener('resize', positionSteamOut);
    window.addEventListener('load', positionSteamOut);

    // ====== SELF-TESTS (no cambian la UI) ======
    (function runSelfTests(){
      const tests = [];
      function ok(name, cond){ tests.push({name, pass: !!cond}); }
      ok('clamp límites', clamp(-5,0,100)===0 && clamp(150,0,100)===100);
      ok('THRESHOLDS ordenados', THRESHOLDS.solidMax < THRESHOLDS.liquidMax && THRESHOLDS.liquidMax < THRESHOLDS.gasMax);
      ok('elementos DOM clave', !!solidLayer && !!liquidLayer && !!gasLayer && !!beaker);
      ok('array eventos existe', Array.isArray(userInteractEvents));
      if(window.console && console.table){ console.table(tests); } else { console.log('SELF-TESTS', tests); }
    })();

    // ====== INICIO ======
    setTemp(0);
  </script>
</body>
</html>

