# oxidize-pdf

## Summary
A pure-Rust PDF toolkit — no C bindings, no native crypto library — covering generation, parsing,
modification, extraction, and validation (including PDF/A compliance and digital-signature
verification). A built-in `rag_chunks()` method returns structure-aware chunks (text, page
numbers, bounding boxes, element types, heading context, token estimates) ready for a RAG
ingestion pipeline.

## Why I Should Care
PDF handling usually means juggling separate libraries for generation, parsing, and validation,
several of which pull in native C dependencies that complicate Docker images and cross-platform
builds. oxidize-pdf covers all of that in one dependency-free crate, and its RAG-chunk output
removes the need to hand-build chunk metadata separately for a document-ingestion pipeline.

## Problems It Can Remove
- Juggling multiple PDF libraries for generate/parse/modify/extract/validate.
- Native C-dependency friction in container builds (no C toolchain needed in the image).
- Hand-building chunk metadata (headings, bounding boxes, token counts) after parsing with a
  library that only returns raw text.

## Practical Uses
- Swap out a native-dependency PDF library (pdfium, poppler, LibreOffice headless) where
  deployment simplicity — a single static binary, no C toolchain in the image — matters more than
  marginal feature parity.
- Generate structure-aware RAG chunks directly from parsing instead of parsing and chunking
  separately.
- Run as a small sidecar HTTP/CLI service from a Node/Express backend when PDF work is needed but
  the rest of the stack stays JS/TS.
- Validate PDF/A compliance or verify digital signatures without shelling out to a Java-based
  validator.

## Product Opportunities
A no-ML, deterministic PDF-to-RAG-chunk pipeline is attractive as an embeddable component inside a
document-ingestion product where predictable, auditable output matters more than squeezing out
marginal accuracy from an LLM-based parser.

## Agent / Automation Opportunities
Could be wrapped as a small MCP server or CLI tool for an agent that needs to read, generate, or
chunk PDFs without pulling in a heavyweight Python/C PDF stack.

## Integration
`cargo add oxidize-pdf`. No official Node/WASM bindings — using it from a JS/TS stack means
running it as a Rust binary or small sidecar service rather than a direct `npm install`. Medium
integration effort for a MERN stack for that reason; low if a Rust service is acceptable.

## Architecture Notes
Explicit scope discipline: page rasterization (rendering to a bitmap) is deliberately out of
scope — a separate renderer like Pdfium is expected for that. The project maintains a
`docs/CLAIMS.md` "claims inventory" recording the source, version, verification evidence, and
limits for its PDF/A, signature, and enterprise-scope statements — a notably rigorous practice for
a small project, and worth checking directly before relying on any specific compliance claim.

## Maturity
Emerging, but with a longer track record than most new entries here. Created 2025-07-07 (over a
year old), v5.5.1, 7,993 tests, 192 stars, 27 forks, 12 contributors.

## License
MIT — unrestricted commercial use.

## Alternatives
- **pdf-parse / pdf.js** (Node) — mature JS-native options, text extraction only, no
  structure-aware chunking or PDF/A validation.
- **PyMuPDF / pdfplumber** — stronger parsing fidelity in some cases, but require a Python process
  and native (C) dependencies.
- **unpdf/undoc** (already catalogued) — also Rust-native with npm/WASM bindings; oxidize-pdf's
  differentiator is covering generation and PDF/A/signature validation as well, not just
  extraction.

## Risks / Limitations
- No Node/WASM bindings yet, unlike unpdf/undoc.
- Page rasterization is explicitly unsupported.
- Effectively solo-maintainer-led (12 contributors, 192 stars) — verify any critical-path PDF/A or
  signature claim against the project's own claims inventory before relying on it for compliance
  use.

## Recommendation
PROTOTYPE — worth trying as a sidecar for a document-ingestion pipeline that needs
structure-aware PDF chunks, with the Node-bindings gap as the main practical blocker to adopt it
more directly.

## Change History
### 2026-10-24
Discovered. Catalogued at PROTOTYPE, 7.6/10.
