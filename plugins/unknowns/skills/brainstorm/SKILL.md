---
name: Brainstorm & Prototype
description: Explore several wildly different directions for a feature, design, or approach — often as a single self-contained HTML page of clickable mockups — to surface unknown knowns (taste you only recognize when you see it). Use at the start of a session to set scope, or when the user asks to brainstorm, see design directions, or prototype before wiring anything up.
when_to_use: >-
  Early exploration, ambiguous visual/UX work, or defining scope before
  committing. Trigger phrases include "brainstorm", "design directions", "mock
  this up", "show me options", "prototype before you build".
disable-model-invocation: true
argument-hint: "[what to explore]"
---

# Brainstorm & Prototype

Cheaply surface the user's **unknown knowns** — the criteria they can only
define once they see them — before implementation makes changes expensive to
undo. Subject: **$ARGUMENTS**

## Ground it first

Search the codebase / real data so the options are grounded, not generic. Note
the actual constraints you find; they shape what's viable.

## Then diverge, hard

Generate genuinely different directions, not variations on one idea. Span the
range from cheapest/safest to most ambitious. For each: a one-line thesis, what
it optimizes for, and the tradeoff it accepts.

- **For visual / UX work:** build a single self-contained HTML page mocking the
  directions side by side with fake data, so the user reacts to layout before
  any backend or state exists. Follow
  `${CLAUDE_PLUGIN_ROOT}/references/html-artifacts.md`.
- **For approach / architecture work:** a ranked list is usually enough; reach
  for HTML only if a diagram or comparison table earns it.

Cast the net slightly too wide on purpose — it's easier for the user to prune
than to imagine the option you didn't show. Explicitly call out any high-value
approach you suspect they hadn't considered.

End by asking which directions resonate. Don't build the real thing yet — this
is a probe.
