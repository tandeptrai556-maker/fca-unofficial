<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>@dongdev/fca-unofficial</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0f;
    --bg2: #0f0f18;
    --surface: #13131e;
    --border: #1e1e30;
    --accent: #4f6ef7;
    --accent2: #7c3aed;
    --accent3: #06b6d4;
    --text: #e8e8f0;
    --muted: #6b6b8a;
    --green: #22c55e;
    --red: #ef4444;
    --yellow: #eab308;
    --mono: 'Space Mono', monospace;
    --sans: 'Syne', sans-serif;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--sans);
    line-height: 1.6;
    overflow-x: hidden;
  }

  /* Grid noise texture */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(79,110,247,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(79,110,247,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  /* Hero glow */
  .hero-glow {
    position: absolute;
    top: -200px;
    left: 50%;
    transform: translateX(-50%);
    width: 800px;
    height: 600px;
    background: radial-gradient(ellipse, rgba(79,110,247,0.15) 0%, rgba(124,58,237,0.08) 40%, transparent 70%);
    pointer-events: none;
  }

  /* ─── Layout ─── */
  .container {
    max-width: 900px;
    margin: 0 auto;
    padding: 0 24px;
    position: relative;
    z-index: 1;
  }

  /* ─── Header / Hero ─── */
  header {
    position: relative;
    padding: 80px 0 60px;
    text-align: center;
    overflow: hidden;
  }

  .pkg-tag {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    font-family: var(--mono);
    font-size: 11px;
    color: var(--accent);
    border: 1px solid rgba(79,110,247,0.3);
    padding: 4px 12px;
    border-radius: 2px;
    margin-bottom: 24px;
    letter-spacing: 0.1em;
    background: rgba(79,110,247,0.05);
    animation: fadeDown 0.6s ease both;
  }

  .pkg-tag::before {
    content: '';
    width: 6px;
    height: 6px;
    background: var(--accent);
    border-radius: 50%;
    animation: pulse 2s infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.5; transform: scale(0.8); }
  }

  h1 {
    font-family: var(--sans);
    font-size: clamp(36px, 6vw, 72px);
    font-weight: 800;
    line-height: 1.05;
    letter-spacing: -0.03em;
    margin-bottom: 16px;
    animation: fadeDown 0.6s 0.1s ease both;
  }

  h1 .at { color: var(--muted); font-weight: 400; }
  h1 .pkg { 
    background: linear-gradient(135deg, var(--accent), var(--accent2), var(--accent3));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .hero-desc {
    font-size: 16px;
    color: var(--muted);
    max-width: 480px;
    margin: 0 auto 32px;
    font-weight: 400;
    animation: fadeDown 0.6s 0.2s ease both;
  }

  .badges {
    display: flex;
    gap: 8px;
    justify-content: center;
    flex-wrap: wrap;
    margin-bottom: 40px;
    animation: fadeDown 0.6s 0.3s ease both;
  }

  .badge {
    font-family: var(--mono);
    font-size: 10px;
    padding: 4px 10px;
    border-radius: 2px;
    border: 1px solid;
    letter-spacing: 0.05em;
  }
  .badge.npm { color: #cc3534; border-color: rgba(204,53,52,0.3); background: rgba(204,53,52,0.05); }
  .badge.dl { color: var(--accent3); border-color: rgba(6,182,212,0.3); background: rgba(6,182,212,0.05); }
  .badge.mit { color: var(--green); border-color: rgba(34,197,94,0.3); background: rgba(34,197,94,0.05); }
  .badge.node { color: var(--yellow); border-color: rgba(234,179,8,0.3); background: rgba(234,179,8,0.05); }

  .hero-actions {
    display: flex;
    gap: 12px;
    justify-content: center;
    animation: fadeDown 0.6s 0.4s ease both;
  }

  .btn {
    font-family: var(--mono);
    font-size: 12px;
    padding: 10px 20px;
    border-radius: 2px;
    border: none;
    cursor: pointer;
    text-decoration: none;
    transition: all 0.2s;
    letter-spacing: 0.05em;
  }
  .btn-primary {
    background: var(--accent);
    color: #fff;
  }
  .btn-primary:hover { background: #3d5ce0; transform: translateY(-1px); }
  .btn-ghost {
    background: transparent;
    color: var(--text);
    border: 1px solid var(--border);
  }
  .btn-ghost:hover { border-color: var(--accent); color: var(--accent); }

  @keyframes fadeDown {
    from { opacity: 0; transform: translateY(-16px); }
    to { opacity: 1; transform: translateY(0); }
  }

  /* ─── Install bar ─── */
  .install-bar {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 16px 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 16px;
    margin: 48px 0;
    font-family: var(--mono);
    animation: fadeUp 0.6s 0.5s ease both;
  }

  .install-cmd {
    display: flex;
    align-items: center;
    gap: 12px;
    font-size: 13px;
  }

  .install-cmd .prompt { color: var(--accent); }
  .install-cmd .cmd { color: var(--text); }

  .copy-btn {
    font-family: var(--mono);
    font-size: 10px;
    padding: 6px 12px;
    border: 1px solid var(--border);
    background: transparent;
    color: var(--muted);
    border-radius: 2px;
    cursor: pointer;
    transition: all 0.2s;
    white-space: nowrap;
  }
  .copy-btn:hover { border-color: var(--accent); color: var(--accent); }
  .copy-btn.copied { border-color: var(--green); color: var(--green); }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to { opacity: 1; transform: translateY(0); }
  }

  /* ─── Warning Banner ─── */
  .warning {
    border: 1px solid rgba(234,179,8,0.3);
    background: rgba(234,179,8,0.04);
    border-radius: 4px;
    padding: 16px 20px;
    margin-bottom: 48px;
    display: flex;
    gap: 12px;
  }

  .warning-icon { font-size: 16px; flex-shrink: 0; margin-top: 1px; }

  .warning-content { flex: 1; }
  .warning-title {
    font-family: var(--mono);
    font-size: 11px;
    font-weight: 700;
    color: var(--yellow);
    letter-spacing: 0.1em;
    margin-bottom: 6px;
  }
  .warning-text { font-size: 13px; color: var(--muted); line-height: 1.5; }
  .warning-text strong { color: var(--yellow); }

  /* ─── Section ─── */
  section { margin-bottom: 64px; }

  .section-label {
    font-family: var(--mono);
    font-size: 10px;
    letter-spacing: 0.15em;
    color: var(--accent);
    text-transform: uppercase;
    margin-bottom: 12px;
  }

  h2 {
    font-size: 28px;
    font-weight: 800;
    letter-spacing: -0.02em;
    margin-bottom: 24px;
    color: var(--text);
  }

  h3 {
    font-size: 16px;
    font-weight: 700;
    margin-bottom: 12px;
    color: var(--text);
  }

  p { color: var(--muted); font-size: 14px; margin-bottom: 12px; }

  /* ─── Feature Grid ─── */
  .feature-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
    gap: 1px;
    background: var(--border);
    border: 1px solid var(--border);
    border-radius: 4px;
    overflow: hidden;
  }

  .feature-item {
    background: var(--surface);
    padding: 20px;
    transition: background 0.2s;
  }
  .feature-item:hover { background: var(--bg2); }

  .feature-icon { font-size: 20px; margin-bottom: 10px; }
  .feature-title { font-size: 13px; font-weight: 700; margin-bottom: 4px; color: var(--text); }
  .feature-desc { font-size: 12px; color: var(--muted); line-height: 1.5; }

  /* ─── Code Blocks ─── */
  .code-block {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 4px;
    overflow: hidden;
    margin-bottom: 24px;
    font-family: var(--mono);
  }

  .code-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 16px;
    border-bottom: 1px solid var(--border);
    background: rgba(0,0,0,0.2);
  }

  .code-title {
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 0.05em;
  }

  .code-dots {
    display: flex;
    gap: 6px;
  }

  .dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
  }
  .dot.r { background: #ff5f57; }
  .dot.y { background: #febc2e; }
  .dot.g { background: #28c840; }

  .code-body {
    padding: 20px;
    font-size: 12px;
    line-height: 1.7;
    overflow-x: auto;
    color: #abb2bf;
    white-space: pre;
  }

  /* Syntax colors */
  .kw { color: #c678dd; }
  .fn { color: #61afef; }
  .str { color: #98c379; }
  .cm { color: #5c6370; font-style: italic; }
  .num { color: #d19a66; }
  .var { color: #e06c75; }
  .prop { color: #e5c07b; }

  /* ─── API Table ─── */
  .api-group {
    margin-bottom: 32px;
  }

  .api-group-title {
    font-family: var(--mono);
    font-size: 11px;
    letter-spacing: 0.1em;
    color: var(--accent3);
    text-transform: uppercase;
    margin-bottom: 12px;
    padding-bottom: 8px;
    border-bottom: 1px solid var(--border);
  }

  .api-list {
    display: flex;
    flex-direction: column;
    gap: 2px;
  }

  .api-item {
    display: flex;
    align-items: flex-start;
    gap: 12px;
    padding: 10px 12px;
    border-radius: 3px;
    transition: background 0.15s;
    cursor: default;
  }
  .api-item:hover { background: var(--surface); }

  .api-name {
    font-family: var(--mono);
    font-size: 12px;
    color: var(--accent);
    white-space: nowrap;
    min-width: 200px;
  }

  .api-desc {
    font-size: 12px;
    color: var(--muted);
    line-height: 1.4;
  }

  /* ─── Message Types Table ─── */
  .msg-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
    border: 1px solid var(--border);
    border-radius: 4px;
    overflow: hidden;
  }

  .msg-table th {
    background: var(--surface);
    padding: 10px 16px;
    text-align: left;
    font-family: var(--mono);
    font-size: 10px;
    letter-spacing: 0.1em;
    color: var(--muted);
    font-weight: 400;
    border-bottom: 1px solid var(--border);
  }

  .msg-table td {
    padding: 10px 16px;
    border-bottom: 1px solid rgba(30,30,48,0.5);
    vertical-align: middle;
    color: var(--muted);
    font-size: 12px;
  }

  .msg-table tr:last-child td { border-bottom: none; }
  .msg-table tr:hover td { background: var(--surface); }

  .msg-table td:first-child {
    color: var(--text);
    font-weight: 700;
    font-size: 13px;
  }

  .msg-table td code {
    font-family: var(--mono);
    font-size: 11px;
    color: var(--accent);
    background: rgba(79,110,247,0.07);
    padding: 2px 6px;
    border-radius: 2px;
  }

  /* ─── Event types ─── */
  .events-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
    gap: 8px;
  }

  .event-chip {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 10px 14px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 3px;
    font-family: var(--mono);
    font-size: 11px;
    color: var(--text);
    transition: all 0.2s;
  }

  .event-chip:hover {
    border-color: var(--accent);
    color: var(--accent);
  }

  .event-dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: var(--accent3);
    flex-shrink: 0;
  }

  /* ─── Projects ─── */
  .projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
    gap: 12px;
  }

  .project-card {
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 16px;
    background: var(--surface);
    transition: all 0.2s;
    text-decoration: none;
  }

  .project-card:hover {
    border-color: rgba(79,110,247,0.4);
    transform: translateY(-2px);
    box-shadow: 0 8px 24px rgba(0,0,0,0.3);
  }

  .project-name {
    font-weight: 700;
    font-size: 14px;
    color: var(--text);
    margin-bottom: 4px;
  }

  .project-desc {
    font-size: 12px;
    color: var(--muted);
    line-height: 1.5;
  }

  /* ─── Tabs (AppState section) ─── */
  .tabs {
    display: flex;
    gap: 0;
    border-bottom: 1px solid var(--border);
    margin-bottom: 24px;
  }

  .tab {
    font-family: var(--mono);
    font-size: 11px;
    padding: 8px 16px;
    color: var(--muted);
    border-bottom: 2px solid transparent;
    cursor: pointer;
    transition: all 0.2s;
    letter-spacing: 0.05em;
    user-select: none;
    margin-bottom: -1px;
  }

  .tab:hover { color: var(--text); }
  .tab.active { color: var(--accent); border-bottom-color: var(--accent); }

  .tab-panel { display: none; }
  .tab-panel.active { display: block; }

  /* ─── Divider ─── */
  .divider {
    border: none;
    border-top: 1px solid var(--border);
    margin: 48px 0;
  }

  /* ─── Footer ─── */
  footer {
    padding: 40px 0 60px;
    border-top: 1px solid var(--border);
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: gap;
    gap: 16px;
  }

  .footer-brand {
    font-weight: 800;
    font-size: 16px;
    background: linear-gradient(135deg, var(--accent), var(--accent3));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .footer-links {
    display: flex;
    gap: 16px;
  }

  .footer-link {
    font-family: var(--mono);
    font-size: 11px;
    color: var(--muted);
    text-decoration: none;
    transition: color 0.2s;
    letter-spacing: 0.05em;
  }

  .footer-link:hover { color: var(--accent); }

  .footer-legal {
    font-size: 11px;
    color: rgba(107,107,138,0.5);
  }

  /* ─── Scrollbar ─── */
  ::-webkit-scrollbar { width: 4px; height: 4px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 2px; }
  ::-webkit-scrollbar-thumb:hover { background: var(--accent); }

  /* ─── Animations on scroll ─── */
  .reveal {
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.5s ease, transform 0.5s ease;
  }
  .reveal.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* ─── Nav ─── */
  nav {
    position: sticky;
    top: 0;
    z-index: 100;
    background: rgba(10,10,15,0.85);
    backdrop-filter: blur(12px);
    border-bottom: 1px solid var(--border);
  }

  .nav-inner {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 12px 24px;
    max-width: 900px;
    margin: 0 auto;
  }

  .nav-logo {
    font-family: var(--mono);
    font-size: 12px;
    color: var(--text);
    letter-spacing: -0.01em;
    text-decoration: none;
  }

  .nav-logo span { color: var(--accent); }

  .nav-links {
    display: flex;
    gap: 20px;
  }

  .nav-link {
    font-family: var(--mono);
    font-size: 11px;
    color: var(--muted);
    text-decoration: none;
    transition: color 0.2s;
    letter-spacing: 0.05em;
  }

  .nav-link:hover { color: var(--text); }

  .nav-badge {
    font-family: var(--mono);
    font-size: 10px;
    color: var(--accent);
    border: 1px solid rgba(79,110,247,0.3);
    padding: 2px 8px;
    border-radius: 2px;
  }

  @media (max-width: 600px) {
    .nav-links { display: none; }
    .hero-actions { flex-direction: column; align-items: center; }
    .footer { flex-direction: column; align-items: flex-start; }
    .api-name { min-width: 150px; }
  }
</style>
</head>
<body>

<!-- Nav -->
<nav>
  <div class="nav-inner">
    <a href="#" class="nav-logo"><span>@dongdev/</span>fca-unofficial</a>
    <div class="nav-links">
      <a href="#features" class="nav-link">Features</a>
      <a href="#quickstart" class="nav-link">Quick Start</a>
      <a href="#api" class="nav-link">API</a>
      <a href="#projects" class="nav-link">Projects</a>
    </div>
    <span class="nav-badge">v latest</span>
  </div>
</nav>

<!-- Hero -->
<header>
  <div class="hero-glow"></div>
  <div class="container">
    <div class="pkg-tag">npm install @dongdev/fca-unofficial@latest</div>
    <h1>
      <span class="at">@dongdev/</span><br>
      <span class="pkg">fca-unofficial</span>
    </h1>
    <p class="hero-desc">
      Unofficial Facebook Messenger API for Node.js — automate chat on personal accounts via browser emulation.
    </p>
    <div class="badges">
      <span class="badge npm">NPM PACKAGE</span>
      <span class="badge dl">ACTIVE DOWNLOADS</span>
      <span class="badge mit">MIT LICENSE</span>
      <span class="badge node">NODE ≥ 12.0.0</span>
    </div>
    <div class="hero-actions">
      <a href="https://www.npmjs.com/package/@dongdev/fca-unofficial" class="btn btn-primary">View on NPM →</a>
      <a href="https://github.com/Donix-VN/fca-unofficial" class="btn btn-ghost">GitHub</a>
    </div>
  </div>
</header>

<!-- Main content -->
<main>
  <div class="container">

    <!-- Install -->
    <div class="install-bar reveal">
      <div class="install-cmd">
        <span class="prompt">$</span>
        <span class="cmd">npm install <strong>@dongdev/fca-unofficial</strong>@latest</span>
      </div>
      <button class="copy-btn" onclick="copyInstall(this)">COPY</button>
    </div>

    <!-- Warning -->
    <div class="warning reveal">
      <div class="warning-icon">⚠️</div>
      <div class="warning-content">
        <div class="warning-title">DISCLAIMER — USE AT YOUR OWN RISK</div>
        <div class="warning-text">
          Accounts may be banned for <strong>spammy activity</strong> — bulk messaging, fast logins, suspicious URLs.
          Use <strong>AppState</strong> over credentials, implement <strong>rate limiting</strong>, and comply with Facebook ToS.
          Visit <strong>fca.dongdev.id.vn</strong> or use Firefox to reduce session issues.
        </div>
      </div>
    </div>

    <!-- Features -->
    <section id="features" class="reveal">
      <div class="section-label">// 01 — CAPABILITIES</div>
      <h2>Everything you need</h2>
      <div class="feature-grid">
        <div class="feature-item">
          <div class="feature-icon">💬</div>
          <div class="feature-title">Full Messenger API</div>
          <div class="feature-desc">Send messages, files, stickers, reactions, polls — the complete toolkit.</div>
        </div>
        <div class="feature-item">
          <div class="feature-icon">⚡</div>
          <div class="feature-title">Real-time via MQTT</div>
          <div class="feature-desc">Listen to messages, reactions, and thread events in real time.</div>
        </div>
        <div class="feature-item">
          <div class="feature-icon">👤</div>
          <div class="feature-title">User Account Support</div>
          <div class="feature-desc">Works with personal Facebook accounts — not just Pages or bots.</div>
        </div>
        <div class="feature-item">
          <div class="feature-icon">🔐</div>
          <div class="feature-title">AppState Sessions</div>
          <div class="feature-desc">Persist login state across restarts — no re-auth needed.</div>
        </div>
        <div class="feature-item">
          <div class="feature-icon">🔷</div>
          <div class="feature-title">TypeScript Support</div>
          <div class="feature-desc">Ships with full TypeScript definitions out of the box.</div>
        </div>
        <div class="feature-item">
          <div class="feature-icon">🔄</div>
          <div class="feature-title">Actively Maintained</div>
          <div class="feature-desc">Regularly updated to keep up with Facebook's changing API.</div>
        </div>
      </div>
    </section>

    <!-- Quick Start -->
    <section id="quickstart" class="reveal">
      <div class="section-label">// 02 — QUICK START</div>
      <h2>Get running fast</h2>

      <div class="tabs">
        <div class="tab active" onclick="switchTab(this, 'echo')">Echo Bot</div>
        <div class="tab" onclick="switchTab(this, 'send')">Send Message</div>
        <div class="tab" onclick="switchTab(this, 'file')">Send File</div>
        <div class="tab" onclick="switchTab(this, 'appstate')">AppState</div>
      </div>

      <div id="tab-echo" class="tab-panel active">
        <div class="code-block">
          <div class="code-header">
            <div class="code-dots"><div class="dot r"></div><div class="dot y"></div><div class="dot g"></div></div>
            <div class="code-title">echo-bot.js</div>
          </div>
          <div class="code-body"><span class="kw">const</span> <span class="var">login</span> = <span class="fn">require</span>(<span class="str">"@dongdev/fca-unofficial"</span>);

<span class="fn">login</span>({ appState: [] }, (<span class="var">err</span>, <span class="var">api</span>) => {
  <span class="kw">if</span> (<span class="var">err</span>) <span class="kw">return</span> console.<span class="fn">error</span>(<span class="var">err</span>);

  <span class="var">api</span>.<span class="fn">listenMqtt</span>((<span class="var">err</span>, <span class="var">event</span>) => {
    <span class="kw">if</span> (<span class="var">err</span>) <span class="kw">return</span> console.<span class="fn">error</span>(<span class="var">err</span>);

    <span class="cm">// Echo back every message received</span>
    <span class="var">api</span>.<span class="fn">sendMessage</span>(<span class="var">event</span>.<span class="prop">body</span>, <span class="var">event</span>.<span class="prop">threadID</span>);
  });
});</div>
        </div>
      </div>

      <div id="tab-send" class="tab-panel">
        <div class="code-block">
          <div class="code-header">
            <div class="code-dots"><div class="dot r"></div><div class="dot y"></div><div class="dot g"></div></div>
            <div class="code-title">send-message.js</div>
          </div>
          <div class="code-body"><span class="kw">const</span> <span class="var">login</span> = <span class="fn">require</span>(<span class="str">"@dongdev/fca-unofficial"</span>);

<span class="fn">login</span>({ appState: [] }, (<span class="var">err</span>, <span class="var">api</span>) => {
  <span class="kw">if</span> (<span class="var">err</span>) <span class="kw">return</span> console.<span class="fn">error</span>(<span class="str">"Login error:"</span>, <span class="var">err</span>);

  <span class="kw">const</span> <span class="var">recipientID</span> = <span class="str">"000000000000000"</span>; <span class="cm">// Facebook user ID</span>

  <span class="var">api</span>.<span class="fn">sendMessage</span>(<span class="str">"Hey! 👋"</span>, <span class="var">recipientID</span>, <span class="var">err</span> => {
    <span class="kw">if</span> (<span class="var">err</span>) console.<span class="fn">error</span>(<span class="var">err</span>);
    <span class="kw">else</span> console.<span class="fn">log</span>(<span class="str">"✅ Message sent!"</span>);
  });
});

<span class="cm">// 💡 Tip: find your user ID in cookies under 'c_user'</span></div>
        </div>
      </div>

      <div id="tab-file" class="tab-panel">
        <div class="code-block">
          <div class="code-header">
            <div class="code-dots"><div class="dot r"></div><div class="dot y"></div><div class="dot g"></div></div>
            <div class="code-title">send-file.js</div>
          </div>
          <div class="code-body"><span class="kw">const</span> <span class="var">login</span> = <span class="fn">require</span>(<span class="str">"@dongdev/fca-unofficial"</span>);
<span class="kw">const</span> <span class="var">fs</span> = <span class="fn">require</span>(<span class="str">"fs"</span>);

<span class="fn">login</span>({ appState: [] }, (<span class="var">err</span>, <span class="var">api</span>) => {
  <span class="kw">if</span> (<span class="var">err</span>) <span class="kw">return</span> console.<span class="fn">error</span>(<span class="var">err</span>);

  <span class="kw">const</span> <span class="var">msg</span> = {
    body: <span class="str">"Check out this image! 📷"</span>,
    attachment: <span class="var">fs</span>.<span class="fn">createReadStream</span>(<span class="str">`${__dirname}/image.jpg`</span>)
  };

  <span class="var">api</span>.<span class="fn">sendMessage</span>(<span class="var">msg</span>, <span class="str">"000000000000000"</span>, <span class="var">err</span> => {
    <span class="kw">if</span> (<span class="var">err</span>) console.<span class="fn">error</span>(<span class="var">err</span>);
    <span class="kw">else</span> console.<span class="fn">log</span>(<span class="str">"✅ Image sent!"</span>);
  });
});</div>
        </div>
      </div>

      <div id="tab-appstate" class="tab-panel">
        <div class="code-block">
          <div class="code-header">
            <div class="code-dots"><div class="dot r"></div><div class="dot y"></div><div class="dot g"></div></div>
            <div class="code-title">use-appstate.js</div>
          </div>
          <div class="code-body"><span class="kw">const</span> <span class="var">fs</span> = <span class="fn">require</span>(<span class="str">"fs"</span>);
<span class="kw">const</span> <span class="var">login</span> = <span class="fn">require</span>(<span class="str">"@dongdev/fca-unofficial"</span>);

<span class="cm">// Load saved session</span>
<span class="kw">const</span> <span class="var">appState</span> = JSON.<span class="fn">parse</span>(<span class="var">fs</span>.<span class="fn">readFileSync</span>(<span class="str">"appstate.json"</span>, <span class="str">"utf8"</span>));

<span class="fn">login</span>({ appState: <span class="var">appState</span> }, (<span class="var">err</span>, <span class="var">api</span>) => {
  <span class="kw">if</span> (<span class="var">err</span>) <span class="kw">return</span> console.<span class="fn">error</span>(<span class="var">err</span>);

  console.<span class="fn">log</span>(<span class="str">"✅ Logged in via AppState!"</span>);

  <span class="cm">// Save updated state</span>
  <span class="var">fs</span>.<span class="fn">writeFileSync</span>(
    <span class="str">"appstate.json"</span>,
    JSON.<span class="fn">stringify</span>(<span class="var">api</span>.<span class="fn">getAppState</span>(), <span class="kw">null</span>, <span class="num">2</span>)
  );
});</div>
        </div>
      </div>
    </section>

    <!-- Message Types -->
    <section class="reveal">
      <div class="section-label">// 03 — MESSAGE TYPES</div>
      <h2>What you can send</h2>
      <table class="msg-table">
        <thead>
          <tr>
            <th>TYPE</th>
            <th>USAGE</th>
            <th>NOTE</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Text</td>
            <td><code>{ body: "hello" }</code></td>
            <td>Plain string also accepted</td>
          </tr>
          <tr>
            <td>Sticker</td>
            <td><code>{ sticker: "369239263222822" }</code></td>
            <td>Use Facebook sticker IDs</td>
          </tr>
          <tr>
            <td>File / Image</td>
            <td><code>{ attachment: fs.createReadStream(path) }</code></td>
            <td>Readable stream</td>
          </tr>
          <tr>
            <td>URL</td>
            <td><code>{ url: "https://example.com" }</code></td>
            <td>Renders link preview</td>
          </tr>
          <tr>
            <td>Large Emoji</td>
            <td><code>{ emoji: "👍", emojiSize: "large" }</code></td>
            <td>small / medium / large</td>
          </tr>
        </tbody>
      </table>
      <p style="margin-top: 8px;">A message can include regular text plus <em>one of</em>: sticker, attachment, or URL.</p>
    </section>

    <!-- API Reference -->
    <section id="api" class="reveal">
      <div class="section-label">// 04 — API REFERENCE</div>
      <h2>Full method listing</h2>

      <div class="api-group">
        <div class="api-group-title">Messaging</div>
        <div class="api-list">
          <div class="api-item"><span class="api-name">api.sendMessage()</span><span class="api-desc">Send text, attachments, stickers, or URLs to a thread</span></div>
          <div class="api-item"><span class="api-name">api.sendTypingIndicator()</span><span class="api-desc">Show typing indicator in a thread</span></div>
          <div class="api-item"><span class="api-name">api.getMessage()</span><span class="api-desc">Get message history from a thread with limit</span></div>
          <div class="api-item"><span class="api-name">api.editMessage()</span><span class="api-desc">Edit the content of a sent message</span></div>
          <div class="api-item"><span class="api-name">api.deleteMessage()</span><span class="api-desc">Delete a message by ID</span></div>
          <div class="api-item"><span class="api-name">api.unsendMessage()</span><span class="api-desc">Unsend a message you've already sent</span></div>
          <div class="api-item"><span class="api-name">api.setMessageReaction()</span><span class="api-desc">React to a message with an emoji</span></div>
          <div class="api-item"><span class="api-name">api.forwardAttachment()</span><span class="api-desc">Forward an attachment to another thread</span></div>
          <div class="api-item"><span class="api-name">api.uploadAttachment()</span><span class="api-desc">Upload a file and get its attachment ID</span></div>
          <div class="api-item"><span class="api-name">api.createPoll()</span><span class="api-desc">Create a poll in a group thread</span></div>
        </div>
      </div>

      <div class="api-group">
        <div class="api-group-title">Read & Delivery</div>
        <div class="api-list">
          <div class="api-item"><span class="api-name">api.markAsRead()</span><span class="api-desc">Mark a thread as read</span></div>
          <div class="api-item"><span class="api-name">api.markAsReadAll()</span><span class="api-desc">Mark all threads as read at once</span></div>
          <div class="api-item"><span class="api-name">api.markAsDelivered()</span><span class="api-desc">Mark messages as delivered</span></div>
          <div class="api-item"><span class="api-name">api.markAsSeen()</span><span class="api-desc">Mark a thread as seen</span></div>
        </div>
      </div>

      <div class="api-group">
        <div class="api-group-title">Threads</div>
        <div class="api-list">
          <div class="api-item"><span class="api-name">api.getThreadInfo()</span><span class="api-desc">Get metadata and participants of a thread</span></div>
          <div class="api-item"><span class="api-name">api.getThreadList()</span><span class="api-desc">Fetch paginated list of threads</span></div>
          <div class="api-item"><span class="api-name">api.getThreadHistory()</span><span class="api-desc">Retrieve older messages from a thread</span></div>
          <div class="api-item"><span class="api-name">api.searchForThread()</span><span class="api-desc">Search threads by name</span></div>
          <div class="api-item"><span class="api-name">api.deleteThread()</span><span class="api-desc">Delete a thread from inbox</span></div>
          <div class="api-item"><span class="api-name">api.muteThread()</span><span class="api-desc">Mute a thread for N seconds</span></div>
          <div class="api-item"><span class="api-name">api.changeArchivedStatus()</span><span class="api-desc">Archive or unarchive a thread</span></div>
        </div>
      </div>

      <div class="api-group">
        <div class="api-group-title">Thread Customization</div>
        <div class="api-list">
          <div class="api-item"><span class="api-name">api.changeThreadColor()</span><span class="api-desc">Set the theme color of a thread</span></div>
          <div class="api-item"><span class="api-name">api.changeThreadEmoji()</span><span class="api-desc">Change the default emoji for a thread</span></div>
          <div class="api-item"><span class="api-name">api.changeGroupImage()</span><span class="api-desc">Set the group chat profile picture</span></div>
          <div class="api-item"><span class="api-name">api.setTitle()</span><span class="api-desc">Rename a group thread</span></div>
          <div class="api-item"><span class="api-name">api.changeNickname()</span><span class="api-desc">Set a user's nickname in a thread</span></div>
        </div>
      </div>

      <div class="api-group">
        <div class="api-group-title">Users & Groups</div>
        <div class="api-list">
          <div class="api-item"><span class="api-name">api.getUserInfo()</span><span class="api-desc">Fetch profile info for one or more users</span></div>
          <div class="api-item"><span class="api-name">api.getUserID()</span><span class="api-desc">Look up a Facebook user ID by username</span></div>
          <div class="api-item"><span class="api-name">api.getFriendsList()</span><span class="api-desc">Get the current user's friends list</span></div>
          <div class="api-item"><span class="api-name">api.getCurrentUserID()</span><span class="api-desc">Return the logged-in user's ID</span></div>
          <div class="api-item"><span class="api-name">api.createNewGroup()</span><span class="api-desc">Create a group with given participants</span></div>
          <div class="api-item"><span class="api-name">api.addUserToGroup()</span><span class="api-desc">Add a user to an existing group thread</span></div>
          <div class="api-item"><span class="api-name">api.removeUserFromGroup()</span><span class="api-desc">Remove a participant from a group</span></div>
          <div class="api-item"><span class="api-name">api.changeAdminStatus()</span><span class="api-desc">Promote or demote a group admin</span></div>
          <div class="api-item"><span class="api-name">api.handleFriendRequest()</span><span class="api-desc">Accept or decline a friend request</span></div>
          <div class="api-item"><span class="api-name">api.unfriend()</span><span class="api-desc">Remove a user from your friends list</span></div>
        </div>
      </div>

      <div class="api-group">
        <div class="api-group-title">Authentication & Config</div>
        <div class="api-list">
          <div class="api-item"><span class="api-name">api.getAppState()</span><span class="api-desc">Retrieve the current session AppState for persistence</span></div>
          <div class="api-item"><span class="api-name">api.setOptions()</span><span class="api-desc">Configure listenEvents, selfListen, logLevel, etc.</span></div>
          <div class="api-item"><span class="api-name">api.logout()</span><span class="api-desc">Log out of the current session</span></div>
          <div class="api-item"><span class="api-name">api.listenMqtt()</span><span class="api-desc">Start real-time event listener via MQTT — returns stop function</span></div>
        </div>
      </div>

    </section>

    <!-- Event Types -->
    <section class="reveal">
      <div class="section-label">// 05 — EVENTS</div>
      <h2>Event types</h2>
      <p>Received via <code style="font-family: var(--mono); color: var(--accent); background: rgba(79,110,247,0.07); padding: 2px 6px; border-radius: 2px; font-size: 12px;">api.listenMqtt()</code> when <code style="font-family: var(--mono); color: var(--accent); background: rgba(79,110,247,0.07); padding: 2px 6px; border-radius: 2px; font-size: 12px;">listenEvents: true</code> is set.</p>
      <br>
      <div class="events-grid">
        <div class="event-chip"><div class="event-dot"></div>message</div>
        <div class="event-chip"><div class="event-dot"></div>event</div>
        <div class="event-chip"><div class="event-dot"></div>typ</div>
        <div class="event-chip"><div class="event-dot"></div>read_receipt</div>
        <div class="event-chip"><div class="event-dot"></div>presence</div>
        <div class="event-chip"><div class="event-dot"></div>read</div>
        <div class="event-chip"><div class="event-dot"></div>delivery_receipt</div>
      </div>
    </section>

    <!-- Projects -->
    <section id="projects" class="reveal">
      <div class="section-label">// 06 — ECOSYSTEM</div>
      <h2>Built with this API</h2>
      <div class="projects-grid">
        <a class="project-card" href="https://github.com/lequanglam/c3c" target="_blank">
          <div class="project-name">c3c</div>
          <div class="project-desc">Customizable plugin-based bot for Facebook & Discord.</div>
        </a>
        <a class="project-card" href="https://github.com/miraiPr0ject/miraiv2" target="_blank">
          <div class="project-name">Miraiv2</div>
          <div class="project-desc">Simple and extensible Facebook Messenger bot framework.</div>
        </a>
        <a class="project-card" href="https://github.com/mjkaufer/Messer" target="_blank">
          <div class="project-name">Messer</div>
          <div class="project-desc">Command-line interface for Facebook Messenger.</div>
        </a>
        <a class="project-card" href="https://github.com/matrix-hacks/matrix-puppet-facebook" target="_blank">
          <div class="project-name">matrix-puppet-facebook</div>
          <div class="project-desc">Bridge Facebook Messenger to the Matrix protocol.</div>
        </a>
        <a class="project-card" href="https://github.com/Bjornskjald/miscord" target="_blank">
          <div class="project-name">Miscord</div>
          <div class="project-desc">Easy Facebook ↔ Discord message bridge.</div>
        </a>
        <a class="project-card" href="https://github.com/ivkos/botyo" target="_blank">
          <div class="project-name">Botyo</div>
          <div class="project-desc">Modular, extensible group chat bot platform.</div>
        </a>
        <a class="project-card" href="https://github.com/AstroCB/Messenger-CLI" target="_blank">
          <div class="project-name">Messenger-CLI</div>
          <div class="project-desc">Full-featured terminal client for Facebook Messenger.</div>
        </a>
        <a class="project-card" href="https://github.com/concierge/Concierge" target="_blank">
          <div class="project-name">Concierge</div>
          <div class="project-desc">Highly modular chat bot with a built-in package manager.</div>
        </a>
      </div>
    </section>

    <!-- Footer -->
    <footer>
      <div>
        <div class="footer-brand">@dongdev/fca-unofficial</div>
        <div class="footer-legal" style="margin-top: 4px;">MIT License · Unofficial · Not affiliated with Meta</div>
      </div>
      <div class="footer-links">
        <a href="https://www.npmjs.com/package/@dongdev/fca-unofficial" class="footer-link">NPM</a>
        <a href="https://github.com/Donix-VN/fca-unofficial" class="footer-link">GITHUB</a>
        <a href="https://github.com/Donix-VN/fca-unofficial/issues" class="footer-link">ISSUES</a>
        <a href="https://www.facebook.com/mdong.dev" class="footer-link">SUPPORT</a>
      </div>
    </footer>

  </div>
</main>

<script>
  // Tabs
  function switchTab(el, id) {
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
    el.classList.add('active');
    document.getElementById('tab-' + id).classList.add('active');
  }

  // Copy install
  function copyInstall(btn) {
    navigator.clipboard.writeText('npm install @dongdev/fca-unofficial@latest').then(() => {
      btn.textContent = 'COPIED!';
      btn.classList.add('copied');
      setTimeout(() => { btn.textContent = 'COPY'; btn.classList.remove('copied'); }, 2000);
    });
  }

  // Scroll reveal
  const observer = new IntersectionObserver(entries => {
    entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('visible'); } });
  }, { threshold: 0.1 });

  document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
</script>
</body>
</html>
