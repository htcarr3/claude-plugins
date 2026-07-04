# Archetype playbook

The ~7 document shapes from Thariq's gallery, each mapped to the components that
build it. Pick the closest archetype, take its recipe, fill in real content. These
are starting points, not straitjackets — combine freely.

## 1. Implementation plan
*When:* proposing how to build something, for review before coding.
Header (eyebrow "Implementation plan") → prompt box → a `.grid-4` of stat tiles
(scope: N files, M phases) → numbered `.section`s per phase → **code blocks** for
the signatures/snippets worth reviewing → a risk `table.data` with severity pills
→ `.qa` open questions. Lead with the decisions that need input (pairs well with
the `unknowns:plan-unknowns` skill). Reviewable code snippets are the point.

## 2. Status / progress report
*When:* summarizing where work stands (sprint, project, rollout).
Header with date + repo → `.grid-4` stat tiles (the KPIs) → a `.timeline` of what
happened → a `table.data` of workstreams with status pills → callouts for risks →
export bar ("copy as prompt" to continue). This is what `assets/template.html`
demonstrates end-to-end.

## 3. Incident report
*When:* documenting an outage/regression.
Header (eyebrow "Incident", severity pill) → summary panel (impact, duration,
scope) → `.timeline` of detection → mitigation → resolution → "Root cause"
`.section` with a code block or SVG → "Action items" `table.data` → `.qa`.

## 4. Code review / PR writeup
*When:* explaining a change to reviewers.
Header → summary stats (files, +/− lines) → per-area `.section`s → **code blocks**
with before/after and inline `.callout`s for annotations → an SVG module/flow
diagram if structure changed → risk pills. Pairs with `review-flow:tour-branch`.

## 5. Research / concept explainer
*When:* teaching an idea or synthesizing sources.
Header → intro panel → `.section`s that build the concept progressively → **SVG
illustrations** (the differentiator — draw the idea) → `.callout`s for key
takeaways → tabs for parallel cases. Prose stays tight; the visuals carry load.
For charts specifically, use the `dataviz` skill.

## 6. Comparison / exploration grid
*When:* laying out N options to choose between.
Header → a `.grid-2` or `.grid-3` of `.panel`s, one per option, each with a title,
a mock/snippet, and pros/cons as pills → a recommendation callout → an export
button that copies the chosen option as a prompt. "Generate N distinct approaches
side by side so I can compare" is the canonical request.

## 7. Custom editor / interactive tool
*When:* the reader needs to *do* something (triage, tune, reorder, toggle).
Header → the interactive surface (form controls, checkboxes, sliders, drag lists)
→ a live-updating summary → a prominent **export bar** ("copy diff", "copy as
JSON"). This is the most interactive archetype; keep all state client-side in the
included script. Build only the controls the task needs.

---

**Cross-references:** deep visual/design polish → `artifact-design`; any chart,
plot, or dashboard viz → `dataviz`; the self-contained/output rules → `authoring.md`.
