<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Geraldo Art — Developer Portfolio</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;700;800&family=JetBrains+Mono:wght@300;400;600&display=swap" rel="stylesheet"/>
<style>
  :root {
    --bg: #020408;
    --surface: #080f18;
    --card: #0c1824;
    --accent: #00d4ff;
    --accent2: #0066ff;
    --accent3: #00ff88;
    --text: #e8f4f8;
    --muted: #4a7090;
    --border: rgba(0,212,255,0.12);
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'JetBrains Mono', monospace;
    overflow-x: hidden;
    cursor: none;
  }

  /* CUSTOM CURSOR */
  .cursor {
    position: fixed;
    width: 12px; height: 12px;
    background: var(--accent);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9999;
    transform: translate(-50%, -50%);
    transition: transform 0.1s, width 0.2s, height 0.2s, background 0.2s;
    mix-blend-mode: screen;
  }
  .cursor-ring {
    position: fixed;
    width: 40px; height: 40px;
    border: 1px solid rgba(0,212,255,0.4);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9998;
    transform: translate(-50%, -50%);
    transition: transform 0.15s ease, width 0.3s, height 0.3s;
  }
  body:hover .cursor { opacity: 1; }

  /* NOISE OVERLAY */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.03'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 1;
    opacity: 0.4;
  }

  /* GRID BACKGROUND */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background-image: 
      linear-gradient(rgba(0,212,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,212,255,0.03) 1px, transparent 1px);
    background-size: 60px 60px;
    pointer-events: none;
    z-index: 0;
  }

  /* NAV */
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 100;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 20px 48px;
    background: rgba(2,4,8,0.8);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid var(--border);
  }
  .nav-logo {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: 1.2rem;
    letter-spacing: -0.02em;
    color: var(--accent);
  }
  .nav-links { display: flex; gap: 32px; }
  .nav-links a {
    color: var(--muted);
    text-decoration: none;
    font-size: 0.75rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    transition: color 0.2s;
  }
  .nav-links a:hover { color: var(--accent); }

  /* SECTIONS */
  section {
    position: relative;
    z-index: 2;
    padding: 120px 48px;
    max-width: 1200px;
    margin: 0 auto;
  }

  /* HERO */
  #hero {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding-top: 140px;
    max-width: 100%;
    padding-left: 80px;
    padding-right: 80px;
  }

  .hero-label {
    font-size: 0.7rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 24px;
    opacity: 0;
    animation: fadeUp 0.8s forwards 0.2s;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .hero-label::before {
    content: '';
    display: inline-block;
    width: 32px; height: 1px;
    background: var(--accent);
  }

  .hero-name {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: clamp(4rem, 10vw, 9rem);
    line-height: 0.9;
    letter-spacing: -0.04em;
    opacity: 0;
    animation: fadeUp 0.9s forwards 0.4s;
    background: linear-gradient(135deg, #fff 0%, var(--accent) 60%, var(--accent2) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .hero-role {
    font-size: clamp(1rem, 2.5vw, 1.4rem);
    color: var(--muted);
    margin-top: 24px;
    opacity: 0;
    animation: fadeUp 0.9s forwards 0.6s;
    font-weight: 300;
  }
  .hero-role span { color: var(--accent3); }

  .hero-cta {
    display: flex;
    gap: 16px;
    margin-top: 48px;
    opacity: 0;
    animation: fadeUp 0.9s forwards 0.8s;
  }

  .btn-primary {
    background: var(--accent);
    color: var(--bg);
    padding: 14px 32px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.8rem;
    font-weight: 600;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    border: none;
    cursor: none;
    text-decoration: none;
    transition: all 0.25s;
    clip-path: polygon(8px 0%, 100% 0%, calc(100% - 8px) 100%, 0% 100%);
  }
  .btn-primary:hover {
    background: #fff;
    transform: translateY(-2px);
  }

  .btn-ghost {
    background: transparent;
    color: var(--accent);
    padding: 14px 32px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.8rem;
    font-weight: 600;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    border: 1px solid var(--border);
    cursor: none;
    text-decoration: none;
    transition: all 0.25s;
    clip-path: polygon(8px 0%, 100% 0%, calc(100% - 8px) 100%, 0% 100%);
  }
  .btn-ghost:hover {
    border-color: var(--accent);
    background: rgba(0,212,255,0.05);
  }

  /* FLOATING CODE BLOCK */
  .hero-code {
    position: absolute;
    right: 80px;
    top: 50%;
    transform: translateY(-50%);
    background: var(--card);
    border: 1px solid var(--border);
    padding: 28px 32px;
    font-size: 0.8rem;
    line-height: 1.8;
    opacity: 0;
    animation: fadeLeft 1s forwards 1s;
    max-width: 380px;
    box-shadow: 0 0 60px rgba(0,212,255,0.06), 0 40px 80px rgba(0,0,0,0.5);
  }
  .hero-code::before {
    content: '● ● ●';
    display: block;
    color: var(--muted);
    margin-bottom: 16px;
    font-size: 0.6rem;
    letter-spacing: 0.2em;
  }
  .c-key { color: #c792ea; }
  .c-var { color: var(--accent); }
  .c-str { color: var(--accent3); }
  .c-brace { color: #ffcb6b; }
  .c-comment { color: var(--muted); }

  /* SCROLL INDICATOR */
  .scroll-indicator {
    position: absolute;
    bottom: 40px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
    opacity: 0;
    animation: fadeUp 1s forwards 1.4s;
    color: var(--muted);
    font-size: 0.65rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
  }
  .scroll-line {
    width: 1px;
    height: 50px;
    background: linear-gradient(to bottom, var(--accent), transparent);
    animation: scrollPulse 2s infinite;
  }

  /* SECTION HEADERS */
  .section-tag {
    font-size: 0.65rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .section-tag::before {
    content: '';
    display: inline-block;
    width: 24px; height: 1px;
    background: var(--accent);
  }

  .section-title {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: clamp(2rem, 5vw, 3.5rem);
    letter-spacing: -0.03em;
    line-height: 1;
    margin-bottom: 60px;
  }

  /* ABOUT */
  .about-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 60px;
    align-items: start;
  }
  .about-text {
    font-size: 1rem;
    line-height: 1.9;
    color: rgba(232,244,248,0.7);
    font-weight: 300;
  }
  .about-text strong { color: var(--accent); font-weight: 600; }

  .stats-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
  }
  .stat-card {
    background: var(--card);
    border: 1px solid var(--border);
    padding: 24px;
    position: relative;
    overflow: hidden;
    transition: border-color 0.3s, transform 0.3s;
  }
  .stat-card::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(0,212,255,0.05), transparent);
    opacity: 0;
    transition: opacity 0.3s;
  }
  .stat-card:hover { border-color: rgba(0,212,255,0.3); transform: translateY(-4px); }
  .stat-card:hover::before { opacity: 1; }
  .stat-number {
    font-family: 'Syne', sans-serif;
    font-size: 2.5rem;
    font-weight: 800;
    color: var(--accent);
    line-height: 1;
    margin-bottom: 6px;
  }
  .stat-label { font-size: 0.7rem; color: var(--muted); letter-spacing: 0.1em; }

  /* SKILLS */
  .skills-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 20px;
  }
  .skill-group {
    background: var(--card);
    border: 1px solid var(--border);
    padding: 28px;
    transition: border-color 0.3s, transform 0.3s;
    position: relative;
    overflow: hidden;
  }
  .skill-group::after {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.3s;
  }
  .skill-group:hover { border-color: rgba(0,212,255,0.2); transform: translateY(-4px); }
  .skill-group:hover::after { transform: scaleX(1); }
  .skill-group-title {
    font-size: 0.65rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 20px;
  }
  .skill-tags { display: flex; flex-wrap: wrap; gap: 8px; }
  .skill-tag {
    background: rgba(0,212,255,0.06);
    border: 1px solid rgba(0,212,255,0.15);
    color: rgba(232,244,248,0.8);
    padding: 6px 14px;
    font-size: 0.72rem;
    letter-spacing: 0.05em;
    transition: all 0.2s;
  }
  .skill-tag:hover {
    background: rgba(0,212,255,0.12);
    border-color: rgba(0,212,255,0.4);
    color: var(--accent);
  }

  /* PROJECTS */
  .projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
    gap: 24px;
  }
  .project-card {
    background: var(--card);
    border: 1px solid var(--border);
    padding: 36px;
    position: relative;
    overflow: hidden;
    transition: all 0.35s;
    text-decoration: none;
    display: block;
    color: inherit;
    cursor: none;
  }
  .project-card::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(0,212,255,0.05) 0%, transparent 60%);
    opacity: 0;
    transition: opacity 0.35s;
  }
  .project-card:hover { transform: translateY(-8px); border-color: rgba(0,212,255,0.3); box-shadow: 0 30px 60px rgba(0,0,0,0.4), 0 0 30px rgba(0,212,255,0.05); }
  .project-card:hover::before { opacity: 1; }
  .project-num {
    font-family: 'Syne', sans-serif;
    font-size: 3.5rem;
    font-weight: 800;
    color: rgba(0,212,255,0.08);
    line-height: 1;
    margin-bottom: -10px;
  }
  .project-title {
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 1.3rem;
    margin-bottom: 12px;
    transition: color 0.2s;
  }
  .project-card:hover .project-title { color: var(--accent); }
  .project-desc { font-size: 0.82rem; color: var(--muted); line-height: 1.7; margin-bottom: 24px; }
  .project-tech { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 20px; }
  .project-tech span {
    font-size: 0.65rem;
    padding: 3px 10px;
    background: rgba(0,102,255,0.1);
    border: 1px solid rgba(0,102,255,0.2);
    color: #64b5f6;
    letter-spacing: 0.08em;
  }
  .project-arrow {
    color: var(--muted);
    font-size: 0.75rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    display: flex;
    align-items: center;
    gap: 8px;
    transition: color 0.2s, gap 0.2s;
  }
  .project-card:hover .project-arrow { color: var(--accent); gap: 14px; }

  /* CONTACT */
  .contact-inner {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 80px;
    align-items: center;
  }
  .contact-text { font-size: 1rem; color: rgba(232,244,248,0.6); line-height: 1.8; font-weight: 300; }
  .contact-links { display: flex; flex-direction: column; gap: 16px; }
  .contact-link {
    display: flex;
    align-items: center;
    gap: 16px;
    text-decoration: none;
    color: var(--text);
    padding: 20px 24px;
    background: var(--card);
    border: 1px solid var(--border);
    transition: all 0.25s;
    font-size: 0.85rem;
    cursor: none;
  }
  .contact-link:hover {
    border-color: rgba(0,212,255,0.3);
    transform: translateX(8px);
    color: var(--accent);
    background: rgba(0,212,255,0.04);
  }
  .contact-link-icon {
    width: 36px; height: 36px;
    background: rgba(0,212,255,0.1);
    border: 1px solid rgba(0,212,255,0.2);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1rem;
    flex-shrink: 0;
  }

  /* FOOTER */
  footer {
    position: relative;
    z-index: 2;
    border-top: 1px solid var(--border);
    padding: 40px 80px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 0.72rem;
    color: var(--muted);
    letter-spacing: 0.1em;
  }
  .footer-logo {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: 1.1rem;
    color: var(--accent);
  }

  /* DIVIDER */
  .divider {
    position: relative;
    z-index: 2;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--accent), transparent);
    margin: 0 80px;
    opacity: 0.2;
  }

  /* ANIMATIONS */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(30px); }
    to { opacity: 1; transform: translateY(0); }
  }
  @keyframes fadeLeft {
    from { opacity: 0; transform: translate(30px, -50%); }
    to { opacity: 1; transform: translate(0, -50%); }
  }
  @keyframes scrollPulse {
    0%, 100% { opacity: 1; transform: scaleY(1); }
    50% { opacity: 0.3; transform: scaleY(0.5); }
  }

  /* REVEAL ON SCROLL */
  .reveal {
    opacity: 0;
    transform: translateY(40px);
    transition: opacity 0.8s ease, transform 0.8s ease;
  }
  .reveal.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* GLOW ORBS */
  .orb {
    position: fixed;
    border-radius: 50%;
    filter: blur(120px);
    pointer-events: none;
    z-index: 0;
  }
  .orb-1 {
    width: 600px; height: 600px;
    background: rgba(0,102,255,0.06);
    top: -200px; right: -200px;
  }
  .orb-2 {
    width: 400px; height: 400px;
    background: rgba(0,212,255,0.04);
    bottom: 10%; left: -100px;
  }

  /* STATUS BADGE */
  .status-badge {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 6px 16px;
    background: rgba(0,255,136,0.06);
    border: 1px solid rgba(0,255,136,0.2);
    font-size: 0.65rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--accent3);
    margin-bottom: 32px;
    opacity: 0;
    animation: fadeUp 0.8s forwards 0.1s;
  }
  .status-dot {
    width: 6px; height: 6px;
    background: var(--accent3);
    border-radius: 50%;
    animation: pulse 2s infinite;
  }
  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.4; transform: scale(1.4); }
  }

  @media (max-width: 900px) {
    nav { padding: 16px 24px; }
    .nav-links { display: none; }
    #hero { padding: 120px 24px 80px; }
    .hero-code { display: none; }
    section { padding: 80px 24px; }
    .about-grid, .contact-inner { grid-template-columns: 1fr; gap: 40px; }
    footer { padding: 32px 24px; flex-direction: column; gap: 12px; }
    .divider { margin: 0 24px; }
  }
