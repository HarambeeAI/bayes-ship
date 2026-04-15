---
description: Resume from last checkpoint after session restart or compaction. Reads progress.json and reconstructs state.
disable-model-invocation: true
---

# Resume — Continue from last checkpoint

## Process

1. Read `.claude/state/progress.json`.
2. Determine current phase and task from the progress data.
3. Read the relevant state files for the current phase:
   - If in discovery: read PROJECT.md
   - If in research/spec/architect: read CONTEXT.md + any completed state files
   - If in execution: read PRD.md + ARCHITECTURE.md + current plan file
4. If a worktree exists, switch to it.
5. Report current state to the user.
6. Suggest the next action.

## Rules

- This is the recovery mechanism. It must work after compaction, session restart, or laptop sleep.
- Never re-run completed phases. Pick up from exactly where things left off.
