# OpenDocuments

## Summary
Self-hosted RAG platform in Node.js/TypeScript that indexes GitHub, Notion, Google Drive, Confluence, S3, local files, and general web sources into one searchable, citation-backed corpus.

## Why I Should Care
It is the rare RAG project that matches the target stack (Node/TypeScript) exactly, so there's no cross-runtime tax to adopt it, and multi-source + citations are both usually the hardest 60% to build correctly.

## Problems It Can Remove
Source-connector plumbing (Notion API, Drive API, Confluence API, S3, GitHub) times five, plus citation tracking through a RAG pipeline, is easily 1-2 weeks of integration work this replaces outright.

## Practical Uses
- Internal 'ask our docs' search across Notion + GitHub + Drive without building a custom ingestion pipeline
- Backbone for a customer-facing knowledge-search feature in a product, since citations are built in
- Direct fit for the MERN/Node stack — no cross-language integration cost

## Product Opportunities
- Could become the search/knowledge layer inside a product without writing a RAG pipeline from scratch
- Citations-by-default matters if this ever faces an end customer who needs to trust the answer

## Agent / Automation Opportunities
- Natural fit as an MCP server exposing 'search my docs' to a coding agent or support agent
- Could back a support-agent tool call directly

## Integration
Deployment: self-hosted, npm package
Interfaces: library (Node.js/TypeScript), npm CLI
Integration effort: **Low**

## Maturity
Emerging. GitHub API verified: MIT, 110 stars, pushed_at 2026-07-26. Release/commit gap flagged as a risk, not fabricated as current.

## License
MIT. MIT — unrestricted commercial and SaaS use.

## Alternatives
- RAGFlow (larger, more established, Python-based)
- Danswer/Onyx (larger, more operational overhead)
- hand-rolled LangChain RAG pipeline

## Risks / Limitations
- Latest tagged release (v0.3.0, April) trails the last commit (July) by 3 months — verify what has changed on main before depending on the tagged version
- 110 stars, single-maintainer-looking project — bus-factor risk

## Recommendation
**PROTOTYPE** (score 7.8/10) — see Why I Should Care above.

## Change History
### 2026-08-29
First discovered and reviewed. Verified via GitHub API: license MIT, current version v0.3.0 (2026-04-22).
