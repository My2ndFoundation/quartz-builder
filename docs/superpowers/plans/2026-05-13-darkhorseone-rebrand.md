# DarkhorseOne Rebrand Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Apply the DARKHORSEONE Design System to the Quartz template (colors, typography, branding) so every built page ships in the brand. Rebrand the user-facing surface to "DarkhorseOne Ltd".

**Architecture:** Rewire Quartz's existing nine-token CSS theme variables (`--light`, `--secondary`, etc.) to the DS palette in `quartz.config.ts`. Layer DS "bridge tokens" (`--bg-3`, `--accent-deep`, `--danger`, …) on top in `custom.scss` so the richer DS vocabulary works in either theme. Use Quartz's `googleFonts` pipeline for Syncopate/Inter/JetBrains Mono; self-host Gaoel for the wordmark. Swap branding in `PageTitle.tsx` and `Footer.tsx`.

**Tech Stack:** TypeScript, Preact JSX, SCSS, Quartz 4 build system.

**Spec:** `docs/superpowers/specs/2026-05-13-darkhorseone-rebrand-design.md`

---

## File Structure

**Files to modify:**
- `quartz.config.ts` — theme colors + typography + `pageTitle`.
- `quartz/styles/custom.scss` — DS bridge tokens, `@font-face` for Gaoel, heading rules, brand utilities.
- `quartz/components/Footer.tsx` — strip Quartz attribution, render DarkhorseOne Ltd.
- `quartz/components/PageTitle.tsx` — wrap title in `<span class="brand-wordmark">`.

**Files to create:**
- `quartz/static/fonts/gaoel.ttf` — copied from DS folder.
- `quartz/static/logo-color.svg` — copied from DS folder.
- `quartz/static/logo-black.svg` — copied from DS folder.

**Verification target:** A successful `npx quartz build` produces `public/index.html` referencing the new fonts/colors; a quick visual smoke test in `npx quartz build --serve` confirms light and dark themes both render correctly.

**Testing note:** Quartz has no unit-test harness for theme rendering. Verification in each task is a combination of (a) `tsc --noEmit` type checks via the build pipeline, (b) `npx quartz build` succeeding, and (c) a defined visual/grep check. There are no Jest/Vitest tests to write.

---

### Task 1: Copy brand assets into the Quartz static folder

**Files:**
- Create: `quartz/static/fonts/gaoel.ttf`
- Create: `quartz/static/logo-color.svg`
- Create: `quartz/static/logo-black.svg`

- [ ] **Step 1: Verify the source files exist**

Run:
```bash
ls "docs/DARKHORSEONE Design System/fonts/gaoel.ttf" \
   "docs/DARKHORSEONE Design System/assets/logo-color.svg" \
   "docs/DARKHORSEONE Design System/assets/logo-black.svg"
```
Expected: all three files listed, no "No such file" errors.

- [ ] **Step 2: Create the destination folder**

Run:
```bash
mkdir -p quartz/static/fonts
```
Expected: no output, folder created.

- [ ] **Step 3: Copy the font**

Run:
```bash
cp "docs/DARKHORSEONE Design System/fonts/gaoel.ttf" quartz/static/fonts/gaoel.ttf
```
Expected: file exists at `quartz/static/fonts/gaoel.ttf`.

- [ ] **Step 4: Copy the logos**

Run:
```bash
cp "docs/DARKHORSEONE Design System/assets/logo-color.svg" quartz/static/logo-color.svg
cp "docs/DARKHORSEONE Design System/assets/logo-black.svg"  quartz/static/logo-black.svg
```
Expected: both SVGs in `quartz/static/`.

- [ ] **Step 5: Verify the copies**

Run:
```bash
ls -la quartz/static/fonts/gaoel.ttf quartz/static/logo-color.svg quartz/static/logo-black.svg
```
Expected: three files with non-zero size.

- [ ] **Step 6: Commit**

```bash
git add quartz/static/fonts/gaoel.ttf quartz/static/logo-color.svg quartz/static/logo-black.svg
git commit -m "chore(branding): add DarkhorseOne logo and Gaoel font to static assets"
```

---

### Task 2: Rewire the theme in `quartz.config.ts`

**Files:**
- Modify: `quartz.config.ts` (the `configuration.pageTitle` field and the entire `configuration.theme` block)

- [ ] **Step 1: Read the current `configuration` block**

Run:
```bash
sed -n '10,55p' quartz.config.ts
```
Expected: shows `pageTitle: "Quartz 4"`, `typography: { header, body, code }`, and the `colors.lightMode` / `colors.darkMode` blocks.

- [ ] **Step 2: Replace `pageTitle`**

In `quartz.config.ts`, change:
```ts
    pageTitle: "Quartz 4",
```
to:
```ts
    pageTitle: "DarkhorseOne Ltd",
```

