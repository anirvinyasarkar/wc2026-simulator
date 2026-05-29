
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FIFA World Cup 2026 — Money Simulator</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=IBM+Plex+Mono:wght@400;500&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
<style>
:root{--bg:#0a0a0a;--surface:#111;--surface2:#1a1a1a;--border:rgba(255,255,255,.08);--border2:rgba(255,255,255,.15);--text:#f0f0f0;--muted:#888;--accent:#c9f500;--gold:#f5c842;--blue:#4da6ff;--green:#00e676;--red:#ff5252;--purple:#b39dff;}
*{margin:0;padding:0;box-sizing:border-box;}
body{background:var(--bg);color:var(--text);font-family:'Inter',sans-serif;font-size:14px;line-height:1.6;min-height:100vh;}
.header{border-bottom:1px solid var(--border);padding:14px 24px;display:flex;align-items:center;justify-content:space-between;}
.logo{font-family:'Syne',sans-serif;font-size:13px;font-weight:700;letter-spacing:.08em;color:var(--muted);text-transform:uppercase;}
.logo span{color:var(--accent);}
.header-right{display:flex;align-items:center;gap:12px;}
.live-badge{display:flex;align-items:center;gap:6px;font-size:11px;font-weight:500;color:var(--green);font-family:'IBM Plex Mono',monospace;}
.live-dot{width:7px;height:7px;border-radius:50%;background:var(--green);animation:pulse 1.5s infinite;}
@keyframes pulse{0%,100%{opacity:1;transform:scale(1);}50%{opacity:.4;transform:scale(.8);}}
.refresh-btn{display:flex;align-items:center;gap:6px;background:rgba(201,245,0,.1);border:1px solid rgba(201,245,0,.25);border-radius:20px;padding:5px 14px;font-family:'IBM Plex Mono',monospace;font-size:11px;color:var(--accent);cursor:pointer;transition:all .15s;}
.refresh-btn:hover{background:rgba(201,245,0,.2);}
.refresh-btn:disabled{opacity:.4;cursor:not-allowed;}
.refresh-btn .spin{display:inline-block;}
.refresh-btn.loading .spin{animation:spin .8s linear infinite;}
@keyframes spin{to{transform:rotate(360deg);}}
.last-updated{font-family:'IBM Plex Mono',monospace;font-size:10px;color:var(--muted);}

.boot-overlay{position:fixed;inset:0;background:var(--bg);z-index:100;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:20px;transition:opacity .4s;}
.boot-overlay.hidden{opacity:0;pointer-events:none;}
.boot-logo{font-family:'Syne',sans-serif;font-size:22px;font-weight:800;letter-spacing:-.5px;}
.boot-logo span{color:var(--accent);}
.boot-status{font-family:'IBM Plex Mono',monospace;font-size:12px;color:var(--muted);text-align:center;max-width:360px;line-height:1.8;}
.boot-bar-wrap{width:280px;height:3px;background:var(--surface2);border-radius:2px;overflow:hidden;}
.boot-bar-fill{height:100%;background:var(--accent);border-radius:2px;width:0%;transition:width .3s ease;}
.boot-step{color:var(--accent);}

.hero{padding:40px 24px 28px;text-align:center;border-bottom:1px solid var(--border);}
.hero-eyebrow{font-family:'IBM Plex Mono',monospace;font-size:11px;color:var(--muted);letter-spacing:.1em;text-transform:uppercase;margin-bottom:10px;}
.hero-number{font-family:'Syne',sans-serif;font-size:clamp(48px,9vw,80px);font-weight:800;line-height:1;letter-spacing:-2px;}
.hero-number .currency{font-size:.45em;vertical-align:super;color:var(--accent);}
.hero-sub{font-size:13px;color:var(--muted);margin-top:8px;}
.hero-update{display:inline-block;margin-top:8px;font-family:'IBM Plex Mono',monospace;font-size:10px;color:var(--muted);background:var(--surface);border:1px solid var(--border);border-radius:20px;padding:3px 10px;}

.ticker-strip{border-bottom:1px solid var(--border);padding:9px 0;overflow:hidden;background:var(--surface);}
.ticker-inner{display:flex;gap:48px;animation:ticker 35s linear infinite;white-space:nowrap;width:max-content;}
.ticker-item{font-family:'IBM Plex Mono',monospace;font-size:11px;color:var(--muted);display:flex;gap:8px;}
.ticker-item .val{color:var(--accent);font-weight:500;}
.ticker-item .up{color:var(--green);}
.ticker-item .dn{color:var(--red);}
@keyframes ticker{from{transform:translateX(0);}to{transform:translateX(-50%);}}

.main{max-width:1080px;margin:0 auto;padding:28px 24px;}
.nav-tabs{display:flex;gap:4px;flex-wrap:wrap;margin-bottom:24px;border-bottom:1px solid var(--border);padding-bottom:0;}
.nav-tab{font-family:'Syne',sans-serif;font-size:12px;font-weight:600;letter-spacing:.05em;text-transform:uppercase;padding:9px 14px;border:none;background:none;color:var(--muted);cursor:pointer;border-bottom:2px solid transparent;margin-bottom:-1px;transition:all .15s;}
.nav-tab:hover{color:var(--text);}
.nav-tab.active{color:var(--accent);border-bottom-color:var(--accent);}
.tab-content{display:none;}
.tab-content.active{display:block;}

.grid-4{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:10px;margin-bottom:22px;}
.grid-3{display:grid;grid-template-columns:repeat(auto-fit,minmax(190px,1fr));gap:10px;margin-bottom:22px;}
.card{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:18px;}
.card-sm{padding:13px 15px;}
.stat-label{font-family:'IBM Plex Mono',monospace;font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:.07em;margin-bottom:5px;}
.stat-value{font-family:'Syne',sans-serif;font-size:26px;font-weight:700;line-height:1;}
.stat-note{font-size:11px;color:var(--muted);margin-top:3px;}
.stat-change{font-family:'IBM Plex Mono',monospace;font-size:10px;margin-top:3px;}
.stat-change.up{color:var(--green);}
.stat-change.dn{color:var(--red);}

.section-title{font-family:'Syne',sans-serif;font-size:16px;font-weight:700;color:var(--text);margin-bottom:14px;display:flex;align-items:center;gap:10px;}
.section-title::after{content:'';flex:1;height:1px;background:var(--border);}

.bar-block{margin-bottom:13px;}
.bar-header{display:flex;justify-content:space-between;align-items:baseline;margin-bottom:4px;}
.bar-name{font-size:13px;font-weight:500;}
.bar-amount{font-family:'IBM Plex Mono',monospace;font-size:12px;}
.bar-track{height:8px;background:var(--surface2);border-radius:4px;overflow:hidden;}
.bar-fill{height:100%;border-radius:4px;transition:width .6s cubic-bezier(.4,0,.2,1);}
.bar-sub{font-size:11px;color:var(--muted);margin-top:3px;}

.flow-table{width:100%;border-collapse:collapse;}
.flow-table th{font-family:'IBM Plex Mono',monospace;font-size:10px;text-transform:uppercase;letter-spacing:.07em;color:var(--muted);text-align:left;padding:7px 11px;border-bottom:1px solid var(--border);}
.flow-table td{padding:10px 11px;border-bottom:1px solid var(--border);font-size:13px;vertical-align:top;}
.flow-table tr:last-child td{border-bottom:none;}
.flow-table .amt{font-family:'IBM Plex Mono',monospace;font-weight:500;white-space:nowrap;}
.flow-table .pos{color:var(--green);}
.flow-table .neg{color:var(--red);}
.flow-table .neu{color:var(--gold);}
.flow-table .note{font-size:11px;color:var(--muted);display:block;margin-top:2px;}

.prize-row{display:flex;align-items:center;padding:9px 0;border-bottom:1px solid var(--border);gap:12px;}
.prize-row:last-child{border-bottom:none;}
.prize-rank{font-family:'IBM Plex Mono',monospace;font-size:11px;color:var(--muted);min-width:80px;}
.prize-bar-wrap{flex:1;}
.prize-bar-fill{height:6px;border-radius:3px;background:var(--gold);}
.prize-context{font-size:10px;color:var(--muted);margin-top:2px;}
.prize-amount{font-family:'IBM Plex Mono',monospace;font-weight:500;font-size:13px;color:var(--gold);min-width:50px;text-align:right;}

.counter-display{background:var(--surface);border:1px solid var(--border);border-radius:16px;padding:28px;text-align:center;margin-bottom:20px;}
.counter-main{font-family:'Syne',sans-serif;font-size:clamp(38px,7vw,64px);font-weight:800;color:var(--accent);letter-spacing:-2px;line-height:1;}
.counter-label{font-size:13px;color:var(--muted);margin-top:6px;}
.slider-wrap{display:flex;align-items:center;gap:14px;margin:18px 0;}
.slider-label{font-family:'IBM Plex Mono',monospace;font-size:11px;color:var(--muted);white-space:nowrap;}
input[type=range]{flex:1;-webkit-appearance:none;height:4px;background:var(--surface2);border-radius:2px;outline:none;}
input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;width:18px;height:18px;border-radius:50%;background:var(--accent);cursor:pointer;border:2px solid var(--bg);}
.slider-val{font-family:'IBM Plex Mono',monospace;font-size:13px;font-weight:500;color:var(--accent);min-width:50px;}

