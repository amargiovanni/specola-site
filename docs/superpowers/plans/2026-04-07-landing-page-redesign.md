# Specola Landing Page Redesign — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Complete rewrite of the Specola landing page from dark premium to light editorial warm, with 3 new sections (Per chi e, Confronto, FAQ), Italian language, and subtle fade & rise animations.

**Architecture:** Single-file `index.html` rewrite. All CSS in `<style>`, all JS in `<script>`, zero external dependencies except Google Fonts. Incremental build — each task produces a working page that can be verified in the browser.

**Tech Stack:** HTML5, CSS3 (custom properties), vanilla JavaScript (Intersection Observer, scroll events), Google Fonts (Instrument Serif, DM Sans, JetBrains Mono).

**Spec:** `docs/superpowers/specs/2026-04-07-landing-page-redesign-design.md`

---

## File Map

- **Modify:** `index.html` — complete rewrite, same file

---

### Task 1: Foundation — HTML head, CSS variables, global styles

**Files:**
- Modify: `index.html` (full rewrite — replace entire file)

This task creates the skeleton: `<head>`, CSS custom properties, reset, typography, grain overlay, animation keyframes, and an empty `<body>`. After this task the page loads as a blank warm canvas with paper grain texture.

- [ ] **Step 1: Write the foundation HTML**

Replace the entire contents of `index.html` with:

```html
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Specola — Il tuo osservatorio quotidiano</title>
    <meta name="description" content="App nativa macOS che trasforma centinaia di fonti RSS in briefing giornalieri intelligenti. Analisi AI con Claude, OpenAI o LM Studio. Output in DOCX, PDF o EPUB.">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
    <style>
        :root {
            --canvas: #fafaf9;
            --paper: #ffffff;
            --cream: #f5f0eb;
            --ink: #1a1a1a;
            --muted: #6b6155;
            --dim: #a39888;
            --gold: #c4a882;
            --gold-dark: #8b6914;
            --border: #e8e0d6;
            --cta-bg: #1a1a1a;
            --cta-hover: #c4a882;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        html { scroll-behavior: smooth; }

        body {
            font-family: 'DM Sans', sans-serif;
            background: var(--canvas);
            color: var(--muted);
            line-height: 1.7;
            overflow-x: hidden;
        }

        /* Grain overlay */
        body::before {
            content: '';
            position: fixed;
            inset: 0;
            background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.02'/%3E%3C/svg%3E");
            pointer-events: none;
            z-index: 9999;
        }

        /* Typography helpers */
        h1, h2, h3 { font-weight: 400; }

        .section-title {
            font-family: 'Instrument Serif', serif;
            font-size: clamp(2rem, 4vw, 2.5rem);
            color: var(--ink);
            line-height: 1.2;
            margin-bottom: 1rem;
        }

        .section-title em {
            font-style: italic;
        }

        .section-subtitle {
            font-size: 1.05rem;
            color: var(--muted);
            max-width: 520px;
            line-height: 1.8;
            margin-bottom: 3rem;
        }

        /* Scroll reveal */
        .reveal {
            opacity: 0;
            transform: translateY(20px);
            transition: opacity 0.8s ease-out, transform 0.8s ease-out;
        }

        .reveal.visible {
            opacity: 1;
            transform: translateY(0);
        }

        /* Stagger delays */
        .reveal-d1 { transition-delay: 0.15s; }
        .reveal-d2 { transition-delay: 0.3s; }
        .reveal-d3 { transition-delay: 0.45s; }
        .reveal-d4 { transition-delay: 0.6s; }

        /* Hero animation (plays on load, not scroll) */
        @keyframes fade-up {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Reduced motion */
        @media (prefers-reduced-motion: reduce) {
            .reveal { opacity: 1; transform: none; transition: none; }
            * { animation-duration: 0.01ms !important; }
        }

/* === PLACEHOLDER: sections CSS will be added by subsequent tasks === */

    </style>
</head>
<body>

<!-- Content will be added by subsequent tasks -->

<script>
// JS will be added in Task 13
</script>

</body>
</html>
```

- [ ] **Step 2: Verify in browser**

Open `index.html` in a browser. Expected: blank warm page (`#fafaf9` background) with subtle paper grain texture visible on close inspection.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat(site): scaffold foundation with editorial warm theme variables"
```

---

### Task 2: Navigation

**Files:**
- Modify: `index.html` — add nav CSS before the placeholder comment, add nav HTML after `<body>`

- [ ] **Step 1: Add navigation CSS**

Insert before the `/* === PLACEHOLDER` comment in `<style>`:

```css
        /* === NAVIGATION === */
        nav {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            z-index: 100;
            padding: 1.25rem 2rem;
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: rgba(250, 250, 249, 0.85);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            transition: all 0.3s ease-out;
        }

        nav.scrolled {
            padding: 0.75rem 2rem;
            border-bottom: 1px solid var(--border);
        }

        .nav-brand {
            display: flex;
            align-items: center;
            gap: 0.75rem;
            text-decoration: none;
        }

        .nav-diamond {
            width: 10px;
            height: 10px;
            background: var(--gold);
            transform: rotate(45deg);
            flex-shrink: 0;
        }

        .nav-title {
            font-family: 'Instrument Serif', serif;
            font-size: 1.25rem;
            color: var(--ink);
            letter-spacing: 0.04em;
        }

        .nav-links {
            display: flex;
            gap: 2rem;
            align-items: center;
        }

        .nav-links a {
            color: var(--muted);
            text-decoration: none;
            font-size: 0.85rem;
            font-weight: 400;
            transition: color 0.3s ease-out;
        }

        .nav-links a:hover { color: var(--ink); }

        .nav-cta {
            background: var(--cta-bg) !important;
            color: var(--paper) !important;
            padding: 0.5rem 1.25rem;
            border-radius: 6px;
            font-weight: 500 !important;
            transition: all 0.3s ease-out !important;
        }

        .nav-cta:hover {
            background: var(--cta-hover) !important;
            color: var(--ink) !important;
        }

        /* Mobile hamburger */
        .nav-hamburger {
            display: none;
            background: none;
            border: none;
            cursor: pointer;
            padding: 4px;
        }

        .nav-hamburger span {
            display: block;
            width: 20px;
            height: 2px;
            background: var(--ink);
            margin: 4px 0;
            transition: all 0.3s;
        }

        .nav-mobile {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            background: var(--paper);
            padding: 5rem 2rem 2rem;
            flex-direction: column;
            gap: 1.5rem;
            z-index: 99;
            border-bottom: 1px solid var(--border);
        }

        .nav-mobile.open { display: flex; }

        .nav-mobile a {
            color: var(--ink);
            text-decoration: none;
            font-size: 1.1rem;
            font-weight: 500;
        }
