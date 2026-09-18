# Repowise

## Summary
Indexes a codebase's dependency graph, git history, tests, docs, and architectural decisions once into a local index, then answers cited questions, computes change-impact/blast-radius for a diff, scores code health, flags dead code, and generates always-current documentation — surfaced via CLI, MCP tools, editor views, PR analysis, and a local dashboard.

## Why I Should Care
Consolidates several things usually handled by separate tools (a docs generator, a coverage/impact tool, manual architecture-decision records, and an agent-context layer) into one locally-indexed system, and backs its claims with a disclosed benchmark methodology rather than a bare marketing number.

## Problems It Can Remove
Removes the recurring cost of an agent (or a new team member) re-deriving the same architecture understanding every session, and the separate manual effort of keeping docs, dead-code cleanup, and change-impact analysis current by hand.

## Practical Uses
- Give a coding agent cited, task-shaped context instead of letting it read whole files or re-derive architecture from scratch.
- Compute symbol-level blast radius for a diff and run only the tests that diff actually exercises.
- Surface defect-prone files and dead code across a multi-repo workspace before a refactor.
- Generate documentation from the same index used for agent context, rather than maintaining a separate docs tree.
- Catch a breaking API/contract change before merge via automated PR analysis.

## Product Opportunities
The code-health/dead-code/change-impact analysis could be repackaged as a PR-gate check for a commercial engineering-productivity add-on.

## Agent / Automation Opportunities
Ships as MCP tools specifically to cut agent "rediscovery" overhead; its disclosed benchmark claims 3.8 vs. 7.2 tool calls and 393 vs. 13,984 tokens for an equivalent task (n=43, method and limitations published alongside the losing comparison rows).

## Integration
`pip install repowise`; self-hosted, no API key needed for core analysis. MCP server, local dashboard, editor views, and PR/CI analysis are all part of the same index. Medium integration effort — more moving parts than a single CLI, though the core analysis runs entirely locally.

## Architecture Notes
Zero LLM calls for the graph, risk, health, tests, dead-code, and PR-review analysis itself; generated prose is optional on top. Claims to be the "graph accuracy leader at matched coverage" against five other tools across 37,853 oracle edges, with the benchmark's sample and method published.

## Maturity
Emerging — 6 months old (created 2026-03-23), 6,696 stars with a real multi-contributor team (top contributor 1,179 commits, several others in the tens-to-hundreds), 217 open issues at review time indicating active, not-yet-fully-settled development.

## License
AGPL-3.0 for the self-hosted core, with a commercial license available. **Flagged loudly**: running a modified version of Repowise as a network service requires releasing your modifications' source under AGPL unless the commercial license is purchased. Fine for internal/local use (CLI, MCP, dashboard on your own machine); requires the commercial license before embedding it into a hosted product sold to others.

## Alternatives
**mex** (catalogued, PROTOTYPE 8.1) — narrower scope: a drift-checked living wiki grounded in a Tree-sitter+SQLite code graph, without Repowise's code-health scoring, dead-code detection, or PR/CI analysis. **ripwire** (catalogued) — deterministic code-graph context for agents, without Repowise's git-history/decision-tracking or dashboard layer. The status quo for most teams is a bespoke combination of a docs generator, a coverage tool, and manual architecture-decision records.

## Risks / Limitations
- AGPL-3.0 core — confirm licensing terms carefully before any hosted/commercial embedding.
- Fast star growth relative to age; not yet a long track record.
- 217 open issues suggests an actively-developed but not fully settled surface area.

## Recommendation
PROTOTYPE — worth indexing a real repo with it to evaluate the agent-context and code-health claims firsthand before deciding between self-hosting under AGPL or the commercial license for anything customer-facing.

## Change History
### 2026-09-18
Initial discovery and review. Rotation slot 3 (developer utilities, debugging, testing).
