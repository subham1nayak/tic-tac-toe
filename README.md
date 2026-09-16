# tic-tac-toe
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Tic Tac Toe — Animated</title>
  <style>
    :root{
      --bg1:#070A12;
      --bg2:#0D1230;
      --card: rgba(255,255,255,.08);
      --card2: rgba(255,255,255,.06);
      --stroke: rgba(255,255,255,.18);
      --text: rgba(255,255,255,.92);
      --muted: rgba(255,255,255,.68);

      --x:#7BE7FF;
      --o:#FF7BEF;
      --win:#7CFFB2;

      --shadow: 0 20px 60px rgba(0,0,0,.55);
      --radius: 18px;
    }

    *{ box-sizing:border-box; }
    html,body{ height:100%; }
    body{
      margin:0;
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, "Apple Color Emoji","Segoe UI Emoji";
      color:var(--text);
      background:
        radial-gradient(1200px 800px at 20% 10%, rgba(123,231,255,.14), transparent 60%),
        radial-gradient(900px 700px at 90% 30%, rgba(255,123,239,.12), transparent 55%),
        radial-gradient(700px 700px at 50% 100%, rgba(124,255,178,.10), transparent 60%),
        linear-gradient(180deg, var(--bg1), var(--bg2));
      display:grid;
      place-items:center;
      padding: 28px 16px;
      overflow-x:hidden;
    }

    .app{
      width:min(980px, 100%);
      display:grid;
      grid-template-columns: 1fr;
      gap: 14px;
      align-items:start;
    }

    .topbar{
      display:flex;
      gap:14px;
      align-items:stretch;
      flex-wrap:wrap;
      justify-content:space-between;
    }

    .card{
      background: linear-gradient(180deg, var(--card), var(--card2));
      border: 1px solid rgba(255,255,255,.10);
      box-shadow: var(--shadow);
      border-radius: var(--radius);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
    }

    .header.card{
      padding: 16px 18px;
      flex: 1 1 420px;
      min-width: 320px;
    }

    .header h1{
      margin:0 0 6px 0;
      font-weight: 780;
      letter-spacing:.2px;
      font-size: 18px;
    }

    .status{
      display:flex;
      gap:10px;
      align-items:center;
      flex-wrap:wrap;
      color: var(--muted);
      font-size: 13px;
      line-height: 1.3;
    }

    .pill{
      display:inline-flex;
      align-items:center;
      gap:8px;
      padding: 8px 10px;
      border-radius: 999px;
      border: 1px solid rgba(255,255,255,.12);
      background: rgba(255,255,255,.06);
      color: rgba(255,255,255,.86);
      font-weight: 650;
    }
    .dot{ width:10px; height:10px; border-radius:50%; display:inline-block; }
    .dot.x{ background: var(--x); }
    .dot.o{ background: var(--o); }
    .dot.win{ background: var(--win); }

    .controls.card{
      padding: 14px;
      flex: 0 0 auto;
      min-width: 320px;
      display:grid;
      gap: 10px;
      align-content:start;
    }

    .row{ display:flex; gap:10px; flex-wrap:wrap; }
    label{
      font-size: 12px;
      color: var(--muted);
      display:flex;
      flex-direction:column;
      gap:6px;
      min-width: 150px;
      flex: 1 1 160px;
    }

    select, button{
      appearance:none;
      -webkit-appearance:none;
      border: 1px solid rgba(255,255,255,.14);
      background: rgba(0,0,0,.18);
      color: rgba(255,255,255,.92);
      border-radius: 12px;
      padding: 10px 12px;
      font-weight: 650;
      outline:none;
      transition: transform .12s ease, border-color .12s ease, background .12s ease, box-shadow .12s ease;
    }

    select:hover, button:hover{
      border-color: rgba(255,255,255,.22);
      transform: translateY(-1px);
    }
    button{
      cursor:pointer;
      width:100%;
    }
    button.primary{
      background: linear-gradient(180deg, rgba(123,231,255,.20), rgba(255,123,239,.12));
    }
    button.ghost{
      background: rgba(255,255,255,.06);
    }

    .main{
      display:grid;
      grid-template-columns: 1fr;
      gap: 14px;
    }

    .boardWrap.card{
      padding: 18px;
    }

    .boardShell{
      width: min(520px, 100%);
      margin: 0 auto;
      position: relative;
    }

    .board{
      aspect-ratio: 1 / 1;
      width: 100%;
      display:grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
      padding: 10px;
      border-radius: 20px;
      background:
        radial-gradient(600px 300px at 50% 0%, rgba(255,255,255,.06), transparent 60%),
        rgba(255,255,255,.04);
      border: 1px solid rgba(255,255,255,.10);
      box-shadow: inset 0 0 0 1px rgba(0,0,0,.18);
      position: relative;
      overflow:hidden;
    }

    .cell{
      border: 1px solid rgba(255,255,255,.12);
      background: rgba(0,0,0,.15);
      border-radius: 16px;
      display:grid;
      place-items:center;
      cursor:pointer;
      position: relative;
      overflow:hidden;
      transition: transform .12s ease, border-color .12s ease, background .12s ease;
      user-select:none;
      -webkit-tap-highlight-color: transparent;
    }

    .cell:hover{
      transform: translateY(-1px);
      border-color: rgba(255,255,255,.22);
      background: rgba(255,255,255,.06);
    }

    .cell:active{
      transform: translateY(0px) scale(.99);
    }

    .cell[disabled]{
      cursor:not-allowed;
      opacity: 0.98;
    }

    .cell::after{
      content:"";
      position:absolute;
      inset:-50%;
      background: radial-gradient(circle at var(--mx, 50%) var(--my, 50%),
        rgba(255,255,255,.14),
        rgba(255,255,255,0) 50%);
      opacity:0;
      transition: opacity .18s ease;
    }
    .cell:hover::after{ opacity:1; }

    .mark{
      width: 74%;
      height: 74%;
      filter: drop-shadow(0 10px 18px rgba(0,0,0,.35));
    }

    .mark path, .mark circle{
      fill: none;
      stroke-width: 10;
      stroke-linecap: round;
      stroke-linejoin: round;
      stroke-dasharray: 110;
      stroke-dashoffset: 110;
    }

    .mark.x path{ stroke: var(--x); }
    .mark.o circle{ stroke: var(--o); }

    .draw1{ animation: draw 260ms cubic-bezier(.2,.9,.2,1) forwards; }
    .draw2{ animation: draw 260ms cubic-bezier(.2,.9,.2,1) forwards; animation-delay: 90ms; }
    .drawO{ animation: draw 340ms cubic-bezier(.2,.9,.2,1) forwards; }

    @keyframes draw{
      to{ stroke-dashoffset: 0; }
    }

    .winCell{
      border-color: rgba(124,255,178,.55) !important;
      background: rgba(124,255,178,.10) !important;
      box-shadow: 0 0 0 1px rgba(124,255,178,.15), 0 10px 22px rgba(0,0,0,.35);
      transform: translateY(-1px);
    }

    .overlayLine{
      position:absolute;
      inset: 10px;
      pointer-events:none;
    }
    .overlayLine svg{
      width:100%;
      height:100%;
      display:block;
    }
    .overlayLine line{
      stroke: rgba(124,255,178,.92);
      stroke-width: 12;
      stroke-linecap: round;
      filter: drop-shadow(0 12px 20px rgba(0,0,0,.45));
      stroke-dasharray: 400;
      stroke-dashoffset: 400;
      transition: opacity .2s ease;
      opacity: 0;
    }
    .overlayLine.show line{
      opacity: 1;
      animation: winline 520ms cubic-bezier(.2,.9,.2,1) forwards;
    }
    @keyframes winline{
      to{ stroke-dashoffset: 0; }
    }

    .footerNote{
      color: rgba(255,255,255,.58);
      font-size: 12px;
      padding: 0 2px;
      text-align:center;
    }

    /* Confetti canvas */
    canvas#confetti{
      position: fixed;
      inset: 0;
      width: 100vw;
      height: 100vh;
      pointer-events: none;
      z-index: 99;
    }

    /* Reduced motion */
    @media (prefers-reduced-motion: reduce){
      *{ animation: none !important; transition: none !important; }
      .cell:hover{ transform:none; }
      .overlayLine line{ stroke-dashoffset: 0; }
    }
  </style>