```

- [ ] **Step 2: Add navigation HTML**

Replace `<!-- Content will be added by subsequent tasks -->` with:

```html
<!-- NAV -->
<nav>
    <a href="#" class="nav-brand">
        <div class="nav-diamond"></div>
        <span class="nav-title">Specola</span>
    </a>
    <div class="nav-links">
        <a href="#come-funziona">Come funziona</a>
        <a href="#funzionalita">Funzionalit&agrave;</a>
        <a href="#per-chi">Per chi &egrave;</a>
        <a href="#faq">FAQ</a>
        <a href="https://github.com/amargiovanni/specola" class="nav-cta">Scarica su GitHub</a>
    </div>
    <button class="nav-hamburger" onclick="document.querySelector('.nav-mobile').classList.toggle('open')" aria-label="Menu">
        <span></span><span></span><span></span>
    </button>
</nav>
<div class="nav-mobile">
    <a href="#come-funziona" onclick="this.parentElement.classList.remove('open')">Come funziona</a>
    <a href="#funzionalita" onclick="this.parentElement.classList.remove('open')">Funzionalit&agrave;</a>
    <a href="#per-chi" onclick="this.parentElement.classList.remove('open')">Per chi &egrave;</a>
    <a href="#faq" onclick="this.parentElement.classList.remove('open')">FAQ</a>
    <a href="https://github.com/amargiovanni/specola" onclick="this.parentElement.classList.remove('open')">Scarica su GitHub</a>
</div>

<!-- Remaining sections will be added by subsequent tasks -->
```

- [ ] **Step 3: Verify in browser**

Open `index.html`. Expected: fixed navigation bar at top with warm background, gold diamond logo, "Specola" in serif, 4 nav links + GitHub CTA button. Page background is warm canvas.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat(site): add editorial navigation with mobile hamburger"
```

---

### Task 3: Hero section

**Files:**
- Modify: `index.html` — add hero CSS before placeholder comment, add hero HTML after nav mobile div

- [ ] **Step 1: Add hero CSS**

Insert after the nav CSS block (before `/* === PLACEHOLDER`):

```css
        /* === HERO === */
        .hero {
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            padding: 8rem 2rem 4rem;
            background: var(--canvas);
        }

        .hero-glow {
            position: absolute;
            width: 600px;
            height: 600px;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(196,168,130,0.06) 0%, transparent 70%);
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            pointer-events: none;
        }

        .hero-content {
            position: relative;
            z-index: 2;
            max-width: 720px;
            text-align: center;
        }

        .hero-eyebrow {
            display: inline-block;
            font-size: 0.75rem;
            font-weight: 500;
            letter-spacing: 0.15em;
            text-transform: uppercase;
            color: var(--dim);
            margin-bottom: 1.5rem;
            opacity: 0;
            animation: fade-up 0.8s ease-out forwards;
        }

        .hero h1 {
            font-family: 'Instrument Serif', serif;
            font-size: clamp(2.5rem, 6vw, 3.5rem);
            line-height: 1.15;
            color: var(--ink);
            letter-spacing: -0.5px;
            margin-bottom: 1.5rem;
            opacity: 0;
            animation: fade-up 0.8s ease-out 0.15s forwards;
        }

        .hero-subtitle {
            font-size: 1.15rem;
            color: var(--muted);
            max-width: 560px;
            margin: 0 auto 2.5rem;
            line-height: 1.8;
            font-weight: 400;
            opacity: 0;
            animation: fade-up 0.8s ease-out 0.3s forwards;
        }

        .hero-actions {
            display: flex;
            gap: 1rem;
            justify-content: center;
            flex-wrap: wrap;
            opacity: 0;
            animation: fade-up 0.8s ease-out 0.45s forwards;
        }

        .btn-primary {
            background: var(--cta-bg);
            color: var(--paper);
            padding: 0.85rem 2rem;
            border-radius: 6px;
            text-decoration: none;
            font-weight: 500;
            font-size: 0.95rem;
            transition: all 0.3s ease-out;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            border: none;
            cursor: pointer;
        }

        .btn-primary:hover {
            background: var(--cta-hover);
            color: var(--ink);
        }

        .btn-secondary {
            background: transparent;
            color: var(--ink);
            padding: 0.85rem 2rem;
            border-radius: 6px;
            text-decoration: none;
            font-weight: 400;
            font-size: 0.95rem;
            border: 1px solid var(--border);
            transition: all 0.3s ease-out;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
        }

        .btn-secondary:hover {
            background: var(--cream);
        }

        .hero-stats {
            margin-top: 2.5rem;
            font-size: 0.82rem;
            color: var(--dim);
            opacity: 0;
            animation: fade-up 0.8s ease-out 0.6s forwards;
        }

        .hero-stats span { margin: 0 0.5rem; }
```

- [ ] **Step 2: Add hero HTML**

Insert after `<!-- Remaining sections will be added by subsequent tasks -->`:

```html
<!-- HERO -->
<section class="hero">
    <div class="hero-glow"></div>
    <div class="hero-content">
        <div class="hero-eyebrow">Applicazione macOS</div>
        <h1>Il tuo osservatorio<br>quotidiano su ci&ograve;<br>che conta</h1>
        <p class="hero-subtitle">
            Specola trasforma centinaia di fonti RSS in un briefing intelligente, ogni mattina. Analisi AI, zero rumore.
        </p>
        <div class="hero-actions">
            <a href="https://github.com/amargiovanni/specola" class="btn-primary">
                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"/></svg>
                Scarica Specola
            </a>
            <a href="#come-funziona" class="btn-secondary">Scopri come funziona</a>
        </div>
        <div class="hero-stats">
            200+ fonti <span>&middot;</span> 3 formati <span>&middot;</span> 3 minuti al giorno
        </div>
    </div>
</section>

<!-- Remaining sections will be added by subsequent tasks -->
```

- [ ] **Step 3: Verify in browser**

Expected: full-height hero with warm canvas background, subtle center glow, staggered fade-up animations for eyebrow → headline → subtitle → CTAs → stats. Dark primary CTA button, outlined secondary button. Gold hover on primary CTA.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat(site): add hero section with Italian copy and fade-up animations"
```

---

### Task 4: Come funziona (3 steps)

**Files:**
- Modify: `index.html` — add CSS + HTML

- [ ] **Step 1: Add "Come funziona" CSS**

Insert after hero CSS (before `/* === PLACEHOLDER`):

```css
        /* === COME FUNZIONA === */
        .flow {
            padding: 6rem 2rem;
            background: var(--cream);
        }

        .flow-inner {
            max-width: 1000px;
            margin: 0 auto;
        }

        .flow-steps {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 24px;
            margin-top: 3rem;
        }

        .flow-step {
            background: var(--paper);
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 2rem;
        }

        .flow-step-num {
            font-family: 'Instrument Serif', serif;
            font-size: 3rem;
            color: var(--gold);
            line-height: 1;
            margin-bottom: 1rem;
        }

        .flow-step-label {
            font-size: 0.7rem;
            font-weight: 600;
            letter-spacing: 0.15em;
            text-transform: uppercase;
            color: var(--dim);
            margin-bottom: 0.5rem;
        }

        .flow-step h3 {
            font-family: 'DM Sans', sans-serif;
            font-size: 1.1rem;
            font-weight: 600;
            color: var(--ink);
            margin-bottom: 0.5rem;
        }

        .flow-step p {
            font-size: 0.92rem;
            color: var(--muted);
            line-height: 1.7;
        }
