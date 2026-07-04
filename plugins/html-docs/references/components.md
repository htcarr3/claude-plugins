# Component index

Every component is already styled in `${CLAUDE_PLUGIN_ROOT}/assets/template.html`
and demonstrated in its body. Don't rebuild them — copy the markup and swap the
content. This index is the map; the template is the source of truth.

| Component          | Class(es)                                                     | Use for                                                                                                                |
|--------------------|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Doc header         | `.doc-head`, `.eyebrow`, `.doc-meta`                          | Title bar: eyebrow category, `<h1>`, date/repo on the right.                                                           |
| Prompt / repro box | `.prompt-box`                                                 | The originating prompt, for reproducibility. Put it near the top.                                                      |
| Section            | `.section`, `.sec-head`, `.num`                               | Numbered section with a serif `<h2>`. The backbone of most docs.                                                       |
| Grid               | `.grid` + `.grid-2/3/4`                                       | Responsive columns (collapse on narrow screens).                                                                       |
| Stat tile          | `.stat`, `.num-lg`, `.stat-label`, `.delta`                   | KPIs / metrics. `.stat--warn` adds a clay left border; `.delta.up/.down`.                                              |
| Panel / card       | `.panel`                                                      | Generic bordered surface for grouped content or a diagram.                                                             |
| Badge / pill       | `.pill` + `.pill--accent/success/warning/danger/info`         | Inline status labels; great inside tables.                                                                             |
| Callout            | `.callout` + `.callout--info/success/warning/danger`          | Set-apart note with a coloured left border and optional `.callout-title`.                                              |
| Table              | `table.data` (wrap in `.table-scroll`)                        | Tabular data; header is uppercase muted, rows hover ivory.                                                             |
| Code block         | `.code`, `.code-label`, `.kw/.str/.cm/.fn`                    | Reviewable snippet on slate with a file label. Hand-tag tokens for colour.                                             |
| Timeline           | `.timeline`, `.tl-item`, `.tl-date`, `.tl-dot`, `.tl-body`    | Chronology (deploys, incidents, milestones).                                                                           |
| Tabs               | `.tabs`, `.tab-btn[data-tab]`, `.tab-panel#id`                | Group dense content without a second page. JS included.                                                                |
| Buttons            | `.btn`, `.btn--primary`, `.btn--ghost`                        | Actions.                                                                                                               |
| Export bar         | `.export-bar` + `.btn[data-copy-text]` / `[data-copy-target]` | **Two-way interaction** — copy JSON / Markdown / a follow-up prompt to clipboard. JS included.                         |
| Open questions     | `.qa`, `.q`, `.a`                                             | Unresolved decisions with a lean/answer.                                                                               |
| SVG diagram        | inline `<svg>` (see template §07)                             | Data-flow / architecture. Use palette hexes: boxes `#fff`/`#D1CFC5`, accent box `#FBEAE1`/`#D97757`, arrows `#87867F`. |

## Two-way interaction (worth using)

The export bar is the highest-leverage pattern from the source material. When a
document holds structured data the reader might act on — chosen parameters, a
picked option, a triage decision — add a button that copies it as JSON or as a
ready-to-paste prompt, so the reader can feed it straight back into the next turn.
Populate `data-copy-text` with the payload, or `data-copy-target="#id"` to copy a
hidden element's text.
