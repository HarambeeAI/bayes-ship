---
description: Create an isolated git worktree for feature implementation. Verifies clean test baseline before work begins. Run after architecture approval.
disable-model-invocation: true
---

# Worktree — Isolated workspace setup

## Process

1. Create a new git branch: `feature/<project-name>-v1` (or use phase number if building incrementally).
2. Create a git worktree in an isolated directory: `../<project-name>-worktree`.
3. Run project setup in the worktree: install dependencies, build.
4. **Verify clean test baseline**: Run the full test suite. ALL existing tests must pass.
   - If tests fail: STOP. Surface failures to the user. Do NOT start implementation on a broken baseline.
   - If no tests exist (new project): note this and proceed.
5. Record worktree location in `progress.json`.
6. Output: "Workspace ready. Run `/bayes-ship:ship-plan 1` to plan Phase 1."

## Rules

- If `use_worktrees` is set to `false` in settings, skip worktree creation and work in the current directory. Still verify test baseline.
- If a worktree already exists for this project, ask the user whether to reuse or create a new one.
