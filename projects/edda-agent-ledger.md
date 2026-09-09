# Edda

## Summary
Edda is a local, tamper-evident, append-only ledger for coding agents: it persists
decisions, work state, and cross-agent coordination in a `.edda/` directory on disk so
that neither dies with a crashed session or a completed process, and layers a
cross-provider code-review workflow (`edda review --agent pi --model ...`) on top.

## Why I Should Care
Two specific, recurring pains with agent-driven development are directly addressed here:
re-litigating the same architectural decision because the reasoning died with a
compacted transcript, and losing track of what a crashed parallel agent session had
already finished. Both are things I've hit informally; this gives them a durable,
inspectable record instead of "ask the agent again and hope."

## Problems It Can Remove
- Re-explaining or re-deciding the same technical tradeoff across sessions because the
  prior reasoning lived only in a now-compacted transcript.
- Manually reconstructing what a crashed or killed parallel agent session had already
  completed.
- Building a custom cross-provider review workflow (e.g., have Claude Code author, then
  have a different model review the same commit) from scratch.

## Practical Uses
- Keep a durable record of "why we chose X over Y" that any future agent session (or
  human) can read back instead of re-deciding.
- Run parallel agent sessions with confidence that a crash is recoverable rather than a
  silent loss of work state.
- Use `edda review --agent pi --model openai-codex/gpt-5.6-sol` to get a second-provider
  review of a branch authored by a different agent/model, with the reviewed SHA, findings,
  and cost recorded.
- Use `edda run -- <command>` to get a clean-SHA local receipt for a command's outcome.

## Product Opportunities
- The tamper-evident, append-only ledger pattern is directly reusable as the audit-trail
  primitive for any product that needs to prove what an autonomous process did and why.

## Agent / Automation Opportunities
- Works with Claude Code, Cursor, Codex, OpenClaw, and any MCP client — not tied to a
  single agent vendor.
- The cross-provider review command is itself an agent-orchestration primitive: one
  agent authors, a different model/provider reviews, and the outcome is recorded rather
  than trusted verbally.

## Integration
Installable via `cargo install edda` (crates.io) or a GitHub release binary. Operates
entirely locally against a `.edda/` directory — no server, no external dependency.
Integration effort: **Low** — it's a local CLI that layers onto an existing agent
workflow rather than requiring infrastructure changes.

## Architecture Notes
Three-layer design worth studying independent of adoption: Layer 1 (memory that survives
sessions), Layer 2 (fleet coordination that survives crashed agents), Layer 3 (control —
deciding what runs next) all built on the same append-only local-ledger primitive rather
than three separate subsystems.

## Maturity
Emerging but disciplined. Solo-authored (705 commits, single contributor) but the issue
tracker itself is evidence of real engineering rigor — the project dogfoods its own
"fleet" coordination tooling for its own development, with test-flakiness and
calibration issues filed and tracked as first-class bugs rather than hidden. v0.6.0
released the day of this review; 74 open issues against 36 stars reflects active internal
tracking, not distress.

## License
Dual MIT/Apache-2.0 — no commercial-use restrictions. (Note: the GitHub API reports only
`Apache-2.0` as the detected SPDX license; the actual repo ships both `LICENSE-MIT` and
an Apache-2.0 license file.)

## Alternatives
Ad-hoc use of a project's own `CLAUDE.md`/`AGENTS.md` files for decision memory (no
tamper-evidence, no fleet coordination); general agent-memory tools (mex, already
catalogued) which focus on codebase knowledge rather than decision/coordination ledgers
specifically.

## Why This One
The agent-memory space is extremely crowded right now (dozens of near-identical "give
your agent persistent memory" repos surfaced in today's discovery alone). Edda is
differentiated by scope: it's not attempting semantic recall over a codebase, it's
specifically a decision/coordination ledger plus a cross-provider review workflow — a
narrower and more concretely useful problem than generic "agent memory."

## Risks / Limitations
- Single-author project — real bus-factor risk despite high commit volume and testing
  discipline.
- Young (created 2026-02-19, ~7 months old); the review CLI and fleet-coordination
  features are still under active internal rework per its own issue tracker.
- Local-only by design — no built-in mechanism for sharing the ledger across machines
  or team members.

## Recommendation
PROTOTYPE — worth trying on a personal project with heavy parallel-agent use to see
whether the decision-ledger and crash-recovery claims hold up in practice.

## Change History
### 2026-09-09
Initial discovery and review. Slot 1 (AI agents, MCP, coding productivity) run.
