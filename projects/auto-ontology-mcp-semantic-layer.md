# Auto Ontology MCP

## Summary
An MCP server that brings NVIDIA's published Auto Ontology semantic-layer research to any MCP client (Claude, ChatGPT, Codex, Cursor, OpenClaw, Hermes Agent, etc.) for two jobs: safer text-to-SQL against a connected database, and a personal knowledge base built from documents. For the database job, it builds reviewed business Terms and join paths, then validates generated SQL against the ontology and real data before running it — catching wrong joins, case-mismatched filter values, and fan-out double-counting that a plain "send the schema in the prompt" approach misses silently. For the knowledge-base job, it ingests receipts, statements, papers and notes into typed fact records and linked concept pages with full-text search, and can export the whole thing as an Obsidian vault. Unlike the original NVIDIA Auto Ontology (which runs its own NIM-hosted LLMs behind a Next.js/FastAPI/Postgres stack), this project pushes every language-reasoning step to the model already running in the MCP client, so there is no separate model endpoint to host and no API key to configure — only deterministic work (schema extraction, validation, SQLite storage) runs locally.

## Why I Should Care
Text-to-SQL's actual failure mode is rarely a crash — it's a plausible-looking wrong number. This project's validate-before-run step against a reviewed ontology directly targets that specific risk, with a worked table of concrete failure cases in its README (wrong join inferred from matching integer IDs, a case-mismatched filter silently returning zero rows, a one-to-many join inflating a sum). That is directly applicable to any reporting or admin-panel feature built on the existing Postgres/MongoDB stack where a user or agent is allowed to ask questions in natural language.

## Problems It Can Remove
- Silent wrong-answer text-to-SQL (bad joins, case-mismatched filters, fan-out double-counting) in any natural-language reporting feature.
- The "model dumps a markdown wiki page" pattern (Karpathy's LLM Wiki) for building a personal knowledge base, replaced with a queryable, typed ontology supporting SQL-style aggregation instead of prose search.
- Hosting and paying for a separate model endpoint just to run a semantic-layer/ontology tool — this one runs entirely through the MCP client's own model.

## Practical Uses
- Add a validated text-to-SQL layer in front of a reporting database so generated queries are checked against reviewed join paths and real column values before execution.
- Build a personal knowledge base from receipts, statements, papers and screenshots that can later be queried with exact numbers and quotes, with sources preserved.
- Export the accumulated ontology as an Obsidian vault with `[[links]]` and a graph view for browsing outside the MCP session.

## Product Opportunities
The validated-SQL-over-a-reviewed-ontology pattern (join-path checking, fan-out warnings, value-mismatch detection before execution) is worth studying as a reusable safety layer for any internal admin-panel or client-facing reporting feature that lets an agent or end user query a database in natural language.

## Agent / Automation Opportunities
Runs with no separate model endpoint or API key — every extraction, reasoning and SQL-generation step happens through the MCP client's own configured model, so cost and capability simply track whatever agent is already in use. The `export_wiki` tool makes the agent's accumulated knowledge inspectable as a standard Obsidian vault outside the MCP session itself.

## Integration
Local MCP server (stdio or streamable HTTP); deterministic work (schema extraction, profiling, key inference, validation, read-only query execution) runs in local SQLite files, with no external service beyond the already-configured MCP client's own model calls. The `auto-ontology-mcp demo` command sets up a synthetic retail database and sample documents to try before pointing it at real data. Integration effort: **Low** — no model endpoint to stand up, no API key to configure.

## Architecture Notes
The ontology layer tracks reviewed business Terms, default join-path roles (with alternatives named explicitly when ambiguous, e.g. billing vs. delivery city), and real stored values — so `validate_sql` can reject a join the ontology disagrees with, or flag that a filter's casing doesn't match any real row, before a query ever runs. A `check_answerable` tool checks whether a question can even be answered against the covered schema before attempting SQL, surfacing gaps (e.g. "nothing covers ad spend") instead of guessing.

## Maturity
Experimental. Created 2026-11-06 (19 days old at review), no tagged GitHub release yet — pre-1.0, evaluate expectations accordingly despite the polish of the documentation and demo.

## License
Apache-2.0, confirmed via the repo's license badge. No commercial-use restrictions.

## Alternatives
WrenAI and Chat2DB-style text-to-SQL tools (already evaluated in prior runs) generate SQL but without this project's explicit validate-before-run ontology check. Hand-rolled "send the schema in the prompt" text-to-SQL is the default failure mode this is built to catch. claude-self-reflect and ClickHouse/loom (already catalogued agent-memory MCPs) overlap only on the personal-knowledge-base half of this project's two jobs, not the database-validation half.

## Risks / Limitations
- No tagged release yet and only 19 days old — pre-1.0 stability risk.
- Entirely dependent on the quality of whatever model is running in the connected MCP client for every reasoning step, since the project does no model hosting of its own.
- Star:subscriber ratio (~136:1) is moderate; worth re-checking traction given the project's youth.
- Built atop NVIDIA/auto-ontology (NVIDIA's own published research) but is an independent third-party implementation by a single maintainer ("gogoymh"), not an official NVIDIA release — the README's framing could be misread otherwise.

## Recommendation
PROTOTYPE — worth trying the bundled demo against a real reporting use case before depending on it, given its very early stage.

## Change History
### 2026-11-25
First seen and reviewed. Discovered via GitHub search for agent-memory/personal-knowledge-base keywords (slot 1: AI agents, MCP, coding productivity). Verified license, stars/forks/subscribers via the GitHub API (no tagged release found); README read directly to confirm the ontology-validation architecture and its relationship to NVIDIA's original Auto Ontology research.
