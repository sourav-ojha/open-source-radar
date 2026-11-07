# Rocky

## Summary
A SQL transformation engine, positioned as a dbt alternative, that type-checks an entire SQL
pipeline before anything runs: compile-time type/reference/contract errors, column-level lineage
diffs showing exactly what a change breaks downstream, named branches with replay, and a
plan/apply workflow (`rocky plan` saves a change, `rocky apply` runs it) so nothing executes
without an explicit review step. Ships as a single static Rust binary with a built-in browser UI,
a VS Code extension, a Python SDK, and a Dagster integration.

## Why I Should Care
dbt's SQL pipelines fail at runtime — a broken reference or type mismatch shows up when the job
runs against the warehouse, not before. Rocky's compile step catches this offline, and
`rocky lineage-diff` turns "what does this change break" into a PR-ready Markdown artifact
instead of a manual dependency-graph read. The explicit `[policy]` rule set — "an AI-written plan
needs an approval marker" — is a directly relevant pattern for letting a coding agent propose SQL
pipeline changes without letting it auto-apply them to a warehouse.

## Problems It Can Remove
Replaces the combination of dbt plus a separately-maintained review/approval process for pipeline
changes: compile-time contract checking, declarative data-quality checks/tests, and a plan/apply
split that's dbt's `run` command with a mandatory review gate built in rather than bolted on via
CI config.

## Practical Uses
- Replacing or running alongside an existing dbt project (`rocky import-dbt` brings a dbt project
  in; `rocky emit-sql` writes plain SQL back out — explicitly designed for no lock-in).
- A reviewable pipeline-change workflow where an AI coding agent proposes SQL model changes and a
  human approves via the plan/apply gate before they touch the warehouse.
- Column-level lineage diffs as a PR check for any warehouse-backed analytics pipeline.
- Local development entirely on DuckDB via the playground before connecting a real warehouse.

## Product Opportunities
Less a product-building block than an internal-tooling upgrade: worth adopting if a SQL
transformation pipeline (dbt or hand-rolled) already exists and is a source of silent breakage.

## Agent / Automation Opportunities
The `[policy]` system (allow / require review / deny, with an explicit approval marker required
for AI-written plans) is a reusable pattern for any workflow where an agent proposes a
database-affecting change. Dagster integration (`dagster-rocky`) extends this into existing
orchestration setups.

## Integration
Single static Rust binary (`just build` from source, or the install script); Docker Compose and
Helm chart provided for a self-hosted `rocky serve`. Postgres support is beta, not yet the "most
complete" adapter (that's Databricks). **Integration effort: Medium** — straightforward to run
the binary and playground, more effort to productionize against a non-Databricks warehouse given
the beta-tier support.

## Architecture Notes
"A clean compile finds the problems that your SQL, schemas and config make visible. It does not
prove that every query runs or returns correct values" — the README is explicit about what the
type-checker does and doesn't guarantee, a level of honesty about limitations worth noting given
how often tools in this space oversell correctness guarantees.

## Maturity
Emerging but unusually active — created 2026-04-13 (under 7 months old at review), already on
engine release v1.89.0 (89 releases), Apache-2.0, GitHub Sponsors funding, VS Code extension
published to the Marketplace. Databricks support is "generally available"; Postgres/Snowflake/
BigQuery/Redshift/ClickHouse/SQL Server/Spark are beta.

## License
Apache-2.0. No restrictions on commercial use or embedding.

## Alternatives
- dbt — the mainstream baseline this is positioned directly against; Rocky's explicit no-lock-in
  import/export path acknowledges this.
- The already-catalogued pgschema — declarative Postgres *schema* migration specifically, not a
  SQL *transformation pipeline* tool; different layer of the stack, not a direct competitor.
- Dagster/Airflow for orchestration — Rocky integrates with Dagster rather than replacing it.

## Risks / Limitations
- 0 subscribers at review despite 309 stars and real release cadence — watch for the subscriber
  count to catch up as a confirmation of organic adoption vs. a marketing push.
- Postgres and most non-Databricks warehouses are beta-tier; the "most complete" adapter is
  Databricks, a stack mismatch for a MERN/Postgres-first profile.
- Very young project (7 months) with an aggressive release cadence (89 releases) — stability and
  API surface are still likely to shift.

## Recommendation
PROTOTYPE — worth trying against the DuckDB playground first, then a non-critical Postgres
pipeline, specifically to evaluate the compile-time contract checking and the AI-agent approval
workflow; hold off on anything warehouse-critical given the beta-tier Postgres support.

## Change History
### 2026-11-07
Discovered via GitHub search (slot 4: data, search, documents, RAG). First catalog entry.
