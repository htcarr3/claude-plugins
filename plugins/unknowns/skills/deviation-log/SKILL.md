---
name: Deviation Log
description: Keep a running implementation-notes.md deviation log during a build, recording where the code forced a departure from the plan and the conservative choice taken, so the next attempt is smarter. Use when starting a non-trivial implementation, or when the user asks to keep a deviation log or implementation notes.
when_to_use: >-
  Beginning or midway through a non-trivial implementation, especially one
  working from a spec or prototype. Trigger phrases include "keep a deviation
  log", "implementation notes", "track your decisions as you go".
argument-hint: "[optional path; defaults to implementation-notes.md]"
---

# Deviation Log

No amount of planning removes every unknown unknown — some only appear once the
code fights back. Capture them as you go, so a second pass (or the user's review)
starts from what you learned, not from scratch.

## Set up the log

Create `${ARGUMENTS:-implementation-notes.md}` at the repo root if it doesn't
exist, with sections: **Plan** (what you set out to do), **Deviations**, and
**Open questions**. Suggest gitignoring it unless the user wants it committed.

## While implementing

When the code forces you off the plan — an edge case, a wrong assumption, a
constraint you didn't know about:

1. **Take the conservative option** — the one easiest to revisit later — unless
   the user has said otherwise.
2. **Log it under Deviations**: what you expected, what you actually found, the
   choice you made, and why. One tight entry.
3. **Keep going.** Don't stall the build to escalate every small fork; the log
   is how the user reviews these in a batch afterward.

Escalate immediately only for a deviation that invalidates the plan's premise or
is genuinely irreversible.

## After

Summarize the Deviations so the user can see where reality differed from the map
— that delta is the input to the next iteration, and often to a `quiz` or
`pitch`.
