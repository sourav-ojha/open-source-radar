# Codanna

## Summary
Local code intelligence MCP server and CLI for AI coding agents. Indexes a repository
on disk (15 languages, tree-sitter based) and serves symbol search, semantic search,
call graphs, dependency tracking, document RAG, and impact analysis. Its headline
feature is fusing several of these into a single MCP call — one query returns a
symbol's signature, docstring, callers, callees, and blast-radius impact together,
instead of an agent running several separate tool calls.

## Why I Should Care
Directly hits the AI-agent-leverage axis: fewer tool-call round-trips per coding-agent
session on a large repo means less token spend and faster turnaround, while running
fully local with no source code leaving the machine by default.

## Problems It Can Remove
Removes the grep-and-read loop an agent otherwise runs to understand unfamiliar code —
find a symbol, read the file, grep for callers, read those files, and repeat. Codanna
collapses this into one pre-correlated response.

## Practical Uses
- Wire into Claude Code, Cursor, Windsurf, Codex, or Gemini as a persistent MCP server
  for session-long work on a large or unfamiliar codebase.
- Use the one-shot CLI mode (`codanna mcp ...`) inside slash-commands, bash hooks, or
  CI without running a daemon.
- Index project docs alongside code (`codanna documents`) for combined code+doc RAG.
- Use `analyze_impact` before a refactor to see exactly what breaks.

## Product Opportunities
None specific — this is a developer/agent-tooling utility rather than an embeddable
product component.

## Agent / Automation Opportunities
This is fundamentally an MCP server plus CLI purpose-built for coding agents — the
core opportunity *is* the agent integration. Also ships a Claude Code plugin
(`codanna-toolset`) with visual x-ray/graph skills.

## Integration
Install via curl script, Homebrew, Nix, or Cargo. `codanna init && codanna index src`
to build the index, then either run `codanna serve` as a persistent MCP server or call
`codanna mcp <tool>` directly. ~150MB embedding model downloads on first use. Medium
integration effort — more setup than a single npm install, but no external
infrastructure required.

## Architecture Notes
Rust, tree-sitter parsers per language. Claimed 76,000–249,000 symbols/second parse
throughput and sub-10ms warm-server lookups (0.3ms exact match, ~3ms semantic),
published with reproduction commands in their benchmarks doc. Remote OpenAI-compatible
embeddings are opt-in only; default path is fully local.

## Maturity
Emerging but active. v0.16.0, 71 forks, multiple contributors beyond the primary
maintainer (524 of ~600+ commits), CI in place, last push 2026-09-24.

## License
Apache-2.0, with an attribution requirement noted in NOTICE. No restrictions on
commercial or embedded use beyond standard attribution.

## Alternatives
Several similar "code intelligence for AI agents" MCP servers have surfaced across
recent runs (already-catalogued Semble, Repowise, plus multiple new entrants seen but
not catalogued this run — see Rejected/Duplicate Candidates in the 2026-09-25 daily
digest). This is a genuinely saturated category; Codanna's differentiation is breadth
(15 languages), speed, and the fused single-call response shape, but that claim needs
hands-on comparison against the others before treating it as settled.

## Risks / Limitations
- Crowded niche — multiple near-identical tools; pick one only after a real
  side-by-side trial.
- Requires a Rust toolchain to build from source (`pkg-config libssl-dev` on Linux);
  Windows support explicitly experimental.
- ~150MB model download and per-repo indexing step before first use.

## Recommendation
PROTOTYPE — worth a real trial on an active large-ish repo to see whether the fused
MCP responses meaningfully reduce agent tool-call overhead versus what's already in
use, before picking a single code-intelligence-for-agents tool to standardize on.

## Change History
### 2026-09-25
Discovered via GitHub topic search (`topic:code-search`) during slot 3 discovery.
Catalogued as PROTOTYPE, 8.3/10.
