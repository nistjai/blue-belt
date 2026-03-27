# Dharti Solar — Website Structure & File Reference

> **Tech Stack**: Astro 6.1.1 · Tailwind CSS 4.2.2 · TypeScript · Vanilla JS  
> **Build**: `npm run dev` (dev server) · `npm run build` (static output → `dist/`)  
> **Terminal Note** (Windows): Use `cmd /c "cd /d <path> && <command>"` if PowerShell execution policy blocks scripts.

---

## Directory Tree

```
blue-belt/
│
├── astro.config.mjs          # Astro + Tailwind Vite plugin config
├── package.json               # Dependencies & npm scripts
├── tsconfig.json              # TypeScript strict mode
├── WEBSITE_STRUCTURE.md       # ← You are here
├── COMPONENT_STRUCTURE.md     # Earlier component notes
├── PROJECT_LOG.md             # Development changelog
├── README.md                  # Project readme
│
├── public/                    # Static assets (copied as-is to dist/)
│   ├── favicon.ico
│   └── favicon.svg
│
├── src/
│   ├── pages/
│   │   └── index.astro        # Single-page entry — composes ALL sections
│   │
│   ├── components/            # One .astro file per section/widget
│   │   ├── Navbar.astro
│   │   ├── Hero.astro
│   │   ├── Brands.astro
│   │   ├── Services.astro
│   │   ├── WhyChooseUs.astro
│   │   ├── Subsidy.astro
│   │   ├── Calculator.astro
│   │   ├── Projects.astro
│   │   ├── Testimonials.astro
│   │   ├── CTA.astro
│   │   ├── Contact.astro
│   │   ├── Footer.astro
│   │   └── WhatsAppButton.astro
│   │
│   ├── images/                # Local images (processed by Astro pipeline)
│   │   ├── dhartisolarlogo.png
│   │   └── udaipur-solar.svg
│   │
│   └── styles/
│       ├── global.css         # Theme system + animations + utilities
│       └── utilities.css      # (Reserved — currently empty)
│
└── dist/                      # Build output (static HTML/CSS/JS)
```

---

## Page Composition Order

`src/pages/index.astro` renders every section top-to-bottom:

| #  | Component           | Section ID      | Scroll-linked from Navbar? |
|----|---------------------|-----------------|---------------------------|
| —  | `<canvas>`          | `particle-canvas` | No (background layer)   |
| —  | `Navbar`            | —               | Fixed on top              |
| 1  | `Hero`              | —               | No (landing view)         |
| 2  | `Brands`            | `#brands`       | Yes                       |
| 3  | `Services`          | `#services`     | Yes                       |
| 4  | `WhyChooseUs`       | `#why-us`       | Yes                       |
| 5  | `Subsidy`           | `#subsidy`      | Yes                       |
| 6  | `Calculator`        | `#calculator`   | Yes                       |
| 7  | `Projects`          | `#projects`     | Yes                       |
| 8  | `Testimonials`      | `#testimonials` | No                        |
| 9  | `CTA`               | —               | No                        |
| 10 | `Contact`           | `#contact`      | Yes                       |
| —  | `Footer`            | —               | No                        |
| —  | `WhatsAppButton`    | —               | Floating (fixed)          |

---

## File-by-File Details

### Config & Root Files

| File | Purpose | When to touch |
|------|---------|---------------|
| `astro.config.mjs` | Astro framework setup, Tailwind Vite plugin registration | Adding integrations, changing build output, adding Vite plugins |
| `package.json` | npm scripts (`dev`, `build`, `preview`), dependencies | Adding packages, changing scripts, version bumps |
| `tsconfig.json` | TypeScript config (extends `astro/tsconfigs/strict`) | Changing TS strictness, adding path aliases |

---

### `src/pages/index.astro` — Main Entry Page

**What it does:**
- Imports `global.css` (theme system)
- Imports & composes all 13 component files
- Contains `<head>` with SEO meta, Google Fonts (Inter), favicon
- Contains `<script>` block with 4 systems:
  1. **Theme IIFE** — reads `localStorage('theme')`, applies `.light` class
  2. **Scroll Reveal** — `IntersectionObserver` adds `.visible` to `.animate-on-scroll` elements
  3. **Counter Animation** — animates `[data-count]` elements (used in Hero stats)
  4. **Dot Grid Canvas** — WQF-style interactive particle system with spring physics

**Touch this file when:**
- Adding/removing/reordering sections
- Changing page `<title>`, meta description, or SEO tags
- Modifying scroll-reveal behavior or counter animation
- Tweaking particle canvas physics (spacing, repel radius, friction, etc.)
- Adding new fonts or external `<link>` tags

---

### `src/styles/global.css` — Theme & Animation System

**Sections inside (~385 lines):**

