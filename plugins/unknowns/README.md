# Unknowns Field Guide

Skills for surfacing your **unknowns** — the gap between what you told Claude
(the map) and the actual codebase and constraints (the territory) — before,
during, and after implementation. Adapted from Thariq's *Field Guide to Fable:
Finding Your Unknowns*.

You don't run every technique every time. This is a toolbox you reach for when
ambiguity or unfamiliarity makes a guess costly.

## Skills

Invoke as `/unknowns:<skill>`.

| Skill                     | Phase  | What it does                                                                                             |
|---------------------------|--------|----------------------------------------------------------------------------------------------------------|
| `/unknowns:blindspot`     | Pre    | Blindspot pass over an unfamiliar area — finds unknown unknowns and teaches you enough to prompt better. |
| `/unknowns:interview`     | Pre    | Interviews you one question at a time, prioritizing answers that change the architecture.                |
| `/unknowns:brainstorm`    | Pre    | Several wildly different directions / clickable HTML mockups to react to.                                |
| `/unknowns:plan-unknowns` | Pre    | Implementation plan led by the decisions most likely to change, mechanics at the bottom.                 |
| `/unknowns:deviation-log` | During | Keeps `implementation-notes.md` logging where the code forced a departure from plan.                     |
| `/unknowns:quiz`          | Post   | HTML report on what a change actually did + a quiz you must pass before merging.                         |
| `/unknowns:pitch`         | Post   | Packages spec + prototype + notes + demo into one shareable buy-in doc.                                  |

## Invocation model

`blindspot`, `plan-unknowns`, and `deviation-log` can also be offered by Claude
automatically when the moment fits. The deliberate, output-heavy moves —
`interview`, `brainstorm`, `quiz`, `pitch` — are **user-invoked only**
(`disable-model-invocation: true`); you decide when to run them.

## Optional: Unknowns Mode

The plugin also ships an `unknowns-mode` output style — an always-on disposition
that surfaces unknowns before ambiguous work and keeps deviation logs by default.
Turn it on with `/output-style unknowns-mode`. It's opt-in; the skills above work
without it.

## HTML artifacts

Skills that produce something to react to emit a self-contained HTML page — a
live artifact on Claude Desktop/Web, or a written `.html` file in the terminal.
See `references/html-artifacts.md`.
