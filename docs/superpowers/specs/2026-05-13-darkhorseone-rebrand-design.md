# DarkhorseOne Rebrand of Quartz Template — Design

**Date:** 2026-05-13
**Status:** Approved (pending implementation)
**Owner:** Nick Ma

---

## Goal

Apply the `docs/DARKHORSEONE Design System` to the Quartz site template so that
every page built by `npx quartz build` ships the DarkhorseOne fonts, colors, and
visual conventions. Rebrand the user-facing surface (page title, footer) to
**DarkhorseOne Ltd**.

## Non-goals

- No content changes under `content/`.
- No new Header component or logo placement (logo files are made addressable in
  `quartz/static/`, but wiring them into the header is deferred).
- No checker-pattern divider band, paper-texture overlay, or eyebrow line above
  article titles. The utilities exist in CSS so a future pass can adopt them.
- No favicon swap.
- No i18n string changes; only the footer's JSX is changed.

## Source of truth

- `docs/DARKHORSEONE Design System/light-theme.css` — canonical light tokens.
- `docs/DARKHORSEONE Design System/dark-theme.css` — canonical dark tokens.
- `docs/DARKHORSEONE Design System/colors_and_type.css` — typography scale.
- `docs/DARKHORSEONE Design System/fonts/gaoel.ttf` — local wordmark font.
- `docs/DARKHORSEONE Design System/assets/logo-color.svg`,
  `logo-black.svg` — logo files.

When values differ between this spec and the DS CSS files, the DS files win.

---

## Decisions (from brainstorm)

1. **Headings: full DS spec.** All `h1`–`h6` use Syncopate, `text-transform: uppercase`,
   wide letter-spacing, weight 700.
2. **Dark mode: kept, with a brand-aligned dark palette** drawn verbatim from
   `dark-theme.css`.
3. **Fonts: Google Fonts CDN for Syncopate / Inter / JetBrains Mono; local
   `@font-face` for Gaoel** (wordmark only — not on Google Fonts).
4. **Branding: full rebrand.** Footer drops the "Quartz vX" link entirely and
   shows `© {year} DarkhorseOne Ltd`. Page title defaults to `DarkhorseOne Ltd`.

---

## Architecture

Quartz already exposes a CSS-variable theme contract via
`quartz/util/theme.ts → joinStyles()`. That function emits a `:root` block
(light) and a `:root[saved-theme="dark"]` block, each containing nine variables:
`--light`, `--lightgray`, `--gray`, `--darkgray`, `--dark`, `--secondary`,
`--tertiary`, `--highlight`, `--textHighlight`, plus `--titleFont`,
`--headerFont`, `--bodyFont`, `--codeFont`. Every built-in component and SCSS
rule consumes these. The cleanest path is to **rewire the existing tokens**
rather than introduce new ones — that way callouts, search highlights, syntax
themes, links, and the like inherit the brand automatically.

The DS introduces a richer vocabulary (`--bg`, `--bg-2`, `--bg-3`, `--bg-inset`,
`--rule`, `--rule-strong`, `--accent`, `--accent-deep`, `--danger`,
`--edge-highlight`, `--green`, `--green-deep`, `--green-ink`, `--ink`,
`--ink-soft`, `--muted`). These **bridge tokens** are declared as additional
custom properties inside `custom.scss` — two scoped blocks, one for light and
one for `:root[saved-theme="dark"]` — so any new component written against the
DS vocabulary works in either theme without further config.

---

## Variable mapping

### Light mode (`lightMode` in `quartz.config.ts`)

| Quartz token     | Value     | DS source             |
| ---------------- | --------- | --------------------- |
| `light`          | `#f4efe3` | `--bg` (cream)        |
| `lightgray`      | `#ebe4d2` | `--bg-2` (cards)      |
| `gray`           | `#cbc3ae` | `--rule`              |
| `darkgray`       | `#3d4d44` | `--ink-soft` (body)   |
| `dark`           | `#072a20` | `--green-ink` (headings) |
| `secondary`      | `#0e4a3a` | `--green` (links)     |
| `tertiary`       | `#d9a441` | `--accent` (gold)     |
| `highlight`      | `rgba(14, 74, 58, 0.10)` | green tint |
| `textHighlight`  | `#d9a44166` | gold ~40% alpha     |

