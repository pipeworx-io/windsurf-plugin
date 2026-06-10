# Pipeworx for Windsurf

Give Cascade one MCP that reaches **3,300+ live-data tools across 750+ sources** — SEC filings, USPTO patents, FRED, Census, FDA, EPA, USAspending, Polymarket, Zillow, weather, and 740+ more — without loading 3,300+ tool schemas into your context window.

## Install

Windsurf doesn't currently expose a public plugin submission format, so this is a copy-paste install kit.

**1. Add Pipeworx to your global Windsurf MCP config.** Open (or create) `~/.codeium/windsurf/mcp_config.json` and merge in:

```json
{
  "mcpServers": {
    "pipeworx": {
      "serverUrl": "https://gateway.pipeworx.io/pipeworx-catalog/mcp"
    }
  }
}
```

**2. Restart Cascade** (or hit Refresh in Windsurf's MCP settings). You should see `pipeworx` connected with ~26 tools.

**3. (Recommended) Drop the routing rule into your project.** Copy `.windsurf/rules/pipeworx.md` from this repo into your project's `.windsurf/rules/` directory. Or for global use, paste its contents into `~/.codeium/windsurf/memories/global_rules.md`. The rule teaches Cascade when to reach for `ask_pipeworx` / `discover_tools` instead of hand-writing facts.

## Try it

After install, ask Cascade things like:

| Ask | What it triggers |
|---|---|
| *"What just happened to Apple?"* | `sec_8k_recent` → SEC 8-K events classified by severity |
| *"Spread between Polymarket and Kalshi on the next Fed decision?"* | `polymarket_kalshi_spread` → live cross-venue mispricing |
| *"Overdue Phase 3 readouts at Moderna?"* | `pharma_pipeline_catalysts` → biotech catalyst calendar |
| *"DoD cybersecurity contracts this week?"* | `usa_award_search` → sub-second USAspending mirror |
| *"Median home value and renter share in Lubbock, TX?"* | `housing_market_snapshot` + `housing_metro_demand` |
| *"Unemployment rate last month?"* | `fred_get_series` → official FRED data |

Cascade picks the right tool via `ask_pipeworx` — no pack-name memorization required.

## How it loads light

The install exposes **~26 meta-tools**, not all 3,300+ — `ask_pipeworx({question})` and friends route at runtime so you get the full catalog without the context tax.

## Free tier + signup

50 calls/day anonymous, IP-bound. [Sign up free in 10s via GitHub](https://pipeworx.io/signup?via=windsurf_plugin) for 200/day + a stable account.

## Verify after install

Try a real query in Cascade:

> What was the unemployment rate last month?

## What's loaded

- **`ask_pipeworx`** — natural-language router across all 750+ sources.
- **`discover_tools`** — top-20 relevant tools for a task, with full schemas.
- **`entity_profile`** / **`compare_entities`** / **`recent_changes`** / **`resolve_entity`** — fan-out across multiple packs in one call.
- **`validate_claim`** — fact-check claims against SEC XBRL.
- **`remember`** / **`recall`** / **`forget`** — persistent memory across sessions.
- **`list_packs`** / **`search_packs`** / **`get_pack_tools`** / **`get_connection_config`** / **`get_platform_status`** / **`search_mcp_directory`** — browse the catalog.

## Direct pack access

For a specific pack's tools loaded directly (e.g., `attom_property_search`), add a scoped entry to `mcp_config.json`:

```json
{
  "mcpServers": {
    "pipeworx-attom": {
      "serverUrl": "https://gateway.pipeworx.io/attom/mcp"
    }
  }
}
```

## Bring your own key

For BYO-tier (200/day), add `?_apikey=YOUR_KEY` to `serverUrl` (Windsurf's remote-MCP config doesn't reliably honor headers yet across versions).

## Submitting to Windsurf's Plugin Store

Windsurf curates the in-app MCP marketplace; no public submission spec yet. When Codeium opens it up, we'll publish here and link from https://pipeworx.io.

## Links

- Gateway: https://gateway.pipeworx.io
- Status: https://pipeworx.io/status
- Source: https://github.com/pipeworx-io/pipeworx

## License

MIT

---

⭐ Star if you'd use this — helps other Windsurf users discover it.
