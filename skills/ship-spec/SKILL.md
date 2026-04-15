---
description: Generate a structured PRD from discovery and research outputs. Includes spec review subagent with 3-iteration cap. Requires user approval before proceeding. Must run after ship-research.
disable-model-invocation: true
---

# Spec — PRD generation with automated review

## Pre-condition

Read `.claude/state/CONTEXT.md` and `.claude/state/RESEARCH.md`. Both must exist.

## Process

1. Read CONTEXT.md + RESEARCH.md.
2. Generate a structured PRD with these sections:
   - **Introduction and problem statement**
   - **Goals** (measurable)
   - **User stories** (US-001 format). Each story must be completable in 2-5 minutes of AI work. If a story cannot be described in 2-3 sentences, it is too big — split it.
   - **Acceptance criteria** per story (verifiable, not vague). Every story includes "Typecheck passes" and "All tests pass" as final criteria. UI stories add "Verify changes work in browser."
   - **Non-goals** (what this version will NOT do)
   - **Technical considerations** (informed by research)
   - **Data model sketch**
3. Order stories by dependency: schema → backend → frontend → integration → UI polish.

### Spec review subagent

Before presenting to the user, dispatch a spec-document-reviewer subagent:
- Provide ONLY the spec file path and CONTEXT.md (never full session history)
- Reviewer checks for: completeness, internal consistency, clarity, scope creep, YAGNI violations
- Only flags issues that would cause real problems during implementation
- If issues found: revise the spec and re-dispatch the reviewer
- **Maximum 3 iterations.** If reviewer still finds issues after 3 tries, surface the unresolved conflict to the user for a decision.

4. Present the reviewed PRD to the user in digestible chunks:
   - Goals section first (approve/adjust)
   - User stories summary (approve/see details/adjust)
   - Non-goals (approve/adjust)
5. Save approved PRD to `.claude/state/PRD.md`.
6. Update `progress.json`: set `phases.spec.status` to `"approved"`.
7. Output: "Ready. Run `/bayes-ship:ship-architect` to define the system architecture."

## Gate

The user MUST explicitly approve the PRD before `/bayes-ship:ship-architect` can run. No implicit approval.

## Story sizing reference

Right-sized: Add a database column and migration. Add a single UI component. Update a server action. Add a filter dropdown.

Too big (must split): "Build the dashboard" → schema, queries, UI components, filters. "Add authentication" → schema, middleware, login UI, session handling.
