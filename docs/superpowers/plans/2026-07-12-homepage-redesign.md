# Homepage Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking. When writing the HTML/CSS/JS, first load the **frontend-design** skill for craft.

**Goal:** Replace the generic dark/purple developer-template `index.html` with a polished, credible, "official" academic homepage for Anrui Wang (AI for Science), built entirely from the real CV, plus a synced `README.md`.

**Architecture:** One self-contained static `index.html` (inline `<style>` + `<script>`, no build step) served by GitHub Pages. Single centered ~760px column in the Jon Barron / al-folio / Clarity idiom: light-minimal with a `prefers-color-scheme`-aware dark toggle, one restrained accent, publication-style research rows as the centerpiece. Then bring `README.md` in line.

**Tech Stack:** Plain HTML/CSS/JS. Google Fonts (Space Grotesk / Inter / JetBrains Mono), Font Awesome 6.4 (icons). No frameworks, no bundler. Verification via `python3 -m http.server` + browser and `grep` structural checks.

## Global Constraints

_Every task implicitly includes these._

- **Name (H1 + `<title>`):** `Anrui Wang`.
- **Content is real or clearly-marked placeholder — never invented.** All research/skills/coursework copy comes verbatim (condensed) from `cv/cv.tex`.
- **Placeholders (only these two):** email → `[you@example.com]`; affiliation → `[Your University]`. Render them as visible bracketed text, not fake real-looking data.
- **Zero dead links on the live page.** Live links: GitHub (`https://github.com/ENNCELADUS`), CV (`cv/cv.pdf`). The email is a marked placeholder that does not navigate anywhere broken. Never `href="#"` or a broken URL. Google Scholar / LinkedIn are **excluded**.
- **Avatar:** `https://github.com/ENNCELADUS.png` (fixes the current malformed URL).
- **No `source.unsplash.com`** anywhere (dead service). No GitHub "snake" animation.
- **Dropped skills:** do NOT list JS/React/Node/AWS/Docker/C++/Java — none are in the CV.
- **Design tokens (use verbatim):**
  - Light: `--bg:#fafafa; --surface:#ffffff; --text:#1a1a1a; --muted:#6b7280; --border:#e5e7eb; --accent:#2563eb; --accent-hover:#1d4ed8;`
  - Dark: `--bg:#0d1117; --surface:#161b22; --text:#e6edf3; --muted:#8b949e; --border:#30363d; --accent:#60a5fa; --accent-hover:#93c5fd;`
- **Fonts:** Space Grotesk (600/700) for headings; Inter (400/500/600) body ~17px, line-height 1.65; JetBrains Mono (400/500) for section numbers, dates, tags.
- **A11y/perf:** mobile-first & responsive; respect `prefers-reduced-motion`; visible keyboard focus; semantic landmarks. Reveal-once motion, nothing looping.
- **Commit** after every task.

## File Structure

- `index.html` — **overwritten** (single deliverable page; inline CSS + JS). The old version stays in git history as reference.
- `README.md` — **modified** to match the new positioning.
- `cv/cv.pdf` — **linked** (already exists; not modified by this plan).
- No new files, no directories.

---

### Task 1: Page shell — head, design tokens, sticky nav, theme toggle, grain

**Files:**
- Modify (overwrite): `index.html`

**Interfaces:**
- Produces: `<html>` gets `data-theme="light|dark"`; a `.reveal` class convention (used by Task 5); CSS custom properties named exactly as in Global Constraints; section anchors `#about #research #skills #contact` and a nav that later tasks link to.

- [ ] **Step 1: Overwrite `index.html` with the shell**

Replace the entire file with:

