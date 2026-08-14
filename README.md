# date-Invitation
Nicole, Would you like to go on date?

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Cozy Date with Jay 💖</title>
  <style>
    :root{
      --bg:#ffeef2;
      --card:#fff;
      --accent:#ff5c8a;
      --muted:#8b6774;
      --shadow: 0 8px 30px rgba(0,0,0,0.08);
      font-family: "Segoe UI", Roboto, system-ui, -apple-system, "Helvetica Neue", Arial;
    }
    html,body{height:100%}
    body{
      margin:0;
      background: linear-gradient(180deg,var(--bg),#fff);
      display:flex;
      align-items:center;
      justify-content:center;
      padding:24px;
    }
    .card{
      width:100%;
      max-width:560px;
      background:var(--card);
      border-radius:16px;
      box-shadow:var(--shadow);
      padding:28px;
      text-align:center;
      position:relative;
      overflow:hidden;
    }
    h1{margin:0 0 12px; font-size:22px; color:var(--accent)}
    p.lead{margin:0 0 18px; color:var(--muted)}
    .btn-row{
      display:flex;
      gap:12px;
      justify-content:center;
      flex-wrap:wrap;
      margin-top:14px;
    }
    .choice-btn{
      background:linear-gradient(180deg,#fff,#ffe6ee);
      border:2px solid #ffd6e7;
      color:var(--accent);
      padding:12px 18px;
      border-radius:999px;
      font-weight:600;
      cursor:pointer;
      min-width:120px;
      transition:transform .18s ease, box-shadow .18s ease;
      position:relative;
    }
    .choice-btn:active{transform:scale(.98)}
    .muted{
      color:#6b5860;
      font-size:14px;
      margin-top:16px;
    }
    .hidden{display:none}
    .message{
      font-size:18px;
      color:#542233;
      margin-top:18px;
    }
    /* final hearts */
    .hearts{
      pointer-events:none;
      position:absolute;
      inset:0;
      overflow:hidden;
    }
    .heart{
      position:absolute;
      width:18px;height:18px;
      transform:translateY(100vh) scale(0.6);
      opacity:0.95;
      animation:rise 3s linear infinite;
      background:radial-gradient(circle at 30% 30%, #fff 0 8%, transparent 9%),
                 linear-gradient(135deg,#ff5c8a 0%, #ff8fb8 100%);
      clip-path: path("M10 3c-3-1.5-7 1-7 4 0 6 6.5 8.5 10 15 3.5-6.5 10-9 10-15 0-3-4-5.5-7-4-1.5.6-2.5 1.8-3 2.3-.5-.5-1.5-1.7-3-2.3z");
    }
    @keyframes rise{
      0%{transform:translateY(110%) scale(.6); opacity:0}
      10%{opacity:1}
      70%{opacity:0.9}
      100%{transform:translateY(-30%) scale(1); opacity:0}
    }

    /* small tooltip */
    .hint{
      font-size:13px;
      color:#8b6774;
      margin-top:10px;
    }

    /* responsive tweaks */
    @media (max-width:420px){
      .choice-btn{min-width:100px;padding:10px 14px}
    }
  </style>
</head>
<body>
  <main class="card" role="main" aria-labelledby="title">
    <h1 id="title">Hey, would you like a cozy movie date with Jay? 💫</h1>
    <p class="lead">Pick a movie vibe — Romantic or Horror</p>

    <div id="step-movie" class="step">
      <div class="btn-row" id="movieRow">
        <button id="romanticBtn" class="choice-btn">Romantic</button>
        <button id="horrorBtn" class="choice-btn">Horror</button>
      </div>
      <div class="hint" id="movieHint">Choose what you feel like watching.</div>
    </div>

    <div id="step-romantic" class="step hidden" aria-hidden="true">
      <p class="lead">A romantic movie it is — would you say yes to the date?</p>
      <div class="btn-row">
        <button id="yesBtn" class="choice-btn">Yes</button>
        <button id="noBtn" class="choice-btn">No</button>
      </div>
      <div class="hint">Only the sweetest answer is allowed here 💖</div>
    </div>

    <div id="step-date" class="step hidden" aria-hidden="true">
      <p class="lead">Pick a date for us</p>
      <div class="btn-row">
        <button id="dateFixedBtn" class="choice-btn">2026-08-22</button>
        <button id="dateOtherBtn" class="choice-btn">Other</button>
      </div>
      <div class="hint">If you choose the special date, I'll celebrate with a smile 😌</div>
    </div>

    <div id="final" class="hidden" aria-hidden="true">
      <p class="message" id="finalMessage"></p>
    </div>

    <div class="hearts" id="hearts"></div>
  </main>

  <script>
    // Core elements
    const romanticBtn = document.getElementById('romanticBtn');
    const horrorBtn = document.getElementById('horrorBtn');
    const stepMovie = document.getElementById('step-movie');
    const stepRomantic = document.getElementById('step-romantic');
    const stepDate = document.getElementById('step-date');
    const final = document.getElementById('final');
    const finalMessage = document.getElementById('finalMessage');
    const yesBtn = document.getElementById('yesBtn');
    const noBtn = document.getElementById('noBtn');
    const dateFixedBtn = document.getElementById('dateFixedBtn');
    const dateOtherBtn = document.getElementById('dateOtherBtn');
    const movieRow = document.getElementById('movieRow');
    const hearts = document.getElementById('hearts');

    // Utility to move a button to a random position inside its parent container
    function moveAway(btn){
      const parent = btn.parentElement;
      // make parent relative if not
      const rect = parent.getBoundingClientRect();
      const btnRect = btn.getBoundingClientRect();

      // compute allowed ranges
      const maxX = Math.max(4, rect.width - btnRect.width - 8);
      const maxY = Math.max(4, rect.height - btnRect.height - 8);

      // random position inside parent (convert to percent)
      const x = Math.floor(Math.random() * maxX);
      const y = Math.floor(Math.random() * maxY);

      // convert to transform for smooth movement
      btn.style.transition = "transform 0.28s cubic-bezier(.22,.9,.3,1)";
      btn.style.transform = `translate(${x - (btnRect.left - rect.left)}px, ${y - (btnRect.top - rect.top)}px)`;
      // after animation, reset transform and actually reorder to keep layout stable
      setTimeout(()=>{
        btn.style.transition = "";
        btn.style.transform = "";
        // move node to end or random index to make it "slip away" visually
        if(Math.random() > 0.5 && parent.children.length > 1){
          parent.appendChild(btn);
        } else {
          parent.insertBefore(btn, parent.children[0]);
        }
      }, 320);
    }

    // For mobile touch friendliness: apply move on touchstart and mouseenter
    function makeElvasive(btn){
      let moved = false;
      const startMove = (ev) => {
        // small guard: sometimes clicks should be allowed (we'll prevent default selection in handlers)
        moved = true;
        moveAway(btn);
      };
      btn.addEventListener('mouseenter', startMove);
      btn.addEventListener('touchstart', (e)=>{
        startMove(e);
        // avoid triggering the click
        e.preventDefault();
      }, {passive:false});
      // also on click, ensure it moves and does not trigger selection
      btn.addEventListener('click', (e) => {
        if(moved){
          e.preventDefault();
          e.stopPropagation();
          // playful message
          flashHint(btn, "Oops, that slipped away!");
        } else {
          // fallback: still move
          e.preventDefault();
          moveAway(btn);
          flashHint(btn, "Try the other one 😊");
        }
      });
    }

    function flashHint(btn, text){
      const hint = document.createElement('div');
      hint.textContent = text;
      hint.style.position = 'absolute';
      hint.style.padding = '6px 10px';
      hint.style.background = 'rgba(255,92,138,0.12)';
      hint.style.color = '#7f394d';
      hint.style.borderRadius = '8px';
      hint.style.fontSize = '13px';
      hint.style.top = (btn.offsetTop - 36) + 'px';
      hint.style.left = (btn.offsetLeft) + 'px';
      hint.style.pointerEvents = 'none';
      hint.style.transition = 'opacity .6s';
      hint.style.opacity = '0';
      btn.parentElement.appendChild(hint);
      requestAnimationFrame(()=> hint.style.opacity = '1');
      setTimeout(()=> {
        hint.style.opacity = '0';
        setTimeout(()=> hint.remove(), 600);
      }, 900);
    }

    // make Horror evasive
    makeElusive: void 0;
    makeElusive;
    makeElusive = undefined;
    // attach evasive behavior
    makeElusive = (btn) => makeElusive; // noop guard to avoid lint noise
    // call the real function
    makeElusive = null;

    // Attach evasive behavior for horror
    (function attachEvasive(){
      // We'll use the moveAway behavior for horror on hover/touch/click
      horrorBtn.addEventListener('mouseenter', ()=> moveAway(horrorBtn));
      horrorBtn.addEventListener('touchstart', (e)=>{ moveAway(horrorBtn); e.preventDefault(); }, {passive:false});
      horrorBtn.addEventListener('click', (e)=>{
        e.preventDefault();
        moveAway(horrorBtn);
        flashHint(horrorBtn, "Horror slid away... Let's keep it cozy 💕");
      });
    })();

    // Romantic selection -> show yes/no
    romanticBtn.addEventListener('click', ()=>{
      stepMovie.classList.add('hidden');
      stepRomantic.classList.remove('hidden');
      stepRomantic.setAttribute('aria-hidden','false');
    });

    // No button: dodge on hover/touch and block click
    (function makeNoDodge(){
      let dodgeCount = 0;
      function dodge(ev){
        dodgeCount++;
        moveAway(noBtn);
        // small playful hint
        if(dodgeCount === 1) flashHint(noBtn, "No is shy — it won't pick itself 😊");
        ev.preventDefault();
        ev.stopPropagation();
      }
      noBtn.addEventListener('mouseenter', dodge);
      noBtn.addEventListener('touchstart', (e)=>{ dodge(e); }, {passive:false});
      noBtn.addEventListener('click', (e)=>{
        dodge(e);
      });
    })();

    // Yes selects -> proceed to date
    yesBtn.addEventListener('click', ()=>{
      stepRomantic.classList.add('hidden');
      stepDate.classList.remove('hidden');
      stepDate.setAttribute('aria-hidden','false');
    });

    // Other date: dodge and block
    (function makeOtherDodge(){
      dateOtherBtn.addEventListener('mouseenter', ()=> moveAway(dateOtherBtn));
      dateOtherBtn.addEventListener('touchstart', (e)=>{ moveAway(dateOtherBtn); e.preventDefault(); }, {passive:false});
      dateOtherBtn.addEventListener('click', (e)=>{
        e.preventDefault();
        moveAway(dateOtherBtn);
        flashHint(dateOtherBtn, "This one's not available — maybe the 22nd?");
      });
    })();

    // Fixed date -> show final message
    dateFixedBtn.addEventListener('click', ()=>{
      stepDate.classList.add('hidden');
      final.classList.remove('hidden');
      final.setAttribute('aria-hidden','false');
      finalMessage.textContent = "Your romantic date is booked with Jay — you should smile now 🙂";
      startHearts();
    });

    // Small decorative hearts animation generator
    function startHearts(){
      // create ~12 hearts with randomized positions and animation delays
      for(let i=0;i<12;i++){
        const h = document.createElement('div');
        h.className = 'heart';
        h.style.left = (Math.random()*100) + '%';
        h.style.width = (10 + Math.random()*30) + 'px';
        h.style.height = h.style.width;
        h.style.animationDuration = (2.2 + Math.random()*1.6) + 's';
        h.style.animationDelay = (Math.random()*0.8) + 's';
        hearts.appendChild(h);
        // remove after some time so DOM doesn't grow forever
        setTimeout(()=> h.remove(), 7000 + Math.random()*2000);
      }
      // keep sprinkling a few more for a short while
      let rounds = 0;
      const interval = setInterval(()=>{
        for(let i=0;i<4;i++){
          const h = document.createElement('div');
          h.className = 'heart';
          h.style.left = (Math.random()*100) + '%';
          h.style.width = (10 + Math.random()*28) + 'px';
          h.style.height = h.style.width;
          h.style.animationDuration = (2.2 + Math.random()*1.6) + 's';
          h.style.animationDelay = '0s';
          hearts.appendChild(h);
          setTimeout(()=> h.remove(), 7000);
        }
        rounds++;
        if(rounds>6) clearInterval(interval);
      }, 900);
    }

    // Small accessibility improvement: allow keyboard selection for allowed buttons
    // (Horror / No / Other are intentionally prevented above.)
    romanticBtn.addEventListener('keydown', (e)=> { if(e.key==='Enter') romanticBtn.click(); });
    yesBtn.addEventListener('keydown', (e)=> { if(e.key==='Enter') yesBtn.click(); });
    dateFixedBtn.addEventListener('keydown', (e)=> { if(e.key==='Enter') dateFixedBtn.click(); });

    // Optional: allow customizing text quickly (you can edit these strings)
    // const customIntro = "Hey, would you like a cozy movie date with Jay?";
    // document.getElementById('title').textContent = customIntro;

  </script>
</body>
</html>
