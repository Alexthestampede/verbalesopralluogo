# Agent Instructions: verbale-sopralluogo

## What this repo is

Single-file static webapp (`index.html`) for digital construction-site inspection reports ("verbali di sopralluogo"). No build system, no dependencies, no CI. It is meant to be served from GitHub Pages.

## Project structure

- `index.html` — the entire app: HTML, CSS, and JS in one file. This is the only source file that matters.
- `README.md` — user-facing setup and feature docs in Italian.
- `ISTRUZIONI_SETUP.md` — step-by-step GitHub Pages deployment guide.

## How to run / verify locally

- Open `index.html` in a browser. A local file (`file://`) will load, but **iPhone Safari blocks some interactions on local files**, so testing mobile behavior requires an HTTP server.
- Quick HTTP server for verification:
  ```bash
  python3 -m http.server 8000
  # then open http://localhost:8000
  ```
- No tests, no lint, no typecheck, no package manager.

## Deployment

- Deploys via GitHub Pages from the root of the default branch (`main` or `master`).
- Just push `index.html`; Pages redeploys automatically in ~1–2 minutes.

## Editing guidance

- Everything lives in `index.html`. Key areas to search for when modifying:
  - `<style>` — all CSS.
  - `const FOTO_CONFIG` — photo compression / limits (lines ~385–392).
  - `const STORAGE = 'verbale_sopralluogo_v2'` — localStorage key.
  - `defaultState` — initial form state and default checklist items.
  - `exportPDF()` — PDF generation using jsPDF + jspdf-autotable loaded from CDN.
- External dependencies are loaded from CDN in the `<head>`/`body`:
  - `jspdf.umd.min.js`
  - `jspdf.plugin.autotable.min.js`
  Changing or removing these script tags will break PDF export.

## Runtime behavior agents should know

- All state is kept in `localStorage` under key `verbale_sopralluogo_v2`.
- Photos are compressed to max 1200×1200px, JPEG quality ~0.8, and stored as base64 data URLs.
- JSON export strips images to keep the file small for AI import; PDF export includes images.
- Adding dynamic sections or changing `id` attributes often requires wiring into `refreshAll()` or exposing handlers via `window.handle*` functions because several buttons use inline `onclick` attributes.

## Conventions

- UI language is Italian; preserve Italian labels and date formats (`DD/MM/YYYY`, `it-IT`) when editing output/export.
- The app is mobile-first; keep CSS `@media (max-width:768px)` responsive rules intact.
- Signature canvases rely on pixel-ratio scaling via `devicePixelRatio`; avoid changing canvas sizing logic unless necessary.
