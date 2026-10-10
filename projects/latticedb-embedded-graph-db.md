# LatticeDB

## Summary
LatticeDB is an embedded, single-file property-graph database with native HNSW vector search and BM25 full-text indexing, all queried through one Cypher-like language. The core engine is written in Zig, with bindings for Python, TypeScript/Node.js, Go, and Java (JNI). There is no server process — the entire database is one portable file, opened directly by the embedding application.

## Why I Should Care
Most embedded databases cover one retrieval axis — graph, vector, or text — and leave you to bolt the others on. LatticeDB puts graph traversal, HNSW vector similarity, and BM25 full text behind a single query layer over a single file, with published performance numbers (0.13 microsecond node lookups, 0.83 ms vector search at 1M vectors with 100% recall) that are credible for a single-writer embedded engine. Bindings already reach Node and Python, which drops straight into a MERN-adjacent stack without introducing a server to operate.

## Problems It Can Remove
- Replaces a 3-service stack (Postgres + pgvector + Elasticsearch/Meilisearch) with one embedded file for a single-tenant or desktop product.
- Removes the need to stand up Neo4j plus a separate vector database for a Graph RAG prototype.
- Avoids building and operating a custom agent-memory store that otherwise requires wiring together a relational DB, a vector index, and a text search engine.

## Practical Uses
- Local agent-memory store combining graph relationships, embeddings, and full-text search in one file.
- Graph RAG prototyping without external infrastructure.
- Embedded knowledge graph inside an Electron desktop app or CLI tool that must run with zero external services.
- Local-first applications needing relationship + semantic + text search with no ops burden.

## Product Opportunities
Could replace a multi-service search/graph/vector stack with one embedded file for a single-tenant or desktop product, or serve as the engine behind an internal agent-memory/knowledge-graph feature without provisioning new infrastructure.

## Agent / Automation Opportunities
Fits naturally as the storage layer for an agent-memory system or a local Graph RAG pipeline — relationships, embeddings, and full text in one transactional file that an agent process can open directly, no network hop to a database service.

## Integration
`pip install latticedb` or `npm install @hajewski/latticedb` for the Python/Node bindings; also available for Go (cgo) and Java (JDK 21+, JNI). Published packages are expected to bundle the native `liblattice` library on supported platforms. Electron apps need a manual unpack-from-asar step for the native library, documented in the TypeScript binding's README. Integration effort: **Low** for Python/Node prototyping, **Medium** if packaging for distribution across platforms (native library bundling).

## Architecture Notes
Zig core (3.5 MB of the repo's source) handling storage, WAL-backed durability, graph traversal, HNSW vector indexing, and BM25 text indexing behind one query layer; a Cypher-like query language is the primary interface (`MATCH ... WHERE ... RETURN`, with vector distance operators like `<=>` and full-text match operators like `@@` usable in the same query). A built-in deterministic `hash_embed` helper exists purely so examples run with no external embedding service — explicitly not meant for real similarity search.

## Maturity
Emerging, pre-1.0 (current release v0.15.0, published 2026-08-29). Created December 2025. 733 stars, 34 forks, but only 2 GitHub "watchers" — a soft signal worth tracking rather than treating star count alone as proof of adoption. Commit history is dominated by a single author (494 of roughly 500 commits), with three other contributors at 1-2 commits each.

## License
MIT, confirmed via the repository's LICENSE file. No restrictions on commercial or embedded use.

## Alternatives
Neo4j (server-based, not embedded), Kuzu (embedded graph DB, no native vector+FTS fusion in the same query layer), SQLite + sqlite-vec (needs separate FTS5 setup, no native graph model), LanceDB (vector+SQL focus, no graph model).

## Risks / Limitations
- Single-maintainer-dominated project — verify longevity before depending on it beyond a prototype.
- Young and pre-1.0 — on-disk format and API surface may still change between releases.
- Native bindings carry platform-specific packaging concerns (see Electron asar-unpacking requirement).
- Low watcher count relative to stars is worth monitoring for genuine community pickup.

## Recommendation
PROTOTYPE — strong technical fit for embeddable agent-memory and Graph-RAG use cases in a Node/Python stack, but single-maintainer risk and pre-1.0 status mean it belongs in a side project or internal tool first, not a production critical path, until it accumulates more independent usage.

## Change History
### 2026-10-10
Discovered via GitHub search (slot 4: data, search, documents, RAG). First catalog entry.
