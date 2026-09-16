# Beads

## Summary
Beads (`bd`) is a distributed, Dolt-backed graph issue tracker built specifically for
coding agents. It replaces markdown TODO lists and ad-hoc AGENTS.md task notes with a
version-controlled, mergeable task graph that an agent can query for ready work, claim
atomically, and update as it progresses — surviving across sessions and multiple
agents/branches without merge collisions.

## Why I Should Care
Long agent sessions and multi-agent workflows lose state constantly: a markdown TODO
list gets stale, conflicts on merge, or is simply forgotten between sessions. Beads gives
the agent a structured backlog with dependencies, claim semantics, and persistent memory
(`bd remember`) that survives exactly the way project state should.

## Problems It Can Remove
- Hand-maintained TODO.md / AGENTS.md task lists that drift out of sync with reality.
- Merge conflicts when two agents (or an agent and a human) edit the same plan file.
- Re-deriving "what's left to do and what's blocked" from scattered notes every session.
- Building a bespoke internal task-queue-for-agents system from scratch.

## Practical Uses
- `bd ready` / `bd update <id> --claim` / `bd close <id>` as the agent's actual task loop.
- Epics/tasks/subtasks (`bd-a3f8.1.1`) for structuring a large agent-driven refactor.
- `bd init --stealth` to use Beads locally on someone else's repo without committing
  tracker files — useful for personal use on a shared or open-source project.
- `bd remember "insight"` to persist project-specific knowledge that `bd prime` injects
  back into the agent's context automatically on the next session.
- Message-type issues with threading for agent-to-agent or agent-to-human handoffs.

## Product Opportunities
The claim/dependency-graph pattern — agents as first-class citizens claiming work off a
shared, conflict-safe queue — is a reusable primitive for any internal human+agent work
system, not just coding tasks.

## Agent / Automation Opportunities
`bd setup codex|claude|factory|cursor|mux` installs hooks and AGENTS.md guidance
directly for the target agent. A separate MCP server (`beads-mcp` on PyPI) exposes the
task graph to any MCP client without shelling out to the CLI.

## Integration
Install via Homebrew, npm (`@beads/bd`), an install script, or `go install`. No server
required for solo use; Dolt remotes provide git-native sync for team/multi-machine use.
Integration effort: **Low** for a single project trial; **Medium** once relying on
remote-backed sync across multiple clones (schema migrations need one designated clone
to run `bd migrate` and push).

## Architecture Notes
Built on [Dolt](https://github.com/dolthub/dolt), a version-controlled SQL database with
cell-level merge and native branching — so the task graph itself gets git-style
branching/merging instead of being a flat file. Hash-based IDs (`bd-a1b2`) avoid
collisions when multiple agents create tasks concurrently on different branches.
"Compaction" periodically summarizes old closed tasks to keep the context Beads injects
small.

## Maturity
Mature for something this new: created 2025-10-12, 27,188 stars, 489 contributors,
current release v1.3.0 (2026-09-15). Built and maintained by Steve Yegge's Gastown org.
Very active release cadence — expect frequent updates.

## License
MIT. No commercial restrictions. Underlying Dolt dependency is Apache-2.0.

## Alternatives
- Markdown TODO.md / AGENTS.md lists — the actual status quo, with none of Beads'
  dependency tracking or multi-agent safety.
- Linear/Jira/Plane MCP integrations — heavier, hosted or self-hosted services, not
  local-first or stealth-usable on someone else's repo.
- ctx (already catalogued) — tracks agent session *history*; Beads tracks *upcoming
  work*. Complementary rather than competing.

## Risks / Limitations
- Adds Dolt as a new dependency and mental model (branching/merging a database, not just
  files) — a real learning curve before the payoff.
- Remote-sync mode has real operational surface (migration coordination across clones)
  that a solo developer needs to understand before relying on it for anything critical.
- Extremely active development — pin a version rather than tracking `main` directly.

## Recommendation
**PROTOTYPE** — Install on one real project, adopt `bd ready`/`bd claim`/`bd close` as
the actual task loop for a week of Claude Code sessions, and see whether the dependency
graph and cross-session memory actually reduce re-derivation cost before adopting the
git-native sync model more broadly.

## Change History
### 2026-09-16
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
