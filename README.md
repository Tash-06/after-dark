<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dice of Desire — After Dark</title>
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Dice of Desire">
<meta name="theme-color" content="#1b1210">
<style>
  :root{
    --ink:#f2e8da;
    --dim:#c9b8a3;
    --bg-deep:#1b1210;
    --bg-mid:#241816;
    --wine:#7a1f2b;
    --wine-bright:#a4283a;
    --gold:#c9a24a;
    --line: rgba(242,232,218,0.14);
  }
  *{box-sizing:border-box;}
  html,body{
    margin:0; padding:0; height:100%;
    background:
      radial-gradient(1200px 800px at 50% -10%, #2a1c19 0%, var(--bg-deep) 55%),
      var(--bg-deep);
    color:var(--ink);
    font-family: Georgia, 'Iowan Old Style', 'Palatino Linotype', serif;
    -webkit-tap-highlight-color: transparent;
    overflow-x:hidden;
  }
  .wrap{
    min-height:100%;
    display:flex; flex-direction:column; align-items:center;
    padding: 28px 18px 60px;
  }
  header{ text-align:center; margin-bottom: 14px; }
  .eyebrow{
    letter-spacing:.32em; text-transform:uppercase; font-size:11px;
    color: var(--gold); opacity:.85;
  }
  h1{
    font-family: 'Trebuchet MS', 'Arial Black', system-ui, sans-serif;
    font-weight:900; letter-spacing:.02em;
    font-size: clamp(26px, 7.5vw, 38px);
    margin: 6px 0 4px;
  }
  .sub{ color:var(--dim); font-size:14px; font-style:italic; }

  .turn-bar{
    width:100%; max-width:380px; display:flex; align-items:center; justify-content:space-between;
    gap:10px; margin: 16px 0 4px; padding: 10px 14px; border-radius:10px;
    background: var(--bg-mid); border:1px solid var(--line);
  }
  .turn-label{ font-size:11px; letter-spacing:.12em; text-transform:uppercase; color:var(--dim); }
  .turn-name{ font-family:'Trebuchet MS', system-ui, sans-serif; font-weight:800; color:var(--gold); font-size:15px; }
  .turn-edit{
    background:none; border:1px solid var(--line); color:var(--dim); font-size:11px;
    padding:5px 10px; border-radius:20px; cursor:pointer; letter-spacing:.05em; text-transform:uppercase;
  }
  .names-form{ width:100%; max-width:380px; display:none; gap:8px; margin-bottom:4px; }
  .names-form.open{ display:flex; }
  .names-form input{
    flex:1; min-width:0; background:var(--bg-mid); border:1px solid var(--line); border-radius:8px;
    color:var(--ink); padding:9px 10px; font-family: Georgia, serif; font-size:13px;
  }
  .names-form button{
    background:var(--gold); border:none; border-radius:8px; padding:0 14px; color:#1b1210;
    font-weight:800; font-size:12px; letter-spacing:.05em; text-transform:uppercase; cursor:pointer;
  }

  .row{ display:flex; gap:8px; width:100%; max-width:380px; margin: 10px 0; }
  .toggle{
    flex:1; padding:10px 6px; text-align:center; border-radius:8px;
    border:1px solid var(--line); background: var(--bg-mid);
    color:var(--dim); font-size:13px; letter-spacing:.08em; text-transform:uppercase;
    cursor:pointer; user-select:none; transition: all .15s ease;
    font-family: 'Trebuchet MS', system-ui, sans-serif; font-weight:700;
  }
  .toggle.active[data-tier="mild"]{ background:#4a5d3a; border-color:#6b8354; color:var(--ink); box-shadow:0 0 0 1px #6b8354, 0 4px 14px rgba(74,93,58,.4); }
  .toggle.active[data-tier="bold"]{ background:var(--wine); border-color:var(--wine-bright); color:var(--ink); box-shadow:0 0 0 1px var(--wine-bright), 0 4px 14px rgba(122,31,43,.45); }
  .toggle.active[data-tier="unleashed"]{ background:#3a0e14; border-color:#c9284a; color:var(--ink); box-shadow:0 0 0 1px #c9284a, 0 4px 18px rgba(201,40,74,.5); }

  .dice-stage{
    width:100%; max-width:380px; display:flex; gap:16px; margin: 22px 0 8px;
  }
  .die{
    flex:1; aspect-ratio: 1; border-radius:16px;
    background: linear-gradient(160deg, #2a1a1a, #180f0d);
    border:1px solid var(--line);
    display:flex; align-items:center; justify-content:center;
    text-align:center; padding: 14px 10px;
    transition: transform .5s cubic-bezier(.2,.8,.2,1);
  }
  .die.rolling{ animation: shake .5s ease; }
  @keyframes shake{
    0%{ transform: rotate(0deg) scale(1); }
    25%{ transform: rotate(-8deg) scale(1.03); }
    50%{ transform: rotate(6deg) scale(0.98); }
    75%{ transform: rotate(-4deg) scale(1.02); }
    100%{ transform: rotate(0deg) scale(1); }
  }
  .die-label{
    position:relative;
  }
  .die-kicker{
    font-family:'Trebuchet MS', system-ui, sans-serif; font-weight:800;
    letter-spacing:.2em; text-transform:uppercase; font-size:10px; color:var(--gold);
    margin-bottom: 10px; display:block;
  }
  .die-word{
    font-size: clamp(17px, 5.5vw, 21px); line-height:1.3; color:var(--ink);
  }

  .combo{
    width:100%; max-width:380px; text-align:center; margin: 10px 0 4px;
    padding: 16px; border-radius:14px; border:1px solid var(--line);
    background: var(--bg-mid); min-height: 30px;
  }
  .combo-text{ font-size: 15px; color:var(--dim); font-style: italic; }
  .combo-text b{ color: var(--ink); font-style: normal; }

  .roll-btn{
    width:100%; max-width:380px; padding:16px; border-radius:10px;
    background: var(--gold); color:#1b1210; border:none;
    font-family:'Trebuchet MS', system-ui, sans-serif; font-weight:800;
    letter-spacing:.12em; text-transform:uppercase; font-size:15px;
    cursor:pointer; margin-top:14px;
    box-shadow: 0 6px 18px rgba(201,162,74,0.25);
  }
  .roll-btn:active{ transform: scale(0.98); }

  .meta{
    width:100%; max-width:380px; margin-top:14px; font-size:12px; color:var(--dim);
    text-align:center; letter-spacing:.05em;
  }
  footer{ margin-top: 26px; color:var(--dim); font-size:11px; letter-spacing:.08em; text-align:center; opacity:.7;}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <div class="eyebrow">After Dark Series</div>
    <h1>Dice of Desire</h1>
    <div class="sub">Roll both. Whoever's turn it is receives.</div>
  </header>

  <div class="turn-bar">
    <div><span class="turn-label">Receiving —</span> <span class="turn-name" id="turnName">Player 1</span></div>
    <div class="turn-edit" onclick="toggleNamesForm()">Edit names</div>
  </div>
  <div class="names-form" id="namesForm">
    <input type="text" id="p1Input" placeholder="Player 1 name" maxlength="16">
    <input type="text" id="p2Input" placeholder="Player 2 name" maxlength="16">
    <button onclick="saveNames()">Save</button>
  </div>

  <div class="row">
    <div class="toggle active" data-tier="mild" onclick="setTier('mild')">Mild</div>
    <div class="toggle" data-tier="bold" onclick="setTier('bold')">Bold</div>
    <div class="toggle" data-tier="unleashed" onclick="setTier('unleashed')">Unleashed 👀</div>
  </div>

  <div class="dice-stage">
    <div class="die" id="dieAction">
      <div class="die-label">
        <span class="die-kicker">Action</span>
        <div class="die-word" id="actionWord">?</div>
      </div>
    </div>
    <div class="die" id="dieSpot">
      <div class="die-label">
        <span class="die-kicker">Spot</span>
        <div class="die-word" id="spotWord">?</div>
      </div>
    </div>
  </div>

  <div class="combo">
    <div class="combo-text" id="comboText">Roll to find out what <b id="comboName">Player 1</b> gets.</div>
  </div>

  <button class="roll-btn" onclick="roll()">Roll the Dice</button>
  <div class="meta" id="rollCount">Rolls: 0</div>

  <footer>PLAY WITH CONSENT · SKIP ANYTHING, ANYTIME · SWAP ROLES WHENEVER YOU WANT</footer>
</div>

<script>
const FACES = {
  mild: {
    actions: ["Kiss", "Massage", "Trace fingers over", "Nibble", "Whisper against", "Slowly stroke"],
    spots: ["Neck", "Lips", "Ear", "Collarbone", "Shoulder", "Wrist"]
  },
  bold: {
    actions: ["Kiss slowly", "Bite gently", "Trace tongue over", "Squeeze", "Tease with fingertips", "Suck gently on", "Grip", "Pin and kiss"],
    spots: ["Inner thigh", "Lower back", "Hips", "Chest", "Backside", "Stomach", "Just above the waistband", "Wherever curves the most", "Wherever they choose", "Behind the ear, then down"]
  },
  unleashed: {
    actions: ["Rip open", "Pin down and bite", "Slowly undress while kissing", "Command them to strip, then", "Grip hard and", "Take control and", "Whisper a filthy command, then", "Trail kisses lower until they beg"],
    spots: ["Wherever makes them gasp", "Whatever you unzip first", "The one place they've been waiting all night", "Wherever you're told to", "Chest, slow and deliberate", "Backside, gripped and pulled close", "Lower, and lower still", "Wherever they can't say no to", "The spot that ends the game", "Anywhere skin meets skin"]
  }
};

let tier = 'mild';
let players = ['Player 1', 'Player 2'];
let turnIndex = 0;
let rollCount = 0;

function updateTurnDisplay(){
  document.getElementById('turnName').textContent = players[turnIndex];
  const comboName = document.getElementById('comboName');
  if(comboName){ comboName.textContent = players[turnIndex]; }
}

function toggleNamesForm(){
  const form = document.getElementById('namesForm');
  document.getElementById('p1Input').value = players[0];
  document.getElementById('p2Input').value = players[1];
  form.classList.toggle('open');
}

function saveNames(){
  const p1 = document.getElementById('p1Input').value.trim();
  const p2 = document.getElementById('p2Input').value.trim();
  players[0] = p1 || 'Player 1';
  players[1] = p2 || 'Player 2';
  updateTurnDisplay();
  document.getElementById('namesForm').classList.remove('open');
}

function setTier(t){
  tier = t;
  document.querySelectorAll('.toggle[data-tier]').forEach(el=>{
    el.classList.toggle('active', el.dataset.tier===tier);
  });
}

function pick(arr){
  return arr[Math.floor(Math.random()*arr.length)];
}

function roll(){
  const dieA = document.getElementById('dieAction');
  const dieS = document.getElementById('dieSpot');
  dieA.classList.remove('rolling'); void dieA.offsetWidth; dieA.classList.add('rolling');
  dieS.classList.remove('rolling'); void dieS.offsetWidth; dieS.classList.add('rolling');

  const set = FACES[tier];
  let ticks = 0;
  const interval = setInterval(()=>{
    document.getElementById('actionWord').textContent = pick(set.actions);
    document.getElementById('spotWord').textContent = pick(set.spots);
    ticks++;
    if(ticks>=6){
      clearInterval(interval);
      const finalAction = pick(set.actions);
      const finalSpot = pick(set.spots);
      document.getElementById('actionWord').textContent = finalAction;
      document.getElementById('spotWord').textContent = finalSpot;
      const receiver = players[turnIndex];
      document.getElementById('comboText').innerHTML =
        '<b>'+finalAction+'</b> the <b>'+finalSpot.toLowerCase()+'</b> of <b>'+receiver+'</b>.';
      rollCount++;
      document.getElementById('rollCount').textContent = 'Rolls: '+rollCount;
      turnIndex = 1 - turnIndex;
      updateTurnDisplay();
    }
  }, 90);
}

updateTurnDisplay();
</script>
</body>
</html>
