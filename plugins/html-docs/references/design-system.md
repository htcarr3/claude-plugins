# Design system

The look is Anthropic's "html-effectiveness" palette (Thariq's gallery): a warm
single **light** theme — ivory paper, slate ink, clay accent, serif headings over
a system sans body. Every token lives in the `:root` block of
`${CLAUDE_PLUGIN_ROOT}/assets/template.html`; re-theme by editing that block only.

## Palette

| Token        | Value     | Role                                    |
|--------------|-----------|-----------------------------------------|
| `--ivory`    | `#FAF9F5` | page background                         |
| `--white`    | `#FFFFFF` | surfaces (panels, cards, tables)        |
| `--slate`    | `#141413` | ink for headings; code-block background |
| `--gray-700` | `#3D3D3A` | body text                               |
| `--gray-500` | `#87867F` | muted / secondary text                  |
| `--gray-300` | `#D1CFC5` | borders (used at 1.5px)                 |
| `--gray-100` | `#F0EEE6` | soft fills, table headers               |
| `--clay`     | `#D97757` | accent / primary (Anthropic terracotta) |
| `--oat`      | `#E3DACC` | section-number chips, tags              |
| `--olive`    | `#788C5D` | success                                 |
| `--amber`    | `#C78E3F` | warning                                 |
| `--rust`     | `#B04A3F` | danger                                  |
| `--azure`    | `#5C7CA3` | info                                    |

Semantic pills/callouts use tinted backgrounds derived from these (see the
`.pill--*` and `.callout--*` classes).

## Type

- **Serif** (`ui-serif, Georgia`) for `h1`/`h2` and big stat numbers, weight 500,
  slight negative tracking. This serif is the signature of the look — keep it.
- **Sans** (`system-ui`) for body and `h3`, 16px / 1.55.
- **Mono** (`ui-monospace, SF Mono`) for code, dates, repo slugs, deltas.

Scale: display 44–48px · h1 34px · h2 24px · h3 18px · body 16px · small 14px ·
caption/label 11–12px (uppercase, letter-spaced).

## Shape & depth

- Radii: `--r-row: 8px` (buttons/inputs), `--r-panel: 12px` (cards/tables),
  `--r-pill: 999px` (badges). Borders are **1.5px** solid `--gray-300`.
- Shadows are subtle and warm (`rgba(20,20,19,…)`): `--shadow-sm` on panels,
  larger only for overlays. Prefer border over shadow for structure.
- Spacing scale is 4·8·12·16·24·32·48·64 (`--sp-1`…`--sp-8`). Compose with it.

## Extending

Add new components with the existing tokens — never hard-code a hex. If a genuinely
new colour is needed, add it to `:root` first. For deep design decisions beyond
this system, defer to the shipped **`artifact-design`** skill; for charts, defer to
**`dataviz`** (both noted in `authoring.md`). Dark mode is intentionally out of
scope to stay faithful to the source aesthetic; if ever needed, add a
`@media (prefers-color-scheme: dark)` override that remaps the semantic tokens.
