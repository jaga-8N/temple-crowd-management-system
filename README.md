<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>KumbhSafe AI — Predictive Pilgrimage Crowd Intelligence</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,300..900&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.4/dist/chart.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/qrcodejs@1.0.0/qrcode.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.8.2/jspdf.plugin.autotable.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
<style>
:root{
  --night:#0B1020;--night-2:#121A30;--night-3:#1B2742;
  --saffron:#FF8A3D;--gold:#F4C95D;--river:#3FA9A0;
  --danger:#E2493D;--paper:#F3EFE6;--paper-dim:#B9B7AE;
  --line:rgba(243,239,230,0.08);--green:#4CAF7D;
}
*{margin:0;padding:0;box-sizing:border-box;}
html{scroll-behavior:smooth;}
body{background:var(--night);color:var(--paper);font-family:'Inter',sans-serif;overflow-x:hidden;}
.mono{font-family:'JetBrains Mono',monospace;}
h1,h2,h3{font-family:'Fraunces',serif;font-weight:600;letter-spacing:-0.01em;}
::selection{background:var(--saffron);color:var(--night);}

/* NAV */
nav{position:fixed;top:0;left:0;right:0;z-index:100;display:flex;align-items:center;justify-content:space-between;padding:16px 36px;background:rgba(11,16,32,0.85);backdrop-filter:blur(14px);border-bottom:1px solid var(--line);}
.brand{display:flex;align-items:center;gap:10px;font-size:1.05rem;font-weight:600;}
.brand-mark{width:10px;height:10px;border-radius:50%;background:var(--saffron);box-shadow:0 0 14px var(--saffron);}
.brand .accent{color:var(--saffron);font-family:'Fraunces',serif;font-style:italic;}
.navlinks{display:flex;gap:6px;}
.navlinks button{background:none;border:none;color:var(--paper-dim);cursor:pointer;font-family:'Inter',sans-serif;font-size:0.84rem;padding:7px 13px;border-radius:8px;transition:.2s;}
.navlinks button.active,.navlinks button:hover{color:var(--paper);background:rgba(255,138,61,0.12);color:var(--saffron);}
.nav-cta{background:var(--saffron);color:var(--night);padding:8px 18px;border-radius:30px;font-weight:600;font-size:0.84rem;border:none;cursor:pointer;}

/* VIEWS */
.view{display:none;}
.view.active{display:block;}

/* HERO */
.hero{position:relative;height:100vh;min-height:680px;display:flex;flex-direction:column;justify-content:center;align-items:center;text-align:center;padding:0 24px;}
#river-canvas{position:absolute;top:0;left:0;width:100%;height:100%;z-index:0;}
.hero-content{position:relative;z-index:2;max-width:840px;}
.eyebrow{display:inline-flex;align-items:center;gap:8px;font-family:'JetBrains Mono',monospace;font-size:0.75rem;letter-spacing:0.12em;text-transform:uppercase;color:var(--gold);margin-bottom:22px;border:1px solid rgba(244,201,93,0.3);padding:7px 16px;border-radius:30px;background:rgba(244,201,93,0.06);}
.eyebrow .dot{width:6px;height:6px;border-radius:50%;background:var(--river);box-shadow:0 0 8px var(--river);animation:pulse 2s infinite;}
@keyframes pulse{0%,100%{opacity:1;}50%{opacity:0.3;}}
.hero h1{font-size:clamp(2.2rem,5.5vw,4.4rem);line-height:1.05;color:var(--paper);}
.hero h1 em{color:var(--saffron);font-style:italic;}
.hero p{font-size:1.1rem;color:var(--paper-dim);margin:22px auto 34px;max-width:580px;line-height:1.65;}
.hero-actions{display:flex;gap:14px;justify-content:center;flex-wrap:wrap;}
.btn-primary,.btn-ghost{padding:13px 26px;border-radius:30px;font-weight:600;font-size:0.92rem;cursor:pointer;border:none;transition:.22s;}
.btn-primary{background:var(--saffron);color:var(--night);}
.btn-primary:hover{background:var(--gold);transform:translateY(-1px);}
.btn-ghost{background:transparent;border:1px solid var(--line);color:var(--paper);}
.btn-ghost:hover{border-color:var(--saffron);color:var(--saffron);}
.btn-sm{padding:8px 18px;font-size:0.82rem;border-radius:20px;}
.hero-stats{position:relative;z-index:2;display:flex;gap:48px;margin-top:56px;flex-wrap:wrap;justify-content:center;}
.hero-stats div{text-align:center;}
.hero-stats .num{font-family:'Fraunces',serif;font-size:1.9rem;color:var(--gold);}
.hero-stats .lbl{font-size:0.74rem;color:var(--paper-dim);text-transform:uppercase;letter-spacing:0.07em;margin-top:4px;}

/* SECTION */
.section{padding:90px 36px;max-width:1280px;margin:0 auto;}
.section-head{margin-bottom:44px;}
.section-head .tag{font-family:'JetBrains Mono',monospace;color:var(--river);font-size:0.75rem;letter-spacing:0.1em;text-transform:uppercase;}
.section-head h2{font-size:clamp(1.7rem,3vw,2.4rem);margin-top:10px;}
.section-head p{color:var(--paper-dim);margin-top:12px;max-width:580px;line-height:1.6;}

/* CARDS */
.card{background:linear-gradient(160deg,var(--night-2),var(--night-3));border:1px solid var(--line);border-radius:18px;padding:24px;}
.card h3{font-size:1rem;font-weight:600;font-family:'Inter';margin-bottom:4px;}
.card .sub{font-size:0.78rem;color:var(--paper-dim);margin-bottom:16px;}
.cc-grid{display:grid;grid-template-columns:1.3fr 1fr;gap:22px;margin-bottom:22px;}
.cc-row3{display:grid;grid-template-columns:1.4fr 1fr;gap:22px;}

/* COMMAND CENTER */
#zone-canvas{width:100%;height:360px;border-radius:12px;background:#0A0E1C;display:block;}
.legend{display:flex;gap:16px;margin-top:12px;font-size:0.76rem;color:var(--paper-dim);}
.legend span{display:inline-flex;align-items:center;gap:5px;}
.legend i{width:8px;height:8px;border-radius:50%;display:inline-block;}
.risk-list{display:flex;flex-direction:column;gap:9px;max-height:360px;overflow-y:auto;padding-right:4px;}
.risk-item{display:flex;justify-content:space-between;align-items:center;padding:11px 13px;border-radius:9px;background:rgba(255,255,255,0.02);border:1px solid var(--line);font-size:0.84rem;}
.risk-item .zname{font-weight:600;}
.risk-item .zmeta{color:var(--paper-dim);font-size:0.72rem;margin-top:2px;}
.badge{font-family:'JetBrains Mono',monospace;font-size:0.68rem;font-weight:600;padding:3px 9px;border-radius:20px;}
.badge.low{background:rgba(76,175,125,0.15);color:var(--green);}
.badge.med{background:rgba(244,201,93,0.15);color:var(--gold);}
.badge.high{background:rgba(226,73,61,0.18);color:var(--danger);animation:blink 1.4s infinite;}
@keyframes blink{0%,100%{opacity:1;}50%{opacity:0.5;}}
#forecast-chart-wrap{height:270px;}
.alert-feed{display:flex;flex-direction:column;gap:10px;max-height:270px;overflow-y:auto;}
.alert-item{border-left:3px solid var(--danger);padding:9px 13px;background:rgba(226,73,61,0.06);border-radius:0 8px 8px 0;font-size:0.82rem;}
.alert-item.info{border-color:var(--river);background:rgba(63,169,160,0.06);}
.alert-item .time{color:var(--paper-dim);font-size:0.7rem;font-family:'JetBrains Mono',monospace;margin-top:3px;}

/* BOOKING */
.stepper{display:flex;gap:8px;margin-bottom:32px;}
.step{flex:1;text-align:center;padding:11px 6px;border-radius:9px;background:var(--night-2);border:1px solid var(--line);font-size:0.78rem;color:var(--paper-dim);}
.step.active{color:var(--night);background:var(--saffron);font-weight:600;border-color:var(--saffron);}
.step.done{color:var(--green);border-color:var(--green);}
.booking-card{background:var(--night-2);border:1px solid var(--line);border-radius:18px;padding:32px;max-width:700px;margin:0 auto;}
.field{margin-bottom:16px;}
.field label{display:block;font-size:0.8rem;color:var(--paper-dim);margin-bottom:6px;}
.field input,.field select{width:100%;padding:11px 13px;border-radius:9px;border:1px solid var(--line);background:var(--night);color:var(--paper);font-family:'Inter';font-size:0.9rem;}
.field input:focus,.field select:focus{outline:none;border-color:var(--saffron);}
.zone-pick{display:grid;grid-template-columns:repeat(4,1fr);gap:9px;}
.zone-opt{padding:12px 6px;border-radius:9px;border:1px solid var(--line);text-align:center;font-size:0.75rem;cursor:pointer;background:var(--night);}
.zone-opt.selected{border-color:var(--saffron);background:rgba(255,138,61,0.1);color:var(--saffron);}
.slot-pick{display:grid;grid-template-columns:repeat(3,1fr);gap:9px;}
.slot-opt{padding:11px;border-radius:9px;border:1px solid var(--line);text-align:center;font-size:0.8rem;cursor:pointer;background:var(--night);}
.slot-opt.full{opacity:0.32;cursor:not-allowed;}
.slot-opt.selected{border-color:var(--river);background:rgba(63,169,160,0.1);color:var(--river);}
.btn-row{display:flex;justify-content:space-between;margin-top:26px;}
.qr-result{text-align:center;}
#qrcode{display:inline-block;margin:18px auto;padding:14px;background:#fff;border-radius:12px;}
.confirm-detail{display:flex;justify-content:space-between;padding:9px 0;border-bottom:1px solid var(--line);font-size:0.86rem;}
.confirm-detail span:first-child{color:var(--paper-dim);}

