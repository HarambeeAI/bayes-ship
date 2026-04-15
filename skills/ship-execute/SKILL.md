---
description: Execute a plan via fresh subagents with mandatory TDD and two-stage review (spec compliance then code quality) after every task. The core execution engine of Bayes Ship.
disable-model-invocation: true
---

# Execute — Subagent-driven implementation with two-stage review

## Pre-condition

A plan must exist in `.claude/state/plans/phase-<n>/plan.md`. If not, tell the user to run `/bayes-ship:ship-plan` first.

## Process

### Step 1: Load plan

Read the plan file ONCE upfront. Extract all task details so subagents never need to re-read the file.

### Step 2: For each task in the plan

**2a. Dispatch Implementer subagent** (fresh context per task):

Provide the subagent with ONLY:
- The task XML (extracted from the plan)
- The relevant SKILL.md files referenced in `<skill>`
- ARCHITECTURE.md for context
- The test-driven-development skill
- Nothing else. Never inherit session history.

The Implementer follows TDD:
1. Write the failing test from `<test-first>` (RED)
2. Watch it fail
3. Write minimal implementation from `<action>` (GREEN)
4. Watch tests pass
5. Refactor if needed
6. Report status signal:
   - `DONE` — ready for review
   - `DONE_WITH_CONCERNS` — complete but flagging issues
   - `NEEDS_CONTEXT` — blocked on a question (escalate to user)
   - `BLOCKED` — cannot proceed (escalate immediately)

On `NEEDS_CONTEXT`: check ARCHITECTURE.md and RESEARCH.md for the answer. If found, provide it and let the implementer continue. If not found, ask the user.

On `BLOCKED`: stop and alert the user immediately.

**2b. Spec Compliance Review** (fresh subagent):

Dispatch a spec-compliance-reviewer subagent with:
- The original task XML from the plan
- The git diff of the implementer's changes
- The PRD acceptance criteria for this story

The reviewer is SKEPTICAL. It does NOT trust the implementer's self-report. It reads actual code and checks:
- Does the implementation match the spec exactly?
- Are there missing requirements?
- Is there over-building (more than the spec requires)? YAGNI violations?

If spec compliance FAILS: re-dispatch the implementer with specific feedback. Do NOT proceed to code quality review.

**2c. Code Quality Review** (fresh subagent):

Only runs AFTER spec compliance passes. Wrong order wastes effort.

Dispatch a code-quality-reviewer subagent with:
- The git SHAs from the implementer's commits
- The coding-standards skill content (if installed)
- The security skill content (if applicable to this task)

Reviews for: code style, patterns, bugs, security issues, test quality, naming, complexity.

Reports issues by severity:
- **Critical** — blocks progress. Re-dispatch implementer.
- **Warning** — should fix. Fix in current cycle.
- **Note** — optional. Log for later.

**2d. Commit and progress**

After both reviews pass:
1. Atomic git commit with conventional commit message
2. Update `progress.json` (increment completed_tasks)
3. Move to next task

### Step 3: Wave parallelism

If multiple tasks in the current wave are independent, dispatch them in parallel. Wait for all tasks in a wave to complete before starting the next wave.

### Step 4: Phase complete

After all tasks in the plan are done:
1. Run `typecheck` on the full project
2. Run the full test suite
3. Report summary: tasks completed, commits, review pass/fail rates
4. Output: "Phase N complete. Run `/bayes-ship:ship-plan <next>` for the next phase, or `/bayes-ship:ship-test` for full QA."

## Rules

- Two-stage review is MANDATORY. Never skip it. Never combine the two stages.
- TDD is the default. If `tdd_mode` is `strict`, delete any code written before tests.
- Every subagent gets a clean context window. No accumulated session garbage.
- Use the model specified in the task's `model` attribute when `model_routing` is `auto`.