.insight-card{background:var(--surface2);border:1px solid var(--border);border-left:3px solid var(--accent);border-radius:0 10px 10px 0;padding:13px 16px;font-size:13px;color:var(--muted);line-height:1.7;margin-top:14px;}
.insight-card strong{color:var(--text);font-weight:500;}

.news-feed{display:flex;flex-direction:column;gap:10px;margin-bottom:20px;}
.news-item{background:var(--surface);border:1px solid var(--border);border-radius:10px;padding:14px 16px;}
.news-item-header{display:flex;align-items:center;gap:8px;margin-bottom:6px;}
.news-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0;}
.news-source{font-family:'IBM Plex Mono',monospace;font-size:10px;color:var(--muted);}
.news-date{font-family:'IBM Plex Mono',monospace;font-size:10px;color:var(--muted);margin-left:auto;}
.news-headline{font-size:13px;font-weight:500;color:var(--text);margin-bottom:4px;line-height:1.5;}
.news-insight{font-size:12px;color:var(--muted);line-height:1.5;}
.news-number{font-family:'IBM Plex Mono',monospace;font-size:11px;font-weight:500;margin-top:5px;}

.loading-state{display:flex;align-items:center;gap:10px;padding:20px 0;color:var(--muted);font-family:'IBM Plex Mono',monospace;font-size:12px;}
.dot-anim span{display:inline-block;animation:dots .8s infinite;opacity:0;}
.dot-anim span:nth-child(2){animation-delay:.2s;}
.dot-anim span:nth-child(3){animation-delay:.4s;}
@keyframes dots{0%,100%{opacity:0;}50%{opacity:1;}}

