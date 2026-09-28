# Mira Voss — Responsive Portfolio

A responsive single-page portfolio for a fictional landscape photographer, built with plain HTML and CSS (no JavaScript, no frameworks). It was made as a front-end internship exercise on responsive design: flexible grids, fluid images, and media queries.

## Project structure

```
.
├── index.html                    # Page markup
├── styles.css                    # All styling and media queries
├── img/                          # 23 generated JPEGs (each in 2–3 sizes)
├── docs/
│   ├── responsive-report.md      # Methods, layout changes, challenges
│   ├── responsive-report.docx    # Same report as a Word document
│   └── screenshots/              # Images used by the Markdown report
└── README.md
```

`report.md` and `report.docx` in the top-level folder belong to an earlier exercise (a coffee-roastery page) and are not part of this project.

## How to run

The site is static, so there is nothing to install or build.

**Option 1: open the file.** Keep `index.html`, `styles.css` and `img/` together in one folder, then double-click `index.html`.

**Option 2: VS Code Live Server.** Open the folder in VS Code (File → Open Folder), install the "Live Server" extension by Ritwick Dey, then right-click `index.html` and choose "Open with Live Server". The page reloads whenever you save.

**Option 3: Python.** From the project folder run `python3 -m http.server 8000`, then visit http://localhost:8000.

An internet connection is needed for the web fonts (Newsreader and IBM Plex Sans from Google Fonts). Without one, the page falls back to system serif and sans-serif fonts and still lays out correctly.

## Responsive behaviour

| Viewport | Gallery | About | Navigation | Footer |
|---|---|---|---|---|
| Desktop (over 900px) | 3 columns | 2 columns | Inline links | 4 columns |
| Tablet (601–900px) | 2 columns | 1 column | Inline links | 2 columns |
| Mobile (600px and below) | 1 column | 1 column | Hamburger menu | 1 column |

Breakpoints are at **900px** and **600px**. The gallery column count also adapts on its own between breakpoints because it uses `repeat(auto-fill, minmax(260px, 1fr))`.

## Techniques used

- **Fluid images:** `max-width: 100%; height: auto` on all images, with `width`/`height` attributes to prevent layout shift.
- **Responsive images:** `srcset` and `sizes` on gallery and About images, so each device downloads an appropriately sized file.
- **Art direction:** a `<picture>` element in the hero serves a different crop on phones, tablets and desktops.
- **Flexible grid:** CSS Grid with `auto-fill` and `minmax()` for the gallery; Flexbox for nav, buttons and forms.
- **Fluid typography and spacing:** `clamp()` for headings, the pull quote, section padding, and the hero height.
- **No-JavaScript mobile menu:** a checkbox plus `:checked` sibling selector.
- **Accessibility:** skip link, visible focus outlines, `alt` text on every image, semantic landmarks, and `prefers-reduced-motion` support.

## Browser support

Built for current versions of Chrome, Firefox, Safari and Edge. It relies on CSS Grid, `clamp()`, `aspect-ratio`, `<picture>` and `srcset`, so very old browsers are not supported.

## Testing

Layout was checked in Chromium at 1440px, 834px and 390px. At each width the page had no horizontal overflow, and the gallery showed 3, 2 and 1 columns respectively. It has not been tested on physical iOS or Android devices, so a real-device check is recommended.

## Images

All artwork is procedurally generated abstract landscapes made with Python and Pillow, so there are no third-party copyright concerns. The names, places and press quote in the page copy are fictional.

## Documentation

See [`docs/responsive-report.md`](docs/responsive-report.md) (or the `.docx` version) for the design decisions, screenshots at each viewport, and the challenges encountered.
