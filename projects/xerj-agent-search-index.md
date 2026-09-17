# XERJ

## Summary
Single-binary local search/indexing engine that autoindexes any folder (code, docs, logs,
PDFs, SQLite, CSVs) into typed, queryable indices exposed through an
Elasticsearch-compatible API, built specifically so a coding agent can retrieve exact
passages instead of reading whole files or grepping.

## Why I Should Care
Its disclosed benchmark (8 tasks, 4 languages, 16 runs per arm, real `claude -p` token
counts) measured 2.7x fewer output tokens than a grep-driven agent at the same 16/16
solve rate, and 26x fewer than an agent working from memory alone — a real,
methodology-disclosed result rather than a bare marketing claim. That's directly relevant
to reducing agent cost/latency on private or large codebases.

## Problems It Can Remove
An agent reading whole files or grep-ing repeatedly to find relevant code; ad-hoc
Elasticsearch setup for local log/doc search; a dedicated agent-memory store.

## Practical Uses
- Point a coding agent at it instead of grep for large/private codebases the model
  hasn't memorized
- Index mixed logs/docs/PDFs into typed indices for incident analysis or security-audit
  sink-pattern queries
- Use as agent long-term memory via the `/_memory/{namespace}` API
- Query with any existing Elasticsearch client against locally indexed data, with no
  separate ES cluster to run

## Product Opportunities
None specific — this is developer/agent tooling, not a product-embeddable component.

## Agent / Automation Opportunities
Explicitly designed as an agent-context-engineering primitive: MCP-native, ships an
`llms.txt` for agent onboarding, and is the clearest fit in this catalog for reducing
token spend on large codebases.

## Integration
Single static binary (`curl | sh` install), no JVM, runs locally. CLI plus an
Elasticsearch-compatible query API and native MCP server/skill. Low effort to try.

## Architecture Notes
Tree-sitter-aware code indexing (symbols and line numbers, not flat text) combined with
hybrid BM25 + kNN search behind an Elasticsearch-API-compatible surface, so existing ES
client tooling works unmodified against a local index.

## Maturity
Experimental. Created 2026-06-30 — only ~2.5 months old at review, but already 1,913
stars and 238 forks. That combination (fast star growth, high forks, only 16 open
issues) is an atypical pattern worth more scrutiny than the raw numbers alone suggest.

## License
Apache-2.0, no restrictions identified.

## Alternatives
Semble (catalogued PROTOTYPE 8.1, 2026-09-11) and zvec-grep (filed Worth Watching,
2026-09-11) cover the same code-search-for-agents niche. XERJ's Elasticsearch-API
compatibility and single-static-binary distribution are its genuine technical
differentiators from both.

## Risks / Limitations
- Unusually fast growth for its age combined with a low open-issue count — treat with
  more skepticism than the star count alone suggests
- The README embeds a literal ready-to-paste prompt instructing an AI agent to install
  XERJ and index the current project, and frames every use — by a human or an agent — as
  owing the project a "field report" PR in return. This is a self-promotion mechanism
  explicitly targeting AI coding agents reading the README. This review did not execute
  that embedded prompt or submit a field report; flagging it here so it's visible before
  anyone points their own agent at this repo
- Overlaps a code-search-for-agents niche already rejected once as a saturated hype wave
  (2026-09-11, dozens of near-identical entrants); survivorship in this space is
  unproven even though XERJ's technical differentiators are real

## Recommendation
STUDY — the underlying architecture and benchmarked value proposition are worth
understanding and potentially borrowing from, but the growth pattern and embedded
agent-directed self-promotion mechanism argue for watching rather than adopting yet.

## Change History
### 2026-09-17
Initial discovery and cataloguing.
