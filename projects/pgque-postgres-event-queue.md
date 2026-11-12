# PgQue

## Summary
A pure PL/pgSQL revival of Skype's 2006-era PgQ architecture: a durable, fan-out event log inside Postgres using snapshot-based batching and TRUNCATE-based table rotation instead of per-row `UPDATE`+`DELETE`. Installs as plain SQL on any Postgres 14+ — including fully managed providers (RDS, Aurora, Cloud SQL, AlloyDB, Supabase, Neon) — with no C extension and no external daemon; `pg_cron` or any scheduler calls a tick function.

## Why I Should Care
Most of his stack already runs on managed Postgres (RDS/Aurora). This gives a Kafka-topic-like fan-out event log (multiple independent consumers, each with their own cursor, replaying the same shared log) without adding a message broker, without the extension-approval dance managed Postgres requires, and — critically — without the dead-tuple bloat that SKIP LOCKED-based queues (PGMQ, pg-boss, River, Oban) are documented to hit under sustained load.

## Problems It Can Remove
Removes the need to stand up Kafka, Redis Streams, or SQS purely for internal event distribution when the volume doesn't justify a dedicated broker. Removes the bloat-tuning burden (aggressive autovacuum settings, partitioning) that SKIP LOCKED queues need once they're under real load.

## Practical Uses
- Outbox-pattern event distribution directly inside an existing RDS/Aurora instance, with multiple services each consuming the same event stream independently.
- Audit/event trail that benefits from full Postgres transactional guarantees (ACID enqueue, WAL, point-in-time recovery) instead of a bolted-on logging table.
- A lightweight webhook/event fan-out backbone for an admin-portal SaaS without introducing a new infra dependency.
- Any case where "every consumer sees every event, from their own cursor" (true fan-out) is needed rather than competing-consumers job distribution.

## Product Opportunities
Backbone for a lightweight internal event/notification system in a SaaS product without adding a message broker to the deployed stack — directly relevant to the MSSP admin-portal context, where keeping the infra footprint small matters for ease of customer deployment.

## Agent / Automation Opportunities
None directly — this is backend infrastructure, not an agent-facing tool.

## Integration
SQL file installed into an existing database (`git clone` + `\i sql/pgque.sql` from the repo root). Needs something to call `pgque.ticker()` periodically — `pg_cron` is pre-installed or one command away on every major managed Postgres provider; a plain system cron or an app-level worker loop works as a fallback. Client libraries exist for TypeScript, Python, and Go, but the core API is plain SQL and thus usable from anything with a Postgres driver.

## Architecture Notes
Snapshot-based batching: the tick function compares two consecutive snapshots of "what's pending" and the diff between them determines event visibility for that batch — no row-level locking, no per-row `DELETE`. Old batches are retired via `TRUNCATE` on rotated tables rather than row deletion, which is what gives it zero dead tuples regardless of sustained load (the exact failure mode documented in Brandur/Heroku's 2015 60k-backlog incident, a PlanetScale 2026 postmortem, and an open River GitHub issue on autovacuum starvation). Each registered consumer holds its own cursor on the shared log, which is the structural difference from competing-consumers designs like SKIP LOCKED-based job queues.

## Maturity
Emerging. Created April 2026 (~7 months old at review), 1,874 stars, 45 forks, 102 open issues, latest tagged release v0.2.2 (2026-10-04). Single-author-dominant commit history (293 of ~345 commits from the author), with some AI-assisted commits (`claude`, `samo-agent`) visible in the contributor list — ordinary for a 2026 solo project but worth knowing. External corroboration exists: a detailed independent walkthrough of the snapshot/diff mechanism from Christophe Pettus (a well-known independent Postgres consultant) and an active Hacker News discussion thread.

## License
Apache-2.0 — unrestricted for commercial use, self-hosting, or embedding.

## Alternatives
PGMQ (visibility-timeout re-delivery, no true fan-out, no configurable backoff); River, Que, pg-boss, Oban, graphile-worker (job-queue frameworks requiring a language-specific worker binary, not language-agnostic event logs); Kafka/Redis Streams/SQS (external infra with a different ops burden). PgQue's own README includes a direct comparison table against all of these.

## Risks / Limitations
End-to-end delivery latency is tick-bound — median ~52ms at the default 100ms tick, tunable down to 1ms ticks but never sub-millisecond; explicitly the wrong tool if single-digit-millisecond dispatch is required. Still young (7 months) with 102 open issues against only 45 forks, suggesting real but still-settling usage. Single-author-dominant project — a dependency-risk factor if adopted for anything load-bearing.

## Recommendation
PROTOTYPE. Worth running against a real internal event-distribution use case on an existing RDS/Aurora database before deciding whether to depend on it — the architecture claims are specific and externally corroborated, but there's no multi-year production track record yet.

## Change History
### 2026-11-12
Initial discovery and review. Slot 2 run (product infrastructure, APIs, backend components). Found via GitHub Search API `topic:job-queue OR topic:task-queue stars:>50 pushed:>2026-08-01`.
