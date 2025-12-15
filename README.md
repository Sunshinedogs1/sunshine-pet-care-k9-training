import os, json, textwrap

# Create project directory
project_dir = "sunshine-pet-care-k9-training"
os.makedirs(project_dir, exist_ok=True)
assets_dir = os.path.join(project_dir, "assets")
os.makedirs(assets_dir, exist_ok=True)

# Create a simple logo SVG (cartoon sun with paw)
logo_svg = '''<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 200">
  <defs>
    <linearGradient id="sunGrad" x1="0" x2="0" y1="0" y2="1">
      <stop offset="0%" stop-color="#FFD23F"/>
      <stop offset="100%" stop-color="#FF9F1C"/>
    </linearGradient>
  </defs>
  <circle cx="100" cy="100" r="38" fill="url(#sunGrad)" stroke="#F77F00" stroke-width="4"/>
  <!-- Rays -->
  ''' + "\n".join([f"<line x1='100' y1='100' x2='{100+65*__import__('math').cos(a)}' y2='{100+65*__import__('math').sin(a)}' stroke='#F77F00' stroke-width='6' stroke-linecap='round'/>" for a in [i*__import__('math').pi/8 for i in range(16)]]) + '''
  <!-- Paw print -->
  <circle cx="100" cy="100" r="16" fill="#6A4C93"/>
  <circle cx="80" cy="85" r="6" fill="#6A4C93"/>
  <circle cx="120" cy="85" r="6" fill="#6A4C93"/>
  <circle cx="85" cy="115" r="6" fill="#6A4C93"/>
  <circle cx="115" cy="115" r="6" fill="#6A4C93"/>
</svg>'''

with open(os.path.join(assets_dir, "logo.svg"), "w") as f:
    f.write(logo_svg)

