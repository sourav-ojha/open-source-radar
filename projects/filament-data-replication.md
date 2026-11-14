# Filament

## Summary
A pluggable data-replication engine (full, incremental, or change-data-capture) with
durable checkpointing, batch integrity verification on both sides of a write, and safe
pause/resume from the last checkpoint. Sources, sinks, state storage, and event transport
are all replaceable interfaces; runs as a standalone service with an API and web UI, or
embeds directly in a Go application.

## Why I Should Care
Data-movement tasks tend to start as a sync script and outgrow it badly before anyone
reaches for Debezium's full Java/Kafka stack. Filament sits in between: a single Go binary
with real checkpoint/integrity guarantees and a pluggable source/sink model, without
Kafka as a hard dependency.

## Problems It Can Remove
Removes the correctness risk of a hand-rolled sync script (silent data loss or
duplication on a crash mid-sync) without taking on Debezium/Kafka's operational weight.

## Practical Uses
- Replicate data from a Postgres/MySQL source to a search index, warehouse, or another
  database without standing up Kafka and Debezium's full footprint.
- Run a lighter-weight CDC pipeline where the already-catalogued go-pq-cdc's Postgres-only
  scope is too narrow and Debezium is more than the job needs.
- Embed the replication engine directly inside a Go service rather than running it as a
  separate standalone system.

## Product Opportunities
Backing engine for a "sync your database to our product" onboarding flow without building
a bespoke ETL pipeline per source/sink pair.

## Agent / Automation Opportunities
REST API and web UI are the primary interfaces; no MCP server, but the API surface is
straightforward enough for an agent to drive replication jobs via HTTP calls if wired up.

## Integration
Deployment: single binary (install script, Homebrew tap), Docker, self-hosted service
with its own API/web UI, or embeddable as a Go library. Integration effort: **Medium** —
local development needs Go, Docker, Node.js, pnpm, and NATS, though production deployment
is lighter (a single binary).

## Architecture Notes
Checkpointing and two-sided batch integrity verification are the core correctness
mechanisms — worth studying as a reference for how to build resumable, verifiable data
pipelines without a message-queue dependency as the source of truth for progress.

## Maturity
Emerging. Created 2026-06-30 (~5 months), 309 stars, 42 forks, 2 subscribers. Tests
configured in CI, Slack community, DeepWiki docs, latest release v0.43.0 (2026-11-14).
Not archived.

## License
Apache-2.0, no restrictions.

## Alternatives
- Debezium (mainstream, Java/Kafka-based, heavier operationally).
- go-pq-cdc (catalogued) — Postgres-source-specific, narrower scope than Filament's
  pluggable source/sink model.
- Airbyte (mainstream, much larger connector catalog, heavier deployment).

## Risks / Limitations
- 309 stars / 2 subscribers (154:1 ratio) is elevated, though 42 forks and 27 open issues
  suggest real engagement rather than inflation.
- 5 months old — young for data-movement infrastructure; test the checkpoint/resume
  guarantees directly before trusting it with anything where data loss or duplication
  matters.
- Local development requires a heavier toolchain (Go, Docker, Node.js, pnpm, NATS) than the
  single-binary production deployment suggests.

## Recommendation
PROTOTYPE — run a real replication job (Postgres to a search index or a second database)
through a deliberate crash-and-resume test before trusting its checkpoint guarantees with
anything production-critical.

## Change History
### 2026-11-14
Initial discovery and review. GitHub API verified: Apache-2.0, 309 stars, 42 forks, 2
subscribers, created 2026-06-30, pushed_at 2026-11-14, not archived.
