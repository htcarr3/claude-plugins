# Source registry

The one durable thing this plugin encodes: **where to look**, not what's there.
Everything below is a pointer to a live source. Fetch it fresh every time — never
trust a remembered value from a source listed here. "Where to look" ages slowly;
"what it says" ages by the week.

When a skill needs current truth, pull from the relevant sources below, prefer
the most recently dated material, and **note the date you fetched it** so the
staleness is visible in whatever you produce.

## The installed CLI is a source too — check it first

The single most authoritative source for *this machine* is the version actually
installed. Before consulting the web, establish the ground truth locally:

- `claude --version` — the version everything else must be true *for*.
- `claude --help`, `claude plugin --help`, `claude config --help` — the real,
  installed surface of commands and flags.
- The shipped meta-skills and built-in agents (`~/.claude/skills/`, the agent
  registry) — but treat these as *possibly stale*; that staleness is often the
  thing an audit is looking for.

Web docs describe the latest release; the local CLI is what the user is running.
When they disagree, the local version wins for "what works right now," and the
gap itself is worth reporting.

## Official documentation

- **Claude Code** — https://code.claude.com/docs — hooks, skills, plugins,
  slash commands, settings, subagents, output styles, MCP, permissions, IDE
  integrations. The primary source for anything about the CLI harness.
- **Claude API & Agent SDK** — https://docs.claude.com — tool use, the Agent
  SDK, model IDs and capabilities, prompt engineering guides.
- **Changelog / release notes** — the `anthropics/claude-code` repository
  (CHANGELOG.md and releases) for what changed recently and when. This is where
  new config surfaces show up before the prose docs catch up.

## Best-practice / engineering writing

- **Anthropic engineering blog** — https://www.anthropic.com/engineering — the
  canonical "how to build effectively with Claude" essays (agent design, context
  engineering, tool design, writing effective tools/skills).
- **Anthropic news** — https://www.anthropic.com/news — model releases and
  capability changes that shift what "best practice" even means.

## Practitioners (weight lower than official sources, but early signal)

- **Thariq Shihipar** (Anthropic) — https://thariqs.github.io and @trq212 on X —
  practical field guides for working with the newest models (e.g. the "unknowns"
  framework this marketplace's `unknowns` plugin is built from). Good for
  techniques that haven't reached the official docs yet.
- Leave room here for others as they prove reliable. A practitioner earns a slot
  by being right about the *current* model, not by being well-known.

## Using this registry

1. **Local first.** Pin the installed version and its real surface.
2. **Official next.** Docs + changelog for the authoritative current behaviour.
3. **Engineering writing** for the *why* and the judgment calls docs don't make.
4. **Practitioners** for leading-edge technique, clearly marked as less settled.

If a source 404s, has moved, or looks abandoned, say so in your output and don't
paper over it with a remembered answer — a dead source is itself a finding, and
this file is what should get updated.
