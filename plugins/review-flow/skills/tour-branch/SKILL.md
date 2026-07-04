---
name: tour-branch
description: Walk a git branch commit-by-commit like a guided tour — narrating what changed and why, connecting the commits into a coherent story, and checking understanding as you go.
when_to_use: >-
  When the user wants to deeply understand a branch's changes — reviewing a
  branch, onboarding to a feature, or preparing for PR review. Trigger phrases
  include "tour this branch", "walk me through this branch", "explain these
  commits", "onboard me to this feature".
disable-model-invocation: true
argument-hint: "[base-branch]"
allowed-tools: Bash(git *), Bash(gh *), Read, Glob, Grep, Agent, AskUserQuestion, EnterPlanMode, ExitPlanMode
---

# Branch Tour Guide

Narrate the current branch's commit history as a guided tour, connecting each commit into a coherent story. The goal is to onboard the user so they deeply understand the changes — not just what changed, but why, and how it fits into the existing codebase.

## Phase 1: Gather Context and Build the Plan

Enter plan mode using EnterPlanMode. The plan will serve as the tour's reference document.

### Step 1: Git context

Identify the current branch, then determine the **base branch** — use `$ARGUMENTS` if provided, otherwise the repo default (`git symbolic-ref --short refs/remotes/origin/HEAD`, stripping the `origin/` prefix), falling back to `main`. Don't assume `main`; repos vary (`master`, `develop`, …). Use the resolved base everywhere below in place of `<base>`:

```bash
git rev-parse --abbrev-ref HEAD
git log <base>..HEAD --oneline --reverse
git diff <base>..HEAD --stat
```

### Step 2: Issue tracker (optional)

If an issue-tracker MCP is connected and the branch name encodes an issue ID (a common pattern is `username/ISSUE-ID-slug`), fetch the issue and add its title, description, and any acceptance criteria to the plan. For example, with a Linear MCP, detect the ID and call `mcp__linear-server__get_issue`. If no tracker is connected or the branch encodes no ID, skip this step — don't block the tour on it.

### Step 3: Pull request (optional)

Check whether the branch has an open PR:
```bash
gh pr view --json title,body,comments,reviews,url 2>/dev/null
```

