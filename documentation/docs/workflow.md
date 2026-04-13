---
sidebar_position: 5
title: Workflow
---

## Workflow

### 0. Configure specs location — `/specforge:config` (optional)

By default specs save to `.claude/specforge/` inside your project. If you want to save them somewhere else — like a local folder or a shared team folder — run this once per project before you start:

```bash
/specforge:config ~/Documents/Features
```

You only need to do this once. The path is saved to `.claude/specforge/config.json` and picked up automatically by all other commands.

---

### 1. Create a spec — `/specforge:create` or `/specforge:jira`

There are two ways to create a spec.

**From Jira** — if your team uses Jira and you have the `mcp-atlassian` MCP server configured:

```bash
/specforge:jira PROJ-1234
```

This fetches the ticket title, description and acceptance criteria directly via MCP and generates a spec from it. No copy-pasting required. See [Jira Integration](./jira-integration) for setup.

**Manually** — works without any external tools:

Before writing any code, you need a clear definition of what you're building. The create command takes your plain English description and turns it into a structured spec covering requirements, acceptance criteria, technical notes and out of scope items. Claude shows you the draft first and only saves it after you confirm, so you stay in control of what goes on record.

The command takes three parameters separated by `|`:

| Parameter | Description | Example |
|---|---|---|
| Ticket number | Your issue tracker ticket ID | `PROJ-1234` |
| Title | Short feature name | `Update header navigation` |
| Description | What needs to be built and why | `Change background color from white to black` |

```bash
/specforge:create PROJ-1234 | Update header navigation | Background color should change from white to black
```

The spec is saved to `<specs-dir>/PROJ-1234/spec.md`.

---

### 2. Plan the feature — `/specforge:plan`

A good plan prevents wasted effort. This command reads your spec and breaks the work into a prioritised, tagged checklist of tasks. Each task is tagged as `[backend]`, `[frontend]`, or `[both]` so there is zero ambiguity about who does what. Any tasks that span both backend and frontend produce a Shared Contracts section — field names, endpoint paths and data shapes that both sides must agree on before implementation starts. This is also where complexity is estimated and ambiguities are flagged before any code is written.

```bash
/specforge:plan PROJ-1234
```

The plan is saved to `<specs-dir>/PROJ-1234/plan.md`.

---

### 3. Implement the feature — `/specforge:implement`

Implementation runs in three phases using parallel sub-agents to avoid conflicts and maximise speed. In Phase 1, the backend and frontend agents work simultaneously on their respective tasks. In Phase 2, the shared agent handles anything that spans both sides, with full context of what was already built. The plan checklist is updated as each task completes so you always know where things stand.

```bash
/specforge:implement PROJ-1234
```

---

### 4. Review the implementation — `/specforge:review`

Once implementation is done, two review agents run in parallel. The spec coverage agent checks every requirement and acceptance criterion against the code, marking each as done, partial or missing, and flags any scope creep. The code quality agent looks for error handling gaps, security concerns and consistency issues. Their findings are merged into a single review report, giving you a clear picture of what shipped versus what was specified.

```bash
/specforge:review PROJ-1234
```

The review is saved to `<specs-dir>/PROJ-1234/review.md`.

---

### 5. Check status — `/specforge:status`

At any point in the workflow you can check where a feature stands. This command reads the spec, plan and review files and gives you a quick summary of task progress broken down by type, review coverage if a review exists, and the next recommended action.

```bash
/specforge:status PROJ-1234
```
