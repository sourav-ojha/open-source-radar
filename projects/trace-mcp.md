# trace-mcp

## Summary
An MCP server plus desktop app that builds a framework-aware dependency graph of a
codebase once, then serves change-impact analysis, code-linked decision memory, and
cross-session recall through MCP tools, so a coding agent stops re-deriving codebase
structure from scratch every turn. Claims 81 languages / 87 framework integrations across
181 tools; also indexes Markdown knowledge vaults (Obsidian/Logseq) as a peer domain.

## Why I Should Care
The stated thesis — that most of a long agent session's token/latency/hallucination cost
comes from repeated re-exploration of the same repo structure, not from task complexity —
matches the actual pain of running coding agents against a non-trivial codebase. If the
headline numbers hold up under hands-on use, this is a direct lever on agent cost and
reliability, which sits squarely on this profile's primary relevance axis.

## Problems It Can Remove
Aims to remove the "80 Grep calls and 190 file reads to answer one dependency question"
pattern that dominates long agent sessions, and the need to re-explain past architectural
decisions every session.

## Practical Uses
- Ask a coding agent "what breaks if I change this model?" and get a precomputed blast
  radius instead of a long chain of exploratory tool calls.
- Recover the reasoning behind a past decision (`query_decisions`) instead of re-deriving
  it from git blame or Slack history.
- Orient a new agent session in roughly 300 tokens (`get_wake_up`) instead of re-reading
  the repo's structure from scratch.
- Index a personal Obsidian/Logseq vault the same way, getting wikilink-aware change-impact
  and backlink search over notes.

## Product Opportunities
None identified directly — this is a developer-tooling play, not a component to embed in a
product.

## Agent / Automation Opportunities
Purpose-built as an MCP server for Claude Code, Codex, Cursor, Windsurf, and Zed. The
`get_change_impact`, `plan_turn`, and `search_sessions` tools are the kind of capability
that would otherwise require a custom retrieval layer built by hand.

## Integration
Low effort on paper: `npm install -g trace-mcp`, then `trace init` and `trace add`. A
desktop app (macOS/Windows) provides a GPU graph explorer over the same index the MCP
server serves.

## Architecture Notes
Builds the dependency graph once and keeps it incrementally fresh rather than
recomputing structure on every agent turn — the graph is the reusable asset, and both the
MCP server and the desktop explorer read from it. Framework-aware edges (e.g.
`Inertia::render()` linking PHP to Vue, `@Injectable()` creating a DI edge) are the
differentiator versus a plain language-server-style graph.

## Maturity
Emerging, effectively a single-author project (created 2026-04-03). 170 stars, 20 forks,
7 open issues, v3.23.2 released 2026-09-07. Default branch is `master`, not `main`.

## License
MIT — no restrictions on commercial use, redistribution, or embedding.

## Alternatives
Plain grep/glob-based agent exploration (the baseline trace-mcp benchmarks itself
against); Sourcegraph and similar code-intelligence platforms (heavier, not agent-native);
text-only decision-memory tools that don't link decisions to the dependency graph.

## Risks / Limitations
Effectively single-author — real bus-factor risk. The headline claim (72.7% fewer input
tokens, comprehension parity, measured over 60 merged PRs in repos the author doesn't own)
is self-reported and self-benchmarked; a published preregistration exists but has not been
independently reproduced by this review. The README's marketing-forward production values
are unusually elaborate for the project's age, which is not itself disqualifying but
warrants a hands-on trial before relying on the tool, rather than taking the numbers at
face value.

## Recommendation
PROTOTYPE — worth a trial on a real repo to sanity-check the token-reduction and
change-impact claims firsthand before depending on it for anything important.

## Change History
### 2026-09-08
Initial discovery and catalog entry.
