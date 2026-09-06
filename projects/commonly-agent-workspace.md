# Commonly

## Summary
A self-hostable, open-source workspace — a "social kernel" — that gives coding agents
(Claude Code, Cursor, Codex, or a custom runtime) a portable identity: their own name,
memory, skills and workstation, independent of which runtime executes them. Humans and
agents share "pods" with a persistent task board.

## Why I Should Care
It targets exactly the coding-agent workflow already in use (Claude Code) and extends it:
instead of each tool/session starting from a blank slate, an agent's identity and memory
persist across runtimes and sessions, and multiple agents can hand off work to each other
on a shared task board.

## Problems It Can Remove
Removes the "you are the integration layer" problem of re-explaining project context to
every new agent tool or session, and removes vendor lock-in to a single agent runtime's
memory model.

## Practical Uses
- Give multiple coding agents (Claude Code, Cursor, Codex) a shared project memory instead
  of re-explaining context to each
- Run a persistent "pod" where a human and several agents share a task board for a project
- Evaluate multi-agent handoff workflows before committing to one vendor's agent runtime
- 1:1 "Agent DM" chat with an agent that already knows the project it lives in

## Product Opportunities
Could inform an internal multi-agent dev workflow, or be the base for a client-facing "AI
team" feature in a product.

## Agent / Automation Opportunities
This is fundamentally agent infrastructure: a marketplace for installing agents/skills, a
task board agents self-assign from, and a documented tiering for runtimes (native
in-process, cloud sandbox, or bring-your-own-runtime via HTTP).

## Integration
`git clone` + `./install.sh` brings up the full local stack via Docker Compose in one
command. Low-to-medium effort for local self-hosting; the BYO-runtime tier requires
pointing your own agent runtime's HTTP integration at Commonly, which is more setup.

## Architecture Notes
Explicitly separates the "social kernel" (identity, memory, pod membership, history) from
the runtime that executes an agent — the same agent identity can run natively in-process,
in a cloud sandbox, or in a self-managed runtime, without losing its memory or pod
membership.

## Maturity
Emerging. Created 2025-02-03 (~19 months old), 8 contributors, current release v2.1.0
(2026-07-23), pushed to daily, active Discord community and CI badges in the README.

## License
Apache-2.0. Permissive, no per-agent fees claimed for self-hosting. A cloud-sandbox tier is
billed on compute use, but the core is self-hostable without it.

## Alternatives
Vendor-locked hosted agent platforms; ad hoc memory files/CLAUDE.md conventions (the
default today); coleam00/Archon (previously evaluated — more RAG/knowledge-base focused
than multi-runtime agent identity).

## Risks / Limitations
- Early-stage (8 contributors, 1,326 stars over 19 months) — expect rough edges
- Value depends on actually running multiple agent runtimes side by side; if daily work
  stays Claude-Code-only, the "any runtime" pitch is less load-bearing

## Recommendation
PROTOTYPE — worth trying in a low-stakes side project to see whether persistent
cross-session agent memory changes the workflow enough to justify running the extra
service.

## Change History
### 2026-09-06
Discovered during slot 5 run (AI-agent-infrastructure find outside the slot's primary
theme, kept per AGENT.md's "an exceptional find outside it is still welcome" guidance).
