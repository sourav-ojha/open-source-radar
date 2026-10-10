# schemagate

## Summary
schemagate performs row-level-security-aware schema pruning for text-to-SQL agents. Before an agent's prompt is built, schemagate filters which tables and columns the caller is actually allowed to read, based on their grants/role, so a restricted table is absent from the prompt entirely rather than present but silently returning zero rows after the database's own RLS strips them. It supports LangChain, MCP, or any SQL agent, against Postgres, Oracle, MySQL, SQL Server, and SQLite.

## Why I Should Care
Row-level security alone produces a quiet, confusing failure: the model writes valid SQL against a table the caller cannot read, RLS strips every row, and the user sees "no records found" — indistinguishable from "this data does not exist." schemagate closes that gap at the schema-selection step, and does it across five database engines with a benchmarked, token-efficient implementation (its own numbers: ~383 vs ~2,036 prompt tokens on a bundled 42-object demo schema, 8 of 42 objects surfaced for the caller's actual grants).

## Problems It Can Remove
- Removes the risk of a text-to-SQL agent implicitly leaking a restricted table's existence through a "no rows" response.
- Removes hand-rolled, per-project role-based SELECT-list filtering in application code.
- Cuts prompt token spend on a multi-tenant SQL agent by scoping the schema to what the caller can actually see.

## Practical Uses
- Add an access-control layer in front of an internal text-to-SQL/BI chat agent.
- Add table/column-level authorization to an existing MCP SQL server without rewriting the agent.
- Audit which tables a given role/principal can reach via a natural-language query, as a security review step.

## Product Opportunities
Directly applicable to an MSSP/security-admin-portal context: a packaged, auditable access-control layer for any internal or customer-facing natural-language database assistant. Could be positioned as a compliance feature ("provably cannot leak a restricted table into the prompt") for a SaaS BI/reporting product.

## Agent / Automation Opportunities
Works as a LangChain component or directly in front of an MCP SQL server — the natural integration point is any agent that currently hands a full database schema to an LLM before writing SQL.

## Integration
`pip install schemagate`, then `schemagate demo "<question>"` to try it against a bundled demo schema with no database or API key, or `schemagate select ... --url <connection-string>` against a real database. CLI and Python library both ship from the same package. Integration effort: **Low** — it's a schema-filtering step inserted before an existing agent's prompt construction, not a new service to run.

## Architecture Notes
The core operation is grant-intersection: given a caller's principal/role and a live database schema, compute the subset of tables/columns that principal may read, then feed only that subset (as DDL or a table shortlist) into the prompt. It works identically across Postgres, Oracle, MySQL, SQL Server, and SQLite by normalizing each engine's grant model to a common representation.

## Maturity
Emerging — created September 2026, five releases (v0.1.58 through v1.3.0) in under three weeks, actively iterating. Solo-maintained: 248 of 252 commits from the author, remainder from dependabot and one Claude-assisted commit. CI, PyPI releases, an OpenSSF Best Practices badge, a public demo site, and an MCP Registry listing are all in place despite the low star count (8).

## License
Apache-2.0, confirmed via the repository's LICENSE file and PyPI metadata. No restrictions on commercial or embedded use.

## Alternatives
Vanna (text-to-SQL, no access-control layer — schemagate's own docs position directly against it as "coming from Vanna"), hand-rolled role-based SELECT-list filtering in application code, relying on database-native RLS alone (exactly the gap this closes, since RLS filters rows, not which tables enter the prompt).

## Risks / Limitations
- Very early traction (8 stars, single contributor) — verify continued maintenance before relying on it in production.
- No independent security audit mentioned — review the grant-intersection logic directly before trusting it as a sole access-control layer.
- Narrow scope by design: this controls what enters the prompt, not a replacement for database-level RLS/grants, which should stay in place as defense in depth.

## Recommendation
PROTOTYPE — a small, well-engineered utility solving a real and specific problem (silent RLS failures in text-to-SQL agents) that's directly relevant to a security/MSSP context. Worth testing against a real internal SQL agent despite the low star count, given the maturity signals (CI, PyPI, benchmarks, MCP registry listing) that outweigh the star count as evidence of quality.

## Change History
### 2026-10-10
Discovered via GitHub search (slot 4: data, search, documents, RAG). First catalog entry.
