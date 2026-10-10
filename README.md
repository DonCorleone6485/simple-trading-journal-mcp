# Simple Trading Journal MCP server

Read your own trading journal from Claude, ChatGPT or Cursor. Ask "What cost me the most this month?" and get the answer from your real trades: win rate, P&L, R-multiples, mistake costs and discipline stats.

[Simple Trading Journal](https://www.simpletradejournal.io) is a free-to-start trading journal for forex and prop-firm traders. It syncs MetaTrader 4 and 5 trades automatically without your broker password, imports cTrader, TradeLocker, DXtrade and Match-Trader reports, and tracks performance, discipline and prop-firm limits in 9 languages.

This repository only documents the hosted (remote) MCP server. There is nothing to install or run.

- **Server URL:** `https://www.simpletradejournal.io/api/mcp` (Streamable HTTP)
- **Auth:** `Authorization: Bearer <key>`. Create a key in the journal under "AI connection" (Pro plan; 3-day trial without a card).
- **Docs:** https://www.simpletradejournal.io/ai-trading-journal

## Connect

**Claude Code**

```bash
claude mcp add --transport http simple-trading-journal https://www.simpletradejournal.io/api/mcp --header "Authorization: Bearer KEY"
```

**Claude Desktop / Cursor** (through `mcp-remote`)

```json
{
  "mcpServers": {
    "simple-trading-journal": {
      "command": "npx",
      "args": ["mcp-remote", "https://www.simpletradejournal.io/api/mcp", "--header", "Authorization: Bearer KEY"]
    }
  }
}
```

**ChatGPT:** create a custom GPT → Actions → Import from URL → `https://www.simpletradejournal.io/openapi.json`, Authentication: API Key, Bearer.

Custom connectors on the claude.ai website do not accept keys yet, so they are not supported.

## Tools

| Tool | What it returns |
|---|---|
| `get_stats` | Win rate, net P&L, profit factor, average R, max drawdown, results by symbol and setup |
| `list_trades` | Recent trades with symbol, direction, risk, P&L, R, costs and preset mistake tags (max 50 per call) |
| `get_mistake_costs` | Net P&L cost of each preset mistake tag (moved stop, revenge trade …) |
| `get_discipline` | Trades that broke habits (overtrading, revenge, size jumps) vs clean trades |
| `add_trade`, `add_note`, `tag_mistake` | Only when "Allow writing" is on for the key. Add-only: nothing is ever deleted or changed. |

All read tools accept optional `from` / `to` dates (YYYY-MM-DD).

## Privacy and limits

- A key sees only the one journal you chose.
- Sent: trade numbers (symbol, direction, risk, P&L, R, date) and preset mistake tags. Never sent: notes, day notes, photos, custom tags.
- 300 requests a day per key; up to 100 writes a day when writing is allowed. Keys stop working if you leave Pro.
- Not investment advice.

Contact: support@simpletradejournal.io