/* AI CHAT */
.chat-wrap{max-width:700px;margin:0 auto;background:var(--night-2);border:1px solid var(--line);border-radius:18px;display:flex;flex-direction:column;height:540px;}
.chat-head{padding:16px 20px;border-bottom:1px solid var(--line);display:flex;align-items:center;gap:10px;}
.chat-head .dot2{width:8px;height:8px;border-radius:50%;background:var(--river);box-shadow:0 0 8px var(--river);}
.chat-body{flex:1;overflow-y:auto;padding:20px;display:flex;flex-direction:column;gap:12px;}
.msg{max-width:78%;padding:11px 15px;border-radius:14px;font-size:0.88rem;line-height:1.55;}
.msg.bot{background:var(--night-3);align-self:flex-start;border-bottom-left-radius:4px;}
.msg.user{background:var(--saffron);color:var(--night);align-self:flex-end;border-bottom-right-radius:4px;}
.typing{display:flex;gap:4px;align-items:center;padding:11px 15px;}
.typing span{width:6px;height:6px;border-radius:50%;background:var(--paper-dim);animation:bounce 1.2s infinite;}
.typing span:nth-child(2){animation-delay:.2s;}
.typing span:nth-child(3){animation-delay:.4s;}
@keyframes bounce{0%,60%,100%{transform:translateY(0);}30%{transform:translateY(-6px);}}
.suggest-chips{display:flex;gap:7px;padding:0 20px 12px;flex-wrap:wrap;}
.chip{font-size:0.75rem;border:1px solid var(--line);padding:6px 12px;border-radius:20px;cursor:pointer;color:var(--paper-dim);}
.chip:hover{border-color:var(--saffron);color:var(--saffron);}
.chat-input{display:flex;gap:9px;padding:14px;border-top:1px solid var(--line);}
.chat-input input{flex:1;padding:11px 15px;border-radius:30px;border:1px solid var(--line);background:var(--night);color:var(--paper);font-family:'Inter';}
.chat-input input:focus{outline:none;border-color:var(--saffron);}
.chat-input button{background:var(--saffron);border:none;color:var(--night);width:42px;height:42px;border-radius:50%;cursor:pointer;font-size:1rem;}

/* LOST & FOUND */
.lf-grid{display:grid;grid-template-columns:1fr 1fr;gap:22px;}
.upload-area{border:2px dashed var(--line);border-radius:14px;padding:0;text-align:center;color:var(--paper-dim);cursor:pointer;transition:.2s;position:relative;overflow:hidden;min-height:180px;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:10px;}
.upload-area:hover{border-color:var(--saffron);background:rgba(255,138,61,0.03);}
.upload-area.has-image{border-color:var(--river);padding:0;}
.upload-area input[type=file]{position:absolute;inset:0;opacity:0;cursor:pointer;width:100%;height:100%;}
.upload-area img{width:100%;height:200px;object-fit:cover;border-radius:12px;display:block;}
.upload-icon{font-size:2.2rem;}
.upload-label{font-size:0.84rem;}
.upload-sub{font-size:0.72rem;color:var(--paper-dim);}
.match-result{display:flex;gap:12px;align-items:center;background:rgba(63,169,160,0.08);border:1px solid rgba(63,169,160,0.3);border-radius:12px;padding:14px;margin-top:14px;font-size:0.84rem;}
.match-result .avatar{width:52px;height:52px;border-radius:50%;object-fit:cover;border:2px solid var(--river);}
.match-result .avatar-placeholder{width:52px;height:52px;border-radius:50%;background:var(--night-3);display:flex;align-items:center;justify-content:center;font-size:1.4rem;border:2px solid var(--river);}
.no-match{background:rgba(226,73,61,0.07);border:1px solid rgba(226,73,61,0.25);border-radius:12px;padding:14px;margin-top:14px;font-size:0.84rem;color:var(--danger);}
.lf-reports{display:flex;flex-direction:column;gap:10px;max-height:320px;overflow-y:auto;margin-top:12px;}
.lf-report-item{display:flex;gap:12px;align-items:center;padding:11px 13px;background:rgba(255,255,255,0.02);border:1px solid var(--line);border-radius:10px;font-size:0.82rem;}
.lf-report-item img{width:38px;height:38px;border-radius:50%;object-fit:cover;}
.lf-report-item .placeholder-img{width:38px;height:38px;border-radius:50%;background:var(--night-3);display:flex;align-items:center;justify-content:center;font-size:1rem;}

/* GOVT ANALYTICS */
.analytics-header{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:32px;flex-wrap:wrap;gap:16px;}
.analytics-header h2{font-size:clamp(1.5rem,2.8vw,2.2rem);}
.date-filter{display:flex;gap:8px;flex-wrap:wrap;}
.date-btn{padding:7px 14px;border-radius:8px;border:1px solid var(--line);background:var(--night-2);color:var(--paper-dim);font-size:0.78rem;cursor:pointer;}
.date-btn.active{background:var(--saffron);color:var(--night);border-color:var(--saffron);font-weight:600;}
.kpi-row{display:grid;grid-template-columns:repeat(4,1fr);gap:16px;margin-bottom:24px;}
.kpi-card{background:linear-gradient(160deg,var(--night-2),var(--night-3));border:1px solid var(--line);border-radius:14px;padding:20px;}
.kpi-card .kpi-icon{font-size:1.5rem;margin-bottom:8px;}
.kpi-card .kpi-val{font-family:'Fraunces',serif;font-size:1.9rem;font-weight:700;line-height:1;}
.kpi-card .kpi-lbl{font-size:0.74rem;color:var(--paper-dim);margin-top:5px;text-transform:uppercase;letter-spacing:0.06em;}
.kpi-card .kpi-delta{font-size:0.74rem;margin-top:6px;font-family:'JetBrains Mono',monospace;}
.kpi-card .kpi-delta.up{color:var(--green);}
.kpi-card .kpi-delta.down{color:var(--danger);}
.analytics-grid{display:grid;grid-template-columns:1.5fr 1fr;gap:22px;margin-bottom:22px;}
.analytics-row2{display:grid;grid-template-columns:1fr 1fr 1fr;gap:22px;margin-bottom:22px;}
.chart-wrap-md{height:260px;}
.chart-wrap-sm{height:220px;}
.table-wrap{overflow-x:auto;}
table{width:100%;border-collapse:collapse;font-size:0.82rem;}
table th{padding:10px 12px;text-align:left;color:var(--paper-dim);font-weight:500;font-size:0.74rem;text-transform:uppercase;letter-spacing:0.06em;border-bottom:1px solid var(--line);}
table td{padding:10px 12px;border-bottom:1px solid rgba(243,239,230,0.04);}
table tr:last-child td{border-bottom:none;}
table tr:hover td{background:rgba(255,255,255,0.02);}
.status-pill{font-size:0.68rem;font-weight:600;padding:3px 8px;border-radius:10px;font-family:'JetBrains Mono',monospace;}
.status-pill.confirmed{background:rgba(76,175,125,0.15);color:var(--green);}
.status-pill.pending{background:rgba(244,201,93,0.15);color:var(--gold);}
.status-pill.cancelled{background:rgba(226,73,61,0.15);color:var(--danger);}
.progress-bar-wrap{background:rgba(255,255,255,0.06);border-radius:4px;height:6px;margin-top:4px;}
.progress-bar{height:6px;border-radius:4px;background:var(--saffron);}
.lang-row{display:flex;align-items:center;gap:10px;padding:7px 0;font-size:0.82rem;}
.lang-row .lang-name{width:70px;color:var(--paper-dim);}
.lang-row .lang-bar-wrap{flex:1;background:rgba(255,255,255,0.06);border-radius:4px;height:5px;}
.lang-row .lang-bar{height:5px;border-radius:4px;background:var(--river);}
.lang-row .lang-pct{width:36px;text-align:right;font-family:'JetBrains Mono',monospace;font-size:0.74rem;color:var(--paper-dim);}
.zone-analytics-list{display:flex;flex-direction:column;gap:8px;max-height:260px;overflow-y:auto;}
.zone-analytics-item{display:flex;justify-content:space-between;align-items:center;padding:9px 12px;background:rgba(255,255,255,0.02);border:1px solid var(--line);border-radius:9px;font-size:0.8rem;}
.export-row{display:flex;gap:10px;margin-bottom:20px;flex-wrap:wrap;}
.export-btn{display:flex;align-items:center;gap:6px;padding:9px 16px;border-radius:9px;border:1px solid var(--line);background:var(--night-2);color:var(--paper-dim);font-size:0.8rem;cursor:pointer;transition:.2s;}
.export-btn:hover{border-color:var(--saffron);color:var(--saffron);}

