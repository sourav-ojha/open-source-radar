# pdf-mcp

## Summary
An MCP server giving coding agents agentic RAG over one PDF or a whole folder of PDFs:
hybrid BM25+semantic search, selective page reads instead of ingesting whole documents,
table/chart/scanned-text extraction across multi-column and CJK layouts, and page-level
evidence so the agent can render a page and check extracted text against what's actually
printed. Hidden text, including prompt-injection attempts, is flagged rather than silently
passed through.

## Why I Should Care
Most document-RAG tooling optimizes purely for recall and treats a PDF's text as trusted
input. pdf-mcp treats it as untrusted: hidden text (including injected instructions aimed
at the agent reading it) is surfaced, not silently fed into context. Combined with
selective page reads, this is a genuinely safer and cheaper default for any coding-agent
workflow that processes documents from outside sources.

## Problems It Can Remove
Removes the need to dump a whole PDF into an agent's context window (and pay the token
cost) just to answer one question about it. Removes the blind trust most document-RAG
pipelines place in extracted text, by giving the agent a way to visually verify a page
against the raw extraction.

## Practical Uses
- Let Claude Code or another coding agent search and read one PDF or a folder of PDFs
  without ingesting every page into context.
- Check an agent's extracted-text answer against the actual rendered page before trusting
  it for anything compliance- or finance-adjacent.
- Flag PDFs carrying hidden or injected text before an agent acts on them.
- Warm and search a whole folder of PDFs (contracts, reports, scanned forms) as one corpus
  via hybrid BM25+semantic search.

## Product Opportunities
Usable as the ingestion/retrieval layer for a "chat with your documents" feature where
some source PDFs are untrusted uploads from customers or third parties.

## Agent / Automation Opportunities
Native MCP server (`mcp-name: io.github.jztan/pdf-mcp`) — install and use as-is inside
Claude Code or any other MCP client with no custom glue required.

## Integration
Deployment: `pip install pdf-mcp` (PyPI), runs as an MCP server over stdio; also has a
100%-client-side browser demo with no install. Integration effort: **Low**.

## Architecture Notes
Length-sorted small-batch CPU encoding for warming a folder's index, with optional CUDA
embedding for one-to-two-order-of-magnitude speedup. Page-screenshot rendering gives the
agent (or a human) a way to verify text extraction visually rather than trusting OCR/text
extraction blindly — worth studying as a pattern for any pipeline that extracts text from
untrusted documents.

## Maturity
Emerging. Created 2026-01-28 (~10 months), 171 stars, 17 forks, 2 subscribers. PyPI
package, CI, codecov, active development (latest release v3.8.0, 2026-11-14). Not archived.

## License
MIT, no restrictions.

## Alternatives
- unpdf/undoc (catalogued) — document extraction library, no MCP server or page-evidence
  verification loop.
- Dedoc / oxidize-pdf (catalogued) — parsing-focused, not agent-facing retrieval with an
  evidence-check workflow.
- Manually dumping a PDF to text and pasting into context — no page-level search, no
  injected-text detection, burns far more tokens on large documents.

## Risks / Limitations
- 171 stars / 2 subscribers / 17 forks — thin community for a 10-month-old project;
  verify behavior on your own document set before relying on the injection-flagging as a
  security control.
- Python-only server; a Node/TS-native product would still need to shell out to or proxy
  this rather than import it directly.

## Recommendation
PROTOTYPE — wire it into a real Claude Code session against a folder of real PDFs
(including at least one with embedded/hidden text) and confirm the injection-flagging and
page-evidence loop behave as advertised before relying on either.

## Change History
### 2026-11-14
Initial discovery and review. GitHub API verified: MIT, 171 stars, 17 forks, 2
subscribers, created 2026-01-28, pushed_at 2026-11-14, not archived. Default branch is
`develop`, not `main`.