</head>
<body>
  <canvas id="confetti" aria-hidden="true"></canvas>

  <div class="app">
    <div class="topbar">
      <div class="header card">
        <h1>Tic‑Tac‑Toe (Animated)</h1>
        <div class="status" id="status">
          <span class="pill"><span class="dot x"></span><span id="turnText">X to move</span></span>
          <span class="pill"><span class="dot o"></span><span id="modeText">2 Players</span></span>
          <span class="pill" id="hintPill" style="display:none;"><span class="dot win"></span><span id="hintText">Win!</span></span>
        </div>
      </div>

      <div class="controls card">
        <div class="row">
          <label>
            Mode
            <select id="mode">
              <option value="pvp">2 Players (Local)</option>
              <option value="ai">Vs Computer (Perfect)</option>
            </select>
          </label>
          <label>
            Starting Player
            <select id="start">
              <option value="X">X starts</option>
              <option value="O">O starts</option>
            </select>
          </label>
        </div>
        <div class="row">
          <button class="primary" id="newGame">New Game</button>
          <button class="ghost" id="resetScore">Reset Score</button>
        </div>
        <div class="row" style="justify-content:space-between; gap:10px;">
          <div class="pill" style="flex:1; justify-content:space-between;">
            <span><span class="dot x"></span> X</span><span id="scoreX">0</span>
          </div>
          <div class="pill" style="flex:1; justify-content:space-between;">
            <span><span class="dot o"></span> O</span><span id="scoreO">0</span>
          </div>
          <div class="pill" style="flex:1; justify-content:space-between;">
            <span>Draws</span><span id="scoreD">0</span>
          </div>
        </div>
      </div>
    </div>

    <div class="main">
      <div class="boardWrap card">
        <div class="boardShell">
          <div class="board" id="board" role="grid" aria-label="Tic tac toe board"></div>
          <div class="overlayLine" id="overlayLine" aria-hidden="true">
            <svg viewBox="0 0 300 300">
              <line id="winLine" x1="0" y1="0" x2="0" y2="0"></line>
            </svg>
          </div>
        </div>
      </div>

      <div class="footerNote">
        Tip: hover a tile for a subtle light effect. On win, you’ll get an animated line + confetti.
      </div>
    </div>
  </div>