/* FEATURE CARDS HOME */
.feature-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:20px;}

footer{padding:44px 36px;text-align:center;color:var(--paper-dim);font-size:0.8rem;border-top:1px solid var(--line);}

@media(max-width:960px){
  .cc-grid,.cc-row3,.lf-grid,.analytics-grid{grid-template-columns:1fr;}
  .kpi-row{grid-template-columns:repeat(2,1fr);}
  .analytics-row2{grid-template-columns:1fr 1fr;}
  .zone-pick{grid-template-columns:repeat(2,1fr);}
  .feature-grid{grid-template-columns:1fr;}
}
@media(max-width:600px){
  .kpi-row{grid-template-columns:1fr 1fr;}
  .analytics-row2{grid-template-columns:1fr;}
  nav{padding:14px 18px;}
  .navlinks button{font-size:0.75rem;padding:6px 8px;}
}
</style>
</head>
<body>

<nav>
  <div class="brand"><span class="brand-mark"></span>KumbhSafe <span class="accent">AI</span></div>
  <div class="navlinks">
    <button data-view="home" class="active">Home</button>
    <button data-view="command">Command Center</button>
    <button data-view="booking">Book Slot</button>
    <button data-view="assistant">AI Assistant</button>
    <button data-view="lostfound">Lost & Found</button>
    <button data-view="analytics">Gov Analytics</button>
  </div>
  <button class="nav-cta" data-view="analytics">Gov Portal →</button>
</nav>

<!-- ===== HOME ===== -->
<div class="view active" id="view-home">
  <section class="hero">
    <canvas id="river-canvas"></canvas>
    <div class="hero-content">
      <div class="eyebrow"><span class="dot"></span> Live across 16 zones · Risk engine active</div>
      <h1>Stampedes are <em>predictable.</em><br>We act before they happen.</h1>
      <p>KumbhSafe AI forecasts dangerous crowd density 15–30 minutes ahead, auto-reroutes pilgrims, and gives authorities one command center for a million-person event.</p>
      <div class="hero-actions">
        <button class="btn-primary" data-view="command">Open Command Center</button>
        <button class="btn-ghost" data-view="booking">Book a Pilgrim Slot</button>
      </div>
    </div>
    <div class="hero-stats">
      <div><div class="num mono">16</div><div class="lbl">Monitored Zones</div></div>
      <div><div class="num mono">15min</div><div class="lbl">Forecast Horizon</div></div>
      <div><div class="num mono">&lt;2s</div><div class="lbl">Alert Latency</div></div>
      <div><div class="num mono">24/7</div><div class="lbl">Autonomous Watch</div></div>
    </div>
  </section>
  <section class="section">
    <div class="section-head">
      <div class="tag">Why it's different</div>
      <h2>Dashboards show you a crowd.<br>We show you the next 15 minutes.</h2>
      <p>Most crowd-management tools display CCTV feeds and counts. KumbhSafe layers a forecasting and intervention engine — turning monitoring into prevention.</p>
    </div>
    <div class="feature-grid">
      <div class="card"><h3>🌊 Digital Twin Simulation</h3><div class="sub">A living animated model of pilgrim flow across every ghat and gate — not a static map.</div></div>
      <div class="card"><h3>🔮 Predictive Risk Scoring</h3><div class="sub">Each zone gets a live risk score forecasting density before it becomes dangerous.</div></div>
      <div class="card"><h3>🗣️ Multilingual AI Assistant</h3><div class="sub">Pilgrims ask "When should I visit Zone 4?" in their language and get live data-grounded answers.</div></div>
      <div class="card"><h3>📊 Government Analytics</h3><div class="sub">Booking trends, zone occupancy, language distribution and incident reports in one gov portal.</div></div>
    </div>
  </section>
</div>

<!-- ===== COMMAND CENTER ===== -->
<div class="view" id="view-command">
  <section class="section" style="padding-top:110px;">
    <div class="section-head">
      <div class="tag">Authority Command Center</div>
      <h2>Live Crowd Intelligence</h2>
      <p>Real-time density, forecasted risk, and one-tap intervention across all 16 zones.</p>
    </div>
    <div class="cc-grid">
      <div class="card">
        <h3>Live Density Map</h3>
        <div class="sub">Animated particle field — each dot is a cluster of pilgrims. Updates every 2.5s.</div>
        <canvas id="zone-canvas"></canvas>
        <div class="legend">
          <span><i style="background:#4CAF7D"></i> Safe</span>
          <span><i style="background:#F4C95D"></i> Moderate</span>
          <span><i style="background:#E2493D"></i> Critical</span>
        </div>
      </div>
      <div class="card">
        <h3>Zone Risk Ranking</h3>
        <div class="sub">Sorted by predicted risk score (next 15 min)</div>
        <div class="risk-list" id="risk-list"></div>
      </div>
    </div>
    <div class="cc-row3">
      <div class="card">
        <h3>Density Forecast — Zone 4 (Har Ki Pauri)</h3>
        <div class="sub">AI projection with confidence band, next 30 minutes</div>
        <div id="forecast-chart-wrap"><canvas id="forecast-chart"></canvas></div>
      </div>
      <div class="card">
        <h3>Live Alert Feed</h3>
        <div class="sub">Auto-generated by the risk engine</div>
        <div class="alert-feed" id="alert-feed"></div>
      </div>
    </div>
  </section>
</div>

<!-- ===== BOOKING ===== -->
<div class="view" id="view-booking">
  <section class="section" style="padding-top:110px;max-width:760px;">
    <div class="section-head">
      <div class="tag">Pilgrim Portal</div>
      <h2>Book a Darshan Slot</h2>
      <p>Reserve a time window for a specific zone to avoid peak density. Takes under a minute.</p>
    </div>
    <div class="stepper">
      <div class="step active" id="step-tab-1">1 · Details</div>
      <div class="step" id="step-tab-2">2 · Zone</div>
      <div class="step" id="step-tab-3">3 · Slot</div>
      <div class="step" id="step-tab-4">4 · Confirm</div>
    </div>
    <div class="booking-card">
      <div class="bstep" id="bstep-1">
        <div class="field"><label>Full Name</label><input type="text" id="p-name" placeholder="e.g. Ramesh Kumar"></div>
        <div class="field"><label>Phone Number</label><input type="tel" id="p-phone" placeholder="+91 9XXXXXXXXX"></div>
        <div class="field"><label>Group Size</label><input type="number" id="p-group" value="1" min="1" max="20"></div>
        <div class="field"><label>Preferred Language</label>
          <select id="p-lang"><option>English</option><option>Hindi</option><option>Marathi</option><option>Tamil</option><option>Bengali</option><option>Telugu</option></select>
        </div>
        <div class="btn-row"><span></span><button class="btn-primary" onclick="nextStep(1)">Continue →</button></div>
      </div>
      <div class="bstep" id="bstep-2" style="display:none;">
        <p style="color:var(--paper-dim);font-size:0.82rem;margin-bottom:14px;">Select your preferred zone. Live risk shown.</p>
        <div class="zone-pick" id="zone-pick"></div>
        <div class="btn-row"><button class="btn-ghost" onclick="prevStep(2)">← Back</button><button class="btn-primary" onclick="nextStep(2)">Continue →</button></div>
      </div>
      <div class="bstep" id="bstep-3" style="display:none;">
        <p style="color:var(--paper-dim);font-size:0.82rem;margin-bottom:14px;">Available time slots for your selected zone today.</p>
        <div class="slot-pick" id="slot-pick"></div>
        <div class="btn-row"><button class="btn-ghost" onclick="prevStep(3)">← Back</button><button class="btn-primary" onclick="nextStep(3)">Confirm Booking →</button></div>
      </div>
      <div class="bstep" id="bstep-4" style="display:none;">
        <div class="qr-result">
          <h3 style="color:var(--green);">✓ Booking Confirmed</h3>
          <div id="qrcode"></div>
          <div id="confirm-details"></div>
        </div>
        <div class="btn-row"><span></span><button class="btn-primary" onclick="resetBooking()">Book Another Slot</button></div>
      </div>
    </div>
  </section>
</div>

<!-- ===== AI ASSISTANT ===== -->
<div class="view" id="view-assistant">
  <section class="section" style="padding-top:110px;">
    <div class="section-head">
      <div class="tag">AI Pilgrim Assistant</div>
      <h2>Ask anything about the event, live</h2>
      <p>Grounded in real-time zone data — not a generic chatbot.</p>
    </div>
    <div class="chat-wrap">
      <div class="chat-head"><span class="dot2"></span><strong>KumbhSafe Assistant</strong><span style="color:var(--paper-dim);font-size:0.76rem;margin-left:6px;">· grounded in live zone data</span></div>
      <div class="chat-body" id="chat-body">
        <div class="msg bot">Namaste 🙏 I'm your KumbhSafe assistant. Ask me about crowd levels, the safest time to visit a ghat, or how to book a slot.</div>
      </div>
      <div class="suggest-chips">
        <div class="chip" onclick="askChip(this)">When should I visit Har Ki Pauri?</div>
        <div class="chip" onclick="askChip(this)">Which zone is least crowded right now?</div>
        <div class="chip" onclick="askChip(this)">How do I report a lost child?</div>
        <div class="chip" onclick="askChip(this)">Which zones are safe now?</div>
      </div>
      <div class="chat-input">
        <input type="text" id="chat-in" placeholder="Type your question...">
        <button onclick="sendChat()">↑</button>
      </div>
    </div>
  </section>