.comparison-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(110px,1fr));gap:1px;background:var(--border);border:1px solid var(--border);border-radius:12px;overflow:hidden;margin-bottom:20px;}
.cmp-cell{background:var(--surface);padding:13px 14px;text-align:center;}
.cmp-label{font-size:10px;color:var(--muted);margin-bottom:3px;}
.cmp-val{font-family:'Syne',sans-serif;font-size:15px;font-weight:700;}
.cmp-sub{font-family:'IBM Plex Mono',monospace;font-size:9px;color:var(--muted);margin-top:2px;}

.source-line{font-family:'IBM Plex Mono',monospace;font-size:10px;color:var(--muted);margin-top:28px;padding-top:14px;border-top:1px solid var(--border);opacity:.6;}
.watermark{text-align:center;padding:16px;font-family:'Syne',sans-serif;font-size:11px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:var(--muted);border-top:1px solid var(--border);}
.watermark span{color:var(--accent);}

@media(max-width:600px){.hero-number{font-size:48px;}.main{padding:18px 14px;}.nav-tab{font-size:11px;padding:8px 10px;}.flow-table{font-size:12px;}}
</style>
</head>
<body>

<div class="boot-overlay" id="boot">
  <div class="boot-logo">Sports Economics <span>Observatory</span></div>
  <div class="boot-bar-wrap"><div class="boot-bar-fill" id="boot-bar"></div></div>
  <div class="boot-status" id="boot-status">Connecting to live data feed<span class="dot-anim"><span>.</span><span>.</span><span>.</span></span></div>
</div>

<header class="header">
  <div class="logo">Sports Economics <span>Observatory</span></div>
  <div class="header-right">
    <div class="last-updated" id="last-updated">Fetching latest data...</div>
    <button class="refresh-btn" id="refresh-btn" onclick="refreshAllData()" disabled>
      <span class="spin">↻</span> Refresh data
    </button>
    <div class="live-badge"><div class="live-dot"></div>WC2026</div>
  </div>
</header>

<div class="hero">
  <div class="hero-eyebrow">FIFA World Cup 2026 · Total economic footprint</div>
  <div class="hero-number"><span class="currency">$</span><span id="hero-val">—</span></div>
  <div class="hero-sub" id="hero-sub">Loading latest figures...</div>
  <div class="hero-update" id="hero-update">Last updated: fetching...</div>
</div>

<div class="ticker-strip"><div class="ticker-inner" id="ticker"></div></div>

<div class="main">
  <div class="nav-tabs">
    <button class="nav-tab active" onclick="switchTab('overview',this)">Overview</button>
    <button class="nav-tab" onclick="switchTab('revenue',this)">Revenue</button>
    <button class="nav-tab" onclick="switchTab('spend',this)">FIFA spend</button>
    <button class="nav-tab" onclick="switchTab('prize',this)">Prize money</button>
    <button class="nav-tab" onclick="switchTab('flow',this)">Money flow</button>
    <button class="nav-tab" onclick="switchTab('counter',this)">Live counter</button>
    <button class="nav-tab" onclick="switchTab('updates',this)">Latest updates</button>
  </div>

  <div id="tab-overview" class="tab-content active">
    <div class="grid-4" id="overview-stats"></div>
    <div class="section-title">Context — what this compares to</div>
    <div class="comparison-grid">
      <div class="cmp-cell"><div class="cmp-label">FIFA 2026</div><div class="cmp-val" style="color:var(--accent)" id="cmp-fifa">$11B</div><div class="cmp-sub">39 days</div></div>
      <div class="cmp-cell"><div class="cmp-label">Paris Olympics 2024</div><div class="cmp-val" style="color:var(--blue)">$4.8B</div><div class="cmp-sub">17 days</div></div>
      <div class="cmp-cell"><div class="cmp-label">NFL full season</div><div class="cmp-val" style="color:var(--gold)">$21B</div><div class="cmp-sub">Per year</div></div>
      <div class="cmp-cell"><div class="cmp-label">Qatar 2022</div><div class="cmp-val" style="color:var(--muted)">$7.5B</div><div class="cmp-sub">Previous WC</div></div>
      <div class="cmp-cell"><div class="cmp-label">Cape Verde GDP</div><div class="cmp-val" style="color:var(--red)">$2.1B</div><div class="cmp-sub">Whole country</div></div>
      <div class="cmp-cell"><div class="cmp-label">Global ad spend boost</div><div class="cmp-val" style="color:var(--green)" id="cmp-ads">$10.5B</div><div class="cmp-sub">Added Q2 2026</div></div>
    </div>
    <div class="insight-card" id="overview-insight">Loading economic analysis...</div>
  </div>

  <div id="tab-revenue" class="tab-content">
    <div class="grid-4" id="rev-stats"></div>
    <div class="section-title">Revenue breakdown</div>
    <div id="rev-bars"></div>
    <div class="insight-card" id="rev-insight">Loading...</div>
  </div>

  <div id="tab-spend" class="tab-content">
    <div class="grid-4" id="spend-stats"></div>
    <div class="section-title">Expenditure allocation</div>
    <div id="spend-bars"></div>
    <div class="insight-card" id="spend-insight">Loading...</div>
  </div>

  <div id="tab-prize" class="tab-content">
    <div class="grid-4" id="prize-stats"></div>
    <div class="section-title">Prize money by finish</div>
    <div id="prize-rows"></div>
    <div class="insight-card" id="prize-insight">Loading...</div>
  </div>

  <div id="tab-flow" class="tab-content">
    <div class="section-title">The full money flow — from source to final recipient</div>
    <table class="flow-table"><thead><tr><th>From</th><th>To</th><th>Amount</th><th>Context</th></tr></thead><tbody id="flow-rows"></tbody></table>
    <div class="insight-card" id="flow-insight">Loading...</div>
  </div>

  <div id="tab-counter" class="tab-content">
    <div class="counter-display">
      <div style="font-family:'IBM Plex Mono',monospace;font-size:10px;color:var(--muted);margin-bottom:8px;letter-spacing:.08em;text-transform:uppercase">FIFA revenue earned since tournament kickoff</div>
      <div class="counter-main" id="cnt-main">$0</div>
      <div class="counter-label" id="cnt-sub">Tournament: June 11 – July 19, 2026</div>
    </div>
    <div class="grid-3">
      <div class="card card-sm"><div class="stat-label">Per second of tournament</div><div class="stat-value" style="color:var(--accent);font-size:20px" id="cnt-sec">—</div></div>
      <div class="card card-sm"><div class="stat-label">Per match played</div><div class="stat-value" style="color:var(--gold);font-size:20px" id="cnt-match">—</div></div>
      <div class="card card-sm"><div class="stat-label">Club benefit (total so far)</div><div class="stat-value" style="color:var(--red);font-size:20px" id="cnt-club">—</div></div>
    </div>
    <div class="slider-wrap">
      <span class="slider-label">Day 0</span>
      <input type="range" id="day-slider" min="0" max="38" step="1" value="0" oninput="updateCounter(+this.value)">
      <span class="slider-label">Day 38</span>
      <span class="slider-val" id="slider-out">Day 0</span>
    </div>
    <div class="insight-card" id="counter-insight">Drag the slider to move through the tournament and watch the billions accumulate.</div>
  </div>

  <div id="tab-updates" class="tab-content">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:16px;">
      <div class="section-title" style="margin-bottom:0">Latest economics updates</div>
      <button class="refresh-btn" onclick="fetchLatestNews()" id="news-refresh-btn"><span class="spin">↻</span> Refresh</button>
    </div>
    <div id="news-feed-container">
      <div class="loading-state"><span class="spin">↻</span> Searching for latest updates<span class="dot-anim"><span>.</span><span>.</span><span>.</span></span></div>
    </div>
    <div class="insight-card" style="margin-top:16px">This tab searches the web for the latest FIFA World Cup 2026 economic news every time you refresh. New sponsor announcements, revenue updates, hotel occupancy data, prize money changes — all surfaced automatically.</div>
  </div>

  <div class="source-line" id="source-line">Sources: FIFA Annual Report 2025 · Sports Value · Ampere Analysis · CNBC · SI.com · Refreshed automatically on page load.</div>
