# Xberg

## Summary
Polyglot document-intelligence engine with a Rust core: text/table/metadata extraction across 107 formats, OCR (Tesseract/PaddleOCR/Candle/VLM), audio/video transcription, layout+table reconstruction, code intelligence for 371 languages, embeddings, and schema-driven structured extraction. Runs as a library (15 language bindings), CLI, REST API, or MCP server. The successor to the Kreuzberg engine already partially tracked here via its html-to-markdown component.

## Why I Should Care
Consolidates format detection, OCR, layout/table reconstruction, transcription, embeddings, and structured extraction into one engine callable natively from Node.js or Python, removing the multi-library pipeline (pdf parser + OCR engine + table extractor + chunker) most RAG ingestion code assembles by hand.

## Problems It Can Remove
OCR backend fallback chains, layout-model table reconstruction, and 107-format MIME detection are each multi-week correctness problems on their own; this project has already solved all of them with published benchmarks and CI coverage across 15 runtimes.

## Practical Uses
- Single ingestion step for a RAG pipeline that has to handle PDFs, Office docs, images, email, and e-books without stitching together separate libraries
- OCR + layout reconstruction for scanned documents dropped into a document-management feature
- Code-intelligence extraction (functions/classes/symbols across 371 languages) for syntax-aware RAG chunking over a codebase
- Structured JSON extraction from arbitrary documents via a schema, without hand-written prompts
- Drop-in MCP server so a coding agent can read and query any document type a user hands it

## Product Opportunities
- Backbone ingestion engine for a document-processing or knowledge-base micro-SaaS product
- Self-hosted alternative to paid document-intelligence APIs (Unstructured, LlamaParse-style services) for a product's ingestion layer

## Agent / Automation Opportunities
- Ships an official MCP server out of the box — no wrapping needed
- CLI usable directly inside a coding-agent tool loop for reading arbitrary files
- REST API mode (`xberg serve`) for a shared ingestion microservice multiple agents/services call

## Integration
Deployment: library, npm/pip/cargo/maven/nuget packages, Docker, Helm chart, WASM
Interfaces: library (Node.js, Python, Rust, Go, Java, C#, +9 more), REST API, CLI, MCP server
Integration effort: **Low**

## Architecture Notes
One Rust core with FFI-based bindings for 15 languages, rather than per-language reimplementations. Feature-gated at the Cargo level (`url-ingestion`, `transcription`, `reranker`, layout/ORT), so a from-source build only pulls in what's used — prebuilt packages bundle a common default set.

## Maturity
Mature engine under a young rebrand. GitHub API verified: MIT, 9,325 stars, pushed_at 2026-09-19 (actively maintained), created 2025-01-31. Multi-contributor (top contributor 8,700 commits, but 4+ other regular contributors incl. dependabot). Real CI (Rust CI badge), Codecov coverage tracking.

## License
MIT. Verified against LICENSE file (Kreuzberg, Inc.). No restrictions on commercial use, SaaS deployment, redistribution, or embedding.

## Alternatives
- yfedoseev/pdf_oxide + office_oxide (1,035★/129★, MIT/Apache-2.0) — much narrower (PDF/Office extraction only) but faster for that narrow task (0.8ms mean, published benchmark suite) and has more language bindings (19-20) if raw extraction speed matters more than breadth
- firecrawl/pdf-inspector (19,226★, MIT) — Firecrawl's narrow PDF classifier/extractor (text-based vs scanned routing in ~10-50ms), useful as a lightweight pre-filter but not a full document-intelligence engine
- docling-project/docling (established, not evaluated in depth this run) — Python-first, IBM-backed, similar scope but no first-class Node/Rust bindings
- opendataloader-project/opendataloader-pdf (catalogued, USE NOW) — narrower PDF-only accessibility-focused parser

## Risks / Limitations
- Young rebrand (Kreuzberg → Xberg, v1 line since ~Sept 2026) — verify the migration is stable before deep production dependency, even though the underlying engine has a longer history
- Very broad feature surface (OCR, transcription, embeddings, code intelligence) increases the audit surface versus a narrower single-purpose library
- Optional features (url-ingestion, transcription, reranker) are Cargo feature flags — prebuilt packages bundle a common subset, so confirm the flags needed are actually included before relying on them

## Recommendation
**USE NOW** (score 8.8/10) — see Why I Should Care above.

## Change History
### 2026-09-19
First discovered and reviewed. Verified via GitHub API and LICENSE file: MIT, current version v1.2.5 (2026-09-18), pypi/npm versions cross-checked. README confirms this is the successor to Kreuzberg (already partially tracked as kreuzberg-html-to-markdown, a narrower sibling repo for just HTML→Markdown).
