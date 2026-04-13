---
description: Generate a pull request description from spec, plan and review
argument-hint: <ticket-number>
allowed-tools: Read, Write, Bash(mkdir -p *)
model: inherit
---
You are helping generate a pull request description for a completed feature.

The user input is: $ARGUMENTS

Parse the ticket number from the input (e.g. "PROJ-0001").

## Specs directory

Read the project config at: @.claude/specforge/config.json

If the file exists, use the value of the `specsDir` field as the base directory.
If the file does not exist, fall back to `.claude/specforge/` inside the current project.

The full path for this ticket is: <specs-dir>/<ticket-number>/

## Read the feature files

Read the following files:
- Spec:   <specs-dir>/<ticket-number>/spec.md (REQUIRED)
- Plan:   <specs-dir>/<ticket-number>/plan.md (REQUIRED)
- Review: <specs-dir>/<ticket-number>/review.md (OPTIONAL — skip spec coverage section if missing)

If spec or plan are missing, fail gracefully with a clear error message telling the user which files are required.

## Generate the PR description

Using the spec, plan, and review files, create a structured PR description with these sections:

### 1. Title
Derive from the spec's feature name (e.g. "Feature: PR Overview Command" → "Add PR Overview Command")
Keep it concise (under 70 characters).

### 2. Summary (2-3 sentences)
- What does this feature do and why does it exist? (take from spec Overview section)

### 3. Changes
Summarize what was done, grouped by the plan's task tags:
- **Backend:** (summarize [backend] tasks from the plan)
- **Frontend:** (summarize [frontend] tasks from the plan)
- **Shared:** (summarize [both] tasks from the plan)

If the plan lists specific files in its "Files to Create or Modify" section, include those. Otherwise summarize the types of changes.

### 4. Spec Coverage (if review.md exists)
Pull the key findings from the review file:
- What acceptance criteria were met?
- Any coverage gaps or caveats flagged by the review?
- Keep this section brief (2-3 bullets max)

If review.md does NOT exist, add a note: "_Note: No review has been run yet. You may want to run `/specforge:review <ticket-number>` before merging._"

### 5. Testing
Suggested test scenarios derived from the spec's Acceptance Criteria section:
- Extract 2-3 key test cases from the acceptance criteria
- Present as bullet points

### 6. Open Issues or Caveats
If the review flagged any outstanding issues, blockers, or limitations, list them here.
If none exist, omit this section.

## Constrain the output

Aim for under 500 words total. Use concise language. Do NOT include the full spec or plan in the PR description — synthesize only the highlights.

## Show to user for confirmation

Display the generated PR description in a clear format:

```
# [PR Title]

## Summary
...

## Changes
...

## Spec Coverage
...

## Testing
...
```

Then ask: **"Does this look right? Any changes before I save it?"**

Only save after the user confirms.

## Save the PR description

Save the PR description to: <specs-dir>/<ticket-number>/pr.md

After saving, tell the user:
- The file has been saved and they can use it when creating their PR
- Suggest: "You can create the PR with: `gh pr create --title \"<title>\" --body-file <path-to-pr.md>`"
- Suggest the next step: "Check the full status with `/specforge:status <ticket-number>`"

## Rules

1. Fail gracefully if spec or plan are missing (don't try to generate a PR without required context)
2. Treat review as optional — command still works without it, just notes the gap
3. The confirm-first pattern is mandatory — never save the file without explicit user confirmation
4. Do NOT run `gh pr create` or any command that creates external resources — only generate the file
