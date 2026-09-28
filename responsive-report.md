# Responsive Design Documentation — Mira Voss Portfolio

**Deliverables:** `index.html`, `styles.css`, `img/` (23 generated JPEGs), this document.
**Tested in:** Chromium 141 at 1440px (desktop), 834px (tablet), 390px (mobile).

## 1. Project summary

A single-page portfolio for a fictional landscape photographer. The subject was chosen because it is image-heavy, which makes the three techniques the brief asks for easy to demonstrate and to verify: **flexible grids, fluid images, and media queries.**

All artwork is procedurally generated (Python/Pillow) so there are no copyright concerns. Each image exists in several pixel sizes so the browser can choose the right file for the device.

## 2. Responsive strategy

| Technique | Where it is used | Why |
|---|---|---|
| Fluid images (`max-width:100%; height:auto`) | Global `img` rule | No image can overflow its container |
| `<picture>` art direction | Hero | Different crop per device: portrait on phones, wide on desktop |
| `srcset` + `sizes` | Gallery and About images | Browser downloads 640w / 1000w / 1600w based on layout width and screen density |
| Flexible grid: `repeat(auto-fill, minmax(260px, 1fr))` | Gallery | Column count adapts to available space |
| Fluid type: `clamp()` | h1, h2, pull quote, section padding | Sizes scale smoothly between breakpoints |
| Media queries | 900px and 600px | Structural changes: stacking, nav collapse |
| `aspect-ratio` + `object-fit: cover` | Gallery, About | Uniform tiles regardless of source ratio |
| `width`/`height` attributes on `<img>` | All content images | Reserves space before load, preventing layout shift |

**Approach: desktop-first with two `max-width` breakpoints.** Breakpoints were placed where the layout actually needed to change, not at specific device widths. Most adaptation (gallery columns, type size, spacing) happens *fluidly between* breakpoints; the media queries handle only what fluid rules cannot: switching the About section to one column and replacing the nav with a toggle menu.

## 3. How the design adjusts

### Desktop (≥ 901px)
- Inline nav with a bordered call-to-action.
- Gallery: **3 columns**.
- About: two columns (image left, text right).
- Footer: four columns.

![Desktop above the fold](screenshots/desktop-fold.jpg)

### Tablet (601–900px)
- Gallery: **2 columns** (the grid recalculates on its own, no media query).
- About: stacks to one column, image capped at 420px.
- Footer: 2 × 2 grid.
- Hero shortens; the medium-crop hero image is served.

![Tablet above the fold](screenshots/tablet-fold.jpg)

### Mobile (≤ 600px)
- Nav collapses to a hamburger toggle, built with a checkbox and `:checked` (no JavaScript).
- Gallery: **1 column**; the 640px image variant is downloaded.
- Hero switches to a portrait crop and becomes taller relative to width.
- Footer and mailing-list form stack vertically.

| Menu closed | Menu open |
|---|---|
| ![Mobile](screenshots/mobile-fold.jpg) | ![Mobile menu](screenshots/mobile-menu-open.jpg) |

### Measured results

| Viewport | Gallery columns | Horizontal overflow |
|---|---|---|
| 1440px | 3 | 0px |
| 834px | 2 | 0px |
| 390px | 1 | 0px |

## 4. Challenges and how they were solved

**1. Hero image and overlay stacking.** The hero uses a `<picture>` positioned behind an overlay. In my first (old-engine) screenshot tool the image rendered as a small in-flow box instead of filling the section, because the shorthand `inset: 0` was not understood. I replaced it with explicit `top/right/bottom/left: 0` and added `display:block` on the `<picture>`. Chromium rendered the shorthand correctly, so this is a compatibility hardening rather than a bug I reproduced in a modern browser.

**2. Global `img { max-width:100% }` conflicting with a full-bleed image.** The rule that makes content images safe also prevents an image from growing to fill a wider container. The hero image deliberately overrides it.

**3. Hero text misaligned with the page grid.** The text block was capped at 720px on the same element that centres the page wrapper, so it floated in the middle instead of aligning with the content edge below. Found by reviewing the real browser render. Fix: cap the width on the *children* of the wrapper instead of the wrapper itself.

**4. Runaway `vh` heights.** A bare `min-height: 78vh` can become enormous on tall or unusual windows. Fix: `clamp(440px, 78vh, 820px)`.

**5. Testing tooling.** My first screenshot tool used a very old rendering engine that lacks CSS Grid, `clamp()` and reliable `srcset` handling, and produced misleading, broken renders (collapsed grids, missing images, a 7,000px hero). I confirmed the markup was valid by isolating each feature, then switched to a real Chromium for all results in this document. Lesson: test in a current browser and verify with measurable checks (column counts, `scrollWidth`) rather than trusting one screenshot.

**6. Image weight.** Serving one large image to every device wastes bandwidth on phones. Solved with three pre-sized variants per image and `sizes="(max-width:600px) 100vw, (max-width:900px) 50vw, 33vw"`, which mirrors the actual grid widths.

## 5. Accessibility and robustness

- Skip link, visible `:focus-visible` outlines, descriptive `alt` text on every image.
- `prefers-reduced-motion` disables transitions and smooth scrolling.
- Semantic structure: `header`, `nav`, `main`, five `section`s, `figure`/`figcaption`, `footer`.
- Lazy loading on below-the-fold images.
- Font stacks have system fallbacks, so the layout holds if web fonts fail to load.

## 6. Known limitations and next steps

- Only two breakpoints; a large-desktop (1600px+) tier could add a wider gallery.
- The hero uses `100vh`-style units; on mobile browsers whose address bar collapses, `svh`/`dvh` units would be more precise.
- Real device testing (iOS Safari, Android Chrome) is still recommended; results here come from Chromium emulation.

## 7. Full-page views

| Desktop | Tablet | Mobile |
|---|---|---|
| ![](screenshots/desktop-full.jpg) | ![](screenshots/tablet-full.jpg) | ![](screenshots/mobile-full.jpg) |