- [ ] **Step 3: Replace the typography block**

In `quartz.config.ts`, change:
```ts
      typography: {
        header: "Schibsted Grotesk",
        body: "Source Sans Pro",
        code: "IBM Plex Mono",
      },
```
to:
```ts
      typography: {
        title: "Syncopate",
        header: "Syncopate",
        body: "Inter",
        code: "JetBrains Mono",
      },
```

- [ ] **Step 4: Replace the `lightMode` palette**

In `quartz.config.ts`, change:
```ts
        lightMode: {
          light: "#faf8f8",
          lightgray: "#e5e5e5",
          gray: "#b8b8b8",
          darkgray: "#4e4e4e",
          dark: "#2b2b2b",
          secondary: "#284b63",
          tertiary: "#84a59d",
          highlight: "rgba(143, 159, 169, 0.15)",
          textHighlight: "#fff23688",
        },
```
to:
```ts
        lightMode: {
          light: "#f4efe3",
          lightgray: "#ebe4d2",
          gray: "#cbc3ae",
          darkgray: "#3d4d44",
          dark: "#072a20",
          secondary: "#0e4a3a",
          tertiary: "#d9a441",
          highlight: "rgba(14, 74, 58, 0.10)",
          textHighlight: "#d9a44166",
        },
```

- [ ] **Step 5: Replace the `darkMode` palette**

In `quartz.config.ts`, change:
```ts
        darkMode: {
          light: "#161618",
          lightgray: "#393639",
          gray: "#646464",
          darkgray: "#d4d4d4",
          dark: "#ebebec",
          secondary: "#7b97aa",
          tertiary: "#84a59d",
          highlight: "rgba(143, 159, 169, 0.15)",
          textHighlight: "#b3aa0288",
        },
```
to:
```ts
        darkMode: {
          light: "#061f17",
          lightgray: "#0a2a20",
          gray: "#1f3a30",
          darkgray: "#c9c1a8",
          dark: "#f4efe3",
          secondary: "#2e8c6e",
          tertiary: "#e6b558",
          highlight: "rgba(46, 140, 110, 0.15)",
          textHighlight: "#e6b55844",
        },
```

- [ ] **Step 6: Run type-check via the build pipeline**

Run:
```bash
npx tsc --noEmit -p .
```
Expected: exits 0, no type errors. (The `title` field is already typed in `quartz/util/theme.ts:28`, so adding it should compile cleanly.)

- [ ] **Step 7: Commit**

```bash
git add quartz.config.ts
git commit -m "feat(theme): apply DarkhorseOne color and typography tokens"
```

---

### Task 3: Add `@font-face`, DS bridge tokens, and the brand-wordmark utility to `custom.scss`

**Files:**
- Modify: `quartz/styles/custom.scss`

- [ ] **Step 1: Read the current file**

Run:
```bash
cat quartz/styles/custom.scss
```
Expected output:
```
@use "./base.scss";

// put your custom CSS here!
```

- [ ] **Step 2: Replace the file contents**

Replace the entire contents of `quartz/styles/custom.scss` with:

```scss
@use "./base.scss";

/* ---- DS bridge tokens (light) ---- */
:root {
  --bg-3:           #e0d8c0;
  --bg-inset:       #072a20;
  --rule-strong:    #b8aa86;
  --accent-deep:    #a87a22;
  --danger:         #8a2e1b;
  --edge-highlight: inset 0 1px 0 0 rgba(255, 255, 255, 0.6);
}

/* ---- DS bridge tokens (dark) ---- */
:root[saved-theme="dark"] {
  --bg-3:           #11382c;
  --bg-inset:       #04150f;
  --rule-strong:    #2e5345;
  --accent-deep:    #c89236;
  --danger:         #d96a52;
  --edge-highlight: inset 0 1px 0 0 rgba(255, 255, 255, 0.04);
}

/* ---- Brand wordmark font ---- */
@font-face {
  font-family: "Gaoel";
  src: url("static/fonts/gaoel.ttf") format("truetype");
  font-display: swap;
}

/* ---- Brand wordmark (PageTitle / Footer) ---- */
.brand-wordmark {
  font-family: "Gaoel", "Syncopate", sans-serif;
  letter-spacing: 0.04em;
}
```

- [ ] **Step 3: Run a full build to confirm SCSS compiles**

Run:
```bash
npx quartz build 2>&1 | tail -20
```
Expected: build succeeds (`Done processing X files` line near the end), no SCSS compilation errors.

- [ ] **Step 4: Confirm the bridge tokens land in the compiled CSS**

Run:
```bash
grep -E "(--bg-3|--rule-strong|--accent-deep|Gaoel)" public/index.css | head -10
```
Expected: at least 4 hits — both light & dark `--bg-3` declarations, the `@font-face` Gaoel rule, etc.