</style>
</head>
<body>

<div class="cursor" id="cursor"></div>
<div class="cursor-ring" id="cursorRing"></div>

<div class="orb orb-1"></div>
<div class="orb orb-2"></div>

<!-- NAV -->
<nav>
  <div class="nav-logo">G.Art</div>
  <div class="nav-links">
    <a href="#about">About</a>
    <a href="#skills">Skills</a>
    <a href="#projects">Projects</a>
    <a href="#contact">Contact</a>
  </div>
</nav>

<!-- HERO -->
<div id="hero" style="position:relative;">
  <div class="status-badge">
    <span class="status-dot"></span>
    Available for opportunities
  </div>
  <div class="hero-label">Software Developer — Mozambique</div>
  <h1 class="hero-name">Geraldo<br/>Art</h1>
  <p class="hero-role">
    Building <span>web</span> & <span>mobile</span> experiences<br/>
    with clean code and sharp design
  </p>
  <div class="hero-cta">
    <a href="#projects" class="btn-primary">View Work</a>
    <a href="#contact" class="btn-ghost">Get In Touch</a>
  </div>

  <div class="hero-code">
    <span class="c-comment">// who I am</span><br/>
    <span class="c-key">const</span> <span class="c-var">developer</span> = <span class="c-brace">{</span><br/>
    &nbsp;&nbsp;name: <span class="c-str">"Geraldo Art"</span>,<br/>
    &nbsp;&nbsp;stack: <span class="c-brace">[</span><span class="c-str">"React"</span>, <span class="c-str">"Node"</span>, <span class="c-str">"RN"</span><span class="c-brace">]</span>,<br/>
    &nbsp;&nbsp;education: <span class="c-str">"Eng. Informática"</span>,<br/>
    &nbsp;&nbsp;passion: <span class="c-str">"AI + Mobile"</span>,<br/>
    &nbsp;&nbsp;open: <span class="c-var">true</span><br/>
    <span class="c-brace">}</span>;<br/><br/>
    <span class="c-comment">// currently building</span><br/>
    <span class="c-key">const</span> <span class="c-var">status</span> = <span class="c-str">"Creating impact 🚀"</span>;
  </div>

  <div class="scroll-indicator">
    <div class="scroll-line"></div>
    Scroll
  </div>
