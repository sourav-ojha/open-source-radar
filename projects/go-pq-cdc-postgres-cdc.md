# go-pq-cdc

## Summary
Lightweight Go library/binary (MIT, from Trendyol) for Change Data Capture on PostgreSQL using logical replication, streaming row-level changes to Kafka, Elasticsearch, or a custom sink — includes a published benchmark against Debezium and a point-in-time snapshot feature for initial sync before streaming begins.

## Why I Should Care
Gives a Postgres-only CDC path without pulling in a JVM/Kafka Connect deployment just to stream database changes downstream — relevant directly to the Postgres piece of the stack.

## Problems It Can Remove
Removes the need to run Debezium's full Kafka Connect + JVM stack, or to hand-roll a polling/dual-write sync job, just to keep a search index or cache in sync with Postgres.

## Practical Uses
- Sync a Postgres table to Elasticsearch/OpenSearch for search, without a polling or dual-write job.
- Feed row-level change events into a webhook/notification pipeline (e.g. notify on new orders) without standing up Kafka Connect.
- Point-in-time snapshot plus ongoing CDC for backfilling a new read replica or cache with no downtime.
- Replace a heavier Debezium + Kafka Connect + JVM deployment with a single lightweight Go binary for a smaller-scale CDC need.

## Product Opportunities
Could be the sync engine behind a "keep your search index in sync with Postgres automatically" feature for a product or admin portal, or useful glue for a micro-SaaS needing near-real-time replication without committing to a full streaming platform.

## Agent / Automation Opportunities
Could back an MCP tool or automation that notifies an agent of relevant database changes as they happen, rather than the agent polling the database itself.

## Integration
Go library (`go get github.com/Trendyol/go-pq-cdc`) or a standalone binary/Docker deployment. Integration effort: **Medium** — requires enabling Postgres logical replication and wiring a sink (Kafka/Elasticsearch documented out of the box; anything else needs a custom sink implementation).

## Architecture Notes
Uses PostgreSQL's `pg_export_snapshot()` for a consistent point-in-time snapshot, then streams ongoing changes via logical replication. Chunk-based processing and auto-selected partitioning strategy (integer range, CTID block, or offset) keep large-table snapshots memory-efficient; supports multi-instance parallel processing and automatic crash recovery.

## Maturity
Mature in production use (built and open-sourced by Trendyol, a large e-commerce company). GitHub API verified: MIT, 211 stars, pushed 2026-10-02, created 2024-04-27, latest release v1.11.14 (2026-08-24).

## License
MIT, confirmed via GitHub metadata. No restrictions.

## Alternatives
- Debezium (13,174★, mature, JVM/Kafka Connect-based) — the mainstream default, but far heavier infrastructure for a comparable job; go-pq-cdc publishes a direct benchmark against it in-repo.
- Supabase Realtime / pg_eventserv — simpler for Supabase-specific or notify-only use cases, not a general CDC-to-Kafka/Elasticsearch pipeline.
- A hand-rolled `pgoutput` consumer — the DIY path this project already wraps correctly (partitioning, crash recovery, snapshotting).

## Risks / Limitations
- Smaller community (211 stars) than Debezium; verify operational maturity (replication-slot management, failover behavior) for a given production workload before relying on it at scale.
- Go-only — no native Node.js binding, so it runs as a standalone service/sidecar rather than an in-process library for a Node backend.
- Documented sinks are Kafka/Elasticsearch-oriented; anything else needs a custom sink written against its Go API.

## Recommendation
**PROTOTYPE** (score 7.5/10) — worth testing against a real Postgres-to-search-index sync need before committing, given the smaller community relative to Debezium.

## Change History
### 2026-10-03
First discovered and reviewed. Verified via GitHub API: MIT, 211 stars, current version v1.11.14 (2026-08-24). Surfaced from a '"change data capture" in:description' search; the Debezium benchmark claim is in-repo but not independently re-run in this pass.
