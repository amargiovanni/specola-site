# Specola Landing Page — Redesign Spec

> Radical redesign of specola-site: from dark premium to light editorial, conversion-optimized, Italian language.

---

## Context

Specola is a native macOS menubar app that transforms 200+ RSS feeds into AI-powered daily intelligence briefings. The current landing page is a single-file HTML with a dark premium aesthetic. This redesign moves to a warm editorial light mode to maximize conversion and appeal to a broad audience.

**Approach:** Single-file HTML rewrite (Approach 1). Zero dependencies, zero build step, vanilla JS + CSS variables. Same architecture, completely new design.

---

## Design Decisions

| Decision | Choice | Rationale |
|---|---|---|
| Visual direction | Editorial / Warm | Premium paper-like feel matches a product that generates daily briefings |
| Target audience | Universal hero, deep-dive for both tech leads and knowledge workers | Broad appeal without diluting the message |
| Animations | Subtle fade & rise, staggered | Elegant, never distracting, premium feel |
| New sections | Comparison, FAQ, Who it's for | Remove objections, help visitors self-identify |
| Preview | Improved DOCX mockup in editorial style | Show the output quality, build desire |
| Language | Italian only | Primary target market |
| Architecture | Single HTML file, no build tools | Fast to deploy, zero complexity, instant load |

---

## Visual Foundation

### Color Palette

```css
:root {
  --canvas: #fafaf9;        /* Main background */
  --paper: #ffffff;          /* Cards, elevated elements */
  --cream: #f5f0eb;          /* Alternate section backgrounds */
  --ink: #1a1a1a;            /* Headlines, primary text */
  --muted: #6b6155;          /* Secondary text, descriptions */
  --dim: #a39888;            /* Tertiary text, labels */
  --gold: #c4a882;           /* Decorative lines, dividers, hover states */
  --gold-dark: #8b6914;      /* Links, interactive elements */
  --border: #e8e0d6;         /* Card borders, separators */
  --cta-bg: #1a1a1a;         /* Primary CTA background */
  --cta-hover: #c4a882;      /* Primary CTA hover */
}
```

### Typography

| Role | Font | Weight | Usage |
|---|---|---|---|
| Headlines | Instrument Serif | 400 | Section titles, hero headline |
| Accent | Instrument Serif *italic* | 400 italic | Taglines, quotes, emphasis |
| Body/UI | DM Sans | 400, 500, 600 | Body text, buttons, navigation |
| Code | JetBrains Mono | 400 | Code blocks, terminal commands |

All three fonts are already loaded via Google Fonts in the current site.

### Global Effects

- **Grain overlay:** SVG fractal noise at 2% opacity for paper texture
- **Card shadow:** `0 1px 3px rgba(0,0,0,0.04)` — barely visible depth
- **Border radius:** `8px` on cards, `6px` on buttons
- **Scroll animations:** `0.8s ease-out` fade-up with `0.15s` stagger between siblings
- **Hover transitions:** `0.3s ease-out` for all interactive elements
- **Smooth scroll:** `scroll-behavior: smooth` on html

---

## Page Structure

### 1. Navigation (fixed)

- **Layout:** Logo left, nav links center, CTA right
- **Logo:** Diamond icon + "Specola" in Instrument Serif
- **Links:** Come funziona, Funzionalita, Per chi e, FAQ
- **CTA:** "Scarica su GitHub" compact button (DM Sans 500)
- **Scroll behavior:** Compresses on scroll, gains `backdrop-filter: blur(12px)`, bottom border `var(--border)`
- **Mobile:** Hamburger menu, links in slide-down panel

### 2. Hero

- **Background:** `var(--canvas)` with subtle warm radial gradient center (`rgba(196,168,130,0.06)`)
- **Eyebrow:** `APPLICAZIONE MACOS` — DM Sans 500, uppercase, `letter-spacing: 2px`, color `var(--dim)`, font-size `12px`
- **Headline:** *"Il tuo osservatorio quotidiano su cio che conta"* — Instrument Serif, `48px` desktop / `32px` mobile, color `var(--ink)`, `letter-spacing: -0.5px`
- **Subtitle:** "Specola trasforma centinaia di fonti RSS in un briefing intelligente, ogni mattina. Analisi AI, zero rumore." — DM Sans 400, `18px`, color `var(--muted)`, max-width `560px`
- **CTA row:**
  - Primary: "Scarica Specola" — `var(--cta-bg)` background, white text, `padding: 14px 28px`, `border-radius: 6px`, hover → `var(--cta-hover)` bg with ink text
  - Secondary: "Scopri come funziona" — transparent, `border: 1px solid var(--border)`, color `var(--ink)`, hover → `var(--cream)` bg
