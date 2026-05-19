# Data-Analytics-Effort-Estimator
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dashboard Estimator — Peterson Solutions</title>
<link href="https://fonts.googleapis.com/css2?family=Ubuntu:wght@300;400;500;700&display=swap" rel="stylesheet">
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
:root {
  --navy: #1b1e42; --navy-mid: #252852; --aqua: #44cbce; --yellow: #f1e747;
  --dark-grey: #4f6566; --light-grey: #799495; --bg: #f2f6f6; --white: #ffffff;
  --font: 'Ubuntu','Calibri',Arial,sans-serif;
}
body { font-family: var(--font); background: var(--bg); color: var(--dark-grey); min-height: 100vh; }

header {
  background: var(--navy); padding: 32px 40px 40px;
  position: relative; overflow: hidden;
}
header::before {
  content:''; position:absolute; width:1100px; height:600px;
  background:var(--navy-mid); border-radius:50%;
  top:-420px; right:-300px; transform:rotate(-25deg); opacity:.7;
}
header::after {
  content:''; position:absolute; width:800px; height:400px;
  background:var(--navy-mid); border-radius:50%;
  top:40px; right:-100px; transform:rotate(-40deg); opacity:.5;
}
.hdr { position:relative; z-index:2; max-width:780px; margin:0 auto; }
.logo { display:flex; align-items:center; gap:10px; margin-bottom:32px; }
.logo-name { font-size:18px; font-weight:700; letter-spacing:.08em; color:var(--white); text-transform:uppercase; }
.logo-name span { font-weight:300; color:rgba(255,255,255,.6); }
header h1 { font-size:13px; font-weight:700; color:var(--aqua); letter-spacing:.1em; text-transform:uppercase; margin-bottom:10px; }
header h2 { font-size:38px; font-weight:300; color:var(--white); line-height:1.2; }
header h2 strong { font-weight:700; }
header p { color:rgba(255,255,255,.5); font-size:14px; margin-top:10px; }

.progress-wrap { background:var(--navy-mid); padding:0 40px; }
.progress-inner { max-width:780px; margin:0 auto; display:flex; }
.prog-step {
  flex:1; padding:13px 0; font-size:12px; font-weight:500;
  color:rgba(255,255,255,.3); text-align:center; position:relative;
  letter-spacing:.02em; transition:color .2s;
}
.prog-step.done { color:rgba(255,255,255,.5); }
.prog-step.active { color:var(--aqua); }
.prog-step.active::after {
  content:''; position:absolute; bottom:0; left:0; right:0;
  height:3px; background:var(--aqua); border-radius:2px 2px 0 0;
}

main { max-width:780px; margin:0 auto; padding:32px 40px 80px; }

.step { display:none; }
.step.active { display:block; }

.step-intro { margin-bottom:24px; }
.step-intro h3 { font-size:26px; font-weight:700; color:var(--navy); margin-bottom:8px; }
.step-intro p { font-size:15px; color:var(--light-grey); line-height:1.6; }

.choices { display:grid; gap:12px; }
.choices.cols-2 { grid-template-columns:1fr 1fr; }
.choices.cols-3 { grid-template-columns:1fr 1fr 1fr; }

