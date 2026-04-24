# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A zero-dependency static website. Three files make up the whole site:

- `index.html` — semantic markup and all copy. Sections: header, hero, features, about, contact, footer.
- `styles.css` — design tokens in `:root` custom properties, a `[data-theme="dark"]` override, and a `prefers-color-scheme: dark` fallback. Layout uses CSS Grid (`.feature-grid`) and `clamp()` for fluid typography/spacing.
- `script.js` — a single IIFE that (1) stamps the current year into `#year` and (2) toggles `data-theme` on `<html>`, persisting the choice to `localStorage` under the key `fresh:theme`.

No build step, no package manager, no framework.

## Running locally

Open `index.html` directly in a browser, or serve the directory:

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```

A local server is only needed if you add features that require an HTTP origin (fetch, service workers, modules loaded with import).

## Conventions

- **Theming is token-driven.** Change colors/spacing by editing the CSS custom properties in `:root` and the `[data-theme="dark"]` block — do not hardcode colors in component rules. The dark palette must be updated in **both** `[data-theme="dark"]` and the `prefers-color-scheme: dark` media query so manual and automatic dark mode stay in sync.
- **Theme state lives on `<html>`.** The toggle in `script.js` reads/writes `data-theme` on `document.documentElement`; CSS selectors depend on this. If the attribute is absent, the OS preference wins.
- **Keep it framework-free.** Prefer adding a small vanilla helper over introducing a bundler, package manager, or dependency. If a library is genuinely needed, reach for a `<script type="module">` CDN import before adding tooling.
- **Accessibility is load-bearing.** Preserve semantic landmarks (`header`/`main`/`footer`/`nav`), the visible `:focus-visible` ring, and the `prefers-reduced-motion` block at the bottom of `styles.css`.
- **No tests, no linter** are configured. Verify changes by loading the page in a browser and exercising the theme toggle and responsive breakpoints.