```html
<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Anrui Wang</title>
    <meta name="description" content="Anrui Wang — CS undergraduate working on AI for Science: single-cell and graph representation learning.">
    <!-- Set theme before paint to avoid flash -->
    <script>
      (function () {
        try {
          var t = localStorage.getItem('theme');
          if (!t) t = matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
          document.documentElement.setAttribute('data-theme', t);
        } catch (e) {}
      })();
    </script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&family=Space+Grotesk:wght@600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
      :root {
        --bg:#fafafa; --surface:#ffffff; --text:#1a1a1a; --muted:#6b7280;
        --border:#e5e7eb; --accent:#2563eb; --accent-hover:#1d4ed8;
        --maxw:760px;
        --sans:'Inter',system-ui,-apple-system,sans-serif;
        --display:'Space Grotesk',var(--sans);
        --mono:'JetBrains Mono',ui-monospace,monospace;
      }
      [data-theme="dark"] {
        --bg:#0d1117; --surface:#161b22; --text:#e6edf3; --muted:#8b949e;
        --border:#30363d; --accent:#60a5fa; --accent-hover:#93c5fd;
      }
      * { margin:0; padding:0; box-sizing:border-box; }
      html { scroll-behavior:smooth; }
      body {
        font-family:var(--sans); font-size:17px; line-height:1.65;
        color:var(--text); background:var(--bg);
        -webkit-font-smoothing:antialiased; overflow-x:hidden;
        transition:background .3s ease, color .3s ease;
      }
      a { color:var(--accent); text-decoration:none; }
      a:hover { color:var(--accent-hover); }
      h1,h2,h3 { font-family:var(--display); line-height:1.2; font-weight:700; }
      .container { max-width:var(--maxw); margin:0 auto; padding:0 24px; }
      section { padding:56px 0; }
      .section-label {
        font-family:var(--mono); font-size:.8rem; font-weight:500;
        color:var(--accent); letter-spacing:.02em; margin-bottom:20px;
        display:block; text-transform:uppercase;
      }
      :focus-visible { outline:2px solid var(--accent); outline-offset:3px; border-radius:3px; }

      /* Grain overlay */
      .grain {
        position:fixed; inset:0; pointer-events:none; z-index:1; opacity:.035;
        background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='120' height='120'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='3'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
      }

      /* Nav */
      .nav {
        position:sticky; top:0; z-index:10;
        backdrop-filter:blur(12px); -webkit-backdrop-filter:blur(12px);
        background:color-mix(in srgb, var(--bg) 78%, transparent);
        border-bottom:1px solid var(--border);
      }
      .nav-inner {
        max-width:var(--maxw); margin:0 auto; padding:12px 24px;
        display:flex; align-items:center; justify-content:space-between;
      }
      .nav-brand { font-family:var(--display); font-weight:700; color:var(--text); font-size:1.05rem; }
      .nav-links { display:flex; gap:22px; align-items:center; }
      .nav-links a { color:var(--muted); font-size:.92rem; font-weight:500; }
      .nav-links a:hover { color:var(--text); }
      .theme-toggle {
        background:none; border:1px solid var(--border); color:var(--text);
        width:34px; height:34px; border-radius:8px; cursor:pointer;
        display:inline-flex; align-items:center; justify-content:center;
        transition:border-color .2s ease, color .2s ease;
      }
      .theme-toggle:hover { border-color:var(--accent); color:var(--accent); }
      @media (max-width:600px) {
        .nav-links { gap:14px; }
        .nav-links a:not(.nav-cv) { display:none; }  /* keep it clean on mobile; CV+toggle stay */
      }
    </style>
</head>
<body>
    <div class="grain" aria-hidden="true"></div>

    <nav class="nav">
      <div class="nav-inner">
        <a class="nav-brand" href="#top">Anrui Wang</a>
        <div class="nav-links">
          <a href="#about">About</a>
          <a href="#research">Research</a>
          <a href="#skills">Skills</a>
          <a class="nav-cv" href="cv/cv.pdf">CV</a>
          <a href="#contact">Contact</a>
          <button class="theme-toggle" id="themeToggle" aria-label="Toggle dark mode">
            <i class="fa-solid fa-moon"></i>
          </button>
        </div>
      </div>
    </nav>

    <main id="top">
      <!-- Header, About, News, Research, Skills, Coursework, Contact added in later tasks -->
    </main>

    <script>
      // Theme toggle
      (function () {
        var btn = document.getElementById('themeToggle');
        var icon = btn.querySelector('i');
        function sync() {
          var dark = document.documentElement.getAttribute('data-theme') === 'dark';
          icon.className = dark ? 'fa-solid fa-sun' : 'fa-solid fa-moon';
        }
        sync();
        btn.addEventListener('click', function () {
          var dark = document.documentElement.getAttribute('data-theme') === 'dark';
          var next = dark ? 'light' : 'dark';
          document.documentElement.setAttribute('data-theme', next);
          try { localStorage.setItem('theme', next); } catch (e) {}
          sync();
        });
      })();
    </script>
</body>
</html>
```

