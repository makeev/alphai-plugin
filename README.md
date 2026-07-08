# AlphaAI plugin

[AlphaAI](https://alphai.io) for your agent — AI-enriched financial news, SEC
Form 4 insider trades and market signals, packaged in the
[Open Plugins](https://open-plugins.com) format for Cursor, Claude Code and any
compatible host.

Every article in the AlphaAI feed is pre-analyzed at ingest: per-ticker impact,
a category, and a 1–10 relevance score. SEC Form 4 insider filings become
scored, structured events minutes after they hit EDGAR. This plugin wires that
feed into your agent with zero glue code — OAuth on connect, no API key
to paste.

## What's inside

| Component | What it does |
|---|---|
| **MCP server** (`.mcp.json`) | Hosted at `https://mcp.alphai.io/mcp` — news search, per-ticker feeds, trending, SEC Form 4 insider trades, two-ticker analysis, news alerts (OAuth 2.1) |
| **5 skills** (`skills/`) | `stock-brief` · `market-pulse` · `insider-radar` · `peer-readacross` · `manage-alerts` — finance workflows that teach the agent when and how to use the tools |
| **1 rule** (`rules/`) | Prefer `alphai_*` tools over generic web search for financial-news tasks; frame output as research, not advice |

Skills are mirrored from
[makeev/alphai-claude-skills](https://github.com/makeev/alphai-claude-skills)
(the canonical source — edits land there first).

## Install

### Cursor

One click from the [cursor.directory listing](https://cursor.directory/plugins/alphai)
("Add to Cursor"), or add manually to `~/.cursor/mcp.json` (global) or
`<project>/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "alphai": {
      "url": "https://mcp.alphai.io/mcp"
    }
  }
}
```

Cursor flags the server "Needs login" as soon as it's added — click it and
approve in the browser.

### Claude Code

```
/plugin marketplace add makeev/alphai-plugin
/plugin install alphai@alphai
```

Claude Code asks you to approve the MCP server on install and prompts for
OAuth on connect — your browser opens to authorize (re-authenticate any time
with `/mcp`). (MCP only, without the plugin:
`claude mcp add --transport http alphai https://mcp.alphai.io/mcp`.)

### Headless (n8n, bots, CI)

No browser on the box? The same `ak_live_` API keys that call the AlphaAI
REST API also authenticate the MCP server. Send one as a static Bearer
header and skip OAuth entirely:

```json
{
  "mcpServers": {
    "alphai": {
      "url": "https://mcp.alphai.io/mcp",
      "headers": { "Authorization": "Bearer ak_live_YOUR_KEY" }
    }
  }
}
```

Create a key at [alphai.io/account](https://alphai.io/account); ready-made
recipes for n8n, Claude Code, and the SDKs are on
[alphai.io/mcp](https://alphai.io/mcp).

### Any Open Plugins host

```
npx plugins add makeev/alphai-plugin
```

## Pricing

Free tier: **20 req/min · 100 req/day, no card** — enough to evaluate every
tool. Paid plans from $2.99/mo: [alphai.io/pricing](https://alphai.io/pricing).
Docs and live playground: [alphai.io/developers](https://alphai.io/developers).

## Disclaimer

AlphaAI output is AI-generated financial information for **research, not
investment advice** — see [alphai.io/terms](https://alphai.io/terms).

## License

[MIT](LICENSE)