### Dark mode (`darkMode` in `quartz.config.ts`)

| Quartz token     | Value     | DS source                     |
| ---------------- | --------- | ----------------------------- |
| `light`          | `#061f17` | `--bg` (deeper than green-ink) |
| `lightgray`      | `#0a2a20` | `--bg-2` (cards)              |
| `gray`           | `#1f3a30` | `--rule`                      |
| `darkgray`       | `#c9c1a8` | `--ink-soft` (body)           |
| `dark`           | `#f4efe3` | `--ink` (headings = cream)    |
| `secondary`      | `#2e8c6e` | `--green` dark-tuned (links)  |
| `tertiary`       | `#e6b558` | `--accent` brightened         |
| `highlight`      | `rgba(46, 140, 110, 0.15)` | dark-tuned green tint |
| `textHighlight`  | `#e6b55844` | brightened gold ~27% alpha  |

### Bridge tokens added in `custom.scss`

Light (under `:root`):

```scss
--bg-3:           #e0d8c0;
--bg-inset:       #072a20;
--rule-strong:    #b8aa86;
--accent-deep:    #a87a22;
--danger:         #8a2e1b;
--edge-highlight: inset 0 1px 0 0 rgba(255, 255, 255, 0.6);
```

Dark (under `:root[saved-theme="dark"]`):

```scss
--bg-3:           #11382c;
--bg-inset:       #04150f;
--rule-strong:    #2e5345;
--accent-deep:    #c89236;
--danger:         #d96a52;
--edge-highlight: inset 0 1px 0 0 rgba(255, 255, 255, 0.04);
```

---

## Typography

`quartz.config.ts → theme.typography`:

```ts
typography: {
  title:  "Syncopate",
  header: "Syncopate",
  body:   "Inter",
  code:   "JetBrains Mono",
},
fontOrigin: "googleFonts",
cdnCaching: true,
```

This drives `--titleFont`, `--headerFont`, `--bodyFont`, `--codeFont` in
`joinStyles`, and Quartz fetches the three families from Google Fonts.

**Local Gaoel** lives at `quartz/static/fonts/gaoel.ttf` and is loaded via
`@font-face` in `custom.scss`. It is used only by the `.brand-wordmark`
utility class — i.e., the PageTitle string and any footer wordmark — and
falls back to Syncopate.

The `@font-face` `url(...)` uses the relative path `static/fonts/gaoel.ttf`
because Quartz emits the compiled stylesheet at the build root and
`Plugin.Static()` copies `quartz/static/` to `/static/`. Implementation
should verify the resolved URL by inspecting the network panel; if the
relative form fails, fall back to absolute `/static/fonts/gaoel.ttf`
(safe given the current `baseUrl` has no subpath).

### Heading rule (custom.scss)

```scss
h1, h2, h3, h4, h5, h6 {
  font-family: "Syncopate", "Inter", sans-serif;
  text-transform: uppercase;
  letter-spacing: 0.01em;
  font-weight: 700;
}
h1 { letter-spacing: -0.005em; }
```

Inherited heading sizes from `base.scss` (1.75rem / 1.4rem / 1.12rem / 1rem)
are kept. The DS table specifies larger sizes (46px / 20–26px / 16px) — those
are display-page sizes; the wiki keeps the smaller hierarchy for long-form
readability. (This is the only DS deviation in the spec; flagged explicitly.)

---

## Branding swap

### `quartz.config.ts`

```ts
configuration: {
  pageTitle: "DarkhorseOne Ltd",
  pageTitleSuffix: "",
  // …rest unchanged
}
```

### `quartz/components/PageTitle.tsx`

Wrap the rendered title text in `<span class="brand-wordmark">{title}</span>`
so the Gaoel `@font-face` applies. The wrapping `<h2 class="page-title">`
still gets `font-family: var(--titleFont)` from the component's own CSS, but
each element resolves its own font-family independently — the span's
`.brand-wordmark` rule wins for the text node it contains. No other behavior
change.

### `quartz/components/Footer.tsx`