- [ ] **Step 2: Serve and verify the shell**

Run:
```bash
cd /Users/richardwang/Documents/homepage && python3 -m http.server 8000 >/dev/null 2>&1 &
sleep 1 && open http://localhost:8000
```
Expected in browser: off-white page, sticky blurred nav with "Anrui Wang" + links + moon icon. Clicking the toggle flips light↔dark (icon → sun), persists on reload. No console errors. Subtle grain visible.

- [ ] **Step 3: Automated structural checks**

Run:
```bash
grep -c "source.unsplash.com" index.html   # expect 0
grep -c 'href="#"' index.html              # expect 0
grep -c "github.com/ENNCELADUS.png\|ENNCELADUS" index.html  # >=1 (nav/links wired later; ok if only brand now)
grep -c "data-theme" index.html            # expect >=2
```
Expected: first two are `0`.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Rebuild homepage shell: tokens, sticky nav, theme toggle, grain"
```

---

### Task 2: Header, About, and News

**Files:**
- Modify: `index.html` (inside `<main id="top">`, and add CSS to `<style>`)

**Interfaces:**
- Consumes: `.container`, `.section-label`, tokens, `#about` anchor from Task 1.
- Produces: `#about` section (nav target); header link row establishing the live-link/placeholder policy.

- [ ] **Step 1: Add header + about + news markup**

Insert at the top of `<main id="top">`:

```html
<header class="hero">
  <div class="container hero-inner">
    <img class="avatar" src="https://github.com/ENNCELADUS.png" alt="Anrui Wang" width="132" height="132" loading="eager">
    <div class="hero-text">
      <h1>Anrui Wang</h1>
      <p class="hero-tagline">CS undergraduate &middot; AI for Science</p>
      <p class="hero-affil">[Your University]</p>
      <div class="hero-links">
        <span class="placeholder-link"><i class="fa-regular fa-envelope"></i> [you@example.com]</span>
        <a href="cv/cv.pdf"><i class="fa-regular fa-file-lines"></i> CV</a>
        <a href="https://github.com/ENNCELADUS"><i class="fa-brands fa-github"></i> GitHub</a>
      </div>
    </div>
  </div>
</header>

<section id="about" class="reveal">
  <div class="container">
    <span class="section-label">01 / About</span>
    <p class="prose">I'm an undergraduate computer science student working at the intersection of machine learning and biology — <em>AI for Science</em>. My research centers on representation learning for single-cell genomics and graph-structured biological data: models that read perturbation and dependency signals to predict synthetic-lethal gene pairs, and that reconstruct protein–protein interaction networks under topological constraints. I'm broadly interested in single-cell and graph representation learning, and I'm looking for research opportunities and collaborations in these areas.</p>
  </div>
</section>

<section id="news" class="reveal">
  <div class="container">
    <span class="section-label">02 / News</span>
    <ul class="news-list">
      <li><span class="news-date">Jan 2026</span><span>Began research with Prof. Xuming He on topology-constrained deep learning for inductive interactome reconstruction.</span></li>
      <li><span class="news-date">Nov 2025</span><span>Started research with Prof. Jie Zheng on multi-omics modeling for synthetic-lethality prediction.</span></li>
    </ul>
  </div>
</section>
```

- [ ] **Step 2: Add header/about/news CSS**

Append to `<style>`:

