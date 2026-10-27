# Rostam

## Summary
An embeddable Go engine that combines a vector database (HNSW/IVF/Vamana indexes, SQ8/binary/PQ quantization, hybrid dense+sparse plus BM25 search) and a sub-microsecond key-value store behind one `Store` interface. Runs as an in-process library with no server, a standalone TCP server, or a Raft-replicated cluster, with Python/REST/gRPC clients and a bundled MCP stdio server for AI-agent memory.

## Why I Should Care
Most projects that need both vector search and a fast KV/cache layer end up running two separate systems (e.g. Qdrant + Redis). Rostam's pitch — one embeddable Go engine, no server to operate if used as a library — directly reduces the infrastructure footprint for a self-hosted AI feature, which matters more here than raw throughput.

## Problems It Can Remove
Removes the need to deploy and operate a separate vector DB plus a separate cache/KV service for a RAG or agent-memory feature. Also removes the need to hand-build a semantic LLM response cache or RAG ingestion pipeline — both ship as CLI/proxy tools out of the box.

## Practical Uses
- Embed as a Go library to add vector search + caching to a service with zero extra infrastructure to deploy.
- Point a coding agent at the bundled MCP stdio server for persistent, queryable agent memory.
- Run the OpenAI-compatible semantic caching proxy in front of an LLM API to cut repeated-query spend.
- Use the RAG CLI to ingest documents and get cited answers against the built-in BM25 index.

## Product Opportunities
A lean, embeddable retrieval+cache layer for a micro-SaaS AI feature that needs to ship without adding a vector-DB line item to the infra bill.

## Agent / Automation Opportunities
MCP stdio server purpose-built for agent memory; WASM stored procedures allow custom server-side query logic without round-tripping data to application code (cgo build / Docker image required for this feature — pure-Go release binaries don't include it).

## Integration
Medium effort. Embeddable as a Go library (Direct backend) for the simplest path; standalone server with REST/gRPC/Python clients for polyglot use; Raft cluster mode for HA. WASM stored procs need the Docker image, not the plain release binary. mmap persistence is Linux/Windows-64-bit only; AVX2 kernels are Linux/amd64-only.

## Architecture Notes
A single `rostam.Store` facade sits over three backends — Direct (in-process), Embedded (per-shard Raft), Client (TCP) — which is a clean way to let the same application code run unmodified from solo embedded use up to a replicated cluster. The vector engine is kept standalone with minimal in-repo dependencies, which is worth noting as a pattern for anyone building a similarly ambitious combined-engine project.

## Maturity
Beta; APIs may change before 1.0. 37 stars, 3 forks, 1 subscriber, 2 open issues at review time — a very small team for the breadth of scope (vector DB + KV + Raft + WASM stored procedures + MCP server + caching proxy + RAG CLI). The project's own benchmark page explicitly caveats its numbers as same-session comparisons with up to 42% hardware drift between sessions, and states its own benchmarks are "not a substitute" for running `make bench` independently — unusually honest, but a signal the headline throughput claims (1.8-2x Milvus/pgvector, ~4x Qdrant) should be treated as directional only.

## License
Apache-2.0. No restrictions on commercial or embedded use.

## Alternatives
- Milvus, Qdrant, pgvector — vector search only, mainstream, already excluded from this radar's discovery scope.
- Redis, Memcached — KV/cache only, mainstream.
- Running the above as two separate services — the status quo Rostam collapses into one engine.

## Risks / Limitations
- Small-team risk given the project's scope — verify any single component (vector search, KV, Raft, WASM) independently before depending on all of them at once.
- Self-reported benchmarks, explicitly caveated by the authors themselves as not independently reproduced.
- Bulk ingest is HTTP-only; the KV store has no REST interface.
- Server refuses to bind to a reachable address without authentication unless `-insecure` is passed — sane default, but confirm before exposing publicly.

## Recommendation
PROTOTYPE — worth testing as an embedded library for a specific RAG or agent-memory feature where collapsing two infra dependencies into one is valuable, but treat the broader cluster/WASM feature set as unproven until tried directly.

## Change History
### 2026-10-27
First reviewed. Surfaced via an "embedded database engine" GitHub search during slot 7 (experimental/hidden gems).
