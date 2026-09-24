# ktx

## Summary
A local-first, self-improving "context layer for data agents." ktx ingests a data
warehouse (Postgres, MongoDB, MySQL, Snowflake, BigQuery, ClickHouse, SQL Server, SQLite,
DuckDB, Athena), existing semantic layers (dbt, MetricFlow, LookML, Looker, Metabase,
Sigma), and company wiki/Notion knowledge, then serves it to coding/data agents through a
CLI and MCP server as approved, reusable metric definitions and a searchable wiki —
read-only by design.

## Why I Should Care
General-purpose coding agents re-explore a database schema on every question and invent
their own metric logic, producing numbers that don't match approved definitions. ktx
fixes this specifically for agents already in daily use (Claude Code, Codex, Cursor,
OpenCode) against databases already in use (Postgres/MongoDB), without standing up a
separate BI platform.

## Problems It Can Remove
- Rebuilding a text-to-SQL or "ask your data" pipeline from scratch for an internal
  dashboard or client reporting feature.
- Agents silently inventing metric logic that doesn't match the canonical definition
  used elsewhere (dbt/Looker/wiki).
- Manually reconciling contradictory business definitions scattered across a wiki, a dbt
  project, and a BI tool.

## Practical Uses
- Point ktx at a production Postgres/Mongo database and let an agent answer "what was
  revenue by region last quarter" using approved metric definitions instead of ad-hoc SQL.
- Ingest an existing dbt or Metabase semantic layer plus the team wiki into one searchable
  MCP surface.
- Run `ktx sl "revenue"` or `ktx wiki "refund policy"` directly from a terminal for a fast
  answer without opening an agent session.

## Product Opportunities
A plausible foundation for an "ask your data" feature on an internal admin dashboard or a
client-facing reporting product, without building a bespoke text-to-SQL layer.

## Agent / Automation Opportunities
Ships a CLI and an on-demand local MCP daemon (`ktx mcp start`) built specifically for
Claude Code, Codex, Cursor, and OpenCode integration; supports Anthropic API, Google
Vertex AI, AI Gateway, and local Claude Code/Codex session auth as LLM backends.

## Integration
`npm install -g @kaelio/ktx && ktx setup` — low effort. Runs entirely locally; the only
data that leaves the machine is what's sent to the configured LLM provider. Connections
to the warehouse are read-only.

## Architecture Notes
Combines three sources into one join graph: raw-table introspection (detects joinable
columns, resolves chasm/fan traps automatically), ingested semantic-layer definitions,
and wiki/knowledge content — then flags contradictions across sources for human review
instead of silently picking one. Project state lives in a plain directory
(`ktx.yaml`, `semantic-layer/`, `wiki/`) meant to be committed to git, with `.ktx/` kept
local for secrets/state.

## Maturity
Emerging. Created May 2026, ~1,600 stars in about 4.5 months, Y Combinator-backed (P25).
No tagged GitHub/npm release since July 2026 (`v0.16.0`) despite continued commits
through September — track `ktx status` directly rather than assuming release-tag parity.

## License
Apache-2.0. No restrictions on commercial use, redistribution, or embedding.

## Alternatives
- **Cube.js** semantic layer — established, heavier infra footprint.
- **dbt Semantic Layer / MetricFlow** — ktx explicitly ingests these rather than
  replacing them.
- **WrenAI** (GenBI text-to-SQL) — different architecture, no wiki-ingestion layer.
- **getnao/nao** — same "analytics agent" niche, surfaced the same day; license unclear
  (GitHub reports NOASSERTION), not deep-reviewed.

## Risks / Limitations
- Collects opt-out usage telemetry (install/command reliability metrics) and separately
  routes crash reports through PostHog Error Tracking, which can include local file
  paths or the local username in stack traces — read the telemetry docs before pointing
  it at a sensitive project.
- Y Combinator-backed — watch whether the CLI stays fully open or a hosted tier appears.
- Pre-1.0 and moving fast; no release in 2+ months despite active commits is worth
  keeping an eye on.

## Recommendation
PROTOTYPE — worth a small trial against a non-critical Postgres or MongoDB database to
see how well the automatic join-graph and contradiction-detection features hold up before
depending on it for anything client-facing.

## Change History
### 2026-09-24
Discovered and catalogued. Genuinely differentiated from mainstream BI/semantic-layer
tools by ingesting existing semantic layers plus wiki content into one MCP-servable
surface for coding agents specifically, rather than being another standalone BI platform.
