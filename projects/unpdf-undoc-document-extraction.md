# unpdf / undoc

## Summary
Sibling Rust libraries (MIT, from the `iyulab` org) for high-performance document-to-Markdown/JSON extraction: unpdf for PDF (CJK/RTL support, multi-column layout detection via a recursive XY-Cut algorithm, form-field extraction, streaming page-by-page parsing, image deduplication), undoc for DOCX/XLSX/PPTX. Both publish native Rust crates plus Python, .NET, and npm/WASM bindings usable directly from Node.js or a browser with no server-side process required.

## Why I Should Care
A document-to-Markdown extraction step that runs as a native npm dependency — including a WASM build that works in-browser — removes the usual need to stand up a separate Python extraction microservice in an otherwise all-JS/TS stack.

## Problems It Can Remove
Removes the need to hand-roll a PDF/Office parsing pipeline or bridge to a Python service just to turn a document into clean text/Markdown for an LLM context window.

## Practical Uses
- Extract PDF/DOCX/XLSX/PPTX to Markdown directly inside a Node.js backend for a RAG ingestion pipeline, with no separate Python microservice.
- Run the WASM build client-side in a browser for instant document preview/conversion with no upload to a server.
- Prepare CJK/RTL-heavy documents (Korean/Chinese/Japanese/Arabic/Hebrew) for LLM context, where many PDF extractors mangle spacing or reading order.
- Bulk-convert a document archive to Markdown for a knowledge-base product, using the streaming API to bound memory on very large files.

## Product Opportunities
Natural ingestion layer for a document-processing or knowledge-base micro-SaaS aimed at non-English-heavy markets, where CJK support is a real differentiator. The WASM/browser path needing no backend at all could back a lightweight "convert any doc to clean Markdown" standalone utility product.

## Agent / Automation Opportunities
npm-installable and runs fully in-process — easy to wrap as an MCP tool or agent-callable function without a sidecar service. CLI output (Markdown/JSON) is well-suited as a coding-agent tool call for reading arbitrary documents handed to it.

## Integration
`npm install @iyulab/unpdf` (or `@iyulab/undoc`), `cargo install unpdf-cli`, or a pre-built binary per platform. Integration effort: **Low**.

## Architecture Notes
Streaming pipeline (`PdfParser::for_each_page`) yields pages as they parse, bounding peak memory regardless of document size, with an internal reorder buffer that guarantees deterministic page ordering even under parallel (Rayon) page parsing. Identical images across pages are deduplicated on disk rather than re-written per page.

## Maturity
Emerging — unpdf created January 2026, undoc created December 2025, both actively released (npm: unpdf 0.23.0, undoc 0.13.1, both published 2026-10-01). Modest adoption so far (58 and 36 stars).

## License
MIT on both repos, confirmed via GitHub metadata and badges. No restrictions on commercial use or embedding.

## Alternatives
- Xberg / Kreuzberg (catalogued, USE NOW) — far broader (107 formats, OCR, transcription, embeddings) but heavier and more complex; unpdf/undoc are narrower and lighter for the PDF/Office-only case.
- yfedoseev/pdf_oxide + office_oxide — same narrow-and-fast positioning, more language bindings (19-20) and a published benchmark suite; a close direct competitor worth comparing before committing.
- landing-ai/ade-cli — an Apache-2.0 CLI wrapping a hosted, credit-metered commercial API; not usable offline (rejected today as a thin wrapper, see the daily digest's Rejected section).

## Risks / Limitations
- Very young with modest adoption so far — verify extraction quality against real document samples before depending on it.
- Single-maintainer-presenting org (iyulab); check for sustained maintenance over a longer window before deep production reliance.
- No OCR — these only extract embedded text/structure, so scanned/image-only PDFs still need a separate OCR step (Xberg or Dedoc cover that case).

## Recommendation
**PROTOTYPE** (score 8.1/10) — a genuine hidden-gem fit for an all-JS/TS stack (direct npm/WASM embedding, no Python sidecar), but new enough to validate against real documents before relying on it.

## Change History
### 2026-10-03
First discovered and reviewed. Verified via GitHub API and npm registry: both MIT; unpdf 58★ (created 2026-01-27), undoc 36★ (created 2025-12-20); npm versions unpdf@0.23.0 / undoc@0.13.1 (published 2026-10-01). Surfaced from a topic:document-extraction search filtered to recently-pushed repos.
