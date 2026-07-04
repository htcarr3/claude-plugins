---
name: Research Current Practice
description: Fetch and synthesize the *current* state of Claude Code / Claude best practice on a topic from live docs and trusted sources, producing a dated brief. Use before advising on or changing a project's Claude setup, or whenever the user asks what the latest/recommended approach is — so the answer is grounded in fresh docs, not stale memory.
when_to_use: >-
  Before making any claim about how Claude Code, the Agent SDK, or Claude models
  should be configured or used — and any time the user asks "what's the current
  best way to…", "what does the latest doc say", or "is this still the
  recommended approach". Trigger phrases include "research", "check the latest",
  "what's current", "look up the docs".
argument-hint: "[topic — e.g. 'skill frontmatter fields' or 'agent context best practices']"
---

# Research Current Practice

The failure mode this skill exists to prevent: answering from remembered
knowledge that has since gone stale. Docs, defaults, and best practice for Claude
Code and Claude models change on the order of weeks. So the rule here is simple —
**fetch, don't recall.** Topic: **$ARGUMENTS**

## Method

1. **Establish local ground truth first.** Run `claude --version` and the
   relevant `--help` output so the brief is anchored to the version actually
   installed, not the latest release in the abstract.
2. **Consult the source registry** — `${CLAUDE_PLUGIN_ROOT}/references/sources.md`
   — and fetch the sources relevant to the topic, in its priority order (local →
   official docs + changelog → engineering writing → practitioners). Use
   WebSearch to find the right pages and WebFetch to read them. Prefer the most
   recently dated material and note when something looks stale.
3. **Cross-check.** Where a practitioner or blog post disagrees with the official
   docs, say so and weight the official source higher unless the docs are visibly
   behind (the changelog is the tiebreaker for "what's newest").

## Produce the brief

Synthesize — don't dump. A good brief leads with the answer and is honest about
its own shelf life:

1. **Bottom line** — the current recommended approach, stated plainly.
2. **What changed / what's new** — if this differs from a common older approach,
   name the delta. This is the highest-value part, because it's what a stale
   config or a stale mental model gets wrong.
3. **Specifics** — the concrete fields, flags, settings, or steps, each tied to
   the source you got it from (link it).
4. **Caveats & unknowns** — anything the docs left ambiguous, anything you
   couldn't verify, any dead/moved source (which should get fixed in
   `sources.md`).

Stamp the brief with **the date you fetched** and **the CLI version** it's true
for, so its staleness is visible later. Keep it to a scannable page — render a
self-contained HTML artifact if the topic is broad enough to warrant one,
otherwise inline Markdown.

This brief is the grounding input for `harness:audit` and `harness:apply`. When
either of those runs, it should either reuse a fresh brief or invoke this skill
first — never advise from memory.
