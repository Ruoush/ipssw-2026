# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **medical simulation education workshop** — a self-contained set of HTML tools that teach simulation educators how to use AI to rapidly build interactive training tools. The guiding philosophy is **"Just in Time Fidelity"**: purpose-built, scenario-specific tools created quickly with AI assistance.

## Files

- **`index.html`** — The primary course file (renamed from course-guide.html). Self-guided, sidebar-nav layout with 8 lessons + Getting Started + Glossary. ~2400 lines. Actively developed.
- **`pre-work.html`** — Sent to learners before the course. Covers AI account setup, code editor choice. V2 complete.
- **`aed-app.html`** — A fully functional AED simulator. The live demo and reference example used in Lesson 2. Stable — not actively being edited.
- **`workshop-guide.html`** — Legacy file. No longer relevant; do not reference or edit.

## Deployment

- **GitHub:** github.com/Ruoush/ipssw-2026
- **Live URL:** https://ipssw-2026.vercel.app
- **pre-work:** https://ipssw-2026.vercel.app/pre-work.html
- **AED app:** https://ipssw-2026.vercel.app/aed-app.html
- Auto-deploys on push to `main`. To deploy: `git add`, `git commit`, `git push`.

## Deadlines

- **2026-03-31** — Materials mostly finished; workable copy submitted for review.
- **2026-05-03** — Live workshop date.

## No Build System

No package manager, build step, or dependencies. All files are standalone HTML. To "run": open in a browser.

## Architecture

**Course files** (index.html, pre-work.html) have no single-file constraint — use whatever structure is best.

**Learner-built tools** (what learners build during the workshop) must be single-file HTML — learners copy-paste AI output with no build tooling.

## index.html Layout — Three-Column

```
[sidebar 220px fixed] [main content flex:1] [scratchpad 280px fixed]
```

- `.sidebar` — `position: fixed; left: 0; width: 220px`
- `.main` — `margin-left: 220px; margin-right: 280px; transition: margin-right 0.25s`
- `.main.scratchpad-collapsed` — `margin-right: 32px`
- `.lesson-area` — `max-width: 860px`
- `.scratchpad` — `position: fixed; right: 0; top: 52px; bottom: 0; width: 280px`
- `.scratchpad.collapsed` — `width: 32px`
- Mobile (≤768px): sidebar hidden, scratchpad hidden, main fills full width

## Lesson Structure (index.html)

**Current lesson order:**
1. Getting Started (~10 min) — AI account, code editor, terminal
2. Glossary (reference) — 32 terms, alphabetical
3. Lesson 1 — What Is JITF? (~10 min)
4. Lesson 2 — See It in Action (~10 min)
5. Lesson 3 — How to Talk to AI (~15 min)
6. Lesson 4 — Build Your Own Tool (~30 min)
7. Lesson 5 — Share & Reflect (~15 min)
8. Lesson 6 — Resources & Next Steps (~10 min)
9. Lesson 7 — Publish Your Tool (~20 min) — GitHub + Vercel, purple color scheme
10. Lesson 8 — Troubleshooting (reference) — ends with completion card

**Lesson HTML structure order** (must be preserved):
1. `<div class="lesson-header">` — eyebrow, title, desc, time-badge
2. `<div class="fac-tips-block">` — facilitator tips (hidden by default)
3. `<div class="content-card card-[color]">` — learning objectives
4. Content cards
5. `<div class="lesson-nav">` — prev/next buttons

**JS lesson routing:**
- `LESSONS` array controls order
- `LESSON_LABELS` object maps IDs to display names
- `showLesson(id)` — shows `sec-[id]` section, hides all others
- Section IDs: `sec-getting-started`, `sec-glossary`, `sec-lesson-1` … `sec-lesson-8`

## Card Color Conventions

- `card-blue` — Learning objectives (most lessons), Getting Started
- `card-teal` — Lesson 2
- `card-amber` — Lesson 3 objectives, caution callouts
- `card-red` — Lesson 4 objectives
- `card-green` — Lesson 5 objectives
- `card-purple` — **All "Your Turn" learner activity cards** across ALL lessons; Lesson 6 objectives; Lesson 7 content (signals advanced/bonus)
- Inline red callouts — caution/warning (IT policy, clinical accuracy, security)
- Inline amber callouts — general warnings (file naming, etc.)
- Inline green callouts — positive/confirmation (not used for security — see design decisions)

## CSS Design Tokens

- Colors: `--blue`, `--teal`, `--amber`, `--red`, `--green`, `--purple` (and `*-light` variants)
- Surface: `--bg`, `--surface`, `--border`, `--text`, `--muted`
- Code blocks: `--code-bg`, `--code-text`
- Dark mode via `[data-theme="dark"]` on `<html>`
- Sidebar width: `--sidebar-w: 220px`

## Key Components (index.html)

- `.time-badge` — pill showing lesson duration in lesson-header
- `.build-loop` / `.loop-step` / `.loop-arrow` — visual Build Loop diagram (L2)
- `.course-map table` — lesson overview table (Getting Started)
- `.template-editor` / `.template-editor-wrap` / `.btn-copy` — editable prompt textarea with copy button (L3, L4)
- `.browser-mock` — CSS browser chrome mockup (L7)
- `.scratchpad` — persistent notes panel with localStorage, Load Planning Template button
- `.activity-steps` — numbered step list for build activities
- `.prompt-card` — prompt template cards (L3)
- `.glossary-item` / `.glossary-term` / `.glossary-def` — glossary entries
- `.fac-tips-block` — facilitator tips (toggled by header button, `data-tips` attribute)
- `copyPrompt(id)` — copies textarea or pre content to clipboard
- `toggleScratchpad()` — collapses/expands scratchpad, toggles `.scratchpad-collapsed` on `.main`
- `loadPlanningTemplate()` — fills scratchpad textarea with planning prompts

## aed-app.html Internals

State machine: Power On → Attach Pads → Analyzing Rhythm → Shock Advised → Begin CPR

Key APIs:
- **Web Speech API** (`speechSynthesis`) — voice prompts
- **Web Audio API** (`AudioContext`) — beep/tone generation
- **CSS `@keyframes`** — shock flash, CPR pulse, blinking status light

CPR metronome: **110 bpm**, 2-minute countdown timer.

Preserve: dark-theme medical device aesthetic, step-progress dot indicators, mute toggle, mobile-responsive layout.

## Development Workflow

1. Kenneth walks through the course as a learner and records feedback in Google Docs
2. He shares the Google Doc (published to web URL) — fetch it with WebFetch to read notes
3. Enter plan mode, clarify decisions, get approval
4. Implement changes to index.html (use Grep to find exact locations — file is ~2400 lines)
5. `git add`, `git commit`, `git push` → Vercel auto-deploys
6. Verify at ipssw-2026.vercel.app

**Always use Grep to find insertion points** before editing index.html. Do not read the whole file — it exceeds the context limit.

## Terminology Rules

- ✅ "agentic AI" or "agentic AI tools" — preferred term
- ✅ "Claude, ChatGPT, and Gemini" — when listing examples
- ❌ "AI chatbots" — do not use
- ❌ "LLMs" — avoid unless in a technical context

## pre-work.html Notes

- Step 2 (Code Editor): marked optional. Cursor recommended, VS Code second, TextEdit/Notepad fallback.
- Step 3 (Terminal): collapsed in `<details class="terminal-optional">`. Intentionally optional and de-emphasized.
- SVG logos for Claude/ChatGPT/Gemini already in place.
- Claude Code optional callout already added after VS Code option row.
- "No laptop?" card removed — not needed.
