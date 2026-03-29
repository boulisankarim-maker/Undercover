# Undercover
Voici enfin le jeu Undercover en ladder en ligne.


<style>
  @import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:wght@400;500&display=swap');
  *{box-sizing:border-box;margin:0;padding:0}
  :root{
    --accent:#E24B4A;
    --accent2:#378ADD;
    --gold:#EF9F27;
    --bg:var(--color-background-primary);
    --bg2:var(--color-background-secondary);
    --txt:var(--color-text-primary);
    --txt2:var(--color-text-secondary);
    --brd:var(--color-border-tertiary);
  }
  #app{font-family:'DM Sans',sans-serif;color:var(--txt);padding:1rem 0;min-height:400px}
  h1{font-family:'Bebas Neue',sans-serif;font-size:2.4rem;letter-spacing:2px;color:var(--accent);margin-bottom:4px}
  .sub{font-size:13px;color:var(--txt2);margin-bottom:1.5rem}
  .screen{display:none}
  .screen.active{display:block}
  .card{background:var(--bg);border:0.5px solid var(--brd);border-radius:12px;padding:1rem 1.25rem;margin-bottom:12px}
  .btn{display:inline-block;padding:10px 24px;border-radius:8px;border:0.5px solid var(--brd);background:var(--bg);color:var(--txt);font-family:'DM Sans',sans-serif;font-size:14px;font-weight:500;cursor:pointer;transition:background 0.15s}
  .btn:hover{background:var(--bg2)}
  .btn-primary{background:var(--accent);color:#fff;border-color:var(--accent)}
  .btn-primary:hover{background:#c93a39}
  .btn-blue{background:var(--accent2);color:#fff;border-color:var(--accent2)}
  .btn-blue:hover{background:#2a70c4}
  input[type=text]{width:100%;padding:10px;border:0.5px solid var(--brd);border-radius:8px;font-family:'DM Sans',sans-serif;font-size:14px;color:var(--txt);background:var(--bg2);margin-bottom:8px}
  .player-row{display:flex;align-items:center;justify-content:space-between;padding:10px 0;border-bottom:0.5px solid var(--brd)}
  .player-row:last-child{border-bottom:none}
  .role-reveal{text-align:center;padding:2rem 1rem}
  .role-word{font-family:'Bebas Neue',sans-serif;font-size:3rem;letter-spacing:3px;color:var(--accent2);margin:1rem 0}
  .role-type{font-size:13px;font-weight:500;padding:6px 16px;border-radius:20px;display:inline-block;margin-bottom:1rem}
  .vote-btn{display:block;width:100%;text-align:left;padding:12px 16px;border:0.5px solid var(--brd);border-radius:8px;background:var(--bg);color:var(--txt);font-family:'DM Sans',sans-serif;font-size:14px;cursor:pointer;margin-bottom:8px;transition:all 0.15s}
  .vote-btn:hover{background:var(--bg2);border-color:var(--accent)}
  .vote-btn.selected{border-color:var(--accent);background:#FCEBEB;color:#A32D2D}
  .result-box{text-align:center;padding:2rem}
  .result-title{font-family:'Bebas Neue',sans-serif;font-size:2.5rem;letter-spacing:2px;margin-bottom:0.5rem}
  .leaderboard-row{display:flex;align-items:center;gap:12px;padding:10px 0;border-bottom:0.5px solid var(--brd)}
  .leaderboard-row:last-child{border-bottom:none}
  .lb-rank{font-family:'Bebas Neue',sans-serif;font-size:1.3rem;width:32px;text-align:center;color:var(--txt2)}
  .lb-name{flex:1;font-weight:500}
  .lb-pts{font-size:13px;color:var(--txt2)}
  .steps{display:flex;gap:8px;margin-bottom:1.5rem;flex-wrap:wrap}
  .step{font-size:12px;padding:4px 10px;border-radius:20px;border:0.5px solid var(--brd);color:var(--txt2)}
  .step.active{background:var(--accent);color:#fff;border-color:var(--accent)}
  .turn-indicator{background:var(--bg2);border-radius:8px;padding:10px 16px;margin-bottom:16px;font-size:14px;color:var(--txt2)}
  .turn-indicator strong{color:var(--txt)}
  .clue-list{margin-top:12px}
  .clue-item{font-size:14px;padding:6px 0;border-bottom:0.5px solid var(--brd);display:flex;gap:8px}
  .clue-item:last-child{border-bottom:none}
  .clue-player{font-weight:500;min-width:90px}
  .clue-word{color:var(--txt2)}
  .grid2{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:12px}
  .stat-card{background:var(--bg2);border-radius:8px;padding:10px 14px;text-align:center}
  .stat-val{font-family:'Bebas Neue',sans-serif;font-size:1.8rem;color:var(--accent)}
  .stat-lbl{font-size:12px;color:var(--txt2)}
  #hint{font-size:12px;color:var(--txt2);margin-top:4px}
</style>

<div id="app">

<div id="screen-home" class="screen active">
  <h1>Undercover</h1>
  <p class="sub">Le jeu des imposteurs — 3 à 5 joueurs</p>
  <div class="card" style="margin-bottom:16px">
    <p style="font-size:13px;color:var(--txt2);margin-bottom:12px">Nom des joueurs</p>
    <div id="player-inputs">
      <input type="text" placeholder="Joueur 1" id="p0">
      <input type="text" placeholder="Joueur 2" id="p1">
      <input type="text" placeholder="Joueur 3" id="p2">
    </div>
    <div style="display:flex;gap:8px;margin-top:4px">
      <button class="btn" onclick="addPlayer()" id="btn-add">+ Ajouter</button>
      <button class="btn" onclick="removePlayer()" id="btn-rem" style="display:none">- Retirer</button>
    </div>
  </div>
  <button class="btn btn-primary" onclick="startGame()" style="width:100%">Lancer la partie</button>
  <div style="margin-top:16px">
    <button class="btn" onclick="showLeaderboard()" style="width:100%">Classement</button>
  </div>
</div>

<div id="screen-reveal" class="screen">
  <h1>Undercover</h1>
  <p class="sub">Passe le téléphone à chaque joueur</p>
  <div class="role-reveal card">
    <p style="font-size:14px;color:var(--txt2);margin-bottom:8px" id="reveal-player-name"></p>
    <button class="btn btn-blue" onclick="showRole()" id="btn-show-role">Voir mon rôle</button>
    <div id="role-content" style="display:none">
      <span class="role-type" id="role-type-badge"></span>
      <div class="role-word" id="role-word-display"></div>
      <p style="font-size:13px;color:var(--txt2);margin-bottom:1rem" id="role-desc"></p>
      <button class="btn" onclick="nextReveal()">OK, j'ai vu</button>
    </div>
  </div>
</div>

<div id="screen-game" class="screen">
  <h1>Undercover</h1>
  <div class="steps" id="game-steps"></div>
  <div class="turn-indicator">Tour de <strong id="current-player-name"></strong></div>
  <div class="card">
    <p style="font-size:13px;color:var(--txt2);margin-bottom:8px">Donne un indice sur ton mot :</p>
    <input type="text" id="clue-input" placeholder="Ton indice...">
    <p id="hint"></p>
    <button class="btn btn-primary" onclick="submitClue()" style="margin-top:4px">Valider l'indice</button>
  </div>
  <div id="clue-list" class="clue-list" style="display:none">
    <p style="font-size:12px;color:var(--txt2);margin-bottom:8px">Indices donnés :</p>
    <div id="clues-given"></div>
  </div>
</div>

<div id="screen-vote" class="screen">
  <h1>Vote</h1>
  <p class="sub">Qui est l'infiltré ?</p>
  <div id="vote-buttons"></div>
  <button class="btn btn-primary" onclick="confirmVote()" id="btn-confirm-vote" style="width:100%;margin-top:8px" disabled>Éliminer</button>
</div>

<div id="screen-result" class="screen">
  <div class="result-box">
    <div class="result-title" id="result-title"></div>
    <p style="margin:8px 0 1.5rem;color:var(--txt2)" id="result-sub"></p>
    <div class="card" id="result-reveal" style="margin-bottom:16px;text-align:left">
      <p style="font-size:13px;color:var(--txt2);margin-bottom:8px">Les rôles :</p>
      <div id="result-roles"></div>
    </div>
    <div class="grid2">
      <div class="stat-card"><div class="stat-val" id="stat-rounds"></div><div class="stat-lbl">manches</div></div>
      <div class="stat-card"><div class="stat-val" id="stat-elim"></div><div class="stat-lbl">éliminés</div></div>
    </div>
    <button class="btn btn-primary" onclick="newGame()" style="width:100%;margin-bottom:8px">Rejouer</button>
    <button class="btn" onclick="goHome()" style="width:100%">Accueil</button>
  </div>
</div>

<div id="screen-leaderboard" class="screen">
  <h1>Classement</h1>
  <p class="sub">Tes stats globales</p>
  <div class="card">
    <div id="lb-list"></div>
  </div>
  <button class="btn" onclick="goHome()" style="margin-top:12px;width:100%">Retour</button>
</div>

</div>

<script>
const WORDS=[["Pizza","Hamburger"],["Chien","Chat"],["Plage","Piscine"],["Football","Rugby"],["Cinéma","Théâtre"],["Voiture","Moto"],["Roi","Président"],["Guitare","Violon"],["Sorcier","Vampire"],["Dentiste","Médecin"],["Prison","École"],["Soleil","Lune"],["Requin","Dauphin"],["Château","Cabane"],["Paris","Lyon"]];
const RANKS=[{name:"Neuille",min:0,color:"#888780"},{name:"Zig",min:10,color:"#378ADD"},{name:"Boss",min:25,color:"#639922"},{name:"Slayeur",min:50,color:"#EF9F27"},{name:"Tigre",min:100,color:"#E24B4A"}];

let state={players:[],roles:{},words:{},alive:[],clues:[],round:0,currentIdx:0,selectedVote:null};
let leaderboard=JSON.parse(localStorage.getItem('uc_lb')||'{}');
let revealIdx=0;

function getRank(pts){let r=RANKS[0];for(let rk of RANKS)if(pts>=rk.min)r=rk;return r;}

function addPlayer(){
  const inputs=document.querySelectorAll('#player-inputs input');
  const n=inputs.length;
  if(n>=5)return;
  const d=document.getElementById('player-inputs');
  const el=document.createElement('input');
  el.type='text';el.placeholder='Joueur '+(n+1);el.id='p'+n;
  d.appendChild(el);
  if(n+1>=5)document.getElementById('btn-add').style.display='none';
  document.getElementById('btn-rem').style.display='inline-block';
}

function removePlayer(){
  const inputs=document.querySelectorAll('#player-inputs input');
  if(inputs.length<=3)return;
  inputs[inputs.length-1].remove();
  if(inputs.length-1<5)document.getElementById('btn-add').style.display='inline-block';
  if(inputs.length-1<=3)document.getElementById('btn-rem').style.display='none';
}

function getPlayers(){
  const inputs=document.querySelectorAll('#player-inputs input');
  let players=[];
  inputs.forEach((inp,i)=>{const v=inp.value.trim()||('Joueur '+(i+1));players.push(v);});
  return players;
}

function startGame(){
  const players=getPlayers();
  if(players.length<3){alert('Il faut au moins 3 joueurs !');return;}
  state.players=[...players];state.alive=[...players];state.clues=[];state.round=0;state.currentIdx=0;
  assignRoles();revealIdx=0;
  showScreen('screen-reveal');showRevealForPlayer(0);
}

function assignRoles(){
  const n=state.players.length;
  const wPair=WORDS[Math.floor(Math.random()*WORDS.length)];
  const mainWord=wPair[0];const spyWord=wPair[1];
  state.roles={};state.words={};
  let spyIdx=Math.floor(Math.random()*n);
  let whiteIdx=-1;
  if(n===5){whiteIdx=Math.floor(Math.random()*n);while(whiteIdx===spyIdx)whiteIdx=Math.floor(Math.random()*n);}
  state.players.forEach((p,i)=>{
    if(i===spyIdx){state.roles[p]='infiltre';state.words[p]=spyWord;}
    else if(i===whiteIdx){state.roles[p]='mrwhite';state.words[p]='';}
    else{state.roles[p]='civil';state.words[p]=mainWord;}
  });
  state.mainWord=mainWord;state.spyWord=spyWord;
}

function showRevealForPlayer(idx){
  const p=state.players[idx];
  document.getElementById('reveal-player-name').textContent=p;
  document.getElementById('role-content').style.display='none';
  document.getElementById('btn-show-role').style.display='inline-block';
}

function showRole(){
  const p=state.players[revealIdx];
  const role=state.roles[p];const word=state.words[p];
  const badge=document.getElementById('role-type-badge');
  const wordEl=document.getElementById('role-word-display');
  const desc=document.getElementById('role-desc');
  if(role==='civil'){badge.textContent='Civil';badge.style.background='#EAF3DE';badge.style.color='#3B6D11';wordEl.textContent=word;desc.textContent='Donne des indices sans trahir ton mot !';}
  else if(role==='infiltre'){badge.textContent='Infiltré';badge.style.background='#FCEBEB';badge.style.color='#A32D2D';wordEl.textContent=word;desc.textContent='Tu as un mot différent. Fais-toi passer pour un civil !';}
  else{badge.textContent='Mr White';badge.style.background='#FAEEDA';badge.style.color='#854F0B';wordEl.textContent='???';desc.textContent='Tu n\'as pas de mot. Bluff et devine !';}
  document.getElementById('role-content').style.display='block';
  document.getElementById('btn-show-role').style.display='none';
}

function nextReveal(){
  revealIdx++;
  if(revealIdx>=state.players.length){startRound();return;}
  showRevealForPlayer(revealIdx);
}

function startRound(){
  state.round++;state.currentIdx=0;state.clues=[];
  showScreen('screen-game');updateGameUI();
}

function updateGameUI(){
  const alive=state.alive;
  const steps=document.getElementById('game-steps');
  steps.innerHTML=alive.map((p,i)=>`<span class="step${i===state.currentIdx?' active':''}">${p}</span>`).join('');
  document.getElementById('current-player-name').textContent=alive[state.currentIdx];
  document.getElementById('clue-input').value='';
  document.getElementById('hint').textContent='';
  const clueSec=document.getElementById('clue-list');
  if(state.clues.length>0){
    clueSec.style.display='block';
    document.getElementById('clues-given').innerHTML=state.clues.map(c=>`<div class="clue-item"><span class="clue-player">${c.player}</span><span class="clue-word">${c.clue}</span></div>`).join('');
  }else{clueSec.style.display='none';}
}

function submitClue(){
  const val=document.getElementById('clue-input').value.trim();
  if(!val){document.getElementById('hint').textContent='Entre un indice !';return;}
  const cur=state.alive[state.currentIdx];
  const curWord=state.words[cur]||'';
  if(val.toLowerCase()===curWord.toLowerCase()){document.getElementById('hint').textContent='Tu ne peux pas dire ton mot exact !';return;}
  state.clues.push({player:cur,clue:val});
  state.currentIdx++;
  if(state.currentIdx>=state.alive.length){showVote();return;}
  updateGameUI();
}

function showVote(){
  showScreen('screen-vote');state.selectedVote=null;
  document.getElementById('vote-buttons').innerHTML=state.alive.map(p=>`<button class="vote-btn" onclick="selectVote('${p}',this)">${p}</button>`).join('');
  document.getElementById('btn-confirm-vote').disabled=true;
}

function selectVote(name,el){
  state.selectedVote=name;
  document.querySelectorAll('.vote-btn').forEach(b=>b.classList.remove('selected'));
  el.classList.add('selected');
  document.getElementById('btn-confirm-vote').disabled=false;
}

function confirmVote(){
  if(!state.selectedVote)return;
  const elim=state.selectedVote;
  const role=state.roles[elim];
  state.alive=state.alive.filter(p=>p!==elim);
  const infiltre=state.players.find(p=>state.roles[p]==='infiltre');
  const mrwhite=state.players.find(p=>state.roles[p]==='mrwhite');
  const aliveInfiltre=state.alive.includes(infiltre);
  const aliveMrWhite=mrwhite&&state.alive.includes(mrwhite);
  const aliveCivils=state.alive.filter(p=>state.roles[p]==='civil');
  const aliveImpostors=state.alive.filter(p=>state.roles[p]==='infiltre'||state.roles[p]==='mrwhite');
  if(role==='infiltre'||role==='mrwhite'){
    if(!aliveInfiltre&&!aliveMrWhite){showResult(false,elim);return;}
  }
  if(state.alive.length<=2||aliveCivils.length<=aliveImpostors.length){showResult(true,elim);return;}
  startRound();
}

function showResult(infiltreWins,lastElim){
  showScreen('screen-result');
  const infiltre=state.players.find(p=>state.roles[p]==='infiltre');
  const title=document.getElementById('result-title');
  const sub=document.getElementById('result-sub');
  if(infiltreWins){title.textContent='L\'infiltré gagne !';title.style.color='#E24B4A';sub.textContent=`${infiltre} s'en est tiré !`;}
  else{title.textContent='Les civils gagnent !';title.style.color='#639922';sub.textContent='L\'infiltré a été découvert !';}
  document.getElementById('result-roles').innerHTML=state.players.map(p=>{
    const r=state.roles[p];const w=state.words[p]||'???';
    const color=r==='civil'?'#3B6D11':r==='infiltre'?'#A32D2D':'#854F0B';
    return `<div class="player-row"><span>${p}</span><span style="color:${color};font-size:13px">${r==='civil'?'Civil':r==='infiltre'?'Infiltré':'Mr White'} — ${w}</span></div>`;
  }).join('');
  document.getElementById('stat-rounds').textContent=state.round;
  document.getElementById('stat-elim').textContent=state.players.length-state.alive.length;
  updateLeaderboard(infiltreWins,infiltre);
}

function updateLeaderboard(infiltreWins,infiltre){
  state.players.forEach(p=>{
    if(!leaderboard[p])leaderboard[p]={pts:0,wins:0,games:0};
    leaderboard[p].games++;
    const role=state.roles[p];let pts=0;
    if(infiltreWins&&role==='infiltre'){pts=15;leaderboard[p].wins++;}
    else if(!infiltreWins&&role==='civil'){pts=10;leaderboard[p].wins++;}
    else if(!infiltreWins&&role==='mrwhite'){pts=5;}
    leaderboard[p].pts+=pts;
  });
  localStorage.setItem('uc_lb',JSON.stringify(leaderboard));
}

function showLeaderboard(){
  showScreen('screen-leaderboard');
  const entries=Object.entries(leaderboard).sort((a,b)=>b[1].pts-a[1].pts);
  const lb=document.getElementById('lb-list');
  if(entries.length===0){lb.innerHTML='<p style="color:var(--txt2);font-size:14px">Aucune partie jouée.</p>';return;}
  lb.innerHTML=entries.map(([name,data],i)=>{
    const rk=getRank(data.pts);
    return `<div class="leaderboard-row"><span class="lb-rank">${i+1}</span><span class="lb-name">${name}<br><span style="font-size:11px;color:${rk.color};font-family:'Bebas Neue',sans-serif">${rk.name}</span></span><span class="lb-pts">${data.pts} pts<br><span style="font-size:11px;color:var(--txt2)">${data.wins}W / ${data.games}G</span></span></div>`;
  }).join('');
}

function newGame(){startGame();}
function goHome(){showScreen('screen-home');}
function showScreen(id){document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));document.getElementById(id).classList.add('active');}
</script>
