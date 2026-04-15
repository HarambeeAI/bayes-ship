---
description: Task decomposition methodology for Bayes Ship. File decomposition first, then 2-5 minute atomic tasks, XML plan format, model routing, wave grouping. Auto-invoked during planning phases.
---

# Atomic Planning

## File decomposition first

Before defining tasks, map out ALL files:
- What files will be created?
- What files will be modified?
- What is each file's single responsibility?
- What are the interfaces between files?

Lock in decomposition decisions here. Units should have clear boundaries.

## Task sizing

Each task: 2-5 minutes of AI work. If it cannot be described in 2-3 sentences, split it.

Right-sized examples:
- Add a database column and migration
- Add a single UI component to a page
- Update a server action with new logic
- Add a filter dropdown

Too big (must split):
- "Build the dashboard" → schema + queries + UI components + filters
- "Add authentication" → schema + middleware + login UI + session handling

## XML plan format

```xml
<task type="auto" model="sonnet">
  <n>Task title</n>
  <files>file1.ts, file2.ts</files>
  <skill>skill-name</skill>
  <test-first>Write test: [description]. Watch fail (RED).</test-first>
  <action>[Implementation]. Watch pass (GREEN).</action>
  <verify>Run tests. All pass. Typecheck passes.</verify>
  <done>Completion criteria.</done>
</task>
```

## Model routing

- `haiku`: mechanical 1-2 file tasks
- `sonnet`: standard multi-file integration (default)
- `opus`: architecture decisions, complex refactors, review

## Wave grouping

- Wave 1: all independent tasks (run in parallel)
- Wave 2: tasks that depend on Wave 1 results
- Wave N: tasks that depend on Wave N-1

Never put dependent tasks in the same wave.
