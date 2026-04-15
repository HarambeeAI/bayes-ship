---
description: Enforces RED-GREEN-REFACTOR TDD cycle during implementation. Write the failing test first, watch it fail, write minimal code, watch it pass, commit. Code written before tests must be deleted and restarted. Use when writing any implementation code within the Bayes Ship pipeline.
---

# Test-Driven Development

## The cycle

1. **RED**: Write a test that describes the desired behavior. Run it. Watch it FAIL. If it passes, either the test is wrong or the feature already exists.
2. **GREEN**: Write the MINIMUM code to make the test pass. No more. Not "while I'm here" additions. Not "it would be nice to also" extras. The minimum.
3. **REFACTOR**: Clean up the code. Extract functions, rename variables, remove duplication. Tests must still pass after refactoring.
4. **COMMIT**: One atomic commit. Conventional commit message.

## Hard rules

- If you catch yourself writing implementation before a test: STOP. Delete the implementation. Write the test first. This is non-negotiable in strict mode.
- Every test must fail before the implementation exists. A test that passes immediately is not testing what you think.
- "I'll add tests after" is not TDD. It is test-after development and produces weaker tests that verify what was built rather than what was specified.

## Anti-patterns to avoid

- **Testing implementation instead of behavior**: Test what the function DOES, not how it does it.
- **Tests that never fail**: If a test can't fail, it's not a test.
- **Giant test setup**: If setup is 50 lines and assertion is 1 line, the unit under test is too large.
- **Testing private methods**: Test the public interface. Private methods are implementation details.
- **Mocking everything**: If you mock the database, the HTTP client, and the filesystem, you're testing that your mocks work, not your code.

## When tdd_mode is relaxed

In relaxed mode, tests may be written after implementation. The RED-GREEN-REFACTOR cycle becomes optional. However, tests must still exist before the task is marked DONE.
