# Portfolio V2 — Hari Rajashekar

Personal portfolio site for Hari Rajashekar (growth engineer). One landing page plus four case studies, all flat HTML at the repo root. No framework, no build step.

## Live

https://humanfryo.github.io/Portfolio-V2/

## Pages

| File                          | Live URL                                                        | What it is                                |
| ----------------------------- | --------------------------------------------------------------- | ----------------------------------------- |
| `index.html`                  | `/`                                                             | Landing page — hero, projects grid, about |
| `channel-fusion.html`         | `/channel-fusion.html`                                          | Case study 01 — B2B outbound (IT MSP)     |
| `barky-ai.html`               | `/barky-ai.html`                                                | Case study 02 — voice agent               |
| `linkedin-engine.html`        | `/linkedin-engine.html`                                         | Case study 03 — self-hosted Next.js tool  |
| `daily-outreach-agent.html`   | `/daily-outreach-agent.html`                                    | Case study 04 — personal job-search agent |

Every page mirrors the same nav and footer. The landing page's "Featured" section and each project card link to a case study. Each case study has a breadcrumb back to `index.html` and two related-work cards pointing at sibling case studies.

## Local dev

No build, no server needed:

```sh
# just open it
open index.html          # macOS
start index.html         # Windows
xdg-open index.html      # Linux
```

If you want a local server (e.g. for `file://` quirks):

```sh
python -m http.server 8000
# then visit http://localhost:8000
```

## Deploy

GitHub Pages serves from `main` branch root. `git push` triggers a rebuild; the new version is live in 30–90 seconds.

```sh
git add <files>
git commit -m "..."
git push
```

No CI, no actions, no secrets.

## File structure

```
.
├── index.html                  # landing page
├── channel-fusion.html         # case study pages
├── barky-ai.html
├── linkedin-engine.html
├── daily-outreach-agent.html
├── CLAUDE.md                   # conventions for AI agents working on this repo
├── README.md                   # this file
├── .gitignore                  # excludes root-level "Case Study - *.html" duplicates
└── hari-s-portfolio/
    ├── README.md               # original design-tool handoff README
    └── project/                # canonical source design prototypes
        ├── Portfolio.html
        ├── Case Study - Channel Fusion.html
        ├── Case Study - Barky AI.html
        ├── Case Study - LinkedIn Engine.html
        └── Case Study - Daily Outreach Agent.html
```

## Adding a new case study

See [`CLAUDE.md`](./CLAUDE.md) for the step-by-step recipe (which artifacts to strip from design exports, link conventions, etc.).