</div>

<div class="watermark">The Sports Economics <span>Observatory</span> &nbsp;·&nbsp; Analyzing the game beyond the field</div>

<script>
const FALLBACK = {
  totalFootprint: 13.9,
  fifaRevenue: 11,
  broadcastRights: 4.26,
  ticketHospitality: 3.10,
  sponsorship: 2.80,
  fifaForward: 0.65,
  other: 0.19,
  totalExpenditure: 3.76,
  operations: 1.12,
  prizesContributions: 1.02,
  teamServices: 0.81,
  venues: 0.42,
  commsMarketing: 0.30,
  admin: 0.09,
  prizePool: 655,
  winnerPrize: 50,
  runnerUpPrize: 33,
  thirdPlacePrize: 29,
  fourthPlacePrize: 27,
  quarterFinalPrize: 19,
  roundOf16Prize: 15,
  roundOf32Prize: 11,
  groupStagePrize: 9,
  clubBenefit: 72,
  totalTeamContributions: 727,
  usGdpBoost: 17.2,
  adSpendBoost: 10.5,
  lastUpdated: "May 2026",
  overviewInsight: "FIFA earns 93% of its $11B revenue through contracts signed before a single match is played. The outcome on the pitch is irrelevant to FIFA's financial result. They have already won.",
  revInsight: "Broadcast rights at $4.26B represent 39% of total revenue — and were sold across 200+ territories before the tournament began. US rights alone rose 94% vs Qatar.",
  spendInsight: "Zero new stadiums were built for 2026. Unlike Qatar's $220B infrastructure spend, all venues were pre-existing — dramatically increasing the proportion of spending that flows to teams and operations.",
  prizeInsight: "The $655M prize pool is 50% larger than Qatar 2022's $440M. But the clubs that developed every player here receive just $72M through the Club Benefit Programme — 0.65% of FIFA's total revenue.",
  flowInsight: "The structural injustice: clubs spent years and millions developing every player in this tournament. Their total compensation is $72M from an $11B revenue event — less than what a single Premier League club spends on wages in a month.",
  newsItems: []
};

let DATA = {...FALLBACK};

const SYSTEM_PROMPT = `You are a sports economics data extraction assistant. Search the web for the latest FIFA World Cup 2026 financial and economic news. Extract the most current figures available. 

Return ONLY a valid JSON object with NO markdown, NO backticks, NO explanation — just raw JSON starting with { and ending with }. Use this exact structure:

{
  "totalFootprint": <number in billions, e.g. 13.9>,
  "fifaRevenue": <number in billions>,
  "broadcastRights": <number in billions>,
  "ticketHospitality": <number in billions>,
  "sponsorship": <number in billions>,
  "fifaForward": <number in billions>,
  "other": <number in billions>,
  "totalExpenditure": <number in billions>,
  "operations": <number in billions>,
  "prizesContributions": <number in billions>,
  "teamServices": <number in billions>,
  "venues": <number in billions>,
  "commsMarketing": <number in billions>,
  "admin": <number in billions>,
  "prizePool": <number in millions>,
  "winnerPrize": <number in millions>,
  "runnerUpPrize": <number in millions>,
  "thirdPlacePrize": <number in millions>,
  "fourthPlacePrize": <number in millions>,
  "quarterFinalPrize": <number in millions>,
  "roundOf16Prize": <number in millions>,
  "roundOf32Prize": <number in millions>,
  "groupStagePrize": <number in millions>,
  "clubBenefit": <number in millions>,
  "totalTeamContributions": <number in millions>,
  "usGdpBoost": <number in billions>,
  "adSpendBoost": <number in billions>,
  "lastUpdated": "<current month and year>",
  "overviewInsight": "<1-2 sentence sharp economic insight about the overall picture>",
  "revInsight": "<1-2 sentence insight about revenue>",
  "spendInsight": "<1-2 sentence insight about FIFA spending>",
  "prizeInsight": "<1-2 sentence insight about prize money>",
  "flowInsight": "<1-2 sentence insight about money flow and who gets paid>",
  "newsItems": [
    {
      "headline": "<news headline>",
      "source": "<source name>",
      "date": "<date>",
      "insight": "<1-sentence economic significance>",
      "number": "<key number from the story>",
      "type": "<one of: revenue|prize|sponsor|tourism|clubs|general>"
    }
  ]
}

Search for: FIFA World Cup 2026 revenue sponsorship prize money economics latest news. Include up to 6 news items. Use the most current data available. If a figure hasn't changed, use the known value.`;

