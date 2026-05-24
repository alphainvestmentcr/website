---
name: premium-landing-page
description: >
  Build a spectacular single-page website (pure HTML + CSS + GSAP, zero framework) with a dark luxury
  aesthetic: deep black background, gold or brand-color accents, split hero with an atmospheric image
  asset fused via CSS mask, tonal background journey across sections, pill-shaped CTAs, animated
  uppercase section labels, GSAP ScrollTrigger reveals, and a complete 3-role typography system.
  Works for any brand — fintech, real estate, professional services, personal brand, SaaS.
---

## Overview

This skill reproduces the full design system, HTML structure, CSS architecture, and GSAP animation
strategy developed for **Alpha Investment CR** — a wealth management landing page. All brand-specific
content is marked with `[BRAND]` placeholders so you can apply the pattern to any topic.

When invoked, gather the **Brand Intake** below, then produce a single `index.html` file (all styles
and scripts inline) following each phase in order.

---

## Phase 0 — Brand Intake

Ask the user for the following before writing any code. Keep answers as variables you reference
throughout the build:

| Variable | Question | Example |
|---|---|---|
| `[BRAND_NAME]` | What is the brand name? | Alpha Investment |
| `[TAGLINE]` | One-sentence tagline (hero h1). Wrap a key word in `<em>`. | Protegemos y hacemos crecer su *patrimonio*… |
| `[SUBTITLE]` | 2–3 sentence hero paragraph | Asesoría financiera independiente… |
| `[ACCENT_COLOR]` | Primary accent hex (replaces gold) | #C9A84C |
| `[ACCENT_LIGHT]` | Lighter / soft version | #b8973f |
| `[HERO_ASSET]` | Filename of the hero image (RGBA PNG ideal) | bull_transparent.png |
| `[HERO_ASSET_SIDE]` | Which side does the asset anchor to? (`left` / `right`) | right |
| `[SECTIONS]` | Which content sections to include | Metrics, Servicios, Filosofía, Proceso, Contacto |
| `[LOCALE]` | Language / locale for `<html lang="">` | es |
| `[CTA_PRIMARY]` | Primary CTA label | Agendar consulta privada |
| `[CTA_SECONDARY]` | Secondary / ghost CTA label | Conocer servicios |
| `[LOGO_FONT]` | Google Font for brand wordmark (Cinzel, Cormorant…) | Cinzel |
| `[SERIF_FONT]` | Google Font for headings / titulares | Playfair Display |
| `[SANS_FONT]` | Google Font for body / UI / buttons | Inter |

---

## Phase 1 — `<head>` Setup

```html
<!DOCTYPE html>
<html lang="[LOCALE]">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[BRAND_NAME] — [Short descriptor]</title>
  <meta name="theme-color" content="#0A0A0A">

  <!-- Google Fonts — 3-role system -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=[LOGO_FONT]:wght@400;500&family=[SANS_FONT]:wght@300;400;500;600&family=[SERIF_FONT]:ital,wght@0,700;1,700&display=swap" rel="stylesheet">

  <style>
    /* All CSS inline — see Phase 2 */
  </style>
</head>
```

---

## Phase 2 — CSS Design System

### 2.1 CSS Variables (`:root`)

```css
:root {
  /* Backgrounds — tonal journey (barely perceptible, 5-8% luminance steps) */
  --bg:        #0A0A0A;   /* hero, deep black */
  --bg-soft:   #0F0F11;   /* metrics / services transition */
  --bg-mid:    #121214;   /* philosophy / process — peak of the journey */
  --bg-return: #080809;   /* final CTA / footer — back to deep black */

  /* Borders & lines */
  --line:      rgba([ACCENT_RGB], .18);
  --line-soft: rgba(255,255,255,.06);

  /* Accent palette — replace [ACCENT_RGB] with R,G,B of [ACCENT_COLOR] */
  --gold:      [ACCENT_COLOR];
  --gold-soft: [ACCENT_LIGHT];
  --gold-dim:  rgba([ACCENT_RGB], .55);
  --gold-glow: rgba([ACCENT_RGB], .12);

  /* Text */
  --text:      #EDE7D7;   /* warm near-white */
  --text-dim:  #A8A293;
  --text-mute: #6E6A60;

  /* Typography — 3-role system */
  --serif: '[SERIF_FONT]', Georgia, 'Times New Roman', serif;  /* titulares */
  --sans:  '[SANS_FONT]', system-ui, -apple-system, 'Segoe UI', sans-serif; /* UI/body */
  --logo:  '[LOGO_FONT]', Georgia, serif;                      /* wordmark only */

  /* Layout */
  --maxw: 1240px;
  --ease: cubic-bezier(.25,.1,.25,1);
  --ease-out: cubic-bezier(0,0,.2,1);
}
```

