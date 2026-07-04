# html-docs — consistent HTML output

Make Claude emit polished, **self-contained HTML documents** — reports, plans,
explainers, comparisons, dashboards, custom editors — instead of (or alongside)
chat text, all sharing one design system so every output looks like it came from
the same place. You decide *what* to put on the page; the page-building is solved.

Built to mirror Anthropic's ["unreasonable effectiveness of
HTML"](https://claude.com/blog/using-claude-code-the-unreasonable-effectiveness-of-html)
approach and the design language of Thariq's
[html-effectiveness gallery](https://github.com/thariqs/html-effectiveness) —
warm ivory paper, slate ink, clay accent, serif headings.

## What's in it

| File                          | Role                                                                                                                                                                              |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `assets/template.html`        | The canonical self-contained template — full design system inline + reusable tabs/copy JS. Doubles as a live component gallery. **Copy this; keep the `<style>`; fill the body.** |
| `references/design-system.md` | Tokens, palette, type, shape — the "what the values are".                                                                                                                         |
| `references/components.md`    | Index of every component → class → when to use.                                                                                                                                   |
| `references/archetypes.md`    | The ~7 document shapes (plan, report, review, explainer, comparison, editor) → component recipes.                                                                                 |
| `references/authoring.md`     | Self-contained / reproducible / responsive rules; where to write the file; what to defer.                                                                                         |
| `skills/html-doc/SKILL.md`    | The skill that ties it together.                                                                                                                                                  |

## Use it

Invoke `/html-docs:html-doc` (or just ask for "an HTML report/plan/dashboard" —
it's model-invocable). Claude picks an archetype, copies the template, and fills
in content using the documented components.

Open `assets/template.html` in a browser to see the whole component set rendered.

## Boundaries

Deliberately narrow. Charts/plots/dashboards → the `dataviz` skill. Deep design
fundamentals → the shipped `artifact-design` skill. This plugin owns the
*document shell + components*, nothing more.

## Re-theming

The whole palette is CSS custom properties in the template's `:root`. Swap those
values (e.g., for corporate brand colors) and every document re-themes — no other
edits. Dark mode is intentionally omitted to stay faithful to the source look.
