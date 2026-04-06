# Transcript to HTML Presentation Skill — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a Claude Code skill that converts a pasted video transcript into a self-contained interactive HTML presentation organized by milestones and ideas.

**Architecture:** Two files — `SKILL.md` contains the complete skill instructions (Phase 1 extraction flow + Phase 2 generation instructions), and `references/html-template.md` contains the full HTML/CSS/JS template that Claude fills in with content. The skill is pure markdown — no code execution required.

**Tech Stack:** HTML5, CSS3 (custom properties for themes), vanilla JavaScript, `anthropic-skills:pptx` for optional PowerPoint export.

**Spec:** `docs/superpowers/specs/2026-04-06-transcript-to-presentation-design.md`

---

## File Structure

```
skills/transcript-to-presentation/
├── SKILL.md                          # Main skill — trigger, Phase 1, Phase 2
└── references/
    └── html-template.md              # Complete HTML/CSS/JS template with placeholders
```

---

## Task 1: Create the SKILL.md skeleton with frontmatter and trigger

**Files:**
- Create: `skills/transcript-to-presentation/SKILL.md`

- [ ] **Step 1: Create the file**

```markdown
---
name: transcript-to-presentation
description: Use this skill when the user pastes a video or lecture transcript and wants to turn it into a presentation. Triggers on: pasted transcript text (especially long text that reads like spoken content), "תמלול", "סיכום סרטון", "convert transcript", "make a presentation from this", "turn this into slides", or any request to summarize a transcript visually.
---

# Transcript to Presentation Skill

This skill converts a pasted video or lecture transcript into a beautiful, self-contained HTML presentation structured around milestones and ideas.

## Core Concepts

- **Milestone (אבן דרך):** A key point in the progression of the story or process — a chapter or stage.
- **Idea (רעיון):** A message, claim, or point that explains or supports a milestone.
- Hierarchy: Presentation → Milestones → Ideas.

---

## Phase 1 — Structure Extraction

[PHASE 1 INSTRUCTIONS — added in Task 2]

---

## Phase 2 — HTML Generation

[PHASE 2 INSTRUCTIONS — added in Task 3+]
```

- [ ] **Step 2: Verify the file exists**

```bash
ls skills/transcript-to-presentation/SKILL.md
```

- [ ] **Step 3: Commit**

```bash
git add skills/transcript-to-presentation/SKILL.md
git commit -m "feat: scaffold transcript-to-presentation skill"
```

---

## Task 2: Write Phase 1 instructions in SKILL.md

**Files:**
- Modify: `skills/transcript-to-presentation/SKILL.md` (replace Phase 1 placeholder)

- [ ] **Step 1: Replace the Phase 1 placeholder with complete instructions**

Replace `[PHASE 1 INSTRUCTIONS — added in Task 2]` with:

```markdown
When the user pastes a transcript, do the following steps in order. Do not skip ahead to HTML generation until all steps are complete and the user has approved.

### Step 1 — Extract Structure

Read the transcript carefully. Identify:
- **Milestones:** 3–8 key stages, turning points, or chapters in the content. Each milestone should represent a distinct phase of the story or argument.
- **Ideas:** For each milestone, extract 1–4 supporting messages, claims, or examples.

### Step 2 — Present Outline

Output the extracted structure in this exact format, then ask the user to confirm or correct it:

---
**Proposed presentation name:** [Your suggested title based on the content]

**Milestone 1:** [Title]
  - Idea: [Message or claim]
  - Idea: [Message or claim]

**Milestone 2:** [Title]
  - Idea: [Message or claim]
  ...

---
*Does this structure look right? Feel free to rename milestones, add/remove ideas, or adjust before we continue.*

### Step 3 — Ask Project Settings

After the user approves the outline, ask the following questions **all at once in a single message** (not one by one):

> Before I generate the presentation, I need a few details:
>
> 1. **Name:** Is "[suggested title]" good, or do you want to change it?
> 2. **Language:** The transcript appears to be in [detected language]. Should the presentation be in that language, or a different one?
> 3. **Save location:** Where should I save the HTML file? (e.g., `~/Desktop/my-presentation.html`)
> 4. **Navigation style:** Which navigation modes do you want? (default: arrows + side menu)
>    - A) Arrows only
>    - B) Side menu only
>    - C) Arrows + side menu (default)
>    - D) Landing page + arrows + side menu (all three)
> 5. **Color theme:** Should I auto-select based on the topic, or do you prefer one of these?
>    - Technology (blue/gray)
>    - Creativity (purple/pink)
>    - Business (dark green)
>    - Neutral (dark gray)
>    - Auto (let me choose based on topic)

Wait for the user to answer all five questions. Then proceed to Phase 2.
```

- [ ] **Step 2: Commit**

```bash
git add skills/transcript-to-presentation/SKILL.md
git commit -m "feat: add Phase 1 extraction and settings flow to skill"
```

---

## Task 3: Create the HTML template reference file — base structure and CSS

**Files:**
- Create: `skills/transcript-to-presentation/references/html-template.md`

- [ ] **Step 1: Create the file with base HTML structure and CSS**

```markdown
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

```html
<!DOCTYPE html>
<html lang="{{LANGUAGE_CODE}}">
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
      display: {{SIDEBAR_DISPLAY}}; /* 'block' or 'none' */
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
    <!-- Example item:
    <a class="sidebar-item" onclick="goToSlide(N)">Milestone Title</a>
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
    sidebarItems.forEach((item, i) => item.classList.toggle('active', i === n));
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
```

- [ ] **Step 2: Commit**

```bash
git add skills/transcript-to-presentation/references/html-template.md
git commit -m "feat: add base HTML/CSS/JS template for presentation"
```

---

## Task 4: Add slide type templates to the reference file

**Files:**
- Modify: `skills/transcript-to-presentation/references/html-template.md` (append slide templates section)

- [ ] **Step 1: Append the slide templates section**

Add this section at the end of the file:

```markdown
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

