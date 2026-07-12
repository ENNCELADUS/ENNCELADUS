# Homepage Redesign — Design Spec

**Date:** 2026-07-12
**Owner:** Anrui Wang (GitHub: `ENNCELADUS`)
**Status:** Draft for review

## Goal

Replace the current generic dark/purple developer-template homepage with a
polished, credible, *official* academic personal page that accurately reflects
Anrui Wang's real work in **AI for Science** (single-cell & graph representation
learning). Single static `index.html` on GitHub Pages, no frameworks.

## Why (problems with the current page)

- **Broken assets:** hero background and all imagery use `source.unsplash.com`
  (shut down — nothing loads); avatar URL `avatars.githubusercontent.com/u/ENNCELADUS`
  is malformed.
- **Wrong / placeholder content:** skills list JS/React/Node/AWS/Docker (contradicts
  actual focus); `contact@example.com`; dead `#` social links; `© 2023`.
- **Wrong positioning:** reads as a generic bootcamp dev portfolio, not an
  AI-for-Science researcher.

## Audience & positioning

Primary audience: **academic / research** — grad-school admissions, professors,
research collaborators. Design goal: clean, credible, "official"; text-forward;
projects/research as the centerpiece.

## Aesthetic direction

**Light & minimal**, in the Jon Barron / al-folio / Clarity idiom. One centered
column (~760px max-width), near-black-on-off-white, one restrained accent,
`prefers-color-scheme` support + a dark-mode toggle. Motion deliberately minimal
(heavy animation lowers academic credibility).

Convergent recommendation from two research passes (academic-page conventions +
modern portfolio technique), adapted to a light palette.

## Locked decisions

| Decision | Choice |
|---|---|
| Headline name | **Anrui Wang** |
| GitHub contribution "snake" | **Dropped** (quieter/more official) |
| Header photo | **GitHub avatar** via `https://github.com/ENNCELADUS.png` |
| Email | **Placeholder** (`[you@example.com]`), clearly marked |
| Affiliation | **Placeholder** (`[Your University]`), clearly marked |
| C++/Java skills | **Dropped** — not in CV; use real CV skills |
| Extra links (Scholar/LinkedIn) | **Excluded** — GitHub is the only live link |
| `README.md` (GitHub profile) sync | **In scope** — bring it in line with the new page |
| Architecture | Single static `index.html`, GitHub Pages, self-contained |

## Page structure

Sticky, glassy nav (`backdrop-filter`) with brand monogram + links
(About · Research · Skills · CV · Contact) + dark-mode toggle. Sections:

1. **Header** — photo (GitHub avatar), name "Anrui Wang", identity line
   "CS undergraduate · AI for Science", affiliation placeholder, compact link row.
   **Links (per review): Email (placeholder) · CV PDF (placeholder) · GitHub
   (live) only.** Google Scholar and LinkedIn are **excluded**. **Zero dead links
   on the live page:** GitHub is the only live link; Email and CV are clearly-marked
   placeholders that do **not** navigate anywhere broken (visible `[add …]` text,
   or a `mailto:`/`cv.pdf` the user fills in) — never `href="#"` or a broken URL.
2. **01 / About** — 2–4 sentence bio (draft below).
3. **02 / News** — reverse-chronological; two real research start dates.
4. **03 / Research** (centerpiece) — two real projects as *publication-style*
   thumbnail-left rows: title, supervisor, dates, tight summary, monospace tag row,
   placeholder figure slot (swap real figures later), optional `[Code]/[Paper]`
   links (placeholders, clearly marked, not live-dead).
5. **04 / Skills** — the four CV groups verbatim.
6. **05 / Coursework** — the four CV courses.
7. **Contact / footer** — email placeholder, GitHub, `© 2026 Anrui Wang`.

## Content (from `cv/cv.tex` — nothing invented)

**Bio (draft):** "I'm an undergraduate computer science student working at the
intersection of machine learning and biology — *AI for Science*. My research
centers on representation learning for single-cell genomics and graph-structured
biological data: models that read perturbation and dependency signals to predict
synthetic-lethal gene pairs, and that reconstruct protein–protein interaction
networks under topological constraints. I'm broadly interested in single-cell and
graph representation learning, and I'm looking for research opportunities and
collaborations in these areas."

**Research interests:** AI for Science · Single-cell Representation Learning ·
Graph Representation Learning.

