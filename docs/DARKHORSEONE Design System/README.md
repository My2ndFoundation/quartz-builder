# DARKHORSEONE Design System

## Overview

DARKHORSEONE LIMITED is a business consultancy that "makes your business an unexpected winner." This design system captures the brand's visual identity with a warm, professional aesthetic featuring deep greens and cream tones.

## Sources

This design system was created from the following materials:
- **Logo files**: PNG padded logo, SVG versions (Gaoel + Esportiva fonts)
- **Brand fonts**: Gaoel, Esportiva, Helvetica LT Std Roman
- **Getting Started Guide**: HTML file containing font and color specifications
- **Brand tagline**: "makes your business an unexpected winner"

## Brand Context

DARKHORSEONE positions itself as a transformative business partner — the "dark horse" that helps businesses become unexpected winners. The brand emphasizes:
- **Professionalism**: Serious business expertise with warm, approachable design
- **Reliability**: Trustworthy partner with strong technical foundation
- **Surprise factor**: The "unexpected winner" concept suggests innovative, game-changing approaches

---

## 字体系统 (Typography System)

### Font Families

```css
/* Logo - Brand Name Only */
font-family: "Gaoel", sans-serif;

/* Headings, Hero/Display - Brand Elements */
font-family: "Syncopate", "Inter", sans-serif;

/* Body - Main Text */
font-family: "Inter", system-ui, sans-serif;

/* Labels - Metadata / Technical */
font-family: "JetBrains Mono", monospace;
```

**Google Fonts Import:**
```html
<link href="https://fonts.googleapis.com/css2?family=Syncopate:wght@400;700&family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

### Font Usage Guidelines

#### 1. Gaoel (Logo Only)
- **Usage:** Company name "DARKHORSEONE" only
- **Style:** ALL CAPS, geometric, athletic
- **Load:** `@font-face` from `fonts/gaoel.ttf`

#### 2. Syncopate (Headings & Display)
- **Usage:** All headings, hero/display text, section titles, emphasis elements
- **Characteristics:** ALL CAPS, wide letter-spacing, strong geometric feel
- **Weights:** 400 (regular), 700 (bold)
- **Example:**
  ```css
  h1.title {
    font-family: "Syncopate", "Inter", sans-serif;
    font-weight: 700;
    font-size: 46px;
    line-height: 1.08;
    letter-spacing: -.005em;
    text-transform: uppercase;
  }
  ```

#### 3. Inter (Body Text)
- **Usage:** Body text, descriptions, lists, regular content
- **Characteristics:** Modern, clear, highly readable
- **Weights:** 300, 400, 500, 600, 700
- **Example:**
  ```css
  body {
    font-family: "Inter", system-ui, sans-serif;
  }
  
  .lede {
    font-size: 18px;
    line-height: 1.55;
    font-weight: 400;
  }
  ```

#### 4. JetBrains Mono (Technical Labels)
- **Usage:** Labels, metadata, technical info, small annotations
- **Characteristics:** Monospace, technical feel, works best in ALL CAPS
- **Weights:** 400, 500
- **Always use:** Wide letter-spacing + ALL CAPS
- **Example:**
  ```css
  .eyebrow {
    font-family: "JetBrains Mono", monospace;
    font-size: 11.5px;
    letter-spacing: .22em;
    text-transform: uppercase;
    font-weight: 500;
  }
  ```

### Typography Scale

| Purpose | Size | Line Height | Weight | Font | Letter Spacing |
|---------|------|-------------|--------|------|----------------|
| Main Title H1 | 46px | 1.08 | 700 | Syncopate | -.005em |
| Subtitle H2 | 20-26px | 1.2 | 700 | Syncopate | .01-.02em |
| Card Title H3 | 16px | 1.25 | 700 | Syncopate | .01em |
| Lead Body | 18px | 1.55 | 400 | Inter | normal |
| Body Medium | 15-16px | 1.5-1.55 | 400-600 | Inter | normal |
| Body Small | 13-14.5px | 1.45-1.6 | 400-600 | Inter | normal |
| Labels/Metadata | 10.5-11.5px | 1.5-1.7 | 500 | JetBrains Mono | .14-.24em |
| Large Numbers | 22-30px | 1 | 700 | Syncopate | 0 |

---

## 色彩系统 (Color Palette)

### Primary Colors

```css
:root {
  /* Green Series - Brand Primary */
  --green:        #0e4a3a;    /* Main brand color */
  --green-deep:   #0a3a2d;    /* Deep green - emphasis */
  --green-ink:    #072a20;    /* Ink green - headings */
  
  /* Cream Series - Backgrounds */
  --cream:        #f4efe3;    /* Main background */
  --cream-2:      #ebe4d2;    /* Secondary background - cards */
  
  /* Text Color Series */
  --ink:          #102018;    /* Primary text */
  --ink-soft:     #3d4d44;    /* Secondary text */
  --muted:        #6f7b73;    /* Muted text */
  
  /* Decorative Colors */
  --rule:         #cbc3ae;    /* Dividers */
  --accent:       #d9a441;    /* Gold accent */
  --accent-deep:  #a87a22;    /* Deep gold */
  --danger:       #8a2e1b;    /* Warning red */
}
```

### Color Usage Principles

1. **Background Hierarchy:**
   - Primary background: `var(--cream)` #f4efe3
   - Cards/containers: `var(--cream-2)` #ebe4d2
   - Deep areas: `var(--green)` #0e4a3a
   - Deepest areas: `var(--green-ink)` #072a20

2. **Text Contrast:**
   - On light backgrounds: Use ink/ink-soft/muted series
   - On deep green backgrounds: Use cream/white

3. **Accent Color Usage:**
   - Gold (accent): Labels, highlights, key information
   - Red (danger): Warnings, risk information only
   - Green: Brand recognition, primary elements

---

## 设计元素 (Design Elements)

### 1. Checker Pattern (棋盘格)

```css
.checker {
  height: 12px;
  background-image:
    linear-gradient(45deg, var(--green) 25%, transparent 25%, transparent 75%, var(--green) 75%),
    linear-gradient(45deg, var(--green) 25%, var(--cream-2) 25%, var(--cream-2) 75%, var(--green) 75%);
  background-size: 18px 18px;
  background-position: 0 0, 9px 9px;
}
```
**Usage:** Decorative divider bands, visual rhythm

### 2. Paper Texture

```css
.flyer::before {
  content: "";
  position: absolute; inset: 0;
  background-image:
    radial-gradient(rgba(0,0,0,.04) 1px, transparent 1px),
    radial-gradient(rgba(255,255,255,.05) 1px, transparent 1px);
  background-size: 3px 3px, 5px 5px;
  background-position: 0 0, 1px 2px;
  pointer-events: none;
  mix-blend-mode: multiply;
  opacity: .55;
}
```
**Usage:** Add paper texture, warm feeling

### 3. Eyebrow Label

```css
.eyebrow {
  font-family: "JetBrains Mono", monospace;
  font-size: 11.5px;
  letter-spacing: .22em;
  text-transform: uppercase;
  color: var(--green);
  font-weight: 500;
}
```
**Usage:** Section labels, hierarchy indicators

### 4. Card Border System

```css
.card {
  background: var(--cream-2);
  border-top: 3px solid var(--green);
  padding: 22px;
}

