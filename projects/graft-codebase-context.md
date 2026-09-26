# Graft

## Summary
Graft builds a codebase's architecture into a folder of linked Markdown "nodes" — plain-English explanations of what each subsystem does plus the exact lines that carry the logic — instead of embeddings or a vector index. It wires into Claude Code, Cursor, Codex, and Gemini via CLI and MCP server, and re-derives the graph against the working tree (including uncommitted edits) before every query.

## Why I Should Care
Directly hits the AI-agent-leverage axis with unusually rigorous evidence for a crowded niche: a 50-task SWE-bench Verified run shows 33/50 resolved with Graft wired in versus 27/50 cold, alongside 25% fewer tool calls and 23% fewer tokens — with the full harness and task list published, not just headline numbers. No server or database to run; the graph is git-diffable Markdown a human can read and correct directly.

## Problems It Can Remove
Removes the per-session "re-explore the repo from zero" cost every coding agent pays: grep a term, open a file, follow an import, back out, try again. That rediscovery is measured here to be roughly half of a task's tool calls and tokens on a cold run.

## Practical Uses
- Wire into a large NestJS/React repo so Claude Code stops re-discovering the same architecture every session.
- `graft blast --base origin/main --format markdown` as a CI step that comments blast-radius on a PR, with git-history-derived owners.
- `graft map` for fast orientation when picking up an unfamiliar service in a microservice fleet.
- Structural-only mode (`graft build`, no `--deep`) needs no API key or model call — usable without spending LLM budget at all.
- Works across a folder of separate repos or a monorepo (pnpm/Go/Cargo) without one sub-project drowning another in rankings.

## Product Opportunities
Could be wired into the coding-agent workflow for the MSSP admin-portal SaaS or PTaaS codebase to cut measurable agent tool-call/token spend on day-to-day feature work.

## Agent / Automation Opportunities
This is itself the agent integration: an MCP server (`graft_find_code`, `graft_file_api`, `graft_trace_calls`, `graft_find_all`, `graft_repo_map`, `graft_check_freshness`) plus a deep Claude Code integration — live statusline, post-edit hooks that print blast radius inline, and automatic background graph resync after a turn that touched code.

## Integration
`npm install -g @nanonets/graft` (or `npx`), then `graft init` to wire the coding agents you use and build the graph. No daemon, no database — `graft/` is a regenerable local cache like `node_modules`; what gets committed is the small wiring `init` drops into `.claude/`/`AGENTS.md`. Effort: Low.

## Architecture Notes
Two-pass build: Tier 1 is deterministic tree-sitter (23 languages, no model, no key) producing a per-symbol wiring graph; an optional `--deep` pass adds an LLM-written one-line summary and "crux" (the handful of lines that actually carry the logic) per node, cached by content hash so re-builds only touch what changed. Every query re-checks the working tree against the last build's fingerprint (~3ms) before answering, so results reflect uncommitted edits without a stale-index problem.

## Maturity
Emerging. Created July 2026 (~12 weeks old at review), actively developed (pushed same-day), npm package at v0.20.0, no GitHub release tags yet. Backed by NanoNets, a real funded document-AI company, under the Trail product brand — not a solo weekend project despite the youth.

## License
MIT. No commercial-use restrictions.

## Alternatives
- Codanna (already catalogued) — MCP-only, embedding/vector-based symbol and call-graph fusion, no Claude Code hook integration.
- mex, Repowise, Semble (already evaluated) — vector- or graph-DB-backed code search requiring an index server.
- Plain grep/LSP navigation — free but pays the full re-exploration cost every session, which is exactly what Graft's benchmark targets.

## Risks / Limitations
- Young, company-backed but unproven over a long timeframe; benchmarks are vendor-produced (methodology is published in detail, but not third-party reproduced).
- `--deep` mode calls an external LLM under Sourav's own API key — normal cost/data-exposure tradeoffs of any LLM-extraction step apply.
- Anonymous usage telemetry is on by default (buckets and fixed labels only, no code/paths/queries per `TELEMETRY.md`); opt out via `graft telemetry disable` or `DO_NOT_TRACK=1`.
- Occupies an already-saturated "codebase context for agents" niche (Codanna, mex, Repowise, and a wave of smaller rejected duplicates) — needs a hands-on trial to confirm the benchmark gains hold on Sourav's own repos.

## Recommendation
PROTOTYPE — trial on an active NestJS or React repo for a week and compare Claude Code's tool-call/token usage with and without Graft wired in before deciding whether it replaces or complements Codanna.

## Change History
### 2026-09-26
Initial discovery and review.