```

- [ ] **Step 2: Add "Come funziona" HTML**

Insert after the hero `</section>`, replacing `<!-- Remaining sections will be added by subsequent tasks -->`:

```html
<!-- COME FUNZIONA -->
<section class="flow" id="come-funziona">
    <div class="flow-inner">
        <div class="reveal">
            <h2 class="section-title">Tre passaggi, zero sforzo</h2>
            <p class="section-subtitle">Configura una volta, ricevi ogni giorno. Specola gira silenziosamente nel menubar e gestisce tutto.</p>
        </div>
        <div class="flow-steps">
            <div class="flow-step reveal">
                <div class="flow-step-num">01</div>
                <div class="flow-step-label">Raccogli</div>
                <h3>RSS in parallelo</h3>
                <p>Parsing OPML automatico, 200+ feed concorrenti su 20 thread paralleli. Tu importi le fonti, Specola fa il resto.</p>
            </div>
            <div class="flow-step reveal reveal-d1">
                <div class="flow-step-num">02</div>
                <div class="flow-step-label">Analizza</div>
                <h3>Pre-filtro + dual-model AI</h3>
                <p>Pre-filtro locale intelligente che taglia il 60-75% del rumore. Poi analisi dual-model: veloce per categoria, approfondita per la sintesi.</p>
            </div>
            <div class="flow-step reveal reveal-d2">
                <div class="flow-step-num">03</div>
                <div class="flow-step-label">Ricevi</div>
                <h3>Il formato che preferisci</h3>
                <p>Scegli il formato &mdash; DOCX, PDF o EPUB. Ogni briefing archiviato nel portale HTML consultabile.</p>
            </div>
        </div>
    </div>
</section>

<!-- Remaining sections will be added by subsequent tasks -->
```

- [ ] **Step 3: Verify in browser**

Expected: cream background section with 3 white cards in a row. Gold numbers (01, 02, 03), uppercase labels, bold titles, muted descriptions. Cards fade-rise with stagger on scroll.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat(site): add 'Come funziona' section with 3-step flow cards"
```

---

### Task 5: Il briefing (Preview)

**Files:**
- Modify: `index.html` — add CSS + HTML

- [ ] **Step 1: Add preview CSS**

Insert after flow CSS (before `/* === PLACEHOLDER`):

```css
        /* === IL BRIEFING === */
        .preview {
            padding: 6rem 2rem;
            background: var(--canvas);
        }

        .preview-inner {
            max-width: 1000px;
            margin: 0 auto;
        }

        .preview-grid {
            display: grid;
            grid-template-columns: 1fr 1.2fr;
            gap: 3rem;
            align-items: center;
            margin-top: 3rem;
        }

        .briefing-sections {
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        .briefing-item {
            display: flex;
            align-items: flex-start;
            gap: 1rem;
            padding: 0.75rem 0;
        }

        .briefing-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: var(--gold);
            margin-top: 0.5rem;
            flex-shrink: 0;
        }

        .briefing-item strong {
            display: block;
            color: var(--ink);
            font-size: 0.95rem;
            font-weight: 600;
            margin-bottom: 0.15rem;
        }

        .briefing-item span {
            font-size: 0.85rem;
            color: var(--muted);
            line-height: 1.6;
        }

        /* DOCX mockup */
        .docx-mockup {
            background: var(--paper);
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 8px 32px rgba(0,0,0,0.08);
            transform: perspective(800px) rotateY(-2deg);
            transition: transform 0.5s ease-out;
            border-top: 4px solid var(--gold);
        }

        .docx-mockup:hover {
            transform: perspective(800px) rotateY(0deg);
        }

        .docx-header {
            padding: 0.75rem 1.5rem;
            border-bottom: 1px solid var(--border);
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 0.65rem;
            color: var(--dim);
        }

        .docx-header .diamond {
            display: inline-block;
            width: 6px;
            height: 6px;
            background: var(--gold);
            transform: rotate(45deg);
            margin-right: 6px;
        }

        .docx-body {
            padding: 2rem 2.5rem;
        }

        .docx-body h2 {
            font-family: 'Instrument Serif', serif;
            font-size: 1.3rem;
            color: var(--ink);
            border-bottom: 3px solid var(--gold);
            padding-bottom: 0.5rem;
            margin-bottom: 1.25rem;
        }

        .docx-body h3 {
            font-size: 0.85rem;
            color: var(--ink);
            border-left: 3px solid var(--gold);
            padding-left: 0.75rem;
            margin: 1.25rem 0 0.5rem;
            font-weight: 600;
        }

        .docx-body p {
            font-size: 0.78rem;
            color: var(--muted);
            line-height: 1.7;
            margin-bottom: 0.5rem;
        }

        .docx-body a {
            color: var(--gold-dark);
            text-decoration: underline;
            font-size: 0.78rem;
        }

        .docx-body .tag {
            display: inline-block;
            background: var(--cream);
            color: var(--ink);
            font-size: 0.65rem;
            padding: 0.15rem 0.5rem;
            border-radius: 3px;
            font-weight: 500;
        }

        .docx-fade {
            height: 60px;
            background: linear-gradient(to bottom, transparent, var(--paper));
            margin-top: -60px;
            position: relative;
        }
```

- [ ] **Step 2: Add preview HTML**

Insert after the flow `</section>`:

