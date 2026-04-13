---
sidebar_position: 6
title: File structure per ticket
---

## File Structure Per Ticket

```
<specs-dir>/
  PROJ-1234/
    spec.md      ← what to build (source of truth)
    plan.md      ← how to build it (tagged checklist)
    review.md    ← what was verified
```

Where `<specs-dir>` is configured via `/specforge:config`, or `.claude/specforge/` inside your project if not set.
