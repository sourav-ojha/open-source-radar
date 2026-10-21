# Datus Agent

## Summary
CLI-first data-engineering agent with a semantic layer ("Dosi") that compiles one business-domain model into SQL for 13+ dialects across 21 database adapters. Exposes a REST/MCP API (`Datus-API`, `datus-mcp`) for other agents and applications, plus chat surfaces (web, Slack, Feishu, VS Code). The context engine combines a business-domain tree with vector search over embedded LanceDB/SQLite, or shared PostgreSQL storage for teams.

## Why I Should Care
Natural-language-to-SQL and "give an agent safe, schema-aware access to a data warehouse" are both things that otherwise mean hand-building a text-to-SQL pipeline and context layer. This does both, compiles to Postgres among its 13+ dialects (directly in-stack), and exposes the result over MCP so any MCP-capable agent can use it without custom glue.

## Problems It Can Remove
Removes the need to hand-write a text-to-SQL pipeline or a schema-context layer for an LLM querying a database directly.

## Practical Uses
- Natural-language-to-SQL against Postgres without building a custom pipeline.
- Give a coding agent an MCP-exposed semantic layer over a data warehouse instead of letting it write raw, schema-blind SQL.
- Share one business-domain semantic model across a CLI, a Slack bot, and application code via the REST API, rather than re-deriving query logic per surface.

## Product Opportunities
Could underpin an internal analytics/reporting feature for the MSSP admin-portal SaaS without building a custom text-to-SQL layer from scratch.

## Agent / Automation Opportunities
Native MCP server (`datus-mcp`) for Claude Desktop/Cursor-style clients. Subagents bundle context, tools, and rules per business domain; skills follow the `agentskills.io` convention; a plugin framework adds external integrations.

## Integration
`pip install` plus CLI setup; connecting a real data source requires configuring the relevant database adapter and building out the semantic model, which is more involved than a drop-in tool. Integration effort: **Medium**.

## Architecture Notes
Three entry points for three audiences — CLI for data engineers, Datus-Chat for analysts, Datus-API for applications — sharing one semantic-model compiler and context engine. The domain-tree-plus-vector-search context engine, and compiling a single model to many SQL dialects, are the ideas most worth studying even independent of adoption.

## Maturity
Emerging, pre-1.0. Created July 2025, real company (Datus-ai) behind it, 10 distinct contributors, active releases (v0.4.3 released a week before review). GitHub API verified: 1,860 stars, 275 forks, 23 subscribers, archived:false.

## License
**Apache-2.0** — the GitHub API's automated license field reports `NOASSERTION`, but the actual `LICENSE` file (confirmed by reading it directly via raw.githubusercontent.com) is genuine, complete Apache License 2.0 text. Treat as Apache-2.0, with the API-detection discrepancy noted for anyone relying on automated license scanning of this repo.

## Alternatives
- **Canner/WrenAI** (catalogued) — broader governed text-to-SQL/GenBI agent-skill engine.
- **OtterMind/Chat2DB** (evaluated) — narrower chat-first SQL client without the semantic-layer compilation step.

## Risks / Limitations
- Pre-1.0 (v0.4.3) — API and CLI surface likely still shifting; treat as a prototype-stage dependency, not a production data-pipeline component yet.
- License shows as NOASSERTION in automated GitHub metadata despite a genuine Apache-2.0 LICENSE file — re-verify before any commercial redistribution decision if relying on tooling other than a direct file read.
- Outside this run's rotation slot (data/RAG rather than AI-agent/MCP); catalogued anyway given the direct fit with the Postgres/data-engineering axis.

## Recommendation
**PROTOTYPE** — worth a hands-on trial against a Postgres dataset to evaluate the semantic-layer compiler and MCP integration before considering it for anything production-facing.

## Change History
### 2026-10-21
Initial discovery and catalog entry. Slot 1 (AI agents, MCP, coding productivity) run; found outside the slot's primary area but kept per §5.
