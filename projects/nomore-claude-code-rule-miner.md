# nomore

## Summary
A local CLI (no LLM required by default) that finds the corrections a developer keeps repeating to Claude Code and turns them into evidence-backed CLAUDE.md rules, each traceable to a specific GitHub issue and its reaction count. Ships with a ready-made 20-rule starter pack mined from roughly 8,000 ranked `anthropics/claude-code` issues.

## Why I Should Care
Operationalizes exactly the "feedback memory" problem this radar process itself has to manage by hand — a corpus-backed starter rule pack plus a local miner for an individual's own repeated corrections, each rule traceable to the issue and reaction count that justified it rather than asserted from vibes.

## Problems It Can Remove
Removes the manual, ad hoc process of noticing "I've told Claude this five times" and remembering to write it down as a CLAUDE.md rule.

## Practical Uses
- Pull the 20-rule starter pack into a project's CLAUDE.md in one command via an `@nomore-rules.md` import.
- Run locally against a project's own correction history to surface repeated instructions and turn them into a standing rule.
- Propose a new community rule back via the project's issue template, backed by the same methodology.

## Product Opportunities
None identified — this is a workflow utility, not a product-building block.

## Agent / Automation Opportunities
Directly augments Claude Code's own rule-following behavior by generating the CLAUDE.md files Claude Code reads; no LLM call required for the core mining feature.

## Integration
`npx @globalmpc/nomore`, fully local. Low integration effort.

## Architecture Notes
The rule pack's methodology is unusually transparent: every `anthropics/claude-code` issue labelled `area:model` or `data-loss`, grouped by title similarity and ranked by reaction count, with 20 rules hand-picked from the top of that ranking and reproducible via a documented script.

## Maturity
Experimental. Created 2026-10-24 (10 days old at review). No GitHub Releases/tags found at time of review. 101 stars / 0 forks / 4 subscribers / 1 open issue. Single contributor account (`agent-mpc`).

## License
MIT, no restrictions.

## Alternatives
Hand-maintained CLAUDE.md (status quo); Claude's own built-in memory/feedback behavior (persistent across sessions but not an explicit, shareable, versioned rules file); no directly equivalent correction-mining tool found elsewhere in this catalog.

## Risks / Limitations
- Single contributor account named `agent-mpc` — the name suggests this may itself be an agent-run or agent-assisted account; worth tracking for independent maintenance.
- 0 forks despite 101 stars and 4 subscribers — a mild flag, though the subscriber count is proportionally normal and not consistent with the extreme fake-engagement ratios seen elsewhere in this catalog.
- The local correction-mining feature itself was not hands-on verified here, only the pre-built rule pack's documented methodology.

## Recommendation
PROTOTYPE — today's Small but High-Leverage Utility. Worth pulling the starter rule pack into a real project's CLAUDE.md as a first, nearly-free step.

## Change History
### 2026-11-03
First seen and reviewed. Discovered via GitHub search for recently created small CLI utilities; verified license and engagement metrics via GitHub API directly.
