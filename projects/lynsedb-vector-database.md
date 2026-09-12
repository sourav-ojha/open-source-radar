# LynseDB

## Summary
LynseDB is a Python-first vector database with a Rust storage/search core (Apache-2.0). The same client API works embedded (local files), as a single HTTP server, or as a coordinator-backed sharded cluster. Supports dense vectors, BM25/hybrid search, sparse vectors, and domain-specific distance metrics (geospatial, binary fingerprints, probability distributions) through one collection API.

## Why I Should Care
Most embedded-vector-DB options (Chroma, LanceDB) force a rewrite when a prototype needs to scale to a real service. LynseDB's core pitch is keeping the same client API from local file storage through a sharded cluster — useful for a product that starts as a side feature and might need to scale later.

## Problems It Can Remove
Removes the need to stand up a separate vector-DB service for early-stage RAG/agent-memory features, and removes the migration cost of switching vector DBs when a feature does need to scale.

## Practical Uses
- Start a RAG or agent-memory feature with an embedded local vector store, then move to a hosted server or shard cluster without changing client code.
- Combine dense-vector and BM25 hybrid search in one collection API.
- Use domain-specific distance metrics (geospatial Haversine, fingerprint Tanimoto/Dice) that typical vector DBs don't expose.

## Product Opportunities
Embed as the retrieval layer inside a product without standing up a separate vector-DB service; cluster mode could back a small multi-tenant SaaS's retrieval needs.

## Agent / Automation Opportunities
Usable as the embedding/retrieval backend behind an agent-memory MCP server; local embedded mode is a good fit for a CLI tool that needs semantic search without external infra.

## Integration
`pip install lynsedb`; embedded mode requires zero infra. Server mode ships Docker/Kubernetes examples with API keys, health checks, metrics, and OpenAPI. Low effort to start, medium if standing up cluster mode.

## Architecture Notes
Rust core handles vector storage, search, indexes, filters, WAL, snapshots, and server execution; the Python layer stays a thin client across all three deployment modes (embedded/server/cluster). Cluster mode adds shard groups, stable hash bucket routing, coordinator fan-out search, and replica write mirroring.

## Maturity
Emerging — created December 2023, actively maintained (latest release v0.9.0, 2026-08-25), but only 40 stars after ~2 years signals limited real-world validation.

## License
Apache-2.0. No restrictions on commercial or SaaS use, redistribution, or embedding.

## Alternatives
Qdrant, Chroma, LanceDB, Milvus Lite.

## Risks / Limitations
Small community despite active maintenance — verify cluster-mode maturity before production use. Rust core means fewer contributors can review internals.

## Recommendation
PROTOTYPE — worth testing the embedded-to-server growth path in a small RAG or agent-memory feature before committing.

## Change History
### 2026-09-12
Initial discovery and review.
