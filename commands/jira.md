---
description: Fetch a Jira ticket and create a feature spec from it automatically
argument-hint: <ticket-number>
allowed-tools: Read, Write, Bash(jira *), Bash(which *), Bash(mkdir -p *)
model: inherit
---
You are creating a feature spec by fetching a Jira ticket.

The ticket number is: $ARGUMENTS

## Step 1 — Check Jira CLI is available

Run `which jira` to check if the Jira CLI is installed.

If it is not found, stop and tell the user:
- The Jira CLI is required for this command
- Install it from: https://github.com/ankitpokhrel/jira-cli
- Once installed and authenticated, run this command again

## Step 2 — Fetch the ticket

Run `jira issue view $ARGUMENTS --plain` to fetch the ticket details.

If the command returns an error (ticket not found, not authenticated, etc.), stop and show the error clearly. Do not proceed.

## Step 3 — Parse the ticket

From the output, extract:
- **Title** — the issue summary/title
- **Description** — the full issue description
- **Acceptance criteria** — if present as a field or section in the description
- **Any other relevant fields** — priority, labels, issue type if present

Show the user what you found in a clear summary and ask:
- "Does this look right? Shall I create the spec from this?"
- Only proceed after confirmation

## Step 4 — Create the spec

Using the parsed ticket details, generate a spec following this template:

# Feature: <title>
**Ticket:** $ARGUMENTS

## Overview
2-3 sentences describing what this feature is and why it exists.

## Requirements
- [ ] (derived from the ticket description and acceptance criteria)

## Acceptance Criteria
- (taken directly from the ticket where present, otherwise derived from the description)

## Technical Notes
- (any implementation hints found in the ticket)

## Out of Scope
- (explicitly list what this feature does NOT cover)

Rules:
1. Prefer content directly from the ticket over inference where possible
2. Flag anything you assumed with a note like "(assumed)"
3. Do NOT save the file yet — show the spec to the user first and ask for confirmation

## Step 5 — Save the spec

Read the project config at: @.claude/specforge/config.json

If the file exists, use the value of the `specsDir` field as the base directory.
If the file does not exist, fall back to `.claude/specforge/` inside the current project.

Save to: <specs-dir>/$ARGUMENTS/spec.md

Create the ticket directory if it does not already exist.
