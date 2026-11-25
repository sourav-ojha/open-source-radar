# TokenEconomizer

## Summary
A plugin for Claude Code, Codex, GitHub Copilot CLI and Pi (experimental support for opencode, Cursor Agent, Grok Build, Hermes Agent, the Antigravity CLI and Factory Droid) that makes a coding agent cheaper to run in repositories worked in repeatedly — without changing the agent's model, weights or code. It learns from the sessions an agent has already run in a given repository and uses that experience to cut waste in the sessions that follow, via three mechanisms: micro-skills (remembers repo-specific facts, confirmed over repeated sessions before being trusted), tool pruning (stops sending definitions for tools that go unused), and a failure-loop guard (a judge model nudges — never stops — a turn that looks stuck). From Microsoft Research, Microsoft Security and UC Berkeley, with a published arXiv paper backing the benchmark numbers.

## Why I Should Care
This wraps the exact agent this radar itself runs on (Claude Code), and the measured savings (18.4-30.9% lower mean cost across five agent/model combinations on SWE-Bench, cheaper in all 30 repository x combination cells, Pass@1 up in 4 of 5) come from a real benchmark methodology rather than a marketing number. Installed once in a repo worked in daily, it removes the recurring cost of an agent re-discovering the same build/test commands and re-sending definitions for tools it never uses in that repo.

## Problems It Can Remove
- Re-explaining the same repo-specific facts (the right test command, an environment variable a build step needs) every session.
- Paying, every request, for the full definitions of tools that are never actually used in a given repository.
- An agent burning a full context window repeating the same failing approach instead of being redirected.

## Practical Uses
- Install in whichever repo is worked in with Claude Code most often and let the micro-skills mechanism learn its quirks over the first few sessions.
- Compare measured per-session cost before/after enabling it to decide if a given agent/model combination is worth the ongoing spend.
- Use the tool-pruning mechanism on a repo where only a handful of an agent's available tools are ever actually invoked.
- Rely on the failure-loop guard as a second line of defense during long autonomous agent runs (e.g. an unattended workflow) to avoid silently wasting a run's turn budget.

## Product Opportunities
None directly — this is an agent-cost-control add-on, not a building block for a product being built.

## Agent / Automation Opportunities
Directly applicable to this very radar's own run: TokenEconomizer wraps Claude Code, so installing it against the open-source-radar repo (or any other repo run through Claude Code regularly) would apply the same cost-control loop here. Its own model calls (the fact proposer, the loop judge) run through the user's already-configured agent/plan rather than needing a separate API key or billing relationship.

## Integration
A plugin install into Claude Code, Codex, GitHub Copilot CLI or Pi (mechanism: each agent's own plugin/settings system). Each of the three mechanisms (micro-skills, tool pruning, failure-loop guard) can be toggled independently. What it learns is stored locally in `~/.tokeneconomizer` and does not sync across machines. Integration effort: **Low** — it is designed to be installed once and left running, failing open (your session continues normally) if any part of it errors.

## Architecture Notes
Four stated design principles: work through what the agent already reads (facts enter context at session start, tools are removed through the agent's own settings — nothing depends on the agent learning a new tool); count cost as billed (computed with cache pricing, including the plugin's own model calls); protect task success (facts form only from repeated evidence, tools are pruned only after going unused, the loop guard leaves alone a turn still making progress); and keep every action reversible (a fact contradicted by later sessions is dropped, a pruned tool can be brought back with one command). The paper's published thresholds (a fact needs 3 confirming sessions; a tool is pruned after going unused across 10 prompts spanning 2+ sessions or 5 sessions of any length; the loop judge acts only at ≥0.7 confidence, at most once per turn) suggest real tuning rather than defaults picked arbitrarily.

## Maturity
Emerging. v0.1.0 tagged 2026-10-26, created 2026-10-13 — six weeks old at review. Backed by a published arXiv paper (2610.22732) with SWE-Bench, Web-Bench and two security-auditing-benchmark results, which is unusually rigorous evidence for a project this young.

## License
MIT. No restrictions on commercial or internal use.

## Alternatives
Hand-written CLAUDE.md/AGENTS.md repo notes (static, manual upkeep, no tool-pruning or loop-detection equivalent); prompt-cache-aware agent harnesses such as Dao Code (already catalogued, STUDY) which optimize cache-hit pricing at the harness layer rather than learning repo-specific waste patterns; vendor usage dashboards (Claude Console, OpenAI usage page), which report spend after the fact but do nothing to reduce it.

## Risks / Limitations
- Early release (v0.1.0) — no long production track record yet, though the research behind it is published and benchmarked.
- Its own model calls (fact proposer, loop judge) add to session cost and depend on the user's own agent/model being configured sensibly.
- Learned facts are per-machine (`~/.tokeneconomizer`), with no built-in sync — switching machines means relearning from scratch.

## Recommendation
USE NOW — low installation friction, a real measured benefit, and direct applicability to the exact agent (Claude Code) already in daily use, including for this radar's own run.

## Change History
### 2026-11-25
First seen and reviewed. Discovered via GitHub search for AI-agent cost/budget-tracking keywords (slot 1: AI agents, MCP, coding productivity). Verified license, stars/forks/subscribers and release tag via the GitHub API; README and arXiv abstract read directly to confirm the three-mechanism architecture and benchmark claims.