</div>

<div class="divider"></div>

<!-- ABOUT -->
<section id="about">
  <div class="reveal">
    <div class="section-tag">About me</div>
    <h2 class="section-title">Passionate about<br/>great software</h2>
  </div>
  <div class="about-grid reveal">
    <div>
      <p class="about-text">
        I'm a <strong>Computer Engineering student</strong> and full-stack developer from Mozambique, passionate about building software that makes a real difference. I craft experiences across web and mobile — from pixel-perfect UIs to robust backend systems.<br/><br/>
        I'm deeply interested in <strong>Artificial Intelligence</strong> and how it can be applied to solve real-world problems. Always learning, always shipping.
      </p>
    </div>
    <div class="stats-grid">
      <div class="stat-card">
        <div class="stat-number">3+</div>
        <div class="stat-label">Featured Projects</div>
      </div>
      <div class="stat-card">
        <div class="stat-number">4+</div>
        <div class="stat-label">Languages</div>
      </div>
      <div class="stat-card">
        <div class="stat-number">Web</div>
        <div class="stat-label">& Mobile</div>
      </div>
      <div class="stat-card">
        <div class="stat-number">100%</div>
        <div class="stat-label">Committed</div>
      </div>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- SKILLS -->
<section id="skills">
  <div class="reveal">
    <div class="section-tag">Tech Stack</div>
    <h2 class="section-title">What I work<br/>with</h2>
  </div>
  <div class="skills-grid reveal">
    <div class="skill-group">
      <div class="skill-group-title">Languages</div>
      <div class="skill-tags">
        <span class="skill-tag">JavaScript</span>
        <span class="skill-tag">Python</span>
        <span class="skill-tag">Java</span>
        <span class="skill-tag">C</span>
      </div>
    </div>
    <div class="skill-group">
      <div class="skill-group-title">Frontend</div>
      <div class="skill-tags">
        <span class="skill-tag">React</span>
        <span class="skill-tag">HTML5</span>
        <span class="skill-tag">CSS3</span>
        <span class="skill-tag">Tailwind CSS</span>
      </div>
    </div>
    <div class="skill-group">
      <div class="skill-group-title">Backend</div>
      <div class="skill-tags">
        <span class="skill-tag">Node.js</span>
        <span class="skill-tag">PHP</span>
        <span class="skill-tag">REST APIs</span>
      </div>
    </div>
    <div class="skill-group">
      <div class="skill-group-title">Mobile</div>
      <div class="skill-tags">
        <span class="skill-tag">React Native</span>
        <span class="skill-tag">Expo</span>
      </div>
    </div>
    <div class="skill-group">
      <div class="skill-group-title">Database</div>
      <div class="skill-tags">
        <span class="skill-tag">MySQL</span>
        <span class="skill-tag">SQL</span>
      </div>
    </div>
    <div class="skill-group">
      <div class="skill-group-title">Tools</div>
      <div class="skill-tags">
        <span class="skill-tag">Git</span>
        <span class="skill-tag">GitHub</span>
        <span class="skill-tag">VS Code</span>
        <span class="skill-tag">Figma</span>
      </div>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- PROJECTS -->