```css
.hero { padding:72px 0 40px; position:relative; z-index:2; }
.hero-inner { display:flex; gap:32px; align-items:center; }
.avatar {
  border-radius:50%; object-fit:cover; flex-shrink:0;
  border:1px solid var(--border); box-shadow:0 4px 24px rgba(0,0,0,.08);
}
.hero-text h1 { font-size:2.6rem; letter-spacing:-.02em; }
.hero-tagline { font-size:1.15rem; color:var(--text); margin-top:6px; }
.hero-affil { color:var(--muted); margin-top:2px; }
.hero-links { display:flex; flex-wrap:wrap; gap:18px; margin-top:16px; font-size:.95rem; }
.hero-links a, .placeholder-link { display:inline-flex; align-items:center; gap:7px; }
.placeholder-link { color:var(--muted); font-style:italic; }
.prose { color:var(--text); font-size:1.05rem; }
.prose em { font-style:italic; color:var(--accent); }
.news-list { display:flex; flex-direction:column; gap:12px; }
.news-list li { display:flex; gap:16px; align-items:baseline; }
.news-date {
  font-family:var(--mono); font-size:.82rem; color:var(--muted);
  flex:0 0 74px; white-space:nowrap;
}
@media (max-width:600px) {
  .hero-inner { flex-direction:column; text-align:center; gap:20px; }
  .hero-links { justify-content:center; }
  .hero-text h1 { font-size:2.1rem; }
  .news-list li { flex-direction:column; gap:2px; }
}
```

- [ ] **Step 3: Verify**

Reload `http://localhost:8000`. Expected: avatar image loads from GitHub (round, bordered); name/tagline/`[Your University]`; link row shows italic `[you@example.com]` placeholder (non-clickable), a working **CV** link (opens `cv/cv.pdf`), and **GitHub** link (opens profile). About paragraph reads well; News shows two dated items. Mobile (narrow the window): hero stacks and centers.

Run:
```bash
grep -c "\[you@example.com\]" index.html   # expect 1
grep -c "\[Your University\]" index.html    # expect 1
grep -c "cv/cv.pdf" index.html              # expect >=2 (nav + header)
```

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add header, about, and news sections with real CV content"
```

---

### Task 3: Research section (centerpiece)

**Files:**
- Modify: `index.html` (add `#research` section after `#news`; add CSS)

**Interfaces:**
- Consumes: `.container`, `.section-label`, `.reveal`, tokens.
- Produces: `#research` nav target.

- [ ] **Step 1: Add research markup**

Insert after the `#news` section:

```html
<section id="research" class="reveal">
  <div class="container">
    <span class="section-label">03 / Research</span>

    <article class="pub">
      <div class="pub-thumb thumb-a" aria-hidden="true"></div>
      <div class="pub-body">
        <h3 class="pub-title">Synthetic-Lethality Prediction via STATE-Based Multi-Omics Modeling</h3>
        <p class="pub-meta">Supervised by Prof. Jie Zheng · Nov 2025 – present</p>
        <p class="pub-desc">Predicting synthetic-lethal gene pairs for cancer-selective drug-target prioritization by integrating Perturb-seq perturbation transcriptomics, DepMap functional dependency (CRISPR GeneEffect), and ESM2 protein-sequence embeddings. Models perturbation responses as cell-state representations via a prototype-based distribution-regression approach that recovers dependency signal missed by pseudobulk and attention baselines.</p>
        <div class="pub-tags">
          <span>Perturb-seq</span><span>DepMap</span><span>scVI</span><span>ESM2</span><span>distribution regression</span>
        </div>
      </div>
    </article>

    <article class="pub">
      <div class="pub-thumb thumb-b" aria-hidden="true"></div>
      <div class="pub-body">
        <h3 class="pub-title">Topology-Constrained Inductive Interactome Reconstruction</h3>
        <p class="pub-meta">Supervised by Prof. Xuming He · Jan 2026 – present</p>
        <p class="pub-desc">Reframes protein–protein interaction prediction from independent pairwise classification into inductive reconstruction of the full interaction network, using a topology-constrained conditional generator (pairwise compatibility, hub propensity, community membership, set-level density) distilled from a masked-edge topology teacher while staying strictly inductive on intrinsic ESM protein features.</p>
        <div class="pub-tags">
          <span>GNN</span><span>graph generation</span><span>topology</span><span>ESM</span>
        </div>
      </div>
    </article>
  </div>
</section>
```

- [ ] **Step 2: Add research CSS**

Append to `<style>`:

