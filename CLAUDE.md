# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **medical simulation education workshop** — a self-contained set of HTML tools that teach simulation educators how to use AI to rapidly build interactive training tools. The guiding philosophy is **"Just in Time Fidelity"**: purpose-built, scenario-specific tools created quickly with AI assistance.

## Files

- **`course-guide.html`** — The primary course file. Self-guided, sidebar-nav layout with 7 lessons + Getting Started + Glossary. This is the main file being actively developed. First draft complete 2026-03-13.
- **`pre-work.html`** — Sent to learners before the course. Covers AI account setup, terminal access, and code editor choice (~10 min read). First draft complete 2026-03-13.
- **`aed-app.html`** — A fully functional AED (Automated External Defibrillator) simulator. The live demo and reference example used in Lesson 2 of course-guide.html. Stable — not actively being edited.
- **`workshop-guide.html`** — Legacy file from an earlier direction. No longer relevant; do not reference or edit.

## Deadlines

- **2026-03-31** — Materials mostly finished; workable copy submitted for review.
- **2026-05-03** — Live workshop date.

## No Build System

There is no package manager, build step, or dependencies. All files are standalone HTML documents that run directly in any modern browser. To "run" any file, open it in a browser.

## Architecture: Single-File Pattern

Both files intentionally follow a **single-file architecture**:
- All HTML, CSS, and JavaScript live in one `.html` file
- Zero external libraries or CDN links
- Works offline after the file is opened
- No environment configuration

This is a deliberate design constraint — the workshop teaches non-coders to use AI to generate these files, so complexity must stay minimal.

## aed-app.html Internals

The AED simulator uses a simple **state machine** to track which of 5 procedural steps the user is on:
1. Power On → 2. Attach Pads → 3. Analyzing Rhythm → 4. Shock Advised → 5. Begin CPR

Key APIs used (all native, no polyfills needed):
- **Web Speech API** (`speechSynthesis`) — voice prompts for each step
- **Web Audio API** (`AudioContext`) — procedural beep/tone generation
- **CSS `@keyframes`** — shock flash overlay, CPR pulse beat animation, blinking status light

The CPR metronome runs at **110 bpm** (per AHA guidelines) with a **2-minute countdown timer**.

## What "Development" Looks Like Here

The intended workflow is AI-assisted iteration on a single HTML file:
1. Kenneth walks through the file as a learner and records feedback in Google Docs
2. He shares the Google Doc (published to web) — fetch it to read his notes
3. Enter plan mode, clarify decisions, get approval
4. Implement changes file by file
5. Open in browser to test; iterate based on experience — there are no pre-planned TODOs

## Terminology Rules (enforce throughout both files)

- ✅ "agentic AI" or "agentic AI tools" — preferred term
- ✅ "Claude, ChatGPT, and Gemini" — when listing examples
- ❌ "AI chatbots" — do not use
- ❌ "LLMs" — avoid unless in a technical context

## course-guide.html Design Patterns

**Lesson structure order** (must be preserved in all lessons):
1. `<div class="lesson-header">` — eyebrow, title, desc
2. `<div class="fac-tips-block">` — facilitator tips (hidden by default, toggle in header)
3. `<div class="content-card card-[color]">` — learning objectives
4. Content cards
5. `<div class="lesson-nav">` — prev/next buttons

**Card color conventions:**
- `card-blue` — Lessons 1, 7, Getting Started objectives
- `card-teal` — Lesson 2
- `card-amber` — Lesson 3, caution callouts
- `card-red` — Lesson 4
- `card-green` — Lesson 5
- `card-purple` — Lesson 6

**CSS custom properties (design tokens):**
- Colors: `--blue`, `--teal`, `--amber`, `--red`, `--green`, `--purple` (and `*-light` variants)
- Surface: `--bg`, `--surface`, `--border`, `--text`, `--muted`
- Dark mode via `[data-theme="dark"]` on `<html>`
- Sidebar width: `--sidebar-w: 220px`

**Facilitator tips** are hidden by default (`display:none`) and revealed via `.fac-tips-block.visible` class toggled by the header button.

## pre-work.html Design Patterns

- Step 2 (Terminal) is collapsed inside a `<details>` element — it is intentionally optional and de-emphasized. Do not restore it to a required/prominent position.
- CSS mock terminal windows and CSS editor mockups are used instead of real screenshots — preserves single-file, offline-capable pattern.
- "No laptop?" card has been removed — not needed.

When making changes to `aed-app.html`, preserve:
- The dark-theme medical device aesthetic (CSS custom properties drive the color scheme)
- The step-progress dot indicators
- The mute toggle for voice/audio
- Mobile-responsive layout (flexbox-based, no fixed widths)
