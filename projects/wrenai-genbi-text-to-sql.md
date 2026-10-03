# WrenAI (Wren Engine)

## Summary
Open-source governed text-to-SQL / "GenBI" engine from Canner: a semantic layer (MDL — plain YAML/Markdown kept in a git repo you own) sits in front of 22+ data sources (Postgres, BigQuery, Snowflake, ClickHouse, Redshift, Databricks, and more) and is exposed to any AI coding agent (Claude Code, Cursor, Cline, Codex, and 50+ others) through an MCP server, CLI, and installable agent skill — so an agent answers natural-language questions with governed SQL, charts, and dashboards instead of writing raw SQL directly against production.

## Why I Should Care
Extends the "AI-agent leverage" axis into the BI/analytics surface: instead of handing an agent raw database credentials and hoping its SQL is safe and semantically correct, WrenAI gives it a reviewable semantic layer and governed query path.

## Problems It Can Remove
Removes the need to hand-write a custom NL-to-SQL translation layer, or to let an agent generate raw SQL against production with no guardrails or shared metric definitions.

## Practical Uses
- Give a coding agent a governed way to answer "what were last month's signups by plan" against a Postgres replica without letting it write raw SQL.
- Build an internal GenBI feature (ask-your-database-in-English) for an admin-portal SaaS product.
- Maintain one semantic layer of metric/dimension definitions shared across multiple agents and dashboards instead of redefining business logic per tool.
- Prototype a client-facing analytics assistant for the MSSP security admin-portal using the same engine.

## Product Opportunities
Could be packaged as a "talk to your data" add-on for a SaaS admin portal, or become the backend for a micro-SaaS offering non-technical teams natural-language reporting over an existing warehouse or Postgres database.

## Agent / Automation Opportunities
The primary distribution path is now an agent skill / MCP server — install directly into Claude Code and query configured data sources conversationally. The semantic layer plus governed SQL generation gives an agent trustworthy, reviewable queries instead of hallucinated raw SQL.

## Integration
Three-command quickstart installs the skill/MCP server into a supported agent; the engine itself (MDL semantic layer, CLI, 22+ connectors) is self-hosted and Apache-2.0. Integration effort: **Medium** — requires defining the semantic layer (MDL) for your data sources and an LLM API key or local model to drive SQL generation.

## Architecture Notes
Everything the engine writes (semantic model, metric definitions) is plain YAML/Markdown in a repo the user owns — "Git Sync" is what turns that same repo into a governed, team-wide deployment (self-hosted or Wren AI Cloud), without re-modeling anything. Notably, the project recently moved away from its original Docker-based GenBI web app toward this skill/MCP-first distribution model.

## Maturity
Mature by scale (17,799 stars, active since March 2024, real company backing), but the current skill/MCP-first architecture is recent — the README itself documents the pivot away from the Docker-based app. GitHub API verified: pushed 2026-10-02, latest release wren-v0.15.0 (2026-09-21).

## License
Multi-licensed: core engine, SDK, skills, and examples are Apache-2.0; docs are CC BY 4.0. Confirmed via the repo's own license-overview file. Free and self-hostable for the core engine. Wren AI Cloud and self-hosted "Enterprise Plus" are separate paid tiers layered on top (team-wide Git Sync deployment, extra governance), not required for individual or self-hosted use.

## Alternatives
- getnao/nao (evaluated, not catalogued) — smaller, similar NL-to-SQL analytics-agent pitch, less traction and fewer connectors.
- holoviz/lumen — NL-to-SQL/charts/dashboards, Python-native, smaller community.
- Direct LLM + raw DB credentials (no semantic layer) — faster to wire up but no governance, no reusable metric definitions, and no guardrail against an agent hitting production directly.

## Risks / Limitations
- Recently pivoted distribution model (Docker app → skill/MCP) — the newer model has less track record than the project's overall star history suggests.
- Needs an LLM API key (or local model) to generate SQL; query correctness still depends on that model and the quality of the semantic-layer definitions.
- The "22+ connectors" and governance-depth claims were verified against the README/docs site only, not independently re-tested against a live data source in this pass.

## Recommendation
**PROTOTYPE** (score 7.8/10) — promising enough to test in a real workflow (install the skill, point it at a staging Postgres instance), but the recent architecture pivot means it's worth validating before deeper reliance.

## Change History
### 2026-10-03
First discovered and reviewed. Verified via GitHub API: Apache-2.0 core (multi-licensed repo), 17,799 stars, current version wren-v0.15.0 (2026-09-21). Surfaced from a topic:text-to-sql search.
