---
description: Autonomous build mode. After spec and architecture are approved, runs the entire build pipeline (worktree, plan, execute, test, finish-branch) without stopping. Logs all autonomous decisions. Use --checkpoints for semi-autonomous mode.
disable-model-invocation: true
---

# Auto — Autonomous build mode

## Usage

`/bayes-ship:ship-auto` — fully autonomous
`/bayes-ship:ship-auto --checkpoints` — semi-autonomous (pauses between phases)

## Pre-condition

Both `.claude/state/PRD.md` and `.claude/state/ARCHITECTURE.md` must exist and be approved (check progress.json). Autonomous mode only covers the BUILD phase. Discovery, research, spec, and architecture always require human input.

## Process

Announce to the user:

"Autonomous build activated. I will:
1. Create a git worktree
2. Plan and execute each phase
3. Run TDD + two-stage review on every task (non-negotiable)
4. Run full QA verification
5. Merge to main
6. Deploy (if credentials available)

All decisions logged to .claude/state/auto-decisions.md
I will only stop if genuinely BLOCKED.
Starting now..."

### Orchestration loop

```
1. Create worktree (same as /bayes-ship:ship-worktree)

for each phase in ROADMAP:
    2. Plan the phase (same as /bayes-ship:ship-plan)
       → No user approval needed. Spec + architecture are the contract.

    3. Execute all tasks (same as /bayes-ship:ship-execute)
       → TDD mandatory. Two-stage review mandatory.
       → Atomic commits per task.

    4. Handle NEEDS_CONTEXT autonomously:
       → Check ARCHITECTURE.md and RESEARCH.md for the answer
       → If found: use it, log to auto-decisions.md
       → If not: pick most conservative/standard option, log it
       → Format: { context, sources_checked, decision, confidence, phase, task }

    5. Handle BLOCKED:
       → Attempt workaround (retry with different approach, max 2)
       → If still blocked: STOP and alert user. Save progress.

    6. Verify phase (all tests pass, typecheck clean)
       → If fails: re-plan failed tasks, re-execute (max 2 retries)
       → If still failing: STOP and alert user.

    [Semi-autonomous checkpoint — only with --checkpoints]:
       "Phase N complete: X tasks, Y commits, Z auto-decisions.
        See auto-decisions.md. Continue? (yes / stop)"

after all phases:
    7. Run /bayes-ship:ship-test (full QA)
    8. Run /bayes-ship:ship-finish-branch → auto-merge (squash)
    9. If deploy credentials in environment → /bayes-ship:ship-deploy
       If not → "Build complete. Run /bayes-ship:ship-deploy with credentials."
```

### auto-decisions.md format

```markdown
# Auto-Decisions Log

## Decision N: [Title]
- Context: [What the implementer asked]
- Sources checked: [Which files were consulted]
- Decision: [What was decided]
- Confidence: High/Medium/Low
- Phase: N, Task: M
```

## Safety guarantees

- TDD is NEVER skipped in autonomous mode
- Two-stage review is NEVER skipped in autonomous mode
- BLOCKED always stops and alerts the user
- Every auto-decision is logged with context and confidence
- Verification-before-completion runs at the end
