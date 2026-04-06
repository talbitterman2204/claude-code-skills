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

---

## Phase 2 — HTML Generation

[PHASE 2 INSTRUCTIONS — added in Task 3+]