# CSS
style_css = '''/* Sunshine Pet Care & K9 Training - Theme */
@import url('https://fonts.googleapis.com/css2?family=Baloo+2:wght@400;600;700&family=Nunito:wght@400;600;800&display=swap');
:root {
  --sunshine: #FFD23F; /* sunny yellow */
  --orange: #FF9F1C;
  --purple: #6A4C93;
  --blue: #1B9AAA;
  --green: #2CA58D;
  --bg: #FFF9E9;
  --text: #222;
}
* { box-sizing: border-box; }
html, body { margin: 0; padding: 0; background: var(--bg); color: var(--text); font-family: 'Nunito', system-ui, -apple-system; }
header {
  position: sticky; top: 0; z-index: 1000; background: white; box-shadow: 0 6px 18px rgba(0,0,0,.08);
}
.navbar { display: flex; align-items: center; gap: 1rem; padding: .75rem 1rem; max-width: 1200px; margin: 0 auto; }
.navbar img { width: 56px; height: 56px; }
.brand { font-family: 'Baloo 2'; font-weight: 800; font-size: 1.4rem; color: var(--purple); }
.navlinks { margin-left: auto; display: flex; flex-wrap: wrap; gap: .5rem; }
.navlinks a { text-decoration: none; color: var(--purple); background: #f4e8ff; padding: .5rem .8rem; border-radius: 999px; font-weight: 700; }
.navlinks a:hover { background: var(--sunshine); color: #3b2a63; }

.hero { display: grid; grid-template-columns: 1.1fr .9fr; gap: 2rem; align-items: center; max-width: 1200px; margin: 0 auto; padding: 2rem 1rem; }
.hero-card { background: white; border-radius: 24px; padding: 2rem; box-shadow: 0 12px 24px rgba(0,0,0,.08); }
.hero h1 { font-family: 'Baloo 2'; font-size: clamp(2rem, 4vw, 3rem); color: var(--orange); margin: 0 0 .5rem; }
.hero p { font-size: 1.2rem; }
.cta { display: flex; gap: 1rem; margin-top: 1rem; }
.btn { border: none; border-radius: 16px; padding: .8rem 1.2rem; font-weight: 800; cursor: pointer; }
.btn.primary { background: var(--orange); color: #fff; }
.btn.secondary { background: var(--blue); color: #fff; }
.btn.tertiary { background: var(--green); color: #fff; }
.btn:hover { filter: brightness(1.05); transform: translateY(-1px); transition: all .2s ease; }

.section { max-width: 1200px; margin: 0 auto; padding: 2rem 1rem; }
.section h2 { font-family: 'Baloo 2'; color: var(--purple); font-size: clamp(1.6rem, 3vw, 2.2rem); }
.grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: 1rem; }
.card { background: white; border-radius: 20px; padding: 1rem; box-shadow: 0 8px 18px rgba(0,0,0,.06); }
.card h3 { margin-top: 0; }
.badge { display: inline-flex; align-items: center; gap: .4rem; background: #fff3c4; color: #6a4c00; padding: .3rem .6rem; border-radius: 999px; font-weight: 700; }

/* Progress tracker */
.progress-wrapper { background: white; border-radius: 20px; padding: 1rem; box-shadow: 0 8px 18px rgba(0,0,0,.06); }
.progress-bar { height: 20px; background: #eee; border-radius: 999px; overflow: hidden; }
.progress-fill { height: 100%; width: 0%; background: linear-gradient(90deg, var(--green), var(--blue)); transition: width .8s ease; }
.levels { display: grid; grid-template-columns: repeat(5, 1fr); gap: .5rem; margin-top: 1rem; }
.level { text-align: center; padding: .6rem; border-radius: 12px; background: #f6fafe; border: 2px dashed #cde3f1; }
.level.unlocked { background: #e8fff6; border-color: #b5f0d9; }
.trophy { font-size: 2rem; }
.ribbon { display:inline-block; background: var(--sunshine); color:#5b4400; padding:.2rem .5rem; border-radius:8px; font-weight:800; }

footer { background: #fff; border-top: 1px solid #eee; }
footer .section { display: grid; gap: 1rem; }
.small { font-size: .9rem; color: #555; }

/* Responsive */
@media (max-width: 800px) { .hero { grid-template-columns: 1fr; } }
'''

with open(os.path.join(project_dir, "style.css"), "w") as f:
    f.write(style_css)