- **Micro-stats row:** Below CTAs, `margin-top: 32px`. Three items separated by `·`: "200+ fonti" · "3 formati" · "3 minuti al giorno" — DM Sans 400, `13px`, color `var(--dim)`
- **Animation:** Staggered fade-rise: eyebrow (0s) → headline (0.15s) → subtitle (0.3s) → CTAs (0.45s) → stats (0.6s)

### 3. Come funziona

- **Background:** `var(--cream)`
- **Section title:** "Tre passaggi, zero sforzo" — Instrument Serif, `36px`
- **Layout:** 3-column grid (`1fr 1fr 1fr`), gap `24px`. Mobile: single column
- **Cards:** White background, `border: 1px solid var(--border)`, `border-radius: 8px`, `padding: 32px`
  - **Number:** Instrument Serif, `48px`, color `var(--gold)`, margin-bottom `16px`
  - **Label:** DM Sans 600, `11px`, uppercase, `letter-spacing: 1.5px`, color `var(--dim)`
  - **Title:** DM Sans 600, `18px`, color `var(--ink)`
  - **Description:** DM Sans 400, `15px`, color `var(--muted)`, 2 lines max
- **Content:**
  - 01 / RACCOGLI: "Parsing OPML automatico, 200+ feed concorrenti su 20 thread paralleli. Tu importi le fonti, Specola fa il resto."
  - 02 / ANALIZZA: "Pre-filtro locale intelligente che taglia il 60-75% del rumore. Poi analisi dual-model: veloce per categoria, approfondita per la sintesi."
  - 03 / RICEVI: "Scegli il formato — DOCX, PDF o EPUB. Ogni briefing archiviato nel portale HTML consultabile."
- **Animation:** Cards fade-rise with 0.15s stagger on scroll-reveal

### 4. Il briefing (Preview)

- **Background:** `var(--canvas)`
- **Section title:** *"Non un riassunto. Un briefing di intelligence."* — Instrument Serif italic, `36px`
- **Layout:** 2-column grid (`1fr 1fr`), gap `48px`. Mobile: single column
- **Left column — Briefing sections list:**
  - 5 items, each with:
    - Gold dot (`8px` circle, `var(--gold)`)
    - Section name in DM Sans 600, `16px`, color `var(--ink)`
    - One-line description in DM Sans 400, `14px`, color `var(--muted)`
  - Sections:
    1. Da sapere oggi — Le novita chiave della giornata
    2. Richiede attenzione — Sviluppi che impattano il tuo lavoro
    3. Per area tematica — Analisi approfondita per argomento
    4. Da leggere con calma — Articoli lunghi selezionati per te
    5. Spunti — Idee, connessioni, provocazioni
- **Right column — DOCX mockup:**
  - White card with `box-shadow: 0 8px 32px rgba(0,0,0,0.08)`
  - Slight 3D rotation: `transform: perspective(800px) rotateY(-2deg)`
  - Header with briefing title and date
  - Sample content in Italian with colored tags
  - Fade-out gradient at bottom (`linear-gradient(transparent, white)`)
  - Gold accent line at top of document (`4px solid var(--gold)`)

### 5. Funzionalita

- **Background:** `var(--cream)`
- **Section title:** "Costruito per il tuo workflow" — Instrument Serif, `36px`
- **Layout:** 2-column grid, gap `16px`. Mobile: single column
- **Cards:** White bg, `border: 1px solid var(--border)`, `border-radius: 8px`, `padding: 24px`
  - Emoji icon `24px`, margin-bottom `12px`
  - Title: DM Sans 600, `15px`, color `var(--ink)`
  - Description: DM Sans 400, `14px`, color `var(--muted)`, 1 line
  - Hover: `translateY(-2px)`, shadow `0 4px 12px rgba(0,0,0,0.06)`
- **12 features (Italian):**
  1. Scegli la tua AI — Claude, OpenAI o locale con LM Studio
  2. Dual-model intelligente — Modello veloce per categoria, capace per la sintesi
  3. Su misura per te — Profilo professionale che filtra cio che conta
  4. DOCX, PDF o EPUB — Il formato che preferisci, pronto in un click
  5. Portale HTML — Archivio consultabile di tutti i tuoi briefing
  6. Widget Centro Notifiche — I punti chiave sempre visibili sul desktop
  7. Programmalo e dimenticalo — Scheduling automatico, briefing pronti ogni mattina
  8. Ogni voce ha la fonte — Link originali cliccabili in ogni sezione
  9. Italiano e inglese — Briefing nella lingua che preferisci
  10. Tre temi visivi — Corporate, Minimal o Dark per i tuoi documenti
  11. Ricerca Spotlight — Trova qualsiasi argomento nei briefing passati
  12. Sparkline nel menubar — Attivita dei feed a colpo d'occhio

