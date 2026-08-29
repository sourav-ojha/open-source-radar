# html-to-markdown (Kreuzberg)

## Summary
High-performance, CommonMark-compliant HTML-to-Markdown converter with a Rust core, shipped as native bindings for Python, Node.js, Java, Go, and WASM. Maintained by the Kreuzberg team, whose broader product is a document-intelligence engine extracting structured data from 98+ formats with built-in OCR.

## Why I Should Care
Every ingestion pipeline that touches web content eventually needs an HTML→Markdown step; this one is faster, more spec-compliant, and available natively in Node without a subprocess or WASM workaround most alternatives require.

## Problems It Can Remove
CommonMark compliance and edge cases (nested tables, code blocks, malformed HTML) are a multi-day correctness problem this project has already solved across 6 language runtimes.

## Practical Uses
- Clean any scraped or fetched web page down to Markdown for an LLM context window
- Normalize customer-uploaded HTML reports into Markdown for storage/search in OffenFlow
- Feed a RAG pipeline's ingestion step without hand-rolling a converter
- Drop-in replacement for a bespoke regex-based HTML stripper

## Product Opportunities
- Embed as the HTML-normalization step in any document ingestion pipeline
- Pair with the Kreuzberg document-intelligence engine for a full ingestion stack if OCR/98-format support is ever needed

## Agent / Automation Opportunities
- Trivial to wrap as an MCP tool (html_to_markdown) for any agent that fetches web content
- CLI usable directly in a coding-agent's tool loop

## Integration
Deployment: library, npm package, pip package, cargo crate, maven, wasm
Interfaces: library (Node.js), library (Python), library (Rust), library (Java), library (Go), WASM
Integration effort: **Low**

## Maturity
Mature. GitHub API verified: MIT, 860 stars, pushed_at 2026-08-29 (same day as this review — actively maintained).

## License
MIT. MIT, no restrictions on commercial or SaaS use, redistribution, or embedding.

## Alternatives
- supermemoryai/markdowner (abandoned, no commits since mid-2024 despite 1,999 stars)
- Digidai/website2markdown (narrower scope, Cloudflare-Worker-hosted service rather than embeddable library)
- turndown (npm, unmaintained pace by comparison)

## Risks / Limitations
- Young org (xberg-io) — verify long-term maintenance commitment before deep production dependency
- Native bindings mean a compiled dependency in the build chain, not pure JS

## Recommendation
**USE NOW** (score 8.3/10) — see Why I Should Care above.

## Change History
### 2026-08-29
First discovered and reviewed. Verified via GitHub API: license MIT, current version v3.11.6 (2026-08-28).
