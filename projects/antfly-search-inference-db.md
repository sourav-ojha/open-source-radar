# Antfly

## Summary
A search-and-inference database with a Zig core that puts full-text (BM25), dense and
sparse vector search, graph traversal, and in-process embedding/reranking/OCR/
transcription models over one table — with built-in RAG agents, MCP/A2A protocol
adapters, and a Postgres CDC mirror — exposed through embeddable SDKs for TypeScript,
Go, Python, and Rust as well as an HTTP server and a Postgres extension.

## Why I Should Care
Building a search or RAG feature normally means wiring together a full-text engine, a
vector DB, a graph DB, and glue code for chunking/embedding/reranking. Antfly collapses
that into one Apache-2.0 engine with a real TypeScript SDK and an embeddable package
(`@antfly/embedded`), which is unusually convenient for a Node/React stack rather than
assuming a Python-first ML pipeline.

## Problems It Can Remove
Standing up Elasticsearch/OpenSearch + Pinecone/Qdrant + Neo4j + a RAG framework
separately; hand-building a CDC pipeline to keep a search index in sync with Postgres;
writing custom reranking/fusion logic for hybrid search.

## Practical Uses
- Replace a multi-service search stack with one engine for a product's search feature
- Stand up a support-answer, site-search, or ticket-routing RAG agent from the
  documented end-to-end guides
- Mirror a Postgres table into Antfly over logical replication for hybrid search over
  existing app data with no separate ETL job
- Run it embedded (Antfly Lite / `@antfly/embedded`) inside a Node service or the
  browser for local-first search, no server required
- Query it from Postgres directly via the `antfly_postgres` extension's `@@@` operator

## Product Opportunities
A "search + RAG backend" micro-SaaS offering that undercuts assembling Algolia +
Pinecone + a reranker + a RAG framework separately; or embed it as the retrieval layer
inside a support-bot or docs-search commercial product without three separate vendor
bills.

## Agent / Automation Opportunities
Native MCP and A2A protocol adapters let a coding agent or any agent framework query it
directly as a tool. It also ships a query-builder agent that turns a natural-language
question into a structured Antfly query, with its own retrieval-quality eval metrics.

## Integration
Docker/docker-compose or Kubernetes for the server; Homebrew tap for the CLI; build from
source with Zig. For direct use from a Node/TS app, the `@antfly/sdk` (HTTP client) or
`@antfly/embedded` (in-process, including a browser WASM build) packages avoid running a
separate server entirely. Low effort for the embedded path, medium for operating the
full distributed server (multi-Raft).

## Architecture Notes
Hybrid search fuses BM25, RaBitQ-compressed dense vectors with SPFresh-style updates,
and SPLADE sparse vectors via reciprocal-rank or relative-score fusion in one query.
Graph indexes build automatic relationship extraction and traversal over the same data.
Runs as a single `.aflite` file, a single node with hot standby, a multi-Raft cluster, or
(in progress) serverless over S3-compatible object storage. SQL and a Postgres wire
protocol are tracked as in-progress work (antflydb/antfly#145).

## Maturity
Emerging / pre-1.0. Created 2026-03-15 (~7.5 months old), latest tagged release v0.2.5
(2026-09-28). 480 stars, 32 forks, 56 open issues at review time — active development,
real infrastructure investment (Homebrew tap, Docker registry, hosted docs site,
Antfly Cloud), but genuinely pre-1.0 with explicitly in-progress major features.

## License
Apache-2.0. No restrictions on commercial or SaaS use.

## Alternatives
Elasticsearch/OpenSearch + a separate vector DB + a graph DB wired together by hand;
ParadeDB (already catalogued, Postgres-native full-text + vector, narrower scope, no
built-in inference/RAG/graph); WhoDB and similar self-hosted DB workspaces (already
catalogued — a client over existing databases, not a storage engine itself).

## Risks / Limitations
- Pre-1.0 with a Zig core — expect breaking changes before 1.0; SQL/Postgres-wire and
  serverless-over-object-storage are explicitly marked in-progress
- Only 1 GitHub subscriber against 480 stars / 32 forks is an unusual ratio, though the
  real fork count, Homebrew tap, Docker registry, and documented Postgres extension argue
  against a fake-engagement pattern — worth re-checking if adoption signals stall
- Running the full server means operating a new piece of distributed infrastructure
  (multi-Raft) rather than a managed service

## Recommendation
PROTOTYPE — the architecture and embeddable TypeScript surface are genuinely useful for
a Node/React stack; worth a small real experiment (e.g. Antfly Lite embedded in a side
project) before betting production search/RAG infrastructure on it pre-1.0.

## Change History
### 2026-10-31
Initial discovery and cataloguing (slot 4: data/search/documents/RAG).
