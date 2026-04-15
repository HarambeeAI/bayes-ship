---
description: View and configure Bayes Ship framework preferences. Settings persist in .claude/state/settings.json.
disable-model-invocation: true
---

# Settings — Configure framework preferences

## Available settings

| Setting | Default | Options | Description |
|---------|---------|---------|-------------|
| `build_mode` | `guided` | `guided`, `autonomous`, `semi-autonomous` | How the build phase runs |
| `discover_mode` | `interview` | `interview`, `assumptions` | Q&A style during discovery |
| `tdd_mode` | `strict` | `strict`, `relaxed` | Strict deletes pre-test code |
| `auto_commit` | `true` | `true`, `false` | Commit after each task |
| `auto_deploy` | `false` | `true`, `false` | Deploy in autonomous mode if creds available |
| `auto_chain` | `false` | `true`, `false` | Auto-advance to next phase after approval |
| `parallel_waves` | `true` | `true`, `false` | Run independent tasks in parallel |
| `use_worktrees` | `true` | `true`, `false` | Use git worktrees for isolation |
| `model_routing` | `auto` | `auto`, `manual` | Use plan model tags or current model |
| `default_stack` | `next` | `next`, `nest`, `fastapi`, `custom` | Preferred tech stack |
| `payment_provider` | `daraja` | `daraja`, `paystack`, `stripe` | Default payment integration |
| `auth_provider` | `betterauth` | `betterauth`, `nextauth`, `custom` | Default auth library |
| `visual_companion` | `true` | `true`, `false` | Browser mockups during discovery |

## Usage

- `/bayes-ship:ship-settings` — show current settings
- `/bayes-ship:ship-settings tdd_mode relaxed` — change a setting
- `/bayes-ship:ship-settings reset` — reset to defaults

Settings are saved to `.claude/state/settings.json`.
