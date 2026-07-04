---
name: replay-branch
description: Replay a messy branch's changes into a clean, logically-grouped commit history on a parallel branch, preserving the original as reference. Use to reshape iterative or fixup commits before PR review.
when_to_use: >-
  When a branch has messy, iterative, or fixup commits that should be
  reorganized into clean logical commits for review. Trigger phrases include
  "replay this branch", "clean up my commits", "reorganize the history",
  "squash into logical commits".
disable-model-invocation: true
argument-hint: "[base-branch]"
allowed-tools: Bash(git *), Read, Grep, Glob, AskUserQuestion
---

# Replay Branch

Create a clean commit history by replaying changes from the current branch into a new
branch with logical, well-organized commits. The original branch is left untouched as a
reference.

## Phase 1: Analyze

1. **Identify the branches.** Current branch is `git rev-parse --abbrev-ref HEAD`. The base
   is `$ARGUMENTS` if given, else the repo default —
   `git symbolic-ref --short refs/remotes/origin/HEAD 2>/dev/null` (strip the `origin/`),
   falling back to `main`. Do **not** assume `main`; many repos use `master`, `develop`, etc.
2. Review commit history: `git log <base>..HEAD --oneline --reverse`
3. Review the overall change set, including adds, deletes, and renames:
   `git diff <base>...HEAD --name-status`
4. Read key changed files to understand the work.
5. Group changes into logical commits by feature/concern. **Every path in the
   `--name-status` output must land in exactly one commit** — including deletions (`D`) and
   renames (`R`). A path that's dropped from the plan will surface as a non-empty
   verification diff at the end.
6. Bundle specs/tests with their corresponding feature commit (not as a separate commit).

## Phase 2: Plan

Use AskUserQuestion to clarify:
- Are the proposed commit groupings correct?
- Any commits that should be kept separate?
- Any changes to commit messages?

Generate a replay plan:

```
## Replay Plan

**Source branch:** <current-branch>
**Clean branch:** <current-branch>-clean
**Base:** <base>

### Commit 1: <type>(<scope>): <description>

**Files:**
- A/M  path/to/file.rb
- A/M  spec/path/to/file_spec.rb
- D    path/to/removed_file.rb

**Description:** What this commit accomplishes

### Commit 2: ...

---

### Verification
After replay: `git diff <source-branch> <clean-branch>`
Expected: no output (identical trees)
```

## Phase 3: Execute

After the user approves the plan:

1. Create the clean branch off the base:
   ```bash
   git checkout -b <branch>-clean <base>
   ```

2. For each planned commit, materialize its files by change type, then commit:
   ```bash
   # Added / modified files — copy the source branch's version onto the index + worktree
   git checkout <source-branch> -- <added_or_modified_paths>

   # Deleted files — remove them (git checkout can't represent a deletion)
   git rm <deleted_paths>

   # Renamed files — add the new path and remove the old one
   git checkout <source-branch> -- <new_path>
   git rm <old_path>

   git commit -m "<message>"
   ```
   The `git rm` step is the one people miss: `git checkout <source> -- <path>` only ever
   *writes* a file, so a file the branch deleted stays present on the clean branch unless
   you explicitly remove it.

3. Verify the trees are identical:
   ```bash
   git diff <source-branch> <clean-branch>
   ```

4. If the diff is **not** empty, it means a path was missed, mis-grouped, or a deletion
   wasn't applied. Reconcile it into the right commit (amend or add a commit) until the diff
   is empty — do not finish with a dirty verification.

## Phase 4: Finalize

Output:

```
## Replay Complete

| Branch            | Value              |
|-------------------|--------------------|
| Source (original) | `<branch>`         |
| Clean (new)       | `<branch>-clean`   |
| Base              | `<base>`           |

Verification: ✓ No differences
```

Ask: "Would you like to create a pull request from `<branch>-clean`?"
