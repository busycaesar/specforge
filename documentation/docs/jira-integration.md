---
sidebar_position: 7
title: Jira integration
---

## Jira Integration

> **Experimental:** This feature has not yet been tested in production. Use with caution and report any issues.
>
> **This feature is optional.** Specforge works perfectly without it — use `/specforge:create` instead. Only set this up if you actively want to pull tickets directly from Jira.
>
> **Security notice:** Connecting any AI tool to Jira carries risk. The MCP server will act with your account's permissions. Be cautious, use the least-privileged option available to you, and never connect an account with admin access.

The `/specforge:jira` command supports two MCP servers. It will automatically use whichever one you have configured, preferring the official Atlassian server if both are present.

---

### Option A — Official Atlassian Rovo MCP (recommended)

Uses OAuth — no API token stored anywhere. Authenticates via browser using your Atlassian account.

**Caution:** The official server exposes all Jira tools (create, edit, transition, etc.), not just read. Specforge only calls `getJiraIssue`, but the full surface is available to Claude Code. Only use this if you are comfortable with that.

**Setup:**

```bash
claude mcp add --transport sse atlassian https://mcp.atlassian.com/v1/mcp
```

Restart Claude Code. A browser window will open to complete OAuth. Once authenticated, verify with `/mcp` — you should see `atlassian` listed.

Official docs: https://support.atlassian.com/atlassian-rovo-mcp-server/docs/getting-started-with-the-atlassian-remote-mcp-server/

---

### Option B — Community mcp-atlassian server

Uses an API token with two layers of restriction: `ENABLED_TOOLS` limits Claude Code to only `jira_get_issue`, and `READ_ONLY_MODE` blocks all write operations at the server level as a hard guarantee.

**Setup:**

1. Install `uv` if not already available: https://docs.astral.sh/uv/getting-started/installation/

2. Create an Atlassian API token at https://id.atlassian.com/manage-profile/security/api-tokens. Name it something like `specforge-readonly`.

3. Add to your `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "mcp-atlassian": {
      "command": "uvx",
      "args": ["mcp-atlassian"],
      "env": {
        "JIRA_URL": "https://your-company.atlassian.net",
        "JIRA_USERNAME": "your.email@company.com",
        "JIRA_API_TOKEN": "your_api_token",
        "ENABLED_TOOLS": "jira_get_issue",
        "READ_ONLY_MODE": "true"
      }
    }
  }
}
```

`ENABLED_TOOLS` restricts Claude Code to only the issue read tool. `READ_ONLY_MODE` adds a second independent layer — it blocks all write operations at the server level regardless of any other setting. Together they give the strongest possible read-only guarantee.

4. Restart Claude Code and verify with `/mcp` — you should see `mcp-atlassian` listed with only `jira_get_issue`.

Community server repo: https://github.com/sooperset/mcp-atlassian

---

### Usage

Once either option is configured:

```bash
/specforge:jira PROJ-1234
```

Specforge fetches the ticket, shows you what it found, and gives you a ready-to-run `/specforge:create` command with the details filled in.