<section id="projects">
  <div class="reveal">
    <div class="section-tag">Work</div>
    <h2 class="section-title">Featured<br/>Projects</h2>
  </div>
  <div class="projects-grid reveal">
    <a href="https://github.com/GeraldoChin/food-app" class="project-card" target="_blank">
      <div class="project-num">01</div>
      <div class="project-title">Food App</div>
      <p class="project-desc">A full-featured mobile app for food ordering and discovery, built with React Native. Focuses on smooth UX and intuitive navigation.</p>
      <div class="project-tech">
        <span>React Native</span>
        <span>Mobile</span>
        <span>JavaScript</span>
      </div>
      <div class="project-arrow">View on GitHub →</div>
    </a>
    <a href="https://github.com/GeraldoChin/Sistema-de-Emprestimo-" class="project-card" target="_blank">
      <div class="project-num">02</div>
      <div class="project-title">Loan System</div>
      <p class="project-desc">A complete loan management system built with React. Handles financial tracking, client management, and reporting with a clean dashboard interface.</p>
      <div class="project-tech">
        <span>React</span>
        <span>JavaScript</span>
        <span>CSS</span>
      </div>
      <div class="project-arrow">View on GitHub →</div>
    </a>
    <a href="https://github.com/GeraldoChin/Portifolio-React-Admin" class="project-card" target="_blank">
      <div class="project-num">03</div>
      <div class="project-title">Portfolio & Admin</div>
      <p class="project-desc">A professional portfolio website with an integrated admin panel. Showcases projects and skills with a content management interface.</p>
      <div class="project-tech">
        <span>React</span>
        <span>Admin Panel</span>
        <span>Responsive</span>
      </div>
      <div class="project-arrow">View on GitHub →</div>
    </a>
  </div>