```html
<!-- IL BRIEFING -->
<section class="preview" id="briefing">
    <div class="preview-inner">
        <div class="reveal">
            <h2 class="section-title"><em>Non un riassunto. Un briefing di intelligence.</em></h2>
        </div>
        <div class="preview-grid">
            <div class="briefing-sections reveal">
                <div class="briefing-item">
                    <div class="briefing-dot"></div>
                    <div>
                        <strong>Da sapere oggi</strong>
                        <span>Le novit&agrave; chiave della giornata</span>
                    </div>
                </div>
                <div class="briefing-item">
                    <div class="briefing-dot"></div>
                    <div>
                        <strong>Richiede attenzione</strong>
                        <span>Sviluppi che impattano il tuo lavoro</span>
                    </div>
                </div>
                <div class="briefing-item">
                    <div class="briefing-dot"></div>
                    <div>
                        <strong>Per area tematica</strong>
                        <span>Analisi approfondita per argomento</span>
                    </div>
                </div>
                <div class="briefing-item">
                    <div class="briefing-dot"></div>
                    <div>
                        <strong>Da leggere con calma</strong>
                        <span>Articoli lunghi selezionati per te</span>
                    </div>
                </div>
                <div class="briefing-item">
                    <div class="briefing-dot"></div>
                    <div>
                        <strong>Spunti</strong>
                        <span>Idee, connessioni, provocazioni</span>
                    </div>
                </div>
            </div>
            <div class="reveal reveal-d1">
                <div class="docx-mockup">
                    <div class="docx-header">
                        <span><span class="diamond"></span> SPECOLA</span>
                        <span>2026-04-07</span>
                    </div>
                    <div class="docx-body">
                        <h2>Specola &mdash; Briefing del 7 aprile 2026</h2>
                        <h3>Da sapere oggi</h3>
                        <p><strong>L'AI Act entra nella fase esecutiva.</strong> Da oggi le aziende che operano nell'UE devono conformarsi ai requisiti di trasparenza per i sistemi ad alto rischio. Per chi sviluppa SaaS B2B, questo significa audit trail obbligatori e documentazione tecnica verificabile. <a href="#">(Il Sole 24 Ore)</a></p>
                        <p><strong>GitHub lancia Copilot Workspace in GA.</strong> L'ambiente di sviluppo AI-first esce dalla beta. Integra planning, coding e testing in un flusso unico. Rilevante per team che stanno valutando l'adozione di AI coding tools. <a href="#">(GitHub Blog)</a></p>
                        <h3>Richiede attenzione</h3>
                        <p><span class="tag">AZIONE</span> La scadenza PSD3 per l'adeguamento delle API di pagamento si avvicina. Se il tuo prodotto gestisce transazioni B2B nell'eurozona, verifica la compliance entro il 15 aprile. <a href="#">(EBA)</a></p>
                    </div>
                    <div class="docx-fade"></div>
                </div>
            </div>
        </div>
    </div>
</section>

<!-- Remaining sections will be added by subsequent tasks -->
```

- [ ] **Step 3: Verify in browser**

Expected: canvas background, italic serif title, 2-column layout. Left: 5 briefing items with gold dots. Right: white DOCX mockup with gold top border, slight 3D rotation, realistic Italian content, fade-out at bottom. Mockup flattens on hover.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat(site): add briefing preview section with DOCX mockup"
```

---

### Task 6: Funzionalita (Features grid)

**Files:**
- Modify: `index.html` — add CSS + HTML

- [ ] **Step 1: Add features CSS**

Insert after preview CSS (before `/* === PLACEHOLDER`):

```css
        /* === FUNZIONALITA === */
        .features {
            padding: 6rem 2rem;
            background: var(--cream);
        }

        .features-inner {
            max-width: 1000px;
            margin: 0 auto;
        }

        .features-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 16px;
            margin-top: 3rem;
        }

        .feature {
            background: var(--paper);
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 1.5rem;
            transition: all 0.3s ease-out;
        }

        .feature:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.06);
        }

        .feature-icon {
            font-size: 1.25rem;
            margin-bottom: 0.75rem;
        }

        .feature h3 {
            font-family: 'DM Sans', sans-serif;
            font-size: 0.95rem;
            font-weight: 600;
            color: var(--ink);
            margin-bottom: 0.35rem;
        }

        .feature p {
            font-size: 0.85rem;
            color: var(--muted);
            line-height: 1.6;
        }
```

- [ ] **Step 2: Add features HTML**

Insert after the preview `</section>`:

```html
<!-- FUNZIONALITA -->
<section class="features" id="funzionalita">
    <div class="features-inner">
        <div class="reveal">
            <h2 class="section-title">Costruito per il tuo workflow</h2>
        </div>
        <div class="features-grid">
            <div class="feature reveal">
                <div class="feature-icon">&#x1f9e0;</div>
                <h3>Scegli la tua AI</h3>
                <p>Claude, OpenAI o locale con LM Studio. Cambi in qualsiasi momento dalle impostazioni.</p>
            </div>
            <div class="feature reveal">
                <div class="feature-icon">&#x26a1;</div>
                <h3>Dual-model intelligente</h3>
                <p>Modello veloce per l'analisi per categoria, modello capace per la sintesi finale. ~80% di risparmio.</p>
            </div>
            <div class="feature reveal">
                <div class="feature-icon">&#x1f3af;</div>
                <h3>Su misura per te</h3>
                <p>Profilo professionale che filtra ci&ograve; che conta. Ruolo, stack, interessi, progetti attivi.</p>
            </div>
            <div class="feature reveal">
                <div class="feature-icon">&#x1f4d1;</div>
                <h3>DOCX, PDF o EPUB</h3>
                <p>Il formato che preferisci, pronto in un click. Tipografia professionale con link cliccabili.</p>
            </div>
            <div class="feature reveal">
                <div class="feature-icon">&#x1f310;</div>
                <h3>Portale HTML</h3>
                <p>Archivio consultabile di tutti i tuoi briefing. Apri nel browser, condividi, metti nei preferiti.</p>
            </div>
            <div class="feature reveal">
                <div class="feature-icon">&#x1f514;</div>
                <h3>Widget Centro Notifiche</h3>
                <p>I punti chiave sempre visibili sul desktop. Tocca per aprire il briefing completo.</p>
            </div>
            <div class="feature reveal">
                <div class="feature-icon">&#x23f0;</div>
                <h3>Programmalo e dimenticalo</h3>
                <p>Scheduling automatico, briefing pronti ogni mattina. Se il Mac dorme, parte al risveglio.</p>
            </div>
            <div class="feature reveal">
                <div class="feature-icon">&#x1f517;</div>
                <h3>Ogni voce ha la fonte</h3>
                <p>Link originali cliccabili in ogni sezione. Il briefing &egrave; un punto di partenza, non un vicolo cieco.</p>
            </div>
            <div class="feature reveal">
                <div class="feature-icon">&#x1f30d;</div>
                <h3>Italiano e inglese</h3>
                <p>Briefing nella lingua che preferisci. Stessa profondit&agrave;, stessa struttura, stessa qualit&agrave;.</p>
            </div>
            <div class="feature reveal">
                <div class="feature-icon">&#x1f3a8;</div>
                <h3>Tre temi visivi</h3>
                <p>Corporate, Minimal o Dark per i tuoi documenti. Applicato a HTML, DOCX, PDF e EPUB.</p>
            </div>
            <div class="feature reveal">
                <div class="feature-icon">&#x1f50d;</div>
                <h3>Ricerca Spotlight</h3>
                <p>Trova qualsiasi argomento nei briefing passati. Indicizzazione automatica, zero tag manuali.</p>
            </div>
            <div class="feature reveal">
                <div class="feature-icon">&#x1f4ca;</div>
                <h3>Sparkline nel menubar</h3>
                <p>Attivit&agrave; dei feed a colpo d'occhio. 7 barre mostrano il volume degli ultimi briefing.</p>
            </div>
        </div>
    </div>
