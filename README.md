# htcarr3 — Claude Code plugins

A personal [plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces).
Workflow toolkits packaged as plugins so my global `~/.claude/` stays lean and I
pull each one into only the projects that want it.

## Use it

```bash
# Add this marketplace (local path while developing; a git URL once pushed)
claude plugin marketplace add ~/Code/Personal/claude-plugins

# Install a plugin (user scope = available everywhere)
claude plugin install unknowns@htcarr3

# Or per-project (writes to that repo's .claude/settings.json)
claude plugin install unknowns@htcarr3 --scope project
```

Per-project enablement lives in `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "htcarr3": { "source": { "source": "github", "repo": "htcarr3/claude-plugins" } }
  },
  "enabledPlugins": { "unknowns@htcarr3": true }
}
```

(Swap the `github` source for a local `directory` source while it stays local.)

## Plugins

| Plugin        | What it is                                                                                                                                   |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| `unknowns`    | Field-guide techniques for surfacing unknowns before/during/after implementation.                                                            |
| `harness`     | Self-improvement layer — research current best practice, audit this project's Claude config against it, apply the gaps.                      |
| `review-flow` | Branch review & session helpers — tour a branch, triage a cold-session review, replay messy history into clean commits.                      |
| `html-docs`   | Consistent self-contained HTML output — reports, plans, explainers, comparisons, dashboards, editors — on one Anthropic-style design system. |

## Conventions

- New plugins use the `skills/<name>/SKILL.md` layout (not legacy `commands/`).
- Validate before committing: `claude plugin validate .`
- Versions are left to the git commit SHA for fast iteration; set an explicit
  `version` in a plugin's `plugin.json` only for a stable release cadence.
