# PhaseN Site Builder themes

CSS-only themes for [PhaseN Site Builder](https://github.com/PhaseN-Digital/phasen-sitebuilder-releases).
`clean`, `bold` and `elegant` are the themes built into the app, in the same
format as everything else here.

## Themes

| Theme | Look | Font |
|---|---|---|
| `clean` | Light and simple, suits most businesses (built in). | Modern |
| `bold` | Dark, big headings, strong colour (built in). | Modern |
| `elegant` | Warm, centred, serif type (built in). | Classic serif |
| `corporate` | Navy and slate, crisp and businesslike. | Modern |
| `trades` | Charcoal with a strong amber accent. | Modern |
| `cafe` | Warm cream and terracotta, serif headings. | Friendly |
| `clinic` | Calm teal, light and airy. | Modern |
| `professional` | Deep green and gold, classic serif type. | Classic serif |
| `studio` | Bold black and white, large type. | Helvetica / Arial |
| `community` | Friendly purple and orange, rounded shapes. | Rounded |
| `minimal` | Monochrome, quiet and spacious. | Modern |

All the added themes' default colours meet WCAG AA (4.5:1) for text on the
background and for button and banner text.

## Theme format

Each theme is a folder `themes/<id>/`. The id is lowercase letters, digits and
dashes (at most 40 characters) and is what sites store, so never rename one.

```
themes/<id>/
  theme.json    required
  theme.css     required
  preview.png   optional, a screenshot (PNG, at most 512 KB)
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

**CSS only.** Themes can't contain JavaScript or HTML templates, and the
checks reject:

- `@import` (of anything), `javascript:` URLs, `expression()`,
  `behavior:`/`-moz-binding`, and any `<` character
- a `theme.css` over 32 KB
- any file other than `theme.json`, `theme.css` and `preview.png`

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
together, in a PR to each repo.

## Licence

All themes in this repository are released under the [MIT licence](LICENSE). By submitting a theme (for example via a pull request from PhaseN Site Builder), you agree to license it under the same terms and confirm you have the right to do so.
