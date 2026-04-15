---
description: Git worktree management for isolated feature development. Create worktrees, verify test baselines, clean up after merge. Auto-invoked when worktree operations are needed.
---

# Git Worktrees

## Why worktrees

- **Isolation**: Feature work never contaminates main branch
- **Parallel development**: Multiple features in progress simultaneously
- **Clean rollback**: Discard worktree and branch if feature fails
- **Baseline verification**: Confirm existing tests pass before starting

## Creating a worktree

1. `git branch feature/<name>`
2. `git worktree add ../<name>-worktree feature/<name>`
3. `cd ../<name>-worktree`
4. Install dependencies (npm install, pip install, etc.)
5. Run full test suite — ALL tests must pass
6. If tests fail: STOP. Do not start work on a broken baseline.

## Finishing a worktree

After merge or discard:
1. `git worktree remove ../<name>-worktree`
2. `git branch -d feature/<name>` (if merged)
3. `git branch -D feature/<name>` (if discarded)
