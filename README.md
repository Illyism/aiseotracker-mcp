# AI SEO Tracker MCP Server: AI Search Visibility, Citations & Prompts

Give Claude Code, Cursor, Claude Desktop, Codex, or any MCP client live access to how ChatGPT, Perplexity, Gemini, Microsoft Copilot, and Google AI Overviews rank and cite a brand. Read tracked prompts, citation URLs, missing third-party sources (listicles, comparisons, Reddit threads, directories), brand answer snippets, competitor benchmarks, and scan status over Streamable HTTP with browser OAuth (no static API keys).

```bash
claude mcp add --transport http aiseotracker https://aiseotracker.com/mcp
```

This repository holds the setup files, plugin manifests, and agent skill for the hosted AI SEO Tracker MCP server at `https://aiseotracker.com/api/agent/mcp` (and its short alias `https://aiseotracker.com/mcp`).

## Quick Setup

There are no static API keys to copy or rotate. Add the server URL in your MCP client and approve the sign-in in your browser on first use.

### Claude Code

```bash
claude mcp add --transport http aiseotracker https://aiseotracker.com/mcp
```

Run `/mcp` inside Claude Code and select `aiseotracker` to complete browser OAuth.

### Cursor

Save this in `.cursor/mcp.json` in your project (or `~/.cursor/mcp.json` globally):

```json
{
  "mcpServers": {
    "aiseotracker": {
      "url": "https://aiseotracker.com/api/agent/mcp"
    }
  }
}
```

Reload Cursor, open **Cursor Settings → MCP**, and click **Connect** next to `aiseotracker`.

### Claude Desktop and claude.ai

Open **Customize → Connectors**, select **+ Add → Add custom connector**, and enter:

- **Name:** `AI SEO Tracker`
- **URL:** `https://aiseotracker.com/api/agent/mcp`

Connect and approve the browser OAuth prompt.

### VS Code

Add this to `.vscode/mcp.json` (VS Code uses `servers` instead of `mcpServers`):

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

### OpenAI Codex CLI

Add this to `~/.codex/config.toml`:

```toml
[mcp_servers.aiseotracker]
url = "https://aiseotracker.com/api/agent/mcp"
```

### Claude Code Plugin & Agent Skill

This repository is also a Claude Code plugin marketplace and agent skill package:

```bash
claude plugin marketplace add Illyism/aiseotracker-mcp
claude plugin install aiseotracker@aiseotracker-mcp
```

Or install the skill standalone for any agent:

```bash
npx skills add Illyism/aiseotracker-mcp
```

## Tools

AI SEO Tracker exposes 12 tools namespaced with `aiseo_`. Eight are read-only and four perform safe actions (`aiseo_create_project`, `aiseo_add_prompts`, `aiseo_activate_plan` checkout link handoff, and `aiseo_submit_feedback`).

| Tool | Type | Description |
| --- | --- | --- |
| `aiseo_list_projects` | Read | List every AI SEO Tracker project the signed-in user can access. Call this first to discover `projectSlug`. |
| `aiseo_get_scan` | Read | Check whether a project scan is `running` or `done` (`done` means no in-flight jobs), with `scannedPromptCount` and `promptCount`. |
| `aiseo_list_prompts` | Read | List tracked prompts with brand visibility status (`winning`, `opportunity`, `missing`) and per-model ranks across ChatGPT, Perplexity, Gemini, Copilot, and AI Overviews. |
| `aiseo_list_sources` | Read | List citation sources (URLs and domains that AI answers cite) for a project, with optional `domain` substring filter. |
| `aiseo_list_missing_sources` | Read | List third-party pages AI answers cite for the project's prompts (listicles, comparisons, Reddit threads, directories) where the project's own site is not yet cited. |
| `aiseo_get_source_history` | Read | Inspect citation history for one URL across AI engines to see whether it is still cited and when it dropped. |
| `aiseo_list_snippets` | Read | Read exact AI answer snippets where a brand (own brand by default, or a competitor) is mentioned. |
| `aiseo_list_competitors` | Read | List competitor brands tracked on a project (names, slugs, visibility status, domains). |
| `aiseo_add_prompts` | Write | Add tracked prompts to a project (up to 20 per call) within plan limits. Only call for queries the user asked to track. |
| `aiseo_create_project` | Write | Create a project for a website URL (or return the existing project for that domain), with optional competitors and initial prompts. |
| `aiseo_activate_plan` | Write | Preview pricing or generate a Polar browser checkout link for the $49 one-time audit, Basic ($79/mo), or Pro ($199/mo). Never charges a card directly. |
| `aiseo_submit_feedback` | Write | Send a bug report, feature request, or question to the AI SEO Tracker team. |

## Recommended Agent Workflow

1. Call `aiseo_list_projects` first to obtain the `projectSlug`. If the user has no project yet, call `aiseo_create_project`.
2. Call `aiseo_get_scan` if a scan may still be running.
3. Query `aiseo_list_prompts`, `aiseo_list_sources`, `aiseo_list_missing_sources`, `aiseo_list_snippets`, and `aiseo_list_competitors` to analyze visibility gaps and outreach targets.
4. For a structured executive audit brief, fetch `https://aiseotracker.com/ai-seo-report/SKILL.md`. For a missing-prompt content campaign, fetch `https://aiseotracker.com/content-coverage/SKILL.md`.

## Try It

Once connected, ask your agent:

```text
Call aiseo_list_projects. For my project, list the missing prompts, the top third-party missing sources we should get cited on, and how AI models currently describe our brand vs competitors.
```

## Links & Support

- **Documentation:** [https://aiseotracker.com/docs/mcp](https://aiseotracker.com/docs/mcp)
- **Tools Reference:** [https://aiseotracker.com/docs/tools](https://aiseotracker.com/docs/tools)
- **Agent Setup Skill:** [https://aiseotracker.com/SKILL.md](https://aiseotracker.com/SKILL.md)
- **Website:** [https://aiseotracker.com](https://aiseotracker.com)
- **Help & Support:** [https://aiseotracker.com/help](https://aiseotracker.com/help) · `support@aiseotracker.com`
