# mngr (Imbue)

## Summary
`mngr` is a Unix-style CLI from Imbue (the AI research lab behind Sculptor/Avalon) for running many coding agents — Claude Code, Codex, or any other CLI-based agent — in parallel, anywhere: locally, in Docker containers, on remote hosts via SSH, or on services like Modal. It frames agent lifecycle management the way git frames code versioning: `create`/`list`/`connect`/`message`/`push`/`pull`/`clone`/`destroy` instead of `commit`/`push`/`pull`/`fork`.

## Why I Should Care
Running more than one or two coding agents at a time quickly turns into ad hoc tmux panes and SSH tabs. `mngr` gives that a real interface — list all running agents, see which are blocked waiting on input, jump into any of them to chat or debug — without adopting a managed multi-agent SaaS or a specific vendor's orchestration platform.

## Problems It Can Remove
- Hand-rolled SSH/tmux juggling when running several agents against different repos or remote boxes at once.
- Vendor lock-in to a single coding-agent provider's own orchestration UI.
- No visibility into which of several running agents is idle, blocked, or done.

## Practical Uses
- Fan out a batch of Claude Code/Codex agents across EC2 boxes or Docker containers for parallel, independent tasks (e.g. one agent per microservice in a MERN monorepo).
- Keep a long-running agent working on a background task while checking in periodically via `mngr connect`.
- Standardize how agents get created/destroyed across a team, independent of which underlying CLI agent each person prefers.

## Product Opportunities
The "git for agents" lifecycle model is a reusable mental model worth applying to any internal tool that needs to track ephemeral, resumable agent sessions across machines.

## Agent / Automation Opportunities
This *is* the agent-orchestration layer — CLI-first, extensible via plugins, no managed service required. Fits directly into the "agent orchestration" and "AI-agent leverage" axis of this radar.

## Integration
- **Installed locally**: `curl -fsSL https://raw.githubusercontent.com/imbue-ai/mngr/main/scripts/install.sh | bash`, or via PyPI (`pip install imbue-mngr`).
- **CLI-invoked**: yes, sole interface.
- **Self-hostable**: entirely — built on SSH, git, and tmux, no managed backend.
- **Integration effort: Low-Medium** — install is trivial; getting real value requires already having remote hosts/containers/Docker set up to fan agents out to.

## Architecture Notes
Deliberately built on primitives that already exist and are well understood (SSH, git, tmux, Docker) rather than a bespoke orchestration protocol — this is the same "boring primitives, not a new platform" philosophy that makes a tool easy to trust and to debug when it breaks.

## Maturity
Emerging: created January 2026, actively pushed daily as of this review, 413 stars / 44 forks. No tagged GitHub release; PyPI package `imbue-mngr` is at 0.2.17 (published 2026-06-18), noticeably behind current `main` — check `main` directly rather than assuming the PyPI package reflects the latest CLI surface. Explicitly a public mirror of Imbue's internal repo with automatic commit export, not a side project.

## License
MIT (confirmed via repository LICENSE file; GitHub's license detector reports NOASSERTION only because of a preceding subfolder-override note in the LICENSE file itself, not because the terms differ from MIT). No commercial-use restrictions.

## Alternatives
tmux/SSH scripts (status quo), Paseo (`getpaseo/paseo`, cataloged today — polished desktop/mobile/web app instead of a CLI-only tool), Preloop (previously cataloged agent control plane), managed multi-agent SaaS platforms. `mngr`'s differentiation is staying CLI-only, provider-agnostic, and infrastructure-light (no daemon/server component required beyond SSH+tmux).

## Risks / Limitations
- No tagged GitHub releases yet; PyPI package lags `main` by several months — track `main` directly for now.
- Modest traction (413 stars) despite credible backing; still early.
- CLI-only — no mobile/remote-monitoring surface (see Paseo for that use case instead).

## Recommendation
PROTOTYPE — try it for fanning out 2-3 agents across local Docker containers before trusting it with remote-host fleets; low switching cost given it wraps existing SSH/git/tmux rather than replacing them.

## Change History
### 2026-09-23
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