<script>
(() => {
  const WIN_LINES = [
    [0,1,2],[3,4,5],[6,7,8],
    [0,3,6],[1,4,7],[2,5,8],
    [0,4,8],[2,4,6]
  ];

  // Predefined line coordinates in a 300x300 viewBox (nice padding)
  const LINE_COORDS = new Map([
    ["0,1,2",  {x1:30,y1:50,  x2:270,y2:50}],
    ["3,4,5",  {x1:30,y1:150, x2:270,y2:150}],
    ["6,7,8",  {x1:30,y1:250, x2:270,y2:250}],
    ["0,3,6",  {x1:50,y1:30,  x2:50,y2:270}],
    ["1,4,7",  {x1:150,y1:30, x2:150,y2:270}],
    ["2,5,8",  {x1:250,y1:30, x2:250,y2:270}],
    ["0,4,8",  {x1:30,y1:30,  x2:270,y2:270}],
    ["2,4,6",  {x1:270,y1:30, x2:30,y2:270}],
  ]);

  const elBoard = document.getElementById('board');
  const elStatusTurn = document.getElementById('turnText');
  const elMode = document.getElementById('mode');
  const elStart = document.getElementById('start');
  const elModeText = document.getElementById('modeText');
  const elNew = document.getElementById('newGame');
  const elResetScore = document.getElementById('resetScore');
  const elHintPill = document.getElementById('hintPill');
  const elHintText = document.getElementById('hintText');

  const elScoreX = document.getElementById('scoreX');
  const elScoreO = document.getElementById('scoreO');
  const elScoreD = document.getElementById('scoreD');

  const overlayLine = document.getElementById('overlayLine');
  const winLine = document.getElementById('winLine');

  const confettiCanvas = document.getElementById('confetti');
  const ctx = confettiCanvas.getContext('2d');

  let board = Array(9).fill(null);
  let current = 'X';
  let active = true;
  let locked = false;

  let score = { X:0, O:0, D:0 };

  function resizeConfetti(){
    const dpr = Math.max(1, window.devicePixelRatio || 1);
    confettiCanvas.width = Math.floor(window.innerWidth * dpr);
    confettiCanvas.height = Math.floor(window.innerHeight * dpr);
    confettiCanvas.style.width = window.innerWidth + "px";
    confettiCanvas.style.height = window.innerHeight + "px";
    ctx.setTransform(dpr,0,0,dpr,0,0);
  }
  window.addEventListener('resize', resizeConfetti);
  resizeConfetti();

  function setHint(show, text){
    elHintPill.style.display = show ? '' : 'none';
    elHintText.textContent = text || '';
  }

  function setModeText(){
    elModeText.textContent = elMode.value === 'ai' ? 'Vs Computer' : '2 Players';
  }
  elMode.addEventListener('change', () => { setModeText(); newGame(); });

  function renderBoard(){
    elBoard.innerHTML = '';
    for (let i=0;i<9;i++){
      const btn = document.createElement('button');
      btn.className = 'cell';
      btn.type = 'button';
      btn.role = 'gridcell';
      btn.setAttribute('aria-label', `Cell ${i+1}`);
      btn.dataset.idx = String(i);

      // mouse light position
      btn.addEventListener('pointermove', (e) => {
        const r = btn.getBoundingClientRect();
        const mx = ((e.clientX - r.left) / r.width) * 100;
        const my = ((e.clientY - r.top) / r.height) * 100;
        btn.style.setProperty('--mx', mx.toFixed(2) + '%');
        btn.style.setProperty('--my', my.toFixed(2) + '%');
      });

      btn.addEventListener('click', () => handleMove(i));
      elBoard.appendChild(btn);

      if (board[i]) placeMarkVisual(btn, board[i], false);
    }
  }

  function placeMarkVisual(cellEl, player, animate=true){
    cellEl.innerHTML = '';
    const svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
    svg.setAttribute('viewBox', '0 0 100 100');
    svg.classList.add('mark');

    if (player === 'X'){
      svg.classList.add('x');
      const p1 = document.createElementNS('http://www.w3.org/2000/svg', 'path');
      p1.setAttribute('d', 'M 20 20 L 80 80');
      p1.setAttribute('pathLength', '110');

      const p2 = document.createElementNS('http://www.w3.org/2000/svg', 'path');
      p2.setAttribute('d', 'M 80 20 L 20 80');
      p2.setAttribute('pathLength', '110');

      svg.appendChild(p1); svg.appendChild(p2);
      cellEl.appendChild(svg);

      if (animate){
        requestAnimationFrame(() => {
          p1.classList.add('draw1');
          p2.classList.add('draw2');
        });
      } else {
        p1.style.strokeDashoffset = 0;
        p2.style.strokeDashoffset = 0;
      }
    } else {
      svg.classList.add('o');
      const c = document.createElementNS('http://www.w3.org/2000/svg', 'circle');
      c.setAttribute('cx', '50');
      c.setAttribute('cy', '50');
      c.setAttribute('r', '30');
      c.setAttribute('pathLength', '110');
      svg.appendChild(c);
      cellEl.appendChild(svg);

      if (animate){
        requestAnimationFrame(() => c.classList.add('drawO'));
      } else {
        c.style.strokeDashoffset = 0;
      }
    }
  }

  function checkWinner(b){
    for (const line of WIN_LINES){
      const [a,c,d] = line;
      if (b[a] && b[a] === b[c] && b[a] === b[d]){
        return { winner: b[a], line };
      }
    }
    if (b.every(v => v)) return { winner: 'D', line: null };
    return { winner: null, line: null };
  }

  function showWinLine(line){
    if (!line) return;
    const key = line.join(',');
    const coords = LINE_COORDS.get(key);
    if (!coords) return;

    winLine.setAttribute('x1', coords.x1);
    winLine.setAttribute('y1', coords.y1);
    winLine.setAttribute('x2', coords.x2);
    winLine.setAttribute('y2', coords.y2);

    overlayLine.classList.remove('show');
    // restart animation
    void overlayLine.offsetWidth;
    overlayLine.classList.add('show');
  }

  function clearWinLine(){
    overlayLine.classList.remove('show');
  }

  function setTurnText(){
    if (!active) return;
    elStatusTurn.textContent = `${current} to move`;
  }

  function disableBoard(disabled){
    [...elBoard.children].forEach(btn => btn.disabled = disabled);
  }

  function handleMove(idx){
    if (!active || locked) return;
    if (board[idx]) return;

    const mode = elMode.value;

    // If AI mode and it's AI's turn, ignore clicks
    if (mode === 'ai' && current === 'O') return;

    playMove(idx, current);

    const result = checkWinner(board);
    if (result.winner) return endGame(result);

    current = (current === 'X') ? 'O' : 'X';
    setTurnText();

    if (mode === 'ai' && current === 'O'){
      aiTurn();
    }
  }

  function playMove(idx, player){
    board[idx] = player;
    const cell = elBoard.children[idx];
    if (cell){
      placeMarkVisual(cell, player, true);
      cell.disabled = true;
    }
  }

  function endGame({winner, line}){
    active = false;
    disableBoard(true);

    if (winner === 'D'){
      score.D++;
      elScoreD.textContent = score.D;
      elStatusTurn.textContent = `Draw`;
      setHint(true, "Draw — try again!");
      clearWinLine();
      return;
    }

    score[winner]++;
    if (winner === 'X') elScoreX.textContent = score.X;
    if (winner === 'O') elScoreO.textContent = score.O;

    elStatusTurn.textContent = `${winner} wins`;
    setHint(true, "Win!");
    showWinLine(line);

    // highlight winning cells
    if (line){
      for (const i of line){
        elBoard.children[i].classList.add('winCell');
      }
    }

    launchConfetti();
  }

  function newGame(){
    board = Array(9).fill(null);
    current = elStart.value;
    active = true;
    locked = false;
    setHint(false);

    clearWinLine();
    renderBoard();
    disableBoard(false);
    [...elBoard.children].forEach(btn => btn.classList.remove('winCell'));

    setModeText();
    setTurnText();

    // If AI mode and O starts, let AI move first
    if (elMode.value === 'ai' && current === 'O'){
      aiTurn();
    }
  }

  elNew.addEventListener('click', newGame);
  elStart.addEventListener('change', newGame);

  elResetScore.addEventListener('click', () => {
    score = {X:0,O:0,D:0};
    elScoreX.textContent = "0";
    elScoreO.textContent = "0";
    elScoreD.textContent = "0";
    newGame();
  });

  // --- Perfect AI (minimax) ---
  function aiTurn(){
    if (!active) return;
    locked = true;
    disableBoard(true);

    // small delay for a nicer feel
    setTimeout(() => {
      const idx = bestMove(board, 'O', 'X');
      disableBoard(false);
      locked = false;

      if (idx == null) return; // should not happen
      playMove(idx, 'O');

      const result = checkWinner(board);
      if (result.winner) return endGame(result);

      current = 'X';
      setTurnText();
    }, 260);
  }

  function bestMove(b, ai, human){
    let bestScore = -Infinity;
    let move = null;

    for (let i=0;i<9;i++){
      if (b[i]) continue;
      b[i] = ai;
      const score = minimax(b, 0, false, ai, human);
      b[i] = null;
      if (score > bestScore){
        bestScore = score;
        move = i;
      }
    }
    return move;
  }

  function minimax(b, depth, isMax, ai, human){
    const res = checkWinner(b);
    if (res.winner){
      if (res.winner === ai) return 10 - depth;
      if (res.winner === human) return depth - 10;
      return 0;
    }

    if (isMax){
      let best = -Infinity;
      for (let i=0;i<9;i++){
        if (b[i]) continue;
        b[i] = ai;
        best = Math.max(best, minimax(b, depth+1, false, ai, human));
        b[i] = null;
      }
      return best;
    } else {
      let best = Infinity;
      for (let i=0;i<9;i++){
        if (b[i]) continue;
        b[i] = human;
        best = Math.min(best, minimax(b, depth+1, true, ai, human));
        b[i] = null;
      }
      return best;
    }
  }

  // --- Confetti (lightweight canvas) ---
  let confettiPieces = [];
  let confettiRunning = false;

  function launchConfetti(){
    const colors = ["#7BE7FF","#FF7BEF","#7CFFB2","#FFFFFF"];
    const W = window.innerWidth, H = window.innerHeight;

    confettiPieces = Array.from({length: 160}, () => ({
      x: Math.random()*W,
      y: -20 - Math.random()*H*0.25,
      vx: (Math.random()-0.5)*4.2,
      vy: 2.5 + Math.random()*4.5,
      r: 3 + Math.random()*5,
      rot: Math.random()*Math.PI*2,
      vr: (Math.random()-0.5)*0.25,
      color: colors[(Math.random()*colors.length)|0],
      alpha: 0.9
    }));

    confettiRunning = true;
    const t0 = performance.now();

    const tick = (t) => {
      if (!confettiRunning) return;
      const dt = Math.min(32, t - (tick._last || t));
      tick._last = t;

      ctx.clearRect(0,0,window.innerWidth, window.innerHeight);

      for (const p of confettiPieces){
        p.vy += 0.02 * dt;          // gravity
        p.x += p.vx * (dt/16);
        p.y += p.vy * (dt/16);
        p.rot += p.vr * dt;

        // drift
        p.vx *= 0.999;

        // wrap a bit
        if (p.x < -40) p.x = window.innerWidth + 40;
        if (p.x > window.innerWidth + 40) p.x = -40;

        // fade out near end
        const life = (t - t0) / 1200;
        p.alpha = Math.max(0, 0.95 - life);

        ctx.save();
        ctx.globalAlpha = p.alpha;
        ctx.translate(p.x, p.y);
        ctx.rotate(p.rot);
        ctx.fillStyle = p.color;
        ctx.fillRect(-p.r, -p.r*0.6, p.r*2, p.r*1.2);
        ctx.restore();
      }

      if (t - t0 < 1200){
        requestAnimationFrame(tick);
      } else {
        confettiRunning = false;
        ctx.clearRect(0,0,window.innerWidth, window.innerHeight);
      }
    };

    requestAnimationFrame(tick);
  }

  // init
  renderBoard();
  setModeText();
  newGame();
})();
</script>
</body>
</html>
