# Abide

## Summary
Abide hooks into Claude Code, Codex, and OpenCode and checks every edit (or turn) against
the prose rules in `AGENTS.md`/`CLAUDE.md` that no linter can enforce — "never let a raw
error reach a user," "don't create premature abstractions" — using a purpose-built cheap
decision model ("Jev," from TypeSafe) instead of a full LLM call. One question per rule,
a calibrated probability back, ~300ms and a few thousandths of a cent per check.

## Why I Should Care
This repo's own operating model (`AGENT.md`) is exactly the kind of document Abide is
built to enforce: dozens of prose rules ("no comments unless...", "no premature
abstractions", "never fabricate a version") that a linter cannot check and that a coding
agent will quietly violate over a long session. Abide's own benchmark replayed 93 real
Claude Code sessions against their own `AGENTS.md` and found the agent broke an
unenforceable rule on 1 turn in 13, from the very first edit.

## Problems It Can Remove
Manual review passes that exist purely to catch "the agent violated a rule that isn't
codeable as a lint rule" — the kind of drift that shows up as inconsistent style,
resurfacing anti-patterns, or ignored architectural constraints across a long agent
session.

## Practical Uses
- Enforce this radar's own `AGENT.md` constraints (no fabricated data, no premature
  abstraction, comment discipline) automatically during agent-driven edits.
- Enforce team conventions (error-handling policy, layering rules, forbidden patterns)
  across any repo where Claude Code/Codex/OpenCode does unattended work.
- Cheap enough (fractions of a cent per edit) to run on every turn of a long session
  rather than sampling.

## Product Opportunities
The underlying pattern — a cheap, typed "judge" model that answers one bounded
probability question per decision instead of a full generative call — is reusable
infrastructure. Worth studying as a component for any product that needs high-frequency,
low-latency policy checks on agent or user-generated content, not just code edits.

## Agent / Automation Opportunities
Hooks directly into Claude Code, Codex, and OpenCode. No MCP server; installed as CLI
hooks via `npx @coldtea/abide init`.

## Integration
`npx @coldtea/abide login` then `init`. Requires a TypeSafe API key (typesafe.ai) or a
Vercel AI Gateway key — the tool does not work without an external key for the judge
model, so it is not fully self-hostable out of the box. No built-in rules; it reads
whatever is already in the project's instruction files.

## Architecture Notes
Sends the rule text and the diff (never the full conversation) to a decision model that
returns a typed probability rather than free text — deliberately avoids the
cost/latency/hallucination problems of asking a general LLM to police every edit. This
"judge model as a cheap sidecar to the generation model" pattern also appears independently
in [[jevgrep-code-search]] and in mu (qybaihe/mu, filed as Worth Watching), suggesting it's
becoming a recognized primitive in agent engineering, not a one-off gimmick.

## Maturity
Experimental. Created 2026-09-18 (~12 days old at review), npm package at 0.0.7, no
tagged GitHub release yet. Two visible contributors. Benchmark methodology is disclosed
and reproducible (93 sessions, 22 cents, results table with an independent reviewer
cross-check), which is unusually rigorous for a project this young, but the project
itself has no track record yet.

## License
MIT on the code. Core functionality depends on an external paid API (TypeSafe) or a
Vercel AI Gateway key for the judge model — a real commercial dependency, not a license
restriction, but it means "self-hosted" only goes as far as the hook logic; the judgment
call itself is a network call to someone else's model.

## Alternatives
Traditional linters/static analysis (can't express these rules at all); asking the coding
agent to "self-review" against its own instructions (slow, expensive, unreliable);
manual code review (works, but doesn't scale to agent-speed edit volume).

## Risks / Limitations
Pre-1.0, two-person team, no GitHub release history to judge stability against. Hard
dependency on an external decision-model API for its core value proposition — if
TypeSafe/the gateway is down or the key lapses, enforcement silently stops. Worth a
scoped trial on a non-critical repo before wiring into anything load-bearing.

## Recommendation
PROTOTYPE — try it on this radar repo or another agent-driven repo against `AGENT.md`
itself; the cost and latency are low enough that a trial is nearly free, but the external
API dependency and 0.0.x maturity argue against committing anywhere critical yet.

## Change History
### 2026-09-30
Initial discovery and review.
