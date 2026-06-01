# Pipeworx

You have access to the Pipeworx MCP gateway: a live-data routing layer over 2,956 tools across 654 packs (SEC EDGAR, FDA, FRED, Census, USPTO patents, ATTOM real estate, EPA, weather, ClinicalTrials.gov, and more).

When the user asks for real, current, verifiable data — prefer calling Pipeworx tools over reciting from training.

## Routing rules

- Unsure which tool? → `ask_pipeworx({ question })`.
- Want options first? → `discover_tools({ task })`.
- Need everything-about-an-entity? → `entity_profile({ name })`.
- Fact-checking? → `validate_claim({ claim })`.

## Persistent memory

Cross-session memory via `remember`, `recall`, `forget`. Stable preferences, project facts, and recurring entities go here.

## Auth tiers

- Anonymous (no key) — 50 calls/day per IP
- BYO (`X-API-Key`) — 500/day
- OAuth (GitHub signup) — 2,000/day
- Paid — unlimited

For higher limits, the user can sign up at https://pipeworx.io.
