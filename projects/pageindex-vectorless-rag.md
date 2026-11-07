# PageIndex

## Summary
A reasoning-based document index for RAG that replaces the vector database entirely. Instead of
chunking and embedding, PageIndex builds a hierarchical tree from a document's structure (like a
table of contents) and has an LLM reason over node titles/summaries to select relevant branches,
rather than retrieving by embedding similarity. Ships a local SDK (bring your own LLM key) and a
hosted cloud option, plus a vision-based variant that works over page images without OCR.

## Why I Should Care
Vector similarity retrieval fails on exactly the documents that matter most for professional
use — long financial reports, legal contracts, regulatory filings — because what's semantically
similar to a query often isn't what's actually relevant, and vice versa. PageIndex's pitch is
that reasoning over document structure fixes this class of retrieval failure without a vector DB,
embeddings pipeline, or chunk-size tuning at all. That's a genuinely different approach from
every other RAG infra project in the catalog, worth understanding even if the final answer is
"use vectors for most things, PageIndex for long structured documents."

## Problems It Can Remove
Removes the chunking-strategy and embedding-pipeline design work for long-document RAG, and the
accompanying vector DB operational cost. The hosted Page Search API claims a 97% token-cost cut
on document QA versus sending chunked context to the LLM, by returning only the pages that answer
a question.

## Practical Uses
- A RAG layer for financial reports, contracts, or compliance filings where section-level
  structure (not keyword similarity) determines relevance.
- An alternative to a vector-DB-backed RAG pipeline for a product that's hitting retrieval
  accuracy problems on long documents specifically.
- The vision-based variant for scanned/image-only documents where OCR quality is the bottleneck.

## Product Opportunities
A document-QA feature for long professional documents (contracts, filings) as a differentiated
add-on to a product that currently does naive chunk-and-embed RAG and is seeing poor retrieval
accuracy on its longest documents.

## Agent / Automation Opportunities
Local SDK is usable directly as an agent tool; also available as an MCP server and a hosted API
per the project's developer docs. Includes a ready-made agentic example built on the OpenAI
Agents SDK.

## Integration
`pip`/`npm` SDK, local mode (your own LLM key) or PageIndex Cloud. MIT-licensed, no self-hosting
restriction for the local mode. **Integration effort: Low** for a document-QA proof of concept;
tree-building itself consumes LLM calls and 30–120s of latency per the project's own documentation
and third-party reviews, so it is not a drop-in replacement for low-latency retrieval paths.

## Architecture Notes
"AlphaGo-inspired" framing: treat retrieval as a search-over-a-tree problem solved by reasoning,
not a nearest-neighbor lookup. The headline 98.7% FinanceBench accuracy claim is tied to
VectifyAI's commercial Mafin 2.5 product and is vendor-reported — treat it as unverified until
independently reproduced, per the project's own README framing and third-party coverage.

## Maturity
Mature — created April 2025 (~1.5 years old), 40k+ stars, 3,456 forks, 152 subscribers, active
releases (v0.5.3, 2026-11-05), Trendshift-featured, extensive third-party blog/review coverage
since mid-2026. This is a well-established project within RAG circles, not a hidden gem — the
value here is the architecture, not novelty.

## License
MIT, verified against the repository LICENSE file. No restrictions on commercial use.

## Alternatives
- Standard vector-DB RAG (pgvector, Qdrant, Milvus, the already-catalogued Antfly/Infino) — the
  default this project argues against for long structured documents.
- GraphRAG/LightRAG/HippoRAG (see today's catalogued Unified KG RAG on AWS) — a different
  non-vector approach, graph-structured rather than tree-structured.

## Risks / Limitations
- Tree-building adds LLM cost and latency at indexing time; scales awkwardly to millions of
  small documents (the project's own PageIndex File System add-on exists specifically to address
  this).
- Best-fit claim (long structured documents) is narrower than "RAG" in general — don't reach for
  this as a default retrieval layer for short/unstructured content.
- Core accuracy benchmark is vendor-published and tied to a commercial product; independent
  verification was not found.

## Recommendation
STUDY — the architecture is worth understanding and testing specifically against a long-document
use case; not a wholesale vector-DB replacement, but a legitimate option to keep in mind the next
time chunked RAG underperforms on a structured document.

## Change History
### 2026-11-07
Discovered via GitHub search (slot 4: data, search, documents, RAG). First catalog entry.
