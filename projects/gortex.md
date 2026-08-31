# Gortex

## Summary
Gortex is a single-binary, fully local code-intelligence engine. It parses codebases in
257 languages via tree-sitter into a persistent graph of functions, classes, call chains,
HTTP routes, and cross-service contracts, and exposes that graph to coding agents and IDEs
through a CLI, an MCP server, and a web UI — aimed at cutting the token cost of giving an
agent codebase context on large or multi-repo projects.

## Why I Should Care
Large MERN monorepos and multi-repo microservice setups are exactly the case where coding
agents burn tokens re-reading files for context every session. A persistent, queryable
code graph that agents can hit via MCP instead of re-scanning the filesystem is a direct
efficiency win, and the cross-repo API-contract detection (matching HTTP/GraphQL/gRPC/
message-topic providers to consumers across repos) is a genuinely useful check for a
microservices-on-EKS setup.

## Problems It Can Remove
Removes (or reduces) the token cost of "let the agent grep/read its way to understanding
the codebase" on every session, and manual cross-repo consistency-checking of API
contracts between services.

## Practical Uses
- Give an agent a persistent call-graph of a large repo instead of re-reading files.
- Detect orphaned API providers/consumers across separate microservice repos.
- Local, zero-server code search/navigation from the CLI or web UI.
- Reduce token spend on agent-driven refactors that touch many files.

## Product Opportunities
None directly — this is developer/agent tooling, not something embedded in a customer
product.

## Agent / Automation Opportunities
175 configurable MCP tools; `gortex install` auto-configures across 19 supported coding
agents (Claude Code, Cursor, Windsurf, Copilot, Codex CLI, Gemini CLI, and others) in one
step.

## Integration
`curl -fsSL https://get.gortex.dev | sh` (or Homebrew/.deb/.rpm/scoop), then
`gortex install && gortex daemon start --detach && gortex track <repo>`. No server
component. Integration effort: **Low**.

## Architecture Notes
Tree-sitter AST parsing across 257 languages/grammars, with "compiler-grade resolution"
layered on top for a subset of languages (Python, TS/JS, PHP, C#, Go, C/C++, Java, Kotlin,
Swift, Zig, Rust, Ruby, Elixir, OCaml, Haskell). Cross-repo contracts are normalized to
canonical IDs (e.g. `http::GET::/api/users/{id}`) and matched for orphan detection.

## Maturity
Emerging by age (created 2026-04-06, ~5 months old, still pre-1.0 at v0.63.9) but unusually
mature supply-chain practice for that age: Sigstore-signed releases, SLSA Level 3, OpenSSF
Scorecard badge, VirusTotal-clean binaries. 1,510 stars, 10+ distinct contributors (not a
single-person project), 60 open issues.

## License
Apache-2.0 — no commercial-use restrictions.

## Alternatives
Sourcegraph (heavier, hosted-first), LSP/ctags-only agent context, plain grep-based agent
context (the default today). Distinct from all of these by being purpose-built for
agent-token-efficiency rather than human code search retrofitted for agent use.

## Risks / Limitations
- Fast star growth in a short window (1,510 stars in ~5 months) alongside heavy badge/
  Discord/Trendshift promotion — verify the "50x fewer tokens" claim hands-on; it is the
  maintainer's own benchmark, not independently reproduced.
- Pre-1.0 (v0.63.9) — breaking changes between releases are plausible.

## Recommendation
**PROTOTYPE** — install against one real large repo and measure actual token savings on a
few representative agent tasks before relying on it more broadly.

## Change History
### 2026-08-31
Initial discovery and review. GitHub API confirmed Apache-2.0, 1,510 stars, created
2026-04-06, pushed_at 2026-08-31, not archived. README verified via
raw.githubusercontent.com; contributor list confirms multiple distinct contributors.