</div>

<!-- ===== LOST & FOUND ===== -->
<div class="view" id="view-lostfound">
  <section class="section" style="padding-top:110px;">
    <div class="section-head">
      <div class="tag">Lost & Found</div>
      <h2>AI-Assisted Reunification</h2>
      <p>Upload a real photo of a missing person — AI matches against sightings. All photo data is temporary and deleted after 24 hours.</p>
    </div>
    <div class="lf-grid">
      <div class="card">
        <h3>Report Missing Person</h3>
        <div class="sub">Upload a clear face photo for AI matching</div>

        <div class="upload-area" id="upload-area">
          <input type="file" id="photo-input" accept="image/*" onchange="handlePhotoUpload(event)">
          <div class="upload-icon">📷</div>
          <div class="upload-label">Click or drag to upload photo</div>
          <div class="upload-sub">JPG, PNG, WEBP · Max 10MB · Deleted after 24h</div>
        </div>

        <div id="photo-preview-wrap" style="display:none;margin-top:12px;">
          <img id="photo-preview" style="width:100%;max-height:200px;object-fit:cover;border-radius:12px;border:1px solid var(--line);">
          <button onclick="clearPhoto()" style="margin-top:8px;background:none;border:none;color:var(--danger);font-size:0.78rem;cursor:pointer;">✕ Remove photo</button>
        </div>

        <div class="field" style="margin-top:16px;"><label>Person's Name (if known)</label><input type="text" id="lf-name" placeholder="e.g. Suresh Kumar"></div>
        <div class="field"><label>Age (approx)</label><input type="number" id="lf-age" placeholder="e.g. 65" min="1" max="120"></div>
        <div class="field"><label>Last Seen Zone</label>
          <select id="lf-zone">
            <option>Zone 4 — Har Ki Pauri</option><option>Zone 9 — Triveni Ghat</option>
            <option>Zone 12 — Sangam Nose</option><option>Zone 1 — Haridwar North</option>
            <option>Zone 7 — Brahma Kund</option>
          </select>
        </div>
        <div class="field"><label>Your Contact Number</label><input type="tel" id="lf-contact" placeholder="+91 9XXXXXXXXX"></div>
        <button class="btn-primary" style="width:100%;margin-top:6px;" onclick="runAIMatch()">🔍 Run AI Match</button>
        <div id="lf-result"></div>
      </div>

      <div>
        <div class="card" style="margin-bottom:20px;">
          <h3>How it works</h3>
          <div class="sub">Face-embedding similarity against volunteer photo submissions and gate cameras.</div>
          <ol style="color:var(--paper-dim);font-size:0.85rem;line-height:2;padding-left:18px;">
            <li>Upload a clear face photo of the missing person</li>
            <li>AI generates a face-embedding vector</li>
            <li>Compared against sightings database in real time</li>
            <li>Top matches routed to nearest help desk staff</li>
            <li>You receive an SMS alert when a match is found</li>
          </ol>
          <div style="margin-top:14px;padding:12px;background:rgba(63,169,160,0.07);border-radius:9px;font-size:0.78rem;color:var(--paper-dim);">
            🔒 Privacy: Photos are hashed and never stored as raw images beyond 24h. Used solely for the reunification match.
          </div>
        </div>
        <div class="card">
          <h3>Recent Reports</h3>
          <div class="sub">Active missing person alerts today</div>
          <div class="lf-reports" id="lf-reports"></div>
        </div>
      </div>
    </div>
  </section>
</div>

<!-- ===== GOVT ANALYTICS ===== -->
<div class="view" id="view-analytics">
  <section class="section" style="padding-top:110px;">
    <div class="analytics-header">
      <div>
        <div class="tag" style="font-family:'JetBrains Mono',monospace;color:var(--river);font-size:0.75rem;letter-spacing:0.1em;text-transform:uppercase;margin-bottom:8px;">Government Portal · Restricted Access</div>
        <h2>Booking & Crowd Analytics</h2>
      </div>
      <div class="date-filter">
        <button class="date-btn active" onclick="setDateFilter(this,'today')">Today</button>
        <button class="date-btn" onclick="setDateFilter(this,'week')">This Week</button>
        <button class="date-btn" onclick="setDateFilter(this,'month')">This Month</button>
      </div>
    </div>

    <div class="export-row">
      <button class="export-btn" id="btn-csv" onclick="exportCSV()">📥 Export CSV</button>
      <button class="export-btn" id="btn-pdf" onclick="exportPDF()">📄 Download Report PDF</button>
      <button class="export-btn" id="btn-email" onclick="sendEmailReport()">📧 Email Report to Gov</button>
    </div>

    <div class="kpi-row">
      <div class="kpi-card">
        <div class="kpi-icon">🎫</div>
        <div class="kpi-val" id="kpi-bookings">4,821</div>
        <div class="kpi-lbl">Total Bookings</div>
        <div class="kpi-delta up">↑ 12% vs yesterday</div>
      </div>
      <div class="kpi-card">
        <div class="kpi-icon">🧑‍🤝‍🧑</div>
        <div class="kpi-val" id="kpi-pilgrims">18,340</div>
        <div class="kpi-lbl">Total Pilgrims</div>
        <div class="kpi-delta up">↑ 8% vs yesterday</div>
      </div>
      <div class="kpi-card">
        <div class="kpi-icon">⚠️</div>
        <div class="kpi-val" id="kpi-incidents">3</div>
        <div class="kpi-lbl">Incidents Today</div>
        <div class="kpi-delta down">↓ 2 resolved</div>
      </div>
      <div class="kpi-card">
        <div class="kpi-icon">🎯</div>
        <div class="kpi-val" id="kpi-occupancy">68%</div>
        <div class="kpi-lbl">Avg Occupancy</div>
        <div class="kpi-delta up">↑ Within safe range</div>
      </div>
    </div>

    <div class="analytics-grid">
      <div class="card">
        <h3>Bookings Over Time</h3>
        <div class="sub">Hourly booking volume across all zones</div>
        <div class="chart-wrap-md"><canvas id="bookings-chart"></canvas></div>
      </div>
      <div class="card">
        <h3>Booking Status Breakdown</h3>
        <div class="sub">Confirmed · Pending · Cancelled</div>
        <div class="chart-wrap-md" style="display:flex;align-items:center;justify-content:center;">
          <canvas id="status-chart" style="max-height:220px;"></canvas>
        </div>
      </div>
    </div>

    <div class="analytics-row2">
      <div class="card">
        <h3>Zone Popularity</h3>
        <div class="sub">Bookings per zone today</div>
        <div class="zone-analytics-list" id="zone-analytics-list"></div>
      </div>
      <div class="card">
        <h3>Language Distribution</h3>
        <div class="sub">Preferred language of pilgrims</div>
        <div id="lang-list"></div>
      </div>
      <div class="card">
        <h3>Peak Hours Heatmap</h3>
        <div class="sub">Booking demand by hour</div>
        <div class="chart-wrap-sm"><canvas id="peak-chart"></canvas></div>
      </div>
    </div>

    <div class="card">
      <h3>Recent Bookings Log</h3>
      <div class="sub" style="margin-bottom:16px;">Latest confirmed bookings across all zones</div>
      <div class="table-wrap">
        <table id="bookings-table">
          <thead>
            <tr>
              <th>Token</th><th>Pilgrim</th><th>Zone</th><th>Slot</th><th>Group</th><th>Language</th><th>Status</th>
            </tr>
          </thead>
          <tbody id="bookings-tbody"></tbody>
        </table>
      </div>
    </div>

  </section>
</div>

<footer>KumbhSafe AI · Predictive Pilgrimage Crowd Intelligence · Built for safer mass gatherings</footer>

<script>
// ===================== NAV =====================
const views = document.querySelectorAll('.view');
function showView(name){
  views.forEach(v=>v.classList.toggle('active', v.id==='view-'+name));
  document.querySelectorAll('.navlinks button').forEach(b=>b.classList.toggle('active', b.dataset.view===name));
  window.scrollTo({top:0,behavior:'smooth'});
  if(name==='analytics') initAnalytics();
}
document.querySelectorAll('[data-view]').forEach(b=>b.addEventListener('click',()=>showView(b.dataset.view)));

// ===================== HERO CANVAS =====================
const riverCanvas = document.getElementById('river-canvas');
const rctx = riverCanvas.getContext('2d');
function resizeRiver(){riverCanvas.width=riverCanvas.offsetWidth;riverCanvas.height=riverCanvas.offsetHeight;}
resizeRiver();
window.addEventListener('resize',resizeRiver);
let rp = Array.from({length:160},()=>({x:Math.random()*riverCanvas.width,y:Math.random()*riverCanvas.height,r:Math.random()*2+1,speed:Math.random()*0.5+0.15,hue:Math.random()}));
function drawRiver(){
  rctx.clearRect(0,0,riverCanvas.width,riverCanvas.height);
  rp.forEach(p=>{
    p.x+=p.speed; if(p.x>riverCanvas.width)p.x=0;
    const c=p.hue>0.66?'244,201,93':(p.hue>0.33?'63,169,160':'255,138,61');
    rctx.beginPath();rctx.arc(p.x,p.y,p.r,0,Math.PI*2);rctx.fillStyle=`rgba(${c},0.5)`;rctx.fill();
  });
  requestAnimationFrame(drawRiver);
}
drawRiver();

