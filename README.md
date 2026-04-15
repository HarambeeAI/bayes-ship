# Bayes Ship

End-to-end product delivery system for Claude Code. From idea to deployed prototype in one pipeline.

## What it does

Bayes Ship takes a product from raw idea to deployed software through a structured pipeline: Discovery Q&A → Parallel Research → PRD Generation → Architecture → TDD Implementation with Two-Stage Review → QA → Deployment.

It combines GSD's context engineering with Superpowers' execution discipline, adds a comprehensive pre-implementation pipeline and domain-specific skills for African markets.

## Installation

### Option 1: From marketplace (recommended)

```
/plugin marketplace add HarambeeAI/bayes-ship
/plugin install bayes-ship@bayes-ship-marketplace
```

### Option 2: Local development

```
git clone https://github.com/HarambeeAI/bayes-ship.git
claude --plugin-dir ./bayes-ship
```

## Quick Start

```
/bayes-ship:ship-new          # Initialize project
/bayes-ship:ship-discover      # Requirements Q&A
/bayes-ship:ship-research      # Parallel tech research (autonomous)
/bayes-ship:ship-spec          # Generate PRD (you approve)
/bayes-ship:ship-architect     # System architecture (you approve, then choose build mode)
/bayes-ship:ship-auto          # Autonomous build — walk away, come back to a prototype
```

## The Pipeline

| Phase | Command | Your Input | What Happens |
|-------|---------|-----------|--------------|
| Setup | `/bayes-ship:ship-new` | Name, description, users, platform | Creates project scaffold |
| Discovery | `/bayes-ship:ship-discover` | ~9 Q&A answers (A/B/C/D) | Extracts requirements |
| Research | `/bayes-ship:ship-research` | None (autonomous) | Parallel agents research tech choices |
| Spec | `/bayes-ship:ship-spec` | Approve PRD | Generates and reviews PRD |
| Architecture | `/bayes-ship:ship-architect` | Approve architecture | Defines stack, data model, APIs, ADRs |
| Build | `/bayes-ship:ship-auto` | None (autonomous) | Plans, builds, tests, reviews all phases |
| QA | `/bayes-ship:ship-test` | None (autonomous) | Verification, coverage, security scan |
| Finish | `/bayes-ship:ship-finish-branch` | Merge strategy | Merge/PR/keep/discard |
| Deploy | `/bayes-ship:ship-deploy` | Credentials | Railway + Vercel deployment |

## Build Modes

After architecture approval, choose how to build:

- **Guided** (default): You run `/bayes-ship:ship-plan N` then `/bayes-ship:ship-execute` for each phase. Full control.
- **Autonomous** (`/bayes-ship:ship-auto`): Builds everything without stopping. TDD and two-stage review still mandatory. Decisions logged to `auto-decisions.md`.
- **Semi-autonomous** (`/bayes-ship:ship-auto --checkpoints`): Pauses between phases for a status check.

## What Makes It Different

### From GSD
- Full product lifecycle (GSD starts at planning, we start at discovery)
- Parallel research agents
- Architecture Decision Records
- Deployment pipeline
- Autonomous build mode

### From Superpowers
- Structured command-driven workflow (vs invisible auto-triggering)
- Domain-specific skills (M-Pesa, BetterAuth, Next.js, Nest.js)
- File-based state persistence across sessions
- Research phase with parallel agents
- Autonomous build mode
- Deployment pipeline

### Adopted from Superpowers
- Two-stage review (spec compliance → code quality)
- Strict TDD (RED-GREEN-REFACTOR)
- Git worktree isolation
- Systematic 4-phase debugging
- Verification-before-completion
- YAGNI enforcement
- "Human partner" agent framing

## Included Skills (44 total)

### Core Process (always installed — 9 skills)
`test-driven-development` · `systematic-debugging` · `git-worktrees` · `verification-before-completion` · `two-stage-review` · `context-engineering` · `atomic-planning` · `coding-standards-ecc`* · `output-skill`*

