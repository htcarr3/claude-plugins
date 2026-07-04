---
name: Apply Config Improvements
description: Implement improvements to a project's Claude Code configuration — from a harness:audit report or a specific recommendation — writing/updating skills, agents, hooks, rules, settings, or CLAUDE.md to match current best practice, in reviewable atomic changes. Use when the user wants to act on an audit or make a concrete config change grounded in current docs.
when_to_use: >-
  After an audit, or when the user asks to change/add/fix part of their Claude
  configuration and wants it done to current best practice. Trigger phrases
  include "apply the audit", "implement these fixes", "update my config", "set up
  hooks/skills/agents for this project".
disable-model-invocation: true
argument-hint: "[what to apply — an audit, a specific gap, or a change to make]"
---

# Apply Config Improvements

This skill mutates the project's Claude configuration, so it runs only when the
user asks for it. The discipline: **change against current truth, in reviewable
pieces.** Scope: **$ARGUMENTS**

## 1. Confirm the target and the standard

- If applying a `harness:audit` report, work from its ranked gaps. If the audit
  is old, re-ground the specific items with `harness:research` before writing —
  don't implement against a stale recommendation.
- If applying a one-off change with no audit behind it, run `harness:research`
  (or fetch `${CLAUDE_PLUGIN_ROOT}/references/sources.md`) for that surface first,
  so the change reflects the current schema/fields/flags — this is exactly where
  a baked-in mental model writes yesterday's config.

## 2. Write it in-context, to the researched shape

Do the authoring here, in the main context — config changes are reviewable work
the user should watch happen, not something to hide inside a subagent (writing is
exactly the interactive work delegation shouldn't bury). Write each surface
(skills, agents, hooks, rules, settings, output styles, permissions, MCP) to the
shape the research established, and match the conventions already in the project
(naming, structure, `paths:` scoping, comment density) — read a couple of
existing examples before adding a new one.

## 3. Keep changes atomic and reviewable

- One coherent change per commit; don't bundle unrelated fixes.
- Never leave the project's Claude config in a broken state — validate what's
  validatable (`claude plugin validate` for a plugin, JSON parse for settings,
  and a quick read-back that skills/agents have well-formed frontmatter).
- For anything destructive or irreversible (deleting a skill, rewriting CLAUDE.md
  wholesale, changing permissions), show the diff and confirm before doing it.

## 4. Close the loop

Summarize what changed and why (tie each change to the audit item or research it
came from), and note anything you deliberately left for the user to decide.
Suggest re-running `harness:audit` to confirm the gaps closed. If you learned
something about *where* current guidance lives that the registry is missing,
update `${CLAUDE_PLUGIN_ROOT}/references/sources.md` — that's how the durable part
stays durable.
