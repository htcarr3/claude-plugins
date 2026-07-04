---
name: Pitch & Explainer
description: Package a finished change (spec, prototype, implementation notes, demo) into a single shareable pitch/explainer doc that accelerates buy-in — leading with the demo and pre-answering the objections a reviewer would raise. Use when the user wants a doc to drop in Slack or send to get approvals.
when_to_use: >-
  After shipping something, to get buy-in or approvals. Trigger phrases include
  "pitch", "write this up for the team", "doc I can drop in Slack", "get buy-in".
disable-model-invocation: true
argument-hint: "[what to package, and the audience]"
---

# Pitch & Explainer

Getting a change approved is its own step. Reviewers start with the same
unknowns the user did — and they'll want proof the user accounted for the
failure modes they'd have worried about. This doc closes both gaps. Subject:
**$ARGUMENTS**

## Gather the material

Pull together whatever exists: the spec, the prototype/mockup, the
implementation notes and their deviations, and a demo (GIF/screenshot/short
clip) if there is one. Ask the user for anything missing rather than inventing it.

## Assemble the doc

Produce a single self-contained doc — a **self-contained HTML artifact** is
usually best for sharing (`${CLAUDE_PLUGIN_ROOT}/references/html-artifacts.md`),
though match whatever format the target audience actually reads:

1. **Lead with the demo.** Show it working first — the GIF or screenshot up top.
2. **The problem & the shape of the solution** — pitched to a reader who wasn't
   in the weeds. Bring them up to your level of understanding fast.
3. **Key decisions & tradeoffs** — the forks you hit and why you chose as you
   did. Surface the deviations honestly.
4. **Preempt the objections** — name the concerns an expert reviewer would
   raise and answer them here, so approval doesn't stall on a round-trip.

Keep it scannable — bullets over paragraphs, the demo doing the heavy lifting.
Optimize for a fast yes from someone who wasn't there.
