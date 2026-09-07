# Duckle

## Summary
Duckle is an open-source ETL/ELT platform for teams who want pipelines running on their own infrastructure. Pipelines are authored on a visual canvas or in Python/SQL, then shipped as a single git-tracked file that runs headless on a schedule via `duckle-runner serve` — in Docker or on a box you own. It compiles to SQL executed by DuckDB, so throughput scales with the box's core count rather than a metered warehouse.

## Why I Should Care
Most self-hosted ETL tools either need their own database or lock pipelines into a proprietary cloud UI. Duckle stores each pipeline as a plain file in git and executes it by compiling to SQL on DuckDB — no vendor cloud, no per-row billing, no lock-in. The claimed throughput (96 million rows out of Postgres to Parquet in 39.9s) is a strong signal for a project only four months old, consistent with DuckDB's known performance profile (independently unverified).

## Problems It Can Remove
- Replaces a hand-rolled Node/Python ETL script for moving data between Postgres/MongoDB and Parquet/warehouses.
- Removes the need for a paid managed ETL SaaS (Fivetran/Airbyte Cloud-style per-row billing) for routine data movement.
- Gives non-engineers a visual pipeline canvas for recurring exports/reports instead of a bespoke admin script per request.

## Practical Uses
- Scheduled data-sync jobs monitored via a web console with roles and an audit trail.
- Reverse-ETL from an app database to a CRM or analytics tool.
- Feeding cleaned, structured data into a RAG pipeline using its 190 built-in source/destination connectors.
- Data-quality checks and lineage on top of existing extraction scripts.

## Product Opportunities
Reusable building block for any product feature needing scheduled data movement (billing exports, reporting, reverse-ETL) without paying per-row SaaS pricing.

## Agent / Automation Opportunities
Ships an MCP server ("connect Claude, Cursor or any agent") letting a coding agent inspect or modify pipelines directly. Also includes a local sandboxed AI pipeline assistant ("Duckie") with no filesystem/network/tool access.

## Integration
Authoring happens via a Tauri 2 desktop app (React 19 + Vite frontend talking to a Rust core); execution happens via a separate headless runner binary/Docker image. This two-part shape (desktop authoring + headless runtime) is a heavier onboarding step than a pure CLI tool. Integration effort: **Medium**.

## Architecture Notes
`duckdb-engine` topologically sorts the pipeline graph, lowers each node into SQL, and executes by shelling out to a downloaded DuckDB CLI (no statically linked database, keeping the binary small). Non-sink nodes materialize as tables so downstream stages can reference them; sinks become `COPY ... TO` statements. Everything persists to a chosen workspace folder as plain JSON/Markdown files.

## Maturity
Emerging/beta. Created 2026-05, 1,271 stars, 10 contributors, active Discord. Explicitly labeled "beta" in its own README — expect breaking changes.

## License
Dual MIT OR Apache-2.0 (confirmed via LICENSE-MIT and LICENSE-APACHE files, Rust convention). No restriction on commercial or embedded use.

## Alternatives
Airbyte, dlt, Meltano, n8n (generic automation, not ETL-optimized), hand-rolled scripts.

## Risks / Limitations
- Beta status — breaking changes and rough edges likely.
- Desktop-app authoring step adds friction versus a pure config/CLI tool.
- Young project (4 months old); real traction (1,271 stars, Trendshift-featured) but long-term maintenance unproven.

## Recommendation
PROTOTYPE — worth testing on a real recurring data-export or reverse-ETL task before relying on it for anything production-critical.

## Change History
### 2026-09-07
Initial discovery and review. Slot 6 (infrastructure, observability, deployment) run.
