# LiteParse

## Summary
A standalone, local-only PDF parsing library from the official LlamaIndex org, focused
exclusively on fast and light spatial text extraction (PDFium-based, ~2-5ms/page) with
bounding boxes, a pluggable OCR system (bundled Tesseract or any HTTP OCR server),
complexity detection to cheaply flag documents that need heavier processing, and
page-screenshot generation for LLM agents. No proprietary LLM features or cloud
dependency in the open-source core; explicitly hands off to the paid LlamaParse cloud
product only for documents it isn't built to handle well.

## Why I Should Care
The document-processing niche is crowded, but most entrants either chase RAG-chunking
features or are solo-maintainer projects. LiteParse is backed by the official run-llama
org, scoped honestly (fast local parsing, not a RAG framework), and ships real bindings
across four language ecosystems from one codebase.

## Problems It Can Remove
Removes the need to pay for cloud document parsing on the simple majority of PDFs, by
giving a fast local pre-filter with complexity detection that only escalates the genuinely
hard documents.

## Practical Uses
- Fast first-pass PDF text extraction in a document-ingestion pipeline, with complexity
  detection routing only hard documents (dense tables, scans, handwriting) to a heavier
  parser or the paid LlamaParse cloud.
- Generate page screenshots for an LLM agent to visually verify extracted text against a
  PDF, similar in spirit to the catalogued pdf-mcp's evidence-checking loop.
- Plug in an existing OCR server (EasyOCR, PaddleOCR) via its standard HTTP OCR API
  instead of being locked into one OCR engine.

## Product Opportunities
Cheap, local pre-filter stage ahead of a paid document-parsing API, to avoid paying
cloud-parsing costs on the simple majority of documents.

## Agent / Automation Opportunities
No MCP server; a library. Page-screenshot generation is directly useful as an input to an
agent's own verification loop, but the agent-facing wiring would need to be built.

## Integration
Deployment: library via crates.io, npm (native and WASM, `@llamaindex/liteparse*`), or
PyPI. Integration effort: **Low**.

## Architecture Notes
PDFium for spatial text parsing (not a from-scratch PDF parser), with a standard OCR API
specification that any HTTP OCR server can implement — a cleaner separation of concerns
than tools that hard-code one OCR engine. Complexity detection runs before a full parse,
letting a pipeline reject or re-route cheaply.

## Maturity
Emerging but well-backed. Created 2026-02-09 (~9 months), 13,043 stars, 900 forks, 37
subscribers. CI configured, multi-ecosystem package releases (latest node-v2.19.0,
2026-11-13). Not archived. From the official run-llama (LlamaIndex) GitHub org.

## License
Apache-2.0, no restrictions.

## Alternatives
- OpenDataLoader PDF / Dedoc / oxidize-pdf / unpdf (all catalogued) — broader
  structure-aware or RAG-chunking focus; LiteParse is narrower and explicitly optimizes
  for raw parsing speed over semantic structure.
- LlamaParse (same org's paid cloud product) — the officially recommended escalation path
  for documents LiteParse can't handle well.
- unstructured.io / docling — heavier, more ML-feature-complete, slower.

## Risks / Limitations
- 13,043 stars vs 37 subscribers (352:1) is an elevated ratio, though the 900 forks and
  official LlamaIndex org backing look like organic adoption rather than inflation.
- 9 months old — young relative to its star count; the project explicitly pushes hard
  documents to its paid cloud product, so verify the free local tier covers your actual
  document mix before committing to it.

## Recommendation
PROTOTYPE — benchmark it against a representative sample of real documents (not just
clean PDFs) to see how often the complexity detector escalates, before deciding whether it
replaces an existing parsing step.

## Change History
### 2026-11-14
Initial discovery and review. GitHub API verified: Apache-2.0, 13,043 stars, 900 forks, 37
subscribers, created 2026-02-09, pushed_at 2026-11-13, not archived.
