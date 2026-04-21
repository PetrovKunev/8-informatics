# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static HTML site with interactive C# programming lessons for Bulgarian 8th-grade informatics (8 клас Информатика). All user-facing text, commit messages, and lesson content are **in Bulgarian** — match that language when editing content or writing commits.

There is no build system, no package manager, and no server. Open the HTML files directly in a browser. `index.html` at the repo root is the landing page (arcade-style map of "levels"); every lesson lives under `уроци/` (Cyrillic folder name).

## File layout

- `index.html` — root landing page. A "Pixel Cards" grid of lesson cards with pixel-art illustrations, search, category filters, and XP progress. Links to lessons use relative paths like `уроци/arrays.html`.
- `уроци/*.html` — one file per lesson, each self-contained (embedded `<style>` and `<script>`). Back to landing is `<a href="../index.html" class="home-btn">⌂</a>`.

When adding a new lesson, put it in `уроци/`, bump the lesson count in the progress strip (`/6`), add a new `<a class="pxcard">` in `.cards-grid` with `data-cat` + `data-keywords` (used by the search/filter), a `LV NN` badge, and extend the XP table in `updateProgressUI()` (`xpTable = {1:100, ...}`) so the total XP reflects the new lesson. Also add the new `data-cat` to the filter button list if it's a new category.

## Design system — "paper + arcade"

The redesigned site uses a warm paper + retro arcade aesthetic. Do **not** mix in the old dark-neon palette when making edits.

- **Surfaces**: cream paper (`--paper: #f4f1e8`), near-black ink (`--ink: #1a1814`), subtle radial-dot grid background.
- **Accents**: `--red` (primary/play), `--teal` (info), `--mustard` (xp/stars), `--plum` (special), `--leaf` (success).
- **Borders & shadows**: 2–2.5px solid `--ink` borders and hard offset drop shadows (`--shadow: 6px 6px 0 0 var(--ink)` / `--shadow-sm: 3px 3px 0 0 var(--ink)`) — no blur, no gradients on shadows. Hover typically nudges `translate(-3px,-3px)` and grows the shadow.
- **Type**: four font families, all loaded from Google Fonts in a single `<link>`:
  - `--sans` = `Space Grotesk` (body)
  - `--mono` = `JetBrains Mono` (code, meta, badges)
  - `--pixel` = `Press Start 2P` (hero headings, CTAs, "level" labels)
  - `--crt` = `VT323` (terminal/CRT-style flourishes)
- All four CSS variable blocks are duplicated into each lesson file — keep them consistent across files when tweaking the palette.

## Lesson page conventions

When adding a new lesson, copy an existing one (e.g. `уроци/for-loop.html`) rather than starting from scratch so the visual language stays consistent.

- **Hero + HUD** at the top (level badge, title in `--pixel`, subtitle in `--sans`).
- **Content sections** with `.ex-card` blocks, visual diagrams, and code samples.
- **Exercise cards** (`.ex-card`, plus `.med` / `.hard` / `.bonus` difficulty modifiers) with two buttons per exercise: `ПОДСКАЗКА` (`toggleHint`) and `РЕШЕНИЕ` (`toggleSolution`). Solutions contain full, runnable C# snippets — not fragments. Button labels are **ALL-CAPS** in the new design (not the old `💡 Подсказка` / `✅ Пълно решение`).
- **Quiz** driven by a `quizData` array at the bottom.
- **Nav footer** with `.nav-link prev` / `.nav-link next` pointing to sibling lessons, plus a `.home-btn` to `../index.html`. Keep the prev/next chain in sync when reordering lessons.

The `toggleHint` / `toggleSolution` helpers are duplicated into each page rather than shared — keep them consistent across files when changing the button label logic.

## Commits

Commit messages are short and written in Bulgarian, describing what was added/changed from the student's perspective (e.g. `Добавени 3 нови урока: if/else, for цикъл, while цикъл`). Follow that style.