| Section | Lines (approx) | What it controls |
|---------|-----------------|------------------|
| Tailwind import | 1 | `@import "tailwindcss"` |
| Dark mode variables (`:root`) | 8–36 | 26 CSS custom properties — colors, shadows, opacities |
| Light mode variables (`:root.light`) | 39–68 | Same 26 properties with light values |
| Base resets | 71–80 | Font smoothing, smooth scroll, body transitions |
| Film grain overlay | 82–93 | `body::before` — subtle noise texture |
| Theme utility classes (`.t-*`) | 96–107 | Shorthand classes: `.t-bg`, `.t-text`, `.t-accent`, etc. |
| Scroll reveal | 110–128 | `.animate-on-scroll` + `.animate-delay-*` classes |
| @keyframes | 131–152 | `float`, `pulse-glow`, `rotate-slow`, `dash-flow`, `shimmer` |
| Particle canvas | 155–168 | `.particle-canvas` fixed positioning + blend modes |
| Ambient section glows | 171–183 | `section::before` gradient glow lines |
| Range slider | 186–222 | Custom thumb/track for `<input type="range">` |
| Scrollbar | 225–229 | Webkit custom scrollbar |
| Selection | 232–235 | `::selection` highlight color |
| Theme toggle | 238–280 | `.theme-toggle` pill button with animated dot |

**Touch this file when:**
- Changing brand colors (accent green, backgrounds, text colors)
- Adding new animations or transitions
- Modifying the dark/light theme values
- Changing the particle canvas blend mode or opacity
- Styling new form elements or custom scrollbars

---

### Component Files

#### `Navbar.astro` — Fixed Navigation Bar

| Detail | Value |
|--------|-------|
| Frontmatter import | `dhartisolarlogo.png` |
| Has `<script>` | Yes — mobile menu toggle + link auto-close |
| Section ID | None (fixed position, not a scroll section) |

**Features:** Glass-morphism backdrop blur, logo, desktop nav links (Services → Why Us → Subsidy → Calculator → Brands → Projects → Contact), mobile hamburger menu, theme toggle button, "Get Quote" CTA.

**Touch when:** Adding/removing nav links, changing logo, modifying mobile menu behavior, changing CTA button text.

---

#### `Hero.astro` — Full-Screen Landing Section

| Detail | Value |
|--------|-------|
| Frontmatter import | `dhartisolarlogo.png` (as hero logo, hidden on mobile) |
| Has `<script>` | No |
| Section ID | None |

**Features:** Ambient gradient orbs (decorative blurs), animated SVG solar panel illustration (desktop-only right side), tagline ("POWERING TOMORROW, TODAY"), giant headline, subtext, two CTA buttons (Get Free Quote + Calculate Savings), gradient divider, stats row with 4 animated counters (`data-count`), scroll indicator arrow.

**Touch when:** Changing headline, tagline, or CTA text; modifying stats numbers (projects, capacity, states, savings); editing the SVG panel graphic; adjusting hero layout or animations.

---

#### `Brands.astro` — Trusted Brand Partners

| Detail | Value |
|--------|-------|
| Frontmatter import | None |
| Has `<script>` | No |
| Has `<style>` | Yes — `@keyframes brand-scroll` marquee animation |
| Section ID | `#brands` |

**Features:** Section header "TRUSTED BY INDUSTRY LEADERS", 3-column gap-px grid:
- **Solar Panels**: Adani, Tata Power Solar, Waaree, Vikram Solar, Loom Solar, Luminous, Goldi Solar, RenewSys
- **Inverters**: Growatt, Havells, Fronius, Goodwe, Solis, Polycab, Microtek
- **Batteries & Mounting**: Luminous, Exide, Okaya, Livguard, Polycab Cables, K2 Mounting

Auto-scrolling brand marquee at the bottom (30s loop, pauses on hover, fade edges).

**Touch when:** Adding/removing brands, changing brand categories, adjusting marquee speed.

---

#### `Services.astro` — Core Service Offerings

| Detail | Value |
|--------|-------|
| Section ID | `#services` |

**Features:** 4-column gap-px grid with numbered cards (01–04): Residential Solar, Commercial & Industrial, Solar Farms, Maintenance & AMC. Each card has SVG icon, title, description, hover-reveal green underline bar.

**Touch when:** Changing service descriptions, adding new services, editing SVG icons.

---

#### `WhyChooseUs.astro` — Competitive Advantages

| Detail | Value |
|--------|-------|
| Section ID | `#why-us` |

**Features:** "WHY DHARTI SOLAR" header, 6-item 3-column gap-px grid numbered 01–06: Multi-Brand Panels, Subsidy Assistance, Serving 3+ States, End-to-End Solution, Fast Installation, Warranty & Support.

**Touch when:** Adding/changing advantage points, updating state count or warranty details.

