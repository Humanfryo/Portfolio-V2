# CLAUDE.md — Working on this repo

Conventions and gotchas for AI agents (Claude Code or similar) picking up this project. Read this before making changes.

## Project model

- **What:** Personal portfolio site for Hari Rajashekar (growth engineer). One landing page (`index.html`) + N case-study pages, all flat HTML at the repo root.
- **Stack:** Static HTML + inline `<style>` + inline `<script>`. No framework. No build step. No bundler. No package.json.
- **Source-of-truth design files:** `hari-s-portfolio/project/*.html` — these are the design-tool exports from claude.ai/design. Production pages at the repo root are derived from them by stripping design-tool / Cloudflare artifacts (see below).
- **Deploy:** GitHub Pages, `main` branch, root path. `git push` → live in 30–90 s. Live URL: https://humanfryo.github.io/Portfolio-V2/

## Design tokens

Every page declares the same palette and type stack in `:root` (kept inline so each page is self-contained). Don't centralize unless the user asks.

```css
:root {
  --bg: #1C2317;          /* dark olive base */
  --bg-raised: #222A1C;
  --bg-card: #242C1E;
  --bg-inset: #161C12;    /* (case-study pages only — code blocks) */
  --line: #34402C;
  --line-soft: #283220;

  --text: #EDE6DA;        /* cream */
  --text-mid: #B4B29E;
  --text-dim: #7D7C68;
  --text-faint: #525442;

  --accent: #D4A72C;      /* gold */
  --accent-dim: #9A7A1C;

  --serif: "Instrument Serif", "Times New Roman", serif;
  --sans:  "DM Sans", system-ui, sans-serif;
  --mono:  "JetBrains Mono", ui-monospace, monospace;

  --page-max: 1280px;
  --page-pad: clamp(24px, 4vw, 64px);
  --read-col: 680px;      /* (case-study pages only) */
}
```

Fonts come from Google Fonts via the same `<link>` block on every page (Instrument Serif + DM Sans + JetBrains Mono). Don't add new fonts without reason.

## Adding a new case study

The user typically drops a design-tool export named `Case Study - Foo.html` into the repo root. That file is **gitignored** (see `.gitignore` pattern `Case Study - *.html`). Workflow:

1. **Read the source.** Open the dropped file end-to-end.
2. **Create the production file** at the repo root with a kebab-case name: `foo.html` (e.g. `barky-ai.html`, `linkedin-engine.html`). Copy the source verbatim, then apply the cleanup checklist below.
3. **Strip design-tool / Cloudflare artifacts** (see next section).
4. **Rewrite link references:** every `Portfolio.html` in the source → `index.html`. Every long related-work `href="Case Study - Foo.html"` → its kebab-case production name (`foo.html`).
5. **Wire the related-work cards** at the bottom of the new page to point at the two most-related sibling case studies (already-built ones).
6. **Wire the matching project card** in `index.html`'s projects grid (`#work` section) to add a `<a class="card-link" href="foo.html">Read case study</a>` link.
7. **Optionally update sibling case studies' related-work cards** to surface the new page (only if it's a closer match than what's currently linked).
8. **Copy the source into the bundle:** `cp "Case Study - Foo.html" hari-s-portfolio/project/` so the canonical design prototype is preserved alongside the others.
9. **Commit + push.** Pages rebuilds.

## Stripped-artifact checklist

Design-tool exports include scaffolding that doesn't belong in production. Remove every instance of:

| Artifact                                                                                                            | Action                                                       |
| ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| `data-screen-label="..."` on `<section>` / `<footer>`                                                               | Delete the attribute                                         |
| `<script data-cfasync="false" src="/cdn-cgi/scripts/.../email-decode.min.js"></script>`                             | Delete the entire `<script>` tag                             |
| `href="/cdn-cgi/l/email-protection#…"` on email anchors                                                             | Restore to `mailto:hari@example.com`                         |
| `<a class="__cf_email__" data-cfemail="...">[email&nbsp;protected]</a>` (inline obfuscated address inside copy)     | Restore to a readable plausible literal (e.g. `firstname.lastname@company.com` if it's an example)            |
| `<div id="tweaks-panel">…</div>` and its associated `<script>` (uses `EDITMODE-BEGIN` / `EDITMODE-END` markers and `window.parent.postMessage('__edit_mode_…')`) | Delete both — only present in the `Portfolio.html` source        |

Keep all the **legitimate** runtime JS: sticky-nav border on scroll, scroll-reveal observer, cursor-follow glow on `[data-glow]` cards.

## Link conventions

Every page mirrors the same nav and footer:

```html
<header class="nav">
  <a href="index.html" class="nav-mark">…Hari Rajashekar</a>
  <nav class="nav-links">
    <a href="index.html#work">Work</a>
    <a href="index.html#about">About</a>
    <a href="index.html#contact">Contact</a>
    <a href="index.html#contact" class="nav-cta">Book a call</a>
  </nav>
</header>
```

- Case-study pages have a breadcrumb (`<div class="crumb"><a href="index.html">← All work</a></div>`) right after the nav.
- Case-study pages' "Related work" section has two `<a class="r-card" href="…">` cards pointing at sibling case studies.
- Footer: `https://cal.com/hari`, `mailto:hari@example.com`, LinkedIn (`href="#"` placeholder), GitHub + Resume PDF in the small links row.

## Things that look wrong but aren't

- **Inline styles everywhere.** Yes, every page duplicates ~500 lines of CSS. Don't extract to a shared stylesheet unless asked — keeping pages self-contained matches the prototype convention and means each file works `file://` with no dependencies.
- **`href="#"` on LinkedIn / GitHub / Resume PDF.** These are intentional placeholders the user hasn't filled in yet. Don't fabricate URLs.
- **Two reveal-script flavors.** `index.html` + `channel-fusion.html` use IntersectionObserver; the three newer case studies use a manual `getBoundingClientRect()` poll with a 1.5s failsafe. Both work; don't normalize unless asked.

## Out of scope unless asked

- Adding a build system (Vite, etc.)
- Extracting shared CSS / JS
- Renaming production files (the kebab-case URLs are already linked from many places)
- Touching `hari-s-portfolio/` source files (those are immutable design exports)
