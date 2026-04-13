---
description: Fetch a Jira ticket and create a spec (experimental — not yet tested in production)
argument-hint: <ticket-number>
allowed-tools: Read, mcp__atlassian__getJiraIssue, mcp__mcp-atlassian__jira_get_issue
model: inherit
---
You are fetching a Jira ticket and preparing its details for spec creation.

The ticket number is: $ARGUMENTS

## Step 1 — Detect which MCP server is available

Check in this order:

1. Is `mcp__atlassian__getJiraIssue` available? → use the official Atlassian Rovo MCP server
2. Is `mcp__mcp-atlassian__jira_get_issue` available? → use the community mcp-atlassian server
3. Neither available → stop and tell the user:
   - Jira integration requires an MCP server to be configured
   - See the README under **Jira Integration** for setup instructions for both options
   - Once configured, restart Claude Code and run this command again

## Step 2 — Fetch the ticket

Call the available tool with the issue key $ARGUMENTS:
- Official server: `mcp__atlassian__getJiraIssue` with issueIdOrKey set to the ticket number
- Community server: `mcp__mcp-atlassian__jira_get_issue` with issue_key set to the ticket number

If the call returns an error (ticket not found, not authenticated, etc.), stop and show the error clearly. Do not proceed.

## Step 3 — Parse the ticket

From the response, extract:
- **Title** — the issue summary
- **Description** — the full issue description, including any acceptance criteria

Show the user a clear summary of what you found and ask:
- "Does this look right? Shall I create the spec from this?"
- Only proceed after confirmation

## Step 4 — Suggest next step

Once confirmed, show the user the ready-to-run command:

"Ready to create the spec? Run:"

`/specforge:create $ARGUMENTS | <title> | <description>`

Where `<title>` and `<description>` are filled in with the values extracted from the ticket.

Do not create the spec file yourself. Do not invoke `/specforge:create` directly.
