---
description: 4-phase root cause debugging process. Gather evidence, form hypotheses, test hypotheses, confirm root cause. Use for any bug diagnosis within the Bayes Ship pipeline. Includes defense-in-depth and condition-based-waiting techniques.
---

# Systematic Debugging

## Process over guessing

Never guess at fixes. Follow the 4-phase process:

### Phase 1: Gather evidence
- Read error messages, stack traces, and logs CAREFULLY
- Identify the exact line and condition where failure occurs
- Note what DOES work (narrowing the search space is as valuable as finding the bug)

### Phase 2: Form hypotheses
- List 2-3 possible causes based on evidence
- Rank by likelihood
- Each hypothesis must be testable

### Phase 3: Test hypotheses
- Add targeted logging or breakpoints
- Inspect state at the point of failure
- Trace execution path
- Eliminate hypotheses one by one

### Phase 4: Confirm root cause
- The fix must address the ACTUAL cause, not a symptom
- Ask: "If I revert this fix, does the bug come back?" (mentally or actually)
- Ask: "Does this fix handle the general case, or just the specific trigger I found?"

## Defence-in-depth

After fixing the immediate bug, consider:
- Should there be a defensive check upstream that catches this class of error earlier?
- Should there be a test that prevents regression?
- Is this a symptom of a broader design issue?

## Condition-based waiting

When debugging async or timing-dependent issues:
- Never use `sleep()` as a fix. It masks the problem.
- Wait for CONDITIONS, not TIME. Poll for the expected state.
- If a race condition exists, the fix is synchronization, not delays.
