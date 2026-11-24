# pgbot

## Summary
A single static Go binary that connects to PostgreSQL read-only, reads Postgres's own statistics views, and prints a findings-first health report — cache hit rate, lock waits with the culprit query, rollback rate, unused-index bytes, transaction-ID wraparound risk, vacuum/checkpoint/replication status — plus what changed since the last run. No agent to install, no external service, and the connecting role never needs write privilege anywhere in the path.

## Why I Should Care
PostgreSQL is in the core stack (alongside MongoDB). This replaces the usual ad hoc routine of hand-querying `pg_stat_statements`, `pg_stat_user_tables` and `pg_stat_activity` whenever something feels slow, or standing up a hosted service (pganalyze, pgHero needs a web app) just to get a vitals check. The read-only-by-construction design means it is safe to point at a production replica without a second thought.

## Problems It Can Remove
- The manual "let me go check a dozen system views" ritual during an incident.
- Justifying a hosted Postgres-observability SaaS for a single-database side project or small team.
- Writing a bespoke cron script that diffs Postgres stats over time to catch slow drift (transaction-ID age approaching wraparound, growing unused indexes) before it becomes an incident.

## Practical Uses
- Point it at a production read replica during an on-call rotation for an instant triage report.
- Run `pgbot inspect --json` in CI after a migration to catch regressions (new unused indexes, slower query plans) before merge.
- Wire the MCP mode into a coding agent so it can self-check a dev database's health as part of a debugging session.
- `pgbot ask "why is it slow?"` for a plain-language read when short on time, backed by the same deterministic findings (works with an Anthropic key, among others).
- Track wraparound risk and connection-usage trends over time via the "what changed since last run" diffing.

## Product Opportunities
The "findings-first, graded report with a health score" presentation pattern (vital-sign gauges, a `checked` line naming what came back clean, CRITICAL/WARNING/NOTE buckets) is a reusable UX idea for any internal ops dashboard — it reads like a competent colleague's triage note rather than a wall of raw metrics.

## Agent / Automation Opportunities
Ships an MCP server mode alongside the CLI, so a coding agent can run `pgbot inspect`/`queries`/`indexes` itself during debugging without shell access to the database. The deterministic `--json` output (no AI key required) is a stable contract for scripting; the optional `ask`/`explain` layer adds a plain-language gloss on top using any OpenAI-compatible provider, including Anthropic, with no external service required beyond the model call itself.

## Integration
```sh
curl -fsSL https://pgbot.dev/install | sh
pgbot inspect "postgres://pgbot_ro@host:5432/db"
```
Single static binary (Go), also available via `go install`. Requires a read-only Postgres role (`pg_monitor` is sufficient). Works against Postgres 14–18. Integration effort: **Low** — no server, no write access, no dependency beyond the connection string.

## Architecture Notes
Deliberately narrow and safe-by-construction: read-only connection, no write privilege requested anywhere, no external service in the data path (the optional AI layer only sends the already-local findings text to a model, not raw data access). The CI badge, Go Report Card badge, and versioned JSON contract suggest real engineering discipline rather than a weekend script.

## Maturity
Emerging. Created 2026-08-11, already at v0.11.1 with frequent releases (same-day push at review time) — an active, fast-moving solo/small-team project rather than a one-off. 1,518 stars / 74 forks / 6 subscribers / 6 open issues. The stars:subscribers ratio (~253:1) is on the high side for this catalog's usual pattern, but paired with real fork activity (74) and a working CI pipeline, consistent with organic rather than inflated engagement.

## License
Apache-2.0, confirmed by reading `LICENSE` directly — the GitHub API reported `NOASSERTION` for this repo, a known misreport pattern seen elsewhere in this catalog. No commercial-use restrictions.

## Alternatives
pganalyze and pgHero (both require a hosted web app or long-running agent); raw `pg_stat_statements` dashboards built by hand; Datadog/New Relic Postgres integrations (SaaS, requires an agent with broader access). pgbot's distinguishing feature is the explicit no-write, no-agent, single-binary design.

## Risks / Limitations
- Young project (3.5 months old at review) — no long production track record yet.
- Small subscriber count (6) relative to stars; worth re-checking in a few months for continued maintenance momentum.
- The optional AI `ask`/`explain` layer sends findings text to a third-party model provider if enabled — fine for most cases, but worth knowing if the database contains sensitive query text.

## Recommendation
USE NOW — near-zero-risk given the read-only design; worth running against one real Postgres database (dev or a replica) immediately.

## Change History
### 2026-11-24
First seen and reviewed. Discovered via GitHub search for schema-diff/database-tooling keywords. Verified license directly from `LICENSE` (API misreported NOASSERTION) and confirmed version/release cadence via the GitHub API.
