---
description: Show the current status of a feature — plan progress, review coverage and next recommended action
argument-hint: <ticket-number>
allowed-tools: Read
model: inherit
---
You are showing the current status of a feature.

## Specs directory

Read the project config at: @.claude/specforge/config.json

If the file exists, use the value of the `specsDir` field as the base directory.
If the file does not exist, fall back to `.claude/specforge/` inside the current project.

The full path for this ticket is: <specs-dir>/$ARGUMENTS/

Read the following files if they exist:
- Spec:   <specs-dir>/$ARGUMENTS/spec.md
- Plan:   <specs-dir>/$ARGUMENTS/plan.md
- Review: <specs-dir>/$ARGUMENTS/review.md
- PR:     <specs-dir>/$ARGUMENTS/pr.md

Output a summary showing:
- Feature name and one-line description
- Plan status: how many tasks are complete vs total, broken down by [backend], [frontend], [both]
- Review status: if a review exists, show the requirement coverage summary
- Next recommended action:
  - If no plan exists → suggest `/specforge:plan $ARGUMENTS`
  - If plan exists but no review → suggest `/specforge:implement $ARGUMENTS`
  - If review exists but no pr.md → suggest `/specforge:pr $ARGUMENTS`
  - If pr.md exists → feature is ready for PR creation
