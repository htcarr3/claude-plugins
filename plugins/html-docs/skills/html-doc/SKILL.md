---
name: html-doc
description: Produce a polished, self-contained HTML document — status report, implementation plan, incident report, code-review writeup, research explainer, comparison grid, or interactive editor — using a consistent Anthropic-style design system, so you decide what to say and never how to build the page.
when_to_use: >-
  When the output would be richer as a viewable/shareable HTML file than as chat
  text — anything dense, structured, visual, or interactive: reports, plans,
  explainers, side-by-side comparisons, dashboards, custom editors. Trigger
  phrases include "make an HTML file/doc", "as an HTML artifact", "write this up
  as a page", "build me a dashboard/report/plan I can open", "compare these side
  by side".
argument-hint: "[what the document should cover]"
allowed-tools: Read, Write, Bash(mkdir *), Bash(open *), Bash(ls *)
---

# HTML document

Produce a self-contained HTML document instead of (or alongside) a chat answer,
when the content is dense, structured, visual, or interactive enough to warrant
it. The design system is already built — your job is the *content*, not the page.
Subject: **$ARGUMENTS**

Why this beats chat text: denser information (tables, SVG, layout), readable at
scale, shareable as a file/link, and optionally two-way (export buttons that copy
data back into a prompt). Prefer a few focused documents over one monolith.

## Steps

1. **Pick the archetype.** Match the request to the closest shape in
   `${CLAUDE_PLUGIN_ROOT}/references/archetypes.md` (plan, status/incident report,
   code review, explainer, comparison grid, interactive editor). Combine freely.

2. **Start from the template.** Copy `${CLAUDE_PLUGIN_ROOT}/assets/template.html`
   and keep its `<style>` and `<script>` blocks **verbatim** — that's the whole
   design system and the reusable tabs/copy interactivity. Build the `<body>` from
   the documented components (`${CLAUDE_PLUGIN_ROOT}/references/components.md`);
   never hand-roll CSS or use off-palette colours.

3. **Fill in real content.** Replace the demo body with the actual document per the
   archetype's recipe. Put the originating prompt in the leading HTML comment and
   the `.prompt-box` for reproducibility. Use stat tiles, tables, callouts, code
   blocks, timelines, and SVG where they earn their place — cut sections that don't.

4. **Add two-way interaction when it helps.** If the document holds data the reader
   might act on (a chosen option, tuned parameters, a triage decision), add an
   export-bar button that copies it as JSON or a ready-to-paste prompt.

5. **Follow the authoring rules** in `${CLAUDE_PLUGIN_ROOT}/references/authoring.md`
   — self-contained, responsive, reproducible.

6. **Write it where it can be seen.**
   - **Terminal:** write to `./artifacts/<name>.html` (create the dir; suggest
     gitignoring it), report the path, and offer to `open` it.
   - **Desktop / Web:** render it via the Artifact tool so the user sees it live;
     load the `artifact-design` skill first for anything design-heavy.

## Boundaries

- **Charts, plots, dashboards:** load the `dataviz` skill and follow it for the
  visualization itself — this skill styles the surrounding document, not the chart.
- **Deep design work:** defer to the shipped `artifact-design` skill.
- Keep the design system faithful: serif headings, system-sans body, clay accent,
  ivory paper. Extend via `:root` tokens, never ad-hoc styles.