### 2.2 Reset & Base

```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  background: var(--bg);
  color: var(--text);
  font-family: var(--sans);
  font-weight: 300;
  line-height: 1.65;
  -webkit-font-smoothing: antialiased;
  overflow-x: hidden;
}
a { color: inherit; text-decoration: none; }

/* Film-grain texture overlay (subtle, ~3% opacity) */
body::after {
  content: '';
  position: fixed; inset: 0;
  pointer-events: none; z-index: 9990;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  opacity: .028;
  mix-blend-mode: overlay;
}
```

### 2.3 Container

```css
.container { width: 100%; max-width: var(--maxw); margin: 0 auto; padding: 0 48px; }
```

### 2.4 Navigation

```css
.nav {
  position: fixed; top: 0; left: 0; right: 0; z-index: 100;
  display: flex; align-items: center; justify-content: space-between;
  padding: 24px 48px;
  background: linear-gradient(to bottom, rgba(10,10,10,.92), transparent);
  backdrop-filter: blur(8px);
}
/* Brand wordmark — uses --logo font */
.brand { display: flex; align-items: center; gap: 12px; font-family: var(--logo); font-size: 20px; font-weight: 500; letter-spacing: .25em; color: var(--text); }
.brand-mark { height: 44px; width: auto; }              /* <img> logo asset */
.brand-words > span { color: var(--gold); font-weight: 500; }  /* accent word in wordmark */

/* Nav links */
.nav-links { display: flex; align-items: center; gap: 36px; list-style: none; }
.nav-links a { font-size: 11px; letter-spacing: .14em; text-transform: uppercase; color: var(--text-dim); position: relative; }
.nav-links a::after { content: ''; position: absolute; left: 0; right: 0; bottom: -2px; height: 1px; background: var(--gold); transform: scaleX(0); transform-origin: left; transition: transform .35s var(--ease-out); }
.nav-links a:hover::after { transform: scaleX(1); }
.nav-links a:hover { color: var(--text); }

/* CTA pill in nav */
.nav-cta { font-size: 11px; font-weight: 500; letter-spacing: .16em; text-transform: uppercase; color: var(--bg); background: var(--gold); padding: 10px 28px; border: 1px solid var(--gold); border-radius: 999px; transition: background .25s, color .25s, box-shadow .25s; }
.nav-cta:hover { background: transparent; color: var(--gold); box-shadow: 0 0 24px var(--gold-glow); }
```

### 2.5 Buttons (pill-shaped)

```css
.btn { display: inline-flex; align-items: center; gap: 10px; font-family: var(--sans); font-size: 10px; font-weight: 600; letter-spacing: .18em; text-transform: uppercase; white-space: nowrap; padding: 14px 48px; border: 1px solid; border-radius: 999px; transition: all .3s var(--ease); }
.btn-primary { color: var(--bg); background: var(--gold); border-color: var(--gold); }
.btn-primary:hover { background: transparent; color: var(--gold); border-color: var(--gold); }
.btn-ghost { color: var(--text-dim); background: transparent; border-color: var(--line); }
.btn-ghost:hover { color: var(--gold); border-color: var(--gold-dim); }
.cta-row { display: flex; gap: 16px; flex-wrap: wrap; align-items: center; }
```

### 2.6 Section Tags (uppercase labels)

```css
/* Elegant uppercase section labels with small geometric prefix */
.section-tag {
  font-size: 12px; letter-spacing: .28em; text-transform: uppercase;
  color: var(--gold); margin-bottom: 16px;
  display: flex; align-items: center; gap: 10px;
}
.section-tag::before { content: '◆'; font-size: 4px; color: var(--gold); flex-shrink: 0; line-height: 1; }
.section-tag--plain::before { content: none; }  /* use for centered/CTA contexts */

.section-title { font-family: var(--serif); font-size: clamp(28px,3.5vw,44px); font-weight: 700; line-height: 1.2; letter-spacing: -.01em; }
.section-title em { font-style: italic; color: var(--gold); }

.section-head { display: grid; grid-template-columns: 1fr 1fr; gap: 48px; align-items: end; margin-bottom: 72px; }
```

