# haiku.rag

## Summary
A local-first agentic RAG library, CLI, and MCP server. Hybrid vector+full-text search with
reciprocal rank fusion, multimodal/cross-modal embeddings, local or hosted reranking,
vision QA, a sandboxed-code-execution "analyze" capability, and a production ingester
service (persistent queue, retries, dead-letter queue, FS/HTTP/S3/WebDAV sources) — all on
an embedded LanceDB with no server required.

## Why I Should Care
It's one of the few local-RAG projects that goes past the ingest-and-ask demo into real
production concerns: a persistent-queue ingester with retries and a dead-letter queue, an
MCP server for direct use from Claude Desktop/Claude Code, and structure-aware chunking via
Docling that preserves page numbers and section headings for real citations.

## Problems It Can Remove
Removes the need to hand-roll hybrid search, cross-modal embeddings, and a production
document-intake pipeline separately — this bundles all three with tests and documentation.

## Practical Uses
- Citation-backed Q&A over a folder of PDFs/docs for internal knowledge retrieval
- MCP server exposing document search/QA/analyze tools directly to Claude Desktop or Claude Code
- Production ingester as the intake layer for a document pipeline that needs retries and a dead-letter queue, not a cron job hitting a folder
- "Analyze" capability runs sandboxed Python for cross-document aggregation questions without a separate BI tool

## Product Opportunities
The MCP server plus production-ingester combination is closer to real infrastructure than
most local-RAG demos — plausible backend for a small internal "ask our docs" tool without
owning the retrieval plumbing.

## Agent / Automation Opportunities
Ships an MCP server out of the box (`haiku-rag mcp --stdio`) exposing document management,
search, QA, and analysis tools to any MCP client.

## Integration
`pip install haiku.rag` (or the dependency-light `haiku.rag-slim`). Requires an embedding
provider (Ollama, OpenAI, VoyageAI, Cohere, LM Studio, vLLM) configured before first use.
Medium effort — Python-only, so it integrates into a Node/TS stack via subprocess, MCP, or
a REST wrapper rather than as a native library.

## Architecture Notes
Built on LanceDB (embedded vector store, with S3/GCS/Azure/LanceDB Cloud options), Pydantic
AI (QA across any supported model), and Docling (document structure parsing). The
production ingester runs as a long-lived `haiku-ingester` service with its own persistent
SQLite queue and FastAPI control plane, separate from the core library.

## Maturity
Mature. Active since October 2024, 588 stars, 11 contributors, CI + codecov, docs site,
PyPI package. 15 open issues at review time is normal for a project at this activity level.

## License
MIT. No restrictions on commercial use, redistribution, or embedding.

## Alternatives
LlamaIndex/LangChain hand-rolled RAG stacks, Danswer/Onyx, txtai, and OpenNotebookLM
(catalogued the same day — webapp-first vs. haiku.rag's library/CLI-first design).

## Risks / Limitations
- Python-only — no native fit for a Node/TS stack beyond subprocess/MCP/REST
- Requires an embedding provider configured before first use; not zero-config
- 15 open issues at review time, typical for the project's activity level, not itself a red flag

## Recommendation
PROTOTYPE — worth testing as an MCP-exposed document knowledge base or as the ingester
behind a small internal docs tool, evaluated against the operational cost of running a
Python service alongside a primarily Node stack.

## Change History
### 2026-09-05
Discovered via GitHub search (rotation slot 4: data, search, documents, RAG). Catalogued
at PROTOTYPE, 8.3/10.
