# CocoIndex

## Summary
Declarative, incremental data-transformation framework (Rust core, Python API) for building RAG and agent-context indexes. Declare a source-to-target flow once; CocoIndex recomputes only the delta — the changed file, row, or code symbol — on every subsequent run instead of a full batch re-embed.

## Why I Should Care
Incremental re-indexing (tracking what changed, invalidating only the affected downstream rows, retrying failures without reprocessing everything) is normally a bespoke and easy-to-get-wrong subsystem inside any RAG pipeline. CocoIndex makes it a declarative Python decorator with a Rust engine underneath handling retries, backoff, and dead-letter cases.

## Problems It Can Remove
Replaces a hand-rolled cron job that diffs source files against a hash table, re-embeds everything that changed, and hopes the bookkeeping doesn't drift — plus the full-corpus re-embed bill that comes with batch-only pipelines.

## Practical Uses
- Keep a RAG index over product docs/PDFs continuously fresh without re-embedding the whole corpus on every doc change.
- Build a code-embedding index (AST-aware chunking + sentence-transformers) that only re-embeds files touched by the latest commit.
- Structured extraction from forms/invoices/intake documents into Postgres with per-record lineage back to the exact source byte.
- Conversation/meeting-transcript-to-knowledge-graph pipelines (Neo4j/Kuzu) for internal tooling.
- Watch a folder of CSV/S3 files and publish each row as a Kafka message without hand-writing a watcher-plus-producer loop.

## Product Opportunities
Reusable ingestion/indexing layer for any product feature needing live-updating semantic search or agent context — e.g. a customer-facing document-search feature — without hand-building the incremental-sync logic underneath it.

## Agent / Automation Opportunities
Ships a "CocoIndex skill" file for AI coding agents so an agent writes correct flow code against the current API version, and a flagship "CocoIndex-code" MCP server product for AST-aware, incremental, semantic code indexing feeding Claude Code/Cursor directly.

## Integration
`pip install cocoindex`, then declare flows in Python against a target store (Postgres/pgvector, LanceDB, Neo4j, Qdrant, Kafka). Self-hosted; no CocoIndex-operated service required for the open-source engine. Effort: Medium — the declarative flow model (sources/targets/flows, `@coco.fn`) has its own learning curve versus an imperative embed-and-upsert script.

## Architecture Notes
Mental model is "React for data engineering": `Target = F(Source)`. The engine tracks per-row provenance so a source-file edit or a code change to the transformation function `F` propagates only to the rows whose output actually depends on what changed — everything else stays cached. The Rust core provides parallel chunking, zero-copy transforms where possible, and per-record failure isolation.

## Maturity
Emerging. Created March 2025, ~11.6k stars, active daily commits, tagged release v1.0.24. Two contributors (georgeh0, badmonster0) account for the large majority of commits — a small core team rather than a solo project, but concentrated bus-factor.

## License
Apache-2.0. No restrictions on commercial or embedded use. A paid "CocoIndex Enterprise" tier exists for PB-scale deployments, but the open-source engine is fully usable standalone.

## Alternatives
- Airbyte/Debezium — general ELT/CDC, not RAG-chunking/embedding-aware.
- LlamaIndex/LangChain ingestion pipelines — batch-oriented by default; incremental re-processing is not the core design point.
- Hand-rolled cron diffing — what this replaces directly.

## Risks / Limitations
- README is heavily marketing-produced (SVG hero graphics, "star us" CTAs throughout) — substance is real (20+ working examples, active CI, tagged 1.0 release) but requires reading past the polish to evaluate.
- Two-person commit concentration despite the star count.
- Declarative flow model requires learning CocoIndex's specific concepts (sources, targets, flows, the incremental engine) rather than writing an ad hoc script.

## Recommendation
PROTOTYPE — trial on a real RAG or code-embedding use case where re-indexing cost or staleness is currently a manual/cron-job problem, and compare against the effort of the existing hand-rolled approach.

## Change History
### 2026-09-26
Initial discovery and review.
