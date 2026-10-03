# Data Contract CLI

## Summary
Open-source CLI and Python library (MIT, mature since 2023) for authoring, linting, and testing Data Contracts against the Open Data Contract Standard (ODCS) — connects to a real data source (Postgres, Snowflake, BigQuery, Databricks, and 15+ others), runs schema and quality checks against live data, diffs two contract versions for breaking changes, and exports to SQL DDL, HTML, JSON Schema, and other formats.

## Why I Should Care
Turns "does this data still match what we promised" into a one-command CI check instead of a manually-maintained schema doc that silently drifts from reality — directly relevant to client-facing integrations in an admin-portal SaaS context.

## Problems It Can Remove
Removes hand-written, ad hoc schema validation scattered across a codebase, and the manual drift between a documented API/data schema and what's actually being sent.

## Practical Uses
- Enforce a schema/quality contract on a client-facing API or webhook payload so breaking changes are caught in CI before shipping.
- Run `datacontract test` in CI against a staging Postgres database to catch a migration that silently breaks a documented schema.
- Generate SQL DDL or an HTML data dictionary straight from a YAML contract instead of hand-maintaining both.
- Use `datacontract breaking` as a CI gate before merging a schema change, independent of any specific ORM.

## Product Opportunities
Could formalize the schema that client systems must send/receive when integrating with a SaaS product, with automatic CI enforcement — a concrete, demonstrable governance feature when onboarding enterprise clients with their own data feeds.

## Agent / Automation Opportunities
An agent could be given `datacontract test` as a tool to self-verify that a schema migration it just wrote actually matches the documented contract before declaring the task done.

## Integration
`uv tool install 'datacontract-cli[all]'` or the provided Docker image; usable as a CLI, a CI/CD step, or a Python library. Integration effort: **Medium** — the ODCS YAML format has a learning curve, though the payoff compounds over time as contracts prevent drift.

## Architecture Notes
Everything is driven from one ODCS-compliant YAML file: the `servers` section holds connection details, `schema` holds structure/semantics, and `quality`/`service level` sections encode freshness and row-count expectations — one file doubles as documentation, a test spec, and an export source (SQL DDL, HTML, JSON Schema).

## Maturity
Mature. GitHub API verified: MIT, 1,077 stars, pushed 2026-10-02, created 2023-07-24, PyPI latest version 1.2.2.

## License
MIT, confirmed via GitHub metadata and PyPI. No restrictions.

## Alternatives
- sodadata/soda-core (2,433★) — overlapping data-quality-contract space, but uses Soda's own DSL rather than the open ODCS standard.
- Great Expectations / dbt contracts — heavier, warehouse/dbt-ecosystem-specific; this tool is standard-based and source-agnostic.
- Hand-written JSON Schema plus ad hoc validation — the status quo this replaces, with no changelog/breaking-diff tooling.

## Risks / Limitations
- The README's self-reported "1.4M/month" PyPI download badge is a static claim, not independently verified via pypistats in this pass — treat as marketing until confirmed.
- Authoring ODCS YAML contracts has a learning curve; the value shows up over months of drift prevention, not on day one.
- Documentation leans toward data-warehouse workflows (Snowflake/BigQuery/Databricks); Postgres/MongoDB support works but is less extensively documented than the warehouse path.

## Recommendation
**PROTOTYPE** (score 7.6/10) — worth trialing on one real client integration's schema before adopting broadly, given the authoring learning curve.

## Change History
### 2026-10-03
First discovered and reviewed. Verified via GitHub API and PyPI: MIT, 1,077 stars, PyPI version 1.2.2. Surfaced from a '"data contract" in:description' search.
