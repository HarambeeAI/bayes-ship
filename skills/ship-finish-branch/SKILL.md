---
description: Complete a feature branch with merge/PR/keep/discard options. Verifies tests pass, cleans up worktree. Run after ship-test.
disable-model-invocation: true
---

# Finish branch — Complete the feature

## Process

1. Verify all tests pass on the current feature branch.
2. Present options:
   - A. **Merge to main** (squash commits into one)
   - B. **Merge to main** (keep individual commits)
   - C. **Open a Pull Request** (for team review)
   - D. **Keep the branch** (come back later)
   - E. **Discard** (delete branch and worktree)
3. Execute the chosen option.
4. Clean up the git worktree after merge or discard.
5. Update `progress.json` with branch status.
6. Output: "Run `/bayes-ship:ship-deploy` to deploy to production."