/* Different cards use different border colors */
.card:nth-child(2) { border-top-color: var(--accent); }
.card:nth-child(3) { border-top-color: var(--green-deep); }
```

### 5. List Styling

```css
.card ul.costs li::before {
  content: "";
  position: absolute;
  left: 0; top: 8px;
  width: 6px; height: 6px;
  background: var(--danger);
}
```
**Features:** Square bullets, semantic colors

---

## 间距系统 (Spacing System)

### Primary Spacing Values

```css
/* Page-level spacing */
padding: 40px 20px;           /* Outer margins */
padding: 50px 64px;           /* Section padding */

/* Component spacing */
gap: 48px;                    /* Large component gaps */
gap: 22-26px;                 /* Medium component gaps */
gap: 12-16px;                 /* Small component gaps */

/* Text spacing */
margin-top: 24px;             /* Paragraph spacing */
margin-bottom: 14px;          /* Within-paragraph spacing */
line-height: 1.5-1.55;        /* Body line-height */
```

### CSS Variables

```css
--pad-page:     64px;
--pad-card:     22px;
--gap-large:    48px;
--gap-medium:   22px;
--gap-small:    12px;
```

---

## ICONOGRAPHY

### Icon Philosophy

DARKHORSEONE's brand uses minimal decorative iconography. The brand's strength comes from bold typography, distinctive logo symbol, and strong color palette.

**Approach:**
- **Minimal icon usage**: Use icons sparingly and only when they aid comprehension
- **No emoji**: This is a professional B2B consultancy
- **Simple geometric forms**: When icons are needed, prefer clean shapes echoing the logo
- **Line-based icons**: Use outline/stroke icons for sophistication

### Recommended Icon System

**Lucide Icons** (from CDN):
```html
<script src="https://unpkg.com/lucide@latest"></script>
```

- **Style**: Outline/stroke icons
- **Stroke width**: 2px (medium weight)
- **Size**: 20-24px for UI elements
- **Color**: Inherit or use `var(--ink-soft)`

### Logo Usage

The checkered flag symbol represents progress and winning:
- Use **white version** on dark green backgrounds
- Use **black version** on white/light backgrounds
- Maintain clear space (minimum 20% of symbol height on all sides)

**Available Logo Files:**
- `assets/logo-padded.png` — Full logo with green background (PNG)
- `assets/logo-color-bg.svg` — Full logo with green background (SVG)
- `assets/logo-color.svg` — White logo on transparent (SVG)
- `assets/logo-black.svg` — Black logo on transparent (SVG)

---

## Design Patterns

### 1. Top Brand Bar

```html
<div class="topband">
  <img src="logo.svg" class="logo" />
  <div class="meta">
    <div><strong>For</strong> &nbsp;Target Audience</div>
    <div><strong>By</strong> &nbsp;DARKHORSEONE</div>
  </div>
</div>
```

### 2. Hero Area

- Eyebrow label
- Large title (Syncopate 700, 46px, ALL CAPS)
- Lead paragraph (Inter 18px)

### 3. Section Headings

- Decorated heading with line
- Right-side metadata labels
- ALL CAPS Syncopate

### 4. Three-Column Card Layout

```css
display: grid;
grid-template-columns: repeat(3, 1fr);
gap: 16px;
```

### 5. Bottom CTA

- Deep green background
- Top checker pattern decoration
- Two-column layout

---

## Key Design Principles

1. **Clear Hierarchy:** Use font size, weight, color to establish clear visual hierarchy
2. **Strong Contrast:** Dark vs light, bold vs regular create strong contrast
3. **Technical Feel:** JetBrains Mono + wide letter-spacing + ALL CAPS = modern technical aesthetic
4. **Warm Texture:** Cream backgrounds + paper texture = approachable warmth
5. **Brand Consistency:** Green series throughout, gold as accent

---

## File Index

- `README.md` — This file, comprehensive brand guidelines
- `colors_and_type.css` — CSS variables for complete design system
- `assets/` — Logo files and brand assets
- `fonts/` — Brand font files (Gaoel, Esportiva, Helvetica LT Std)
- `preview/` — Design system preview cards (view in Design System tab)
- `SKILL.md` — Agent skill definition for cross-tool usage
