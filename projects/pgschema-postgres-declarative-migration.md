# pgschema

## Summary
A CLI from the pgplex suite (built by Bytebase, the open-source DB-governance company) that brings Terraform-style declarative schema migration to PostgreSQL: dump the live schema to SQL, edit the desired state, generate a reviewable migration plan as a diff, then apply it with concurrent-change detection and lock-timeout control.

## Why I Should Care
Direct match for the Postgres + Terraform parts of the stack. The `dump → edit → plan → apply` loop is exactly the Terraform `plan`/`apply` mental model applied to a Postgres schema, which removes the usual pain of hand-writing paired up/down migration files and hoping the DDL is correct.

## Problems It Can Remove
Replaces hand-written migration files (Flyway/golang-migrate-style) and the risk of a migration that doesn't match what was intended, by diffing a declared desired-state SQL file against the live database and generating the DDL automatically — with transaction-adaptive execution and lock-timeout control baked in.

## Practical Uses
- Run `pgschema plan` in CI to produce a reviewable DDL diff before a schema change merges.
- Replace manually written ALTER TABLE migrations with a single `schema.sql` source of truth.
- Manage RLS policies, triggers, materialized views, and custom aggregates declaratively.
- Let a coding agent propose schema changes safely, since the tool's output is a deterministic plan rather than freeform SQL.

## Product Opportunities
A `pgschema plan` CI gate turns Postgres schema review into an ordinary PR diff for any product running Postgres — useful across any of the current or future SaaS products.

## Agent / Automation Opportunities
Explicitly agent-friendly by design: the plan/apply output is structured and deterministic rather than freeform SQL, which is the property that makes it safe to let an AI agent drive a schema change instead of just reviewing one after the fact.

## Integration
Single CLI binary (Homebrew/mise/binary download/Docker/Nix). No server component. Low integration effort — drop into an existing CI pipeline as a `plan` step and a manual/gated `apply` step. Windows is explicitly unsupported (WSL/Linux only).

## Architecture Notes
The dump/plan/apply model is a direct port of Terraform's plan/apply workflow onto Postgres DDL. The README documents an unusually complete slice of Postgres DDL support: primary/foreign/unique/check constraints, all index methods including CONCURRENTLY, views and materialized views with dependency ordering, functions/procedures/aggregates with full parameter semantics, triggers (including constraint triggers), RLS policies, and column-level GRANT/REVOKE — this is a serious DDL-diffing engine, not a toy.

## Maturity
Emerging but not brand new: created 2025-06-08, 1056 stars, 64 forks, current release v1.13.1 (2026-09-20). Backed by Bytebase via the pgplex org, which lowers solo-maintainer/abandonment risk relative to most single-author CLI tools in this radar.

## License
Apache-2.0 — unrestricted commercial use, redistribution, and embedding.

## Alternatives
- **Atlas** (ariga/atlas) — more mature and multi-dialect, but heavier/more opinionated tooling.
- **golang-migrate / Flyway** — the imperative up/down-migration status quo this replaces.
- **reshape** (fabianlindfors) — zero-downtime Postgres migrations, but imperative rather than declarative.

## Risks / Limitations
- Windows unsupported.
- Postgres-only (no multi-dialect story).
- Commercial roadmap of the pgplex/Bytebase org worth checking before deep long-term dependency, even though the current license is permissive.

## Recommendation
PROTOTYPE — worth trying on a real Postgres-backed project's next schema change before committing to it as the default migration workflow.

## Change History
### 2026-10-09
First catalogued. Slot 3 (developer utilities, debugging, testing) discovery run. GitHub API verified: Apache-2.0, 1056 stars, pushed 2026-09-20, not archived.