```css
.pub { display:flex; gap:22px; padding:22px 0; border-top:1px solid var(--border); }
.pub:first-of-type { border-top:none; }
.pub-thumb {
  flex:0 0 140px; height:96px; border-radius:10px; border:1px solid var(--border);
}
.thumb-a { background:linear-gradient(135deg,#2563eb22,#7c3aed22); }
.thumb-b { background:linear-gradient(135deg,#0891b222,#2563eb22); }
[data-theme="dark"] .thumb-a { background:linear-gradient(135deg,#60a5fa2e,#a78bfa2e); }
[data-theme="dark"] .thumb-b { background:linear-gradient(135deg,#22d3ee2e,#60a5fa2e); }
.pub-title { font-size:1.18rem; }
.pub-meta { font-family:var(--mono); font-size:.82rem; color:var(--muted); margin:5px 0 10px; }
.pub-desc { font-size:.98rem; color:var(--text); }
.pub-tags { display:flex; flex-wrap:wrap; gap:8px; margin-top:12px; }
.pub-tags span {
  font-family:var(--mono); font-size:.72rem; color:var(--muted);
  border:1px solid var(--border); border-radius:999px; padding:3px 10px;
}
@media (max-width:600px) {
  .pub { flex-direction:column; gap:14px; }
  .pub-thumb { flex-basis:auto; width:100%; height:80px; }
}
```

- [ ] **Step 3: Verify**

Reload. Expected: two research entries, each a soft-gradient thumbnail on the left + title/supervisor+dates (mono)/description/pill tags. On narrow width, each row stacks (thumbnail full-width above text). No dead links present in the section.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add research section as publication-style rows"
```

---

### Task 4: Skills, Coursework, and Contact/Footer

**Files:**
- Modify: `index.html` (add sections after `#research`; add CSS)

**Interfaces:**
- Consumes: `.container`, `.section-label`, `.reveal`, tokens.
- Produces: `#skills` and `#contact` nav targets.

- [ ] **Step 1: Add markup**

Insert after the `#research` section:

```html
<section id="skills" class="reveal">
  <div class="container">
    <span class="section-label">04 / Skills</span>
    <dl class="skills">
      <div><dt>Programming</dt><dd>Python, PyTorch</dd></div>
      <div><dt>ML / Tooling</dt><dd>HuggingFace Accelerate, single-cell foundation models, ESM series, graph neural networks</dd></div>
      <div><dt>Coding Agents</dt><dd>Claude Code, Codex</dd></div>
      <div><dt>Domains</dt><dd>single-cell genomics, cancer dependency (DepMap), synthetic lethality, protein–protein interaction networks</dd></div>
    </dl>
  </div>
</section>

<section id="coursework" class="reveal">
  <div class="container">
    <span class="section-label">05 / Coursework</span>
    <p class="prose">Machine Learning &middot; Deep Learning &middot; AI for Science and Engineering &middot; Bioinformatics</p>
  </div>
</section>

<section id="contact" class="reveal">
  <div class="container">
    <span class="section-label">Contact</span>
    <p class="prose">Reach me at <span class="placeholder-link">[you@example.com]</span>, or find my code on <a href="https://github.com/ENNCELADUS">GitHub</a>.</p>
  </div>
</section>

<footer class="footer">
  <div class="container">
    <span>© 2026 Anrui Wang</span>
    <a href="https://github.com/ENNCELADUS" aria-label="GitHub"><i class="fa-brands fa-github"></i></a>
  </div>
</footer>
```

- [ ] **Step 2: Add CSS**

Append to `<style>`:

```css
.skills { display:flex; flex-direction:column; gap:14px; }
.skills > div { display:flex; gap:18px; align-items:baseline; }
.skills dt {
  font-family:var(--mono); font-size:.82rem; color:var(--accent);
  flex:0 0 130px; font-weight:500;
}
.skills dd { color:var(--text); font-size:.98rem; }
.footer {
  border-top:1px solid var(--border); padding:28px 0; margin-top:24px;
  position:relative; z-index:2;
}
.footer .container { display:flex; justify-content:space-between; align-items:center; }
.footer span { color:var(--muted); font-size:.9rem; }
.footer a { color:var(--muted); font-size:1.2rem; }
.footer a:hover { color:var(--accent); }
@media (max-width:600px) {
  .skills > div { flex-direction:column; gap:2px; }
  .skills dt { flex-basis:auto; }
}
```