# JavaScript with basic interactivity and localStorage progress tracking
script_js = '''// Sunshine Pet Care & K9 Training - Interactivity
const state = {
  progress: 0,         // percent
  level: 1,            // 1..5
  rewards: []          // strings
};

function loadState(){
  try {
    const saved = JSON.parse(localStorage.getItem('sunshineProgress'));
    if(saved){ Object.assign(state, saved); }
  } catch(e){ console.warn('No saved state'); }
}

function saveState(){
  localStorage.setItem('sunshineProgress', JSON.stringify(state));
}

function updateUI(){
  document.querySelector('.progress-fill').style.width = state.progress + '%';
  document.querySelectorAll('.level').forEach((el, idx)=>{
    const levelNum = idx+1;
    el.classList.toggle('unlocked', levelNum <= state.level);
  });
  const rewardList = document.getElementById('reward-list');
  rewardList.innerHTML = '';
  state.rewards.forEach(r=>{
    const li = document.createElement('li');
    li.textContent = r;
    rewardList.appendChild(li);
  });
}

function award(type){
  const map = {
    trophy: '🏆 Trophy earned! Obedience level completed.',
    ribbon: '🎗️ Ribbon earned! Manners milestone achieved.',
    medal: '🥇 Medal earned! Trick class certified.'
  };
  const msg = map[type] || '⭐ Milestone achieved!';
  state.rewards.push(msg);
  toast(msg);
  saveState();
  updateUI();
}

function toast(msg){
  const t = document.createElement('div');
  t.className = 'toast';
  t.textContent = msg;
  Object.assign(t.style, {
    position:'fixed', bottom:'20px', left:'50%', transform:'translateX(-50%)',
    background:'#1B9AAA', color:'#fff', padding:'10px 16px', borderRadius:'999px',
    boxShadow:'0 8px 18px rgba(0,0,0,.2)', zIndex:9999, fontWeight:'800'
  });
  document.body.appendChild(t);
  setTimeout(()=> t.remove(), 3000);
}

function incrementProgress(amount){
  state.progress = Math.min(100, state.progress + amount);
  const newLevel = Math.min(5, Math.floor(state.progress/20) + 1);
  if(newLevel > state.level){
    state.level = newLevel;
    award('trophy');
  }
  saveState();
  updateUI();
}

// Demo buttons
function bindDemo(){
  document.getElementById('btn-progress').addEventListener('click', ()=> incrementProgress(10));
  document.getElementById('btn-ribbon').addEventListener('click', ()=> award('ribbon'));
  document.getElementById('btn-medal').addEventListener('click', ()=> award('medal'));
}

// Calendly embed init (replace with your actual link)
function initCalendly(){
  const script = document.createElement('script');
  script.src = 'https://assets.calendly.com/assets/external/widget.js';
  script.async = true;
  document.body.appendChild(script);
}

// Ecwid store embed (free plan supports up to 5 products). Replace STORE_ID.
function initStore(){
  const s1 = document.createElement('script');
  s1.src = 'https://app.ecwid.com/script.js?STORE_ID&data_platform=code&data_date=' + Date.now();
  s1.async = true;
  document.body.appendChild(s1);
  s1.onload = () => {
    if(window.xProductBrowser){
      window.xProductBrowser('categoriesPerRow=3','views=grid','categoryView=grid','searchView=list','id=my-store');
    }
  };
}

// YouTube playlist embed: replace PLAYLIST_ID
function initYouTube(){
  const iframe = document.getElementById('yt-playlist');
  iframe.src = 'https://www.youtube.com/embed/videoseries?list=PLAYLIST_ID';
}

// On load
window.addEventListener('DOMContentLoaded', ()=>{
  loadState();
  updateUI();
  bindDemo();
  initCalendly();
  initStore();
  initYouTube();
});
'''

with open(os.path.join(project_dir, "script.js"), "w") as f:
    f.write(script_js)