async function callClaude(prompt, useSearch = false) {
  const body = {
    model: 'claude-sonnet-4-20250514',
    max_tokens: 1000,
    system: SYSTEM_PROMPT,
    messages: [{role: 'user', content: prompt}]
  };
  if (useSearch) {
    body.tools = [{type: 'web_search_20250305', name: 'web_search'}];
  }
  const res = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {'Content-Type': 'application/json'},
    body: JSON.stringify(body)
  });
  const data = await res.json();
  const text = data.content?.filter(c => c.type === 'text').map(c => c.text).join('') || '';
  return text;
}

function parseData(raw) {
  try {
    const clean = raw.replace(/```json|```/g, '').trim();
    const start = clean.indexOf('{');
    const end = clean.lastIndexOf('}');
    if (start === -1 || end === -1) return null;
    return JSON.parse(clean.slice(start, end + 1));
  } catch(e) { return null; }
}

function fmtB(n) { return '$' + (Math.round(n * 100) / 100).toFixed(2) + 'B'; }
function fmtM(n) { return '$' + Math.round(n) + 'M'; }
function fmtBig(n) {
  if (n >= 1e9) return '$' + (n / 1e9).toFixed(2) + 'B';
  if (n >= 1e6) return '$' + Math.round(n / 1e6).toLocaleString() + 'M';
  return '$' + Math.round(n / 1e3).toLocaleString() + 'K';
}

