# WhoDB

## Summary
Self-hosted, lightweight database workspace (Apache-2.0, Go backend + React/TS frontend) covering Postgres, MySQL/MariaDB/TiDB, SQLite/DuckDB, MongoDB/FerretDB, Redis/Valkey/Dragonfly, Elasticsearch/OpenSearch, ClickHouse, CockroachDB, YugabyteDB, QuestDB, and Memcached from one UI, with an interactive schema graph, query scratchpad, import/export/mock-data tooling, optional bring-your-own-key AI chat, and a CLI plus an official MCP server.

## Why I Should Care
It's one tool that actually matches the heterogeneous stack a MERN+AWS developer touches day to day — Postgres and MongoDB in particular — instead of running a different GUI client per engine, and the MCP server turns ad hoc DB exploration into something a coding agent can call directly.

## Problems It Can Remove
Replaces running TablePlus/Studio3T/Compass/a separate Redis GUI side by side. Removes the need to hand-build an MCP wrapper around any single database if agent DB access is the goal — `whodb mcp serve` already exists.

## Practical Uses
- Daily inspection/editing of Postgres and MongoDB data during development, in one browser tab.
- Give a coding agent read/write DB access through the built-in MCP server instead of writing a custom tool.
- Onboard a teammate onto an unfamiliar schema via the interactive relationship graph.
- Ask plain-English questions against a client database during an MSSP engagement, pointed at a local Ollama model so no data leaves the machine.
- Generate mock data and run quick imports/exports while prototyping a feature.

## Product Opportunities
Could sit inside an internal admin tool as the "raw data" escape hatch next to a purpose-built admin panel. The MCP server makes it a reusable building block for any internal agent needing ad hoc DB access across a mixed stack.

## Agent / Automation Opportunities
Ships `whodb mcp serve` out of the box — no custom MCP wrapper needed for read/write DB access. The CLI terminal UI is usable directly inside a coding-agent tool loop for quick schema checks.

## Integration
`docker run --rm -it -p 8080:8080 clidey/whodb`, or install the desktop app (macOS/Windows/Snap) or CLI (`npm install -g @clidey/whodb`). Integration effort: **Low**. Encrypted session persistence needs a generated key mounted at `/data`; set `WHODB_SECURE=true` behind HTTPS.

## Architecture Notes
Go backend talks to each database through its native driver/protocol rather than a lowest-common-denominator abstraction, which is why support depth varies by engine (the connection screen reflects what each source actually supports). AI chat is fully optional and provider-agnostic (OpenAI/Anthropic/Gemini/Ollama/LM Studio), configured per-session or via backend env vars for a shared deployment default.

## Maturity
Mature for a workspace tool of this kind. GitHub API verified: Apache-2.0, 5,031 stars, pushed 2026-10-03, created 2024-06-11, not archived, latest release 0.134.0 (2026-09-30).

## License
Apache-2.0 for WhoDB Community (this repo), verified via LICENSE file. Free to self-host and embed without restriction. A separate commercial "WhoDB Platform" tier (SSO, shared projects, audit logs, pipelines, reporting) exists for organizations that need it, but is not required for the open-source feature set above.

## Alternatives
- DBeaver / TablePlus / Studio3T — mainstream, feature-rich, but none ship an MCP server or a single self-hosted web deployment covering this database breadth.
- Chat2DB (OtterMind, 28,299★) — similar text-to-SQL database client with more AI polish, but Java-based with a heavier footprint and no documented MCP server.
- Adminer — much lighter, but SQL-only (no MongoDB/Redis/ClickHouse/Elasticsearch), no AI chat, no MCP.

## Risks / Limitations
- AI chat / text-to-SQL quality depends entirely on the chosen provider; there's no built-in evaluation of a generated query's correctness before execution.
- Open-core model: SSO, audit logs, and pipelines are reserved for the paid Platform tier, relevant if this ever needs to support multiple users in production.
- Relatively young (June 2024) despite the star count — verify the encryption-key/session handling (documented, opt-in) before exposing beyond localhost.

## Recommendation
**USE NOW** (score 8.6/10) — immediately useful for daily multi-database work, low adoption risk, Apache-2.0, and the MCP server is a genuine agent-leverage bonus with zero extra setup.

## Change History
### 2026-10-03
First discovered and reviewed. Verified via GitHub API: Apache-2.0, 5,031 stars, current version 0.134.0 (2026-09-30). Surfaced independently across topic:data-catalog, topic:data-lineage, and topic:text-to-sql searches.