</section>

<div class="divider"></div>

<!-- CONTACT -->
<section id="contact">
  <div class="reveal">
    <div class="section-tag">Contact</div>
    <h2 class="section-title">Let's build<br/>something</h2>
  </div>
  <div class="contact-inner reveal">
    <div>
      <p class="contact-text">
        I'm currently open to new opportunities — freelance projects, full-time roles, or collaborations. If you have something interesting in mind, let's talk.
      </p>
    </div>
    <div class="contact-links">
      <a href="https://linkedin.com" class="contact-link" target="_blank">
        <div class="contact-link-icon">in</div>
        <div>
          <div style="font-size:0.65rem;color:var(--muted);letter-spacing:0.1em;margin-bottom:2px;">LINKEDIN</div>
          Geraldo Art
        </div>
      </a>
      <a href="mailto:seuemail@gmail.com" class="contact-link">
        <div class="contact-link-icon">@</div>
        <div>
          <div style="font-size:0.65rem;color:var(--muted);letter-spacing:0.1em;margin-bottom:2px;">EMAIL</div>
          seuemail@gmail.com
        </div>
      </a>
      <a href="https://github.com/GeraldoChin" class="contact-link" target="_blank">
        <div class="contact-link-icon">gh</div>
        <div>
          <div style="font-size:0.65rem;color:var(--muted);letter-spacing:0.1em;margin-bottom:2px;">GITHUB</div>
          GeraldoChin
        </div>
      </a>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="footer-logo">G.Art</div>
  <div>Geraldo Art — Software Developer — Mozambique</div>
  <div style="color:var(--accent);">Open to work ✦</div>