Replace the current line:

```tsx
<a href="https://quartz.jzhao.xyz/">Quartz v{version}</a> © {year}
```

with:

```tsx
<p>© {year} <span class="brand">DarkhorseOne Ltd</span></p>
```

Remove the `version` import and the `createdWith` i18n usage from this file.
Leave the `createdWith` key in the locale files untouched (other components or
future use may want it).

---

## `custom.scss` final shape

```scss
@use "./base.scss";

/* DS bridge tokens (light) */
:root {
  --bg-3:           #e0d8c0;
  --bg-inset:       #072a20;
  --rule-strong:    #b8aa86;
  --accent-deep:    #a87a22;
  --danger:         #8a2e1b;
  --edge-highlight: inset 0 1px 0 0 rgba(255, 255, 255, 0.6);
}

/* DS bridge tokens (dark) */
:root[saved-theme="dark"] {
  --bg-3:           #11382c;
  --bg-inset:       #04150f;
  --rule-strong:    #2e5345;
  --accent-deep:    #c89236;
  --danger:         #d96a52;
  --edge-highlight: inset 0 1px 0 0 rgba(255, 255, 255, 0.04);
}

/* Wordmark font */
@font-face {
  font-family: "Gaoel";
  src: url("static/fonts/gaoel.ttf") format("truetype");
  font-display: swap;
}

/* Headings — full DS spec */
h1, h2, h3, h4, h5, h6 {
  font-family: "Syncopate", "Inter", sans-serif;
  text-transform: uppercase;
  letter-spacing: 0.01em;
  font-weight: 700;
}
h1 { letter-spacing: -0.005em; }

/* Brand wordmark (PageTitle, Footer) */
.brand-wordmark {
  font-family: "Gaoel", "Syncopate", sans-serif;
  letter-spacing: 0.04em;
}

/* Eyebrow utility (DS metadata label) */
.eyebrow {
  font-family: "JetBrains Mono", monospace;
  font-size: 0.72rem;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--secondary);
  font-weight: 500;
}

/* Brand-tinted blockquote */
blockquote { border-left-color: var(--secondary); }

/* Footer */
footer { font-family: "Inter", sans-serif; }
footer .brand {
  font-family: "Syncopate", sans-serif;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}
```

---

## Static assets

Copy into `quartz/static/`:

- `fonts/gaoel.ttf` (from `docs/DARKHORSEONE Design System/fonts/gaoel.ttf`)
- `logo-color.svg` (from `docs/.../assets/`)
- `logo-black.svg` (from `docs/.../assets/`)

The logos are addressable as `/static/logo-color.svg` and `/static/logo-black.svg`
after build but are not yet referenced by any component — that's deliberate
(deferred to a later Header pass).

---

## Verification

After running `npx quartz build && npx quartz serve`:

1. Open the home page. Body background is cream `#f4efe3`. Body text is
   ink-soft. Headings render in Syncopate, ALL CAPS, in deep green-ink.
2. The PageTitle in the sidebar reads `DarkhorseOne Ltd` in Gaoel (or
   Syncopate if Gaoel fails to load).
3. The footer reads `© 2026 DarkhorseOne Ltd` with no Quartz link. The brand
   span renders in Syncopate ALL CAPS.
4. Click the dark-mode toggle. Background flips to `#061f17`, headings become
   cream, links become dark-tuned green `#2e8c6e`.
5. Hover a link in either mode — color transitions to gold (`tertiary`).
6. Code blocks use JetBrains Mono.
7. Search modal, callouts, syntax-highlight backgrounds all pick up the new
   palette without further changes.

---

## Reversibility

Every change is contained in:

- `quartz.config.ts` — theme block, `pageTitle`
- `quartz/styles/custom.scss` — additions only
- `quartz/components/Footer.tsx` — JSX/imports change (drop version + i18n
  imports, replace the paragraph)
- `quartz/components/PageTitle.tsx` — single span addition
- `quartz/static/fonts/gaoel.ttf` — new file
- `quartz/static/logo-color.svg`, `logo-black.svg` — new files

A single `git revert` of the implementation commit restores the upstream
Quartz look.
