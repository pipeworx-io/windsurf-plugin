# Pipeworx Windsurf Install Kit

Connect Windsurf (Cascade) to live data from **2,825 tools across 621 packs** — SEC filings, USPTO patents, FRED economic data, FDA drug data, Census, EPA, ATTOM real estate, weather, and 613+ more.

Backed by the [Pipeworx](https://pipeworx.io) MCP gateway at `gateway.pipeworx.io`.

## Install

Windsurf doesn't currently expose a public plugin submission format, so this is a copy-paste install kit.

**1. Add Pipeworx to your global Windsurf MCP config.**

Open (or create) `~/.codeium/windsurf/mcp_config.json` and merge in the `mcpServers` block from `mcp_config.json` in this repo:

```json
{
  "mcpServers": {
    "pipeworx": {
      "serverUrl": "https://gateway.pipeworx.io/pipeworx-catalog/mcp"
    }
  }
}
```

**2. Restart Cascade** (or hit Refresh in Windsurf's MCP settings).

You should see `pipeworx` connected with ~17 tools.

**3. (Recommended) Drop the routing rule into your project.**

Copy `.windsurf/rules/pipeworx.md` from this repo into your project's `.windsurf/rules/` directory. Or, for global use across all projects, paste its contents into `~/.codeium/windsurf/memories/global_rules.md`.

The rule teaches Cascade when to reach for `ask_pipeworx` / `discover_tools` instead of hand-writing facts.

## Verify after install

Try a real query in Cascade:

> What was the unemployment rate last month?

Cascade should call `ask_pipeworx` (which routes to `fred_get_series`) and return a real number.

## How it works

The install loads **17 meta-tools** from the Pipeworx gateway — not all 2,825 underlying tools. That's deliberate: dumping every tool definition into the context window burns tokens you'll never use.

The loaded meta-tools:

- **`ask_pipeworx`** — natural-language router. *"What's Apple's latest 10-K?"* hits SEC EDGAR. *"Side effects of Ozempic?"* hits FDA.
- **`discover_tools`** — top-20 most relevant tools for a task, with full schemas.
- **`entity_profile`**, **`recent_changes`**, **`compare_entities`**, **`resolve_entity`** — fan-out across multiple packs in one call.
- **`validate_claim`** — fact-check claims against SEC XBRL. Returns a verdict + citation.
- **`remember`** / **`recall`** / **`forget`** — persistent memory across sessions.
- **`list_packs`**, **`search_packs`**, **`get_pack_tools`**, **`get_connection_config`**, **`get_platform_status`**, **`search_mcp_directory`** — browse the catalog.

## Need direct pack access?

If you want a specific pack's tools loaded directly (e.g., to call `attom_property_search` without going through `ask_pipeworx`), add a scoped entry to your `mcp_config.json`:

```json
{
  "mcpServers": {
    "pipeworx-attom": {
      "serverUrl": "https://gateway.pipeworx.io/attom/mcp"
    }
  }
}
```

## Higher rate limits

The install runs on the anonymous tier (50 calls/day per IP). For higher limits (500/day BYO, 2,000/day OAuth, or unlimited paid), [sign up at pipeworx.io](https://pipeworx.io) and add an `X-API-Key` header in `mcp_config.json`. (Header support in Windsurf's remote-MCP config is rolling; if your version doesn't honor `headers`, use the BYO query-param form: `?_apikey=YOUR_KEY` appended to `serverUrl`.)

## Submitting to Windsurf's Plugin Store

Windsurf curates the in-app MCP marketplace; there is no public submission spec yet. When Codeium opens it up, we'll publish here and link from https://pipeworx.io.

## Links

- Gateway: https://gateway.pipeworx.io
- Stack guide: https://pipeworx.io/stack
- Source: https://github.com/pipeworx-io/pipeworx

## License

MIT