**News:**
- *Jan 2026* — Began research with Prof. Xuming He on topology-constrained deep
  learning for inductive interactome reconstruction.
- *Nov 2025* — Started research with Prof. Jie Zheng on multi-omics modeling for
  synthetic-lethality prediction.

**Research row 1 — Synthetic-Lethality Prediction via STATE-Based Multi-Omics
Modeling** — Prof. Jie Zheng · Nov 2025 – present. Predicting synthetic-lethal
gene pairs for cancer-selective drug-target prioritization by integrating
Perturb-seq transcriptomics, DepMap functional dependency (CRISPR GeneEffect),
and ESM2 protein embeddings; models perturbation responses as cell-state
representations via a prototype-based distribution-regression approach that
recovers dependency signal missed by pseudobulk and attention baselines.
Tags: `Perturb-seq` `DepMap` `scVI` `ESM2` `distribution regression`.

**Research row 2 — Topology-Constrained Inductive Interactome Reconstruction** —
Prof. Xuming He · Jan 2026 – present. Reframes protein–protein interaction
prediction from pairwise classification into inductive reconstruction of the full
interaction network via a topology-constrained conditional generator (pairwise
compatibility, hub propensity, community membership, set-level density), distilled
from a masked-edge topology teacher while staying strictly inductive on ESM
features. Tags: `GNN` `graph generation` `topology` `ESM`.

**Skills (verbatim groups):**
- Programming: Python, PyTorch
- ML / Tooling: HuggingFace Accelerate, single-cell foundation models, ESM series,
  graph neural networks
- Coding Agents: Claude Code, Codex
- Domains: single-cell genomics, cancer dependency (DepMap), synthetic lethality,
  protein–protein interaction networks

**Selected coursework:** Machine Learning · Deep Learning · AI for Science and
Engineering · Bioinformatics.

**Publications:** none yet (research began late 2025) → no Publications section;
Research carries that weight.

## Design system

- **Type:** Space Grotesk (headings) · Inter (body, ~17px, line-height ~1.6) ·
  JetBrains Mono (section numbers, dates, tags). Google Fonts.
- **Color (light):** bg `#fafafa`, surface `#ffffff`, text `#1a1a1a`, muted
  `#6b7280`, accent `#2563eb` (links/highlights only). **Dark:** bg `#0d1117`,
  surface `#161b22`, text `#e6edf3`, accent `#60a5fa`. Single accent — no rainbow.
- **Motion:** IntersectionObserver fade/translate-in per section (reveal once, no
  loops); link-underline transition; card hover-lift. Respect
  `prefers-reduced-motion`.
- **Premium cues:** glassy sticky nav; barely-there fixed SVG grain overlay;
  monospace "01 / Research" labels.
- **Robustness:** mobile-first, responsive (research rows collapse to single
  column on narrow screens); only external deps are Google Fonts + Font Awesome;
  working avatar; no `source.unsplash.com`.

## Non-goals (YAGNI)

- No build step, framework, or multi-page site.
- No blog, no publications section (nothing to list yet).
- No heavy animation / 3D / cursor gimmicks.

## `README.md` sync (in scope)

Bring the GitHub profile `README.md` in line with the new page and real CV:
- Replace the wrong tech stack (Python/PyTorch/JS/etc. shields) with the real
  focus — **AI for Science**, single-cell & graph representation learning; skills
  from the CV.
- Fix the malformed avatar reference; drop `contact@example.com`; drop the dead
  "Featured Projects" placeholders in favor of the two real research directions.
- Update `© 2023` → current; keep it tasteful and consistent with the homepage's
  positioning (README can stay badge-friendly, just accurate).
- Keep the GitHub-stats/typing-SVG widgets only if they still render; remove the
  contribution "snake" reference to match the homepage decision.

## Deliverables

1. Rewritten `index.html` (primary).
2. Synced `README.md`.

## Success criteria

- Page renders correctly with **no broken images/links** (desktop + mobile).
- All content is real (from CV) or a clearly-marked placeholder; no fake data.
- Reads as an official AI-for-Science researcher page; passes a quick
  "does this look credible for grad-school admissions" gut check.
- Light/dark both look intentional; Lighthouse-ish: fast, accessible, responsive.
- `README.md` no longer contradicts the homepage (skills, contact, year, avatar).
