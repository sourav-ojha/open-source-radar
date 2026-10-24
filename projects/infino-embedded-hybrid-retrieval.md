# Infino

## Summary
An embedded retrieval library (Rust core, with Python/Node/Rust bindings) providing full-text
(BM25), vector, hybrid, and SQL query over a single Parquet-backed table, stored on local disk or
object storage. No server process to run — `infino.connect()` works in-process — with published
warm-latency benchmarks at 1M and 10M documents.

## Why I Should Care
Adding search or basic RAG retrieval to a small product feature usually means standing up and
tuning a separate service (Elasticsearch, a vector DB, or hand-wiring Postgres FTS + pgvector).
Infino folds BM25, vector, and SQL query into one embeddable table with an npm package, which fits
directly into a Node/Express backend without adding infrastructure.

## Problems It Can Remove
- Running and operating a dedicated search service just to add "search my data" to a feature.
- Hand-fusing BM25 and vector ranking scores when hybrid search is needed.
- The cost and ops overhead of a networked search cluster for a feature with modest scale.

## Practical Uses
- Add hybrid search to a small Node/Express service with `npm install @infino-ai/infino` and no
  separate search infra.
- Store the index as Parquet on S3/R2 for a serverless or multi-instance deployment with no
  persistent search server to manage.
- Prototype RAG retrieval locally (`memory://`) and move the same table definition to object
  storage for production without changing query code.
- Embed directly inside an MCP server or agent tool as the retrieval backend instead of depending
  on a networked vector-DB service.

## Product Opportunities
A single `hybrid_search()` call fusing BM25 and vector ranking is the kind of building block that
removes a full sprint of search-infra work from any product feature that needs "search my data."

## Agent / Automation Opportunities
Light enough to embed directly inside an MCP server or custom agent tool as the retrieval backend,
avoiding a networked vector-DB dependency for small-to-medium corpora.

## Integration
`npm install @infino-ai/infino`, `pip install infino`, or `cargo add infino`. Runs in-process;
storage target is local disk or any S3-compatible object store. Low integration effort. Installs
the mimalloc global allocator by default — disable it (`default-features = false`) if embedding in
a process that sets its own allocator.

## Architecture Notes
Ordinary Parquet is the storage format — no custom index file format to manage — with FTS and
vector indexes layered on top. Published benchmarks: vector top-10 search at 591µs (1M docs, warm,
object storage) to 5ms (10M docs); BM25 top-10 including row fetch at 125µs to 2ms over the same
range. Reproducible via `cargo bench`.

## Maturity
Emerging. Created 2026-05-19 (~5 months old), pre-1.0 (v0.11.11), 139 stars, 26 forks, 14
contributors. Published and version-matched across npm, PyPI, and crates.io.

## License
Apache-2.0 — unrestricted commercial use.

## Alternatives
- **pgvector + Postgres full-text search** — free if Postgres is already in the stack, but
  requires running and tuning Postgres as the search layer.
- **Qdrant/Weaviate/Milvus** — full vector-DB servers, more capable at scale, heavier to run for a
  single small service.
- **turbopuffer, Marqo** — similar vector+BM25-on-object-storage design, but commercial/cloud-first
  rather than self-hostable the same way.

## Risks / Limitations
- Young project, pre-1.0 — API surface may still shift.
- 14 contributors and 139 stars — real but modest adoption, limited production track record so
  far.
- No independent benchmark against pgvector/Elasticsearch at equivalent configuration was found —
  the published numbers are first-party.

## Recommendation
PROTOTYPE — worth a trial on a small Node service that currently fakes search with a SQL `LIKE`
query or has no search at all, before committing to it for anything production-critical.

## Change History
### 2026-10-24
Discovered. Catalogued at PROTOTYPE, 7.8/10.