// ===================== ZONE DATA =====================
const zoneNames=["Har Ki Pauri","Triveni Ghat","Sangam Nose","Ram Ghat","Mahakal Approach","Pashupatinath Path","Brahma Kund","Sati Ghat","Gau Ghat","Narmada Ghat","Kshipra Bridge","Saraswati Tat","Nasik Ramkund","Ujjain Bridge","Allahabad Sector","Haridwar North"];
let zones=zoneNames.map((name,i)=>({id:i+1,name,capacity:5000+(i%4)*1500,count:Math.floor(Math.random()*4000)+500}));
function riskOf(z){return z.count/z.capacity;}
function riskLabel(r){if(r>0.78)return{cls:'high',txt:'CRITICAL'};if(r>0.5)return{cls:'med',txt:'MODERATE'};return{cls:'low',txt:'SAFE'};}

function renderRiskList(){
  const list=document.getElementById('risk-list');
  const sorted=[...zones].sort((a,b)=>riskOf(b)-riskOf(a));
  list.innerHTML=sorted.map(z=>{
    const r=riskOf(z);const lbl=riskLabel(r);
    return`<div class="risk-item"><div><div class="zname">Z${z.id} · ${z.name}</div><div class="zmeta mono">${z.count.toLocaleString()} / ${z.capacity.toLocaleString()}</div></div><span class="badge ${lbl.cls}">${lbl.txt}</span></div>`;
  }).join('');
}
renderRiskList();

// zone pick booking
let selectedZone=null;
function renderZonePick(){
  document.getElementById('zone-pick').innerHTML=zones.slice(0,8).map(z=>{
    const lbl=riskLabel(riskOf(z));
    return`<div class="zone-opt" data-id="${z.id}" onclick="pickZone(${z.id})"><div style="font-weight:600;">Z${z.id}</div><div style="font-size:0.68rem;color:var(--paper-dim);margin:2px 0;">${z.name}</div><div class="badge ${lbl.cls}" style="font-size:0.6rem;">${lbl.txt}</div></div>`;
  }).join('');
}
function pickZone(id){selectedZone=zones.find(z=>z.id===id);document.querySelectorAll('.zone-opt').forEach(el=>el.classList.toggle('selected',+el.dataset.id===id));}
renderZonePick();

// slot pick
const slotTimes=["6:00–7:00 AM","7:00–8:00 AM","8:00–9:00 AM","9:00–10:00 AM","4:00–5:00 PM","5:00–6:00 PM"];
let selectedSlot=null;
function renderSlotPick(){
  document.getElementById('slot-pick').innerHTML=slotTimes.map((t,i)=>{
    const full=i===2;
    return`<div class="slot-opt ${full?'full':''}" data-i="${i}" onclick="${full?'':'pickSlot('+i+')'}">${t}${full?'<br><span style="font-size:0.62rem;">FULL</span>':''}</div>`;
  }).join('');
}
function pickSlot(i){selectedSlot=slotTimes[i];document.querySelectorAll('.slot-opt').forEach(el=>el.classList.toggle('selected',+el.dataset.i===i));}
renderSlotPick();

// ===================== ZONE HEATMAP =====================
const zc=document.getElementById('zone-canvas');
const zctx=zc.getContext('2d');
function resizeZC(){zc.width=zc.offsetWidth;zc.height=zc.offsetHeight;}
resizeZC();
window.addEventListener('resize',resizeZC);
function colorFor(r){if(r>0.78)return'226,73,61';if(r>0.5)return'244,201,93';return'76,175,125';}
function drawZoneMap(){
  resizeZC();zctx.clearRect(0,0,zc.width,zc.height);
  const cw=zc.width/4,ch=zc.height/4;
  zones.forEach((z,i)=>{
    const cx=(i%4)*cw+cw/2,cy=Math.floor(i/4)*ch+ch/2,r=riskOf(z),c=colorFor(r);
    const pc=Math.floor(r*40)+8;
    for(let p=0;p<pc;p++){
      const ang=Math.random()*Math.PI*2,dist=Math.random()*Math.min(cw,ch)*0.36;
      zctx.beginPath();zctx.arc(cx+Math.cos(ang)*dist,cy+Math.sin(ang)*dist,r>0.78?2.4:1.6,0,Math.PI*2);
      zctx.fillStyle=`rgba(${c},${r>0.78?0.9:0.65})`;zctx.fill();
    }
    zctx.font='600 9px JetBrains Mono';zctx.fillStyle='rgba(243,239,230,0.5)';zctx.textAlign='center';
    zctx.fillText('Z'+z.id,cx,cy-Math.min(cw,ch)*0.4);
  });
  requestAnimationFrame(drawZoneMap);
}
drawZoneMap();

setInterval(()=>{
  zones.forEach(z=>{z.count+=Math.floor((Math.random()-0.45)*250);z.count=Math.max(200,Math.min(z.capacity*1.05,z.count));});
  renderRiskList();if(Math.random()<0.5)pushAlert();
},2800);

// ===================== FORECAST CHART =====================
const fctx=document.getElementById('forecast-chart').getContext('2d');
new Chart(fctx,{type:'line',data:{labels:['-15m','-10m','-5m','Now','+5m','+10m','+15m','+20m','+25m','+30m'],datasets:[
  {label:'Upper bound',data:[null,null,null,null,4500,5000,5450,5700,5500,5100],borderColor:'transparent',backgroundColor:'rgba(255,138,61,0.08)',fill:'+1',pointRadius:0,tension:0.35},
  {label:'Lower bound',data:[null,null,null,null,4000,4200,4450,4500,4100,3700],borderColor:'transparent',fill:false,pointRadius:0,tension:0.35},
  {label:'Actual',data:[3200,3400,3650,3900,null,null,null,null,null,null],borderColor:'#3FA9A0',tension:0.3,pointRadius:3},
  {label:'AI Forecast',data:[null,null,null,3900,4250,4600,4950,5100,4800,4400],borderColor:'#FF8A3D',borderDash:[5,4],tension:0.3,pointRadius:3},
]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{labels:{color:'#B9B7AE',font:{size:10}}}},scales:{x:{ticks:{color:'#B9B7AE',font:{size:10}},grid:{color:'rgba(255,255,255,0.04)'}},y:{ticks:{color:'#B9B7AE',font:{size:10}},grid:{color:'rgba(255,255,255,0.04)'}}}}});

// ===================== ALERTS =====================
const alertMsgs=[
  {sev:'high',text:'Zone 4 projected to exceed capacity in ~12 min. Auto-reroute suggested.'},
  {sev:'info',text:'Zone 9 density stabilized. Risk downgraded to MODERATE.'},
  {sev:'high',text:'Sudden inflow spike at Zone 12 entry gate B.'},
  {sev:'info',text:'Forecast model refreshed using latest 5 min sensor data.'},
  {sev:'high',text:'Zone 1 pedestrian flow reversed — possible bottleneck at north gate.'},
];
function pushAlert(){
  const a=alertMsgs[Math.floor(Math.random()*alertMsgs.length)];
  const d=document.createElement('div');d.className='alert-item '+(a.sev==='info'?'info':'');
  d.innerHTML=`${a.text}<div class="time">${new Date().toLocaleTimeString()}</div>`;
  const f=document.getElementById('alert-feed');f.prepend(d);
  while(f.children.length>8)f.removeChild(f.lastChild);
}
pushAlert();pushAlert();

// ===================== BOOKING FLOW =====================
function setStepTab(n){
  for(let i=1;i<=4;i++){
    const t=document.getElementById('step-tab-'+i);t.classList.remove('active','done');
    if(i<n)t.classList.add('done');if(i===n)t.classList.add('active');
  }
}
function nextStep(c){
  if(c===1){
    if(!document.getElementById('p-name').value.trim()||!document.getElementById('p-phone').value.trim()){alert('Please fill in your name and phone number.');return;}
  }
  if(c===2&&!selectedZone){alert('Please select a zone.');return;}
  if(c===3){if(!selectedSlot){alert('Please select a time slot.');return;}finalizeBooking();}
  document.getElementById('bstep-'+c).style.display='none';
  document.getElementById('bstep-'+(c+1)).style.display='block';
  setStepTab(c+1);
}
function prevStep(c){
  document.getElementById('bstep-'+c).style.display='none';
  document.getElementById('bstep-'+(c-1)).style.display='block';
  setStepTab(c-1);
}
function finalizeBooking(){
  const name=document.getElementById('p-name').value.trim();
  const group=document.getElementById('p-group').value;
  const lang=document.getElementById('p-lang').value;
  const token='KS-'+Math.random().toString(36).substring(2,8).toUpperCase();
  bookingLog.unshift({token,name,zone:`Z${selectedZone.id} — ${selectedZone.name}`,slot:selectedSlot,group,lang,status:'confirmed'});
  document.getElementById('confirm-details').innerHTML=`
    <div class="confirm-detail"><span>Pilgrim</span><span>${name}</span></div>
    <div class="confirm-detail"><span>Zone</span><span>Zone ${selectedZone.id} — ${selectedZone.name}</span></div>
    <div class="confirm-detail"><span>Time Slot</span><span>${selectedSlot}</span></div>
    <div class="confirm-detail"><span>Group Size</span><span>${group} persons</span></div>
    <div class="confirm-detail"><span>Language</span><span>${lang}</span></div>
    <div class="confirm-detail"><span>Token</span><span class="mono">${token}</span></div>`;
  document.getElementById('qrcode').innerHTML='';
  new QRCode(document.getElementById('qrcode'),{text:token,width:160,height:160,colorDark:'#0B1020',colorLight:'#ffffff'});
}
function resetBooking(){
  document.getElementById('p-name').value='';document.getElementById('p-phone').value='';
  selectedZone=null;selectedSlot=null;
  document.querySelectorAll('.zone-opt,.slot-opt').forEach(el=>el.classList.remove('selected'));
  for(let i=1;i<=4;i++)document.getElementById('bstep-'+i).style.display=i===1?'block':'none';
  setStepTab(1);
}