If a PR exists, add to the plan:
- PR title and description (the author's own summary of the work)
- Review comments and discussion threads (reviewer concerns, resolved questions)
- Map each review comment to the file/commit it applies to (used later to surface comments at the relevant stop)
- PR URL for reference

If `gh` isn't available or there's no PR, skip — the tour works from commits alone.

### Step 4: Understand the "before" state

Read the key files that this branch modifies **on the base branch** to understand what the code looked like before. Use `git show <base>:<filepath>` for the 3-5 most important files touched by the branch.

Summarize the pre-existing architecture and behavior in the plan under a "Before This Branch" section. This is critical context — the user needs to understand what existed before they can understand what changed.

### Step 5: Domain glossary

Scan the commits for domain-specific concepts, modules, or patterns that may be unfamiliar (e.g., a `Syncable` concern, a push/fetch flow, a specific service object). Add a glossary to the plan with 1–2 sentence explanations of each. Only include terms the user would need to follow the tour — don't exhaustively document the codebase.

### Step 6: Group commits into chapters

Analyze the commit subjects and diffs to identify logical groupings. A 10-commit branch might have 3 commits for schema changes, 4 for the model/service layer, 3 for tests. Group them into chapters with descriptive titles.

### Step 7: Write the plan

Structure the plan as:

```
# Tour: <branch-name>

## Context
- **Issue:** <title> (<ID>) — <summary of description>   (omit if none)
- **PR:** <title> (<URL>) — <summary of description>       (omit if none)
- **Scope:** N commits, M files changed

## Before This Branch
<2-5 sentences describing the pre-existing state of the code being changed,
the problem or gap this branch addresses, and any relevant architectural context>

## Domain Glossary
- **Term**: 1-2 sentence explanation
- ...

## PR Discussion Highlights
- <notable reviewer comments, questions, or concerns>
- Map: <file:line or commit sha> → <comment summary>

## Chapters
### Chapter 1: <title>
- [ ] Stop 1: <sha> — <subject>
- [ ] Stop 2: <sha> — <subject>

### Chapter 2: <title>
- [ ] Stop 3: <sha> — <subject>
...
```

Exit plan mode using ExitPlanMode after the plan is written.

## Phase 2: The Tour

### Opening: Set the Stage

Start with the "before" context from the plan — explain what the code looked like and what problem existed. Then introduce the branch:

- **Branch name** and what it suggests about the work
- **Issue context** (if available): the problem being solved
- **Scope overview**: N commits, M files changed, rough areas of the codebase touched
- **One-sentence thesis**: what this branch achieves
- **Domain glossary**: introduce any unfamiliar terms before they appear in commits

Use a welcoming, conversational tone. Example:

> Before we dive in, let me set the scene. Currently, sync authentication lives in [place] and works like [this]. The problem is [X]. This branch tackles that across N commits in K chapters. A few terms you'll see: [glossary items]. Let me walk you through how it comes together.

### The Tour: Commit by Commit

Process commits in chronological order (`--reverse`), grouped by chapter. Introduce each chapter with a brief sentence about its theme before diving into its stops.

> **Chapter 1: Database Layer** — These first three commits set up the schema changes that everything else builds on.

For each commit:

1. **Read the diff**: `git show <sha> --stat` first, then `git show <sha>` for the full diff
2. **Read key files** if the diff alone doesn't make the intent clear

For each stop, present:

#### Stop N: `<commit subject>`
`<short sha>` | `<files changed> files`

**What changed:** 1–3 sentence summary of the concrete changes.

**Why:** The motivation — what problem this solves or what it sets up for the next commit.

**Interesting bits:** (include only when genuinely notable)
- Tricky decisions or tradeoffs
- Patterns worth noting ("notice how this mirrors...")
- Gotchas or non-obvious implications
- "You might wonder why... — here's why"

**Key files:** List 1–3 most important files to look at, with a brief note on what to notice in each.

**PR feedback:** If any review comments from the plan map to files or lines in this commit, surface them here. Example: "The reviewer asked about X here — the author responded that Y."

**Analysis:** Before moving on, think through:
- Does this change make sense given the branch's goal?
- Could it break anything? (missing migrations, changed interfaces, removed methods still called elsewhere)
- Are there edge cases or error paths not handled?
- Does it interact safely with other commits in this branch?

Flag any concerns as warnings inline (e.g., "Heads up: this removes a public method — check that nothing outside this branch calls it").

### Check for Understanding

After presenting each stop, use AskUserQuestion to pause and check understanding. Include 1–2 quick questions that test whether the user grasps the key concept of the commit. Keep questions conversational and focused on the "why" or implications, not trivia.

Examples:
- "Before we move on — why do you think this migration adds an index on `company_id` rather than a composite index?"
- "Quick check: if a new record is pushed to the external system, what prevents the fetch cron from importing it back and creating a loop?"
- "What would happen if this default scope returned `nil` instead of an empty relation?"

**Adaptive difficulty:**
- If the user answers correctly and confidently for 2–3 stops in a row, lighten up — ask fewer or simpler questions, or just "Any questions before we move on?"
- If the user struggles or misunderstands, slow down — add more background, explain prerequisites, and ask simpler questions to rebuild understanding
- If they say "skip" or similar, stop quizzing entirely for the rest of the tour

If the user's answer shows a misunderstanding, clarify before continuing.

### Progress and transitions

After each stop, update the plan to check off the completed stop.

Between stops, after the user confirms understanding, add a one-sentence transition linking the previous commit to the next:

> Now that the schema is in place, the next commit wires up the model layer...

Skip the transition if the connection is obvious. Between chapters, use a slightly larger bridge:

> That wraps up the database layer. Everything from here builds on those tables. Next up: the model and service layer that actually uses them.

## Phase 3: Wrap Up the Tour

End with:

- **Summary**: What the branch achieved end-to-end (2–3 sentences)
- **Architecture impact**: Any structural changes to how the codebase works
- **Before vs. after**: Concise comparison of the state before and after this branch
- **Risks or open questions**: Anything a reviewer should scrutinize
- **Migration or deployment notes**: If any commits include migrations, jobs, or config changes

## Style Guide

- Conversational and direct — like a colleague at a whiteboard, not a changelog
- Use "we" and "notice how" to create shared understanding
- Highlight what's interesting, skip what's routine
- Don't editorialize on code quality — describe decisions, not judgments
- Keep each stop concise: aim for 5–10 lines, not paragraphs
- Use Markdown headers and formatting for scannability
- Explain the "before" so the "after" makes sense