# HTML
index_html = '''<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Sunshine Pet Care & K9 Training</title>
  <meta name="description" content="Family-friendly, balanced dog training focused on obedience, manners, fun tricks, leash training, and polite greetings. Schedule, watch videos, and shop gear." />
  <link rel="stylesheet" href="style.css" />
  <link rel="icon" href="assets/logo.svg" type="image/svg+xml" />
</head>
<body>
  <header>
    <nav class="navbar">
      <img src="assets/logo.svg" alt="Sunshine Pet Care logo" />
      <div class="brand">Sunshine Pet Care & K9 Training</div>
      <div class="navlinks">
        <a href="#schedule">Schedule</a>
        <a href="#classes">Classes</a>
        <a href="#videos">Videos</a>
        <a href="#shop">Shop</a>
        <a href="#progress">Progress</a>
        <a href="#contact">Contact</a>
      </div>
    </nav>
  </header>

  <section class="hero">
    <div class="hero-card">
      <h1>Balanced Training for Happy Families</h1>
      <p>
        We use <strong>balanced training</strong> to build reliable <strong>obedience</strong> and everyday
        <strong>manners</strong>. Join fun <em>dog trick</em> classes and specialized sessions like <em>leash
        skills</em> and <em>polite greetings</em>.
      </p>
      <div class="cta">
        <a class="btn primary" href="#schedule">Book a Session</a>
        <a class="btn secondary" href="#videos">Watch & Learn</a>
        <a class="btn tertiary" href="#shop">Shop Dog Gear</a>
      </div>
      <p class="small">Family-friendly • Positive & fair • Results you can see</p>
    </div>
    <div>
      <img src="assets/logo.svg" alt="Sunshine cartoon logo" style="width:100%; max-width:360px; display:block; margin:auto;" />
    </div>
  </section>

  <section id="schedule" class="section">
    <h2>Schedule Your Training</h2>
    <div class="card">
      <p>Pick a time that works for you. New clients: start with the Obedience & Manners Assessment.</p>
      <!-- Calendly inline widget. Replace YOUR_LINK with your actual Calendly link -->
      <div class="calendly-inline-widget" data-url="https://calendly.com/YOUR_LINK" style="min-width:320px;height:700px;"></div>
    </div>
  </section>

  <section id="classes" class="section">
    <h2>Classes & Programs</h2>
    <div class="grid">
      <div class="card">
        <span class="badge">Obedience & Manners</span>
        <h3>Foundations</h3>
        <p>Sit, down, place, recall, leash manners, and polite greetings. Balanced, fair, and fun.</p>
      </div>
      <div class="card">
        <span class="badge">Specialized</span>
        <h3>Leash Training</h3>
        <p>Loose-leash walking, impulse control, and handler focus in real-world settings.</p>
      </div>
      <div class="card">
        <span class="badge">Fun</span>
        <h3>Dog Tricks</h3>
        <p>Spin, bow, shake, roll-over, and more! Build confidence while you bond.</p>
      </div>
      <div class="card">
        <span class="badge">Social Skills</span>
        <h3>Greeting People</h3>
        <p>Polite sit-to-greet, no jumping, and calm introductions to friends and family.</p>
      </div>
    </div>
  </section>

  <section id="videos" class="section">
    <h2>Training Videos</h2>
    <div class="card">
      <p>Watch our tutorials and class previews. Subscribe for new uploads!</p>
      <div style="position:relative;padding-bottom:56.25%;height:0;overflow:hidden;border-radius:16px;">
        <iframe id="yt-playlist" style="position:absolute;top:0;left:0;width:100%;height:100%;" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
      </div>
    </div>
  </section>

  <section id="shop" class="section">
    <h2>Shop: Dog Training Gear</h2>
    <div class="card">
      <p>Collars, leashes, treat pouches, toys, and more. Secure checkout.</p>
      <!-- Ecwid store container -->
      <div id="my-store-1000"></div>
      <div id="my-store"></div>
    </div>
  </section>

  <section id="progress" class="section">
    <h2>Progress Tracker</h2>
    <div class="progress-wrapper">
      <div class="progress-bar"><div class="progress-fill"></div></div>
      <div class="levels">
        <div class="level">Level 1<br/><span class="ribbon">Starter</span></div>
        <div class="level">Level 2<br/><span class="ribbon">Leash Basics</span></div>
        <div class="level">Level 3<br/><span class="ribbon">Manners Pro</span></div>
        <div class="level">Level 4<br/><span class="ribbon">Trickster</span></div>
        <div class="level">Level 5<br/><span class="ribbon">Sunshine Star</span></div>
      </div>
      <div style="margin-top:1rem; display:flex; gap:.5rem; flex-wrap:wrap;">
        <button id="btn-progress" class="btn primary">+10% Progress</button>
        <button id="btn-ribbon" class="btn secondary">Award Ribbon</button>
        <button id="btn-medal" class="btn tertiary">Award Medal</button>
      </div>
      <h3>Virtual Rewards</h3>
      <ul id="reward-list"></ul>
      <p class="small">Note: Progress is saved in your browser. For multi-device tracking, ask us to enable account-based tracking.</p>
    </div>
  </section>

  <section id="contact" class="section">
    <h2>Contact & Location</h2>
    <div class="grid">
      <div class="card">
        <h3>Get in Touch</h3>
        <p>Email: <a href="mailto:hello@sunshinepetcareandk9training.com">hello@sunshinepetcareandk9training.com</a></p>
        <p>Phone: <a href="tel:+1-555-555-5555">(555) 555-5555</a></p>
        <p class="small">Serving families in the greater area.</p>
      </div>
      <div class="card">
        <h3>Social</h3>
        <p><a href="#">Instagram</a> • <a href="#">Facebook</a> • <a href="#">YouTube</a></p>
      </div>
    </div>
  </section>

  <footer>
    <div class="section">
      <div class="small">© <span id="year"></span> Sunshine Pet Care & K9 Training. All rights reserved.</div>
      <div class="small">Training style: Balanced training focused on obedience & manners. Fun classes: tricks, leash training, greeting people.</div>
    </div>
  </footer>

  <script src="script.js"></script>
  <script>document.getElementById('year').textContent = new Date().getFullYear();</script>
</body>
</html>'''

