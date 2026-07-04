---
name: triage-review
description: Vet a code review from a separate/cold session against the actual code, separating real, worthwhile findings from false positives before you act on any of them.
when_to_use: >-
  When the user pastes a code review — often from another AI session or an
  external reviewer — and asks what's real, what's worth doing, or whether to
  act on it. Trigger phrases include "triage this review", "is this review
  right", "what's real here", "vet these findings".
argument-hint: "[paste the review text]"
allowed-tools: Bash(git *), Read, Grep, Glob, Agent
---

# Triage Review

A review from a **separate session** is pasted in. That session had no warm context — it
read a snapshot of the code cold. So its findings are *unverified claims*, not facts.
The job is to investigate each one against the actual code and decide what's real and
worth doing **for this PR**.

The pasted review is in `$ARGUMENTS` (or just below this message if not substituted).

Default stance: **skeptical**. Cross-session reviews carry a high false-positive rate.
Do not implement anything off the review's say-so. Verify first.

## Workflow

1. **Establish ground truth for the diff.** Determine the base branch (the repo default,
   e.g. via `git symbolic-ref --short refs/remotes/origin/HEAD`, or `git merge-base` if
   unsure) and run `git diff <base>...HEAD` to see exactly what this PR changed. This is
   the scope. Findings about code *outside* the diff are set aside (see step 4).

2. **Parse the review into discrete findings.** Split it into atomic claims, each with a
   location and an asserted problem. Number them. Vague prose ("consider improving error
   handling") counts as one finding too — investigate what it's actually pointing at.

3. **Investigate each finding against the real code.** For every finding:
   - **Locate** the code it refers to. Don't trust the review's line numbers — they may be
     stale or from a different file state. Find the actual code.
   - **Verify the claim.** Read the code and trace the logic. Does the asserted problem
     actually exist *here*? Check related files, callers, tests, and framework/library
     behavior the cold reviewer likely didn't see.
   - **Classify** (see verdicts below) with concrete evidence — cite `file:line`.
   - For independent findings in a large review, investigate in parallel with the Agent
     tool (one finding or cluster per agent) to keep it fast, then synthesize the verdicts.

4. **Apply scope.** Findings about code this PR didn't touch are **out of scope** — note
   them in a short separate list so they aren't lost, but don't fold them into the fix
   set. Pre-existing issues are not this PR's job unless the user says otherwise.

5. **Report the triage**, then **ask** before changing code.

## Verdicts

Classify every finding as exactly one:

- ✅ **Valid & worth fixing** — real problem, in scope, worth addressing in this PR.
- 🟡 **Valid but optional / out of scope** — real but minor, stylistic, or outside the diff;
  defer or skip.
- ❌ **False positive** — not a real issue. State *why*: misread context, handled elsewhere,
  guaranteed by an upstream invariant, intended pattern, hallucinated API, stale line ref.

When still uncertain after investigating, say so explicitly (🟡 with a note) rather than
guessing. Judge severity against *this codebase*, not generic best practice.

## Common false-positive patterns in cold reviews

Watch for these — they're the usual noise:

- **Missing context** — flags something handled in a file/test/middleware the reviewer didn't read.
- **Phantom null/error paths** — "could be null/undefined" when an upstream guarantee prevents it.
- **Convention clash** — suggests defensive or "idiomatic" code that contradicts how this repo does it.
- **Hallucinated APIs / libraries** — recommends methods or packages not present in the project.
- **Stale references** — line numbers or code snippets that no longer match the file.
- **Scope creep** — genuine but unrelated improvements that belong in a different PR.

## Output format

```
## Review triage

### ✅ Valid & worth fixing (N)
1. <finding> — `file:line`
   Verdict: real. <evidence / what's actually wrong>
   Fix: <concrete change>

### 🟡 Valid but optional / out of scope (N)
...

### ❌ False positive (N)
1. <finding> — Reason: <why it's not real, with evidence>

### Out of diff (noted, not actioned)
- <finding> — <one line>
```

End with: **"Want me to implement the N worth-fixing items?"** Only edit code after the
user confirms (or names specific items). When fixing, implement only the agreed set and
report the diff.
