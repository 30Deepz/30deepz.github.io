<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Cybersécurité — Exposé Complet</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
  <style>
    /* ─── RESET & BASE ─────────────────────────────────────────── */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    :root {
      --bg:        #0a0e1a;
      --bg2:       #0f1629;
      --card:      #131d35;
      --accent:    #00d4ff;
      --accent2:   #7b2fff;
      --danger:    #ff4d6d;
      --warn:      #ffb703;
      --green:     #06d6a0;
      --text:      #e2e8f0;
      --muted:     #8892a4;
      --border:    rgba(0,212,255,.15);
      --glow:      0 0 24px rgba(0,212,255,.25);
      --radius:    14px;
      --font:      'Segoe UI', system-ui, sans-serif;
    }
    html { scroll-behavior: smooth; }
    body {
      background: var(--bg);
      color: var(--text);
      font-family: var(--font);
      line-height: 1.6;
      overflow-x: hidden;
    }

    /* ─── SCROLLBAR ────────────────────────────────────────────── */
    ::-webkit-scrollbar { width: 6px; }
    ::-webkit-scrollbar-track { background: var(--bg); }
    ::-webkit-scrollbar-thumb { background: var(--accent2); border-radius: 3px; }

    /* ─── HERO ─────────────────────────────────────────────────── */
    .hero {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 2rem;
      position: relative;
      overflow: hidden;
      background: radial-gradient(ellipse at 50% 0%, rgba(123,47,255,.35) 0%, transparent 65%),
                  radial-gradient(ellipse at 80% 80%, rgba(0,212,255,.2) 0%, transparent 55%),
                  var(--bg);
    }
    .hero::before {
      content: '';
      position: absolute; inset: 0;
      background-image:
        linear-gradient(rgba(0,212,255,.04) 1px, transparent 1px),
        linear-gradient(90deg, rgba(0,212,255,.04) 1px, transparent 1px);
      background-size: 60px 60px;
      animation: gridMove 20s linear infinite;
    }
    @keyframes gridMove { to { background-position: 60px 60px; } }

    .hero-badge {
      display: inline-flex; align-items: center; gap: .5rem;
      background: rgba(0,212,255,.12);
      border: 1px solid var(--border);
      border-radius: 999px;
      padding: .35rem 1rem;
      font-size: .8rem;
      letter-spacing: .08em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 1.5rem;
      position: relative; z-index: 1;
    }
    .hero h1 {
      font-size: clamp(2.4rem, 6vw, 5rem);
      font-weight: 800;
      letter-spacing: -.02em;
      line-height: 1.1;
      position: relative; z-index: 1;
      background: linear-gradient(135deg, #fff 30%, var(--accent) 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }
    .hero-sub {
      margin-top: 1.2rem;
      font-size: 1.15rem;
      color: var(--muted);
      max-width: 620px;
      position: relative; z-index: 1;
    }
    .hero-tags {
      display: flex; flex-wrap: wrap; gap: .6rem;
      justify-content: center;
      margin-top: 2rem;
      position: relative; z-index: 1;
    }
    .tag {
      padding: .3rem .9rem;
      border-radius: 999px;
      font-size: .8rem;
      font-weight: 600;
      letter-spacing: .04em;
      border: 1px solid;
    }
    .tag-blue  { background: rgba(0,212,255,.1);  border-color: rgba(0,212,255,.4);  color: var(--accent); }
    .tag-purple{ background: rgba(123,47,255,.1); border-color: rgba(123,47,255,.4); color: #b57bff; }
    .tag-red   { background: rgba(255,77,109,.1); border-color: rgba(255,77,109,.4); color: var(--danger); }
    .tag-green { background: rgba(6,214,160,.1);  border-color: rgba(6,214,160,.4);  color: var(--green); }
    .tag-warn  { background: rgba(255,183,3,.1);  border-color: rgba(255,183,3,.4);  color: var(--warn); }

    .scroll-hint {
      position: absolute; bottom: 2rem; left: 50%; transform: translateX(-50%);
      display: flex; flex-direction: column; align-items: center; gap: .4rem;
      color: var(--muted); font-size: .75rem; z-index: 1;
      animation: bounce 2s ease-in-out infinite;
    }
    .scroll-hint::after {
      content: '';
      width: 1px; height: 40px;
      background: linear-gradient(to bottom, var(--accent), transparent);
    }
    @keyframes bounce { 0%,100%{transform:translateX(-50%) translateY(0)} 50%{transform:translateX(-50%) translateY(8px)} }

    /* ─── NAV ──────────────────────────────────────────────────── */
    nav {
      position: sticky; top: 0; z-index: 100;
      background: rgba(10,14,26,.85);
      backdrop-filter: blur(16px);
      border-bottom: 1px solid var(--border);
      padding: .8rem 2rem;
      display: flex; gap: 1.5rem; flex-wrap: wrap;
      justify-content: center;
    }
    nav a {
      color: var(--muted);
      text-decoration: none;
      font-size: .85rem;
      font-weight: 500;
      letter-spacing: .04em;
      transition: color .2s;
    }
    nav a:hover { color: var(--accent); }

    /* ─── LAYOUT ───────────────────────────────────────────────── */
    .container { max-width: 1200px; margin: 0 auto; padding: 0 1.5rem; }
    section { padding: 5rem 0; }
    section:nth-child(even) { background: var(--bg2); }

    .section-header {
      text-align: center;
      margin-bottom: 3rem;
    }
    .section-label {
      display: inline-block;
      font-size: .75rem;
      font-weight: 700;
      letter-spacing: .12em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: .6rem;
    }
    .section-title {
      font-size: clamp(1.6rem, 3.5vw, 2.4rem);
      font-weight: 800;
      letter-spacing: -.02em;
      color: #fff;
    }
    .section-title span { color: var(--accent); }
    .section-desc {
      margin-top: .8rem;
      color: var(--muted);
      max-width: 600px;
      margin-left: auto; margin-right: auto;
    }

    /* ─── CARDS ────────────────────────────────────────────────── */
    .grid-2 { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px,1fr)); gap: 1.5rem; }
    .grid-3 { display: grid; grid-template-columns: repeat(auto-fit, minmax(260px,1fr)); gap: 1.5rem; }
    .grid-4 { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px,1fr)); gap: 1.2rem; }

    .card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1.8rem;
      transition: transform .25s, box-shadow .25s, border-color .25s;
    }
    .card:hover {
      transform: translateY(-4px);
      box-shadow: var(--glow);
      border-color: rgba(0,212,255,.35);
    }
    .card-icon {
      width: 48px; height: 48px;
      border-radius: 12px;
      display: flex; align-items: center; justify-content: center;
      font-size: 1.4rem;
      margin-bottom: 1rem;
    }
    .icon-blue   { background: rgba(0,212,255,.15); }
    .icon-red    { background: rgba(255,77,109,.15); }
    .icon-purple { background: rgba(123,47,255,.15); }
    .icon-green  { background: rgba(6,214,160,.15); }
    .icon-warn   { background: rgba(255,183,3,.15); }

    .card h3 {
      font-size: 1.05rem;
      font-weight: 700;
      color: #fff;
      margin-bottom: .5rem;
    }
    .card p, .card ul { color: var(--muted); font-size: .9rem; }
    .card ul { padding-left: 1.2rem; }
    .card ul li { margin-bottom: .3rem; }

    /* ─── KEYWORD PILLS ────────────────────────────────────────── */
    .keywords {
      display: flex; flex-wrap: wrap; gap: .5rem;
      margin-top: 1rem;
    }
    .kw {
      padding: .2rem .7rem;
      border-radius: 6px;
      font-size: .75rem;
      font-weight: 600;
      background: rgba(0,212,255,.08);
      border: 1px solid rgba(0,212,255,.2);
      color: var(--accent);
    }
    .kw.red    { background: rgba(255,77,109,.08);  border-color: rgba(255,77,109,.2);  color: var(--danger); }
    .kw.purple { background: rgba(123,47,255,.08);  border-color: rgba(123,47,255,.2);  color: #b57bff; }
    .kw.green  { background: rgba(6,214,160,.08);   border-color: rgba(6,214,160,.2);   color: var(--green); }
    .kw.warn   { background: rgba(255,183,3,.08);   border-color: rgba(255,183,3,.2);   color: var(--warn); }

    /* ─── STAT CARDS ───────────────────────────────────────────── */
    .stat-card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1.5rem;
      text-align: center;
      transition: transform .25s, box-shadow .25s;
    }
    .stat-card:hover { transform: translateY(-4px); box-shadow: var(--glow); }
    .stat-value {
      font-size: 2.4rem;
      font-weight: 900;
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }
    .stat-label { color: var(--muted); font-size: .85rem; margin-top: .3rem; }

    /* ─── IMAGE SECTIONS ───────────────────────────────────────── */
    .img-card {
      border-radius: var(--radius);
      overflow: hidden;
      border: 1px solid var(--border);
      position: relative;
    }
    .img-card img {
      width: 100%; height: 220px;
      object-fit: cover;
      display: block;
      transition: transform .4s;
    }
    .img-card:hover img { transform: scale(1.04); }
    .img-caption {
      padding: .8rem 1rem;
      background: var(--card);
      font-size: .8rem;
      color: var(--muted);
      text-align: center;
    }

    /* ─── CHART CONTAINERS ─────────────────────────────────────── */
    .chart-wrap {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1.5rem;
    }
    .chart-title {
      font-size: .95rem;
      font-weight: 700;
      color: #fff;
      margin-bottom: 1rem;
      text-align: center;
    }
    canvas { max-height: 320px; }

    /* ─── TIMELINE ─────────────────────────────────────────────── */
    .timeline { position: relative; padding-left: 2rem; }
    .timeline::before {
      content: '';
      position: absolute; left: 0; top: 0; bottom: 0;
      width: 2px;
      background: linear-gradient(to bottom, var(--accent), var(--accent2));
    }
    .tl-item {
      position: relative;
      padding: 0 0 2rem 1.5rem;
    }
    .tl-item::before {
      content: '';
      position: absolute; left: -1.5rem; top: .35rem;
      width: 12px; height: 12px;
      border-radius: 50%;
      background: var(--accent);
      box-shadow: 0 0 10px var(--accent);
    }
    .tl-title { font-weight: 700; color: #fff; margin-bottom: .3rem; }
    .tl-body  { color: var(--muted); font-size: .9rem; }

    /* ─── PROCESS STEPS ────────────────────────────────────────── */
    .steps { display: flex; flex-direction: column; gap: 1rem; }
    .step {
      display: flex; gap: 1rem; align-items: flex-start;
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1.2rem 1.5rem;
      transition: border-color .2s;
    }
    .step:hover { border-color: rgba(0,212,255,.4); }
    .step-num {
      min-width: 36px; height: 36px;
      border-radius: 50%;
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      display: flex; align-items: center; justify-content: center;
      font-weight: 800; font-size: .9rem; color: #fff;
      flex-shrink: 0;
    }
    .step-content h4 { color: #fff; font-size: .95rem; margin-bottom: .25rem; }
    .step-content p  { color: var(--muted); font-size: .88rem; }

    /* ─── HIGHLIGHT BOX ────────────────────────────────────────── */
    .highlight {
      background: linear-gradient(135deg, rgba(0,212,255,.08), rgba(123,47,255,.08));
      border: 1px solid rgba(0,212,255,.25);
      border-radius: var(--radius);
      padding: 1.5rem 2rem;
      margin: 2rem 0;
    }
    .highlight h4 { color: var(--accent); font-size: 1rem; margin-bottom: .5rem; }
    .highlight p  { color: var(--muted); font-size: .9rem; }

    /* ─── DANGER BOX ───────────────────────────────────────────── */
    .danger-box {
      background: rgba(255,77,109,.07);
      border: 1px solid rgba(255,77,109,.25);
      border-radius: var(--radius);
      padding: 1.2rem 1.5rem;
    }
    .danger-box h4 { color: var(--danger); margin-bottom: .4rem; font-size: .95rem; }
    .danger-box p  { color: var(--muted); font-size: .88rem; }

    /* ─── FOOTER ───────────────────────────────────────────────── */
    footer {
      background: var(--bg);
      border-top: 1px solid var(--border);
      padding: 2.5rem 1.5rem;
      text-align: center;
      color: var(--muted);
      font-size: .82rem;
    }
    footer strong { color: var(--accent); }

    /* ─── RESPONSIVE ───────────────────────────────────────────── */
    @media (max-width: 640px) {
      nav { gap: .8rem; }
      .grid-2, .grid-3, .grid-4 { grid-template-columns: 1fr; }
    }

    /* ─── ANIMATIONS ───────────────────────────────────────────── */
    .fade-in {
      opacity: 0; transform: translateY(24px);
      transition: opacity .6s ease, transform .6s ease;
    }
    .fade-in.visible { opacity: 1; transform: translateY(0); }

    /* ─── PROGRESS BARS ────────────────────────────────────────── */
    .progress-wrap { margin-bottom: 1rem; }
    .progress-label {
      display: flex; justify-content: space-between;
      font-size: .85rem; color: var(--muted); margin-bottom: .35rem;
    }
    .progress-bar {
      height: 8px; border-radius: 4px;
      background: rgba(255,255,255,.08);
      overflow: hidden;
    }
    .progress-fill {
      height: 100%; border-radius: 4px;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      width: 0;
      transition: width 1.2s ease;
    }

    /* ─── TABLE ────────────────────────────────────────────────── */
    .styled-table {
      width: 100%;
      border-collapse: collapse;
      font-size: .88rem;
    }
    .styled-table th {
      background: rgba(0,212,255,.1);
      color: var(--accent);
      padding: .75rem 1rem;
      text-align: left;
      font-weight: 700;
      border-bottom: 1px solid var(--border);
    }
    .styled-table td {
      padding: .7rem 1rem;
      border-bottom: 1px solid rgba(255,255,255,.05);
      color: var(--muted);
    }
    .styled-table tr:hover td { background: rgba(0,212,255,.04); color: var(--text); }
    .badge {
      display: inline-block;
      padding: .15rem .55rem;
      border-radius: 4px;
      font-size: .72rem;
      font-weight: 700;
    }
    .badge-high   { background: rgba(255,77,109,.15); color: var(--danger); }
    .badge-med    { background: rgba(255,183,3,.15);  color: var(--warn); }
    .badge-low    { background: rgba(6,214,160,.15);  color: var(--green); }
  </style>
</head>
<body>

<!-- ═══════════════════════════════════════════════════════════════
     HERO
═══════════════════════════════════════════════════════════════ -->
<section class="hero">
  <div class="hero-badge">🔐 Exposé · Informatique</div>
  <h1>Cybersécurité</h1>
  <p class="hero-sub">Menaces, protections, techniques d'attaque et bonnes pratiques — un panorama complet du monde numérique sécurisé.</p>
  <div class="hero-tags">
    <span class="tag tag-red">Malware</span>
    <span class="tag tag-red">Ransomware</span>
    <span class="tag tag-red">Phishing</span>
    <span class="tag tag-blue">Antivirus</span>
    <span class="tag tag-blue">Pare-feu</span>
    <span class="tag tag-blue">Chiffrement</span>
    <span class="tag tag-purple">RAT</span>
    <span class="tag tag-purple">Backdoor</span>
    <span class="tag tag-green">MFA</span>
    <span class="tag tag-green">Bonnes pratiques</span>
    <span class="tag tag-warn">DDoS</span>
    <span class="tag tag-warn">Ingénierie sociale</span>
  </div>
  <div class="scroll-hint">Défiler</div>
</section>

<!-- ═══════════════════════════════════════════════════════════════
     NAV
═══════════════════════════════════════════════════════════════ -->
<nav>
  <a href="#definition">Définition</a>
  <a href="#menaces">Menaces</a>
  <a href="#antivirus">Antivirus</a>
  <a href="#rat">Prise de contrôle</a>
  <a href="#protection">Protection</a>
  <a href="#stats">Statistiques</a>
  <a href="#pratiques">Bonnes pratiques</a>
</nav>

<!-- ═══════════════════════════════════════════════════════════════
     1 · DÉFINITION
═══════════════════════════════════════════════════════════════ -->
<section id="definition">
  <div class="container">
    <div class="section-header fade-in">
      <span class="section-label">Introduction</span>
      <h2 class="section-title">Qu'est-ce que la <span>Cybersécurité</span> ?</h2>
      <p class="section-desc">Protection des systèmes, réseaux et données contre les attaques numériques.</p>
    </div>

    <div class="grid-2 fade-in">
      <div>
        <div class="img-card">
          <img src="images/hacker.jpg" alt="Hacker cybersécurité" />
          <div class="img-caption">Source : Campus Technology — Illustration d'un acteur malveillant</div>
        </div>
      </div>
      <div style="display:flex;flex-direction:column;gap:1rem;">
        <div class="highlight">
          <h4>Définition</h4>
          <p>Ensemble des technologies, processus et pratiques visant à protéger les réseaux, ordinateurs, programmes et données contre les attaques, dommages ou accès non autorisés.</p>
        </div>
        <div class="card">
          <div class="card-icon icon-blue">🎯</div>
          <h3>Les 3 piliers — CIA</h3>
          <p>La cybersécurité repose sur trois principes fondamentaux :</p>
          <div class="keywords" style="margin-top:.8rem">
            <span class="kw">Confidentialité</span>
            <span class="kw">Intégrité</span>
            <span class="kw">Disponibilité</span>
          </div>
        </div>
        <div class="card">
          <div class="card-icon icon-red">⚠️</div>
          <h3>Enjeux majeurs</h3>
          <div class="keywords">
            <span class="kw red">Vol de données</span>
            <span class="kw red">Espionnage</span>
            <span class="kw red">Sabotage</span>
            <span class="kw warn">Fraude financière</span>
            <span class="kw warn">Atteinte à la réputation</span>
          </div>
        </div>
      </div>
    </div>

    <div class="grid-4 fade-in" style="margin-top:2rem">
      <div class="stat-card">
        <div class="stat-value">4,88 M$</div>
        <div class="stat-label">Coût moyen d'une violation de données (2024)</div>
      </div>
      <div class="stat-card">
        <div class="stat-value">+15%</div>
        <div class="stat-label">Incidents traités par l'ANSSI en 2024 vs 2023</div>
      </div>
      <div class="stat-card">
        <div class="stat-value">60%</div>
        <div class="stat-label">Des violations impliquent le facteur humain</div>
      </div>
      <div class="stat-card">
        <div class="stat-value">10,5 T$</div>
        <div class="stat-label">Coût annuel estimé de la cybercriminalité en 2025</div>
      </div>
    </div>
  </div>
</section>

<!-- ═══════════════════════════════════════════════════════════════
     2 · MENACES
═══════════════════════════════════════════════════════════════ -->
<section id="menaces">
  <div class="container">
    <div class="section-header fade-in">
      <span class="section-label">Types d'attaques</span>
      <h2 class="section-title">Les Principales <span>Menaces</span></h2>
      <p class="section-desc">Un panorama des vecteurs d'attaque les plus courants et les plus dangereux.</p>
    </div>

    <div class="grid-3 fade-in">

      <div class="card">
        <div class="card-icon icon-red">🦠</div>
        <h3>Malware</h3>
        <p>Logiciel malveillant conçu pour endommager, perturber ou obtenir un accès non autorisé.</p>
        <div class="keywords">
          <span class="kw red">Virus</span>
          <span class="kw red">Ver</span>
          <span class="kw red">Trojan</span>
          <span class="kw red">Spyware</span>
          <span class="kw red">Adware</span>
          <span class="kw red">Rootkit</span>
        </div>
      </div>

      <div class="card">
        <div class="card-icon icon-red">💰</div>
        <h3>Ransomware</h3>
        <p>Chiffre les fichiers de la victime et exige une rançon pour les déchiffrer. Représente ~75% des intrusions système.</p>
        <div class="keywords">
          <span class="kw red">Chiffrement AES</span>
          <span class="kw red">Bitcoin</span>
          <span class="kw warn">Rançon</span>
          <span class="kw warn">Double extorsion</span>
        </div>
      </div>

      <div class="card">
        <div class="card-icon icon-warn">🎣</div>
        <h3>Phishing</h3>
        <p>Usurpation d'identité par e-mail ou site web pour soutirer identifiants, mots de passe ou données bancaires.</p>
        <div class="keywords">
          <span class="kw warn">Spear phishing</span>
          <span class="kw warn">Smishing</span>
          <span class="kw warn">Vishing</span>
          <span class="kw warn">Whaling</span>
        </div>
      </div>

      <div class="card">
        <div class="card-icon icon-purple">💥</div>
        <h3>DDoS</h3>
        <p>Saturation d'un serveur par un flux massif de requêtes provenant d'un réseau de machines infectées (botnet).</p>
        <div class="keywords">
          <span class="kw purple">Botnet</span>
          <span class="kw purple">Amplification</span>
          <span class="kw purple">Volumétrique</span>
          <span class="kw purple">Layer 7</span>
        </div>
      </div>

      <div class="card">
        <div class="card-icon icon-blue">🕵️</div>
        <h3>Man-in-the-Middle</h3>
        <p>Interception des communications entre deux parties à leur insu pour espionner ou modifier les échanges.</p>
        <div class="keywords">
          <span class="kw">ARP Spoofing</span>
          <span class="kw">SSL Stripping</span>
          <span class="kw">DNS Spoofing</span>
        </div>
      </div>

      <div class="card">
        <div class="card-icon icon-warn">🧠</div>
        <h3>Ingénierie sociale</h3>
        <p>Manipulation psychologique pour pousser une personne à divulguer des informations confidentielles ou effectuer une action.</p>
        <div class="keywords">
          <span class="kw warn">Pretexting</span>
          <span class="kw warn">Baiting</span>
          <span class="kw warn">Quid pro quo</span>
        </div>
      </div>

      <div class="card">
        <div class="card-icon icon-red">💉</div>
        <h3>Injection SQL</h3>
        <p>Insertion de code malveillant dans une requête SQL pour manipuler ou extraire des données d'une base de données.</p>
        <div class="keywords">
          <span class="kw red">Base de données</span>
          <span class="kw red">Exfiltration</span>
          <span class="kw red">Authentification bypass</span>
        </div>
      </div>

      <div class="card">
        <div class="card-icon icon-purple">🔑</div>
        <h3>Keylogger</h3>
        <p>Enregistre chaque frappe clavier pour capturer mots de passe, numéros de carte bancaire et messages privés.</p>
        <div class="keywords">
          <span class="kw purple">Surveillance</span>
          <span class="kw purple">Capture</span>
          <span class="kw purple">Exfiltration</span>
        </div>
      </div>

      <div class="card">
        <div class="card-icon icon-blue">🔗</div>
        <h3>Supply Chain Attack</h3>
        <p>Compromission d'un fournisseur tiers pour atteindre indirectement des cibles plus importantes (ex. SolarWinds).</p>
        <div class="keywords">
          <span class="kw">Tiers</span>
          <span class="kw">Mise à jour piégée</span>
          <span class="kw">Dépendances</span>
        </div>
      </div>

    </div>

    <div style="margin-top:2.5rem;" class="fade-in">
      <div class="img-card" style="max-width:800px;margin:0 auto;">
        <img src="images/cyberattack.jpg" alt="Cyberattaque avancée" style="height:300px;" />
        <div class="img-caption">Source : Online Education Degrees in Cybersecurity — Attaques avancées persistantes (APT)</div>
      </div>
    </div>
  </div>
</section>

<!-- ═══════════════════════════════════════════════════════════════
     3 · ANTIVIRUS
═══════════════════════════════════════════════════════════════ -->
<section id="antivirus">
  <div class="container">
    <div class="section-header fade-in">
      <span class="section-label">Défense</span>
      <h2 class="section-title">Fonctionnement des <span>Antivirus</span></h2>
      <p class="section-desc">Comment les logiciels de protection détectent et neutralisent les menaces.</p>
    </div>

    <div class="grid-2 fade-in">
      <div>
        <div class="img-card">
          <img src="images/antivirus.jpg" alt="Antivirus protection" />
          <div class="img-caption">Source : Cybersecurity Guide — Rôle de l'antivirus dans la défense moderne</div>
        </div>
        <div class="highlight" style="margin-top:1.2rem">
          <h4>Définition</h4>
          <p>Logiciel conçu pour prévenir, détecter, isoler et supprimer les programmes malveillants d'un système informatique en temps réel ou à la demande.</p>
        </div>
      </div>
      <div>
        <h3 style="color:#fff;margin-bottom:1.2rem;font-size:1.1rem;">Méthodes de détection</h3>
        <div class="steps">
          <div class="step">
            <div class="step-num">1</div>
            <div class="step-content">
              <h4>Détection par signature</h4>
              <p>Comparaison des fichiers à une base de données de signatures connues (empreintes de malwares). Efficace contre les menaces connues.</p>
            </div>
          </div>
          <div class="step">
            <div class="step-num">2</div>
            <div class="step-content">
              <h4>Analyse heuristique</h4>
              <p>Détection de comportements suspects et de patterns anormaux pour identifier des variantes inconnues de malwares.</p>
            </div>
          </div>
          <div class="step">
            <div class="step-num">3</div>
            <div class="step-content">
              <h4>Analyse comportementale</h4>
              <p>Surveillance en temps réel des actions d'un programme : accès fichiers, connexions réseau, modifications registre.</p>
            </div>
          </div>
          <div class="step">
            <div class="step-num">4</div>
            <div class="step-content">
              <h4>Sandboxing</h4>
              <p>Exécution du fichier suspect dans un environnement isolé pour observer son comportement sans risque pour le système réel.</p>
            </div>
          </div>
          <div class="step">
            <div class="step-num">5</div>
            <div class="step-content">
              <h4>Machine Learning (IA)</h4>
              <p>Modèles entraînés sur des millions d'échantillons pour prédire si un fichier est malveillant, même inconnu.</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="grid-3 fade-in" style="margin-top:2rem">
      <div class="card">
        <div class="card-icon icon-blue">🔍</div>
        <h3>Scan à la demande</h3>
        <p>Analyse manuelle ou planifiée de tout ou partie du système à un instant donné.</p>
        <div class="keywords"><span class="kw">Planification</span><span class="kw">Analyse complète</span></div>
      </div>
      <div class="card">
        <div class="card-icon icon-green">⚡</div>
        <h3>Protection en temps réel</h3>
        <p>Surveillance continue en arrière-plan : chaque fichier ouvert, téléchargé ou exécuté est analysé instantanément.</p>
        <div class="keywords"><span class="kw green">Temps réel</span><span class="kw green">Mémoire active</span></div>
      </div>
      <div class="card">
        <div class="card-icon icon-warn">📧</div>
        <h3>Antivirus email</h3>
        <p>Analyse des pièces jointes et des liens dans les e-mails avant leur ouverture pour bloquer les vecteurs de phishing.</p>
        <div class="keywords"><span class="kw warn">Pièces jointes</span><span class="kw warn">Liens URL</span></div>
      </div>
    </div>

    <div class="fade-in" style="margin-top:2rem">
      <div class="card">
        <h3 style="margin-bottom:1.2rem;">Taux de détection par méthode</h3>
        <div id="progressBars">
          <div class="progress-wrap">
            <div class="progress-label"><span>Signature (menaces connues)</span><span>98%</span></div>
            <div class="progress-bar"><div class="progress-fill" data-width="98" style="background:linear-gradient(90deg,#00d4ff,#7b2fff)"></div></div>
          </div>
          <div class="progress-wrap">
            <div class="progress-label"><span>Heuristique (variantes)</span><span>82%</span></div>
            <div class="progress-bar"><div class="progress-fill" data-width="82" style="background:linear-gradient(90deg,#06d6a0,#00d4ff)"></div></div>
          </div>
          <div class="progress-wrap">
            <div class="progress-label"><span>Comportementale (zero-day)</span><span>74%</span></div>
            <div class="progress-bar"><div class="progress-fill" data-width="74" style="background:linear-gradient(90deg,#ffb703,#ff4d6d)"></div></div>
          </div>
          <div class="progress-wrap">
            <div class="progress-label"><span>Machine Learning</span><span>89%</span></div>
            <div class="progress-bar"><div class="progress-fill" data-width="89" style="background:linear-gradient(90deg,#7b2fff,#ff4d6d)"></div></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ═══════════════════════════════════════════════════════════════
     4 · PRISE DE CONTRÔLE À DISTANCE (RAT)
═══════════════════════════════════════════════════════════════ -->
<section id="rat">
  <div class="container">
    <div class="section-header fade-in">
      <span class="section-label">Menace avancée</span>
      <h2 class="section-title">Prise de contrôle <span>à distance</span></h2>
      <p class="section-desc">Remote Access Trojan (RAT) — l'une des menaces les plus furtives et dangereuses.</p>
    </div>

    <div class="grid-2 fade-in">
      <div>
        <div class="card" style="margin-bottom:1.2rem">
          <div class="card-icon icon-red">🐴</div>
          <h3>Qu'est-ce qu'un RAT ?</h3>
          <p>Un <strong style="color:#fff">Remote Access Trojan</strong> est un malware qui ouvre une porte dérobée (backdoor) sur la machine infectée, permettant à l'attaquant d'en prendre le contrôle total à distance, à l'insu de la victime.</p>
          <div class="keywords" style="margin-top:1rem">
            <span class="kw red">Backdoor</span>
            <span class="kw red">Furtivité</span>
            <span class="kw red">Contrôle total</span>
            <span class="kw purple">Persistance</span>
          </div>
        </div>
        <div class="danger-box">
          <h4>⚠️ Capacités d'un RAT actif</h4>
          <p>Accès aux fichiers · Capture d'écran · Enregistrement clavier · Activation webcam/micro · Installation de malwares · Exfiltration de données · Chiffrement ransomware</p>
        </div>
      </div>
      <div>
        <h3 style="color:#fff;margin-bottom:1rem;font-size:1.05rem;">Cycle d'infection d'un RAT</h3>
        <div class="timeline">
          <div class="tl-item">
            <div class="tl-title">1 · Vecteur d'infection</div>
            <div class="tl-body">E-mail de phishing, pièce jointe piégée, logiciel piraté, macro Office infectée, lien malveillant.</div>
          </div>
          <div class="tl-item">
            <div class="tl-title">2 · Installation silencieuse</div>
            <div class="tl-body">Le RAT se déploie discrètement, souvent déguisé en processus système légitime (svchost, explorer).</div>
          </div>
          <div class="tl-item">
            <div class="tl-title">3 · Établissement de la connexion C&C</div>
            <div class="tl-body">Contact avec le serveur Command & Control de l'attaquant via des canaux chiffrés ou du DNS tunneling.</div>
          </div>
          <div class="tl-item">
            <div class="tl-title">4 · Persistance</div>
            <div class="tl-body">Modification du registre Windows, création de tâches planifiées pour survivre aux redémarrages.</div>
          </div>
          <div class="tl-item">
            <div class="tl-title">5 · Exploitation</div>
            <div class="tl-body">Vol de données, espionnage, déploiement de ransomware, utilisation de la machine comme relais d'attaque.</div>
          </div>
        </div>
      </div>
    </div>

    <div class="grid-3 fade-in" style="margin-top:2rem">
      <div class="card">
        <div class="card-icon icon-red">🔴</div>
        <h3>Signes d'infection</h3>
        <ul>
          <li>Curseur qui se déplace seul</li>
          <li>Ralentissement inhabituel</li>
          <li>Processus inconnus (gestionnaire de tâches)</li>
          <li>Nouveaux logiciels non installés</li>
          <li>Redémarrages intempestifs</li>
          <li>Webcam activée sans raison</li>
        </ul>
      </div>
      <div class="card">
        <div class="card-icon icon-purple">🔬</div>
        <h3>RATs célèbres</h3>
        <ul>
          <li><strong style="color:#fff">Quasar</strong> — espionnage gouvernemental (2017)</li>
          <li><strong style="color:#fff">FlawedAmmyy</strong> — attaques bancaires TA505</li>
          <li><strong style="color:#fff">PiXie</strong> — caché dans un jeu open-source</li>
          <li><strong style="color:#fff">DarkComet</strong> — utilisé dans des conflits géopolitiques</li>
          <li><strong style="color:#fff">njRAT</strong> — très répandu au Moyen-Orient</li>
        </ul>
      </div>
      <div class="card">
        <div class="card-icon icon-green">🛡️</div>
        <h3>Contre-mesures</h3>
        <ul>
          <li>Antivirus + EDR avec analyse comportementale</li>
          <li>Pare-feu avec inspection profonde (DPI)</li>
          <li>Authentification 2FA sur accès distants</li>
          <li>Surveillance des connexions réseau sortantes</li>
          <li>Mises à jour régulières des logiciels</li>
          <li>Formation des utilisateurs au phishing</li>
        </ul>
      </div>
    </div>
  </div>
</section>

<!-- ═══════════════════════════════════════════════════════════════
     5 · PROTECTION
═══════════════════════════════════════════════════════════════ -->
<section id="protection">
  <div class="container">
    <div class="section-header fade-in">
      <span class="section-label">Défenses</span>
      <h2 class="section-title">Mécanismes de <span>Protection</span></h2>
      <p class="section-desc">Les technologies et protocoles qui forment la ligne de défense numérique.</p>
    </div>

    <!-- Pare-feu -->
    <div class="grid-2 fade-in" style="margin-bottom:2rem">
      <div>
        <div class="img-card">
          <img src="images/firewall.png" alt="Fonctionnement pare-feu" />
          <div class="img-caption">Source : Fortinet — Architecture et rôle d'un pare-feu réseau</div>
        </div>
      </div>
      <div style="display:flex;flex-direction:column;gap:1rem;">
        <div class="card">
          <div class="card-icon icon-blue">🧱</div>
          <h3>Pare-feu (Firewall)</h3>
          <p>Dispositif matériel ou logiciel qui filtre le trafic réseau entrant et sortant selon des règles prédéfinies.</p>
          <div class="keywords">
            <span class="kw">Filtrage de paquets</span>
            <span class="kw">Inspection d'état</span>
            <span class="kw">Proxy</span>
            <span class="kw">NGFW</span>
            <span class="kw">DMZ</span>
          </div>
        </div>
        <div class="card">
          <div class="card-icon icon-blue">🔒</div>
          <h3>Chiffrement SSL/TLS</h3>
          <p>Protocole cryptographique assurant la confidentialité et l'intégrité des communications sur Internet (HTTPS).</p>
          <div class="keywords">
            <span class="kw">Clé publique/privée</span>
            <span class="kw">Certificat X.509</span>
            <span class="kw">HTTPS</span>
            <span class="kw">AES-256</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Chiffrement + MFA -->
    <div class="grid-2 fade-in">
      <div>
        <div class="img-card">
          <img src="images/encryption.webp" alt="Chiffrement des données" />
          <div class="img-caption">Source : IDStrong — Principe du chiffrement des données</div>
        </div>
      </div>
      <div>
        <div class="img-card">
          <img src="images/mfa.png" alt="Authentification multi-facteurs MFA" />
          <div class="img-caption">Source : NIST — Niveaux d'authentification multi-facteurs</div>
        </div>
      </div>
    </div>

    <div class="grid-3 fade-in" style="margin-top:2rem">
      <div class="card">
        <div class="card-icon icon-green">🔐</div>
        <h3>Authentification MFA</h3>
        <p>Vérification de l'identité via plusieurs facteurs : mot de passe + code OTP + biométrie. Bloque 99,9% des attaques automatisées.</p>
        <div class="keywords">
          <span class="kw green">OTP</span>
          <span class="kw green">FIDO2</span>
          <span class="kw green">Biométrie</span>
          <span class="kw green">Passkey</span>
        </div>
      </div>
      <div class="card">
        <div class="card-icon icon-blue">🌐</div>
        <h3>VPN</h3>
        <p>Tunnel chiffré entre l'utilisateur et le réseau d'entreprise, masquant le trafic et protégeant les connexions distantes.</p>
        <div class="keywords">
          <span class="kw">OpenVPN</span>
          <span class="kw">WireGuard</span>
          <span class="kw">IPSec</span>
          <span class="kw">Zero Trust</span>
        </div>
      </div>
      <div class="card">
        <div class="card-icon icon-purple">🔎</div>
        <h3>IDS / IPS</h3>
        <p>Systèmes de détection et prévention d'intrusion qui analysent le trafic réseau pour identifier et bloquer les attaques en temps réel.</p>
        <div class="keywords">
          <span class="kw purple">Snort</span>
          <span class="kw purple">Suricata</span>
          <span class="kw purple">Anomalie</span>
        </div>
      </div>
    </div>

    <!-- Tableau comparatif -->
    <div class="fade-in" style="margin-top:2rem">
      <div class="chart-wrap">
        <div class="chart-title">Comparatif des solutions de protection</div>
        <div style="overflow-x:auto">
          <table class="styled-table">
            <thead>
              <tr>
                <th>Solution</th>
                <th>Rôle principal</th>
                <th>Menaces couvertes</th>
                <th>Niveau</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td><strong style="color:#fff">Antivirus / EDR</strong></td>
                <td>Détection & suppression malwares</td>
                <td>Virus, ransomware, spyware, RAT</td>
                <td><span class="badge badge-high">Critique</span></td>
              </tr>
              <tr>
                <td><strong style="color:#fff">Pare-feu</strong></td>
                <td>Filtrage trafic réseau</td>
                <td>DDoS, intrusions, scans</td>
                <td><span class="badge badge-high">Critique</span></td>
              </tr>
              <tr>
                <td><strong style="color:#fff">MFA</strong></td>
                <td>Authentification renforcée</td>
                <td>Vol de credentials, brute force</td>
                <td><span class="badge badge-high">Critique</span></td>
              </tr>
              <tr>
                <td><strong style="color:#fff">VPN / Chiffrement</strong></td>
                <td>Protection des communications</td>
                <td>MITM, écoute réseau</td>
                <td><span class="badge badge-med">Important</span></td>
              </tr>
              <tr>
                <td><strong style="color:#fff">IDS / IPS</strong></td>
                <td>Détection d'intrusions</td>
                <td>Attaques réseau, anomalies</td>
                <td><span class="badge badge-med">Important</span></td>
              </tr>
              <tr>
                <td><strong style="color:#fff">Sauvegarde 3-2-1</strong></td>
                <td>Résilience & récupération</td>
                <td>Ransomware, destruction données</td>
                <td><span class="badge badge-high">Critique</span></td>
              </tr>
              <tr>
                <td><strong style="color:#fff">Sensibilisation</strong></td>
                <td>Réduction du facteur humain</td>
                <td>Phishing, ingénierie sociale</td>
                <td><span class="badge badge-med">Important</span></td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ═══════════════════════════════════════════════════════════════
     6 · STATISTIQUES INTERACTIVES
═══════════════════════════════════════════════════════════════ -->
<section id="stats">
  <div class="container">
    <div class="section-header fade-in">
      <span class="section-label">Données 2024-2025</span>
      <h2 class="section-title"><span>Statistiques</span> Clés</h2>
      <p class="section-desc">Chiffres issus de l'ANSSI, CNIL, IBM, Verizon DBIR et ENISA.</p>
    </div>

    <div class="grid-2 fade-in">
      <div class="chart-wrap">
        <div class="chart-title">Répartition des types d'attaques (2024)</div>
        <canvas id="chartAttacks"></canvas>
      </div>
      <div class="chart-wrap">
        <div class="chart-title">Coût moyen d'une violation (M$) — Évolution</div>
        <canvas id="chartCost"></canvas>
      </div>
    </div>

    <div class="grid-2 fade-in" style="margin-top:1.5rem">
      <div class="chart-wrap">
        <div class="chart-title">Vecteurs d'accès initiaux (DBIR 2025)</div>
        <canvas id="chartVectors"></canvas>
      </div>
      <div class="chart-wrap">
        <div class="chart-title">Incidents traités par l'ANSSI (France)</div>
        <canvas id="chartAnssi"></canvas>
      </div>
    </div>

    <div class="grid-4 fade-in" style="margin-top:2rem">
      <div class="stat-card">
        <div class="stat-value">5 629</div>
        <div class="stat-label">Notifications violations CNIL en 2024 (+20%)</div>
      </div>
      <div class="stat-card">
        <div class="stat-value">144</div>
        <div class="stat-label">Compromissions ransomware signalées à l'ANSSI (2024)</div>
      </div>
      <div class="stat-card">
        <div class="stat-value">75%</div>
        <div class="stat-label">Des intrusions système liées au ransomware (DBIR)</div>
      </div>
      <div class="stat-card">
        <div class="stat-value">29 min</div>
        <div class="stat-label">Temps moyen de propagation d'une attaque (CrowdStrike 2026)</div>
      </div>
    </div>
  </div>
</section>

<!-- ═══════════════════════════════════════════════════════════════
     7 · BONNES PRATIQUES
═══════════════════════════════════════════════════════════════ -->
<section id="pratiques">
  <div class="container">
    <div class="section-header fade-in">
      <span class="section-label">Prévention</span>
      <h2 class="section-title">Bonnes <span>Pratiques</span></h2>
      <p class="section-desc">Les réflexes essentiels pour se protéger au quotidien.</p>
    </div>

    <div class="grid-2 fade-in">
      <div>
        <div class="img-card">
          <img src="images/phishing_lifecycle.png" alt="Cycle de vie d'une attaque ransomware" />
          <div class="img-caption">Source : Mantra.ms — Du phishing au ransomware : cycle complet d'une attaque</div>
        </div>
      </div>
      <div style="display:flex;flex-direction:column;gap:1rem;">
        <div class="card">
          <div class="card-icon icon-green">✅</div>
          <h3>Règle des 3-2-1 (Sauvegarde)</h3>
          <p><strong style="color:#fff">3</strong> copies des données · <strong style="color:#fff">2</strong> supports différents · <strong style="color:#fff">1</strong> copie hors ligne. Protection absolue contre le ransomware.</p>
          <div class="keywords"><span class="kw green">Résilience</span><span class="kw green">Récupération</span><span class="kw green">Hors-ligne</span></div>
        </div>
        <div class="card">
          <div class="card-icon icon-blue">🔄</div>
          <h3>Mises à jour & Patching</h3>
          <p>Appliquer les correctifs de sécurité rapidement, en priorité sur les équipements exposés (pare-feu, VPN). 9 des vulnérabilités les plus exploitées en 2024 touchaient des équipements de bordure.</p>
          <div class="keywords"><span class="kw">Zero-day</span><span class="kw">CVE</span><span class="kw">Patch Tuesday</span></div>
        </div>
      </div>
    </div>

    <div class="grid-4 fade-in" style="margin-top:2rem">
      <div class="card">
        <div class="card-icon icon-green">🔑</div>
        <h3>Mots de passe forts</h3>
        <ul>
          <li>Minimum 16 caractères</li>
          <li>Majuscules, chiffres, symboles</li>
          <li>Unique par service</li>
          <li>Gestionnaire de mots de passe</li>
          <li>Passkeys (FIDO2)</li>
        </ul>
      </div>
      <div class="card">
        <div class="card-icon icon-blue">🧑‍💻</div>
        <h3>Sensibilisation</h3>
        <ul>
          <li>Formation anti-phishing</li>
          <li>Simulations d'attaques</li>
          <li>Politique de sécurité claire</li>
          <li>Signalement des incidents</li>
          <li>Veille sur les menaces</li>
        </ul>
      </div>
      <div class="card">
        <div class="card-icon icon-purple">🔐</div>
        <h3>Principe du moindre privilège</h3>
        <ul>
          <li>Accès minimal nécessaire</li>
          <li>Comptes admin séparés</li>
          <li>Revue régulière des droits</li>
          <li>Segmentation réseau</li>
          <li>Zero Trust Architecture</li>
        </ul>
      </div>
      <div class="card">
        <div class="card-icon icon-warn">📊</div>
        <h3>Surveillance & Réponse</h3>
        <ul>
          <li>SIEM (centralisation logs)</li>
          <li>SOC (Security Operations Center)</li>
          <li>Plan de réponse aux incidents</li>
          <li>Tests de pénétration</li>
          <li>Bug bounty</li>
        </ul>
      </div>
    </div>

    <div class="fade-in" style="margin-top:2rem">
      <div class="highlight">
        <h4>🎯 Règle d'or : la défense en profondeur</h4>
        <p>Aucune solution unique ne suffit. La cybersécurité efficace repose sur plusieurs couches de protection complémentaires : technique (antivirus, pare-feu, chiffrement), organisationnelle (politiques, procédures) et humaine (sensibilisation, formation). C'est le principe de la <strong style="color:#fff">défense en profondeur</strong> (Defense in Depth).</p>
      </div>
    </div>

    <!-- Radar chart -->
    <div class="fade-in" style="margin-top:2rem">
      <div class="chart-wrap" style="max-width:500px;margin:0 auto;">
        <div class="chart-title">Niveau de maturité cybersécurité — Profil type PME</div>
        <canvas id="chartRadar"></canvas>
      </div>
    </div>
  </div>
</section>

<!-- ═══════════════════════════════════════════════════════════════
     FOOTER
═══════════════════════════════════════════════════════════════ -->
<footer>
  <p>Exposé réalisé à partir de sources : <strong>ANSSI</strong> · <strong>CNIL</strong> · <strong>IBM Cost of a Data Breach 2024</strong> · <strong>Verizon DBIR 2025</strong> · <strong>ENISA Threat Landscape 2024</strong> · <strong>CrowdStrike Global Threat Report 2026</strong></p>
  <p style="margin-top:.5rem">Images : Campus Technology · Cybersecurity Guide · NIST · IDStrong · Fortinet · Mantra.ms · Abusix</p>
</footer>

<!-- ═══════════════════════════════════════════════════════════════
     SCRIPTS
═══════════════════════════════════════════════════════════════ -->
<script>
  /* ── Intersection Observer for fade-in ── */
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        e.target.classList.add('visible');
        // Animate progress bars
        e.target.querySelectorAll('.progress-fill').forEach(bar => {
          bar.style.width = bar.dataset.width + '%';
        });
      }
    });
  }, { threshold: 0.1 });
  document.querySelectorAll('.fade-in').forEach(el => observer.observe(el));

  /* ── Chart defaults ── */
  Chart.defaults.color = '#8892a4';
  Chart.defaults.borderColor = 'rgba(255,255,255,.06)';
  Chart.defaults.font.family = "'Segoe UI', system-ui, sans-serif";

  /* ── 1. Doughnut — types d'attaques ── */
  new Chart(document.getElementById('chartAttacks'), {
    type: 'doughnut',
    data: {
      labels: ['Ransomware', 'Phishing', 'DDoS', 'Intrusion', 'Insider', 'Autres'],
      datasets: [{
        data: [32, 28, 15, 12, 7, 6],
        backgroundColor: ['#ff4d6d','#ffb703','#7b2fff','#00d4ff','#06d6a0','#4a5568'],
        borderWidth: 2,
        borderColor: '#131d35',
        hoverOffset: 8
      }]
    },
    options: {
      responsive: true,
      plugins: {
        legend: { position: 'bottom', labels: { padding: 16, usePointStyle: true } },
        tooltip: {
          callbacks: {
            label: ctx => ` ${ctx.label}: ${ctx.parsed}%`
          }
        }
      }
    }
  });

  /* ── 2. Line — coût violation ── */
  new Chart(document.getElementById('chartCost'), {
    type: 'line',
    data: {
      labels: ['2019','2020','2021','2022','2023','2024','2025'],
      datasets: [{
        label: 'Coût moyen (M$)',
        data: [3.92, 3.86, 4.24, 4.35, 4.45, 4.88, 4.44],
        borderColor: '#00d4ff',
        backgroundColor: 'rgba(0,212,255,.1)',
        fill: true,
        tension: 0.4,
        pointBackgroundColor: '#00d4ff',
        pointRadius: 5,
        pointHoverRadius: 8
      }]
    },
    options: {
      responsive: true,
      plugins: { legend: { display: false } },
      scales: {
        y: {
          beginAtZero: false,
          min: 3.5,
          ticks: { callback: v => v + ' M$' }
        }
      }
    }
  });

  /* ── 3. Bar — vecteurs initiaux ── */
  new Chart(document.getElementById('chartVectors'), {
    type: 'bar',
    data: {
      labels: ['Identifiants volés', 'Phishing', 'Vulnérabilités', 'Tiers compromis', 'Erreur humaine'],
      datasets: [{
        label: '% des incidents',
        data: [44, 22, 18, 10, 6],
        backgroundColor: [
          'rgba(0,212,255,.7)',
          'rgba(255,183,3,.7)',
          'rgba(123,47,255,.7)',
          'rgba(6,214,160,.7)',
          'rgba(255,77,109,.7)'
        ],
        borderRadius: 6,
        borderSkipped: false
      }]
    },
    options: {
      responsive: true,
      plugins: { legend: { display: false } },
      scales: {
        y: { ticks: { callback: v => v + '%' } },
        x: { ticks: { maxRotation: 20 } }
      }
    }
  });

  /* ── 4. Bar — ANSSI incidents ── */
  new Chart(document.getElementById('chartAnssi'), {
    type: 'bar',
    data: {
      labels: ['2020','2021','2022','2023','2024'],
      datasets: [
        {
          label: 'Signalements',
          data: [1800, 2200, 2400, 2612, 3004],
          backgroundColor: 'rgba(0,212,255,.65)',
          borderRadius: 5,
          borderSkipped: false
        },
        {
          label: 'Incidents',
          data: [750, 900, 1000, 1183, 1361],
          backgroundColor: 'rgba(255,77,109,.65)',
          borderRadius: 5,
          borderSkipped: false
        }
      ]
    },
    options: {
      responsive: true,
      plugins: { legend: { position: 'bottom', labels: { usePointStyle: true } } },
      scales: {
        y: { stacked: false },
        x: {}
      }
    }
  });

  /* ── 5. Radar — maturité cybersécurité ── */
  new Chart(document.getElementById('chartRadar'), {
    type: 'radar',
    data: {
      labels: ['Antivirus/EDR', 'Pare-feu', 'MFA', 'Sauvegarde', 'Sensibilisation', 'Patching', 'Surveillance'],
      datasets: [
        {
          label: 'PME moyenne',
          data: [70, 65, 40, 55, 35, 50, 30],
          borderColor: '#ff4d6d',
          backgroundColor: 'rgba(255,77,109,.15)',
          pointBackgroundColor: '#ff4d6d',
          borderWidth: 2
        },
        {
          label: 'Niveau recommandé',
          data: [90, 85, 90, 85, 80, 85, 75],
          borderColor: '#00d4ff',
          backgroundColor: 'rgba(0,212,255,.1)',
          pointBackgroundColor: '#00d4ff',
          borderWidth: 2,
          borderDash: [5,3]
        }
      ]
    },
    options: {
      responsive: true,
      scales: {
        r: {
          min: 0, max: 100,
          ticks: { stepSize: 25, backdropColor: 'transparent' },
          grid: { color: 'rgba(255,255,255,.08)' },
          angleLines: { color: 'rgba(255,255,255,.08)' },
          pointLabels: { font: { size: 11 }, color: '#8892a4' }
        }
      },
      plugins: {
        legend: { position: 'bottom', labels: { usePointStyle: true } }
      }
    }
  });
</script>
</body>
</html>
