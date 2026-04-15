---
description: Comprehensive QA verification pass. Runs verification-before-completion against PRD acceptance criteria, writes missing tests, runs security scan. Produces REVIEW.md.
disable-model-invocation: true
---

# Test — Quality assurance verification

## Process

1. Read PRD acceptance criteria for completed phases.

2. **Verification-before-completion**: Dispatch a verifier subagent that:
   - Runs the full test suite and reports ACTUAL results (not summaries)
   - Checks EVERY acceptance criterion against observable behavior
   - If a criterion says "filter persists in URL params," actually navigate and check
   - If any criterion cannot be verified, flag it — do not assume passing

3. **Missing test coverage**: Dispatch a tester subagent that:
   - Identifies untested code paths
   - Writes additional tests (unit, integration, e2e)
   - Targets minimum 80% coverage
   - Runs typecheck and linting

4. **Security scan**: Dispatch a security reviewer subagent that:
   - Checks against OWASP top 10
   - Scans for hardcoded secrets, SQL injection, XSS
   - Validates auth/authz patterns
   - Checks M-Pesa callback validation (if applicable)

5. Save results to `.claude/state/REVIEW.md` organized by severity (critical → warning → note).
6. Report summary to user.
7. Output: "Run `/bayes-ship:ship-finish-branch` to complete the feature branch."
