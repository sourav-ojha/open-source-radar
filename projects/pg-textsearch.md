# pg_textsearch

## Summary
PostgreSQL extension adding BM25 relevance-ranked full-text search via a simple `content <@> 'query'` operator, with configurable k1/b, Block-Max WAND top-k queries, and parallel index builds. Built by Timescale (of TimescaleDB/pgvectorscale).

## Why I Should Care
Closes the one real gap between plain Postgres full-text search and a dedicated search engine — proper BM25 ranking — without adding a second datastore, and it's a permissively-licensed single-purpose extension rather than an AGPL all-in-one platform.

## Problems It Can Remove
Implementing Block-Max WAND top-k scoring and parallel BM25 index builds correctly and fast is a non-trivial C-extension project; Timescale has already published benchmarks and passed Coverity static analysis on it.

## Practical Uses
- Add BM25-ranked keyword search to an existing Postgres table without standing up Elasticsearch/OpenSearch
- Pair with pgvector/pgvectorscale in the same database for hybrid (keyword + semantic) retrieval in a RAG pipeline
- Replace a bolted-on Algolia/Typesense integration for an app whose data already lives in Postgres
- Partitioned/expression indexes for per-tenant or per-category search in a multi-tenant SaaS

## Product Opportunities
Removes a second search-infra dependency (Elasticsearch/OpenSearch cluster) for any product where Postgres is already the system of record — meaningful ops-cost and complexity reduction for a small SaaS.

## Agent / Automation Opportunities
Not agent-specific, but a natural backing store for an MCP server that needs keyword search over structured records already in Postgres.

## Integration
Deployment: Postgres extension (prebuilt binaries for Linux amd64/arm64, or build from source)
Interfaces: SQL (extension)
Integration effort: **Low**

## Architecture Notes
Adds a `bm25` index type and a `<@>` distance-like operator that returns negative BM25 scores (lower = more relevant, matching ascending index-scan order). Runs inside Postgres via `shared_preload_libraries`, so index scans stay in-process rather than round-tripping to an external search service.

## Maturity
Emerging. GitHub API verified: PostgreSQL License, 3,985 stars, pushed_at 2026-09-19, created 2025-07-06. Multi-contributor (Timescale org, 5+ distinct contributors sampled). Real CI, benchmark workflow, and Coverity Scan badge in README.

## License
PostgreSQL License (permissive, BSD/MIT-style). Verified against LICENSE file. No restrictions on commercial use, SaaS deployment, or redistribution.

## Alternatives
- paradedb/paradedb (9,283★, AGPL-3.0) — broader all-in-one Postgres search+vector+aggregation platform (pg_search extension) with more features, but AGPL-3.0 requires source disclosure or a commercial license for a modified hosted service; pg_textsearch is a narrower, permissively-licensed single extension if BM25 keyword ranking is the only gap
- Elasticsearch/OpenSearch (assumed known) — full search clusters, much higher operational overhead for a feature this narrow
- Postgres built-in tsvector/GIN (assumed known) — no BM25 ranking, weaker relevance ordering

## Risks / Limitations
- Pre-1.0 (v1.4.0), created July 2025 — young for a database extension, verify against a non-critical table first
- Requires PostgreSQL 17/18 (19 beta best-effort) and `shared_preload_libraries` changes — a server restart is needed to install, not a zero-downtime add

## Recommendation
**USE NOW** (score 8.3/10) — see Why I Should Care above.

## Change History
### 2026-09-19
First discovered and reviewed. Verified via GitHub API and LICENSE file: PostgreSQL License, current version v1.4.0 (2026-08-18).
