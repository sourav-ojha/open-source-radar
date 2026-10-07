# Planning with Files

## Summary
Planning with Files (PWF) is an Agent Skill that keeps `task_plan.md`, `findings.md`,
and `progress.md` on disk for a coding agent, and uses lifecycle hooks to re-inject the
relevant slice of that plan into context on every turn. The point is narrow and specific:
survive `/clear`, context compaction, and crashes without the agent losing track of what
it was doing. It installs as a native plugin for Claude Code, Codex CLI, OpenCode, and
58+ other agents via the emerging Agent Skills standard.

## Why I Should Care
This agent (the one running this radar) is exactly the kind of long-running, multi-turn
session PWF targets. Context rot after compaction or `/clear` is a real, recurring cost —
re-deriving "what was I doing and why" from scrollback instead of reading a plan file.
PWF turns that into a solved problem instead of a recurring tax on every long session.

## Problems It Can Remove
- Losing task state after `/clear`, a crash, or automatic context compaction.
- Manually re-explaining prior decisions/findings to an agent that just lost context.
- Building a bespoke "write progress notes to a file and hope the agent reads them" hack
  — this already exists as a maintained, benchmarked, cross-agent standard.

## Practical Uses
- Any multi-hour or multi-session Claude Code / Codex task (migrations, multi-file
  refactors, this very radar's own daily run) where losing state mid-task is costly.
- Multi-agent runs where an orchestrator and subagents/workers need a shared,
  crash-proof source of truth instead of passing state through prompts alone.
- Long debugging sessions where `findings.md` accumulates evidence across many
  `/clear` cycles instead of it living only in a context window that will be wiped.

## Product Opportunities
The "lifecycle hook that fires every turn and reads/writes a small set of structured
files" pattern is reusable infrastructure for any internally-built agent product that
needs cheap, durable state without standing up a database or an agent-memory service.

## Agent / Automation Opportunities
Ships as a Claude Code plugin, a Codex CLI native plugin, and a generic Agent Skill
installable via `npx skills`, skills.sh, or the Claude Code plugin marketplace — so it's
directly usable as-is rather than something to build around.

## Integration
`npx skills install` or Claude Code plugin marketplace install; no server, no config
required to start. Integration effort: **Low** — it is a skill/plugin, not a service to
run or a library to import.

## Architecture Notes
Three flat markdown files plus a lifecycle hook that injects a selected slice of their
content each turn, rather than the whole file — keeping the token cost of "always-on
state" small. Explicit "catchup mode" is required to read past session records, which
the README calls out as a deliberate boundary (automatic recovery only reads the current
project's files, not an open-ended history scan).

## Maturity
Mature for its age: created 2026-01-03, v3.23.0 (released 2026-10-06, matches the npm
package version exactly), 526+ commits, 60 real contributors, active CI. Hit #1 on
Trendshift (third-party trending tracker) on 2026-01-06. 27,314 GitHub stars is an
unusually high star-to-watcher ratio (118 watchers) for a 9-month-old repo — flagged for
awareness, but the commit/contributor volume and consistent GitHub↔npm version sync are
real signals against a fake-star explanation, not an endorsement of the star count itself.

## License
MIT. No commercial-use restrictions.

## Alternatives
- Hand-rolled "write a scratch file and tell the agent to read it" conventions — the
  status quo for most agent users, with none of PWF's lifecycle-hook automation.
- Beads (already catalogued) — tracks upcoming *work* (a task graph); PWF tracks
  in-progress *state* (what's been tried, what was found). Complementary, not competing.
- Agent-native memory services (mem0, Hippo, etc., previously evaluated) — heavier,
  often require an embedding model or external store; PWF is zero-dependency flat files.

## Risks / Limitations
- Claimed benchmark ("96.7% assertion pass rate," "3/3 blind A/B wins") is self-reported
  in the repo's own `docs/evals.md` — treat as a directional claim, not independently
  verified.
- Star count (27k) cannot be independently verified against watcher count (118) using
  available API access; judge the project on commit/contributor activity, not stars.
- Marketing-heavy README (badges, trending banners) — substance checked out on
  inspection, but worth noting as a style flag per the quality filters in AGENT.md §16.

## Recommendation
**USE NOW** — install it as a Claude Code plugin on one real multi-session project (this
radar repo is a reasonable test case) and see whether `task_plan.md` actually survives a
deliberate `/clear` with useful content intact.

## Change History
### 2026-10-07
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