### Frontend (optional — 5 skills)
`taste-skill`* · `soft-skill`* · `minimalist-skill`* · `redesign-skill`* · `frontend-patterns`*

### Backend (optional — 2 skills)
`backend-patterns`* · `api-design`*

### AI/Agents (optional — 8 skills)
`langchain-fundamentals`** · `langchain-middleware`** · `langchain-rag`** · `langchain-dependencies`** · `langgraph-fundamentals`** · `langgraph-persistence`** · `agentic-engineering`* · `agent-eval`*

### Quality (optional — 3 skills)
`security-scan`* · `accessibility`* · `ai-regression-testing`*

### Auth & Payments (optional — 3 skills, Bayes custom)
`betterauth-skill` · `daraja-mpesa` · `paystack-skill`

### DevOps (optional — 2 skills, Bayes custom)
`devops-skill` · `railway-deploy`

### Pipeline Commands (17 skills)
`ship-new` · `ship-discover` · `ship-research` · `ship-spec` · `ship-architect` · `ship-worktree` · `ship-plan` · `ship-execute` · `ship-auto` · `ship-test` · `ship-review` · `ship-finish-branch` · `ship-deploy` · `ship-status` · `ship-resume` · `ship-bugfix` · `ship-settings`

*\* Adapted from [everything-claude-code](https://github.com/affaan-m/everything-claude-code) (ECC) or [taste-skill](https://github.com/Leonxlnx/taste-skill)*
*\*\* Adapted from [langchain-skills](https://github.com/langchain-ai/langchain-skills)*

## Agents

14 specialized agents dispatched as subagents with clean context windows:

`discovery-agent` · `researcher` · `synthesizer` · `architect` · `spec-document-reviewer` · `planner` · `implementer` · `spec-compliance-reviewer` · `code-quality-reviewer` · `verifier` · `deployer` · `debugger` · `documenter` · `tester`

## Settings

```
/bayes-ship:ship-settings                    # View all settings
/bayes-ship:ship-settings tdd_mode relaxed   # Change a setting
```

| Setting | Default | Options |
|---------|---------|---------|
| `build_mode` | `guided` | guided / autonomous / semi-autonomous |
| `tdd_mode` | `strict` | strict / relaxed |
| `discover_mode` | `interview` | interview / assumptions |
| `auto_chain` | `false` | true / false |
| `use_worktrees` | `true` | true / false |
| `model_routing` | `auto` | auto / manual |
| `payment_provider` | `daraja` | daraja / paystack / stripe |
| `auth_provider` | `betterauth` | betterauth / nextauth / custom |

## Utility Commands

```
/bayes-ship:ship-status    # Progress dashboard
/bayes-ship:ship-resume    # Resume after session restart
/bayes-ship:ship-bugfix    # Systematic bug diagnosis + fix
/bayes-ship:ship-review    # Manual two-stage code review
```

## State Files

All state persists in `.claude/state/`:

```
.claude/state/
├── PROJECT.md          # Project basics
├── CONTEXT.md          # Discovery output
├── RESEARCH.md         # Research synthesis
├── PRD.md              # Approved spec
├── ARCHITECTURE.md     # Approved architecture
├── ROADMAP.md          # Phase ordering
├── decisions.md        # Architecture Decision Records
├── progress.json       # Task tracking (survives sessions)
├── settings.json       # User preferences
├── auto-decisions.md   # Autonomous mode decision log
├── REVIEW.md           # QA results
├── research/           # Individual research files
└── plans/              # Execution plans per phase
```

## Requirements

- Claude Code (latest version with plugin support)
- Git (for worktrees and version control)
- Node.js 18+ (for project setup)

## License

MIT

## Author

Bayes Consulting — [bayesconsulting.com](https://bayesconsulting.com)
Built by Antony Sure
