# chunklet-py

## Summary
A Python text-splitting library for RAG/LLM pipelines that chunks by natural boundaries (sentence,
paragraph, code structure) rather than raw character counts, with automatic language detection
across 60+ languages, configurable token/line/function limits, chunk overlap, and rich per-chunk
metadata (source, span, document structure).

## Why I Should Care
Text chunking is one of those small jobs every RAG pipeline reinvents, usually badly — splitting
mid-sentence, ignoring language boundaries, losing context between chunks. A focused, tested
library for just this one job removes a recurring, easy-to-get-subtly-wrong piece of plumbing from
any document-ingestion or RAG feature.

## Problems It Can Remove
- Hand-rolled regex/character-count text splitting that cuts sentences mid-way.
- Per-language splitting rules for mixed-language document sets.
- Losing structural context (headings, sections) when chunking long documents.

## Practical Uses
- Replace a hand-rolled splitter in a Python RAG pipeline with one that respects
  sentence/paragraph/code boundaries and tracks source spans for citations.
- Chunk mixed-language document sets without writing per-language rules.
- Chunk source code by function/class boundaries instead of arbitrary line counts before feeding
  it into an LLM context window.

## Product Opportunities
None specific — this is infrastructure plumbing, not a product surface on its own.

## Agent / Automation Opportunities
A reliable, boundary-aware chunker is a small but recurring dependency inside any
document-ingestion or RAG agent tool; worth keeping in mind as a default rather than writing a
custom splitter per project.

## Integration
`pip install chunklet-py`. Python-only — using it from a Node/TS stack means a subprocess or small
microservice call, not a direct import. Low integration effort within a Python service.

## Architecture Notes
Combines natural-boundary detection (sentence/paragraph/code-aware) with configurable hard limits
(tokens/lines/functions) and automatic language detection, rather than treating chunking as a
pure character-count problem.

## Maturity
Emerging but stable-labeled. Created 2025-07-22 (over a year old), v3.0.1, 88 stars, 4 forks, 3
subscribers. CI passing, coverage tracked.

## License
MIT — unrestricted commercial use.

## Alternatives
- **LangChain text splitters** — more features bundled with the rest of LangChain's surface area;
  chunklet-py is a lighter, standalone dependency if LangChain itself isn't already in use.
- **Hand-rolled regex/character-count splitting** — the status quo this directly replaces.

## Risks / Limitations
- Small, early-stage user base (88 stars, 3 subscribers).
- v3 release notes flag breaking changes from v2 — API not yet fully stable.
- Python-only; adds a cross-language integration step for a Node/TS-primary stack.

## Recommendation
PROTOTYPE — small enough to drop into any Python-side document-ingestion script immediately;
today's Small but High-Leverage Utility pick.

## Change History
### 2026-10-24
Discovered. Catalogued at PROTOTYPE, 6.9/10. Today's Small but High-Leverage Utility.
