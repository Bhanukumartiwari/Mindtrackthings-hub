# # Mindtrackthings-hub
<!-- Modern Glassmorphism Advanced UI Example -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>MindTrackThings - Advanced UI</title>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap" rel="stylesheet">
  <style>
    body {
      background: linear-gradient(135deg, #10182f, #193866 70%, #1a213a);
      min-height: 100vh;
      font-family: 'Montserrat', Arial, sans-serif;
      margin: 0;
      overflow-x: hidden;
      color: #f0f7fb;
    }
    .background-particles {
      position: fixed;
      top: 0; left: 0;
      width: 100vw; height: 100vh;
      z-index: 1;
      pointer-events: none;
      /* You can layer a canvas or SVG particle animation here! */
      background: url('https://www.transparenttextures.com/patterns/stardust.png') repeat;
      opacity: 0.15;
      animation: moveBG 60s linear infinite;
    }
    @keyframes moveBG {
      0% { background-position: 0 0; }
      100% { background-position: 1000px 1000px; }
    }
    .container {
      position: relative;
      z-index: 2;
      max-width: 1100px;
      margin: 0 auto;
      padding: 2rem;
    }
    .header {
      text-align: center;
      color: #86c5ff;
      font-size: 2.8rem;
      letter-spacing: 3px;
      font-weight: 700;
      margin-top: 0;
      line-height: 1.2;
      text-shadow: 0 4px 32px #2196f380;
    }
    .mode-toggle {
      position: absolute;
      top: 2rem;
      right: 2rem;
      background: rgba(0,0,0,0.3);
      border: none;
      color: #94d8ff;
      border-radius: 50%;
      width: 48px; height: 48px;
      display: flex; align-items: center; justify-content: center;
      cursor: pointer;
      box-shadow: 0 2px 12px #1976d280;
      font-size: 1.5rem;
      transition: background 0.2s;
    }
    .mode-toggle:hover { background: #1976d277; }
    .chip-group {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 12px;
      margin: 2rem 0;
    }
    .chip {
      padding: .7em 1.4em;
      border-radius: 2em;
      font-weight: 500;
      letter-spacing: 1px;
      background: rgba(255,255,255,0.10);
      border: 2px solid #2196f3;
      color: #fff;
      cursor: pointer;
      box-shadow: 0 2px 12px #2196f350;
      transition: 0.3s all;
      font-size: 1em;
      backdrop-filter: blur(6px);
    }
    .chip.active, .chip:hover {
      background: linear-gradient(90deg, #73e8fa77, #3576ff);
      color: #10182f;
      border-color: #ffffff;
      font-weight: 700;
    }
    .facts {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(290px,1fr));
      gap: 2em;
    }
    .fact-card {
      background: rgba(24, 37, 68, 0.82);
      border-radius: 1.4em;
      border: 2.5px solid #57c7ed55;
      box-shadow: 0 6px 48px #348cf9cc;
      padding: 2em 1.25em;
      position: relative;
      overflow: hidden;
      min-height: 150px;
      cursor: pointer;
      transition: transform 0.18s cubic-bezier(.24,.72,.58,1.3), box-shadow 0.2s;
      will-change: transform;
    }
    .fact-card:hover {
      transform: scale(1.06) rotate(-2deg);
      box-shadow: 0 10px 60px #38d7fd;
      border-color: #fff;
    }
    .fact-category {
      font-size: 0.95em;
      color: #6ad0fb;
      text-transform: uppercase;
      letter-spacing: 0.04em;
      margin-bottom: 0.4em;
      display: inline-block;
    }
    .fact-main {
      font-size: 1.13em;
      color: #f7fbfc;
      margin-bottom: .5em;
      font-weight: 600;
    }
    /* Stylish Scrollbar */
    ::-webkit-scrollbar { width: .7em; }
    ::-webkit-scrollbar-thumb {
      background: #3599ff88; border-radius: 9px;
      border: 3px solid #09152ad1;
    }
    @media (max-width: 600px) {
      .header { font-size: 2em;}
      .container { padding: .5rem;}
      .facts { grid-template-columns: 1fr;}
    }
  </style>
</head>
<body>
<div class="background-particles"></div>
<div class="container">
    <button class="mode-toggle" title="Toggle dark/light mode" onclick="toggleMode()">🌙</button>
    <div class="header">🎯 MindTrackThings</div>
    <div class="chip-group">
      <span class="chip active" data-cat="all">All</span>
      <span class="chip" data-cat="Science">Science</span>
      <span class="chip" data-cat="Technology">Technology</span>
      <span class="chip" data-cat="History">History</span>
      <span class="chip" data-cat="GK">GK</span>
      <span class="chip" data-cat="Current Affairs">Current Affairs</span>
      <span class="chip" data-cat="Polity">Polity</span>
    </div>
    <button class="chip" style="margin-bottom:2em;" onclick="generateFacts()">🚀 Generate 10 Quick Facts</button>
    <div class="facts" id="factsContainer">
      <!-- Fact cards will appear here -->
    </div>
</div>
<script>
  // Fact cards (sample only -- you should fill with your previous factsDB)
  const factsDB = [
    {category:"Science",text:"Gaganyaan is ISRO's first crewed mission to space, slated for 2025."},
    {category:"Technology",text:"Google will build India's largest AI data hub in Andhra Pradesh (2025)."},
    {category:"History",text:"The Indian National Congress was founded in 1885."},
    {category:"GK",text:"India has 28 states and 8 union territories."},
    {category:"Current Affairs",text:"UIDAI launched SITAA in October 2025."},
    {category:"Polity",text:"Article 370 was abrogated in August 2019."},
    // ...add more
  ];
  let currentCat = "all";
  function fillFacts() {
    const box = document.getElementById('factsContainer');
    let list = factsDB.filter(f=>currentCat==='all'||f.category===currentCat);
    let used = [];
    while(used.length < Math.min(10, list.length)) {
      let idx = Math.floor(Math.random()*list.length);
      if(!used.includes(idx)) used.push(idx);
    }
    box.innerHTML = used.map(i=>`
      <div class="fact-card">
        <div class="fact-category">${list[i].category}</div>
        <div class="fact-main">${list[i].text}</div>
      </div>
    `).join('');
  }
  document.querySelectorAll('.chip').forEach(c=>c.onclick=function(){ 
    document.querySelectorAll('.chip').forEach(cc=>cc.classList.remove('active'));
    this.classList.add('active');
    currentCat = this.dataset.cat;
    fillFacts();
  });
  function generateFacts() { fillFacts(); }
  fillFacts();
  // Mode toggle simple effect
  function toggleMode() {
    document.body.classList.toggle('lightmode');
    // expand for light/dark CSS
  }
</script>
</body>
</html>



