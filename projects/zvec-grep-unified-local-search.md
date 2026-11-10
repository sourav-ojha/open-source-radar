# zvec-grep (zg)

## Summary
zvec-grep (`zg`) is a Rust CLI, distributed via npm, that unifies three search modes — ripgrep-equivalent exact/regex search, BM25 keyword search, and vector/semantic search — behind one local-first index and interface, for both humans and coding agents. It searches source code, documents, and structured data while preserving source locations (file/line), and is built on Alibaba's zvec vector engine rather than a from-scratch vector store.

## Why I Should Care
Coding-agent workflows burn tokens re-ranking raw grep output or guessing which embedding call to make next. zg collapses exact-match, lexical, and semantic search into one indexed, ranked result set, with published benchmarks (not marketing copy) showing fewer tool calls and less context spent on agent-driven code QA tasks (SWE-QA-Bench via Claude Code, BrowseComp-Plus via Codex).

## Problems It Can Remove
- Removes the need to glue together ripgrep + a separate embeddings pipeline for hybrid code/doc search.
- Removes an agent's own re-ranking of noisy raw grep results.
- Removes dependency on a hosted search/embeddings API for sensitive codebases (local-first by default).

## Practical Uses
- Searching a large Node/TS monorepo or documentation set by meaning, not just keyword.
- Giving a coding agent (Claude Code, Codex, OpenCode) one hybrid search tool instead of three separate ones.
- Searching mixed source code + Markdown + structured JSON/YAML config in a single query.
- Offline/local search for MSSP-context codebases where sending code to a hosted API is undesirable.

## Product Opportunities
None identified directly — this is a developer/agent tool, not an embeddable product component.

## Agent / Automation Opportunities
CLI-first design makes it immediately scriptable as an agent tool without an MCP wrapper. `zg --install --target opencode --yes` wires it into OpenCode automatically; the project's own benchmarks also run it against Claude Code and Codex, though the exact install flag for those targets isn't confirmed in available documentation.

## Integration
`npm install -g @zvec/zvec-grep`, then `zg --index` on a project root. Index lives in `.zvec-grep/`. No server, no Docker. Integration effort: **Low**.

## Architecture Notes
Hybrid retrieval over three channels (exact/regex, BM25, vector), ranked and returned with source locations, built on Alibaba's zvec engine for the vector layer. Local embeddings model supported (`local/potion-retrieval-32m`); remote embeddings only used with explicit opt-in.

## Maturity
Emerging — created 2026-07-10 (~4 months old), already 4,298 stars / 263 forks / 90 open issues / 14 subscribers. Actively released (v0.3.0, 2026-11-03). The elevated stars:subscribers ratio (~307:1) is a caution flag but is paired with real fork and issue activity rather than the zero-fork/zero-issue pattern used elsewhere to reject suspected fake engagement.

## License
Apache-2.0, confirmed via GitHub API. No restrictions.

## Alternatives
ripgrep (exact-match only), already-catalogued AST-based agent code search tools (CocoIndex Code/ccc), Sourcegraph-style hosted code search (heavier infra, not local-first).

## Risks / Limitations
- Elevated star:subscriber ratio — recheck after a few months.
- Trendshift self-promotion badges in the README should be discounted as marketing, not quality evidence.
- Only 4 months old; long-term maintenance commitment unproven.
- Confirmed agent-CLI integration targets beyond OpenCode are unclear from available documentation.

## Recommendation
USE NOW — low integration effort, real npm package, real benchmarks, directly useful today as a CLI search tool on his Node/TS codebases and as a coding-agent search layer.

## Change History
### 2026-11-10
Initial discovery and review. Slot 7 (experimental projects and hidden gems) run.
