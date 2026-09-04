# mex

## Summary
A repo-local, agent-maintained "living wiki" for a codebase. Builds a deterministic code graph with Tree-sitter and SQLite across TypeScript/JavaScript/Python/Rust (including Express route-to-handler awareness), then routes only the relevant architecture/decisions/conventions pages into an agent's context instead of a single giant instruction file. Drift checks detect when a refactor invalidates a wiki claim.

## Why I Should Care
Directly fits a MERN/TS stack, including explicit Express awareness. Addresses the exact failure mode of heavy coding-agent use: a giant CLAUDE.md/AGENTS.md file that floods context and drifts stale, versus a wiki that grows from real work and flags when it no longer matches the code.

## Problems It Can Remove
- Agents re-discovering the same architecture and conventions at the start of every session.
- A single bloated instruction file that either omits detail or floods context.
- Documentation that silently goes stale after a refactor, with no signal that it's now wrong.

## Practical Uses
- Stop a coding agent re-scanning an entire repo at the start of every session.
- Keep architecture, decisions, and conventions as reviewable, version-controlled Markdown instead of one giant instruction file.
- Catch stale docs via drift detection when a refactor moves or removes the symbols a wiki page pointed to.
- Express route-to-handler awareness is a direct fit for a Node/Express backend specifically.

## Product Opportunities
The living-wiki-plus-drift-detection pattern is reusable infrastructure for keeping agents grounded on any large evolving codebase, including an internal MSSP admin-portal codebase.

## Agent / Automation Opportunities
Installs first-class Claude Code and Codex skills during `mex setup`; the `ROUTER.md`-based context-routing design is built specifically for agent consumption, not human browsing.

## Integration
`npx mex-agent setup` builds the code graph and wiki end to end and installs the relevant agent skills automatically. Low effort. The MCP server package (`packages/mex-mcp`) exists in the repo but is not yet published to npm — build from source if the MCP surface specifically is needed today.

## Architecture Notes
Tree-sitter + SQLite gives a deterministic, framework-aware symbol graph rather than a vector-search approximation. Wiki claims can ground themselves to exact graph nodes via frontmatter, so `mex check` can validate paths, commands, dependencies, and symbol references without spending any AI tokens; `mex sync` only asks the agent to repair what's actually stale.

## Maturity
Emerging. 1,541 stars, pushed same-day, created March 2026, 16 contributors, 493 commits, active Discord, i18n (EN/zh-CN/es/pt-BR), working CI. Very high commit velocity for its age, consistent with heavily agent-assisted development across this cohort.

## License
MIT. No restrictions on commercial use, redistribution, or embedding.

## Alternatives
A single large CLAUDE.md/AGENTS.md file (status quo — floods context and goes stale); mainstream agent-memory tools that compact facts/summaries (lossier and not grounded in a deterministic code graph, unlike catalogued LightAgent/LightMem which target conversational/task memory rather than codebase structure).

## Risks / Limitations
- MCP server package not yet published to npm — the "MCP compatible" badge is partly aspirational until then.
- Requires trusting an agent-maintained wiki to stay accurate; drift checks reduce but don't eliminate staleness risk.
- Young project despite strong star count — worth a stability check before deep reliance.

## Recommendation
PROTOTYPE — pilot on one active MERN/TS repo to see whether the drift-detection loop actually holds up over a few real refactors before rolling it out broadly.

## Change History
### 2026-09-04
Discovered and reviewed. GitHub API verified: MIT, 1,541 stars, pushed_at 2026-09-04 (same day), created 2026-03-21, 16 contributors, 493 commits. npm confirms mex-agent@0.8.0, MIT. MCP package explicitly documented as not yet published.