- [ ] **Step 5: Commit**

```bash
git add quartz/styles/custom.scss
git commit -m "feat(styles): add DS bridge tokens, Gaoel @font-face, brand-wordmark utility"
```

---

### Task 4: Add ALL-CAPS Syncopate heading rules to `custom.scss`

**Files:**
- Modify: `quartz/styles/custom.scss`

- [ ] **Step 1: Append heading rules to `custom.scss`**

Append this block to the end of `quartz/styles/custom.scss`:

```scss
/* ---- Headings: full DS spec ---- */
h1, h2, h3, h4, h5, h6 {
  font-family: "Syncopate", "Inter", sans-serif;
  text-transform: uppercase;
  letter-spacing: 0.01em;
  font-weight: 700;
}
h1 { letter-spacing: -0.005em; }
```

- [ ] **Step 2: Build**

Run:
```bash
npx quartz build 2>&1 | tail -5
```
Expected: build succeeds.

- [ ] **Step 3: Confirm headings render in Syncopate ALL CAPS**

Run:
```bash
grep -A2 "h1, h2, h3, h4, h5, h6" public/index.css | head -10
```
Expected: a block containing `font-family:"Syncopate"` and `text-transform:uppercase`.

- [ ] **Step 4: Commit**

```bash
git add quartz/styles/custom.scss
git commit -m "feat(styles): style headings with Syncopate ALL CAPS per DS"
```

---

### Task 5: Add eyebrow, blockquote, and footer utility styles to `custom.scss`

**Files:**
- Modify: `quartz/styles/custom.scss`

- [ ] **Step 1: Append the remaining utility rules**

Append this block to the end of `quartz/styles/custom.scss`:

```scss
/* ---- Eyebrow utility (DS metadata label) ---- */
.eyebrow {
  font-family: "JetBrains Mono", monospace;
  font-size: 0.72rem;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--secondary);
  font-weight: 500;
}

/* ---- Brand-tinted blockquote ---- */
blockquote { border-left-color: var(--secondary); }

/* ---- Footer ---- */
footer { font-family: "Inter", sans-serif; }
footer .brand {
  font-family: "Syncopate", sans-serif;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}
```

- [ ] **Step 2: Build**

Run:
```bash
npx quartz build 2>&1 | tail -5
```
Expected: build succeeds.

- [ ] **Step 3: Confirm the new rules compile**

Run:
```bash
grep -E "(\.eyebrow|footer \.brand)" public/index.css | head -5
```
Expected: at least 2 matching rules.

- [ ] **Step 4: Commit**

```bash
git add quartz/styles/custom.scss
git commit -m "feat(styles): add eyebrow, brand footer, and tinted blockquote utilities"
```

---

### Task 6: Rebrand `Footer.tsx`

**Files:**
- Modify: `quartz/components/Footer.tsx`

- [ ] **Step 1: Read the current file**

Run:
```bash
cat quartz/components/Footer.tsx
```
Expected output (key lines):
```
import { version } from "../../package.json"
import { i18n } from "../i18n"
…
<p>
  {i18n(cfg.locale).components.footer.createdWith}{" "}
  <a href="https://quartz.jzhao.xyz/">Quartz v{version}</a> © {year}
</p>
```

- [ ] **Step 2: Replace the file contents**

Replace the entire contents of `quartz/components/Footer.tsx` with:

```tsx
import { QuartzComponent, QuartzComponentConstructor, QuartzComponentProps } from "./types"
import style from "./styles/footer.scss"

interface Options {
  links: Record<string, string>
}

export default ((opts?: Options) => {
  const Footer: QuartzComponent = ({ displayClass }: QuartzComponentProps) => {
    const year = new Date().getFullYear()
    const links = opts?.links ?? []
    return (
      <footer class={`${displayClass ?? ""}`}>
        <p>
          © {year} <span class="brand">DarkhorseOne Ltd</span>
        </p>
        <ul>
          {Object.entries(links).map(([text, link]) => (
            <li>
              <a href={link}>{text}</a>
            </li>
          ))}
        </ul>
      </footer>
    )
  }

  Footer.css = style
  return Footer
}) satisfies QuartzComponentConstructor
```

Note: the `cfg`, `version`, and `i18n` imports are removed because they are no longer referenced. The `QuartzComponentProps` import stays — it's still used by the `Footer` signature.

- [ ] **Step 3: Type-check**

Run:
```bash
npx tsc --noEmit -p .
```
Expected: exits 0. No "is declared but its value is never read" errors (we deleted the unused imports).

- [ ] **Step 4: Build**

Run:
```bash
npx quartz build 2>&1 | tail -5
```
Expected: build succeeds.

- [ ] **Step 5: Confirm the footer renders correctly**