function renderAll() {
  const D = DATA;
  document.getElementById('hero-val').textContent = D.totalFootprint;
  document.getElementById('hero-sub').textContent = '104 matches · 48 nations · 16 cities · 3 countries · Jun 11 – Jul 19, 2026';
  document.getElementById('hero-update').textContent = 'Last updated: ' + D.lastUpdated;
  document.getElementById('cmp-fifa').textContent = fmtB(D.fifaRevenue);
  document.getElementById('cmp-ads').textContent = fmtB(D.adSpendBoost);

  document.getElementById('overview-stats').innerHTML = [
    {label:'FIFA total revenue',val:fmtB(D.fifaRevenue),note:'2023–26 cycle',color:'var(--accent)'},
    {label:'Prize pool',val:fmtM(D.prizePool),note:'+50% vs Qatar 2022',color:'var(--gold)'},
    {label:'Club benefit total',val:fmtM(D.clubBenefit),note:((D.clubBenefit/(D.fifaRevenue*1000))*100).toFixed(1)+'% of FIFA revenue',color:'var(--red)'},
    {label:'New stadiums built',val:'$0',note:'All pre-existing',color:'var(--green)'},
  ].map(s => `<div class="card card-sm"><div class="stat-label">${s.label}</div><div class="stat-value" style="color:${s.color}">${s.val}</div><div class="stat-note">${s.note}</div></div>`).join('');
  document.getElementById('overview-insight').innerHTML = D.overviewInsight;

  const revData = [
    {name:'Broadcast rights',val:D.broadcastRights,max:D.broadcastRights,color:'#4da6ff',sub:'Sold across 200+ territories before kickoff. US rights up 94% vs Qatar.'},
    {name:'Tickets & hospitality',val:D.ticketHospitality,max:D.broadcastRights,color:'#00e676',sub:'500M ticket requests. 216% increase vs Qatar 2022.'},
    {name:'Sponsorship (all 16 slots sold)',val:D.sponsorship,max:D.broadcastRights,color:'#f5c842',sub:'Adidas, Visa, Coca-Cola, BofA, Diageo, DoorDash + others. 52% from US companies.'},
    {name:'FIFA Forward + other',val:(D.fifaForward+D.other),max:D.broadcastRights,color:'#7f77dd',sub:'$1.5M/year baseline to every one of 211 member associations.'},
  ];
  document.getElementById('rev-stats').innerHTML = [
    {label:'Total revenue',val:fmtB(D.fifaRevenue),note:'Full cycle',color:'var(--accent)'},
    {label:'Broadcast rights',val:fmtB(D.broadcastRights),note:'Largest stream',color:'var(--blue)'},
    {label:'Sponsorship',val:fmtB(D.sponsorship),note:'Sold out',color:'var(--gold)'},
    {label:'Tickets',val:fmtB(D.ticketHospitality),note:'+216% vs Qatar',color:'var(--green)'},
  ].map(s => `<div class="card card-sm"><div class="stat-label">${s.label}</div><div class="stat-value" style="color:${s.color};font-size:20px">${s.val}</div><div class="stat-note">${s.note}</div></div>`).join('');
  document.getElementById('rev-bars').innerHTML = revData.map(d => {
    const pct = Math.round((d.val/d.max)*100);
    return `<div class="bar-block"><div class="bar-header"><span class="bar-name">${d.name}</span><span class="bar-amount" style="color:${d.color}">${fmtB(d.val)}</span></div><div class="bar-track"><div class="bar-fill" style="width:${pct}%;background:${d.color}"></div></div><div class="bar-sub">${d.sub}</div></div>`;
  }).join('');
  document.getElementById('rev-insight').innerHTML = D.revInsight;

  const spendData = [
    {name:'Operations & logistics',val:D.operations,color:'#4da6ff',pct:Math.round((D.operations/D.totalExpenditure)*100)+'%',sub:'Match ops, logistics, broadcast infrastructure across 16 cities in 3 countries.'},
    {name:'Prizes & contributions',val:D.prizesContributions,color:'#f5c842',pct:Math.round((D.prizesContributions/D.totalExpenditure)*100)+'%',sub:fmtM(D.prizePool)+' prize pool + '+fmtM(D.clubBenefit)+' club benefit = '+fmtM(D.totalTeamContributions)+' total team contributions.'},
    {name:'Team services',val:D.teamServices,color:'#00e676',pct:Math.round((D.teamServices/D.totalExpenditure)*100)+'%',sub:'Hotels, training, transport for 48 squads. Zero new stadiums — major saving vs Qatar.'},
    {name:'Venues & competition',val:D.venues,color:'#d4537e',pct:Math.round((D.venues/D.totalExpenditure)*100)+'%',sub:'Existing stadium preparation, official training sites, competition infrastructure.'},
    {name:'Communications & marketing',val:D.commsMarketing,color:'#b39dff',pct:Math.round((D.commsMarketing/D.totalExpenditure)*100)+'%',sub:'Broadcast production, rights delivery, fan festivals, digital platforms.'},
    {name:'Administration & finance',val:D.admin,color:'#888',pct:Math.round((D.admin/D.totalExpenditure)*100)+'%',sub:'FIFA HQ operational costs, legal, compliance.'},
  ];
  document.getElementById('spend-stats').innerHTML = [
    {label:'Total expenditure',val:fmtB(D.totalExpenditure),note:'FIFA only',color:'var(--accent)'},
    {label:'Operations',val:fmtB(D.operations),note:'30% of spend',color:'var(--blue)'},
    {label:'Team services',val:fmtB(D.teamServices),note:'22% of spend',color:'var(--green)'},
    {label:'Admin',val:fmtB(D.admin),note:'Just 2% of spend',color:'var(--muted)'},
  ].map(s => `<div class="card card-sm"><div class="stat-label">${s.label}</div><div class="stat-value" style="color:${s.color};font-size:20px">${s.val}</div><div class="stat-note">${s.note}</div></div>`).join('');
  document.getElementById('spend-bars').innerHTML = spendData.map(d => {
    const pct = Math.round((d.val/D.operations)*100);
    return `<div class="bar-block"><div class="bar-header"><span class="bar-name">${d.name}</span><div style="display:flex;gap:8px"><span style="font-family:'IBM Plex Mono',monospace;font-size:11px;color:var(--muted)">${d.pct}</span><span class="bar-amount" style="color:${d.color}">${fmtB(d.val)}</span></div></div><div class="bar-track"><div class="bar-fill" style="width:${pct}%;background:${d.color}"></div></div><div class="bar-sub">${d.sub}</div></div>`;
  }).join('');
  document.getElementById('spend-insight').innerHTML = D.spendInsight;

  const prizes = [
    {round:'Winner',amt:D.winnerPrize,ctx:'All-time record. More than most nations\' football budget for a decade.'},
    {round:'Runner-up',amt:D.runnerUpPrize,ctx:''},
    {round:'3rd place',amt:D.thirdPlacePrize,ctx:''},
    {round:'4th place',amt:D.fourthPlacePrize,ctx:''},
    {round:'Quarter-finals (×4)',amt:D.quarterFinalPrize,ctx:''},
    {round:'Round of 16 (×8)',amt:D.roundOf16Prize,ctx:''},
    {round:'Round of 32 (×16)',amt:D.roundOf32Prize,ctx:''},
    {round:'Group stage (×16)',amt:D.groupStagePrize,ctx:'Still more than most associate federation annual budgets.'},
  ];
  document.getElementById('prize-stats').innerHTML = [
    {label:'Total prize pool',val:fmtM(D.prizePool),note:'+50% vs Qatar',color:'var(--gold)'},
    {label:'Winner earns',val:fmtM(D.winnerPrize),note:'Record',color:'var(--accent)'},
    {label:'Group stage min',val:fmtM(D.groupStagePrize),note:'Per nation',color:'var(--green)'},
    {label:'Total team package',val:fmtM(D.totalTeamContributions),note:'Incl. club benefit',color:'var(--blue)'},
  ].map(s => `<div class="card card-sm"><div class="stat-label">${s.label}</div><div class="stat-value" style="color:${s.color};font-size:20px">${s.val}</div><div class="stat-note">${s.note}</div></div>`).join('');
  document.getElementById('prize-rows').innerHTML = prizes.map(p => {
    const pct = Math.round((p.amt/D.winnerPrize)*100);
    return `<div class="prize-row"><div class="prize-rank">${p.round}</div><div class="prize-bar-wrap"><div class="prize-bar-fill" style="width:${pct}%"></div>${p.ctx?`<div class="prize-context">${p.ctx}</div>`:''}</div><div class="prize-amount">${fmtM(p.amt)}</div></div>`;
  }).join('');
  document.getElementById('prize-insight').innerHTML = D.prizeInsight;

  document.getElementById('flow-rows').innerHTML = [
    {from:'Global broadcasters',to:'FIFA',amt:fmtB(D.broadcastRights),cls:'pos',note:'Rights sold before kickoff. Locked regardless of tournament results.'},
    {from:'Global sponsors (16 slots)',to:'FIFA',amt:fmtB(D.sponsorship),cls:'pos',note:'All global positions sold. Adidas, Visa, Coca-Cola, BofA, Diageo + others.'},
    {from:'Fans (ticket buyers)',to:'FIFA',amt:fmtB(D.ticketHospitality),cls:'pos',note:'500M requests for ~3M tickets. 30× oversubscribed in first sales phase.'},
    {from:'FIFA',to:'48 competing nations',amt:fmtM(D.prizePool),cls:'neu',note:fmtM(D.groupStagePrize)+' – '+fmtM(D.winnerPrize)+' per nation based on finish.'},
    {from:'FIFA',to:'211 member associations',amt:'$2.25B',cls:'neu',note:'FIFA Forward 3.0 — $1.5M/year baseline per member. Over 4-year cycle.'},
    {from:'FIFA',to:'~300 clubs globally',amt:fmtM(D.clubBenefit),cls:'neg',note:((D.clubBenefit/(D.fifaRevenue*1000))*100).toFixed(1)+'% of FIFA revenue. Average ~$700K per club. Clubs spent years developing every player here.'},
    {from:'Nations',to:'Players (bonuses)',amt:'~$200M',cls:'neu',note:'Varies by country. Some nations pay $1M+ per player for winning.'},
    {from:'FIFA',to:'FIFA reserves',amt:'~$3B+',cls:'neg',note:'FIFA total assets: $9.48B, up 54% in 2025.'},
  ].map(r => `<tr><td style="color:var(--muted)">${r.from}</td><td style="font-weight:500">${r.to}</td><td class="amt ${r.cls}">${r.amt}</td><td><span class="note">${r.note}</span></td></tr>`).join('');
  document.getElementById('flow-insight').innerHTML = D.flowInsight;

  updateCounter(0);
  buildTicker();
}

