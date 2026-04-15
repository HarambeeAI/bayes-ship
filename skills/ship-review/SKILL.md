---
description: Trigger a manual code review of recent changes. Dispatches both spec compliance and code quality reviewers. Use between phases or after manual edits.
disable-model-invocation: true
---

# Review — Manual two-stage code review

## Process

1. Identify changes to review (uncommitted changes, or last N commits).
2. Dispatch spec-compliance-reviewer subagent: does the code match the spec?
3. If spec compliance passes, dispatch code-quality-reviewer subagent.
4. Report findings by severity: critical / warning / note.
5. If critical issues found, suggest fixes.