</section>

<!-- Remaining sections will be added by subsequent tasks -->
```

- [ ] **Step 3: Verify in browser**

Expected: cream background, 2-column grid of white cards. Each card has emoji icon, bold title, muted description. Cards lift slightly on hover with shadow. Cards fade-rise on scroll.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat(site): add features grid with 12 feature cards in Italian"
```

---

### Task 7: Per chi e (NEW — Persona cards)

**Files:**
- Modify: `index.html` — add CSS + HTML

- [ ] **Step 1: Add persona CSS**

Insert after features CSS (before `/* === PLACEHOLDER`):

```css
        /* === PER CHI E === */
        .personas {
            padding: 6rem 2rem;
            background: var(--canvas);
        }

        .personas-inner {
            max-width: 1000px;
            margin: 0 auto;
        }

        .personas-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 24px;
            margin-top: 3rem;
        }

        .persona {
            background: var(--paper);
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 2.5rem 2rem;
            text-align: center;
        }

        .persona-icon {
            font-size: 2.5rem;
            margin-bottom: 1rem;
        }

        .persona h3 {
            font-family: 'DM Sans', sans-serif;
            font-size: 1.2rem;
            font-weight: 600;
            color: var(--ink);
            margin-bottom: 0.75rem;
        }

        .persona p {
            font-size: 0.9rem;
            color: var(--muted);
            line-height: 1.7;
            margin-bottom: 1.25rem;
        }

        .persona-quote {
            font-family: 'Instrument Serif', serif;
            font-style: italic;
            font-size: 0.9rem;
            color: var(--gold-dark);
            line-height: 1.5;
        }

        .persona-quote::before {
            content: '\201C';
            font-size: 1.5rem;
            line-height: 0;
            vertical-align: -0.3em;
            margin-right: 2px;
        }
```

- [ ] **Step 2: Add persona HTML**

Insert after the features `</section>`:

```html
<!-- PER CHI E -->
<section class="personas" id="per-chi">
    <div class="personas-inner">
        <div class="reveal">
            <h2 class="section-title"><em>Specola &egrave; per chi...</em></h2>
        </div>
        <div class="personas-grid">
            <div class="persona reveal">
                <div class="persona-icon">&#x1f52d;</div>
                <h3>Il leader tecnico</h3>
                <p>CTO, dev lead, architetto. Monitora 200+ fonti tech senza perderti nel rumore. Architettura multi-LLM, funziona offline con LM Studio. I tuoi dati restano tuoi.</p>
                <div class="persona-quote">Finalmente un briefing che capisce la differenza tra hype e signal.</div>
            </div>
            <div class="persona reveal reveal-d1">
                <div class="persona-icon">&#x1f4bc;</div>
                <h3>L'analista</h3>
                <p>Ricercatore, consulente, PM. Un briefing strutturato come un report professionale, pronto da condividere. DOCX con fonti cliccabili, esportabile per il team.</p>
                <div class="persona-quote">Come avere un analista junior che legge tutto e ti prepara la sintesi.</div>
            </div>
            <div class="persona reveal reveal-d2">
                <div class="persona-icon">&#x2615;</div>
                <h3>Il curioso efficiente</h3>
                <p>Knowledge worker, fondatore, appassionato. Tre minuti al mattino per sapere tutto. Si configura al primo avvio, scheduling automatico, zero manutenzione.</p>
                <div class="persona-quote">La mia routine mattutina adesso inizia con Specola e un caff&egrave;.</div>
            </div>
        </div>
    </div>
</section>

<!-- Remaining sections will be added by subsequent tasks -->
```

- [ ] **Step 3: Verify in browser**

Expected: canvas background, italic serif title, 3 white persona cards centered. Each has large emoji, bold name, description, italic gold quote with decorative opening quote mark. Cards stagger on scroll.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat(site): add 'Per chi e' section with 3 persona cards"
```

---

### Task 8: Confronto (NEW — Comparison table)

**Files:**
- Modify: `index.html` — add CSS + HTML

- [ ] **Step 1: Add comparison CSS**

Insert after persona CSS (before `/* === PLACEHOLDER`):

```css
        /* === CONFRONTO === */
        .comparison {
            padding: 6rem 2rem;
            background: var(--cream);
        }

        .comparison-inner {
            max-width: 1000px;
            margin: 0 auto;
        }

        .comparison-table-wrap {
            margin-top: 3rem;
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
        }

        .comparison-table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.88rem;
            min-width: 600px;
        }

        .comparison-table thead th {
            text-align: left;
            padding: 1rem 1.25rem;
            font-weight: 600;
            color: var(--muted);
            font-size: 0.8rem;
            letter-spacing: 0.02em;
            border-bottom: 1px solid var(--border);
        }

        .comparison-table thead th:first-child {
            color: var(--dim);
        }

        .comparison-table thead th.highlight {
            color: var(--ink);
            border-bottom: 3px solid var(--gold);
        }

        .comparison-table tbody td {
            padding: 1rem 1.25rem;
            border-bottom: 1px solid var(--border);
            color: var(--muted);
            vertical-align: top;
        }

        .comparison-table tbody td:first-child {
            font-weight: 500;
            color: var(--ink);
            font-size: 0.85rem;
        }

        .comparison-table tbody td.highlight {
            color: var(--ink);
            font-weight: 500;
            background: rgba(196,168,130,0.04);
        }

        .comparison-check {
            color: var(--gold);
            font-weight: 700;
        }
```

- [ ] **Step 2: Add comparison HTML**

Insert after the personas `</section>`:

```html
<!-- CONFRONTO -->
<section class="comparison" id="confronto">
    <div class="comparison-inner">
        <div class="reveal">
            <h2 class="section-title">Perch&eacute; Specola</h2>
        </div>
        <div class="comparison-table-wrap reveal">
            <table class="comparison-table">
                <thead>
                    <tr>
                        <th></th>
                        <th class="highlight">Specola</th>
                        <th>Feed manuali</th>
                        <th>Newsletter generiche</th>
                        <th>Aggregatori AI</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>Personalizzazione</td>
                        <td class="highlight">Profilo + keyword</td>
                        <td>Manuale</td>
                        <td>Nessuna</td>
                        <td>Basica</td>
                    </tr>
                    <tr>
                        <td>Analisi profonda</td>
                        <td class="highlight">Dual-model AI</td>
                        <td>Nessuna</td>
                        <td>Editoriale</td>
                        <td>Single-model</td>
                    </tr>
                    <tr>
                        <td>Formato output</td>
                        <td class="highlight">DOCX / PDF / EPUB / HTML</td>
                        <td>Browser</td>
                        <td>Email</td>
                        <td>Web</td>
                    </tr>
                    <tr>
                        <td>Privacy</td>
                        <td class="highlight">Offline possibile</td>
                        <td>OK</td>
                        <td>Dati a terzi</td>
                        <td>Cloud-only</td>
                    </tr>
                    <tr>
                        <td>Costo per uso</td>
                        <td class="highlight">Ottimizzato (&minus;60% token)</td>
                        <td>Gratis ma lento</td>
                        <td>Gratis</td>
                        <td>Alto</td>
                    </tr>
                    <tr>
                        <td>Tempo al giorno</td>
                        <td class="highlight">3 minuti</td>
                        <td>60+ minuti</td>
                        <td>15 minuti</td>
                        <td>10 minuti</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</section>

