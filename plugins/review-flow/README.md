# review-flow — branch review & session helpers

Personal utilities for reviewing code and landing agent coding sessions. Three
skills, each for a different moment around a branch:

| Skill           | Invoke                       | For                                                                                                                             |
|-----------------|------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| `tour-branch`   | `/review-flow:tour-branch`   | Walk a branch commit-by-commit as a guided tour — what changed, why, and how it fits — with understanding checks along the way. |
| `triage-review` | `/review-flow:triage-review` | Vet a cold-session code review against the real code, sorting real findings from false positives before you act.                |
| `replay-branch` | `/review-flow:replay-branch` | Replay a messy branch into clean, logically-grouped commits on a parallel branch, leaving the original intact.                  |

## Invocation

`tour-branch` and `replay-branch` are `disable-model-invocation: true` — they're
deliberate, side-effectful workflows you start yourself. `triage-review` stays
model-invocable so Claude can offer it when you paste a review.

Each skill scopes its `allowed-tools` to what it needs (`Bash(git *)`, plus
`Bash(gh *)` for `tour-branch`) so the common git reads don't prompt.

## Notes

- All three detect the **base branch** from the repo default (or an explicit
  argument), not a hardcoded `main` — they work in `master`/`develop`/etc. repos.
- `replay-branch` handles deletions and renames correctly (a `git checkout` of a
  source file can't represent a deletion, so it `git rm`s removed paths and
  verifies the replayed tree is byte-identical to the source).
- `tour-branch`'s issue-tracker and PR steps are optional and degrade gracefully
  when no Linear MCP / `gh` / PR is present.