// ===================== AI CHAT =====================
const chatBody=document.getElementById('chat-body');
function addMsg(text,who){
  const d=document.createElement('div');d.className='msg '+who;d.textContent=text;
  chatBody.appendChild(d);chatBody.scrollTop=chatBody.scrollHeight;
}
function showTyping(){
  const d=document.createElement('div');d.className='msg bot typing';d.id='typing-indicator';
  d.innerHTML='<span></span><span></span><span></span>';
  chatBody.appendChild(d);chatBody.scrollTop=chatBody.scrollHeight;
}
function hideTyping(){const t=document.getElementById('typing-indicator');if(t)t.remove();}
function aiRespond(q){
  showTyping();
  setTimeout(()=>{
    hideTyping();
    const lower=q.toLowerCase();let reply;
    if(lower.includes('har ki pauri')||lower.includes('zone 4')){
      const z=zones[0],r=riskOf(z);
      reply=`Right now Har Ki Pauri (Zone 4) is at ${Math.round(r*100)}% capacity with ${z.count.toLocaleString()} pilgrims. Our AI forecast shows it ${r>0.7?'peaking dangerously in ~12 minutes — I strongly suggest visiting after 5:00 PM when density drops ~30%.':'at manageable levels. Best time to visit is early morning 6–7 AM.'}`;
    } else if(lower.includes('least crowded')||lower.includes('safest')||lower.includes('safe')){
      const sorted=[...zones].sort((a,b)=>riskOf(a)-riskOf(b));
      reply=`Zone ${sorted[0].id} (${sorted[0].name}) is currently the least crowded at ${Math.round(riskOf(sorted[0])*100)}% capacity. Zone ${sorted[1].id} (${sorted[1].name}) is also safe at ${Math.round(riskOf(sorted[1])*100)}%.`;
    } else if(lower.includes('lost')||lower.includes('child')||lower.includes('missing')){
      reply=`Please go to the nearest Help Desk (marked at every gate) immediately. You can also use the Lost & Found tab to upload a photo — our AI matches it against volunteer sightings in real time. Stay calm and call: 1800-XXX-XXXX (toll-free).`;
    } else if(lower.includes('book')||lower.includes('slot')||lower.includes('darshan')){
      reply=`Use the "Book Slot" tab to reserve a time window with lower predicted density. It takes under a minute and generates a QR code entry pass.`;
    } else if(lower.includes('critical')||lower.includes('danger')){
      const critical=zones.filter(z=>riskOf(z)>0.78);
      reply=critical.length>0?`Currently ${critical.length} zone(s) are CRITICAL: ${critical.map(z=>`Zone ${z.id} (${z.name})`).join(', ')}. Please avoid these areas and use alternate routes.`:`No zones are critical right now. All areas are within safe limits.`;
    } else {
      reply=`Based on live data, Zone 4 and Zone 12 are trending toward moderate risk. I recommend visiting early morning or after 5 PM for the best experience. Ask me about a specific zone or ghat!`;
    }
    addMsg(reply,'bot');
  },900);
}
function sendChat(){
  const inp=document.getElementById('chat-in');const v=inp.value.trim();
  if(!v)return;addMsg(v,'user');inp.value='';aiRespond(v);
}
document.getElementById('chat-in').addEventListener('keypress',e=>{if(e.key==='Enter')sendChat();});
function askChip(el){addMsg(el.textContent,'user');aiRespond(el.textContent);}

// ===================== LOST & FOUND =====================
let uploadedPhotoDataURL=null;
function handlePhotoUpload(e){
  const file=e.target.files[0];
  if(!file)return;
  if(file.size>10*1024*1024){alert('File too large. Max 10MB.');return;}
  const reader=new FileReader();
  reader.onload=ev=>{
    uploadedPhotoDataURL=ev.target.result;
    document.getElementById('photo-preview').src=uploadedPhotoDataURL;
    document.getElementById('photo-preview-wrap').style.display='block';
    const ua=document.getElementById('upload-area');
    ua.querySelector('.upload-icon').style.display='none';
    ua.querySelector('.upload-label').style.display='none';
    ua.querySelector('.upload-sub').style.display='none';
  };
  reader.readAsDataURL(file);
}
function clearPhoto(){
  uploadedPhotoDataURL=null;
  document.getElementById('photo-input').value='';
  document.getElementById('photo-preview-wrap').style.display='none';
  const ua=document.getElementById('upload-area');
  ua.querySelector('.upload-icon').style.display='';
  ua.querySelector('.upload-label').style.display='';
  ua.querySelector('.upload-sub').style.display='';
}
function runAIMatch(){
  if(!uploadedPhotoDataURL){alert('Please upload a photo first.');return;}
  const name=document.getElementById('lf-name').value.trim()||'Unknown';
  const age=document.getElementById('lf-age').value||'?';
  const zone=document.getElementById('lf-zone').value;
  const result=document.getElementById('lf-result');
  result.innerHTML=`<div style="margin-top:14px;color:var(--paper-dim);font-size:0.83rem;padding:12px;background:rgba(255,255,255,0.03);border-radius:9px;">
    ⏳ Running AI face-match against ${Math.floor(Math.random()*800+1200).toLocaleString()} sighting records…</div>`;
  setTimeout(()=>{
    const matched=Math.random()>0.35;
    if(matched){
      const mins=Math.floor(Math.random()*20)+3;
      const helpdesk=['Gate 3 Help Desk','Zone 9 Volunteer Post','Main Camp Help Desk','North Gate Information Centre'][Math.floor(Math.random()*4)];
      result.innerHTML=`<div class="match-result">
        <img src="${uploadedPhotoDataURL}" class="avatar" alt="Uploaded photo">
        <div><strong style="color:var(--green);">Match found — ${70+Math.floor(Math.random()*22)}% confidence</strong><br>
        <span style="color:var(--paper-dim);">Spotted near <strong>${helpdesk}</strong>, ${mins} minutes ago. Nearest volunteer has been notified and is moving to the location. Stay at your current position — help is coming.</span></div>
      </div>`;
      addToReports(name,age,zone,uploadedPhotoDataURL,'found');
    } else {
      result.innerHTML=`<div class="no-match">
        <strong>No match found yet</strong><br>
        <span style="font-size:0.79rem;">Your report has been added to the live database. Volunteers will be alerted if a sighting matches. Call the emergency helpline: <strong>1800-XXX-XXXX</strong></span>
      </div>`;
      addToReports(name,age,zone,uploadedPhotoDataURL,'searching');
    }
  },1800);
}
const lfReportsData=[];
function addToReports(name,age,zone,photo,status){
  lfReportsData.unshift({name,age,zone,photo,status,time:new Date().toLocaleTimeString()});
  renderReports();
}
function renderReports(){
  const el=document.getElementById('lf-reports');
  if(lfReportsData.length===0){el.innerHTML='<div style="color:var(--paper-dim);font-size:0.82rem;padding:10px 0;">No active reports</div>';return;}
  el.innerHTML=lfReportsData.map(r=>`
    <div class="lf-report-item">
      <img src="${r.photo}" style="width:38px;height:38px;border-radius:50%;object-fit:cover;" alt="">
      <div style="flex:1;">
        <div style="font-weight:600;">${r.name}, ${r.age}</div>
        <div style="color:var(--paper-dim);font-size:0.74rem;">${r.zone} · ${r.time}</div>
      </div>
      <span class="badge ${r.status==='found'?'low':'med'}">${r.status==='found'?'FOUND':'SEARCHING'}</span>
    </div>`).join('');
}
renderReports();

// ===================== GOVT ANALYTICS =====================
let analyticsInited=false;
let bookingLog=[];

