# Ruvio

## Summary
A single-process, Rust-built key-value server that speaks the Redis wire protocol (RESP2/RESP3) — `redis-cli` and existing RESP clients connect to it unmodified — but folds a distributed lock service, conditional/compare-and-set writes, a change feed, and a delay queue into the same process as plain Redis-style commands (`LEASE`, `SEMAPHORE`, `SETV`, `CHANGES`, `XADD DELAY`, `ZMOVEDUE`) instead of requiring separate systems bolted on top of Redis for each.

## Why I Should Care
A typical small Node/Postgres service that needs caching, a distributed lock (to stop a cron job double-running across instances), a delay queue, and some form of change notification today usually means Redis plus Redlock plus a scheduler library plus some ad-hoc CDC. Ruvio is a serious attempt to collapse all of that into one binary that speaks a protocol every existing tool already knows how to talk to — directly relevant to trimming the infra footprint of an EC2/ECS-deployed service.

## Problems It Can Remove
Removes the need for a separate distributed-lock mechanism (the classic Redlock-correctness debate) by baking fencing-token semantics directly into the `LEASE`/`RENEW` commands. Removes the need for a bespoke delay-queue table or library (`ZPOPBYSCORE`/`ZMOVEDUE` give a due-job queue natively). Removes the need for Postgres logical replication or a separate CDC tool for simple in-memory change-feed needs (`CHANGES`).

## Practical Uses
- Swap-in replacement for Redis in a service that only uses it for caching and simple counters, with headroom to add locks/delay-queue/change-feed later without adding infra.
- Fencing-token-based distributed lock for a worker or cron job that must not run concurrently across instances.
- A due-job delay queue without pulling in BullMQ, pg-boss, or a scheduler library.
- A lightweight change feed for cache-invalidation fan-out without standing up Postgres logical replication.

## Product Opportunities
None directly identified — this is backing infrastructure, not an embeddable product component.

## Agent / Automation Opportunities
None directly — general-purpose backend infrastructure.

## Integration
Single Docker image (`salihcantekin/ruvio`, amd64/arm64), prebuilt binaries for Linux/macOS/Windows, or an AUR package for Arch. Default listens on `127.0.0.1:6379` exactly like Redis. Six first-party typed client libraries (Rust, C#, Go, Java, Python, **Node.js**) each cover all 299 documented commands; a browser-based Operator UI exists for inspecting data, Pub/Sub, and ACL users.

## Architecture Notes
Owner-per-shard engine: the shard that owns a given key parses, records, applies, and replies to it — the hot path never takes a lock or hops to another thread. Each feature only allocates memory when a key actually uses it (documented: 20,000 leases ≈ 3.6MB, 20,000 versioned 64-byte strings ≈ 7.2MB including body) — plain `GET`/`SET`/`INCR` consult none of the extra machinery. Durability is chosen at startup (`memory`, `snapshot`, `everysec`, `always`): a write is checked, appended to a WAL, then applied; if the log can't take the record, the key is left unchanged and later writes return `MISCONF` rather than silently losing data. Cluster routing and Raft-based failover are claimed but not independently verified in this review.

## Maturity
Experimental. Created mid-October 2026 (~1 month old at review), 219 stars, 20 forks, 5 subscribers, latest tagged release `ruvio-0.4.0` (2026-10-29). Despite the young age, packaging is unusually complete for the stage: published packages on crates.io, NuGet, npm, PyPI, and AUR, plus a documented benchmarks page and a compatibility-matrix JSON checked in CI — suggesting either a well-resourced solo author or substantial unreleased prior development.

## License
Server: AGPL-3.0 — flagged. Client libraries: MIT. Plain self-hosted internal use (running the server as a backing store, not redistributing or modifying it for a hosted multi-tenant offering) carries no AGPL obligation; a modified server exposed over a network to third parties would trigger the network-use clause. The Node.js client itself (used from application code) is separately MIT-licensed, so consuming it from app code carries no copyleft obligation regardless of how the server is deployed.

## Alternatives
Redis/Valkey + Redlock + a job-queue library (BullMQ/pg-boss) + custom CDC — the multi-system stack this replaces. KeyDB and DragonflyDB are faster Redis-protocol-compatible servers but without the native lock/queue/CDC primitives Ruvio adds as first-class commands.

## Risks / Limitations
About one month old at review with no independent production track record. Raft-based failover and multi-node cluster routing are claimed in the README but not independently verified here. AGPL-3.0 on the server needs a real legal read before any redistribution or managed-hosting scenario, even though plain self-hosted internal use is unaffected.

## Recommendation
PROTOTYPE. Worth trying as a Redis replacement for one low-stakes internal service to evaluate the lock/delay-queue primitives directly, but too early (one month old) to depend on for anything load-bearing yet.

## Change History
### 2026-11-12
Initial discovery and review. Slot 2 run (product infrastructure, APIs, backend components). Found via GitHub Search API `"rate limiting" OR "API gateway"` keyword sweep that surfaced it as a tangential but more interesting result (`ruvio` implements rate limiting as one of several native primitives, not as its primary focus).