<!-- Remaining sections will be added by subsequent tasks -->
```

- [ ] **Step 3: Verify in browser**

Expected: cream background, serif title, clean comparison table. Specola column highlighted with gold border and subtle background tint. Scrollable horizontally on mobile.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat(site): add comparison table section"
```

---

### Task 9: Architettura

**Files:**
- Modify: `index.html` — add CSS + HTML

- [ ] **Step 1: Add architecture CSS**

Insert after comparison CSS (before `/* === PLACEHOLDER`):

```css
        /* === ARCHITETTURA === */
        .arch {
            padding: 6rem 2rem;
            background: var(--canvas);
        }

        .arch-inner {
            max-width: 1000px;
            margin: 0 auto;
        }

        .arch-diagram {
            margin-top: 3rem;
            background: var(--paper);
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 2.5rem;
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.78rem;
            line-height: 1.9;
            color: var(--muted);
            overflow-x: auto;
            white-space: pre;
        }

        .arch-diagram .hl { color: var(--gold); }
        .arch-diagram .bl { color: var(--gold-dark); }
        .arch-diagram .wh { color: var(--ink); font-weight: 500; }
```

- [ ] **Step 2: Add architecture HTML**

Insert after the comparison `</section>`:

```html
<!-- ARCHITETTURA -->
<section class="arch" id="architettura">
    <div class="arch-inner">
        <div class="reveal">
            <h2 class="section-title">Sotto il cofano</h2>
            <p class="section-subtitle">Tre componenti, un contratto. L'app Swift gestisce UI e scheduling. Il motore Python fa analisi e rendering. Il widget WidgetKit mostra i punti chiave nel Centro Notifiche.</p>
        </div>
        <div class="arch-diagram reveal"><span class="wh">Swift App</span>                              <span class="wh">Python Engine</span>

  MenuBarExtra          <span class="hl">───args──&#9654;</span>      parse_opml()
  Popover                                 fetch_feeds()  <span class="bl">20 threads</span>
  Settings              <span class="hl">&#9664;──JSON──</span>      build_prompt()
  Scheduler  <span class="bl">60s timer</span>                  analyze()  <span class="bl">dual-model: fast + capable</span>
  Notifications                           generate_html()  <span class="bl">sempre</span>
                                          generate_docx() <span class="bl">o</span> pdf() <span class="bl">o</span> epub()
  <span class="wh">Widget Extension</span>                      portal_index()  <span class="bl">archivio statico</span>
  WidgetKit  <span class="bl">systemMedium/Large</span>
  <span class="hl">&#9664;── App Group ──&#9654;</span>  widget_data.json

  <span class="hl">~/Library/Application Support/Specola/</span>
  ├── engine/.venv/     <span class="bl">creato automaticamente al primo avvio</span>
  ├── Feeds.opml        <span class="bl">copia dell'OPML utente</span>
  ├── profile.md        <span class="bl">profilo professionale</span>
  └── history.json      <span class="bl">ultimi 30 briefing</span>

  <span class="hl">~/Documents/Specola/</span>
  ├── index.html        <span class="bl">portale archivio navigabile</span>
  ├── Specola_*.html    <span class="bl">briefing standalone</span>
  └── Specola_*.docx    <span class="bl">o .pdf o .epub</span></div>
    </div>
</section>

<!-- Remaining sections will be added by subsequent tasks -->
```

- [ ] **Step 3: Verify in browser**

