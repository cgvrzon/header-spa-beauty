# GlowQueen — Header & Hero

Static header-hero for a spa and beauty business. First CSS deliverable of the
ConquerBlocks Full Stack master's programme.

> **Status:** in progress. The markup and the design tokens are done; layout and
> components are being built.

## The brief

Reproduce the provided design as a static page. Not responsive — it only has to
match the reference at its native size.

| Requirement | How it is met |
|---|---|
| Free choice of boilerplate | Hand-written, no framework |
| SASS recommended | Dart Sass with `@use`, partials split by responsibility |
| Static design | Fixed 1920 × 1080 composition |
| SVG icons | Inline SVG, so they inherit `currentColor` and react to hover |
| Semantic markup | Landmarks, labelled navigation, `figure`/`figcaption` quotes |
| Hover states | Every interactive element has one |
| Fidelity to the reference | Measurements taken from the source, not estimated (see below) |

## Running it

```bash
npm install
npm run dev      # watch mode, expanded output
npm run build    # single compressed build
```

Then open `index.html`. There is no dev server: the page is static and loads its
stylesheet from `dist/css/main.css`.

## Structure

```
index.html                 the whole page - one header, one hero
src/scss/
  main.scss                the only entry point Sass compiles
  abstracts/_variables.scss  design tokens: colour, type, spacing, shape
  base/_reset.scss           small deliberate reset + a11y helpers
  base/_typography.scss      page-wide type defaults
dist/css/main.css          build output
assets/{img,icons}/        exported design assets
```

Everything under `src/scss/` other than `main.scss` is a partial (leading
underscore) and never produces a file of its own.

## Measurements

The design values in `_variables.scss` are **measured, not guessed**. The
reference PDF is 1920 × 1080 pt, so rendering it at 72 dpi gives 1920 × 1080 px
— a one-to-one match with CSS pixels. Colours were sampled per pixel; distances
were read by scanning for colour transitions along rows and columns.

Key values found that way:

| What | Value |
|---|---|
| Left gutter (shared by logo and headline) | 162px |
| Background band split | y = 820, constant edge to edge |
| Mint panel | x 1285 → 1920 (635px wide) × 820 tall |
| Headline leading | 80px |
| Navigation gap | 60px |

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
