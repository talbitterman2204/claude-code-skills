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
   - `{{TEXT_DIRECTION}}`: `rtl` for Hebrew or Arabic, `ltr` for all others
   - `{{PRESENTATION_TITLE}}`: the confirmed title
   - `{{THEME_CSS_VARIABLES}}`: paste the chosen theme's variable lines (from the reference file)
   - `{{SIDEBAR_DISPLAY}}` and `{{MAIN_MARGIN_LEFT}}`:
     - Sidebar visible (options B, C, D): `block` and `var(--sidebar-width)`
     - Sidebar hidden (option A): `none` and `0`
   - `{{ARROWS_DISPLAY}}`: `flex` for options A, C, D — `none` for option B
   - `{{SIDEBAR_ITEMS}}`: one `<a>` per milestone, using `data-slide` set to the milestone's actual slide index:
     ```html
     <a class="sidebar-item" data-slide="N" onclick="goToSlide(N)">Milestone 1 — Title</a>
     ```
   - `{{ALL_SLIDES}}`: all slides assembled in order using the slide templates

5. Write the completed file to the path the user specified. Use the Write tool — never cat or heredoc.

6. Tell the user: "Presentation saved to `[path]`. Open it in your browser."

### Export Options (offer after saving)

Ask the user if they want either of these:

> **Optional exports:**
> - **PDF:** The file already has print-friendly CSS. Open it in Chrome or Edge, press Ctrl+P, set destination to "Save as PDF", and click Save.
> - **PowerPoint:** I can use the pptx skill to convert this into a .pptx file. Want that?

**If PPTX is requested:** Invoke the `anthropic-skills:pptx` skill and pass it the milestones/ideas structure extracted in Phase 1.
