# AsItIs agent tools

## Summary
A read-only linter for CLAUDE.md, AGENTS.md, SKILL.md and the other files a coding agent reads before it starts work. `asitis loads` shows exactly what loads at session start, what loads later and under what trigger, what doesn't load at all and why, with a token-budget estimate per file. Ships as an npm CLI, a Claude Code plugin, and a GitHub Action.

## Why I Should Care
This directly answers a question this very radar project depends on getting right: does an agent actually load AGENT.md the way the spec assumes, or does a conflicting CLAUDE.local.md, a missing import, or a harness-specific fallback rule silently override it? The tool's own example output shows exactly this kind of silent conflict (an AGENTS.md fallback switched off by a CLAUDE.local.md, with the agent reading a different per-folder CLAUDE.md than expected).

## Problems It Can Remove
Removes guesswork about why an agent seems to be ignoring part of a CLAUDE.md/AGENTS.md setup — instead of trial-and-error edits, it states precisely what loaded, what didn't, and why.

## Practical Uses
- Run against this very repo's AGENT.md/CLAUDE.md setup to confirm the operating spec loads as intended before relying on it in an unattended CI run.
- CI gate (GitHub Action) that fails a PR if a new SKILL.md or rules file silently shadows an existing one.
- Token-budget auditing — see how many tokens of context overhead a project's steering files actually cost per session.

## Product Opportunities
None — a focused internal-tooling utility.

## Agent / Automation Opportunities
Ships as a Claude Code plugin and MCP-adjacent CLI; the GitHub Action form makes it usable as an unattended CI check on steering-file correctness.

## Integration
`npx asitis loads --tool claude-code` — zero install, no dependencies beyond Node. Trivial to drop into a CI pipeline.

## Architecture Notes
Read-only by design: it estimates what a harness would load without actually invoking a model, implying it has to track each harness's own loading rules (discovery paths, fallback behavior, frontmatter limits) as static logic rather than observed behavior — a real maintenance burden as harnesses change their own loading semantics.

## Maturity
Experimental. Created 2026-10-01 (about 7 weeks old), 4 stars. Thin traction, but the core idea is narrow and immediately verifiable by running it.

## License
MIT.

## Alternatives
Manual trial-and-error (the status quo); none of the already-catalogued skill/MCP security scanners (SkillSpector, NVIDIA SkillEvaluator's new HarnessCheck feature as of v1.0.1) solve this specific "what actually loads and why" question, though SkillEvaluator's HarnessCheck is adjacent and worth comparing directly in a future review.

## Risks / Limitations
Very new and thin (4 stars, single maintainer); its static model of each harness's loading behavior will need to keep pace with harness changes to stay accurate.

## Recommendation
WATCH, but worth a one-off run against this radar project's own AGENT.md/state files given how directly it applies — small enough that "try it once" costs nothing.

## Change History
### 2026-11-18
Initial discovery and review as Small but High-Leverage Utility. No tagged release — installed via npm.