---

#### `Subsidy.astro` — PM Surya Ghar Yojana Section

| Detail | Value |
|--------|-------|
| Section ID | `#subsidy` |

**Features:** Decorative rotating SVG sun graphic, "GOVERNMENT SUBSIDY" header, 3 subsidy tier cards:
- Up to 2 kW → ₹30,000/kW
- 2–3 kW → ₹18,000/kW
- Above 3 kW → ₹78,000 total cap

"Apply With Our Help" CTA button.

**Touch when:** Subsidy amounts change (government policy updates), changing CTA text.

---

#### `Calculator.astro` — Solar Savings Calculator

| Detail | Value |
|--------|-------|
| Frontmatter import | None |
| Has `<script>` | Yes — `initCalculator()` function with real-time calculation |
| Section ID | `#calculator` |

**Features:** 2-column layout (sliders left, results right). Three range inputs:
- Monthly electricity bill (₹500–₹20,000)
- Available roof area (100–2,000 sq ft)
- Average sunlight hours/day (3–8 hrs)

Calculated outputs: Recommended kW, estimated system cost, government subsidy, monthly savings, payback period. Animated SVG gauge circle shows savings percentage.

**Touch when:** Changing calculation formulas (cost per kW, subsidy logic), slider ranges, result formatting, gauge animation.

---

#### `Projects.astro` — Recent Installations Portfolio

| Detail | Value |
|--------|-------|
| Frontmatter import | `udaipur-solar.svg` (local image) |
| Section ID | `#projects` |

**Features:** "RECENT INSTALLATIONS" header + stats ("150+ Installations Across 3 States"), 6-project responsive grid:

| # | Location | Type | Capacity | Image Source |
|---|----------|------|----------|-------------|
| 1 | Ahmedabad | Residential rooftop | 5 kW | Unsplash URL |
| 2 | Jaipur | Commercial rooftop | 50 kW | Unsplash URL |
| 3 | Indore | Solar farm | 500 kW | Unsplash URL |
| 4 | Surat | Industrial rooftop | 100 kW | Unsplash URL |
| 5 | Udaipur | Residential rooftop | 3 kW | **Local SVG** (`udaipur-solar.svg`) |
| 6 | Bhopal | Solar farm | 1 MW | Unsplash URL |

**Touch when:** Adding new projects, replacing project images with local files, changing project details.

---

#### `Testimonials.astro` — Customer Reviews

| Detail | Value |
|--------|-------|
| Section ID | `#testimonials` |

**Features:** 3-column gap-px grid, each card has large quotation mark, review text, customer name, and location:
- Rajesh Patel — Ahmedabad
- Amit Sharma — Jaipur
- Priya Mehta — Indore

**Touch when:** Adding reviews, creating a carousel, connecting to a reviews API.

---

#### `CTA.astro` — Call-to-Action Banner

| Detail | Value |
|--------|-------|
| Section ID | None |

**Features:** "Own What's Next." gradient headline, decorative animated SVG energy flow lines, "Book Free Site Visit" button linking to `#contact`, phone number link (+91 73001 32924).

**Touch when:** Changing CTA copy, phone number, or button destination.

---

#### `Contact.astro` — Contact Form & Details

| Detail | Value |
|--------|-------|
| Has `<script>` | Yes — sets redirect URL for FormSubmit |
| Section ID | `#contact` |
| External services | **FormSubmit** (support@dhartisolar.com), **Google Maps** embed |

**Features:** 2-column layout:
- **Left**: Lead capture form → 5 fields (Name, Phone, Email, City, Message) + FormSubmit hidden fields (_subject, _captcha, _template, _next redirect, _honey honeypot)
- **Right**: Contact info blocks — Phone, Email, Address (Office 104/1 Floor, Patidar Complex, Station Road, Sagwara, Rajasthan 314025), Working Hours (Mon–Sat 9AM–7PM), Instagram (@dharti_solar)
- **Bottom**: Google Maps iframe with `var(--map-filter)` for theme-aware styling

**Touch when:** Changing contact info, email, address, phone number, form fields, map location, or FormSubmit settings.

---

#### `Footer.astro` — Site Footer

| Detail | Value |
|--------|-------|
| Frontmatter import | `dhartisolarlogo.png` |

**Features:** 4-column grid:
1. Logo + tagline + social links (Instagram, WhatsApp)
2. Quick Links (Services, Calculator, Projects, Contact)
3. Service Areas (Rajasthan, Gujarat, Madhya Pradesh + city list)
4. Contact info (Phone, Email, Address)

Bottom bar: Copyright © 2026 + Google Maps pin link (Office 104/1 Floor, Patidar Complex, Sagwara).

**Touch when:** Updating copyright year, adding social links, changing service areas, editing footer links.

---

#### `WhatsAppButton.astro` — Floating Chat Button