</footer>

<script>
  // Cursor
  const cursor = document.getElementById('cursor');
  const ring = document.getElementById('cursorRing');
  let mx = 0, my = 0, rx = 0, ry = 0;
  document.addEventListener('mousemove', e => { mx = e.clientX; my = e.clientY; });
  function animateCursor() {
    cursor.style.left = mx + 'px';
    cursor.style.top = my + 'px';
    rx += (mx - rx) * 0.12;
    ry += (my - ry) * 0.12;
    ring.style.left = rx + 'px';
    ring.style.top = ry + 'px';
    requestAnimationFrame(animateCursor);
  }
  animateCursor();

  document.querySelectorAll('a, button').forEach(el => {
    el.addEventListener('mouseenter', () => {
      cursor.style.width = '20px';
      cursor.style.height = '20px';
      ring.style.width = '60px';
      ring.style.height = '60px';
    });
    el.addEventListener('mouseleave', () => {
      cursor.style.width = '12px';
      cursor.style.height = '12px';
      ring.style.width = '40px';
      ring.style.height = '40px';
    });
  });

  // Reveal on scroll
  const reveals = document.querySelectorAll('.reveal');
  const observer = new IntersectionObserver(entries => {
    entries.forEach((entry, i) => {
      if (entry.isIntersecting) {
        setTimeout(() => entry.target.classList.add('visible'), i * 100);
      }
    });
  }, { threshold: 0.1 });
  reveals.forEach(el => observer.observe(el));
</script>
</body>
</html>
