---
name: Audit Claude Setup
description: Grade this project's Claude Code configuration (CLAUDE.md, .claude/ skills/agents/hooks/settings/rules, MCP, permissions) against freshly-researched current best practice, producing a scored gap report with concrete fixes. Use when the user wants to know how good their Claude setup is or where it's fallen behind current guidance.
when_to_use: >-
  The user wants their project's Claude configuration reviewed, scored, or
  brought up to date — or you're about to recommend config changes and need a
  grounded baseline. Trigger phrases include "audit my setup", "how good is my
  Claude config", "what's my harness missing", "grade my .claude".
argument-hint: "[optional dimension to focus on — e.g. 'skills' or 'hooks'; defaults to the whole config]"
---

# Audit Claude Setup

An audit is only worth as much as the yardstick behind it — and a yardstick baked
into a skill goes stale. So this audit grades against **freshly researched**
practice, never against remembered "shoulds". Scope: **${ARGUMENTS:-the whole Claude configuration}**

## 1. Inventory what exists

Read the project's actual Claude surface. Don't assume the layout — discover it:

- `CLAUDE.md` (and any nested/imported ones), `.claude/settings.json` and
  `settings.local.json`, `.claude/rules/`, `.claude/skills/`, `.claude/agents/`,
  `.claude/hooks/` (and hook config), `.mcp.json`, output styles, permissions.
- Note what's present, what's conspicuously absent, and anything that looks
  hand-rolled where a first-class feature now exists.

## 2. Ground the yardstick

For each dimension you're auditing, get the *current* best practice before
judging it. Either reuse a recent `harness:research` brief, or run research now —
invoke `harness:research` (or fetch `${CLAUDE_PLUGIN_ROOT}/references/sources.md`
directly) for the dimensions in scope. **Do not grade from memory.** The whole
point is that "what good looks like" is fetched, not assumed.

## 3. Isolate the heavy read, keep the judgment

Inventorying a whole config surface is a read-heavy sweep — worth isolating so it
doesn't bloat the main context. Spawn the built-in `Explore` (or
`general-purpose`) agent to gather the raw inventory, handed the specific list of
surfaces to walk and the freshly researched standard to check against, and have
it return a distilled findings list rather than file dumps. Keep the grading and
prioritization here, in the main context, where you can weigh it against what you
know about the project — delegate the *reading*, never the judgment.

## 4. Report

Produce a scored gap report — a self-contained HTML artifact works well for a
scannable scorecard, else Markdown:

1. **Score per dimension** — a simple rubric (e.g., strong / adequate / gap /
   absent) across the surfaces that apply.
2. **Gaps, ranked by leverage** — for each: what's there now, what current
   practice recommends (cited to the research), and the concrete fix. Lead with
   the changes that most improve Claude's effectiveness on *this* project.
3. **Stale spots** — config that reflects an older approach the docs have since
   moved past. These are the highest-value finds; call them out explicitly.
4. **Version stamp** — the CLI version and fetch date the audit is true for.

End by offering `harness:apply` to implement the ranked gaps. Report the state
honestly — don't inflate a thin config into a passing grade.
