---
name: Blindspot Pass
description: Surface the unknown unknowns before starting unfamiliar work — what you don't know you don't know about a codebase area, domain, or task. Use when entering an unfamiliar module or problem domain, or when the user asks to find blindspots / unknown unknowns / "what am I missing" before diving in.
when_to_use: >-
  Starting work in an unfamiliar part of the codebase, an unfamiliar domain, or
  any task where the user says they "know nothing about" the area. Trigger
  phrases include "blindspot pass", "unknown unknowns", "what don't I know",
  "what am I missing".
argument-hint: "[what you're about to work on]"
---

# Blindspot Pass

The user is about to work on something they're unfamiliar with. Your job is to
surface their **unknown unknowns** — the risks, prior art, and gotchas they
don't even know to ask about — and teach them enough to prompt you well next
time. You are the expert scout here, not an order-taker.

Target: **$ARGUMENTS**

## First, calibrate to the user

Before searching, establish their starting point (ask briefly only if it isn't
already clear from context):

- How familiar are they with this codebase area / domain? (novice → expert)
- What are they ultimately trying to achieve?
- Where are they in their thinking — exploring, or committed to an approach?

Their answers set the depth and vocabulary of everything below.

## Then, do the pass

Search the codebase and, where useful, the web. Then report:

1. **The lay of the land** — how this area actually works, in plain language
   pitched to their stated familiarity. Build shared vocabulary.
2. **Prior art & history** — existing patterns, past attempts, conventions, and
   the load-bearing constraints they'd otherwise trip over.
3. **Potholes** — the specific failure modes, edge cases, and "everyone gets
   this wrong the first time" hazards. Be concrete and cite files.
4. **Questions they should be asking** — the ones a domain expert would ask that
   the user currently can't. This is the highest-value section.
5. **How to prompt me better next time** — the context and constraints they
   should hand me so I make fewer blind guesses.

Prefer a self-contained HTML artifact when the map is rich enough to benefit
from structure (sections, a checklist, a diagram): see
`${CLAUDE_PLUGIN_ROOT}/references/html-artifacts.md`. A short pass can stay
inline.

Do **not** start implementing. This is reconnaissance to shrink the unknowns
before work begins.
