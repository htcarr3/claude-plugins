# CLAUDE.md

Guidance for Claude Code when working in this repository.

## What this is

`htcarr3` — a personal Claude Code **plugin marketplace**, not an application. It
packages workflow toolkits as plugins so the global `~/.claude/` stays lean and
each project pulls in only what it wants. See [README.md](README.md) for installation
steps and the plugin catalog. Currently, **local-only** (the marketplace is added
by directory path); switch to a `github` source to share it.

There is no build, no test suite, and no runtime here — the "code" is Markdown
(`SKILL.md`), JSON manifests, and the occasional self-contained HTML/CSS asset.

## Layout

```
.claude-plugin/marketplace.json     # lists every plugin: name, ./plugins/<name>, description
plugins/<name>/
  .claude-plugin/plugin.json        # manifest: name, displayName, version, keywords
  skills/<skill>/SKILL.md           # one dir per skill; the dir name IS the /command
  references/*.md                   # supporting docs a skill reads on demand
  assets/                           # optional
  README.md
```

## Workflow

- **Validate before committing:** `claude plugin validate plugins/<name>` (or `.`
  for the marketplace). A red validate is not shippable.
- **See a change live:** `claude plugin marketplace update htcarr3`, then
  `claude plugin install <name>@htcarr3` (add `--scope project` to pin it to one repo).
- **Registering a new plugin** means adding it to `marketplace.json` with an
  explicit `"source": "./plugins/<name>"`.

## Conventions

- **Authoring is procedural, not declarative.** Don't bake doc snapshots or schema
  tables that go stale into a skill. Encode the *method* of fetching current facts
  and leave the facts on the web.
- **Verify skill / plugin authoring against live docs** (`code.claude.com/docs`),
  not memory — frontmatter fields and behaviour drift.
- **`skills/<name>/SKILL.md` layout only** — not the legacy `commands/`. The
  directory name is the slash command; frontmatter `name:` is a display label.
- **Invocation mode is a deliberate choice.** Mark a skill
  `disable-model-invocation: true` when it has side effects the user should watch
  (rewrites history, produces a deliverable, changes config); leave it
  model-invocable when it mostly reads and reasons.
- **Versioning by git SHA.** Set an explicit `version` in `plugin.json` only for a
  stable release; otherwise iterate freely.
- **Don't migrate Anthropic built-ins.** Skills Anthropic ships (skill-creator,
  agent-creator, …) stay built-in; this marketplace is for personal toolkits.
- **Generated HTML artifacts** land in a gitignored `./artifacts/`, never committed.

## Commits

Conventional Commits, scoped to the plugin: `feat(dhh): …`, `docs(dhh): …`. Keep commits atomic and the marketplace
in a validating state.