with open(os.path.join(project_dir, "index.html"), "w") as f:
    f.write(index_html)

# README with setup instructions
readme_md = '''# Sunshine Pet Care & K9 Training — Starter Site

This is a colorful, family-friendly starter website tailored for a balanced dog training business. It includes:

- **Scheduling** via Calendly embed
- **Training videos** via YouTube playlist embed
- **Shop** via Ecwid free store embed (up to 5 products on the free plan)
- **Animated progress tracker** with virtual rewards (localStorage)
- Cartoon logo and playful design

## 1) Edit your embeds

- **Calendly**: Replace `YOUR_LINK` in `index.html` with your real Calendly scheduling URL.
- **YouTube**: Replace `PLAYLIST_ID` in `script.js` with your playlist ID (e.g., `PL...`).
- **Ecwid**: Replace `STORE_ID` in `script.js` with your Ecwid store ID. Then add products in Ecwid.

## 2) Publish free on GitHub Pages

1. Create a free GitHub account.
2. Make a new repository named `sunshine-pet-care-k9-training`.
3. Upload all files from this folder.
4. In repository settings → **Pages** → choose `main` branch `/root` and save. Your site will be live at:
   `https://YOURUSERNAME.github.io/sunshine-pet-care-k9-training/`

## 3) Connect your `.com` domain

Domains are not free. Purchase `sunshinepetcareandk9training.com` (or similar) at a registrar (≈ $10–$15/year). In DNS, set:

- `CNAME` for `www` → `YOURUSERNAME.github.io`
- Optional root domain A records → GitHub Pages IPs (check GitHub docs)

Then in GitHub Pages → **Custom domain**, enter your domain and enforce HTTPS.

## 4) Make edits easily

- Update content in `index.html` (headings, class descriptions, contact info).
- Colors and font in `style.css`.
- Interactions (progress, rewards, embeds) in `script.js`.

## 5) Optional upgrades

- **Client logins & synced progress**: Use services like Firebase Auth + Firestore or Notion forms + Make/Zapier to sync milestones.
- **Bookings**: Calendly free works; paid tiers unlock reminders and more.
- **Store**: Ecwid paid tiers allow more products, discounts, etc.

## 6) Accessibility & performance

- Alt text provided; keep contrast strong.
- Images should be optimized (SVG used for logo).

Enjoy building!'''

with open(os.path.join(project_dir, "README.md"), "w") as f:
    f.write(readme_md)

# Zip the project for download
import zipfile
zip_path = f"{project_dir}.zip"
with zipfile.ZipFile(zip_path, 'w', zipfile.ZIP_DEFLATED) as zipf:
    for root, dirs, files in os.walk(project_dir):
        for file in files:
            fp = os.path.join(root, file)
            zipf.write(fp, os.path.relpath(fp, project_dir))

print({"project_dir": project_dir, "zip": zip_path, "files": os.listdir(project_dir)})



import os
from azure.ai.inference import ChatCompletionsClient
from azure.ai.inference.models import SystemMessage, UserMessage
from azure.core.credentials import AzureKeyCredential

endpoint = "https://models.github.ai/inference"
model = "openai/gpt-4.1"
token = os.environ["GITHUB_TOKEN"]

