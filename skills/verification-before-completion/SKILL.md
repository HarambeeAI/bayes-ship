---
description: Evidence-based verification pattern. Never trust "it works" without running actual checks. Verify every acceptance criterion against observable behavior. Use before marking any task or phase as complete.
---

# Verification Before Completion

## Core principle

Evidence over claims. Never trust that something works without running the actual verification.

## Checklist before declaring anything "done"

1. **Run the tests**: Not "I'm confident the tests pass." Actually run them. Report actual output.
2. **Check acceptance criteria**: For each criterion, verify against observable behavior:
   - "Filter persists in URL params" → actually navigate and check the URL
   - "Returns 401 for invalid credentials" → actually make the request
   - "Shows error message" → actually trigger the error and check the UI
3. **Typecheck**: Run the type checker. Report results.
4. **Visual verification** (for UI changes): Actually look at the rendered output.

## What verification is NOT

- "I'm confident this works" — not verification
- "The code looks correct" — not verification
- "I tested a similar pattern before" — not verification
- "The types are correct so it must work" — not verification

Verification is running the actual check and observing the actual result.

## If a criterion cannot be verified

Flag it. Do not assume it passes. Report: "Cannot verify criterion X because [reason]. Recommend manual verification."