- [ ] **Step 3: Verify**

Reload. Expected: Skills as label/value rows (mono accent labels), coursework line, contact line with placeholder email + live GitHub link, footer with `© 2026 Anrui Wang` and a GitHub icon. Narrow width: skill rows stack. Check dark mode looks intentional across all new sections.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add skills, coursework, contact, and footer"
```

---

### Task 5: Motion and accessibility polish

**Files:**
- Modify: `index.html` (add reveal CSS + IntersectionObserver JS)

**Interfaces:**
- Consumes: `.reveal` class already on every `<section>`.
- Produces: on-scroll reveal; reduced-motion fallback.

- [ ] **Step 1: Add reveal CSS**

Append to `<style>`:

```css
.reveal { opacity:0; transform:translateY(16px); transition:opacity .6s ease, transform .6s ease; }
.reveal.visible { opacity:1; transform:none; }
@media (prefers-reduced-motion: reduce) {
  html { scroll-behavior:auto; }
  .reveal { opacity:1; transform:none; transition:none; }
}
.hero-links a, .nav-links a, .footer a { transition:color .2s ease; }
```

- [ ] **Step 2: Add IntersectionObserver JS**

Insert just before the closing `</script>`... actually add a new `<script>` before `</body>`:

```html
<script>
  (function () {
    var els = document.querySelectorAll('.reveal');
    if (!('IntersectionObserver' in window) ||
        matchMedia('(prefers-reduced-motion: reduce)').matches) {
      els.forEach(function (el) { el.classList.add('visible'); });
      return;
    }
    var io = new IntersectionObserver(function (entries) {
      entries.forEach(function (e) {
        if (e.isIntersecting) { e.target.classList.add('visible'); io.unobserve(e.target); }
      });
    }, { threshold: 0.12, rootMargin: '0px 0px -40px 0px' });
    els.forEach(function (el) { io.observe(el); });
  })();
</script>
```

- [ ] **Step 3: Verify**

Reload and scroll from the top. Expected: each section fades/rises in once as it enters the viewport; already-visible sections near the top reveal immediately (not stuck invisible). Toggle OS "reduce motion" (or trust the fallback) → everything shows with no animation. Keyboard-tab through nav/links → visible focus ring.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add scroll-reveal motion with reduced-motion fallback"
```

---

### Task 6: Full verification sweep

**Files:**
- Modify: `index.html` (only if issues found)

- [ ] **Step 1: Broken-asset & link sweep**

Run:
```bash
cd /Users/richardwang/Documents/homepage
echo "unsplash (want 0):";        grep -c "source.unsplash.com" index.html
echo "href=# (want 0):";          grep -c 'href="#"' index.html
echo "malformed avatar (want 0):"; grep -c "avatars.githubusercontent.com/u/ENNCELADUS" index.html
echo "cv exists:";                ls -la cv/cv.pdf
echo "nav anchors resolve:";
for a in about research skills contact; do grep -q "id=\"$a\"" index.html && echo "  #$a ok" || echo "  #$a MISSING"; done
```
Expected: first three counts `0`; all four anchors `ok`.

- [ ] **Step 2: Responsive + theme visual check**

Serve (if not already) and open. In devtools, check at **375px** and **1280px**, in **both** light and dark:
- No horizontal scroll; hero/research/skills collapse cleanly on mobile.
- Nav stays readable (non-CV links hide on mobile by design; CV + toggle remain).
- Avatar, gradient thumbs, grain, and glass nav all render in both themes.
- Console has no errors/404s (check the Network tab for the font + FA + avatar requests = 200).

- [ ] **Step 3: Fix anything that failed, then commit**

Only if changes were needed:
```bash
git add index.html
git commit -m "Fix issues found in verification sweep"
```
If nothing needed fixing, note it and skip the commit.

---

### Task 7: Sync README.md

**Files:**
- Modify (overwrite): `README.md`