.choice {
  background:var(--white); border:2px solid transparent;
  border-radius:14px; padding:20px 20px 18px; cursor:pointer;
  transition:border-color .2s, box-shadow .2s; position:relative;
}
.choice:hover { border-color:var(--aqua); box-shadow:0 4px 20px rgba(68,203,206,.15); }
.choice.selected { border-color:var(--aqua); background:#f0fcfc; }
.choice.selected::after {
  content:'✓'; position:absolute; top:14px; right:16px;
  width:24px; height:24px; border-radius:50%;
  background:var(--aqua); color:var(--navy);
  font-size:12px; font-weight:700;
  display:flex; align-items:center; justify-content:center;
}
.choice.warn { }
.choice.warn.selected { background:#fffdf0; border-color:var(--yellow); }
.choice.warn.selected::after { background:var(--yellow); }
.choice-icon { font-size:28px; margin-bottom:10px; line-height:1; }
.choice-title { font-size:15px; font-weight:700; color:var(--navy); margin-bottom:4px; }
.choice-desc { font-size:13px; color:var(--light-grey); line-height:1.5; }

.two-inputs { display:grid; grid-template-columns:1fr 1fr; gap:12px; margin-bottom:24px; }
.input-card { background:var(--white); border-radius:14px; padding:20px; border:2px solid transparent; }
.input-card label { font-size:14px; font-weight:700; color:var(--navy); display:block; margin-bottom:12px; }
.input-card small { display:block; font-size:12px; color:var(--light-grey); font-weight:400; margin-top:2px; }
.num-row { display:flex; align-items:center; gap:10px; }
.num-row input {
  width:80px; padding:8px 10px; font-family:var(--font); font-size:22px;
  font-weight:700; text-align:center; color:var(--navy);
  border:2px solid #dde8e8; border-radius:8px; outline:none;
}
.num-row input:focus { border-color:var(--aqua); }
.num-row span { font-size:13px; color:var(--light-grey); }

.spacer { margin-top:28px; }

.nav { display:flex; justify-content:space-between; align-items:center; margin-top:32px; }
.btn-back {
  background:none; border:2px solid #dde8e8; color:var(--dark-grey);
  font-family:var(--font); font-size:14px; font-weight:500;
  padding:12px 24px; border-radius:10px; cursor:pointer;
}
.btn-back:hover { border-color:var(--light-grey); }
.btn-next {
  background:var(--aqua); border:none; color:var(--navy);
  font-family:var(--font); font-size:15px; font-weight:700;
  padding:14px 32px; border-radius:10px; cursor:pointer;
  transition:background .2s, transform .1s;
}
.btn-next:hover { background:#2fa8ab; }
.btn-next:active { transform:scale(.98); }
.btn-next:disabled { background:#dde8e8; color:var(--light-grey); cursor:default; }
.step-count { font-size:13px; color:var(--light-grey); }

/* RESULT */
.result-hero {
  background:var(--navy); border-radius:16px; padding:36px 32px 32px;
  margin-bottom:16px; position:relative; overflow:hidden; text-align:center;
}
.result-hero::before {
  content:''; position:absolute; width:600px; height:300px;
  background:var(--navy-mid); border-radius:50%;
  top:-200px; right:-100px; transform:rotate(-30deg); opacity:.7;
}
.rh { position:relative; z-index:2; }
.result-label { font-size:12px; font-weight:700; letter-spacing:.1em; text-transform:uppercase; color:var(--aqua); margin-bottom:8px; }
.result-days { font-size:76px; font-weight:700; color:var(--white); line-height:1; }
.result-days span { font-size:26px; font-weight:300; color:rgba(255,255,255,.5); margin-left:4px; }
.result-name { font-size:18px; font-weight:500; margin-top:8px; }
.result-summary { font-size:14px; color:rgba(255,255,255,.5); margin-top:10px; line-height:1.6; max-width:500px; margin-left:auto; margin-right:auto; }

.rcards { display:grid; grid-template-columns:1fr 1fr; gap:12px; margin-bottom:16px; }
.rcard { background:var(--white); border-radius:12px; padding:18px 20px; border:1px solid rgba(68,203,206,.12); }
.rcard-label { font-size:11px; font-weight:700; text-transform:uppercase; letter-spacing:.07em; color:var(--light-grey); margin-bottom:6px; }
.rcard-value { font-size:17px; font-weight:700; color:var(--navy); }
.rcard-sub { font-size:12px; color:var(--light-grey); margin-top:3px; }

.what-next { background:var(--white); border-radius:12px; padding:22px 24px; border-left:4px solid var(--aqua); margin-bottom:16px; }
.what-next h4 { font-size:14px; font-weight:700; color:var(--navy); margin-bottom:12px; }
.what-next ol { padding-left:18px; }
.what-next li { font-size:14px; color:var(--dark-grey); margin-bottom:8px; line-height:1.6; }

.risk-box { background:#fffdf0; border-radius:12px; padding:16px 20px; border-left:4px solid var(--yellow); margin-bottom:16px; }
.risk-box h4 { font-size:13px; font-weight:700; color:#7a6200; margin-bottom:6px; }
.risk-box p { font-size:13px; color:#8a7200; line-height:1.5; }

.note-card { background:#f0fcfc; border-radius:12px; padding:18px 22px; border:1px solid rgba(68,203,206,.25); margin-bottom:16px; font-size:14px; color:var(--dark-grey); line-height:1.6; }
.note-card strong { color:var(--navy); }

.price-hero {
  background: var(--navy); border-radius:16px; padding:28px 32px;
  margin-bottom:16px; display:flex; align-items:center; justify-content:space-between;
  gap:20px; flex-wrap:wrap;
}
.price-left {}
.price-tag-label { font-size:11px; font-weight:700; letter-spacing:.1em; text-transform:uppercase; color:rgba(255,255,255,.45); margin-bottom:6px; }
.price-range { font-size:42px; font-weight:700; color:var(--yellow); line-height:1; }
.price-range span { font-size:18px; font-weight:400; color:rgba(255,255,255,.5); margin-left:4px; }
.price-note { font-size:13px; color:rgba(255,255,255,.4); margin-top:6px; }
.price-right { text-align:right; }
.price-per-day { font-size:13px; color:rgba(255,255,255,.4); margin-bottom:4px; }
.price-rate { font-size:22px; font-weight:700; color:var(--aqua); }
.price-days-shown { font-size:13px; color:rgba(255,255,255,.35); margin-top:4px; }

.restart-btn {
  display:block; width:100%; background:none;
  border:2px solid var(--aqua); color:var(--aqua);
  font-family:var(--font); font-size:14px; font-weight:700;
  padding:14px; border-radius:10px; cursor:pointer; margin-top:8px;
  transition:background .2s, color .2s;
}
.restart-btn:hover { background:var(--aqua); color:var(--navy); }

footer { background:var(--navy); text-align:center; padding:24px 40px; font-size:12px; color:rgba(255,255,255,.35); }
footer strong { color:var(--aqua); }

@media (max-width:560px) {
  header { padding:24px 20px 28px; }
  .progress-wrap { padding:0 20px; }
  main { padding:24px 20px 60px; }
  .choices.cols-2, .choices.cols-3 { grid-template-columns:1fr; }
  .two-inputs { grid-template-columns:1fr; }
  .rcards { grid-template-columns:1fr; }
  header h2 { font-size:28px; }
  .result-days { font-size:56px; }
  .prog-step { font-size:10px; padding:11px 4px; }
}
</style>
</head>
<body>

<header>
  <div class="hdr">
    <div class="logo">
      <svg width="36" height="36" viewBox="0 0 40 40" fill="none">
        <circle cx="20" cy="20" r="18" stroke="white" stroke-width="1.5" opacity=".3"/>
        <path d="M4 20 Q12 8 20 12 Q28 16 36 20" stroke="white" stroke-width="2.5" fill="none" stroke-linecap="round"/>
        <path d="M4 20 Q12 28 20 26 Q28 24 36 20" stroke="white" stroke-width="2.5" fill="none" stroke-linecap="round"/>
      </svg>
      <div class="logo-name">PETERSON <span>SOLUTIONS</span></div>
    </div>
    <h1>Data &amp; Analytics</h1>
    <h2>How long will my<br><strong>dashboard take?</strong></h2>
    <p>Answer 5 plain-English questions and get an honest estimate in under 2 minutes.</p>
  </div>
</header>

<div class="progress-wrap">
  <div class="progress-inner">
    <div class="prog-step active" id="prog-0">What you need</div>
    <div class="prog-step" id="prog-1">Your data</div>
    <div class="prog-step" id="prog-2">The screens</div>
    <div class="prog-step" id="prog-3">Sign-off</div>
    <div class="prog-step" id="prog-4">Your estimate</div>
  </div>
</div>

<main>

  <!-- STEP 0 -->
  <div class="step active" id="step-0">
    <div class="step-intro">
      <h3>What do you need built?</h3>
      <p>Pick the option that best describes your project.</p>
    </div>
    <div class="choices cols-2">
      <div class="choice" onclick="pick('type','new')" id="c-type-new">
        <div class="choice-icon">📊</div>
        <div class="choice-title">A brand new dashboard</div>
        <div class="choice-desc">Nothing exists yet — we start from scratch.</div>
      </div>
      <div class="choice" onclick="pick('type','improve')" id="c-type-improve">
        <div class="choice-icon">✏️</div>
        <div class="choice-title">Improve or extend an existing one</div>
        <div class="choice-desc">Something already works but needs changes or new screens.</div>
      </div>
      <div class="choice" onclick="pick('type','automate')" id="c-type-automate">
        <div class="choice-icon">⚙️</div>
        <div class="choice-title">Automate a manual process</div>
        <div class="choice-desc">A repetitive task that currently needs manual steps.</div>
      </div>
      <div class="choice" onclick="pick('type','both')" id="c-type-both">
        <div class="choice-icon">🔗</div>
        <div class="choice-title">Dashboard + automation together</div>
        <div class="choice-desc">Visuals on screen and automated workflows behind the scenes.</div>
      </div>
    </div>
    <div class="nav">
      <span class="step-count">Question 1 of 4</span>
      <button class="btn-next" id="next-0" onclick="goNext(0)" disabled>Next →</button>
    </div>
  </div>

  <!-- STEP 1 -->
  <div class="step" id="step-1">
    <div class="step-intro">
      <h3>Where does your data live?</h3>
      <p>Think about the systems, files or databases that hold the numbers you want to show.</p>
    </div>
    <div class="choices cols-3">
      <div class="choice" onclick="pick('sources','one')" id="c-sources-one">
        <div class="choice-icon">🗄️</div>
        <div class="choice-title">One place</div>
        <div class="choice-desc">One file, one database, or one system — already in one spot.</div>
      </div>
      <div class="choice" onclick="pick('sources','few')" id="c-sources-few">
        <div class="choice-icon">🔀</div>
        <div class="choice-title">A few different places</div>
        <div class="choice-desc">2–4 sources that need to be combined — e.g. Excel + ERP + CRM.</div>
      </div>
      <div class="choice" onclick="pick('sources','many')" id="c-sources-many">
        <div class="choice-icon">🌐</div>
        <div class="choice-title">Many places</div>
        <div class="choice-desc">5 or more systems, feeds, or databases to pull together.</div>
      </div>
    </div>

    <div class="spacer">
      <div class="step-intro" style="margin-bottom:16px">
        <h3 style="font-size:20px">Is your data clean and consistent?</h3>
        <p>Be honest — messy data is the single biggest cause of delays.</p>
      </div>
      <div class="choices cols-3">
        <div class="choice" onclick="pick('quality','clean')" id="c-quality-clean">
          <div class="choice-icon">✅</div>
          <div class="choice-title">Yes, it's clean</div>
          <div class="choice-desc">Consistent formats, no duplicates, structured and ready to use.</div>
        </div>
        <div class="choice warn" onclick="pick('quality','some')" id="c-quality-some">
          <div class="choice-icon">🧹</div>
          <div class="choice-title">Needs some tidying</div>
          <div class="choice-desc">A few inconsistencies or manual corrections required.</div>
        </div>
        <div class="choice warn" onclick="pick('quality','messy')" id="c-quality-messy">
          <div class="choice-icon">⚠️</div>
          <div class="choice-title">It's complicated</div>
          <div class="choice-desc">Different formats, missing values, or no agreed source of truth.</div>
        </div>
      </div>
    </div>

    <div class="nav">
      <button class="btn-back" onclick="goBack(1)">← Back</button>
      <span class="step-count">Question 2 of 4</span>
      <button class="btn-next" id="next-1" onclick="goNext(1)" disabled>Next →</button>
    </div>
  </div>

  <!-- STEP 2 -->
  <div class="step" id="step-2">
    <div class="step-intro">
      <h3>How big is the dashboard?</h3>
      <p>A <strong style="color:var(--navy)">screen</strong> is one page or tab. Be as specific as you can — these numbers directly affect the estimate.</p>
    </div>

    <div class="two-inputs" style="grid-template-columns:1fr 1fr 1fr;">
      <div class="input-card">
        <label>Screens or pages<small>Each tab or page counts as one</small></label>
        <div class="num-row">
          <input type="number" id="inp-screens" value="5" min="1" max="50">
          <span>screens</span>
        </div>
      </div>
      <div class="input-card">
        <label>Charts, tables &amp; numbers<small>Cards, bar charts, line charts, tables, KPI tiles</small></label>
        <div class="num-row">
          <input type="number" id="inp-kpis" value="20" min="0" max="200">
          <span>charts</span>
        </div>
      </div>
      <div class="input-card">
        <label>Filters &amp; slicers<small>Dropdowns, date pickers, toggle buttons</small></label>
        <div class="num-row">
          <input type="number" id="inp-filters" value="5" min="0" max="100">
          <span>filters</span>
        </div>
      </div>
    </div>

    <div class="step-intro" style="margin-bottom:16px">
      <h3 style="font-size:20px">How complex are the numbers?</h3>
      <p>Not the data itself — just how the numbers get calculated.</p>
    </div>
    <div class="choices cols-3">
      <div class="choice" onclick="pick('calc','simple')" id="c-calc-simple">
        <div class="choice-icon">➕</div>
        <div class="choice-title">Simple totals</div>
        <div class="choice-desc">Sums, counts, averages. What you'd do in a basic spreadsheet.</div>
      </div>
      <div class="choice" onclick="pick('calc','medium')" id="c-calc-medium">
        <div class="choice-icon">📅</div>
        <div class="choice-title">Year-to-date, comparisons</div>
        <div class="choice-desc">Comparing this month to last year, running totals, percentages.</div>
      </div>
      <div class="choice warn" onclick="pick('calc','complex')" id="c-calc-complex">
        <div class="choice-icon">🔢</div>
        <div class="choice-title">Custom business logic</div>
        <div class="choice-desc">Forecasts, weighted scores, or complex rules specific to your business.</div>
      </div>
    </div>

    <div class="nav">
      <button class="btn-back" onclick="goBack(2)">← Back</button>
      <span class="step-count">Question 3 of 4</span>
      <button class="btn-next" id="next-2" onclick="goNext(2)" disabled>Next →</button>
    </div>
  </div>

  <!-- STEP 3 -->
  <div class="step" id="step-3">
    <div class="step-intro">
      <h3>Who can see which data?</h3>
      <p>Sometimes different people should only see data relevant to them — a country manager sees their country, a global director sees everything.</p>
    </div>
    <div class="choices cols-3">
      <div class="choice" onclick="pick('access','open')" id="c-access-open">
        <div class="choice-icon">👥</div>
        <div class="choice-title">Everyone sees the same</div>
        <div class="choice-desc">No restrictions — all users see all data.</div>
      </div>
      <div class="choice" onclick="pick('access','some')" id="c-access-some">
        <div class="choice-icon">🔒</div>
        <div class="choice-title">Some people see less</div>
        <div class="choice-desc">A handful of defined roles with different data views.</div>
      </div>
      <div class="choice warn" onclick="pick('access','complex')" id="c-access-complex">
        <div class="choice-icon">🏢</div>
        <div class="choice-title">Complex rules</div>
        <div class="choice-desc">Many roles, regions, or hierarchies controlling what each person sees.</div>
      </div>
    </div>

    <div class="spacer">
      <div class="step-intro" style="margin-bottom:16px">
        <h3 style="font-size:20px">How will you approve the final result?</h3>
        <p>This affects how much review and testing time we need to budget.</p>
      </div>
      <div class="choices cols-3">
        <div class="choice" onclick="pick('signoff','self')" id="c-signoff-self">
          <div class="choice-icon">👤</div>
          <div class="choice-title">Just me</div>
          <div class="choice-desc">I'll look at it, give feedback, and say when it's done.</div>
        </div>
        <div class="choice" onclick="pick('signoff','team')" id="c-signoff-team">
          <div class="choice-icon">🤝</div>
          <div class="choice-title">Me and a few colleagues</div>
          <div class="choice-desc">A small group reviews together before we sign off.</div>
        </div>
        <div class="choice warn" onclick="pick('signoff','formal')" id="c-signoff-formal">
          <div class="choice-icon">📋</div>
          <div class="choice-title">Formal approval process</div>
          <div class="choice-desc">Multiple stakeholders, review rounds, and structured acceptance testing.</div>
        </div>
      </div>
    </div>

    <div class="nav">
      <button class="btn-back" onclick="goBack(3)">← Back</button>
      <span class="step-count">Question 4 of 4</span>
      <button class="btn-next" id="next-3" onclick="goNext(3)" disabled>See my estimate →</button>
    </div>
  </div>

  <!-- STEP 4: RESULT -->
  <div class="step" id="step-4">
    <div id="result-area"></div>
    <button class="restart-btn" onclick="restart()">← Start a new estimate</button>
  </div>

</main>

<footer>
  <strong>PETERSON SOLUTIONS</strong> &nbsp;·&nbsp; Dashboard Estimator &nbsp;·&nbsp; For the world, for ourselves, for our families
</footer>

<script>
const A = {};

function pick(key, val) {
  A[key] = val;
  document.querySelectorAll('[id^="c-' + key + '-"]').forEach(el => el.classList.remove('selected'));
  const el = document.getElementById('c-' + key + '-' + val);
  if (el) el.classList.add('selected');
  refreshButtons();
}

function refreshButtons() {
  const map = {
    0: () => !!A.type,
    1: () => !!A.sources && !!A.quality,
    2: () => !!A.calc,
    3: () => !!A.access && !!A.signoff
  };
  for (let i = 0; i <= 3; i++) {
    const btn = document.getElementById('next-' + i);
    if (btn) btn.disabled = !(map[i] && map[i]());
  }
}

function goNext(step) { setStep(step + 1); }
function goBack(step) { setStep(step - 1); }

function setStep(n) {
  document.querySelectorAll('.step').forEach((s, i) => s.classList.toggle('active', i === n));
  document.querySelectorAll('.prog-step').forEach((p, i) => {
    p.classList.remove('active', 'done');
    if (i < n) p.classList.add('done');
    if (i === n) p.classList.add('active');
  });
  if (n === 4) buildResult();
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function calcScore() {
  const screens  = Math.max(1, parseInt(document.getElementById('inp-screens').value) || 5);
  const charts   = Math.max(0, parseInt(document.getElementById('inp-kpis').value) || 20);
  const filters  = Math.max(0, parseInt(document.getElementById('inp-filters').value) || 5);
  const kpis     = charts + filters;
  let s = 0;
  if (A.type === 'new')      s += 10;
  if (A.type === 'improve')  s += 0;
  if (A.type === 'automate') s += 14;
  if (A.type === 'both')     s += 24;
  if (A.sources === 'few')   s += 10;
  if (A.sources === 'many')  s += 22;
  if (A.quality === 'some')  s += 10;
  if (A.quality === 'messy') s += 22;
  const m = A.calc === 'simple' ? 1.5 : A.calc === 'medium' ? 2.5 : 4;
  s += Math.round(charts * m) + Math.round(filters * 0.5) + screens * 5;
  if (A.access === 'some')    s += 6;
  if (A.access === 'complex') s += 16;
  if (A.signoff === 'team')   s += 6;
  if (A.signoff === 'formal') s += 14;
  return { s, screens, charts, filters, kpis };
}

const outcomes = [
  {
    max: 45,
    days: '3 – 5',
    name: 'Quick turnaround',
    colour: '#44cbce',
    summary: 'Straightforward and well-scoped. With clean data and a clear brief, we can move fast and deliver quickly.',
    steps: ['Kick-off call to confirm requirements and data access','Development sprint','Your review and feedback','Final delivery and sign-off'],
    risk: null
  },
  {
    max: 90,
    days: '8 – 12',
    name: 'Standard project',
    colour: '#f1e747',
    summary: 'A solid, well-defined project. We\'ll need a clear brief up front and one or two rounds of feedback along the way.',
    steps: ['Discovery session to agree scope, data and design','Data preparation and model build','Dashboard development with a mid-point check-in','Your review, refinements, and sign-off'],
    risk: null
  },
  {
    max: 140,
    days: '15 – 25',
    name: 'Complex project',
    colour: '#e89c3a',
    summary: 'This has real complexity — either a wide scope, tricky data, or custom logic. It needs careful planning to deliver well and on time.',
    steps: ['Discovery workshop to map all data sources and agree requirements','Data architecture and model design sign-off','Phased development with regular reviews','Structured testing and stakeholder approval'],
    risk: 'We recommend a phased approach — build the core screens first, then add complexity in subsequent sprints. This reduces risk and gets value into your hands faster.'
  },
  {
    max: 99999,
    days: '30+',
    name: 'Enterprise programme',
    colour: '#e05555',
    summary: 'This is a significant initiative. Trying to scope and deliver it in one go is the biggest risk. Proper phasing and governance is essential.',
    steps: ['Formal scoping and discovery engagement','Data strategy and architecture review','Delivery in defined phases with milestone gates','Dedicated project management and governance throughout'],
    risk: 'At this scale, we strongly recommend a separate scoping engagement before committing to a full timeline. This protects your budget and ensures the work matches your real needs — not just the initial brief.'
  }
];

const typeText = { new:'New dashboard', improve:'Improve existing', automate:'Process automation', both:'Dashboard + automation' };
const srcText  = { one:'1 data source', few:'2–4 sources', many:'5+ sources' };
const qualText = { clean:'Data is clean', some:'Some cleaning needed', messy:'Complex data situation' };
const calcText = { simple:'Simple totals & counts', medium:'Year-to-date & comparisons', complex:'Custom business logic' };
const accText  = { open:'Open access for all', some:'Role-based access', complex:'Complex access rules' };
const soText   = { self:'Self sign-off', team:'Team review', formal:'Formal UAT' };

const RATE = 500;

function parseDayRange(str) {
  if (str.includes('+')) {
    const lo = parseInt(str);
    return { lo, hi: null };
  }
  const parts = str.replace(/\s/g,'').split('–');
  return { lo: parseInt(parts[0]), hi: parseInt(parts[1]) };
}

function formatUSD(n) {
  return '$' + n.toLocaleString('en-US');
}

function buildResult() {
  const { s, screens, charts, filters, kpis } = calcScore();
  const o = outcomes.find(x => s <= x.max) || outcomes[outcomes.length - 1];
  const stepsHTML = o.steps.map((st, i) => `<li><strong style="color:var(--navy)">Step ${i+1}:</strong> ${st}</li>`).join('');
  const riskHTML  = o.risk ? `<div class="risk-box"><h4>⚠ Worth knowing before you start</h4><p>${o.risk}</p></div>` : '';

  const { lo, hi } = parseDayRange(o.days);
  const priceLo = lo * RATE;
  const priceHi = hi ? hi * RATE : null;
  const priceStr = priceHi
    ? `${formatUSD(priceLo)} – ${formatUSD(priceHi)}`
    : `From ${formatUSD(priceLo)}`;
  const daysLabel = hi ? `Based on ${lo}–${hi} days at $500 / day` : `Based on ${lo}+ days at $500 / day`;

  document.getElementById('result-area').innerHTML = `
    <div class="result-hero">
      <div class="rh">
        <div class="result-label">Your estimate</div>
        <div class="result-days">${o.days} <span>days</span></div>
        <div class="result-name" style="color:${o.colour}">${o.name}</div>
        <div class="result-summary">${o.summary}</div>
      </div>
    </div>

    <div class="price-hero">
      <div class="price-left">
        <div class="price-tag-label">Indicative budget</div>
        <div class="price-range">${priceStr} <span>USD</span></div>
        <div class="price-note">${daysLabel}</div>
      </div>
      <div class="price-right">
        <div class="price-per-day">Day rate</div>
        <div class="price-rate">$500 / day</div>
        <div class="price-days-shown">${o.days} working days</div>
      </div>
    </div>

    <div class="rcards">
      <div class="rcard">
        <div class="rcard-label">Project type</div>
        <div class="rcard-value">${typeText[A.type]}</div>
      </div>
      <div class="rcard">
        <div class="rcard-label">Scope</div>
        <div class="rcard-value">${screens} screens</div>
        <div class="rcard-sub">${charts} charts &amp; numbers &nbsp;·&nbsp; ${filters} filters</div>
      </div>
      <div class="rcard">
        <div class="rcard-label">Data</div>
        <div class="rcard-value">${srcText[A.sources]}</div>
        <div class="rcard-sub">${qualText[A.quality]}</div>
      </div>
      <div class="rcard">
        <div class="rcard-label">Sign-off</div>
        <div class="rcard-value">${soText[A.signoff]}</div>
        <div class="rcard-sub">${accText[A.access]}</div>
      </div>
    </div>

    ${riskHTML}

    <div class="what-next">
      <h4>How we would approach this together</h4>
      <ol>${stepsHTML}</ol>
    </div>

    <div class="note-card">
      <strong>This is a guide, not a contract.</strong> Every project is shaped by details we can only uncover in a conversation. A short discovery call with our team will give you a precise timeline and a clear proposal — with no surprises along the way.
    </div>
  `;
}

function restart() {
  Object.keys(A).forEach(k => delete A[k]);
  document.querySelectorAll('.choice').forEach(c => c.classList.remove('selected'));
  refreshButtons();
  document.getElementById('inp-screens').value = 5;
  document.getElementById('inp-kpis').value = 20;
  document.getElementById('inp-filters').value = 5;
  setStep(0);
}
</script>
</body>
</html>
