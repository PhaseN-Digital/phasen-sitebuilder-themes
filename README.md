# PhaseN Site Builder themes

CSS-only themes for [PhaseN Site Builder](https://github.com/PhaseN-Digital/phasen-sitebuilder-releases).
`clean`, `bold` and `elegant` are the themes built into the app, in the same
format as everything else here.

## Themes

| Theme | Look | Font | Previews |
|---|---|---|---|
| `clean` | Light and simple, suits most businesses (built in). | Modern | [home](previews/clean/home.png), [all layouts](previews/clean/) |
| `bold` | Dark, big headings, strong colour (built in). | Modern | [home](previews/bold/home.png), [all layouts](previews/bold/) |
| `elegant` | Warm, centred, serif type (built in). | Classic serif | [home](previews/elegant/home.png), [all layouts](previews/elegant/) |
| `corporate` | Navy and slate, crisp and businesslike. | Modern | [home](previews/corporate/home.png), [all layouts](previews/corporate/) |
| `trades` | Charcoal with a strong amber accent. | Modern | [home](previews/trades/home.png), [all layouts](previews/trades/) |
| `cafe` | Warm cream and terracotta, serif headings. | Friendly | [home](previews/cafe/home.png), [all layouts](previews/cafe/) |
| `clinic` | Calm teal, light and airy. | Modern | [home](previews/clinic/home.png), [all layouts](previews/clinic/) |
| `professional` | Deep green and gold, classic serif type. | Classic serif | [home](previews/professional/home.png), [all layouts](previews/professional/) |
| `studio` | Bold black and white, large type. | Helvetica / Arial | [home](previews/studio/home.png), [all layouts](previews/studio/) |
| `community` | Friendly purple and orange, rounded shapes. | Rounded | [home](previews/community/home.png), [all layouts](previews/community/) |
| `minimal` | Monochrome, quiet and spacious. | Modern | [home](previews/minimal/home.png), [all layouts](previews/minimal/) |

Every theme styles all eight page layouts (`home`, `standard`, `landing`,
`services`, `contact`, `blog-post`, `blog-index`, `legal`). With each theme's
default colours, all text on every layout meets WCAG AA (4.5:1), checked on
the pages the app builds, at desktop width and at 390px with the mobile menu
open. `previews/<theme>/<layout>.png` are 640x400 screenshots of each layout
rendered by the app; they live outside `themes/` because a theme folder may
only hold the files below.

## Theme format

Each theme is a folder `themes/<id>/`. The id is lowercase letters, digits and
dashes (at most 40 characters) and is what sites store, so never rename one.

```
themes/<id>/
  theme.json    required
  theme.css     required
  preview.png   optional, a screenshot (PNG, at most 512 KB)
  preview-<layout>.png  optional, a screenshot of one page layout
                (e.g. preview-services.png; same limits)
  header.html   optional HTML templates (see "HTML templates" below)
  footer.html
  section.html
  layout-<id>.html
```

Nothing else is allowed in the folder.

### theme.json

```json
{
  "label": "Bold",
  "about": "Dark, big headings, strong colour.",
  "font": "sans",
  "colors": { "background": "#0f172a", "text": "#f1f5f9", "primary": "#7c3aed", "accent": "#22d3ee" },
  "version": "1.0.0"
}
```

| Field | Required | Meaning |
|---|---|---|
| `label` | yes | Name shown in the app (at most 200 characters). |
| `about` | yes | One line describing it (at most 200 characters). |
| `font` | yes | A built-in font id (`sans`, `serif`, `humanist`, `rounded`) or a CSS font stack such as `"Courier New", monospace`. Letters, digits, spaces, commas, quotes and dashes only. Use fonts people already have installed: themes can't load web fonts. |
| `colors` | yes | Default `background`, `text`, `primary` (header/banner) and `accent` (buttons) colours, each `#rrggbb`. Users can change them afterwards. |
| `version` | no | `major.minor.patch`. Bump it when you change the theme. |
| `layouts` | no | Notes per page layout, e.g. `{ "home": { "notes": "Taller banner." } }`. Keys must be layout ids (below); each value may only have `notes`, a string up to 500 characters. |

### theme.css

Small overrides on top of the app's own base stylesheet, which lays out every
page. The app puts these variables first, from the user's colour and font
choices, so use them rather than hard-coded colours:

`--background`, `--text`, `--primary`, `--accent`, `--primary-text` and
`--accent-text` (black or white, whichever reads on primary/accent), `--font`,
and `--radius` (corner rounding; set it in `:root`).

The main classes you can style: `.site-header` (with `.brand` and `nav a`),
`.hero` (`.has-image` when it has a photo), `.split` and `.image-left`
(text & image sections), `.richtext`, `.gallery-grid`, `.form`, `.button`,
`.site-footer`, `.notfound` (404 page), `.cookie-banner`, and `.wrap` (the
centred content column). See `themes/bold/theme.css` for a short example.

Every page has a layout, set as a class on `<body>`: `.layout-home`,
`.layout-standard`, `.layout-landing`, `.layout-services`, `.layout-contact`,
`.layout-blog-post`, `.layout-blog-index` and `.layout-legal`. Prefix rules
with one to restyle just that kind of page, e.g. `.layout-home .hero { … }`.
The app's base stylesheet already styles every layout, so these are optional.
A `.layout-<id>` class with any other id is rejected.

### HTML templates

Optional, at most 16 KB each: `header.html`, `footer.html`, `section.html`
(wraps every section) and `layout-<id>.html` (the inside of `<main>` for that
layout id). A theme without them uses the app's built-in markup. They may only
use `{{name}}` placeholders and `{{#if name}} … {{/if}}`; `{{menu}}` (header),
`{{footer.links}}` (footer) and `{{content}}` (section, layouts) are required.
The placeholders, elements and attributes allowed in each are listed in
`TEMPLATES`, `TAGS` and `ATTRS` in `scripts/theme-validate.js`. The checks
reject scripts, `<style>`, event handlers, forms, `javascript:`/`data:` URLs,
external resources (except https images from `IMAGE_HOSTS`) and iframes other
than YouTube (nocookie) and Vimeo players.

**No JavaScript.** The checks also reject:

- `.layout-<id>` selectors, or `layouts` keys in theme.json, naming an
  unknown layout
- `@import` (of anything), `javascript:` URLs, `expression()`,
  `behavior:`/`-moz-binding`, and any `<` character
- a `theme.css` over 32 KB
- any file other than `theme.json`, `theme.css`, `preview.png`,
  `preview-<layout>.png` (a known layout id) and the templates above

## Contributing

1. Fork this repo and add `themes/<your-id>/` with `theme.json` and
   `theme.css` (and a `preview.png` if you like).
2. Check it: `node scripts/theme-validate.js themes` (Node 18+, no install).
3. Try it in the app if you can, with a few page types (banner, text &
   image, gallery, form), and both light and dark colours.
4. Open a pull request. CI runs the same check on every PR. Commit messages
   use [Conventional Commits](https://www.conventionalcommits.org/), e.g.
   `feat: add mono theme`.

Nothing is committed straight to `main`; every change goes through a PR.

## The validator

`scripts/theme-validate.js` is a vendored copy of `lib/theme-validate.js`
from the app's (private) repo, which uses the same code to check themes
before loading them. **The two copies must stay identical**: change both
together, in a PR to each repo. It is self-contained (Node stdlib only), and
carries its own copy of the app's layout ids (`LAYOUT_IDS`); the app's tests
check that list matches the app's layouts.

To re-sync after the app's copy changes, from a checkout of the app repo:

```
cp lib/theme-validate.js ../phasen-sitebuilder-themes/scripts/theme-validate.js
```

then run `node scripts/theme-validate.js themes` here and open a PR.

## Licence

All themes in this repository are released under the [MIT licence](LICENSE). By submitting a theme (for example via a pull request from PhaseN Site Builder), you agree to license it under the same terms and confirm you have the right to do so.
