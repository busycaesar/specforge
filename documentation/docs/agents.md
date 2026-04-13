---
sidebar_position: 8
title: Agents
---

## Agents

SpecForge ships with generic agents that work for any project out of the box:

| Agent | Role |
|---|---|
| `backend` | Controllers, models, migrations, routes |
| `frontend` | Templates, styles, JavaScript |
| `shared` | Files that span both backend and frontend e.g. templates needing data bindings and markup |
| `spec-coverage` | Verifies every requirement was implemented |
| `code-quality` | Reviews for errors, security, consistency |

The implement command runs them in phases:

```
Phase 1: backend + frontend  (parallel)
Phase 2: shared              (after Phase 1)
```

### Customising Agents

SpecForge automatically detects project-level agents before falling back to the bundled ones. If you create any of the following in your project, they will be used instead:

```
your-project/
  .claude/
    agents/
      backend.md        ← overrides bundled backend agent
      frontend.md       ← overrides bundled frontend agent
      shared.md         ← overrides bundled shared agent
      spec-coverage.md  ← overrides bundled spec-coverage agent
      code-quality.md   ← overrides bundled code-quality agent
```

You only need to create the agents you want to customise — any agent not found in your project falls back to the bundled version automatically.

For stack-wide conventions that apply across all agents (coding standards, framework preferences, file structure), add them to your project's `CLAUDE.md` — all agents will pick these up without needing to be customised.
