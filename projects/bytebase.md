# Bytebase

## Summary
A database schema-change and governance tool: SQL review, migration workflows with
approval gates, drift detection, and access control across Postgres, MySQL, MongoDB, and
most major databases. Recently repositioned to also expose an MCP-style interface so AI
agents can propose and run reviewed schema changes rather than only humans.

## Why I Should Care
Most migration tools (Flyway, Liquibase, Prisma Migrate, Mongoose migrations) apply schema
changes; Bytebase adds the governance layer on top — review, approval, drift detection,
audit trail — across multiple database engines in one place, which is what's actually
missing once more than one person (or agent) touches a production schema.

## Problems It Can Remove
Applying migrations directly from CI with no review step; not having an audit trail of who
changed what schema and when; manually diffing a database against what migrations claim it
should look like.

## Practical Uses
- Gate schema migrations behind a review/approval step across a Postgres + MongoDB stack
  instead of applying them directly from CI
- Give an MSSP/PTaaS client-facing project an auditable record of schema changes
- Detect drift between what a migration tool declares and what's actually running

## Product Opportunities
Could be positioned to MSSP clients as part of a change-management/audit story on top of
their databases.

## Agent / Automation Opportunities
Positions itself as "built for humans and agents" with an MCP-style interface letting an
AI agent propose schema changes that still go through the same review/approval gate as a
human-authored one — worth verifying how mature that surface actually is versus the core
(long-proven) review workflow.

## Integration
Runs as its own service (Docker or self-hosted) with its own metadata store; connects out
to target databases. Medium effort — not a drop-in library, it's infrastructure you stand
up and point at existing databases.

## Architecture Notes
Cross-database SQL review and migration-approval engine with drift detection, built over
4+ years by a dedicated team — a genuinely hard engineering surface (parsing and
understanding schema semantics across several database dialects) rather than a thin CI
wrapper.

## Maturity
Mature. Created 2021-01-27, 14,499 stars, 983 forks, latest release 3.22.1 (2026-09-10),
large multi-contributor team with sustained daily activity.

## License
**FLAGGED — open-core split, not a single clean license.** Code outside `enterprise`-named
directories, and not controlling feature/plan/role enablement, is MIT — free to self-host,
modify, and embed commercially. Code under `LICENSE.enterprise` (advanced RBAC, SSO, and
other plan-gated features) may be modified and tested freely, but **production use
requires a paid Bytebase subscription** per its terms. Verify exactly which features are
needed before assuming the whole product is free to self-host in production.

## Alternatives
Liquibase/Flyway (migration-only, no review workflow or drift detection), Atlas
(schema-as-code, narrower scope), manual PR-based migration review.

## Risks / Limitations
- License requires care — the enterprise-gated feature set is not open source despite the
  overall project being described as open source
- Heavier to run than a single binary — its own service with its own metadata store
- The "built for agents" MCP-style positioning is new; the core review/governance workflow
  is the long-proven part

## Recommendation
PROTOTYPE — trial the MIT-licensed core (SQL review, basic migration workflow) against one
real Postgres or MongoDB database before evaluating whether any enterprise-gated feature is
actually needed.

## Change History
### 2026-09-21
Initial discovery and cataloguing. Slot 6 (infrastructure, observability, deployment) run.
