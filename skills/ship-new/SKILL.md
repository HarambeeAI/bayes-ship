---
description: Initialize a new Bayes Ship product project. Creates state directory, captures project basics, detects relevant skills. Run this first before any other ship command.
disable-model-invocation: true
---

# Initialize new product

Create the project scaffold for a Bayes Ship product delivery pipeline.

## Process

1. Create `.claude/state/` directory if it does not exist.
2. Ask the user for:
   - **Project name** (short, lowercase, kebab-case, used for git branches and directories)
   - **One-line description** (what does this product do, in plain language)
   - **Target users**: A. Businesses (B2B) / B. Consumers (B2C) / C. Internal team tool / D. Other
   - **Primary platform**: A. Web app / B. Mobile app / C. API/backend service / D. Desktop app
3. Scan installed skills and flag which are relevant to this project based on the description and platform.
4. Write `.claude/state/PROJECT.md` with all captured information.
5. Initialize `.claude/state/progress.json`:

```json
{
  "project": "<project-name>",
  "current_phase": "setup",
  "phases": {},
  "last_checkpoint": "<timestamp>",
  "git_commits": 0
}
```

6. Output: "Ready. Run `/bayes-ship:ship-discover` to start requirements gathering."

## Rules

- Do NOT start any implementation, brainstorming, or research. Only capture project basics.
- Keep it fast. This should take under 1 minute.
- If the project directory already has a `.claude/state/PROJECT.md`, warn the user and ask if they want to overwrite.
