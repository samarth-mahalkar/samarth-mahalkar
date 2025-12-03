<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>My Animated Profile</title>

  <style>
    /* ========== Reset & base ========== */
    :root{
      --bg1: #0f1724;
      --bg2: #071023;
      --accent: #7c5cff;
      --glass: rgba(255,255,255,0.06);
      --card: rgba(255,255,255,0.03);
      --glass-border: rgba(255,255,255,0.08);
      --text: #e6eef8;
      --muted: #9fb1c8;
      --radius: 14px;
      --maxw: 980px;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      color:var(--text);
      background: linear-gradient(120deg,var(--bg1) 0%, var(--bg2) 50%, #022 100%);
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:40px;
    }

    /* animated radial shine */
    .bg-shine{
      position:fixed;
      inset:0;
      pointer-events:none;
      background: radial-gradient(circle at 10% 20%, rgba(124,92,255,0.12), transparent 6%),
                  radial-gradient(circle at 90% 80%, rgba(34,197,94,0.06), transparent 5%);
      mix-blend-mode:screen;
      opacity:0.9;
      z-index:0;
      animation: rotate-shine 20s linear infinite;
    }
    @keyframes rotate-shine{
      from{ transform: rotate(0deg) scale(1) }
      to{ transform: rotate(360deg) scale(1.02) }
    }

    .container{
      max-width:var(--maxw);
      width:100%;
      z-index:1;
      position:relative;
      display:grid;
      grid-template-columns: 320px 1fr;
      gap:28px;
      align-items:start;
    }

    /* ========== Sidebar ========== */
    .sidebar{
      background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border:1px solid var(--glass-border);
      border-radius:var(--radius);
      padding:26px;
      box-shadow: 0 10px 30px rgba(2,6,23,0.6);
      backdrop-filter: blur(6px);
    }

    .avatar-wrap{
      display:flex;
      gap:16px;
      align-items:center;
    }
    .avatar{
      width:84px;
      height:84px;
      flex:0 0 84px;
      border-radius:50%;
      overflow:hidden;
      border: 2px solid rgba(255,255,255,0.08);
      background: linear-gradient(135deg, rgba(255,255,255,0.03), rgba(255,255,255,0.01));
      display:grid;
      place-items:center;
      box-shadow: 0 6px 18px rgba(7,10,24,0.6);
      transform: translateY(0);
      animation: float 6s ease-in-out infinite;
    }
    .avatar img{ width:100%; height:100%; object-fit:cover; display:block; }

    @keyframes float{
      0% { transform: translateY(0) }
      50% { transform: translateY(-10px) }
      100% { transform: translateY(0) }
    }

    .name{
      font-weight:700;
      font-size:1.15rem;
      margin:0;
    }
    .role{ color:var(--muted); font-size:0.9rem; margin-top:6px }

    .socials{ margin-top:16px; display:flex; gap:10px; }
    .socials a{
      text-decoration:none;
      color:var(--muted);
      font-size:0.95rem;
      padding:8px 10px;
      border-radius:10px;
      border:1px solid transparent;
      background:transparent;
    }
    .socials a:hover{
      color:var(--text);
      background: linear-gradient(90deg, rgba(124,92,255,0.12), rgba(34,197,94,0.04));
      border-color: rgba(255,255,255,0.04);
    }

    /* About box */
    .about{
      margin-top:18px;
      background: var(--glass);
      padding:14px;
      border-radius:12px;
      border:1px solid var(--glass-border);
      font-size:0.95rem;
      line-height:1.45;
      color:var(--muted);
      min-height:76px;
      position:relative;
      overflow:hidden;
    }

    /* typing cursor for JS typed text */
    .typing-cursor{
      display:inline-block;
      width:2px;
      background:var(--text);
      margin-left:6px;
      animation: blink 1s steps(1) infinite;
      vertical-align:bottom;
      height:1.05em;
      opacity:0.9;
    }
    @keyframes blink{ 0%{opacity:1}50%{opacity:0}100%{opacity:1} }

    .tags{
      margin-top:14px;
      display:flex;
      gap:8px;
      flex-wrap:wrap;
    }
    .tag{
      background:rgba(255,255,255,0.03);
      border:1px solid rgba(255,255,255,0.03);
      padding:6px 10px;
      font-size:0.85rem;
      border-radius:999px;
      color:var(--muted);
    }

    /* ========== Main area ========== */
    .main{
      display:flex;
      flex-direction:column;
      gap:20px;
    }

    .header-main{
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:12px;
    }
    .title{
      font-size:1.2rem;
      font-weight:700;
      color:var(--text);
      margin:0;
    }
    .subtitle{ color:var(--muted); font-size:0.95rem }

    .projects{
      display:grid;
      grid-template-columns: repeat(auto-fit, minmax(220px,1fr));
      gap:14px;
    }
    .card{
      background: linear-gradient(180deg, rgba(255,255,255,0.014), rgba(255,255,255,0.01));
      border:1px solid rgba(255,255,255,0.04);
      padding:14px;
      border-radius:12px;
      overflow:hidden;
      transition: transform 350ms cubic-bezier(.2,.9,.3,1), box-shadow 350ms;
      transform: translateY(0);
      cursor:default;
    }
    .card:hover{
      transform: translateY(-8px) scale(1.01);
      box-shadow: 0 18px 40px rgba(2,6,23,0.6);
    }
    .card h4{ margin:0 0 6px 0; font-size:1rem }
    .card p{ margin:0; color:var(--muted); font-size:0.92rem; line-height:1.4 }

    /* project tag */
    .project-meta{ display:flex; gap:8px; margin-top:10px; align-items:center; flex-wrap:wrap }
    .chip{ font-size:0.78rem; padding:6px 8px; border-radius:8px; background:rgba(255,255,255,0.02); border:1px solid rgba(255,255,255,0.02); color:var(--muted) }

    /* Random file highlight */
    .random-box{
      margin-top:10px;
      background: linear-gradient(90deg, rgba(124,92,255,0.04), rgba(34,197,94,0.02));
      border-radius:12px;
      padding:14px;
      border:1px solid rgba(255,255,255,0.03);
      color:var(--text);
    }
    .random-text{ font-style:italic; color:#ffffffcc; font-size:0.98rem; min-height:46px }

    /* footer small */
    .footer{
      margin-top:10px;
      color:var(--muted);
      font-size:0.85rem;
    }

    /* responsive */
    @media (max-width:920px){
      .container{ grid-template-columns: 1fr; padding:12px; }
      .avatar{ width:76px; height:76px }
    }

    /* prefers reduced motion */
    @media (prefers-reduced-motion: reduce){
      .bg-shine, .avatar, .card, .avatar-wrap, .container { animation: none !important; transition: none !important; }
      .typing-cursor{ display:none; }
    }
  </style>
</head>
<body>
  <div class="bg-shine" aria-hidden="true"></div>

  <div class="container" role="main">
    <!-- SIDEBAR -->
    <aside class="sidebar" aria-label="Profile sidebar">
      <div class="avatar-wrap">
        <div class="avatar" role="img" aria-label="Profile picture">
          <!-- Replace with your own picture path or URL -->
          <img src="https://avatars.githubusercontent.com/u/583231?v=4" alt="Avatar">
        </div>

        <div>
          <p class="name">Your Name</p>
          <p class="role">Frontend Developer · Hobbyist · Student</p>

          <div class="socials" aria-hidden="false">
            <a href="#" target="_blank" rel="noopener">GitHub</a>
            <a href="#" target="_blank" rel="noopener">Twitter</a>
            <a href="#" target="_blank" rel="noopener">Website</a>
          </div>
        </div>
      </div>

      <div class="about" id="about">
        <!-- JS will type text into this element -->
        <div id="about-text" aria-live="polite"></div><span class="typing-cursor" aria-hidden="true"></span>
      </div>

      <div class="tags" aria-hidden="false">
        <span class="tag">JavaScript</span>
        <span class="tag">HTML</span>
        <span class="tag">CSS</span>
        <span class="tag">Node</span>
      </div>

      <div class="footer">Tip: add this to a GitHub Pages site to show this as your profile page.</div>
    </aside>

    <!-- MAIN -->
    <section class="main" aria-label="Main content">
      <div class="header-main">
        <div>
          <h2 class="title">Featured Projects</h2>
          <div class="subtitle">A few things I'm playing with</div>
        </div>
        <div class="subtitle">Updated: <strong id="updated-date">—</strong></div>
      </div>

      <div class="projects" aria-live="polite">
        <article class="card" role="article">
          <h4>Project: Random File Explorer</h4>
          <p>A small utility to pick random lines from files for inspiration — used to seed README intros and testing data.</p>
          <div class="project-meta">
            <span class="chip">JS</span>
            <span class="chip">Static</span>
            <span class="chip">Demo</span>
          </div>
        </article>

        <article class="card" role="article">
          <h4>Project: Animated Portfolio</h4>
          <p>A tiny portfolio site with smooth micro-interactions and an animated header.</p>
          <div class="project-meta">
            <span class="chip">CSS</span>
            <span class="chip">Accessibility</span>
          </div>
        </article>

        <article class="card" role="article">
          <h4>Project: README Generator</h4>
          <p>Generates README content from templates and optional random file lines.</p>
          <div class="project-meta">
            <span class="chip">Python</span>
            <span class="chip">CLI</span>
          </div>
        </article>
      </div>

      <div class="random-box" aria-label="Random file highlight">
        <strong>Random File Highlight</strong>
        <div class="random-text" id="random-text">Loading inspirational line…</div>
        <small class="footer">Source: <code>random.txt</code> (put multiple lines, the page selects one randomly)</small>
      </div>
    </section>
  </div>

  <script>
    /* ========= small helper: typing effect for the About box ========= */
    (function(){
      const aboutText = [
        "Hello — I'm a developer who loves turning ideas into code.",
        "I build small tools, experiment with animation, and enjoy simple UIs.",
        "I also use small random files as inspiration for README intros."
      ].join(" ");

      // typed-typing effect
      const target = document.getElementById('about-text');
      const cursor = document.querySelector('.typing-cursor');

      function typeText(text, el, speed=24){
        el.textContent = "";
        let i = 0;
        function step(){
          if (i < text.length){
            el.textContent += text.charAt(i);
            i++;
            setTimeout(step, speed + Math.random()*30);
          } else {
            // keep cursor blinking
          }
        }
        step();
      }

      // If user prefers reduced motion, just show text
      const prefersReduced = window.matchMedia && window.matchMedia('(prefers-reduced-motion: reduce)').matches;
      if (prefersReduced){
        target.textContent = aboutText;
        if (cursor) cursor.style.display = 'none';
      } else {
        typeText(aboutText, target, 22);
      }
    })();

    /* ========= random.txt fetcher =========
       Save a file named 'random.txt' in the same folder with multiple lines.
       The script will pick one line at random and show it.
    */
    (function(){
      const out = document.getElementById('random-text');
      const updatedDateEl = document.getElementById('updated-date');

      // show current date
      const now = new Date();
      updatedDateEl.textContent = now.toLocaleDateString(undefined, { year: 'numeric', month: 'short', day: 'numeric' });

      fetch('random.txt')
        .then(res => {
          if (!res.ok) throw new Error('No random.txt found');
          return res.text();
        })
        .then(text => {
          const lines = text.split(/\r?\n/).map(s => s.trim()).filter(Boolean);
          if (!lines.length) throw new Error('random.txt empty');
          // pick a random line
          const line = lines[Math.floor(Math.random()*lines.length)];
          // small fade-in
          out.style.opacity = 0;
          out.textContent = `"${line}"`;
          out.style.transition = 'opacity 450ms ease';
          requestAnimationFrame(()=> out.style.opacity = 1);
        })
        .catch(err => {
          // fallback inspirational line
          out.textContent = "\"Creativity starts with randomness.\"";
        });
    })();
  </script>
</body>
</html>