// seed fake booking log
const fakeNames=['Ramesh Kumar','Priya Devi','Suresh Patel','Anita Sharma','Vikram Singh','Kavitha Rao','Mohan Das','Sunita Jain'];
const fakeLangs=['Hindi','Tamil','English','Marathi','Bengali','Telugu'];
const fakeStatuses=['confirmed','confirmed','confirmed','pending','cancelled'];
for(let i=0;i<25;i++){
  bookingLog.push({
    token:'KS-'+Math.random().toString(36).substring(2,8).toUpperCase(),
    name:fakeNames[Math.floor(Math.random()*fakeNames.length)],
    zone:`Z${Math.ceil(Math.random()*16)} — ${zoneNames[Math.floor(Math.random()*16)]}`,
    slot:slotTimes[Math.floor(Math.random()*slotTimes.length)],
    group:Math.ceil(Math.random()*8),
    lang:fakeLangs[Math.floor(Math.random()*fakeLangs.length)],
    status:fakeStatuses[Math.floor(Math.random()*fakeStatuses.length)],
  });
}

function setDateFilter(btn,period){
  document.querySelectorAll('.date-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  const mul={today:1,week:7,month:30}[period];
  document.getElementById('kpi-bookings')=(4821*mul).toLocaleString();
  document.getElementById('kpi-pilgrims')=(18340*mul).toLocaleString();
}

function initAnalytics(){
  if(analyticsInited)return;
  analyticsInited=true;
  renderBookingsTable();
  buildBookingsChart();
  buildStatusChart();
  buildZoneAnalytics();
  buildLangList();
  buildPeakChart();
}

function renderBookingsTable(){
  const tbody=document.getElementById('bookings-tbody');
  tbody.innerHTML=bookingLog.slice(0,20).map(b=>`
    <tr>
      <td class="mono" style="font-size:0.74rem;">${b.token}</td>
      <td>${b.name}</td>
      <td>${b.zone}</td>
      <td>${b.slot}</td>
      <td style="text-align:center;">${b.group}</td>
      <td>${b.lang}</td>
      <td><span class="status-pill ${b.status}">${b.status.toUpperCase()}</span></td>
    </tr>`).join('');
}

function buildBookingsChart(){
  const ctx=document.getElementById('bookings-chart').getContext('2d');
  const hours=['5AM','6AM','7AM','8AM','9AM','10AM','11AM','12PM','1PM','2PM','3PM','4PM','5PM','6PM','7PM','8PM'];
  const data=[20,180,340,620,810,740,580,430,380,410,490,680,850,720,430,160];
  new Chart(ctx,{type:'bar',data:{labels:hours,datasets:[{label:'Bookings',data,backgroundColor:data.map(v=>v>700?'rgba(226,73,61,0.7)':v>500?'rgba(244,201,93,0.7)':'rgba(63,169,160,0.7)'),borderRadius:5}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{ticks:{color:'#B9B7AE',font:{size:9}},grid:{display:false}},y:{ticks:{color:'#B9B7AE',font:{size:9}},grid:{color:'rgba(255,255,255,0.04)'}}}}});
}

function buildStatusChart(){
  const ctx=document.getElementById('status-chart').getContext('2d');
  new Chart(ctx,{type:'doughnut',data:{labels:['Confirmed','Pending','Cancelled'],datasets:[{data:[3840,720,261],backgroundColor:['rgba(76,175,125,0.8)','rgba(244,201,93,0.8)','rgba(226,73,61,0.8)'],borderWidth:0,hoverOffset:8}]},options:{responsive:true,maintainAspectRatio:false,cutout:'68%',plugins:{legend:{position:'bottom',labels:{color:'#B9B7AE',font:{size:10},padding:14}}}}});
}

function buildZoneAnalytics(){
  const el=document.getElementById('zone-analytics-list');
  const data=zones.map(z=>({...z,bookings:Math.floor(Math.random()*400)+50})).sort((a,b)=>b.bookings-a.bookings);
  const max=data[0].bookings;
  el.innerHTML=data.map(z=>`
    <div class="zone-analytics-item">
      <div style="flex:1;">
        <div style="font-weight:600;font-size:0.79rem;">Z${z.id} · ${z.name}</div>
        <div class="progress-bar-wrap"><div class="progress-bar" style="width:${Math.round(z.bookings/max*100)}%;background:${riskOf(z)>0.78?'var(--danger)':riskOf(z)>0.5?'var(--gold)':'var(--river)'};"></div></div>
      </div>
      <span class="mono" style="margin-left:12px;font-size:0.76rem;color:var(--paper-dim);">${z.bookings}</span>
    </div>`).join('');
}

function buildLangList(){
  const el=document.getElementById('lang-list');
  const langs=[{l:'Hindi',pct:38},{l:'Tamil',pct:21},{l:'English',pct:16},{l:'Marathi',pct:12},{l:'Bengali',pct:8},{l:'Telugu',pct:5}];
  el.innerHTML=langs.map(l=>`
    <div class="lang-row">
      <span class="lang-name">${l.l}</span>
      <div class="lang-bar-wrap"><div class="lang-bar" style="width:${l.pct}%;"></div></div>
      <span class="lang-pct">${l.pct}%</span>
    </div>`).join('');
}

function buildPeakChart(){
  const ctx=document.getElementById('peak-chart').getContext('2d');
  const hours=['5A','6A','7A','8A','9A','10A','11A','12P','1P','2P','3P','4P','5P','6P','7P','8P'];
  new Chart(ctx,{type:'line',data:{labels:hours,datasets:[{label:'Demand',data:[10,60,180,350,480,420,310,240,210,230,280,400,520,410,230,80],borderColor:'var(--gold)',backgroundColor:'rgba(244,201,93,0.12)',fill:true,tension:0.4,pointRadius:2}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{ticks:{color:'#B9B7AE',font:{size:8}},grid:{display:false}},y:{ticks:{color:'#B9B7AE',font:{size:8}},grid:{color:'rgba(255,255,255,0.04)'}}}}});
}

// ===================== EMAIL JS INIT =====================
const EMAILJS_SERVICE_ID  = 'service_kumbhsafe';   // replace after EmailJS signup
const EMAILJS_TEMPLATE_ID = 'template_kumbhsafe';  // replace after EmailJS signup
const EMAILJS_PUBLIC_KEY  = 'YOUR_PUBLIC_KEY';      // replace after EmailJS signup
const GOV_EMAIL           = 'jagathishnataraja@gmail.com';

(function(){
  try{ emailjs.init({publicKey: EMAILJS_PUBLIC_KEY}); } catch(e){}
})();

// ===================== EXPORT CSV =====================
function exportCSV(){
  const btn=document.getElementById('btn-csv');
  btn.textContent='⏳ Generating…';btn.disabled=true;
  setTimeout(()=>{
    const rows=[
      ['KumbhSafe AI — Booking Export'],
      ['Generated:', new Date().toLocaleString()],
      ['Total Bookings:', bookingLog.length],
      [],
      ['Token','Pilgrim','Zone','Slot','Group Size','Language','Status'],
      ...bookingLog.map(b=>[b.token,b.name,b.zone,b.slot,b.group,b.lang,b.status])
    ];
    const csv=rows.map(r=>r.map(c=>`"${c}"`).join(',')).join('\n');
    const a=document.createElement('a');
    a.href='data:text/csv;charset=utf-8,\uFEFF'+encodeURIComponent(csv);
    a.download=`kumbhsafe_bookings_${new Date().toISOString().slice(0,10)}.csv`;
    a.click();
    btn.textContent='✅ Downloaded!';
    setTimeout(()=>{btn.textContent='📥 Export CSV';btn.disabled=false;},2000);
  },400);
}

// ===================== EXPORT PDF =====================
function exportPDF(){
  const btn=document.getElementById('btn-pdf');
  btn.textContent='⏳ Building PDF…';btn.disabled=true;
  setTimeout(()=>{
    try{
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF({orientation:'portrait',unit:'mm',format:'a4'});
      const now = new Date();
      const pageW = doc.internal.pageSize.getWidth();

      // ── Header band ──
      doc.setFillColor(11,16,32);
      doc.rect(0,0,pageW,32,'F');
      doc.setTextColor(255,138,61);
      doc.setFontSize(18);doc.setFont('helvetica','bold');
      doc.text('KumbhSafe AI',14,13);
      doc.setTextColor(179,183,174);
      doc.setFontSize(8);doc.setFont('helvetica','normal');
      doc.text('Predictive Pilgrimage Crowd Intelligence',14,20);
      doc.text('Government Analytics Report',14,26);
      doc.setTextColor(244,201,93);
      doc.setFontSize(7);
      doc.text('RESTRICTED — GOVERNMENT USE ONLY',pageW-14,10,{align:'right'});
      doc.setTextColor(179,183,174);
      doc.text('Report generated: '+now.toLocaleString(),pageW-14,16,{align:'right'});
      doc.text('Recipient: '+GOV_EMAIL,pageW-14,22,{align:'right'});

      // ── KPI Summary ──
      doc.setTextColor(63,169,160);
      doc.setFontSize(10);doc.setFont('helvetica','bold');
      doc.text('SUMMARY DASHBOARD',14,42);
      doc.setDrawColor(63,169,160);doc.setLineWidth(0.4);
      doc.line(14,44,pageW-14,44);

      const kpis=[
        {label:'Total Bookings',value:'4,821',delta:'+12% vs yesterday'},
        {label:'Total Pilgrims',value:'18,340',delta:'+8% vs yesterday'},
        {label:'Incidents Today',value:'3',delta:'2 resolved'},
        {label:'Avg Zone Occupancy',value:'68%',delta:'Within safe range'},
      ];
      const kpiX=[14,62,110,158];
      kpis.forEach((k,i)=>{
        const x=kpiX[i];
        doc.setFillColor(18,26,48);
        doc.roundedRect(x,47,44,22,2,2,'F');
        doc.setTextColor(255,138,61);
        doc.setFontSize(13);doc.setFont('helvetica','bold');
        doc.text(k.value,x+22,57,{align:'center'});
        doc.setTextColor(179,183,174);
        doc.setFontSize(6);doc.setFont('helvetica','normal');
        doc.text(k.label.toUpperCase(),x+22,62,{align:'center'});
        doc.setTextColor(76,175,125);
        doc.setFontSize(6);
        doc.text(k.delta,x+22,67,{align:'center'});
      });

      // ── Zone Risk Table ──
      doc.setTextColor(63,169,160);
      doc.setFontSize(10);doc.setFont('helvetica','bold');
      doc.text('LIVE ZONE STATUS',14,80);
      doc.line(14,82,pageW-14,82);

      const zoneRows=[...zones].sort((a,b)=>riskOf(b)-riskOf(a)).map(z=>{
        const r=riskOf(z);
        return[
          'Zone '+z.id,
          z.name,
          z.count.toLocaleString(),
          z.capacity.toLocaleString(),
          Math.round(r*100)+'%',
          riskLabel(r).txt
        ];
      });
      doc.autoTable({
        startY:85,
        head:[['Zone','Name','Current Count','Capacity','Occupancy','Risk Level']],
        body:zoneRows,
        theme:'grid',
        headStyles:{fillColor:[11,16,32],textColor:[255,138,61],fontSize:7,fontStyle:'bold'},
        bodyStyles:{fontSize:7,textColor:[220,216,210],fillColor:[18,26,48]},
        alternateRowStyles:{fillColor:[27,39,66]},
        columnStyles:{5:{fontStyle:'bold'}},
        didParseCell:(d)=>{
          if(d.section==='body'&&d.column.index===5){
            if(d.cell.raw==='CRITICAL')d.cell.styles.textColor=[226,73,61];
            else if(d.cell.raw==='MODERATE')d.cell.styles.textColor=[244,201,93];
            else d.cell.styles.textColor=[76,175,125];
          }
        },
        margin:{left:14,right:14}
      });

      // ── Bookings Table ──
      const finalY = doc.lastAutoTable.finalY+10;
      doc.setTextColor(63,169,160);
      doc.setFontSize(10);doc.setFont('helvetica','bold');
      doc.text('RECENT BOOKING LOG',14,finalY);
      doc.line(14,finalY+2,pageW-14,finalY+2);

      const bRows=bookingLog.slice(0,20).map(b=>[b.token,b.name,b.zone,b.slot,b.group,b.lang,b.status.toUpperCase()]);
      doc.autoTable({
        startY:finalY+6,
        head:[['Token','Pilgrim','Zone','Slot','Grp','Language','Status']],
        body:bRows,
        theme:'grid',
        headStyles:{fillColor:[11,16,32],textColor:[255,138,61],fontSize:6.5,fontStyle:'bold'},
        bodyStyles:{fontSize:6.5,textColor:[220,216,210],fillColor:[18,26,48]},
        alternateRowStyles:{fillColor:[27,39,66]},
        didParseCell:(d)=>{
          if(d.section==='body'&&d.column.index===6){
            if(d.cell.raw==='CONFIRMED')d.cell.styles.textColor=[76,175,125];
            else if(d.cell.raw==='PENDING')d.cell.styles.textColor=[244,201,93];
            else d.cell.styles.textColor=[226,73,61];
          }
        },
        margin:{left:14,right:14}
      });

      // ── Footer ──
      const pages=doc.internal.getNumberOfPages();
      for(let i=1;i<=pages;i++){
        doc.setPage(i);
        doc.setFillColor(11,16,32);
        doc.rect(0,doc.internal.pageSize.getHeight()-12,pageW,12,'F');
        doc.setTextColor(179,183,174);doc.setFontSize(6);doc.setFont('helvetica','normal');
        doc.text('KumbhSafe AI — Confidential Government Report',14,doc.internal.pageSize.getHeight()-5);
        doc.text('Page '+i+' of '+pages,pageW-14,doc.internal.pageSize.getHeight()-5,{align:'right'});
      }

      doc.save(`KumbhSafe_Report_${now.toISOString().slice(0,10)}.pdf`);
      btn.textContent='✅ PDF Downloaded!';
      setTimeout(()=>{btn.textContent='📄 Download Report PDF';btn.disabled=false;},2500);
    } catch(err){
      console.error(err);
      btn.textContent='❌ Error — retry';btn.disabled=false;
    }
  },300);
}

// ===================== EMAIL REPORT =====================
function sendEmailReport(){
  const btn=document.getElementById('btn-email');
  btn.textContent='⏳ Sending…';btn.disabled=true;

  const now=new Date().toLocaleString();
  const critical=zones.filter(z=>riskOf(z)>0.78);
  const moderate=zones.filter(z=>riskOf(z)>0.5&&riskOf(z)<=0.78);
  const safe=zones.filter(z=>riskOf(z)<=0.5);
  const topZone=[...zones].sort((a,b)=>riskOf(b)-riskOf(a))[0];

  const summaryLines=`
KumbhSafe AI — Government Analytics Report
Generated: ${now}
─────────────────────────────────────
SUMMARY
  Total Bookings Today : 4,821 (+12% vs yesterday)
  Total Pilgrims       : 18,340 (+8% vs yesterday)
  Incidents            : 3 (2 resolved)
  Avg Occupancy        : 68%

ZONE STATUS
  🔴 Critical Zones (${critical.length}): ${critical.map(z=>`Zone ${z.id} ${z.name}`).join(', ')||'None'}
  🟡 Moderate Zones (${moderate.length}): ${moderate.map(z=>`Zone ${z.id}`).join(', ')||'None'}
  🟢 Safe Zones      (${safe.length}): All others

HIGHEST RISK ZONE RIGHT NOW
  Zone ${topZone.id} — ${topZone.name}
  Current Count : ${topZone.count.toLocaleString()} / ${topZone.capacity.toLocaleString()}
  Occupancy     : ${Math.round(riskOf(topZone)*100)}%

LANGUAGE DISTRIBUTION
  Hindi 38% · Tamil 21% · English 16% · Marathi 12% · Bengali 8% · Telugu 5%

RECENT BOOKINGS (last 5)
${bookingLog.slice(0,5).map(b=>`  ${b.token} | ${b.name} | ${b.zone} | ${b.slot} | ${b.status.toUpperCase()}`).join('\n')}

─────────────────────────────────────
This report was auto-generated by KumbhSafe AI.
For live dashboard access: open the KumbhSafe portal.
  `.trim();

  // Try EmailJS first — falls back to mailto if credentials not set
  const emailjsConfigured = EMAILJS_PUBLIC_KEY !== 'YOUR_PUBLIC_KEY';

  if(emailjsConfigured){
    emailjs.send(EMAILJS_SERVICE_ID, EMAILJS_TEMPLATE_ID, {
      to_email  : GOV_EMAIL,
      to_name   : 'KumbhSafe Government Admin',
      subject   : `KumbhSafe AI — Analytics Report ${new Date().toLocaleDateString()}`,
      message   : summaryLines,
      from_name : 'KumbhSafe AI System',
      reply_to  : GOV_EMAIL,
    }).then(()=>{
      btn.textContent='✅ Email Sent!';
      showEmailToast('Report emailed to '+GOV_EMAIL,'success');
      setTimeout(()=>{btn.textContent='📧 Email Report to Gov';btn.disabled=false;},3000);
    }).catch(err=>{
      console.error(err);
      fallbackMailto(summaryLines, btn);
    });
  } else {
    // Fallback: opens Gmail compose with full report pre-filled
    fallbackMailto(summaryLines, btn);
  }
}

function fallbackMailto(body, btn){
  const subject=encodeURIComponent('KumbhSafe AI — Analytics Report '+new Date().toLocaleDateString());
  const bodyEnc=encodeURIComponent(body);
  const mailto=`mailto:${GOV_EMAIL}?subject=${subject}&body=${bodyEnc}`;
  const a=document.createElement('a');a.href=mailto;a.target='_blank';a.click();
  btn.textContent='✅ Gmail Opened!';
  showEmailToast('Gmail opened — report pre-filled. Click Send in your Gmail tab.','info');
  setTimeout(()=>{btn.textContent='📧 Email Report to Gov';btn.disabled=false;},3000);
}

function showEmailToast(msg,type){
  const t=document.createElement('div');
  t.style.cssText=`position:fixed;bottom:28px;right:28px;z-index:9999;
    padding:14px 20px;border-radius:12px;font-size:0.86rem;max-width:360px;
    background:${type==='success'?'rgba(76,175,125,0.15)':'rgba(63,169,160,0.15)'};
    border:1px solid ${type==='success'?'var(--green)':'var(--river)'};
    color:var(--paper);backdrop-filter:blur(12px);box-shadow:0 8px 32px rgba(0,0,0,0.4);`;
  t.textContent=(type==='success'?'✅ ':'📬 ')+msg;
  document.body.appendChild(t);
  setTimeout(()=>t.remove(),5000);
}
</script>
</body>
</html>