### Landing Page Slide (only when nav includes landing page)

Place this as slide-1 (after opening slide):

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
```

- [ ] **Step 2: Commit**

```bash
git add skills/transcript-to-presentation/references/html-template.md
git commit -m "feat: add slide type templates to HTML reference"
```

---

## Task 5: Write Phase 2 instructions in SKILL.md

**Files:**
- Modify: `skills/transcript-to-presentation/SKILL.md` (replace Phase 2 placeholder)

- [ ] **Step 1: Replace the Phase 2 placeholder with complete instructions**

Replace `[PHASE 2 INSTRUCTIONS — added in Task 3+]` with:

```markdown
Once the user has confirmed the outline and answered all project settings questions, generate the HTML presentation.

### How to Generate the HTML

1. Read `skills/transcript-to-presentation/references/html-template.md` — it contains the complete base HTML file and all slide type templates.

2. Build the slide sequence in this order:
   - Slide 0: Opening slide (always first)
   - Slide 1: Landing page (only if user chose navigation option D)
   - Then for each milestone:
     - Milestone slide
     - Idea slides for each idea under that milestone (in order)

3. For each **idea slide**, pick the visual type that best matches the content using the Visual Type Selection Guide in the reference file. Use real content extracted from the transcript — no generic filler.

4. Set the HTML variables:
   - `{{LANGUAGE_CODE}}`: e.g., `he` for Hebrew, `en` for English
   - `{{PRESENTATION_TITLE}}`: the confirmed title
   - `{{THEME_CSS_VARIABLES}}`: paste the chosen theme's CSS variables block
   - `{{SIDEBAR_DISPLAY}}` / `{{MAIN_MARGIN_LEFT}}` / `{{ARROWS_DISPLAY}}`: set based on navigation choices:
     - Sidebar visible: `block` / `var(--sidebar-width)` — else `none` / `0`
     - Arrows visible: `flex` — else `none`
   - `{{SIDEBAR_ITEMS}}`: one `<a class="sidebar-item" onclick="goToSlide(N)">` per milestone
   - `{{ALL_SLIDES}}`: all slides assembled in order

5. Write the completed file to the path the user specified. Use the Write tool — never cat/heredoc.

6. Tell the user: "Presentation saved to `[path]`. Open it in your browser."

### Export Options (offer after saving)

Ask the user if they want either of these:

> **Optional exports:**
> - **PDF:** I can add print-friendly styles so you can do Ctrl+P → Save as PDF directly from your browser. Want me to add that?
> - **PowerPoint:** I can use the pptx skill to convert this into a .pptx file. Want that too?

**If PDF is requested:** The `@media print` CSS is already included in the template — tell the user to open the HTML file in Chrome or Edge, press Ctrl+P, set destination to "Save as PDF", and click Save.

**If PPTX is requested:** Invoke the `anthropic-skills:pptx` skill and pass it the milestones/ideas structure you already extracted in Phase 1.
```

- [ ] **Step 2: Commit**

```bash
git add skills/transcript-to-presentation/SKILL.md
git commit -m "feat: add Phase 2 HTML generation instructions to skill"
```

---

## Task 6: Verify the skill end-to-end with a sample transcript

**Files:** none created — this is a manual verification step.

- [ ] **Step 1: Paste this sample transcript into a new Claude conversation**

```
In the 1950s, scientists discovered that DNA carries genetic information. Watson and Crick built on Rosalind Franklin's X-ray data to propose the double helix model in 1953. This was a turning point — we finally knew the shape of the molecule of life.

Once the structure was known, the next challenge was understanding how DNA is copied. The process, called replication, was shown to be semi-conservative: each new DNA molecule keeps one original strand and builds a new one alongside it. This elegant mechanism ensures faithful copying across billions of cell divisions.

The third milestone was cracking the genetic code — how sequences of DNA letters translate into proteins. In the 1960s, researchers decoded all 64 codons. This opened the door to understanding how genes make the molecules that run every cell.

Finally, the age of genetic engineering arrived. Scientists learned to cut, paste, and insert DNA using restriction enzymes and later CRISPR. Today we can rewrite the genome with precision that was unimaginable 70 years ago.
```

- [ ] **Step 2: Confirm Phase 1 behavior**

Verify that the skill:
- Extracts 4 milestones (DNA structure, Replication, Genetic code, Genetic engineering)
- Extracts 1-3 ideas per milestone
- Proposes a sensible title (e.g., "The Story of DNA: From Discovery to Engineering")
- Asks all 5 settings questions in one message

- [ ] **Step 3: Answer the settings questions and confirm Phase 2**

Use these answers:
- Name: accept the suggestion
- Language: English
- Save: `test-presentation.html` in the current directory
- Navigation: C (arrows + side menu)
- Theme: Technology

- [ ] **Step 4: Verify the generated HTML**

Open `test-presentation.html` in a browser. Check:
- Opening slide shows the title and milestone count
- Side menu shows all 4 milestones
- Arrow navigation works (← →)
- Keyboard arrows work
- Each milestone slide has a progress bar and emoji
- Each idea slide has a real visual (not a placeholder)
- Back arrow on idea slides returns to the parent milestone

- [ ] **Step 5: Clean up**

```bash
rm test-presentation.html
git add skills/transcript-to-presentation/
git commit -m "feat: complete transcript-to-presentation skill"
```
```

- [ ] **Step 6: Final verification of file structure**

```bash
ls skills/transcript-to-presentation/
# Expected:
# SKILL.md
# references/
#   html-template.md
```
