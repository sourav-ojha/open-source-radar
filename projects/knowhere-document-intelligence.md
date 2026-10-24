# Knowhere

## Summary
A document parsing and retrieval system that turns unstructured files — including long or complex
PDFs and PowerPoint decks — into persistent, hierarchy-aware "memory" for AI agents. Dual Vision +
Text parsing tracks converge into one schema with citations back to source pages/sections, and an
MCP-native "agent-native retrieval" mode lets an agent decide how to search, traverse, and cite
rather than running a fixed retrieve-then-generate pipeline.

## Why I Should Care
The dual vision+text parsing design and agent-native (rather than fixed-pipeline) retrieval are a
genuinely more current take on document RAG than most already-catalogued parsers. But the
documented self-hosted path depends on third-party APIs by default, which matters directly for
anything billed as "self-hosted."

## Problems It Can Remove
In principle: hierarchy reconstruction, a unified vision/text document schema, and agent-native
MCP retrieval tools — each a substantial build on its own. In practice, adopting it as a dependency
also means adopting its external API dependencies (see Risks).

## Practical Uses
- Study the dual vision-track/text-track parsing design for documents mixing clean text with
  scanned pages, tables, and diagrams in the same file.
- Reference implementation for "agent-native retrieval" — an agent choosing search/traverse/cite
  actions against a hierarchy-aware index — versus a fixed retrieve-then-generate pipeline.
- Evaluate for ultra-long PDF/atlas-style document ingestion (hundreds of pages) only if the
  external API dependencies below are acceptable for the use case.

## Product Opportunities
None identified that don't route through the hosted dependencies below; the ideas are more useful
to study than to embed directly today.

## Agent / Automation Opportunities
MCP-native retrieval tools exposing hierarchy-aware navigation (not just flat chunk search) is a
pattern worth copying for any internal document-QA agent tool, independent of whether Knowhere
itself is adopted.

## Integration
Core repo is Apache-2.0. The companion `knowhere-self-hosted` repo is a Docker Compose wrapper
requiring a MinerU API key for PDF parsing and a DeepSeek or Alibaba Cloud DashScope LLM key by
default. A fully local, no-external-API setup would require supplying a custom parser that outputs
Markdown and a local LLM — the README allows for this but does not document it end-to-end.
Integration effort: high, given the setup and dependency questions involved.

## Architecture Notes
"Knowhere 2.0" separates a Vision track (pages understood directly by frontier vision models) from
a Text track (precise extracted structure for text-native documents), converging both into the
same memory schema, hierarchy, retrieval engine, and citation model. The retrieval layer exposes a
unified schema and hierarchy-aware tools so an agent decides how to search/traverse/read/cite,
rather than following one fixed pipeline — the same foundation reportedly powers both built-in
retrieval and external MCP-connected agents.

## Maturity
Emerging. Created 2026-04-30, v1.2.30, 3,756 stars, 338 forks, 9 contributors. Company-backed
(knowhereto.ai), with an open-core go-to-market: the README prominently offers managed-cloud free
credits alongside the self-hosted path.

## License
Core repo: Apache-2.0. Self-hosted deployment repo: no separate license file (thin wrapper, not a
distinct codebase). Flagged per AGENT.md: always verify current license status before relying on
this for a commercial product.

## Alternatives
- **OpenDataLoader PDF, Dedoc, unpdf/undoc** (already catalogued) — fully self-contained
  document-parsing tools with no mandatory third-party API calls.
- **LlamaParse/LlamaCloud, Mindee, Unstructured** — comparable managed document-intelligence APIs,
  all similarly cloud-first.

## Risks / Limitations
- Functionally depends on proprietary infrastructure (MinerU API, DeepSeek/Alibaba LLM API) in its
  documented self-hosted path — the kind of signal AGENT.md explicitly flags for "open-source"
  products unusable without external services.
- stars:subscribers ratio is 3,756:84 (44:1) — elevated but not as extreme as prior flagged fraud
  cases; 338 forks and 9 contributors support real engagement rather than inflation.
- Managed-cloud free-credits offer is prominent in the README, consistent with an open-core
  go-to-market rather than a community-first OSS project.

## Recommendation
STUDY — the parsing/retrieval architecture is worth reading closely; direct adoption is blocked
today by the third-party API dependency unless a fully local parser/LLM substitution is proven
out first.

## Change History
### 2026-10-24
Discovered. Catalogued at STUDY, 6.8/10.
