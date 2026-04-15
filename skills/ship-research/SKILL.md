---
description: Spawn parallel research subagents to investigate tech choices, libraries, and patterns based on discovered requirements. Produces RESEARCH.md. Must run after ship-discover.
disable-model-invocation: true
---

# Research — Parallel tech investigation

## Pre-condition

Read `.claude/state/CONTEXT.md`. If it does not exist, tell the user to run `/bayes-ship:ship-discover` first.

## Process

1. Analyze CONTEXT.md to identify 3-5 research areas. Examples:
   - "Best auth library for Next.js with M-Pesa integration"
   - "Card component patterns for invoice UI"
   - "Payment reconciliation: callback handling, retry logic"
   - "LangGraph vs CrewAI for agent orchestration"

2. Announce the research areas to the user:
   "I've identified N research areas. Spawning parallel researchers. This takes 2-3 minutes."

3. For each research area, dispatch a fresh subagent using the Task tool with:
   - The research question as the primary instruction
   - Relevant context from CONTEXT.md (only the parts related to this area)
   - Instruction to write findings to `.claude/state/research/{area-slug}.md`

4. Wait for all researchers to complete.

5. Dispatch a Synthesizer subagent that:
   - Reads all files in `.claude/state/research/`
   - Produces `.claude/state/RESEARCH.md` with:
     - Recommendations per area (what to use and why)
     - Trade-offs considered
     - Selected approaches
     - Links to relevant documentation

6. Update `progress.json`: set `phases.research.status` to `"complete"`.

7. Present key recommendations to the user as a brief summary.

8. Output: "Ready. Run `/bayes-ship:ship-spec` to generate the PRD."

## Rules

- This phase is fully autonomous. Do not ask the user for input during research.
- Each researcher subagent gets a clean context. Do not pass the full CONTEXT.md to every researcher — only the relevant section.
- If a research area yields no useful results, note this in RESEARCH.md rather than guessing.