### 6. Per chi e (NEW)

- **Background:** `var(--canvas)`
- **Section title:** *"Specola e per chi..."* — Instrument Serif italic, `36px`
- **Layout:** 3-column grid, gap `24px`. Mobile: single column
- **Persona cards:** White bg, `border: 1px solid var(--border)`, `border-radius: 8px`, `padding: 32px`, text-align center
  - Emoji: `40px`, margin-bottom `16px`
  - Role title: DM Sans 600, `20px`, color `var(--ink)`
  - Description: DM Sans 400, `15px`, color `var(--muted)`, 3-4 lines
  - Quote: Instrument Serif italic, `14px`, color `var(--gold-dark)`, `margin-top: 20px`, with decorative `"` before
- **Content:**
  - **Il leader tecnico** (emoji: telescope)
    - "CTO, dev lead, architetto. Monitora 200+ fonti tech senza perderti nel rumore. Architettura multi-LLM, funziona offline con LM Studio. I tuoi dati restano tuoi."
    - *"Finalmente un briefing che capisce la differenza tra hype e signal."*
  - **L'analista** (emoji: briefcase)
    - "Ricercatore, consulente, PM. Un briefing strutturato come un report professionale, pronto da condividere. DOCX con fonti cliccabili, esportabile per il team."
    - *"Come avere un analista junior che legge tutto e ti prepara la sintesi."*
  - **Il curioso efficiente** (emoji: coffee)
    - "Knowledge worker, fondatore, appassionato. Tre minuti al mattino per sapere tutto. Si configura al primo avvio, scheduling automatico, zero manutenzione."
    - *"La mia routine mattutina adesso inizia con Specola e un caffe."*

### 7. Confronto (NEW)

- **Background:** `var(--cream)`
- **Section title:** "Perche Specola" — Instrument Serif, `36px`
- **Layout:** Full-width table, responsive (horizontal scroll on mobile)
- **Table design:**
  - Header row: `var(--cream)` bg, DM Sans 600, `14px`
  - Specola column header: highlighted with `var(--gold)` bottom border
  - Body cells: DM Sans 400, `14px`, color `var(--muted)`
  - Specola column values: color `var(--ink)`, DM Sans 500
  - Checkmarks: `var(--gold)` colored
  - Row borders: `1px solid var(--border)`
- **Columns:** Specola | Feed manuali | Newsletter generiche | Aggregatori AI
- **Rows:**
  - Personalizzazione: Profilo + keyword | Manuale | Nessuna | Basica
  - Analisi profonda: Dual-model AI | Nessuna | Editoriale | Single-model
  - Formato output: DOCX/PDF/EPUB/HTML | Browser | Email | Web
  - Privacy: Offline possibile | OK | Dati a terzi | Cloud-only
  - Costo per uso: Ottimizzato (-60% token) | Gratis ma lento | Gratis | Alto
  - Tempo al giorno: 3 minuti | 60+ minuti | 15 minuti | 10 minuti

### 8. Architettura

- **Background:** `var(--canvas)`
- **Section title:** "Sotto il cofano" — Instrument Serif, `36px`
- **Subtitle:** "Tre componenti, un contratto" — DM Sans 400, `16px`, color `var(--muted)`
- **Layout:** Description text + styled architecture diagram
- **Diagram:** Reworked from ASCII to styled HTML boxes
  - 3 main boxes (Swift / Python / WidgetKit) with warm borders
  - Connection lines represented with CSS borders/pseudo-elements in gold
  - Box labels in DM Sans 600, contents in DM Sans 400
  - Background `var(--paper)` for each box
- **Content matches current site** but translated to Italian labels where appropriate

### 9. Installazione

- **Background:** `var(--cream)`
- **Section title:** "Tre comandi per il tuo primo briefing" — Instrument Serif, `36px`
- **Code block:**
  - Background: `#1a1a1a` (dark contrast block on light page)
  - Font: JetBrains Mono, `14px`
  - Text: `#e8e0d6` (warm light on dark)
  - Border-radius: `12px`
  - Padding: `24px 32px`
  - Content: `git clone`, `cd`, `xcodebuild` commands
- **Note below:** "Python incluso nell'app. Si configura automaticamente al primo avvio." — DM Sans 400, `14px`, color `var(--muted)`
- **GitHub link:** text link with arrow, color `var(--gold-dark)`

### 10. FAQ (NEW)

