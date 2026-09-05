# Sylva

A small, reusable **color system** — a forest-green and sage palette packaged as
[W3C design tokens](https://tr.designtokens.org/format/) for reuse across projects.

Colors only. No components, no framework.

```
tokens/
  primitives.tokens.json   Tier 1 — the raw palette (green, sage, neutral, feedback, base)
  semantic.tokens.json     Tier 2 — meaningful aliases (bg / text / border / action / feedback), per theme
brand/
  gnome.svg                The mascot — standalone pixel SVG
preview/
  index.html               Standalone reference page — every token, live light/dark, contrast checks
```

Open `preview/index.html` in a browser to browse the whole system.

## Two tiers

**Primitives** are raw values with no assigned meaning. Full `50–950` ramps for `green`
(brand) and `sage` (muted green-grey), Tailwind-compatible `neutral`, and `success` /
`warning` / `error` / `info`. The three primary greens (`#0d4d14`, `#1b5e20`, `#2e7d32`),
the accent (`#66bb6a`), and the original sage surface/border/muted values are preserved as
**anchor steps**; the rest of each ramp is generated around them.

**Semantic** tokens are what an application consumes. Every value is a reference such as
`{color.green.800}`. They're split into `light` and `dark` groups that mirror each other —
resolve one set per theme.

```jsonc
// semantic.tokens.json (excerpt)
"light": {
  "action": {
    "primary":      { "$value": "{color.green.800}" },
    "primary-text": { "$value": "{color.base.white}" }
  }
},
"dark": {
  "action": {
    "primary":      { "$value": "{color.green.400}" },
    "primary-text": { "$value": "{color.green.950}" }
  }
}
```

## Consuming it

The tokens are the source of truth. Generate whatever format a project needs with
[Style Dictionary](https://styledictionary.com) v4 (kept out of this repo on purpose):

```bash
npm i -D style-dictionary
```

```js
// sd.config.mjs
export default {
  source: ['tokens/**/*.tokens.json'],
  platforms: {
    css: {
      transformGroup: 'css',
      buildPath: 'dist/',
      files: [{ destination: 'sylva.css', format: 'css/variables' }],
    },
    ts: {
      transformGroup: 'js',
      buildPath: 'dist/',
      files: [{ destination: 'sylva.ts', format: 'javascript/es6' }],
    },
  },
};
```

```bash
npx style-dictionary build --config sd.config.mjs
```

Then map the semantic custom properties into a Tailwind v4 `@theme` block.

## Anchor reference

Steps that carry an exact value from the palette this system was lifted from:

| Token          | Sylva step  | Origin variable           |
| -------------- | ----------- | ------------------------- |
| brand accent   | `green.400`  | `--accent`                |
| primary hover  | `green.700`  | `--primary-hover`         |
| primary        | `green.800`  | `--primary`               |
| primary dark   | `green.900`  | `--primary-dark`          |
| tinted surface | `sage.200`   | `--surface-hover` (light) |
| muted text     | `sage.500`   | `--muted` (light)         |
| tinted border  | `sage.700`   | `--border` (dark)         |

## Brand mark

`brand/gnome.svg` is the gnome mascot — a 14×23 grid of flat `<rect>`s
(`shape-rendering="crispEdges"`), so it scales sharp to any size with no raster assets.
Its 14 colors are its own — hat reds, beard blue-greys, a moss-green collar — and sit
**outside** the token system; they're kept here so the whole identity lives in one repo.
Use it directly, or inline it as an SVG `<symbol>` (see `preview/index.html`).

## License

MIT — see [LICENSE](./LICENSE).