| Detail | Value |
|--------|-------|
| External service | WhatsApp (`wa.me/917300132924`) |

**Features:** Fixed bottom-right green button with WhatsApp SVG icon + ping pulse animation. Theme-aware colors via CSS variables.

**Touch when:** Changing phone number, adjusting position, adding pre-filled message text.

---

### Image Assets (`src/images/`)

| File | Used By | Purpose |
|------|---------|---------|
| `dhartisolarlogo.png` | Navbar, Hero, Footer | Company logo |
| `udaipur-solar.svg` | Projects | Illustrated SVG for Udaipur project card (building, rooftop panels, Aravalli hills, sun) |

---

## Theme System Quick Reference

Theme toggling works by adding/removing `.light` class on `<html>`. Preference is saved in `localStorage('theme')`.

| Variable | Dark Value | Light Value | Used For |
|----------|-----------|-------------|----------|
| `--bg-primary` | `#06080f` | `#ffffff` | Page backgrounds |
| `--bg-secondary` | `#0c1019` | `#f8fafc` | Alternate section backgrounds |
| `--bg-card` | `#0a0e17` | `#ffffff` | Card backgrounds |
| `--bg-nav` | `rgba(6,8,15,0.7)` | `rgba(255,255,255,0.8)` | Navbar glass background |
| `--border` | `rgba(255,255,255,0.06)` | `rgba(0,0,0,0.07)` | All borders |
| `--text-primary` | `#f1f5f9` | `#0f172a` | Main text |
| `--text-secondary` | `#94a3b8` | `#475569` | Subordinate text |
| `--text-muted` | `#4b5563` | `#94a3b8` | Faint labels/numbers |
| `--accent` | `#22c55e` | `#16a34a` | Brand green (buttons, highlights) |
| `--accent-text` | `#020617` | `#ffffff` | Text on accent backgrounds |
| `--particle-color` | `34, 197, 94` | `22, 163, 74` | Dot grid RGB values |
| `--map-filter` | `invert(0.9) hue-rotate(180deg)…` | `none` | Google Maps dark mode filter |

---

## External Services & Integrations

| Service | Where Used | Config Location |
|---------|-----------|-----------------|
| **FormSubmit** | Contact form | `Contact.astro` — action URL, hidden fields |
| **Google Maps** | Contact section + Footer | `Contact.astro` — iframe src, `Footer.astro` — link |
| **Google Fonts** | Page head | `index.astro` — `<link>` tag (Inter font) |
| **WhatsApp** | WhatsAppButton + Footer | `WhatsAppButton.astro`, `Footer.astro` — `wa.me/917300132924` |
| **Instagram** | Contact + Footer | `Contact.astro`, `Footer.astro` — `@dharti_solar` link |
| **Unsplash** | Projects | `Projects.astro` — 5 project card images (external URLs) |

---

## Common Tasks — Which Files to Edit

| Task | Files to Touch |
|------|----------------|
| **Change brand colors** | `global.css` (CSS variables in `:root` and `:root.light`) |
| **Add a new section** | Create `src/components/NewSection.astro` → import & place in `index.astro` → optionally add nav link in `Navbar.astro` |
| **Update contact info** | `Contact.astro`, `Footer.astro`, `CTA.astro` (phone), `WhatsAppButton.astro` (phone) |
| **Change logo** | Replace `src/images/dhartisolarlogo.png` (used by Navbar, Hero, Footer) |
| **Add a new project** | `Projects.astro` — add a card to the grid; optionally add image to `src/images/` |
| **Update subsidy amounts** | `Subsidy.astro` (slab cards), `Calculator.astro` (subsidy formula in `<script>`) |
| **Add a new brand** | `Brands.astro` — add to the appropriate category array + marquee text |
| **Change calculator logic** | `Calculator.astro` — `initCalculator()` function in `<script>` |
| **Add a new nav link** | `Navbar.astro` — desktop `<div>` + mobile `#mobile-menu` `<div>` |
| **Tweak particle grid** | `index.astro` — canvas script variables (spacing, repelRadius, etc.) |
| **Change fonts** | `index.astro` — `<head>` Google Fonts link + `<body>` font-family |
| **Add new animation** | `global.css` — add `@keyframes` + utility class |
| **Update SEO meta** | `index.astro` — `<head>` section (title, description, OG tags) |
| **Change FormSubmit settings** | `Contact.astro` — form action URL + hidden input fields |
| **Add a testimonial** | `Testimonials.astro` — add a card div to the grid |
| **Update service areas** | `Footer.astro` (city list), `WhyChooseUs.astro` (state count) |
| **Replace Unsplash images** | `Projects.astro` — swap URL for local file, import in frontmatter |
| **Change copyright year** | `Footer.astro` — bottom bar text |
