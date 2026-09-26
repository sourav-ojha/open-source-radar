# DatI

## Summary
Self-hosted semantic gateway that connects a relational or analytical database (MySQL, PostgreSQL, ClickHouse, Doris) to AI agents: enrich tables and columns with business-term metadata, then publish built-in or parameterized SQL tools as an MCP (Streamable HTTP) service with per-user credentials, permissions, and data-scope isolation.

## Why I Should Care
This is a narrow, concrete implementation of exactly the capability the operator profile names explicitly — "expose it through an API, CLI, MCP server" — applied to the single most common product-building block: a relational database. The permission and data-scope layer, not just NL2SQL translation, is what separates it from a toy demo.

## Problems It Can Remove
Removes the need to hand-build a schema-aware SQL-generation layer with credential isolation and per-user data scoping every time a database needs to be made safely queryable by an agent.

## Practical Uses
- Turn an existing MySQL/Postgres product database into a governed, natural-language-queryable MCP tool for an internal agent.
- Give a coding agent safe, scoped read access to a staging database via per-user credentials rather than a shared superuser connection.
- Business-term/column-alias enrichment so an LLM understands internal jargon (e.g. "MRR", "churned_at") instead of guessing from raw column names.
- NL2SQL analysis workflows over an analytics warehouse (ClickHouse/Doris) for ad hoc reporting through a chat agent.

## Product Opportunities
A reusable pattern for giving any product's database safe agent access as a feature, instead of building bespoke NL2SQL guardrails per project.

## Agent / Automation Opportunities
The entire product is the agent integration: publishes MCP (Streamable HTTP) tools directly consumable by any MCP-compatible host (Claude Code, Cursor, Antigravity, etc.), with schema inspection and SQL execution as built-in tools plus support for custom parameterized SQL tools.

## Integration
`docker compose up` runs the Spring Boot backend, Vue frontend, and an Elasticsearch instance for semantic retrieval. Self-hosted only — a public demo exists but there is no managed hosting offering. Effort: Medium — an extra service stack (including Elasticsearch) to operate, and a Java/Spring Boot codebase rather than Node if customization is ever needed.

## Architecture Notes
Three internal layers sit between MCP clients and the underlying databases: a semantic layer (business terms, column aliases, enum-dictionary extraction plus semantic search), a security layer (credentials, per-user permissions, data-scope isolation), and a tools layer (built-in schema/SQL tools plus user-defined parameterized SQL tools) — published together as one MCP service per configured "business subject."

## Maturity
Experimental. Created August 2026 (~1 month old at review), 54 stars, single developer, no independent validation yet. Real end-to-end test suite and example projects (AdventureWorks BI, a family-finance assistant) ship in the repo.

## License
Apache-2.0. No restrictions on commercial or embedded use.

## Alternatives
- AgamiAI/agami-core (evaluated the same run, not catalogued) — similar schema-to-semantic-model pitch, less concrete on the MCP-publishing workflow.
- Hand-rolled NL2SQL agent with an LLM writing raw SQL directly — what this replaces, minus the credential/permission/scoping layer.
- No other catalogued entry currently covers this specific "governed DB-to-MCP gateway" niche.

## Risks / Limitations
- Single developer, ~54 stars, ~1 month old — no independent validation.
- Java/Spring Boot + Vue stack rather than Node — runs as a separate self-hosted service, not an embeddable library in a Node app.
- Requires Elasticsearch for the semantic-retrieval feature, adding an operational dependency for a small deployment.
- Community origin (LINUX DO, a Chinese developer forum) with bilingual but not yet fully English-first documentation polish.

## Recommendation
PROTOTYPE — worth a hands-on trial against a non-critical database to evaluate the permission/scoping model before considering it for anything touching production data.

## Change History
### 2026-09-26
Initial discovery and review.