Run:
```bash
grep -E "(DarkhorseOne Ltd|Quartz v)" public/index.html | head -5
```
Expected: at least one hit for "DarkhorseOne Ltd"; zero hits for "Quartz v".

- [ ] **Step 6: Commit**

```bash
git add quartz/components/Footer.tsx
git commit -m "feat(branding): replace Quartz attribution with DarkhorseOne Ltd in footer"
```

---

### Task 7: Wrap `PageTitle.tsx` title in `.brand-wordmark`

**Files:**
- Modify: `quartz/components/PageTitle.tsx`

- [ ] **Step 1: Read the current file**

Run:
```bash
cat quartz/components/PageTitle.tsx
```
Expected output includes:
```
<h2 class={classNames(displayClass, "page-title")}>
  <a href={baseDir}>{title}</a>
</h2>
```

- [ ] **Step 2: Modify the JSX**

In `quartz/components/PageTitle.tsx`, change:
```tsx
    <h2 class={classNames(displayClass, "page-title")}>
      <a href={baseDir}>{title}</a>
    </h2>
```
to:
```tsx
    <h2 class={classNames(displayClass, "page-title")}>
      <a href={baseDir}>
        <span class="brand-wordmark">{title}</span>
      </a>
    </h2>
```

- [ ] **Step 3: Type-check**

Run:
```bash
npx tsc --noEmit -p .
```
Expected: exits 0.

- [ ] **Step 4: Build**

Run:
```bash
npx quartz build 2>&1 | tail -5
```
Expected: build succeeds.

- [ ] **Step 5: Confirm the wordmark span is in the output**

Run:
```bash
grep -E '<span class="brand-wordmark">DarkhorseOne Ltd</span>' public/index.html | head -1
```
Expected: at least one matching line.

- [ ] **Step 6: Commit**

```bash
git add quartz/components/PageTitle.tsx
git commit -m "feat(branding): render PageTitle in Gaoel via .brand-wordmark"
```

---

### Task 8: Full-site visual smoke test

**Files:** none modified — verification only.

- [ ] **Step 1: Start the dev server in the background**

Run (in a separate terminal, or background it):
```bash
npx quartz build --serve
```
Expected: server starts on `http://localhost:8080` (default).

- [ ] **Step 2: Inspect light mode**

Open `http://localhost:8080` in a browser. Verify by eye:

| Check | Expected |
| ----- | -------- |
| Body background | Cream `#f4efe3` |
| Headings | Syncopate, ALL CAPS, deep green-ink color |
| PageTitle in sidebar | Reads "DarkhorseOne Ltd" in Gaoel (geometric/athletic letterforms, distinct from Syncopate) |
| Footer | "© 2026 DarkhorseOne Ltd" with brand span in Syncopate, no Quartz link |
| Links | Brand green `#0e4a3a`, gold `#d9a441` on hover |
| Code blocks | JetBrains Mono |

- [ ] **Step 3: Inspect dark mode**

Click the dark-mode toggle in the sidebar. Verify by eye:

| Check | Expected |
| ----- | -------- |
| Body background | `#061f17` (deep green-black) |
| Headings | Cream `#f4efe3` |
| Body text | Warm cream-grey `#c9c1a8` |
| Links | Dark-tuned green `#2e8c6e`, gold `#e6b558` on hover |
| Footer text | Still legible in cream on the deep canvas |

- [ ] **Step 4: Confirm Gaoel actually loads**

In the browser DevTools → Network tab, filter by `gaoel`. Reload. Expected: one 200 response for `static/fonts/gaoel.ttf` (or `/static/fonts/gaoel.ttf`).

If the request 404s, change the `url(...)` in `quartz/styles/custom.scss` from `static/fonts/gaoel.ttf` to `/static/fonts/gaoel.ttf` (absolute), rebuild, recheck. Commit the fix as `fix(styles): use absolute path for Gaoel font url`.

- [ ] **Step 5: Stop the server and commit any fixes**

Stop the dev server. If no fixes were needed in Step 4, there is nothing to commit.

---

## Implementation Order Summary

1. Copy static assets (Task 1)
2. Rewire theme tokens (Task 2)
3. Bridge tokens + `@font-face` + `.brand-wordmark` (Task 3)
4. Heading rules (Task 4)
5. Utility classes (Task 5)
6. Footer rebrand (Task 6)
7. PageTitle wrap (Task 7)
8. End-to-end smoke test (Task 8)

Each task is independently committable; if work is paused mid-plan, the site still builds.

---

## Out of scope (do not implement)

- Custom Header component with the logo image.
- Checker-pattern divider band.
- Paper-texture overlay.
- Eyebrow line above each article title.
- Favicon swap.
- Content changes under `content/`.
- i18n locale string changes.
