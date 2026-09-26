# seekdb

## Summary
MySQL-protocol-compatible database from the OceanBase team, embeddable in-process or run as a server, unifying vector, full-text, and scalar/relational search behind one SQL query. Ships a Change-Stream async index pipeline for millisecond-fresh vector search after writes, and copy-on-write `FORK DATABASE` / `MERGE TABLE` sandboxes for safe agent-state experimentation.

## Why I Should Care
The FORK/MERGE copy-on-write sandbox is a genuinely novel primitive for agent workflows: letting an autonomous agent write state and then safely roll it back today usually means hand-rolled snapshot/restore logic at the application layer. Here it's a kernel-level database feature, snapshotting an entire database in seconds with no data copy.

## Problems It Can Remove
Removes the need to run three separate systems (relational DB, vector DB, full-text search engine) and glue their results together client-side for an agent-memory or hybrid-search feature.

## Practical Uses
- Single embedded database for an agent's episodic memory instead of separately running Postgres+pgvector plus Elasticsearch/OpenSearch.
- `FORK DATABASE` to snapshot agent state before a risky autonomous action; `MERGE` or `DROP` depending on outcome.
- Hybrid retrieval (vector + full-text + row filters) in one SQL query for a product search feature instead of client-side result merging.
- Existing Node MySQL drivers/ORMs work unmodified against it for prototyping, since it speaks the MySQL wire protocol.
- Already has merged integration PRs into LangChain, LlamaIndex, and Dify, so it drops into an existing RAG stack without a custom connector.

## Product Opportunities
Could simplify a product's memory/search layer down to one database instead of three for early-stage products where operational simplicity matters more than best-in-class per-component performance.

## Agent / Automation Opportunities
Positioned explicitly as "the state store for AI agents": continuous write + millisecond-later read is the canonical agent memory loop, and the async index pipeline is designed specifically so P99 read latency doesn't spike under that workload.

## Integration
`pip install pyseekdb` for embedded/in-process mode (SQLite-like, no server needed), or a Docker image / systemd binary for standalone server mode, scaling up to the full distributed OceanBase cluster if ever needed. Effort: Medium — MySQL protocol compatibility is familiar, but FORK/MERGE and hybrid-search syntax are seekdb-specific SQL extensions to learn.

## Architecture Notes
Async index pipeline ("Change Stream") decouples DML commit from index construction: the write path commits and returns without waiting on index build, while a background pipeline consumes the redo log and updates a two-level (incremental + snapshot) HNSW index. Queries hit both the delta and snapshot indexes with fine-grained read locks, which is what keeps P99 flat under concurrent write+search load per the vendor's own benchmark.

## Maturity
Emerging. Created October 2025 (~11 months old), 3,061 stars, active daily commits, tagged release v1.4.0. Built by OceanBase (the production database vendor behind Alipay/Taobao/DiDi's infrastructure) — mature engineering organization behind a young product.

## License
Apache-2.0. No restrictions on commercial or embedded use.

## Alternatives
- Postgres + pgvector + a separate full-text engine — more individually mature, more moving parts operationally.
- Milvus/Qdrant for vector-only — seekdb's own (vendor-produced, not independently verified) benchmark claims 10.7x its QPS on a streaming write+search workload.
- ParadeDB/pg_textsearch (already evaluated) — Postgres-native BM25 without the vector+relational unification or COW sandboxing.

## Risks / Limitations
- Young project (~11 months) even though the parent OceanBase engine is mature; FORK/MERGE/APPROXIMATE SQL extensions are new surface area without a long track record.
- Performance claims (10.7x Milvus, 3.2x Elasticsearch) are from OceanBase's own launch blog and benchmark repo, not third-party reproduced.
- The features that make it interesting (FORK/MERGE, hybrid search) are seekdb-specific SQL, not portable to a plain MySQL/Postgres migration path later.

## Recommendation
PROTOTYPE — worth a hands-on trial for an agent-memory or hybrid-search use case specifically to test the FORK/MERGE sandbox pattern and the streaming-write-then-read latency claim under a realistic workload.

## Change History
### 2026-09-26
Initial discovery and review.
