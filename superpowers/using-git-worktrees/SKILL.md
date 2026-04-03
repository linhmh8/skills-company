---
name: using-git-worktrees
description: Use when branch isolation would materially reduce risk or confusion and a separate worktree is a better fit than working in the current checkout.
---

# Using Git Worktrees

## Overview

Use worktrees when isolation helps.
Do not create one just because a process doc says so.

## Use When

- a non-trivial change should stay isolated from current local work
- multiple branches or review states need to coexist
- the current checkout is dirty and you want a clean execution space
- the user explicitly asks for isolation

## Skip When

- the task is tiny and local
- the current checkout is already the right place to work
- worktree setup would add more friction than safety

## Directory Selection

Choose in this order:

1. existing project-local worktree directory such as `.worktrees/` or `worktrees/`
2. documented repo preference from project instructions such as `AGENTS.md`
3. user preference
4. user-home default such as `~/.codex/worktrees/<project>/`

If a project-local directory exists but is not ignored, do not auto-commit just to bootstrap a worktree.
Either:

- add the ignore entry as part of the user's intended repo change, or
- ask whether to use a user-home location instead

## Workflow

1. Check for an existing project-local worktree directory.
2. Search project instructions for a documented preference.
3. Resolve the target directory using the priority order above.
4. Create the worktree with a new branch.
5. Run the minimal documented setup or smoke check for the repo.
6. Report the path and baseline status.

## Creation Pattern

```bash
project=$(basename "$(git rev-parse --show-toplevel)")

# Example user-home location
path="$HOME/.codex/worktrees/$project/$BRANCH_NAME"

git worktree add "$path" -b "$BRANCH_NAME"
cd "$path"
```

## Baseline Verification

Prefer the repo's documented bootstrap or check command.

Examples:

- `cargo xtask check`
- `npm test -- <target>`
- `pytest <target>`
- `go test ./...`

If the repo has no clear baseline command, say so explicitly instead of guessing a heavy setup flow.

## Output

Report:

- worktree path
- branch name
- whether baseline verification ran
- whether it passed, failed, or was skipped with reason

## Misuse Patterns

- auto-committing ignore files during setup
- forcing a worktree for tiny edits
- guessing heavyweight bootstrap commands without repo guidance
- hardcoding one global location that does not match Codex

## Bottom Line

Use worktrees when isolation helps enough to justify the setup.
Prefer documented repo conventions, avoid surprise commits, and keep baseline checks honest.