### 2.7 Hero — Split Layout + Atmospheric Asset

```css
.hero {
  position: relative; min-height: 100vh;
  display: flex; align-items: center; padding: 140px 0 120px; overflow: hidden;
  background: var(--bg);
}

/* Radial depth glow behind the asset */
.hero::before {
  content: ''; position: absolute; inset: 0; pointer-events: none; z-index: 0;
  background:
    radial-gradient(ellipse 55% 80% at 88% 55%, rgba([ACCENT_RGB],.09), transparent 65%),
    radial-gradient(ellipse 80% 80% at 50% 50%, transparent 35%, rgba(0,0,0,.45) 100%);
}
/* Fine gold grid texture */
.hero::after {
  content: ''; position: absolute; inset: 0; pointer-events: none; z-index: 0;
  background-image: linear-gradient(rgba([ACCENT_RGB],.04) 1px, transparent 1px),
                    linear-gradient(90deg, rgba([ACCENT_RGB],.04) 1px, transparent 1px);
  background-size: 64px 64px;
}

/* Grid: 55% text / 45% asset */
.hero > .container { width: 100%; display: grid; grid-template-columns: 55fr 45fr; position: relative; z-index: 1; }
.hero-inner { position: relative; z-index: 2; grid-column: 1; padding-right: 56px; text-align: left; }

/* Atmospheric asset — fused via CSS mask */
.hero-asset {
  position: absolute;
  [HERO_ASSET_SIDE]: 8%;    /* e.g. right: 8% */
  bottom: 0; height: 92%; width: auto;
  object-fit: contain; pointer-events: none; z-index: 1; opacity: 0;
  /* Mask fades the edge closest to the text */
  -webkit-mask-image: linear-gradient(to left, #000 52%, transparent 100%);
          mask-image: linear-gradient(to left, #000 52%, transparent 100%);
  filter: drop-shadow(0 0 60px rgba([ACCENT_RGB], .20));
}
/* Breathing glow — only starts after GSAP entrance (add class .asset-live via JS) */
.hero-asset.asset-live { animation: asset-breathe 6s ease-in-out infinite; }
@keyframes asset-breathe {
  0%,100% { filter: drop-shadow(0 0 60px rgba([ACCENT_RGB],.20)); }
  50%      { filter: drop-shadow(0 0 110px rgba([ACCENT_RGB],.44)); }
}

/* Hero text */
.eyebrow { font-size: 12px; letter-spacing: .28em; text-transform: uppercase; color: var(--gold); margin-bottom: 28px; display: flex; align-items: center; gap: 14px; }
.eyebrow::before { content: '◆'; font-size: 4px; color: var(--gold); flex-shrink: 0; }
.hero h1 { font-family: var(--serif); font-size: clamp(40px,5.5vw,70px); font-weight: 700; line-height: 1.1; letter-spacing: -.01em; margin-bottom: 28px; }
.hero h1 em { font-style: italic; color: var(--gold); }
.hero p  { font-size: 18px; color: var(--text-dim); max-width: 100%; line-height: 1.8; margin-bottom: 44px; }
```

### 2.8 Tonal Background Journey

Apply to sections in this order to create a near-invisible depth arc as user scrolls:

```css
.hero                            { background: #0A0A0A; }
.metrics, #services              { background: linear-gradient(180deg, #0A0A0A, #0F0F11); }
.philosophy, #process            { background: #121214; }           /* peak luminance */
.testimonial-section             { background: linear-gradient(180deg, #121214, #0A0A0A); }
.final-cta, footer               { background: #080809; }           /* return to deep black */
```

### 2.9 Fixed Left Accent Line + Scroll Progress

```css
.hero-line { position: fixed; left: 40px; top: 0; width: 1px; height: 100%; background: linear-gradient(to bottom, transparent 0%, var(--gold-dim) 15%, var(--gold-dim) 85%, transparent 100%); opacity: .35; z-index: 1; pointer-events: none; }
.scroll-dot { position: fixed; left: 39px; top: 15%; width: 3px; height: 50px; z-index: 2; pointer-events: none; background: linear-gradient(to bottom, transparent, var(--gold) 40%, var(--gold) 60%, transparent); box-shadow: 0 0 12px var(--gold-dim); opacity: 0; border-radius: 2px; }
#scroll-progress { position: fixed; top: 0; left: 0; height: 2px; background: linear-gradient(90deg, var(--gold-soft), var(--gold)); z-index: 1001; width: 0; box-shadow: 0 0 10px var(--gold-dim); }
```

