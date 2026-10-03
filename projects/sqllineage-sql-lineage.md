# SQLLineage

## Summary
Python library and CLI (MIT, maintained since 2019) that statically parses SQL — no live warehouse connection needed — to build table- and column-level lineage graphs, with a built-in local web UI for interactive visualization.

## Why I Should Care
Answers "what depends on this column" from plain SQL text in seconds, instead of grepping across a repo or tracing it by hand before a risky schema change.

## Problems It Can Remove
Removes manual tracing of "what breaks if I change this column" across a codebase of SQL files, migrations, or reporting views.

## Practical Uses
- Before changing a column in a shared reporting view or migration, run sqllineage against the repo's SQL to see every downstream consumer.
- Generate a visual lineage graph for onboarding documentation of a legacy reporting pipeline with no other docs.
- Embed in CI to flag a PR that touches a column with many downstream dependents for extra review.
- Audit an admin portal's reporting SQL to find dead or duplicate queries referencing the same source table.

## Product Opportunities
Could back a "show me what this field feeds" feature inside an internal admin tool for auditing data flows.

## Agent / Automation Opportunities
An agent making a schema change could call sqllineage first to self-check the blast radius before writing a migration.

## Integration
`pip install sqllineage`, then use as a CLI or Python library. Integration effort: **Low** — no database connection or extra infrastructure required; it works purely from SQL text.

## Architecture Notes
Pure static analysis: parses SQL dialects into an abstract syntax tree and resolves table/column references into a directed lineage graph, with no runtime execution or live connection to the data source.

## Maturity
Mature. GitHub API verified: MIT, 1,677 stars, pushed 2026-10-02, created 2019-05-21, latest release v1.5.9 (2026-09-05).

## License
MIT, confirmed via GitHub metadata. No restrictions.

## Alternatives
- Rocky's `lineage-diff` (today's Worth Watching pick) — richer (PR-ready diff of what changed) but tied to a full SQL-compiler project, currently Databricks/Snowflake/BigQuery/DuckDB-only.
- dbt's built-in lineage — requires buying into the full dbt project structure; sqllineage works on raw SQL files with no framework.

## Risks / Limitations
- Static analysis only — dynamically constructed SQL (built via string concatenation at runtime) won't lineage correctly.
- Best support is on common dialects; verify less-common syntax parses correctly before relying on it for full coverage.

## Recommendation
**USE NOW** (score 7.4/10) — mature, zero-infrastructure, low-risk utility worth dropping into any repo with non-trivial SQL.

## Change History
### 2026-10-03
First discovered and reviewed as today's Small but High-Leverage Utility pick. Verified via GitHub API: MIT, 1,677 stars, current version v1.5.9 (2026-09-05). Surfaced from a topic:data-lineage search.
