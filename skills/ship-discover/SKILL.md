---
description: Interactive requirements gathering through structured Q&A with the user. Extracts features, user flows, data model, integrations, constraints, and non-goals. Produces CONTEXT.md. Must run after /bayes-ship:ship-new.
disable-model-invocation: true
---

# Discovery — Interactive requirements gathering

<HARD-GATE>
Do NOT invoke any implementation skill, write any code, scaffold any project, or take any implementation action until the user has approved the design. This is enforced, not suggested.
</HARD-GATE>

## Pre-condition

Read `.claude/state/PROJECT.md`. If it does not exist, tell the user to run `/bayes-ship:ship-new` first.

## Process

### Step 1: Scope assessment

Before asking detailed questions, assess scope. If the request describes multiple independent subsystems (e.g., "build a platform with chat, file storage, billing, and analytics"), flag this immediately:

"This project has multiple independent subsystems. Before we dive into details, let's decompose into sub-projects. What are the independent pieces, how do they relate, and what order should they be built?"

If the project is too large for a single spec, help the user decompose into sub-projects. Each sub-project gets its own spec, plan, and implementation cycle. Then proceed to discover the first sub-project.

### Step 2: Visual companion offer

If upcoming questions may involve visual content (mockups, layout choices, architecture diagrams), offer the visual companion once:

"Some of what we're working on might be easier to explain if I can show it to you in a web browser. I can put together mockups, diagrams, and comparisons as we go. Want to try it?"

This offer MUST be its own message. Do not combine it with other questions. If the user declines, proceed with text-only discovery. Per question, decide whether the browser or terminal is more appropriate: layout comparisons go to browser; conceptual choices stay in terminal.

### Step 3: Progressive Q&A

Ask questions ONE AT A TIME. Prefer multiple choice when possible. Use lettered options (A/B/C/D) so the user can respond quickly.

Cover these 6 areas in order:
1. **Core features** — What must the product do? What are the key actions?
2. **User flows** — What are the critical paths? What does the user do step by step?
3. **Data model** — What entities exist? How do they relate?
4. **Integrations** — M-Pesa? External APIs? Auth requirements? Third-party services?
5. **Constraints** — Budget, timeline, regulatory (KRA/eTIMS), offline-first, mobile-responsive?
6. **Non-goals** — What should the product explicitly NOT do in this version?

### Step 4: Coverage check

After all areas are covered, present a summary of everything captured and ask:
- "Anything to add or correct?"
- A. Looks good, I'm done
- B. I want to add something
- C. I want to change something

Track coverage internally. Before writing CONTEXT.md, flag any uncovered areas.

### Step 5: Write CONTEXT.md

Write `.claude/state/CONTEXT.md` with all captured requirements organized by the 6 areas above.

Update `.claude/state/progress.json`: set `phases.discovery.status` to `"complete"` with timestamp.

Output: "Ready. Run `/bayes-ship:ship-research` to investigate tech choices."

## Assumptions mode

If the user has set `discover_mode` to `"assumptions"` in `/bayes-ship:ship-settings`, switch to assumptions mode:
1. Read existing code, docs, and PROJECT.md
2. Surface what you would do and why
3. Ask the user to correct only what is wrong
4. Write CONTEXT.md from the assumptions + corrections

## Rules

- NEVER assume. If ambiguous, ask.
- NEVER suggest implementation details (that is the Architect's job).
- Maximum 1 question per message. Do not overwhelm.
- After each answer, briefly acknowledge what was captured before asking the next question.
