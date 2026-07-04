---
name: Plan by Uncertainty
description: Write an implementation plan led by the decisions most likely to change — data models, type interfaces, UX flows — with mechanical refactoring at the bottom, so the user reviews what actually needs their input. Use when the user asks for an implementation plan and you want the risky or ambiguous decisions surfaced first.
when_to_use: >-
  The user is ready to implement and wants a plan to review. Trigger phrases
  include "implementation plan", "plan this out", "how would you build this".
argument-hint: "[what to plan]"
---

# Plan by Uncertainty

Write a plan ordered by **uncertainty, not execution order**. A plan sorted by
build sequence buries the decisions the user most needs to weigh in on under
mechanical steps they'd rubber-stamp. Invert that. Subject: **$ARGUMENTS**

## Structure

Lead with, and spend most of the plan on, the parts most likely to change:

1. **Data model changes** — new tables/columns/relations, migrations, anything
   with persistence consequences.
2. **Type interfaces & contracts** — API shapes, function signatures, the seams
   other code will depend on.
3. **User-facing flows** — UX decisions, states, edge-case behavior the user has
   taste about.

For each, flag the open decision and your recommended default, so the user can
correct you before it's expensive.

Then, briefly, at the bottom: **the mechanical work** — the refactors and wiring
you don't need sign-off on. One or two lines; say "I trust myself on this part."

## Surface remaining unknowns

Call out edge cases you expect to hit but can't resolve without writing code —
these are the unknown unknowns most likely to force a mid-build pivot. Suggest
`deviation-log` for the implementation session so they're captured.

Render as a self-contained HTML artifact when the plan is substantial
(`${CLAUDE_PLUGIN_ROOT}/references/html-artifacts.md`); keep short plans inline.
Produce the plan for review — don't start implementing.