Expected: canvas background, serif title, subtitle, monospace architecture diagram on white card with warm accents. Gold for structural elements, dark gold for annotations.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat(site): add architecture diagram section in Italian"
```

---

### Task 10: Installazione

**Files:**
- Modify: `index.html` — add CSS + HTML

- [ ] **Step 1: Add install CSS**

Insert after architecture CSS (before `/* === PLACEHOLDER`):

```css
        /* === INSTALLAZIONE === */
        .install {
            padding: 6rem 2rem;
            background: var(--cream);
        }

        .install-inner {
            max-width: 700px;
            margin: 0 auto;
            text-align: center;
        }

        .install-code {
            background: #1a1a1a;
            border-radius: 12px;
            padding: 2rem 2.5rem;
            text-align: left;
            margin: 2.5rem 0;
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.82rem;
            line-height: 2;
            color: #e8e0d6;
            overflow-x: auto;
            white-space: pre-wrap;
        }

        .install-code .comment { color: #6b6155; }
        .install-code .cmd { color: #fafaf9; }
        .install-code .flag { color: #c4a882; }

        .install-note {
            font-size: 0.88rem;
            color: var(--muted);
            line-height: 1.8;
            max-width: 480px;
            margin: 0 auto 2rem;
        }

        .install-link {
            color: var(--gold-dark);
            text-decoration: none;
            font-weight: 500;
            transition: color 0.3s ease-out;
        }

        .install-link:hover { color: var(--ink); }

        .install-link::after {
            content: ' \2192';
        }
```

- [ ] **Step 2: Add install HTML**

Insert after the architecture `</section>`:

```html
<!-- INSTALLAZIONE -->
<section class="install" id="installa">
    <div class="install-inner">
        <div class="reveal">
            <h2 class="section-title">Tre comandi per il<br>tuo primo briefing</h2>
        </div>
        <div class="install-code reveal">
<span class="comment"># Clona e compila</span>
<span class="cmd">git clone</span> https://github.com/amargiovanni/specola.git
<span class="cmd">cd</span> specola
<span class="cmd">xcodebuild</span> <span class="flag">-project</span> Specola.xcodeproj <span class="flag">-scheme</span> Specola <span class="flag">-configuration</span> Release <span class="flag">-derivedDataPath</span> build

<span class="comment"># Installa</span>
<span class="cmd">cp -R</span> build/Build/Products/Release/Specola.app /Applications/
        </div>
        <p class="install-note reveal">
            Il motore Python &egrave; incluso nell'app e si configura automaticamente al primo avvio. Scegli il provider AI nelle impostazioni, importa i feed OPML, scrivi il tuo profilo e premi &ldquo;Genera ora&rdquo;.
        </p>
        <div class="reveal">
            <a href="https://github.com/amargiovanni/specola" class="install-link">Vedi su GitHub</a>
        </div>
    </div>
</section>

<!-- Remaining sections will be added by subsequent tasks -->
```

- [ ] **Step 3: Verify in browser**

Expected: cream background, centered content, dark code block with warm-toned syntax highlighting. Install note below, gold "Vedi su GitHub" link with arrow.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat(site): add installation section with dark code block"
```

---

### Task 11: FAQ (NEW — Accordion)

**Files:**
- Modify: `index.html` — add CSS + HTML

- [ ] **Step 1: Add FAQ CSS**

Insert after install CSS (before `/* === PLACEHOLDER`):

```css
        /* === FAQ === */
        .faq {
            padding: 6rem 2rem;
            background: var(--canvas);
        }

        .faq-inner {
            max-width: 680px;
            margin: 0 auto;
        }

        .faq-list {
            margin-top: 3rem;
        }

        .faq-item {
            border-bottom: 1px solid var(--border);
        }

        .faq-question {
            width: 100%;
            background: none;
            border: none;
            padding: 1.25rem 0;
            font-family: 'DM Sans', sans-serif;
            font-size: 1rem;
            font-weight: 600;
            color: var(--ink);
            text-align: left;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 1rem;
        }

        .faq-question:hover { color: var(--gold-dark); }

        .faq-chevron {
            width: 20px;
            height: 20px;
            flex-shrink: 0;
            transition: transform 0.3s ease-out;
            color: var(--dim);
        }

        .faq-item.open .faq-chevron {
            transform: rotate(180deg);
        }

        .faq-answer {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.3s ease-out;
        }

        .faq-answer-inner {
            padding: 0 0 1.25rem;
            font-size: 0.92rem;
            color: var(--muted);
            line-height: 1.7;
        }
```

- [ ] **Step 2: Add FAQ HTML**

Insert after the install `</section>`:

```html
<!-- FAQ -->
<section class="faq" id="faq">
    <div class="faq-inner">
        <div class="reveal">
            <h2 class="section-title">Domande frequenti</h2>
        </div>
        <div class="faq-list reveal">
            <div class="faq-item">
                <button class="faq-question" onclick="toggleFaq(this)">
                    Serve una chiave API?
                    <svg class="faq-chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
                </button>
                <div class="faq-answer"><div class="faq-answer-inner">Dipende dal provider. Con Claude Code CLI e LM Studio non serve. Con OpenAI s&igrave;, ma la configuri una sola volta nel setup guidato.</div></div>
            </div>
            <div class="faq-item">
                <button class="faq-question" onclick="toggleFaq(this)">
                    Funziona offline?
                    <svg class="faq-chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
                </button>
                <div class="faq-answer"><div class="faq-answer-inner">S&igrave;, con LM Studio. Scarichi il modello una volta e Specola analizza tutto in locale, senza mai inviare dati fuori dal tuo Mac.</div></div>
            </div>
            <div class="faq-item">
                <button class="faq-question" onclick="toggleFaq(this)">
                    Quanto costa usarlo?
                    <svg class="faq-chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
                </button>
                <div class="faq-answer"><div class="faq-answer-inner">Specola &egrave; gratuito e open source (MIT). Paghi solo l'eventuale uso API del provider AI che scegli. Il pre-filtro riduce i costi del 60-75%.</div></div>
            </div>
            <div class="faq-item">
                <button class="faq-question" onclick="toggleFaq(this)">
                    Su quali Mac funziona?
                    <svg class="faq-chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
                </button>
                <div class="faq-answer"><div class="faq-answer-inner">Qualsiasi Mac con macOS 14 (Sonoma) o successivo. Apple Silicon raccomandato per LM Studio locale.</div></div>
            </div>
            <div class="faq-item">
                <button class="faq-question" onclick="toggleFaq(this)">
                    Posso personalizzare le fonti?
                    <svg class="faq-chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
                </button>
                <div class="faq-answer"><div class="faq-answer-inner">S&igrave;. Importi un file OPML con le tue fonti RSS, oppure le aggiungi una alla volta. Puoi organizzarle per categoria.</div></div>
            </div>
            <div class="faq-item">
                <button class="faq-question" onclick="toggleFaq(this)">
                    In che formati genera il briefing?
                    <svg class="faq-chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
                </button>
                <div class="faq-answer"><div class="faq-answer-inner">DOCX, PDF ed EPUB. Ogni briefing &egrave; anche disponibile nel portale HTML integrato per la consultazione via browser.</div></div>
            </div>
            <div class="faq-item">
                <button class="faq-question" onclick="toggleFaq(this)">
                    Come scelgo il modello AI?
                    <svg class="faq-chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
                </button>
                <div class="faq-answer"><div class="faq-answer-inner">Dalle impostazioni: Claude (via CLI), OpenAI (con API key), o qualsiasi modello locale via LM Studio. Puoi cambiare in qualsiasi momento.</div></div>
            </div>
            <div class="faq-item">
                <button class="faq-question" onclick="toggleFaq(this)">
                    I miei dati dove vanno?
                    <svg class="faq-chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
                </button>
                <div class="faq-answer"><div class="faq-answer-inner">Restano sul tuo Mac. Feed, briefing e configurazione sono in ~/Library/Application&nbsp;Support/Specola. Nessun cloud, nessun tracking.</div></div>
            </div>
        </div>
    </div>
</section>

<!-- Remaining sections will be added by subsequent tasks -->
```

- [ ] **Step 3: Verify in browser**

Expected: canvas background, centered FAQ with 8 questions. Each question has chevron icon on right. Clicking does nothing yet (JS added in Task 13). Questions have gold hover color.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat(site): add FAQ accordion section with 8 questions"
```

---

### Task 12: Footer

**Files:**
- Modify: `index.html` — add CSS + HTML

- [ ] **Step 1: Add footer CSS**

Insert after FAQ CSS (before `/* === PLACEHOLDER`):

```css
        /* === FOOTER === */
        footer {
            background: #1a1a1a;
            padding: 3.5rem 2rem;
            text-align: center;
        }

        .footer-cta {
            display: inline-block;
            border: 1px solid var(--gold);
            color: var(--gold);
            padding: 0.85rem 2rem;
            border-radius: 6px;
            text-decoration: none;
            font-weight: 500;
            font-size: 0.95rem;
            transition: all 0.3s ease-out;
            margin-bottom: 2rem;
        }

        .footer-cta:hover {
            background: var(--gold);
            color: #1a1a1a;
        }

        .footer-links {
            margin-bottom: 1rem;
        }

        .footer-links a {
            color: #e8e0d6;
            text-decoration: none;
            font-size: 0.85rem;
            transition: color 0.3s ease-out;
        }

        .footer-links a:hover { color: var(--gold); }

        .footer-links span {
            color: #6b6155;
            margin: 0 0.75rem;
        }

        .footer-credit {
            font-size: 0.8rem;
            color: #6b6155;
        }
```

- [ ] **Step 2: Add footer HTML**

Replace `<!-- Remaining sections will be added by subsequent tasks -->` (after FAQ section) with:

```html
<!-- FOOTER -->
<footer>
    <a href="https://github.com/amargiovanni/specola" class="footer-cta">Prova Specola</a>
    <div class="footer-links">
        <a href="https://github.com/amargiovanni/specola">GitHub</a>
        <span>&middot;</span>
        <a href="https://github.com/amargiovanni/specola/blob/main/LICENSE">MIT License</a>
    </div>
    <p class="footer-credit">Fatto con Swift, Python e Claude Code</p>
</footer>
```

- [ ] **Step 3: Verify in browser**

Expected: dark footer with gold-outlined "Prova Specola" CTA that fills gold on hover. GitHub and MIT links in warm cream. Credit in dim gray. Clean, focused.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat(site): add dark footer with gold CTA"
```

---

### Task 13: JavaScript — Scroll reveal, nav behavior, FAQ accordion

**Files:**
- Modify: `index.html` — replace the empty `<script>` block

- [ ] **Step 1: Write JavaScript**

Replace the entire `<script>` block (currently containing `// JS will be added in Task 13`) with:

```html
<script>
// Scroll reveal with Intersection Observer
const revealObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.classList.add('visible');
        }
    });
}, { threshold: 0.15, rootMargin: '0px 0px -40px 0px' });

document.querySelectorAll('.reveal').forEach(el => revealObserver.observe(el));

// Nav scroll behavior
window.addEventListener('scroll', () => {
    const nav = document.querySelector('nav');
    if (window.scrollY > 50) {
        nav.classList.add('scrolled');
    } else {
        nav.classList.remove('scrolled');
    }
});

// FAQ accordion
function toggleFaq(button) {
    const item = button.parentElement;
    const answer = item.querySelector('.faq-answer');
    const isOpen = item.classList.contains('open');

    // Close all others
    document.querySelectorAll('.faq-item.open').forEach(openItem => {
        if (openItem !== item) {
            openItem.classList.remove('open');
            openItem.querySelector('.faq-answer').style.maxHeight = null;
        }
    });

    // Toggle current
    if (isOpen) {
        item.classList.remove('open');
        answer.style.maxHeight = null;
    } else {
        item.classList.add('open');
        answer.style.maxHeight = answer.scrollHeight + 'px';
    }
}
</script>
```

- [ ] **Step 2: Verify in browser**

Test all three behaviors:
1. **Scroll reveal:** Scroll down the page. Sections should fade-rise into view with staggered timing. Each element animates only once (no re-animation on scroll up).
2. **Nav:** Scroll past 50px. Nav should get `scrolled` class: compressed padding, border-bottom appears. Scroll back to top: nav returns to normal.
3. **FAQ:** Click a question. Answer slides open, chevron rotates 180deg. Click another: previous closes, new one opens. Click same: it closes.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat(site): add scroll reveal, nav scroll behavior, FAQ accordion JS"
```

---

### Task 14: Responsive styles

**Files:**
- Modify: `index.html` — replace the placeholder comment with responsive CSS

- [ ] **Step 1: Add responsive CSS**

Replace `/* === PLACEHOLDER: sections CSS will be added by subsequent tasks === */` with:

```css
        /* === RESPONSIVE === */
        @media (max-width: 1024px) {
            .personas-grid { grid-template-columns: 1fr 1fr; }
            .personas-grid .persona:last-child { grid-column: 1 / -1; max-width: 400px; margin: 0 auto; }
        }

        @media (max-width: 768px) {
            .nav-links { display: none; }
            .nav-hamburger { display: block; }

            .hero { padding: 7rem 1.5rem 3rem; min-height: auto; }
            .hero h1 { font-size: 2.2rem; }
            .hero-subtitle { font-size: 1rem; }

            .flow-steps { grid-template-columns: 1fr; }
            .preview-grid { grid-template-columns: 1fr; }
            .docx-mockup { transform: none; }
            .docx-mockup:hover { transform: none; }
            .features-grid { grid-template-columns: 1fr; }
            .personas-grid { grid-template-columns: 1fr; }
            .personas-grid .persona:last-child { max-width: none; }

            .section-title { font-size: 1.75rem; }
            .section-subtitle { font-size: 0.95rem; }

            .comparison-table { font-size: 0.8rem; }
            .comparison-table thead th,
            .comparison-table tbody td { padding: 0.75rem; }

            .arch-diagram { font-size: 0.68rem; padding: 1.5rem; }
        }
```

- [ ] **Step 2: Verify in browser**

Resize browser to <768px width. Expected:
- Nav shows hamburger button, links hidden
- Hero text smaller, comfortable on mobile
- All grids become single column
- DOCX mockup flat (no 3D)
- Comparison table scrolls horizontally
- Architecture diagram font smaller

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat(site): add responsive breakpoints for tablet and mobile"
```

---

### Task 15: Final verification and commit

**Files:**
- Verify: `index.html`

- [ ] **Step 1: Full page review**

Open `index.html` in a desktop browser. Scroll through the entire page and verify each section renders correctly:

1. Nav: fixed, blur, scrolled state works
2. Hero: centered, staggered animations, both CTAs clickable
3. Come funziona: 3 cards, gold numbers, cream background
4. Il briefing: 2-column layout, DOCX mockup with 3D perspective, gold top border
5. Funzionalita: 2-column grid, hover lift on cards
6. Per chi e: 3 persona cards, italic quotes
7. Confronto: comparison table, Specola column highlighted
8. Architettura: monospace diagram, gold accents
9. Installazione: dark code block, warm syntax colors
10. FAQ: accordion opens/closes, one at a time, chevron rotates
11. Footer: dark bg, gold CTA button, cream links

- [ ] **Step 2: Mobile review**

Open in mobile viewport (375px). Verify:
- Hamburger menu works
- All grids are single column
- No horizontal overflow
- Text is readable

- [ ] **Step 3: Check reduced motion**

In browser dev tools, emulate `prefers-reduced-motion: reduce`. Verify all animations are disabled.

- [ ] **Step 4: Commit final state**

```bash
git add index.html
git commit -m "feat(site): complete editorial warm redesign with Italian copy

Radical redesign from dark premium to light editorial warm:
- New color palette with warm cream, gold accents, paper texture
- 3 new sections: Per chi e, Confronto, FAQ
- All copy translated to Italian
- Subtle fade & rise scroll animations
- Responsive layout for mobile and tablet
- FAQ accordion with single-open behavior
- Dark footer with gold CTA for final conversion"
```
