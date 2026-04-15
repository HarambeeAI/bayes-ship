---
description: Context rot prevention and subagent orchestration rules. Enforces clean context windows, file-based state, and delegation patterns. Auto-invoked during any Bayes Ship execution phase.
---

# Context Engineering

## Context rot

Quality degrades as the context window fills:
- 0-30% context: Peak quality. Thorough, comprehensive.
- 50%+: Starts rushing. Cuts corners.
- 70%+: Hallucinations. Forgotten requirements.

## Prevention rules

1. **Keep main session under 40% context.** Delegate heavy work to subagents.
2. **Every execution task runs in a fresh subagent.** Clean 200k token window per task.
3. **Subagents receive ONLY the context they need.** Never pass full session history.
4. **State lives in files, not conversation memory.**
   - PROJECT.md, CONTEXT.md, PRD.md, ARCHITECTURE.md, progress.json
   - Plans in .claude/state/plans/
   - Research in .claude/state/research/
5. **After compaction, re-read progress.json** to reconstruct state.
6. **Never say "I'll be more concise."** That is a context rot symptom. Spawn a subagent instead.

## Subagent isolation principle

When dispatching a subagent:
- Construct exactly the context it needs
- Do not inherit session history
- Do not pass unrelated state files
- The subagent should be able to do its job with only what you gave it

## File-based state protocol

| File | Written by | Read-only after |
|------|-----------|-----------------|
| PROJECT.md | /ship-new | Always |
| CONTEXT.md | /ship-discover | User approves discovery |
| RESEARCH.md | /ship-research | Always |
| PRD.md | /ship-spec | User approves spec |
| ARCHITECTURE.md | /ship-architect | User approves architecture |
| progress.json | Every phase | Never (always updated) |
