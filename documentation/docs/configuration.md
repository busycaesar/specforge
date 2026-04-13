---
sidebar_position: 4
title: Configuration
---

## Configuration

All specs, plans and reviews are saved to a single directory, one subfolder per ticket:

```
<specs-dir>/
  PROJ-1234/
    spec.md
    plan.md
    review.md
```

### Setting the specs directory

By default all files are saved inside the current project at `.claude/specforge/`. To save to a different location, run:

```bash
/specforge:config ~/Documents/Features
```

This creates `.claude/specforge/config.json` in your project:

```json
{
  "specsDir": "~/Documents/Features"
}
```

Both `~` and absolute paths work. The config is per-project — each project can point to a different location. If `/specforge:config` has never been run, all files save to `.claude/specforge/` inside the current project.

If you want teammates to use the same location, commit `.claude/specforge/config.json` to version control. If you want it to be personal, add it to `.gitignore`.
