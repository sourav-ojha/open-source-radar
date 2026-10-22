# KiviDB

## Summary
Multi-threaded, RESP2/RESP3-compatible Redis/Valkey alternative written in Rust. Shards command handling across CPU cores instead of a single thread, and bundles Redis Stack-style full-text search (BM25) and vector search (FLAT/HNSW) directly into the same process. Existing `redis-cli` and Redis client libraries work against it unchanged, by default on port 6380 so it can run alongside a real Redis.

## Why I Should Care
A small product typically ends up running Redis (cache/sessions), a vector DB (RAG/embeddings), and a search engine as three separate systems. KiviDB is a drop-in-protocol single process covering all three, with real engineering behind the claims (benchmarks, integration tests, cluster mode) rather than just a README pitch.

## Problems It Can Remove
Standing up and operating pgvector/Qdrant plus Elasticsearch/Meilisearch plus Redis as three services, with three sets of client code, for a product that doesn't need enterprise scale on any one of them individually.

## Practical Uses
- Session cache + rate-limit counters + full-text search + vector search behind one self-hosted service for a micro-SaaS
- Drop-in cache layer for an existing Node app already using `ioredis`/`node-redis` — no client code changes
- Pilot vector search for a RAG feature without a separate vector-DB dependency
- Run on a different port alongside production Redis to evaluate before any cutover

## Product Opportunities
Collapsing cache + search + vector into one self-hosted component reduces the infra surface a solo/small-team SaaS has to operate, monitor, and pay for — directly relevant to keeping a micro-SaaS's hosting bill and operational burden down.

## Agent / Automation Opportunities
Could back an agent memory/RAG layer over RESP — a protocol Node already has mature, familiar client libraries for — instead of adding a brand-new vector-DB client dependency.

## Integration
Deployment: Docker (Docker Hub/GHCR, amd64+arm64, distroless image available), release binaries for Linux/macOS, `.deb`/`.rpm` with a systemd unit, or cluster mode (gossip, failure detection, failover, replication, ACL file).
Interfaces: RESP2/RESP3 (existing Redis client libraries work unchanged), `FT.*` search commands, optional Lua scripting.
Integration effort: **Low** — protocol-compatible, so adoption in an existing Node service is a connection-string change, not a client-library swap.

## Architecture Notes
Sharded per-core command engine (every CPU core handles commands, not a single thread) is the core performance claim versus classic single-threaded Redis. Persistence via snapshots (`dump.kdb`) plus an optional append-only file, bounding crash data loss to roughly the last second of writes. Uses io_uring on Linux where the kernel allows it, falling back to Tokio otherwise. Vector/text indexes (HNSW graphs, BM25) are included in snapshots so search quality survives a restart; most commands return `LOADING` while indexes rebuild at startup.

## Maturity
Emerging — repo created 2026-10-16, 6 days old at review. Despite the youth: published benchmark methodology and scripts, an automated benchmark report, end-to-end integration tests, a changelog, a contributing guide, and a security policy are all in the repo. The vendor's own benchmark (one AWS session) claims ~21M pipelined GETs/sec on 32 cores (~9x Redis, ~2.3x Dragonfly), but also shows Dragonfly still wins on non-pipelined GET/SET and at higher vector-search recall settings — not a universal win, and not independently reproduced.

## License
AGPL-3.0, flagged loudly per policy. Per the project's own README, applications connecting to KiviDB over the network are separate programs and not themselves subject to the AGPL — the obligation falls on modified copies of KiviDB itself offered as a network service, which must have source made available. A commercial license is offered for organizations that cannot accept AGPL terms. Using an unmodified KiviDB as a self-hosted backend behind a product is the low-risk case; modifying and redistributing the binary itself is not.

## Alternatives
- Redis/Valkey — the mainstream baseline; single-threaded by default, no built-in vector/text search
- Dragonfly — a more established multi-threaded Redis-compatible alternative; wins on non-pipelined ops and high-recall vector search per KiviDB's own benchmark
- KeyDB — older multi-threaded Redis fork, maintenance pace has slowed relative to both

## Risks / Limitations
- 6 days old at review — pre-production, roadmap and long-term maintenance unproven
- AGPL-3.0 licensing requires care around modification/redistribution scenarios
- Vendor-run, single-session benchmark; not independently verified
- Small team (6 contributors) for a database engine with cluster/replication ambitions

## Recommendation
**PROTOTYPE** (score 8.0/10) — worth standing up alongside existing Redis to evaluate the search/vector features on a non-critical workload before considering any cutover.

## Change History
### 2026-10-22
First discovered via Hacker News Show HN (69 points, 2026-10-19). Verified via GitHub API (AGPL-3.0, 396 stars, 6 subscribers, 14 forks, 13 open issues, created 2026-10-16, 6 contributors).
