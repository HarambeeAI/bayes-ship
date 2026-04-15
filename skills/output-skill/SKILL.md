---
description: "Anti-lazy output enforcement. Prevents placeholder comments, skipped code blocks, half-finished outputs, and TODO stubs. Auto-invoked on every implementation task to ensure complete, production-quality output. Source: Leonxlnx/taste-skill."
---

# Output Skill — Complete Output Enforcement

## Purpose

Prevents the agent from cutting corners during long execution runs. As context fills up, agents tend to produce lazy output: placeholder comments, skipped sections, TODO stubs, abbreviated code. This skill enforces complete output.

## Rules (non-negotiable)

### Never output these

- `// TODO: implement this`
- `// ... rest of the code`
- `/* placeholder */`
- `// similar to above`
- `// add more as needed`
- `// etc.`
- `// implementation left as exercise`
- Any comment that substitutes for actual code

### Every function must be complete

- No function stubs with empty bodies
- No functions that say `throw new Error('Not implemented')`
- If a function is in the plan, write it fully. If it's not in the plan, don't create it.

### Every file must be complete

- No partial files that "continue in the next task"
- Each file must be syntactically valid and importable
- All imports must resolve to actual modules

### Every test must assert something meaningful

- No tests with `expect(true).toBe(true)`
- No tests that only check "does not throw"
- Every test must verify actual behavior

## When this fires

This skill is auto-invoked on EVERY implementation task, alongside coding-standards. It serves as a quality floor that catches the most common agent failure mode: producing incomplete output when context gets long.

## If the agent feels the urge to abbreviate

That is a context rot signal. Instead of abbreviating, the task should be split into smaller pieces, or the agent should report NEEDS_CONTEXT to the orchestrator.
