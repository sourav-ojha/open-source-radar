# Semble

## Summary
Semble is a CPU-only, zero-dependency code search library built specifically for
coding agents. It indexes a repository in roughly 500ms and answers natural-
language queries in about 1ms, returning only the relevant snippets rather than
full files — claimed at ~99% fewer tokens than a typical grep-then-read
exploration loop, with retrieval quality (NDCG@10 0.854) reported on par with
code-specialized transformer models.

## Why I Should Care
Every large-repo agent session spends real token budget on exploratory
grep/read cycles before it finds the relevant code. If Semble's benchmark claims
hold up on a real private codebase (not just its published corpus), that's a
direct, measurable reduction in both token cost and time-to-answer for every
agent session working in an unfamiliar or large part of a repo.

## Problems It Can Remove
- The multi-tool grep-then-read exploration loop an agent runs before it finds
  the right code, which burns context and turns.
- Needing an external embeddings API, GPU, or vector database just to get
  semantic code search — this runs entirely on CPU with no API keys.

## Practical Uses
- Install as an MCP server so Claude Code / Cursor / Codex can query a repo in
  natural language instead of grepping blind.
- `semble search "authentication flow" ./my-project` from a script or CI step
  needing a quick answer without spinning up an agent session.
- Index and query a candidate dependency by git URL before cloning it, to scope
  out how it handles something before adopting it.
- `semble find-related` to jump from a known file:line to structurally similar
  code elsewhere in a large, unfamiliar repo.

## Product Opportunities
None identified — this is agent tooling infrastructure, not a product component.

## Agent / Automation Opportunities
This is squarely an agent-infrastructure tool: ships as an MCP server, a CLI
usable via AGENTS.md/CLAUDE.md instructions, and a dedicated installable
sub-agent (`semble-search`), with a one-command installer that auto-detects
which coding agents are present and wires up the integration.

## Integration
`uv tool install semble` then `semble install` (interactive, or scriptable with
`--agent`/`--type`/`--yes` for unattended setup). Integration effort: **Low** —
no API keys, no GPU, no external services; indexes and caches automatically per
project, invalidated on file change.

## Architecture Notes
Worth studying purely as a benchmark story: it claims ~340x faster indexing and
~17x faster querying than a comparable code-specialized transformer approach
while matching its retrieval quality, entirely on CPU. `.gitignore` and
`.sembleignore` (merged, standard gitignore syntax, directory-scoped) control what
gets indexed — a clean, low-friction pattern for scoping large monorepos.

## Maturity
Emerging but fast-growing: created 2026-04-06, already 6,049 stars, latest
tagged release v0.5.6 (2026-09-05, matches the PyPI-published version), only 3
open issues against that star count — suggests an actively triaged, tightly
scoped project rather than one accumulating unaddressed reports.

## License
MIT — no restrictions identified.

## Alternatives
zvec-ai/zvec-grep (Node.js/TypeScript, fuses ripgrep + BM25 + vector search,
also searches docs and structured data, not just code; 3,379 stars, Apache-2.0)
is a legitimate direct alternative with a Node-native stack, worth trying given
a Node/TS-first environment — Semble was picked as the primary recommendation
here for its published, reproducible benchmarks and simpler CPU-only model with
no runtime dependency beyond the interpreter. Already-catalogued mex (living
codebase wiki) and ripwire (deterministic code graph) solve an adjacent but
different problem: persistent structured context, not on-demand low-token
snippet retrieval.

## Risks / Limitations
- Despite the star count, the project itself is young (five months old at review
  time) — worth validating the benchmark claims against a real private repo
  before relying on them.
- Small maintaining team (MinishLab) — bus-factor risk.
- Sits in an increasingly crowded "code search for agents" niche that resurfaces
  new entrants on close to a weekly basis; the space could consolidate.

## Recommendation
PROTOTYPE — install it against one real, large repo and compare its answers and
token usage directly against a normal grep+read agent loop before deciding
whether it becomes a standing part of the agent toolchain.

## Change History
### 2026-09-11
Initial discovery and review. Slot 3 (developer utilities, debugging, testing) run.
