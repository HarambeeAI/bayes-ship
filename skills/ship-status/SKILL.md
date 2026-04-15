---
description: Show current progress dashboard. Displays phase, tasks, context usage, worktrees, review rates.
disable-model-invocation: true
---

# Status — Progress dashboard

Read `.claude/state/progress.json` and display:

- Current phase and sub-phase
- Tasks completed / remaining per phase
- Active worktrees and their branches
- Two-stage review pass/fail rates (if in execution)
- Auto-decisions made (if in autonomous mode)
- Last checkpoint timestamp
- Git commit count

Format as a clean, readable summary. Do not use excessive formatting.
