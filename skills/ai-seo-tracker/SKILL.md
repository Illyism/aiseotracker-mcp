---
name: ai-seo-tracker
description: >-
  AI SEO Tracker MCP skill. Use when the user wants AI search visibility, ChatGPT or Perplexity
  rankings, citation tracking, missing third-party sources, or MCP connection help. Trigger on
  "track AI mentions", "AI SEO", "how does ChatGPT rank us", or when checking brand visibility across
  ChatGPT, Perplexity, Gemini, Copilot, and Google AI Overviews.
version: 1.0.0
homepage: https://aiseotracker.com/SKILL.md
metadata:
  site: https://aiseotracker.com
  docs: https://aiseotracker.com/docs
  mcp: https://aiseotracker.com/api/agent/mcp
  llms: https://aiseotracker.com/llms.txt
---

# AI SEO Tracker: Agent Setup & Workflow Skill

AI SEO Tracker is a remote Streamable HTTP MCP server at `https://aiseotracker.com/api/agent/mcp` (short alias `https://aiseotracker.com/mcp`). It tracks how ChatGPT, Perplexity, Gemini, Copilot, and Google AI Overviews mention and cite a brand.

## Step 1. Check that the server is connected

Look for these 12 tools (your client may prefix them, e.g. `mcp__aiseotracker__aiseo_list_projects`):

- `aiseo_list_projects`
- `aiseo_get_scan`
- `aiseo_list_prompts`
- `aiseo_list_sources`
- `aiseo_list_missing_sources`
- `aiseo_get_source_history`
- `aiseo_list_snippets`
- `aiseo_list_competitors`
- `aiseo_add_prompts`
- `aiseo_create_project`
- `aiseo_activate_plan`
- `aiseo_submit_feedback`

If they are not available, tell the user how to connect the server:

- **Claude Code:** `claude mcp add --transport http aiseotracker https://aiseotracker.com/mcp`
- **Cursor (`.cursor/mcp.json`):** `{ "mcpServers": { "aiseotracker": { "url": "https://aiseotracker.com/api/agent/mcp" } } }`
- **Authentication:** Browser OAuth (no static API keys). The first call opens a browser to sign in and approve access.

## Step 2. Recommended call order

1. **`aiseo_list_projects`** — Always call this first to discover `projectSlug`. Never guess or invent a slug. If the user has no project yet, call **`aiseo_create_project`** with their website URL.
2. **`aiseo_get_scan`** — Check whether a project scan is `running` or `done`. `done` means no in-flight jobs, not necessarily 100% prompt coverage (compare `scannedPromptCount` to `promptCount`). Do not invent a subscribe tool or poll `aiseo_list_prompts` to infer scan state.
3. **Read visibility data** as needed:
   - `aiseo_list_prompts` — Tracked prompts with status (`winning`, `opportunity`, `missing`) and per-model ranks.
   - `aiseo_list_sources` — Domains and URLs cited by AI answers.
   - `aiseo_list_missing_sources` — Third-party pages (listicles, comparisons, Reddit threads, directories) cited for the project's prompts where the project's own site is not cited.
   - `aiseo_get_source_history` — Citation history for one URL across AI engines and when it dropped.
   - `aiseo_list_snippets` — Exact AI answer snippets describing the brand or a competitor.
   - `aiseo_list_competitors` — Competitor brands tracked for the project.
4. **`aiseo_add_prompts`** — Only call for queries the user explicitly asked to track (up to 20 per call).
5. **`aiseo_activate_plan`** — When scans are not active or the user wants to unlock the $49 audit, Basic ($79/mo), or Pro ($199/mo). Call without `confirmPaymentSpend` first, show the price preview, and get user approval before returning the Polar checkout link. The user pays in the browser.

## Step 3. Answer from live tool JSON only

- Copy every rank, URL, competitor name, snippet, and dashboard `href` directly from tool JSON.
- Handle `dataStatus: "no_data"` by explaining that the project needs a scan or is waiting for scan results—never treat `no_data` as an OAuth 401 failure and never invent ranks.
- For the same structured brief as the in-app Report page, fetch `https://aiseotracker.com/ai-seo-report/SKILL.md`.
- For a missing-prompt content campaign, fetch `https://aiseotracker.com/content-coverage/SKILL.md` and stop for approval before writing files.

## Links

- Documentation: https://aiseotracker.com/docs/mcp
- Tools reference: https://aiseotracker.com/docs/tools
- Help & support: https://aiseotracker.com/help
