---
description: Systematic bug diagnosis and fix using 4-phase root cause analysis. Writes a failing test for the bug (RED), fixes it (GREEN), verifies no regressions. Argument is the bug description.
disable-model-invocation: true
---

# Bugfix — Systematic diagnosis and fix

## Usage

`/bayes-ship:ship-bugfix <description of the bug>`

## Process

### Phase 1: Reproduce
Dispatch a debugger subagent to create a minimal reproduction of the bug. If the bug cannot be reproduced, report this and ask the user for more details.

### Phase 2: Root cause tracing (4-phase systematic process)
1. **Gather evidence**: Collect logs, error messages, stack traces, relevant code paths.
2. **Form hypotheses**: List 2-3 possible causes based on evidence.
3. **Test hypotheses**: Add logging, inspect state, trace execution to narrow down.
4. **Confirm root cause**: Prove the fix addresses the actual cause, not a symptom.

### Phase 3: Defence-in-depth
Consider whether the fix should also add defensive checks upstream to prevent similar bugs.

### Phase 4: Fix (TDD)
1. Write a failing test that reproduces the bug exactly (RED).
2. Fix the code to make the test pass (GREEN).
3. Refactor if needed.

### Phase 5: Verify
1. Run the full test suite. Confirm no regressions.
2. Verify the original reproduction now works correctly.
3. Atomic commit with descriptive message referencing the bug.
