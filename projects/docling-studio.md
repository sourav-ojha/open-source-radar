# Docling Studio

## Summary
Vue 3 + FastAPI visual studio (MIT, scub-france) built on IBM's Docling extraction library. Upload a PDF, configure the extraction pipeline (OCR, tables, formulas, picture classification) in the browser, inspect bounding boxes per page, then chunk/embed/index straight into OpenSearch and mirror the full document tree as a Neo4j graph.

## Why I Should Care
Docling is a strong document-extraction engine but headless — Docling Studio fills the "see what actually got extracted before you trust it" gap with a real UI, which matters when tuning a RAG ingestion pipeline's chunking strategy.

## Problems It Can Remove
Removes the trial-and-error of tuning a document-extraction pipeline blind — bounding-box visualization shows exactly what a given configuration extracted per page before committing to it in code.

## Practical Uses
- Visually tune a Docling extraction pipeline (chunking strategy, token limits) before writing ingestion code.
- Debug why a PDF's tables or formulas extracted incorrectly by inspecting bounding boxes over the source page.
- One-click ingest a folder of documents into OpenSearch plus a Neo4j graph for a RAG prototype.

## Product Opportunities
Could shortcut building a "document ingestion admin panel" that a RAG-based product needs, rather than building one from scratch.

## Agent / Automation Opportunities
The ingestion pipeline (Docling → chunking → embedding → OpenSearch) could be automated as a backend step for an agent-facing document search feature; the graph mirror in Neo4j is useful context for an agent doing structure-aware retrieval.

## Integration
Two services (Vue frontend + FastAPI/Docling backend); OpenSearch and Neo4j needed only if using the full ingestion pipeline rather than just the inspector. Medium effort.

## Architecture Notes
Backend uses hexagonal architecture (ports & adapters) — pure domain layer separate from HTTP and persistence — worth a look as a reference structure for a similar ingestion service. Graph storage mirrors the full DoclingDocument tree (sections, paragraphs, tables, pages, chunks) with typed relations.

## Maturity
Emerging — created May 2025, 258 stars, 40 open issues, latest release v0.7.1 (2026-08-24).

## License
MIT. No restrictions on commercial or SaaS use, redistribution, or embedding.

## Alternatives
Docling itself (headless), OpenDataLoader PDF (catalogued, no visual UI), LlamaParse (hosted, proprietary).

## Risks / Limitations
Small single team, no formal release cadence beyond patch tags. Full ingestion pipeline adds two more infra dependencies (OpenSearch, Neo4j) beyond just using the visual inspector.

## Recommendation
PROTOTYPE — worth trying as a pipeline-tuning tool before building a custom document-extraction debugging UI.

## Change History
### 2026-09-12
Initial discovery and review.