function buildTicker() {
  const D = DATA;
  const items = [
    {l:'FIFA total revenue',v:fmtB(D.fifaRevenue),cls:'val'},
    {l:'Broadcast rights',v:fmtB(D.broadcastRights),cls:'val'},
    {l:'Prize pool',v:fmtM(D.prizePool)+' +50% vs Qatar',cls:'up'},
    {l:'Sponsorship — all 16 slots sold',v:fmtB(D.sponsorship),cls:'val'},
    {l:'Ticket requests',v:'500M',cls:'up'},
    {l:'Club benefit',v:fmtM(D.clubBenefit)+' ('+((D.clubBenefit/(D.fifaRevenue*1000))*100).toFixed(1)+'% of revenue)',cls:'dn'},
    {l:'New stadiums built',v:'$0',cls:'val'},
    {l:'Winner prize',v:fmtM(D.winnerPrize),cls:'up'},
    {l:'Group stage minimum',v:fmtM(D.groupStagePrize),cls:'val'},
    {l:'US GDP boost',v:fmtB(D.usGdpBoost),cls:'up'},
    {l:'Ad spend boost Q2 2026',v:fmtB(D.adSpendBoost),cls:'up'},
    {l:'Last updated',v:D.lastUpdated,cls:'val'},
  ];
  const html = items.map(i => `<div class="ticker-item">${i.l} <span class="${i.cls}">${i.v}</span></div>`).join('');
  document.getElementById('ticker').innerHTML = html + html;
}

const TOTAL_REV_DOLLARS = () => DATA.fifaRevenue * 1e9;
const CLUB_DOLLARS = () => DATA.clubBenefit * 1e6;
const PER_SEC = () => TOTAL_REV_DOLLARS() / (39 * 86400);
const PER_MATCH = () => TOTAL_REV_DOLLARS() / 104;

function updateCounter(day) {
  const frac = day / 38;
  document.getElementById('cnt-main').textContent = fmtBig(TOTAL_REV_DOLLARS() * frac);
  document.getElementById('slider-out').textContent = day === 0 ? 'Day 0' : `Day ${day}`;
  document.getElementById('cnt-sec').textContent = '$' + Math.round(PER_SEC()).toLocaleString() + '/sec';
  document.getElementById('cnt-match').textContent = fmtBig(PER_MATCH()) + '/match';
  document.getElementById('cnt-club').textContent = fmtBig(CLUB_DOLLARS() * frac);
  const subs = [
    "Drag the slider to move through the tournament and watch the billions accumulate in real time.",
    "Day 1. The broadcast rights alone generate more per day than most domestic leagues earn in a season.",
    "Day 5. FIFA has already earned more than the entire "+fmtM(DATA.prizePool)+" prize pool in amortised revenue.",
    "Day 10. "+fmtBig(TOTAL_REV_DOLLARS()*(10/38))+" earned. Roughly the GDP of Iceland.",
    "Day 14. Two weeks in. Clubs have received "+fmtBig(CLUB_DOLLARS()*(14/38))+" — less than 1% of FIFA revenue.",
    "Day 19. Halfway. Group stage over. The commercial inventory that made broadcast rights worth "+fmtB(DATA.broadcastRights)+" is now fully monetised.",
    "Day 25. Quarter-finals. TV ad rates peak here — not at the final. Maximum uncertainty, maximum audience.",
    "Day 32. Semi-finals. Transfer window opened July 1. This is a "+fmtB(2)+" player-valuation exercise.",
    "Day 38. Tournament over. Total: ~"+fmtB(DATA.fifaRevenue)+". Club benefit paid: "+fmtM(DATA.clubBenefit)+". That ratio is the story.",
  ];
  const idx = Math.min(Math.floor(day / (38/8)), subs.length - 1);
  document.getElementById('counter-insight').innerHTML = subs[idx];
  document.getElementById('cnt-sub').textContent = day === 0 ? 'Tournament: June 11 – July 19, 2026' : `After ${day} day${day===1?'':'s'} of 38`;
}

