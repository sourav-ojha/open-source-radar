# proxy-monster

## Summary
Self-hosted database access-control proxy for MySQL and PostgreSQL. Clients connect over the native wire protocol unchanged; proxy-monster authenticates, authorizes each statement via Cedar policy, applies deterministic column-level masking that survives joins/subqueries/`SELECT *`, brokers to the target DB with a per-datasource service account, and writes every decision to a tamper-evident audit trail.

## Why I Should Care
Column-level masking that actually follows sensitive values through query transformations (rather than only masking top-level column references) is a harder and more complete version of data-access governance than most self-hosted options attempt — directly relevant to the MSSP admin-portal's data-governance story.

## Problems It Can Remove
- Standing broad, static DB grants instead of time-boxed, just-in-time elevation through an approval workflow.
- Building bespoke column-masking views or triggers that don't survive query rewrites.
- Producing an access audit trail for compliance without instrumenting the application layer.

## Practical Uses
- Prove column-level access controls and produce an audit trail for a client compliance requirement (SOC2/data-governance angle).
- Grant just-in-time, revocable elevated DB access instead of standing broad grants.
- Study the lineage-aware masking architecture even if not adopted directly.

## Product Opportunities
Directly applicable to the MSSP admin-portal's data-governance story if the architecture proves out in practice — not mature enough to commit to today.

## Agent / Automation Opportunities
None specific — this sits behind an application's DB access rather than acting as an agent tool itself.

## Integration
Split control plane (Kotlin/JVM, identity/OIDC/Cedar policy/admin API) and data plane (Go, wire-protocol proxy), talking over gRPC, plus a Go lineage analyzer reached via FFI. Requires a pinned toolchain (JDK 24, Gradle, Go, Node, pnpm via `mise`) and a PostgreSQL-only control-plane store. High integration effort relative to a typical self-hosted utility. Documented deploy targets: local Docker stack and AWS ECS.

## Architecture Notes
Two independent "engine" concepts worth separating when reading the docs: the target databases it protects (MySQL fully enforced, PostgreSQL experimental) versus the control-plane store it runs on (PostgreSQL only). The lineage probe (`sqlglot-go`) parses each statement and follows sensitive values through expressions, functions, subqueries, and joins; anything it can't prove safe is denied by default (fail-closed) as a Cedar policy decision rather than a hardcoded error path.

## Maturity
Experimental. Created 2026-07-29 (~1 month old), 14 stars, PostgreSQL target support explicitly marked experimental. 40 open issues against only 7 forks and 14 stars — worth checking whether these are mostly maintainer-filed before reading it as community engagement.

## License
Apache-2.0. NOTICE file carries attribution requirements for redistributors.

## Alternatives
Teleport (broader database-access scope, partly open-core), Hasura/PostgREST row-level security (different mechanism, no cross-DB wire proxy), manual read-replica + view-based masking (status quo).

## Risks / Limitations
- Pre-1.0 (`server-v0.1.23`) — treat all claims, including the audit-trail guarantees, as unproven until tested directly.
- Heavy toolchain (JDK 24 pinned, Kotlin+Go+gRPC) raises integration effort well above a typical self-hosted utility.
- PostgreSQL as a protected target is explicitly experimental; MySQL is the only fully-enforced target today.

## Recommendation
STUDY — the lineage-aware masking and Cedar-policy design are worth understanding regardless of adoption; too immature to deploy against anything real yet.

## Change History
### 2026-08-30
Discovered and reviewed. GitHub API verified: Apache-2.0, 14 stars, pushed_at 2026-08-30, created 2026-07-29.