- **Background:** `var(--canvas)`
- **Section title:** "Domande frequenti" — Instrument Serif, `36px`
- **Layout:** Single column, max-width `680px`, centered
- **Accordion items:**
  - Question: DM Sans 600, `16px`, color `var(--ink)`, padding `20px 0`
  - Chevron icon right-aligned, rotates on open
  - Answer: DM Sans 400, `15px`, color `var(--muted)`, padding-bottom `20px`
  - Divider: `1px solid var(--border)` between items
  - Animation: `max-height` transition, `0.3s ease-out`
- **Questions:**
  1. Serve una chiave API? — "Dipende dal provider. Con Claude Code CLI e LM Studio non serve. Con OpenAI si, ma la configuri una sola volta nel setup guidato."
  2. Funziona offline? — "Si, con LM Studio. Scarichi il modello una volta e Specola analizza tutto in locale, senza mai inviare dati fuori dal tuo Mac."
  3. Quanto costa usarlo? — "Specola e gratuito e open source (MIT). Paghi solo l'eventuale uso API del provider AI che scegli. Il pre-filtro riduce i costi del 60-75%."
  4. Su quali Mac funziona? — "Qualsiasi Mac con macOS 14 (Sonoma) o successivo. Apple Silicon raccomandato per LM Studio locale."
  5. Posso personalizzare le fonti? — "Si. Importi un file OPML con le tue fonti RSS, oppure le aggiungi una alla volta. Puoi organizzarle per categoria."
  6. In che formati genera il briefing? — "DOCX, PDF ed EPUB. Ogni briefing e anche disponibile nel portale HTML integrato per la consultazione via browser."
  7. Come scelgo il modello AI? — "Dalle impostazioni: Claude (via CLI), OpenAI (con API key), o qualsiasi modello locale via LM Studio. Puoi cambiare in qualsiasi momento."
  8. I miei dati dove vanno? — "Restano sul tuo Mac. Feed, briefing e configurazione sono in ~/Library/Application Support/Specola. Nessun cloud, nessun tracking."

### 11. Footer

- **Background:** `#1a1a1a` (dark inversion)
- **Text color:** `#e8e0d6` (warm cream on dark)
- **Layout:** Centered, `padding: 48px`
- **Content:**
  - Repeated CTA: "Prova Specola" button with gold border, gold text, hover → gold bg with ink text
  - Spacer: `32px`
  - Links row: GitHub · MIT License
  - Credit: "Fatto con Swift, Python e Claude Code" in `var(--dim)` equivalent on dark
- **No excessive footer** — clean, focused on final conversion

---

## Animations Spec

### Scroll-reveal (Intersection Observer)

```javascript
// Trigger: element enters viewport at 15% threshold
// Animation: opacity 0→1, translateY(20px→0)
// Duration: 0.8s ease-out
// Stagger: 0.15s between siblings with .reveal class
// Once: true (no re-animation on scroll up)
```

### Nav scroll behavior

```javascript
// On scroll > 50px: add .scrolled class
// .scrolled: height reduces, backdrop-filter: blur(12px), border-bottom appears
// Transition: 0.3s ease-out
```

### FAQ accordion

```javascript
// Click on question: toggle .open class on item
// .open: max-height animates from 0 to scrollHeight
// Chevron rotates 180deg
// Only one open at a time (close others on open)
```

### Hover states

- **Cards:** `translateY(-2px)`, shadow deepens
- **CTA primary:** bg transitions to `var(--cta-hover)`, text to `var(--ink)`
- **CTA secondary:** bg transitions to `var(--cream)`
- **Nav links:** color transitions to `var(--gold-dark)`
- **All:** `transition: all 0.3s ease-out`

---

## Responsive Breakpoints

| Breakpoint | Behavior |
|---|---|
| Desktop (>1024px) | Full layout as described above |
| Tablet (768-1024px) | 2-col grids become 2-col, persona cards stack, comparison table scrolls |
| Mobile (<768px) | Single column everything, nav becomes hamburger, hero text smaller (32px), 3D mockup perspective removed |

---

## Technical Constraints

- **Single file:** Everything in `index.html` (CSS in `<style>`, JS in `<script>`)
- **Zero dependencies:** Only Google Fonts (Instrument Serif, DM Sans, JetBrains Mono)
- **No build step:** Direct deploy to any static host
- **Performance target:** <50KB total HTML, instant paint, 60fps animations
- **Browser support:** Modern browsers (Chrome, Safari, Firefox, Edge — latest 2 versions)
- **Accessibility:** Semantic HTML, proper heading hierarchy, prefers-reduced-motion support, keyboard-navigable FAQ

---

## Out of Scope

- Multi-language support (Italian only for now)
- Blog or changelog pages
- Analytics integration
- Contact form
- Dark mode toggle (light only)
- Cookie banner
