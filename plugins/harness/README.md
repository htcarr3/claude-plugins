# harness — Claude config self-improvement

The self-improvement layer. Skills that keep a project's Claude Code setup
**current by construction** — they fetch the latest docs and best practice, grade
this project against them, and apply the gaps. The design principle is
*procedural, not declarative*: the skills encode the **method and judgment** (how
to research, what to audit for, what "good" means as a rubric); the **facts** stay
on the web, fetched fresh each run. Nothing here bakes in a snapshot of the docs
that can rot.

The one durable thing it *does* encode is a **source registry**
(`references/sources.md`) — *where* to look, which ages far slower than *what's*
there. Bake the map, fetch the territory.

## The loop

| Skill    | Invoke              | Does                                                                                                          |
|----------|---------------------|---------------------------------------------------------------------------------------------------------------|
| Research | `/harness:research` | Fetch + synthesize current best practice on a topic into a dated brief. Grounds the other two.                |
| Audit    | `/harness:audit`    | Grade this project's config against freshly-researched practice → scored gap report.                          |
| Apply    | `/harness:apply`    | Implement the gaps (or a specific change) to current best practice, in atomic commits. *(User-invoked only.)* |

`research → audit → apply` is the intended flow, but each stands alone. Research
and audit can be offered proactively; apply mutates config, so it only runs when
you ask.

## How it uses subagents

The only work delegated to a subagent is the heavy, read-only sweep — walking a
whole config surface during an `audit` — handed to the built-in
`Explore`/`general-purpose` agent purely for context isolation: it returns a
distilled inventory, not a wall of file dumps. Everything with judgment in it
(grading, prioritizing, and all *writing* in `apply`) stays in the main context,
visible and reviewable — writing config is precisely the interactive work a
subagent should *not* hide. The plugin ships **no bespoke config agents of its
own**; that's deliberate — a maintained agent is one more thing that goes stale,
which is the failure mode this whole plugin exists to avoid. For quick "what's
the current answer for X feature" lookups, the built-in `claude-code-guide` agent
is the lighter tool.

## Keeping the registry alive

When a skill hits a moved/dead source or discovers a better one, it updates
`references/sources.md`. That file is the plugin's only maintenance surface — and
by design it changes rarely.