const typeColors = {revenue:'var(--blue)',prize:'var(--gold)',sponsor:'var(--accent)',tourism:'var(--purple)',clubs:'var(--red)',general:'var(--green)'};

function renderNews(items) {
  if (!items || items.length === 0) {
    document.getElementById('news-feed-container').innerHTML = '<div class="insight-card">No new updates found. The data shown reflects the latest available figures as of '+DATA.lastUpdated+'.</div>';
    return;
  }
  document.getElementById('news-feed-container').innerHTML = `<div class="news-feed">${items.map(n => `
    <div class="news-item">
      <div class="news-item-header">
        <div class="news-dot" style="background:${typeColors[n.type]||'var(--green)'}"></div>
        <div class="news-source">${n.source||'Latest news'}</div>
        <div class="news-date">${n.date||DATA.lastUpdated}</div>
      </div>
      <div class="news-headline">${n.headline}</div>
      <div class="news-insight">${n.insight}</div>
      ${n.number ? `<div class="news-number" style="color:${typeColors[n.type]||'var(--green)'}">${n.number}</div>` : ''}
    </div>`).join('')}</div>`;
}

async function fetchLatestNews() {
  const btn = document.getElementById('news-refresh-btn');
  btn.disabled = true;
  btn.classList.add('loading');
  document.getElementById('news-feed-container').innerHTML = '<div class="loading-state"><span class="spin" style="animation:spin .8s linear infinite;display:inline-block">↻</span>&nbsp;Searching for latest updates<span class="dot-anim"><span>.</span><span>.</span><span>.</span></span></div>';
  try {
    const raw = await callClaude('Search the web for the latest FIFA World Cup 2026 economics news from the past 2 weeks. Find any new sponsor announcements, revenue updates, prize money changes, host city economic data, or hotel/tourism figures. Return the JSON with newsItems populated.', true);
    const parsed = parseData(raw);
    if (parsed && parsed.newsItems) {
      renderNews(parsed.newsItems);
      if (parsed.lastUpdated) DATA.lastUpdated = parsed.lastUpdated;
    } else {
      renderNews([]);
    }
  } catch(e) {
    renderNews([]);
  }
  btn.disabled = false;
  btn.classList.remove('loading');
}

function setBootProgress(pct, msg) {
  document.getElementById('boot-bar').style.width = pct + '%';
  document.getElementById('boot-status').innerHTML = msg;
}

async function refreshAllData() {
  const btn = document.getElementById('refresh-btn');
  btn.disabled = true;
  btn.classList.add('loading');
  document.getElementById('last-updated').textContent = 'Searching web for latest data...';
  try {
    const raw = await callClaude('Search for the latest FIFA World Cup 2026 financial data, revenue figures, prize money, sponsorship deals, and economic impact. Return all fields as JSON with the most current numbers available.', true);
    const parsed = parseData(raw);
    if (parsed) {
      DATA = {...FALLBACK, ...parsed};
      renderAll();
      if (parsed.newsItems && parsed.newsItems.length > 0) renderNews(parsed.newsItems);
      document.getElementById('last-updated').textContent = 'Updated: ' + new Date().toLocaleTimeString();
    } else {
      document.getElementById('last-updated').textContent = 'Updated with cached data · ' + new Date().toLocaleTimeString();
    }
  } catch(e) {
    document.getElementById('last-updated').textContent = 'Using last known data · ' + new Date().toLocaleTimeString();
  }
  btn.disabled = false;
  btn.classList.remove('loading');
}

async function init() {
  renderAll();
  const boot = document.getElementById('boot');

  setBootProgress(15, 'Connecting to Claude API<span class="dot-anim"><span>.</span><span>.</span><span>.</span></span>');
  await new Promise(r => setTimeout(r, 400));

  setBootProgress(35, 'Searching web for latest FIFA World Cup 2026 economic data<span class="dot-anim"><span>.</span><span>.</span><span>.</span></span>');

  try {
    const raw = await callClaude('Search for the most recent FIFA World Cup 2026 financial news and return updated JSON data with the latest figures.', true);
    setBootProgress(75, 'Processing latest data<span class="dot-anim"><span>.</span><span>.</span><span>.</span></span>');
    await new Promise(r => setTimeout(r, 300));
    const parsed = parseData(raw);
    if (parsed) {
      DATA = {...FALLBACK, ...parsed};
      renderAll();
      if (parsed.newsItems && parsed.newsItems.length > 0) renderNews(parsed.newsItems);
    }
    setBootProgress(100, '<span class="boot-step">Data loaded. Ready.</span>');
  } catch(e) {
    setBootProgress(100, '<span class="boot-step">Using latest known data.</span>');
  }

  document.getElementById('last-updated').textContent = 'Updated: ' + new Date().toLocaleTimeString();
  document.getElementById('refresh-btn').disabled = false;

  await new Promise(r => setTimeout(r, 600));
  boot.classList.add('hidden');
  setTimeout(() => boot.style.display = 'none', 500);

  fetchLatestNews();
}

function switchTab(name, btn) {
  document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(b => b.classList.remove('active'));
  document.getElementById('tab-' + name).classList.add('active');
  btn.classList.add('active');
}

init();
</script>
</body>
</html>