- [ ] **Step 1: Overwrite `README.md`**

Replace the entire file with:

```markdown
# Hi, I'm Anrui Wang 👋

CS undergraduate working at the intersection of **machine learning and biology** — *AI for Science*. I build representation-learning models for single-cell genomics and graph-structured biological data.

🔗 **Homepage:** https://ENNCELADUS.github.io &nbsp;·&nbsp; 🧬 Single-cell & graph representation learning

## 🔬 Research

- **Synthetic-Lethality Prediction via Multi-Omics Modeling** — *with Prof. Jie Zheng.* Predicting synthetic-lethal gene pairs for cancer-selective drug-target prioritization by integrating Perturb-seq transcriptomics, DepMap functional dependency, and ESM2 protein embeddings.
- **Topology-Constrained Interactome Reconstruction** — *with Prof. Xuming He.* Reframing protein–protein interaction prediction as inductive reconstruction of the interaction network with a topology-constrained conditional generator over ESM features.

## 🛠️ Skills

![Python](https://img.shields.io/badge/Python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?style=for-the-badge&logo=PyTorch&logoColor=white)
![Hugging Face](https://img.shields.io/badge/Hugging%20Face-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)

- **ML / Tooling:** HuggingFace Accelerate, single-cell foundation models, ESM series, graph neural networks
- **Domains:** single-cell genomics, cancer dependency (DepMap), synthetic lethality, protein–protein interaction networks

## 📊 GitHub Stats

<div align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=ENNCELADUS&show_icons=true&theme=default" alt="GitHub Stats" />
</div>

## 📫 Connect

- 🌐 Homepage: https://ENNCELADUS.github.io
- 💻 GitHub: [@ENNCELADUS](https://github.com/ENNCELADUS)

<div align="center">
  <sub>📍 Shanghai · 🕒 GMT+08:00 · © 2026 Anrui Wang</sub>
</div>
```

- [ ] **Step 2: Verify README**

Run:
```bash
echo "no unsplash (0):";        grep -c "source.unsplash.com" README.md
echo "no example email (0):";   grep -c "contact@example.com" README.md
echo "no 2023 (0):";            grep -c "2023" README.md
echo "no snake ref (0):";       grep -c "snake" README.md
echo "no C++/Java (0):";        grep -ci "c++\|\bjava\b" README.md
```
Expected: all `0`. Optionally preview the rendered markdown (GitHub or a local markdown viewer) — no broken images, all badges load.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "Sync README with new AI-for-Science positioning"
```

---

## Self-Review

**Spec coverage:**
- Light-minimal academic layout, ~760px column, dark toggle → Task 1. ✅
- Header + avatar (fixed URL) + placeholders (email/affiliation) + link policy → Task 2. ✅
- About (bio) + News (two real dates) → Task 2. ✅
- Research centerpiece as publication-style rows → Task 3. ✅
- Skills (4 CV groups) + Coursework (4 courses) + Contact/footer (© 2026) → Task 4. ✅
- Motion (reveal) + reduced-motion + focus → Task 5. ✅
- Type/color tokens, grain, glass nav → Tasks 1–5 (tokens in Global Constraints). ✅
- Zero dead links, no unsplash, no snake, no C++/Java → enforced + swept in Task 6. ✅
- README sync (skills, avatar, contact, year, snake) → Task 7. ✅
- CV link: upgraded to live `cv/cv.pdf` (file exists) — better than spec's placeholder; noted at handoff. ✅

**Placeholder scan:** The only "placeholders" are the two intentional, spec-mandated bracketed strings (`[you@example.com]`, `[Your University]`), rendered as visible marked text. No TBD/TODO/"handle later" in any step; all code blocks are complete.

**Type/name consistency:** `.reveal`/`.visible`, `data-theme`, `--accent` etc. used identically across Tasks 1→5; section ids (`#about #research #skills #contact`) match nav links from Task 1 and are swept in Task 6.

**Note for handoff:** Linking `cv/cv.pdf` (live) instead of a placeholder is a small upgrade over the spec; the compiled CV still contains its own `# Month #Year` header placeholder and "CS student" line — flag to the user to polish the CV source separately (out of scope for this plan).
