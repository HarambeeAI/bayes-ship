---
description: Define system architecture from approved PRD and research. Produces architecture doc, data model, API contracts, ADRs. Requires user approval. Must run after ship-spec.
disable-model-invocation: true
---

# Architecture — System design

## Pre-condition

`.claude/state/PRD.md` must exist and be approved (check progress.json).

## Process

1. Read PRD.md + RESEARCH.md + PROJECT.md.
2. Produce ARCHITECTURE.md with:
   - **Tech stack** with rationale for each choice
   - **System architecture** (component-level: frontend, backend, database, external services)
   - **Data model** (full schema definitions with types, relations, constraints)
   - **API endpoints** (method, path, request/response shape, auth requirements)
   - **Infrastructure layout** (hosting: Railway, Vercel, Supabase, etc.)
   - **Architecture Decision Records** for non-obvious choices:
     ```
     ADR-001: [Decision title]
       Context: [Why this decision was needed]
       Decision: [What was decided]
       Rationale: [Why this option over alternatives]
       Consequences: [What this means for implementation]
     ```
3. Save to `.claude/state/ARCHITECTURE.md` and `.claude/state/decisions.md`.
4. Create `.claude/state/ROADMAP.md` — phases ordered by dependency from the PRD.
5. Present architecture to the user for approval (approve / adjust).
6. After approval, present build mode selection:

   "How do you want to proceed with implementation?
     A. Guided mode (default) — I plan each phase and wait for your go.
     B. Autonomous mode — I build everything without stopping. /bayes-ship:ship-auto
     C. Semi-autonomous — I pause between phases for a checkpoint. /bayes-ship:ship-auto --checkpoints"

7. Update progress.json: set `phases.architecture.status` to `"approved"`.
8. Output next step based on user's build mode choice.

## Gate

User MUST explicitly approve architecture before any implementation.