---

## Phase 3 — HTML Structure

```html
<body>
  <!-- Scroll progress bar -->
  <div id="scroll-progress"></div>
  <div class="hero-line" aria-hidden="true"></div>
  <div class="scroll-dot" id="scrollDot" aria-hidden="true"></div>

  <!-- NAV -->
  <nav class="nav" id="nav">
    <a href="#top" class="brand" aria-label="[BRAND_NAME]">
      <img class="brand-mark" src="[LOGO_FILE]" alt="[BRAND_NAME]" loading="eager">
      <span class="brand-words">[BRAND_WORD1] <span>[BRAND_WORD2]</span></span>
    </a>
    <ul class="nav-links">
      <li><a href="#servicios">Servicios</a></li>
      <li><a href="#proceso">Proceso</a></li>
      <li><a href="#contacto">Contacto</a></li>
    </ul>
    <a href="#contacto" class="nav-cta">[CTA_PRIMARY_SHORT]</a>
  </nav>

  <!-- HERO -->
  <section class="hero" id="top">
    <div class="container">
      <div class="hero-inner">
        <div class="eyebrow">[BRAND_DESCRIPTOR] · [LOCATION]</div>
        <h1>[TAGLINE with <em>key word</em>]</h1>
        <p>[SUBTITLE]</p>
        <div class="cta-row">
          <a href="#contacto" class="btn btn-primary">[CTA_PRIMARY]</a>
          <a href="#servicios" class="btn btn-ghost">[CTA_SECONDARY]</a>
        </div>
      </div>
    </div>
    <!-- Atmospheric asset — decorative, aria-hidden -->
    <img src="[HERO_ASSET]" alt="" aria-hidden="true" class="hero-asset" id="heroAsset"
         loading="eager" width="756" height="1024">
  </section>

  <!-- METRICS STRIP (optional) -->
  <section class="metrics">
    <div class="container">
      <div class="metrics-grid">
        <!-- Repeat: -->
        <div class="metric reveal">
          <div class="metric-value"><span class="count" data-target="[N]">0</span><sup>[UNIT]</sup></div>
          <div class="metric-label">[LABEL]</div>
        </div>
      </div>
    </div>
  </section>

  <!-- CONTENT SECTIONS (repeat pattern) -->
  <section id="[section-id]" class="section [bg-class]">
    <div class="container">
      <div class="section-head">
        <div class="left reveal">
          <div class="section-tag">[SECTION LABEL]</div>
          <h2 class="section-title">[Heading with <em>accent word</em>].</h2>
        </div>
        <div class="right reveal delay-1">
          <p>[Section description paragraph]</p>
        </div>
      </div>
      <!-- Section-specific grid content -->
    </div>
  </section>

  <!-- FINAL CTA -->
  <section class="final-cta" id="contacto">
    <div class="container final-cta-inner">
      <div class="section-tag section-tag--plain reveal" style="justify-content:center">[CTA_SECTION_LABEL]</div>
      <h2 class="reveal delay-1">[CTA Heading with <em>accent</em>].</h2>
      <p class="reveal delay-2">[CTA subtext]</p>
      <div class="cta-row reveal delay-3" style="justify-content:center">
        <a href="mailto:[EMAIL]" class="btn btn-primary">[CTA_PRIMARY]</a>
        <a href="tel:[PHONE]" class="btn btn-ghost">[CTA_SECONDARY_ALT]</a>
      </div>
    </div>
  </section>

  <!-- FOOTER -->
  <footer>...</footer>
</body>
```

---

## Phase 4 — GSAP Animation Strategy

Load GSAP + ScrollTrigger from CDN **after** closing `</body>`:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
```

### 4.1 Hero entrance timeline (staggered, ~900ms)

```js
const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
gsap.registerPlugin(ScrollTrigger);

