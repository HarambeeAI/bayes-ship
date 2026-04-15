---
description: Create atomic execution plans for a specific phase. Includes file decomposition, task sizing (2-5 min), model routing, TDD test-first blocks, and wave grouping. Argument is the phase number.
disable-model-invocation: true
---

# Plan — Atomic task planning

## Usage

`/bayes-ship:ship-plan <phase_number>`

Example: `/bayes-ship:ship-plan 1`

## Pre-condition

`.claude/state/PRD.md`, `.claude/state/ARCHITECTURE.md`, and `.claude/state/ROADMAP.md` must exist.

## Process

1. Read PRD + ARCHITECTURE + ROADMAP. Identify which user stories belong to the specified phase.

2. **File decomposition first**: Before defining tasks, map out ALL files to be created or modified. For each file, state its single responsibility. Lock in decomposition decisions here. Design units with clear boundaries and well-defined interfaces.

3. Break the phase into atomic tasks. **Each task must be completable in 2-5 minutes of AI work.** If a task cannot be described in 2-3 sentences, split it further.

4. Structure each task as XML:

```xml
<task type="auto" model="sonnet">
  <n>Task title</n>
  <files>src/path/to/file.ts, src/path/to/other.ts</files>
  <skill>relevant-skill-name</skill>
  <test-first>
    Write test: [describe the test]. Watch it fail (RED).
  </test-first>
  <action>
    [Implementation instructions]. Watch tests pass (GREEN).
  </action>
  <verify>Run test suite. All tests pass. Typecheck passes.</verify>
  <done>Completion criteria.</done>
</task>
```

5. **Model routing** per task:
   - `model="haiku"` — mechanical 1-2 file tasks (rename, config, simple CRUD)
   - `model="sonnet"` — standard multi-file integration (default)
   - `model="opus"` — architecture decisions, complex refactors, review

6. **Wave grouping**: Group independent tasks into parallel waves. Dependent tasks go into later waves.

7. Save plan to `.claude/state/plans/phase-<n>/plan.md`.
8. Present the plan to the user: file decomposition summary, task list with waves, estimated time.
9. Wait for "go" or adjustments.
10. Update progress.json.
11. Output: "Run `/bayes-ship:ship-execute` to start building."

## Rules

- Every task MUST include a `<test-first>` block (TDD by default). If `tdd_mode` is `relaxed`, the test-first block becomes optional.
- Every task MUST include a `<verify>` block with concrete checks.
- Every task references which skill the implementer should read via `<skill>`.
