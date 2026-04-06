# HTML Presentation Template

When generating the presentation in Phase 2, produce a single self-contained HTML file using this structure. Replace all `{{PLACEHOLDER}}` values with actual content.

Available color themes (paste the chosen theme's CSS variables into `:root`):

**Technology (blue/gray):**
```css
--primary: #2563eb;
--primary-dark: #1e40af;
--accent: #60a5fa;
--bg: #f8fafc;
--surface: #ffffff;
--text: #1e293b;
--text-muted: #64748b;
--border: #e2e8f0;
```

**Creativity (purple/pink):**
```css
--primary: #7c3aed;
--primary-dark: #6d28d9;
--accent: #c4b5fd;
--bg: #faf5ff;
--surface: #ffffff;
--text: #1e1b4b;
--text-muted: #6b7280;
--border: #e9d5ff;
```

**Business (dark green):**
```css
--primary: #059669;
--primary-dark: #047857;
--accent: #6ee7b7;
--bg: #f0fdf4;
--surface: #ffffff;
--text: #064e3b;
--text-muted: #6b7280;
--border: #d1fae5;
```

**Neutral (dark gray):**
```css
--primary: #374151;
--primary-dark: #1f2937;
--accent: #9ca3af;
--bg: #f9fafb;
--surface: #ffffff;
--text: #111827;
--text-muted: #6b7280;
--border: #e5e7eb;
```

---

## Base HTML File

> **`{{TEXT_DIRECTION}}`:** Set to `rtl` for Hebrew (`he`), Arabic (`ar`), or other RTL languages. Set to `ltr` for all others.

```html
<!DOCTYPE html>
<html lang="{{LANGUAGE_CODE}}" dir="{{TEXT_DIRECTION}}">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{PRESENTATION_TITLE}}</title>
  <style>
    /* === THEME === */
    :root {
      {{THEME_CSS_VARIABLES}}
      --slide-max-width: 900px;
      --sidebar-width: 260px;
    }

    /* === RESET === */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
    }

    /* === LAYOUT === */
    .app { display: flex; min-height: 100vh; }

    /* === SIDEBAR (hidden when nav_sidebar = false) === */
    .sidebar {
      width: var(--sidebar-width);
      background: var(--surface);
      border-right: 1px solid var(--border);
      padding: 1.5rem 1rem;
      position: fixed;
      top: 0; left: 0; bottom: 0;
      overflow-y: auto;
      display: {{SIDEBAR_DISPLAY}}; /* block or none — no quotes */
    }
    .sidebar h3 {
      font-size: 0.75rem;
      text-transform: uppercase;
      letter-spacing: 0.1em;
      color: var(--text-muted);
      margin-bottom: 1rem;
    }
    .sidebar-item {
      display: block;
      padding: 0.5rem 0.75rem;
      border-radius: 6px;
      color: var(--text);
      text-decoration: none;
      font-size: 0.875rem;
      cursor: pointer;
      margin-bottom: 0.25rem;
    }
    .sidebar-item:hover { background: var(--bg); }
    .sidebar-item.active { background: var(--primary); color: white; }

    /* === MAIN CONTENT === */
    .main {
      margin-left: {{MAIN_MARGIN_LEFT}}; /* var(--sidebar-width) or 0 */
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 2rem 1rem 5rem;
    }

    /* === SLIDES === */
    .slide {
      width: 100%;
      max-width: var(--slide-max-width);
      display: none;
    }
    .slide.active { display: block; }

    /* === OPENING SLIDE === */
    .opening-slide {
      text-align: center;
      padding: 4rem 2rem;
    }
    .opening-slide h1 {
      font-size: clamp(2rem, 5vw, 3.5rem);
      font-weight: 800;
      color: var(--primary);
      line-height: 1.2;
      margin-bottom: 1rem;
    }
    .opening-slide .subtitle {
      color: var(--text-muted);
      font-size: 1.1rem;
      margin-bottom: 2.5rem;
    }
    .opening-slide .start-btn {
      background: var(--primary);
      color: white;
      border: none;
      padding: 0.875rem 2.5rem;
      border-radius: 8px;
      font-size: 1rem;
      font-weight: 600;
      cursor: pointer;
    }
    .opening-slide .start-btn:hover { background: var(--primary-dark); }

    /* === LANDING PAGE (milestone card grid) === */
    .landing-slide { padding: 2rem; }
    .landing-slide h2 {
      font-size: 1.5rem;
      font-weight: 700;
      margin-bottom: 1.5rem;
      color: var(--primary);
    }
    .milestone-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
      gap: 1rem;
    }
    .milestone-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 1.5rem;
      cursor: pointer;
      border-left: 4px solid var(--primary);
    }
    .milestone-card:hover { box-shadow: 0 4px 12px rgba(0,0,0,0.08); }
    .milestone-card .card-number {
      font-size: 0.75rem;
      font-weight: 700;
      color: var(--primary);
      text-transform: uppercase;
      letter-spacing: 0.08em;
      margin-bottom: 0.5rem;
    }
    .milestone-card h3 { font-size: 1rem; font-weight: 600; }

    /* === MILESTONE SLIDE === */
    .milestone-slide { padding: 2rem 0; }
    .milestone-header {
      background: var(--primary);
      color: white;
      border-radius: 12px;
      padding: 2.5rem 2rem;
      margin-bottom: 1.5rem;
    }
    .milestone-label {
      font-size: 0.75rem;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.1em;
      opacity: 0.8;
      margin-bottom: 0.5rem;
    }
    .milestone-title {
      font-size: clamp(1.5rem, 4vw, 2.5rem);
      font-weight: 800;
      line-height: 1.2;
    }
    .progress-bar-wrap {
      margin-top: 1.5rem;
      background: rgba(255,255,255,0.2);
      border-radius: 999px;
      height: 6px;
    }
    .progress-bar-fill {
      background: white;
      border-radius: 999px;
      height: 100%;
    }
    .milestone-icon {
      font-size: 3rem;
      margin-top: 1rem;
    }
    .ideas-list { list-style: none; display: flex; flex-direction: column; gap: 0.75rem; }
    .idea-item {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 1rem 1.25rem;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 0.75rem;
    }
    .idea-item:hover { border-color: var(--primary); }
    .idea-dot {
      width: 8px; height: 8px;
      border-radius: 50%;
      background: var(--primary);
      flex-shrink: 0;
    }
    .idea-text { font-size: 0.95rem; }

    /* === IDEA SLIDE === */
    .idea-slide { padding: 2rem 0; }
    .idea-back {
      display: inline-flex;
      align-items: center;
      gap: 0.4rem;
      color: var(--primary);
      font-size: 0.875rem;
      font-weight: 600;
      cursor: pointer;
      margin-bottom: 1.5rem;
      text-decoration: none;
    }
    .idea-claim {
      font-size: clamp(1.25rem, 3vw, 1.875rem);
      font-weight: 700;
      line-height: 1.4;
      margin-bottom: 2rem;
      color: var(--text);
    }
    .idea-visual {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 1.5rem;
    }

    /* Visual sub-types */
    .visual-quote {
      border-left: 4px solid var(--primary);
      padding-left: 1.25rem;
      font-size: 1.1rem;
      font-style: italic;
      color: var(--text-muted);
    }
    .visual-list { list-style: none; counter-reset: item; }
    .visual-list li {
      counter-increment: item;
      display: flex;
      gap: 0.75rem;
      padding: 0.5rem 0;
      border-bottom: 1px solid var(--border);
      font-size: 0.95rem;
    }
    .visual-list li:last-child { border-bottom: none; }
    .visual-list li::before {
      content: counter(item);
      background: var(--primary);
      color: white;
      width: 1.5rem; height: 1.5rem;
      border-radius: 50%;
      display: flex; align-items: center; justify-content: center;
      font-size: 0.75rem;
      font-weight: 700;
      flex-shrink: 0;
    }
    .visual-comparison { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }
    .visual-comparison .col { padding: 1rem; border-radius: 8px; background: var(--bg); }
    .visual-comparison .col h4 { font-size: 0.875rem; font-weight: 700; margin-bottom: 0.5rem; color: var(--primary); }
    .visual-timeline { display: flex; flex-direction: column; gap: 0; }
    .timeline-step { display: flex; gap: 1rem; }
    .timeline-line { display: flex; flex-direction: column; align-items: center; }
    .timeline-dot { width: 12px; height: 12px; border-radius: 50%; background: var(--primary); flex-shrink: 0; margin-top: 4px; }
    .timeline-connector { flex: 1; width: 2px; background: var(--border); }
    .timeline-step:last-child .timeline-connector { display: none; }
    .timeline-content { padding-bottom: 1rem; }
    .timeline-content h4 { font-size: 0.875rem; font-weight: 700; margin-bottom: 0.25rem; }
    .timeline-content p { font-size: 0.85rem; color: var(--text-muted); }

    /* === NAVIGATION ARROWS === */
    .nav-arrows {
      position: fixed;
      bottom: 1.5rem;
      left: 50%;
      transform: translateX(-50%);
      display: {{ARROWS_DISPLAY}}; /* 'flex' or 'none' */
      gap: 0.75rem;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 999px;
      padding: 0.5rem 1rem;
      box-shadow: 0 2px 12px rgba(0,0,0,0.1);
    }
    .nav-btn {
      background: none;
      border: none;
      cursor: pointer;
      padding: 0.4rem 0.75rem;
      border-radius: 6px;
      font-size: 1.1rem;
      color: var(--text);
    }
    .nav-btn:hover { background: var(--bg); }
    .nav-btn:disabled { opacity: 0.3; cursor: default; }
    .nav-counter { font-size: 0.8rem; color: var(--text-muted); align-self: center; min-width: 60px; text-align: center; }

    /* === PRINT / PDF === */
    @media print {
      .sidebar, .nav-arrows { display: none !important; }
      .main { margin-left: 0 !important; }
      .slide { display: block !important; page-break-after: always; }
      .slide:last-child { page-break-after: auto; }
      body { background: white; }
    }

    /* === RESPONSIVE === */
    @media (max-width: 640px) {
      .sidebar { display: none !important; }
      .main { margin-left: 0 !important; }
      .visual-comparison { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>
<div class="app">

  <!-- SIDEBAR -->
  <nav class="sidebar" id="sidebar">
    <h3>{{PRESENTATION_TITLE}}</h3>
    {{SIDEBAR_ITEMS}}
    <!-- Example item (N = the slide index of this milestone):
    <a class="sidebar-item" data-slide="N" onclick="goToSlide(N)">Milestone 1 — Milestone Title</a>
    -->
  </nav>

  <!-- MAIN -->
  <main class="main">

    {{ALL_SLIDES}}

    <!-- NAVIGATION ARROWS -->
    <div class="nav-arrows" id="nav-arrows">
      <button class="nav-btn" id="btn-prev" onclick="prevSlide()">←</button>
      <span class="nav-counter" id="nav-counter">1 / N</span>
      <button class="nav-btn" id="btn-next" onclick="nextSlide()">→</button>
    </div>

  </main>
</div>

<script>
  const slides = document.querySelectorAll('.slide');
  const sidebarItems = document.querySelectorAll('.sidebar-item');
  let current = 0;

  function showSlide(n) {
    slides.forEach((s, i) => s.classList.toggle('active', i === n));
    sidebarItems.forEach(item => item.classList.toggle('active', parseInt(item.dataset.slide) === n));
    document.getElementById('nav-counter').textContent = (n + 1) + ' / ' + slides.length;
    document.getElementById('btn-prev').disabled = n === 0;
    document.getElementById('btn-next').disabled = n === slides.length - 1;
    current = n;
    window.scrollTo(0, 0);
  }

  function nextSlide() { if (current < slides.length - 1) showSlide(current + 1); }
  function prevSlide() { if (current > 0) showSlide(current - 1); }
  function goToSlide(n) { showSlide(n); }

  document.addEventListener('keydown', e => {
    if (e.key === 'ArrowRight' || e.key === 'ArrowDown') nextSlide();
    if (e.key === 'ArrowLeft' || e.key === 'ArrowUp') prevSlide();
  });

  showSlide(0);
</script>
</body>
</html>
```

---

## Slide Templates

Use these templates when building `{{ALL_SLIDES}}`. Fill in real content — no placeholders.

### Opening Slide

```html
<div class="slide active" id="slide-0">
  <div class="opening-slide">
    <h1>{{PRESENTATION_TITLE}}</h1>
    <p class="subtitle">{{MILESTONE_COUNT}} milestones · {{TOTAL_IDEAS}} ideas</p>
    <button class="start-btn" onclick="nextSlide()">Start →</button>
  </div>
</div>
```

### Landing Page Slide (only when nav includes landing page — option D)

Place this as the second slide (after opening slide):

```html
<div class="slide" id="slide-landing">
  <div class="landing-slide">
    <h2>Overview</h2>
    <div class="milestone-grid">
      <div class="milestone-card" onclick="goToSlide(N)">
        <div class="card-number">Milestone 1</div>
        <h3>{{MILESTONE_TITLE}}</h3>
      </div>
      <!-- repeat for each milestone -->
    </div>
  </div>
</div>
```

### Milestone Slide

```html
<div class="slide" id="slide-milestone-N">
  <div class="milestone-slide">
    <div class="milestone-header">
      <div class="milestone-label">Milestone N of TOTAL</div>
      <div class="milestone-title">{{MILESTONE_TITLE}}</div>
      <div class="progress-bar-wrap">
        <div class="progress-bar-fill" style="width: {{PROGRESS_PERCENT}}%"></div>
      </div>
      <div class="milestone-icon">{{RELEVANT_EMOJI}}</div>
    </div>
    <ul class="ideas-list">
      <li class="idea-item" onclick="goToSlide(IDEA_SLIDE_INDEX)">
        <span class="idea-dot"></span>
        <span class="idea-text">{{IDEA_CLAIM}}</span>
      </li>
      <!-- repeat for each idea -->
    </ul>
  </div>
</div>
```

**Progress percent formula:** `(milestone_number / total_milestones) * 100`

**Emoji selection guidance:** Choose an emoji that metaphorically represents the milestone theme:
- Growth/progress → 🚀 📈 🌱
- Problem/challenge → ⚠️ 🔍 💡
- People/community → 👥 🤝
- Data/technology → 💻 📊 🔬
- Change/transformation → 🔄 🌊 ⚡
- Success/outcome → ✅ 🏆 🎯

### Idea Slide — Quote visual

Use when the idea is best represented by a direct quote or strong statement:

```html
<div class="slide" id="slide-idea-N">
  <div class="idea-slide">
    <a class="idea-back" onclick="goToSlide(MILESTONE_SLIDE_INDEX)">← Back to {{MILESTONE_TITLE}}</a>
    <div class="idea-claim">{{IDEA_CLAIM}}</div>
    <div class="idea-visual">
      <div class="visual-quote">{{SUPPORTING_QUOTE_OR_ELABORATION}}</div>
    </div>
  </div>
</div>
```

### Idea Slide — Numbered list visual

Use when the idea is supported by several concrete points or examples:

```html
<div class="slide" id="slide-idea-N">
  <div class="idea-slide">
    <a class="idea-back" onclick="goToSlide(MILESTONE_SLIDE_INDEX)">← Back to {{MILESTONE_TITLE}}</a>
    <div class="idea-claim">{{IDEA_CLAIM}}</div>
    <div class="idea-visual">
      <ul class="visual-list">
        <li>{{POINT_1}}</li>
        <li>{{POINT_2}}</li>
        <li>{{POINT_3}}</li>
      </ul>
    </div>
  </div>
</div>
```

### Idea Slide — Comparison visual

Use when the idea contrasts two approaches, before/after, or two perspectives:

```html
<div class="slide" id="slide-idea-N">
  <div class="idea-slide">
    <a class="idea-back" onclick="goToSlide(MILESTONE_SLIDE_INDEX)">← Back to {{MILESTONE_TITLE}}</a>
    <div class="idea-claim">{{IDEA_CLAIM}}</div>
    <div class="idea-visual">
      <div class="visual-comparison">
        <div class="col">
          <h4>{{SIDE_A_LABEL}}</h4>
          <p>{{SIDE_A_CONTENT}}</p>
        </div>
        <div class="col">
          <h4>{{SIDE_B_LABEL}}</h4>
          <p>{{SIDE_B_CONTENT}}</p>
        </div>
      </div>
    </div>
  </div>
</div>
```

### Idea Slide — Timeline visual

Use when the idea describes a sequence of events or steps over time:

```html
<div class="slide" id="slide-idea-N">
  <div class="idea-slide">
    <a class="idea-back" onclick="goToSlide(MILESTONE_SLIDE_INDEX)">← Back to {{MILESTONE_TITLE}}</a>
    <div class="idea-claim">{{IDEA_CLAIM}}</div>
    <div class="idea-visual">
      <div class="visual-timeline">
        <div class="timeline-step">
          <div class="timeline-line"><div class="timeline-dot"></div><div class="timeline-connector"></div></div>
          <div class="timeline-content"><h4>{{STEP_1_TITLE}}</h4><p>{{STEP_1_DESC}}</p></div>
        </div>
        <div class="timeline-step">
          <div class="timeline-line"><div class="timeline-dot"></div><div class="timeline-connector"></div></div>
          <div class="timeline-content"><h4>{{STEP_2_TITLE}}</h4><p>{{STEP_2_DESC}}</p></div>
        </div>
        <!-- add more steps as needed -->
      </div>
    </div>
  </div>
</div>
```

### Visual Type Selection Guide

| Content type | Visual to use |
|---|---|
| A strong claim or quote | Quote |
| Multiple concrete examples or points | Numbered list |
| Before/after, pros/cons, two perspectives | Comparison |
| A process, sequence, or story over time | Timeline |
| A single elaborated statement | Quote |
| Steps in a method or framework | Numbered list or Timeline |
