# MoAI-ADK

## Summary
MoAI-ADK is a verification-driven agent orchestration harness built specifically for
Claude Code, distributed as a single Go binary. It wraps Claude Code sessions in a
SPEC → plan → run → sync workflow with "TRUST 5" quality gates (test-first, readable,
unified, secured, trackable — exact names unconfirmed beyond the README's framing) and
adds model/effort routing that can shift work between Claude and GLM to control cost.

## Why I Should Care
Claude Code is the primary coding agent in play here, and MoAI-ADK's main pitch — make
its output trustworthy via structural gates rather than trusting the model's own
self-report — addresses a concrete, recurring problem: verifying that an agent actually
did what it claims. The multi-LLM cost routing (Claude × GLM) is a direct, measurable
lever on coding-agent spend.

## Problems It Can Remove
- Manually re-checking whether an agent's "done" claim is actually backed by passing
  tests/spec coverage.
- Ad hoc, per-session decisions about which model to route a given task to for cost
  reasons — this makes that a configured policy instead of a manual judgment call.
- Building a custom spec-driven-development wrapper around Claude Code from scratch.

## Practical Uses
- Wrapping a Claude Code session on a real feature with the SPEC-driven plan/run/sync
  flow instead of ad hoc prompting.
- Using the Claude×GLM routing on a cost-sensitive, high-volume task (e.g. bulk
  refactors) where paying full Claude pricing for every turn isn't justified.
- Studying the TRUST 5 gate definitions as a template for a lighter-weight, custom
  verification checklist even without adopting the whole harness.

## Product Opportunities
The "verification gate + multi-model cost router" combination is a reusable pattern for
any internally-built coding-agent product where both output trust and per-task LLM
spend need to be governed, not just Claude Code sessions.

## Agent / Automation Opportunities
Installs as a Claude Code-native harness (single Go binary, zero deps, 16 languages)
rather than a library to embed — it sits around Claude Code sessions rather than being
called by other code.

## Integration
Single Go binary install, no runtime dependencies. Integration effort: **Medium** — it
asks a project to adopt its SPEC/TRUST folder structure and workflow conventions, which
is a bigger commitment than a drop-in CLI tool, even though the binary install itself is
trivial.

## Architecture Notes
CI, CodeQL, and Codecov badges on the repo indicate real engineering hygiene rather than
a thin wrapper. Official documentation site and an accompanying book ("Practical Agentic
Coding with Claude Code") suggest the project is treated as a product, not a weekend
script. Latest tagged release (v3.1.2) trails the README's badge-advertised v3.1.3 by
about six weeks despite daily commit activity — worth confirming the gap is just release
cadence rather than a stalled release process before relying on tagged versions.

## Maturity
Emerging: created 2025-09-16, 1,230 stars, 226 forks, current tagged release v3.1.2
(2026-08-21), Apache-2.0. Multi-language docs (English/Korean/Japanese/Chinese) suggest a
team with real users beyond one geography.

## License
Apache-2.0. No commercial-use restrictions.

## Alternatives
- Spec Kitty, buildermethods/agent-os, GSD Pi (all surfaced the same week) — all pursue
  spec-driven agent workflows; MoAI-ADK's distinguishing feature is being Claude-Code-
  specific with an explicit trust/verification framing and built-in multi-LLM cost
  control, rather than a general multi-agent-harness or codebase-standards tool.

## Risks / Limitations
- Adopting the SPEC/TRUST workflow is a real process change, not a drop-in tool — don't
  underestimate the cost of restructuring an existing project around it.
- Tagged-release lag behind the advertised version in the README is a small but real
  signal to watch before depending on a specific pinned release.
- "TRUST 5" gate semantics were not independently verified beyond the README's own
  framing — confirm what each gate actually checks before trusting it as a quality bar.

## Recommendation
**PROTOTYPE** — try it on one real Claude Code feature branch, specifically to evaluate
the TRUST gates' practical friction and the Claude×GLM cost routing's real savings,
before deciding whether the workflow restructuring is worth it project-wide.

## Change History
### 2026-10-07
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
