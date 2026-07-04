---
name: Comprehension Quiz
description: Produce an HTML report explaining what a change actually did — context, intuition, behavior tied to existing code paths — with a quiz at the bottom the user must pass before merging. Use after a substantial change when the user wants to genuinely understand it before merge, not just skim the diff.
when_to_use: >-
  After a long or complex working session, before merge. Trigger phrases include
  "quiz me", "make sure I understand this change", "explain what happened and
  test me".
disable-model-invocation: true
argument-hint: "[optional branch, PR, or scope of the change]"
---

# Comprehension Quiz

A diff shows *what lines changed*; it hides *what behavior changed*, because much
of the behavior lives in existing code paths the diff doesn't touch. Close that
gap before the user merges. Scope: **${ARGUMENTS:-the current change}**

## Build the report

Inspect the change (diff, touched files, and the code paths they flow through).
Then produce a **self-contained HTML report** — follow
`${CLAUDE_PLUGIN_ROOT}/references/html-artifacts.md` — containing:

1. **What was done** — the change in plain language.
2. **Why & intuition** — the reasoning, and the mental model needed to hold it.
3. **How it behaves at runtime** — how the new code interacts with existing
   paths, including the non-obvious ripple effects a diff won't show.
4. **Gotchas** — anything surprising, and any deviations logged during the build.

## Then the quiz

End the report with a short quiz (5–8 questions) targeting genuine
understanding, not trivia — the questions someone would fail if they only
skimmed the diff. Include an answer key in a collapsed/hidden section.

Tell the user to read it, take the quiz, and only merge once they pass it cleanly.
Offer to explain anything they miss.