if (!prefersReduced) {
  const tl = gsap.timeline({ defaults: { ease: 'power3.out' } });
  gsap.set(['.eyebrow','.hero h1','.hero-inner > p','.cta-row'], { opacity: 0, y: 40 });
  gsap.set('#heroAsset', { opacity: 0, x: 40 });

  tl.to('.eyebrow',        { opacity: 1, y: 0, duration: 0.9 }, 0.4)
    .to('.hero h1',        { opacity: 1, y: 0, duration: 1.0 }, 0.6)
    .to('.hero-inner > p', { opacity: 1, y: 0, duration: 0.9 }, 0.85)
    .to('.cta-row',        { opacity: 1, y: 0, duration: 0.8 }, 1.0)
    .to('#heroAsset',      { opacity: 0.96, x: 0, duration: 1.1, ease: 'power2.out',
                             onComplete: () => document.getElementById('heroAsset').classList.add('asset-live') }, 0.5)
    .to('#scrollDot',      { opacity: 1, duration: 0.6 }, 1.5);
}
```

### 4.2 Section reveals (ScrollTrigger)

```js
// Generic fade-up for .reveal elements
gsap.utils.toArray('.reveal').forEach(el => {
  gsap.from(el, {
    opacity: 0, y: 48,
    duration: 0.9, ease: 'power3.out',
    scrollTrigger: { trigger: el, start: 'top 82%', once: true }
  });
});

// Stagger children inside .services-grid, etc.
gsap.utils.toArray('.services-grid, .philosophy-grid, .process-grid').forEach(grid => {
  gsap.from(grid.children, {
    opacity: 0, y: 40,
    duration: 0.7, ease: 'power2.out', stagger: 0.12,
    scrollTrigger: { trigger: grid, start: 'top 80%', once: true }
  });
});
```

### 4.3 Metric counter-up

```js
document.querySelectorAll('.count').forEach(el => {
  const target = +el.dataset.target;
  ScrollTrigger.create({
    trigger: el, start: 'top 85%', once: true,
    onEnter: () => gsap.to({ val: 0 }, {
      val: target, duration: 2, ease: 'power2.out',
      onUpdate: function() { el.textContent = Math.round(this.targets()[0].val); }
    })
  });
});
```

### 4.4 Parallax on hero asset (scroll)

```js
if (!prefersReduced) {
  gsap.to('#heroAsset', {
    y: '-12%',
    ease: 'none',
    scrollTrigger: { trigger: '.hero', start: 'top top', end: 'bottom top', scrub: true }
  });
}
```

### 4.5 Scroll progress bar + dot

```js
ScrollTrigger.create({
  trigger: 'body', start: 'top top', end: 'bottom bottom', scrub: true,
  onUpdate: self => {
    document.getElementById('scroll-progress').style.width = (self.progress * 100) + '%';
    document.getElementById('scrollDot').style.top = (15 + self.progress * 70) + '%';
  }
});
```

---

## Phase 5 — Responsive (≤768px)

```css
@media (max-width: 768px) {
  .hero > .container { grid-template-columns: 1fr; }
  .hero-inner { padding-right: 0; }
  /* Asset becomes a faint background — never competes with text */
  .hero-asset { opacity: .18 !important; right: -15%; height: 70%; }
  .section-head { grid-template-columns: 1fr; }
  .hero-line, .scroll-dot { display: none; }
  .nav-links, .nav-cta { display: none; }
}
@media (max-width: 480px) {
  .container { padding: 0 24px; }
  .cta-row { flex-direction: column; align-items: stretch; }
  .btn { justify-content: center; }
}
```

---

## Phase 6 — Accessibility & Performance

- Hero asset: `alt=""` + `aria-hidden="true"` (decorative).
- All interactive elements have `:focus-visible` styles.
- `prefers-reduced-motion`: gate ALL GSAP calls behind the `prefersReduced` check. Disable parallax and breathing animation.
- `loading="eager"` on hero asset only; all other images use `loading="lazy"`.
- Provide explicit `width` + `height` on the hero asset to prevent layout shift.
- `<meta name="theme-color">` matches `--bg` for seamless overscroll on mobile.

---

## Design Principles (summary)

| Principle | Implementation |
|---|---|
| **Dark luxury** | Deep neutral blacks (#0A0A0A), never warm-tinted backgrounds |
| **Single accent** | One color (gold or brand accent) — used only for accents, never backgrounds |
| **Tonal journey** | 5-8% luminance steps across sections — depth without drama |
| **Typography hierarchy** | 3 fonts: `--logo` (wordmark), `--serif` (titulares, 700), `--sans` (UI, 300-600) |
| **Pill CTAs** | `border-radius: 999px` — elongated via large horizontal padding |
| **Atmospheric fusion** | Asset positioned `absolute` with CSS `mask-image` gradient — no hard edges |
| **Grain texture** | `body::after` with SVG fractalNoise at ~3% opacity — luxury materiality |
| **Left accent line** | Fixed, full-height — editorial anchor for the scroll journey |
| **No magnetic buttons** | Hover = color change only. Movement = reserved for the asset entrance |
