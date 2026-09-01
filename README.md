# GlowQueen — Header & Hero

Static header-hero for a spa and beauty business. First CSS deliverable of the
ConquerBlocks Full Stack master's programme.

> **Live:** https://cgvrzon.github.io/header-spa-beauty/
>
> **Status:** complete. Measured fidelity against the reference: **99.08%**.

## The brief

Reproduce the provided design as a static page. Not responsive — it only has to
match the reference at its native size.

| Requirement | How it is met |
|---|---|
| Free choice of boilerplate | 7-1 architecture from the Sass Guidelines (see *Structure*) |
| SASS recommended | Dart Sass with `@use`, 13 partials split by responsibility |
| Static design | Fixed 1920 × 1080 canvas, centred |
| SVG icons | Inline SVG using `currentColor`, so hover recolours them |
| Semantic markup | Landmarks, labelled navigation, `figure`/`figcaption` quotes |
| Hover states | All 12 interactive elements, verified programmatically |
| Fidelity to the reference | 99.08%, measured pixel by pixel (see *Fidelity*) |

## Running it

```bash
npm install
npm run dev      # watch mode, expanded output
npm run build    # single compressed build
npm run serve    # static server on :8080
```

Open it through the server rather than double-clicking `index.html`: a `file://`
page has no origin, and the self-hosted fonts may be blocked without any visible
symptom other than the wrong typeface.

## Structure

The stylesheet follows the **7-1 architecture** from the Sass Guidelines. Four of
its seven folders are used; the other three — `pages/`, `themes/`, `vendors/` —
are omitted rather than left empty, because this is a single page, with a single
theme, and no third-party CSS.

```
index.html                     the whole page - one header, one hero
src/scss/
  main.scss                    the only entry point Sass compiles
  abstracts/_variables.scss    design tokens: colour, type, spacing, shape
  base/_reset.scss             small deliberate reset + a11y helpers
  base/_fonts.scss             @font-face for the five self-hosted faces
  base/_typography.scss        page-wide type defaults
  layout/_background-gradient.scss   the two-tone backdrop
  layout/_site-header.scss     the banner row
  layout/_hero.scss            the hero and the portrait
  components/                  logo, button, icon, stats, testimonial
dist/css/main.css              build output
assets/{img,icons,fonts}/      exported design assets and web fonts
```

Everything under `src/scss/` other than `main.scss` is a partial (leading
underscore) and never produces a file of its own.

## Measurements

Every design value is **measured, not guessed** — and the story of where they
came from is worth telling, because the first set was wrong.

They were initially read off the reference PDF: rendered at 72 dpi, a 1920 × 1080
pt document gives 1920 × 1080 px, a one-to-one match with CSS pixels. Colours
were sampled per pixel, distances by scanning for colour transitions.

That method has a blind spot: **sampling a rendered image cannot tell a colour
apart from a blend**. Two "tokens" obtained that way did not exist in the design
at all — one was the navy text at 65% over the cream, picked up from antialiased
glyph edges; the other was a translucent card over the mint panel. Several
distances were also 1–4px wide, because antialiasing widens whatever you sample.

The values below come from the Figma source and supersede that first pass:

| What | Value | First measured as |
|---|---|---|
| Left gutter (logo and headline share it) | **160px** | 162px |
| Background band split | **y = 820** | correct |
| Mint panel | **x 1285 → 1920, 820 tall** | correct |
| Headline | **64px / 80px line height** | 72–76px |
| Navigation gap | **56px** | 60px |

## Typography

Poppins throughout, plus Nunito Sans on two controls where the designer left it —
almost certainly an oversight, reproduced deliberately for fidelity.

Both are **self-hosted**, not loaded from Google's CDN. Serving fonts from the CDN
transmits the visitor's IP to a third party with no legal basis; a German court
ruled on exactly that (LG München I, 20/01/2022, case 3 O 17493/20). The families
are open source, so hosting them is a licence-clean, one-line fix.

Measuring the two families showed they ship differently, which changes where the
weight savings are: Poppins is **static** — one file per weight, so each weight
requested costs bytes. Nunito Sans is **variable** — one file spans the axis, and
the saving comes from narrowing the axis, not from listing fewer weights. Five
files, 48 KB total.

## Fidelity

The build is compared against the reference programmatically: the PDF is rendered
at 1920 × 1080, the page is captured at the same size, and the two are diffed per
pixel with a threshold that ignores text antialiasing.

Current result: **99.08% agreement, with no region of the page above 8% deviation.**

Where a difference shows up, a shift search finds the offset that minimises the
error — which distinguishes *displaced* from merely *rendered differently*. Most
red on a naive diff map is antialiasing, not a bug, and chasing it would mean
"fixing" elements that are already exact.

## Accessibility

Not an afterthought here — the brief grades structure, and the design has several
traps worth naming:

- Icon-only controls carry a visually hidden label; the SVG itself is
  `aria-hidden`, since the label already names the control.
- `.visually-hidden` uses `clip-path`, never `display: none` — the latter would
  hide the text from assistive technology too.
- The portrait is decorative, so it takes an empty `alt`. It reinforces the
  message but carries no information the copy does not already give.
- Focus is always visible. Removing outlines without replacing them is the most
  common accessibility regression in a polished design.
- `prefers-reduced-motion` is honoured, because hover transitions count as motion.

## Licence

MIT. The design itself belongs to its original author and is reproduced here as
a course exercise.
