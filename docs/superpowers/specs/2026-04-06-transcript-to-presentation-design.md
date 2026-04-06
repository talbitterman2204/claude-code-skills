# Transcript to HTML Presentation Skill — Design Spec

**Date:** 2026-04-06
**Status:** Approved

---

## Overview

A Claude Code skill that triggers when the user pastes a video transcript. The skill converts the transcript into a beautiful, interactive HTML presentation structured around milestones and ideas.

---

## Core Concepts

**Milestone (אבן דרך):** A key point in the progression of a story or process. Represents a chapter or stage in the content.

**Idea (רעיון):** A message, claim, or point that explains or supports a milestone. Multiple ideas live under each milestone.

Hierarchy: Presentation → Milestones → Ideas.

---

## Skill Trigger

The skill activates when the user pastes a video/lecture transcript into the conversation.

---

## Two-Phase Flow

### Phase 1 — Structure Extraction

1. Read and analyze the transcript
2. Extract milestones and their supporting ideas
3. Propose a presentation name (auto-extracted from the transcript)
4. Present a text outline for user approval:

```
Presentation name: [suggested title]

Milestone 1: [title]
  - Idea: [message/claim]
  - Idea: [message/claim]

Milestone 2: [title]
  - Idea: [message/claim]
  ...
```

5. Ask the user to confirm or modify the name and outline
6. Ask: presentation language (with suggestion based on transcript language)
7. Ask: where to save the HTML file
8. Ask: navigation style (default: arrows + side menu)
9. Ask: color theme (default: auto-detected from topic)

Only after all answers are confirmed → proceed to Phase 2.

### Phase 2 — HTML Generation

Generate a single self-contained HTML file with all slides.

---

## Slide Types

### 1. Opening Slide
- Presentation title (large)
- Number of milestones
- Sets the visual tone

### 2. Milestone Slide
- Milestone title (large, prominent)
- Sequential number + progress bar (shows position in the story)
- Central visual element representing the milestone concept (icon / color / metaphor)
- List of supporting ideas — each clickable to expand or navigate to the idea slide

### 3. Idea Slide
- The claim/message (large, clear)
- Visual demonstration — one of: chart, timeline, diagram, highlighted quote, comparison, numbered list — chosen based on what fits the content
- Back arrow to the parent milestone

---

## Navigation (Configurable Per Project)

All three navigation modes are supported. Default: arrows + side menu.

| Mode | Description |
|------|-------------|
| Arrows | Previous/next slide buttons, always visible |
| Side menu | Collapsible sidebar listing all milestones, click to jump directly |
| Landing page | Opening card grid of all milestones; click to enter |

The skill asks which combination to use during Phase 1 setup. All three can be active simultaneously.

---

## Visual Design

**Color theme:** Auto-selected based on topic (e.g., technology → blue/gray, creativity → purple/pink, business → dark green). User can override.

**Typography:** Clean sans-serif font. Large headings. High contrast.

**Transitions:** None — no animations between slides.

**Responsive:** Works on both large and small screens.

---

## Per-Project Settings

Collected during Phase 1, before HTML generation:

1. Presentation name (with auto-suggested default)
2. Language (with auto-suggested default based on transcript)
3. Save location (asked every time)
4. Navigation style (default: arrows + side menu)
5. Color theme (default: auto from topic)

---

## Skill File Location

```
skills/transcript-to-presentation/
└── SKILL.md
```

The skill description should trigger on: pasted transcript, "תמלול", "סיכום סרטון", "summary from video", "convert transcript", or similar.

---

## Export Options

After the HTML is generated, the skill offers optional export:

- **PDF** — via browser print-to-PDF (the skill generates a print-friendly CSS version of the HTML and instructs the user to use Ctrl+P → Save as PDF)
- **PowerPoint (PPTX)** — via the `anthropic-skills:pptx` skill, which converts the extracted milestones/ideas structure into a .pptx file

Both are optional and offered at the end of Phase 2.

---

## Out of Scope

- Editing slides after generation (a future iteration)
- Fetching transcripts from URLs automatically
