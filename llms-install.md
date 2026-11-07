# AI SEO Tracker MCP Server — Automated Installation Guide

This guide is written for AI coding agents (Cline, Cursor, Claude Code, Codex, Windsurf, etc.) to install and configure the AI SEO Tracker MCP server automatically. Follow the steps in order.

## What this server does

AI SEO Tracker (`https://aiseotracker.com/api/agent/mcp`, alias `https://aiseotracker.com/mcp`) is a remote Streamable HTTP MCP server that gives an AI agent live access to brand visibility, citations, tracked prompts, missing third-party sources, answer snippets, and competitor benchmarks across ChatGPT, Perplexity, Gemini, Copilot, and Google AI Overviews.

## Prerequisites

None for installation. It is a hosted Streamable HTTP server using OAuth 2.1 in the browser (no static API keys or local dependencies required).

## Installation

Add the remote HTTP server to the client's MCP configuration:

### Cline / Cursor / Claude Desktop (`mcpServers` JSON)

```json
{
  "mcpServers": {
    "aiseotracker": {
      "url": "https://aiseotracker.com/api/agent/mcp"
    }
  }
}
```

### Claude Code CLI

```bash
claude mcp add --transport http aiseotracker https://aiseotracker.com/mcp
```

### VS Code (`.vscode/mcp.json`)

```json
{
  "servers": {
    "aiseotracker": {
      "type": "http",
      "url": "https://aiseotracker.com/api/agent/mcp"
    }
  }
}
```

### OpenAI Codex CLI (`~/.codex/config.toml`)

```toml
[mcp_servers.aiseotracker]
url = "https://aiseotracker.com/api/agent/mcp"
```

## Verify the installation

1. Connect the server and complete the browser OAuth sign-in when prompted.
2. Call `aiseo_list_projects` with `{}`. A working connection returns the signed-in user's accessible projects and their `projectSlug` values (or an empty list ready for `aiseo_create_project`).
