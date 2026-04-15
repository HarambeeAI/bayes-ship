---
description: Two-stage code review protocol. Stage 1: spec compliance (does code match spec?). Stage 2: code quality (is code well-written?). Stage 2 only runs after Stage 1 passes. Use after every implementation task in the Bayes Ship pipeline.
---

# Two-Stage Review

## Why two stages

Reviewing code quality on an implementation that doesn't match the spec is wasted effort. The spec compliance check gates the quality check.

## Stage 1: Spec compliance review

The reviewer is SKEPTICAL. It does NOT trust the implementer's self-report.

Check:
- Does the implementation match the spec exactly?
- Are there MISSING requirements from the spec?
- Is there OVER-BUILDING (code that does more than the spec requires)?
- YAGNI violations? Features not in the spec?

If spec compliance fails: re-dispatch to implementer with specific feedback listing what's missing or what's extra. Do NOT proceed to Stage 2.

## Stage 2: Code quality review

Only runs AFTER Stage 1 passes.

Check:
- Code style and conventions (against coding-standards skill if installed)
- Potential bugs or edge cases
- Security issues (against security skill if applicable)
- Test quality (are the tests actually testing meaningful behavior?)
- Naming clarity and code readability
- Unnecessary complexity

Report by severity:
- **Critical**: Blocks progress. Must fix before commit.
- **Warning**: Should fix. Fix in current cycle.
- **Note**: Optional improvement. Log for later.

## Rules

- Never skip either stage
- Never combine stages into one review
- Never let the implementer review their own work (use a separate subagent)
- Reviewers read actual code and git diffs, not the implementer's description
