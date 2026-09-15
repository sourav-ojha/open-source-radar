# Vicoa

## Summary
An open-source, self-hostable orchestrator for running a fleet of coding agents (Claude Code, Codex, Cursor, GitHub Copilot, Kimi, and others) across machines, steerable from a desktop app, CLI, or native iOS/Android mobile apps. Each agent runs on its own git worktree/branch so several can work the same repo in parallel. Full stack — Postgres-backed FastAPI backend, Electron desktop, Flutter mobile — is self-hostable via Docker; BYO agent subscriptions/API keys.

## Why I Should Care
This is a genuinely differentiated entrant in an otherwise saturated "coding-agent orchestration" wave (a dozen+ near-identical repos were rejected in the 2026-09-14 run). Vicoa's distinguishing feature is real mobile apps with push notifications and live git-diff review — "start at your desk, steer from your pocket" — plus a task board with cron automations that can kick off agent sessions unattended. For someone running several coding agents against side-project repos, checking status and approving diffs from a phone is a concrete time-saver.

## Problems It Can Remove
- Hunting across terminal tabs/SSH sessions to check on multiple running coding-agent sessions.
- Needing to be at a laptop to review an agent's diff or answer a mid-task question.
- Manually kicking off recurring agent tasks (a cron-triggered automation replaces a manual "run this agent daily" habit).

## Practical Uses
- Run Claude Code and Codex in parallel against different branches of the same repo, tracked from one dashboard.
- Get a push notification when an agent needs a decision, and approve/redirect it from a phone.
- Schedule a nightly automation that runs an agent against a maintenance task without manual triggering.

## Product Opportunities
- The "capability card + remote daemon" pattern (a lightweight daemon on any machine, orchestrated from a central self-hosted backend) is reusable for any product that needs to coordinate work across machines a user doesn't want to expose directly.

## Agent / Automation Opportunities
- Directly an agent-orchestration tool; the CLI itself is designed so "your agents can drive Vicoa the same way you do" — i.e., agents can manage other agents through it.
- Automations (cron-triggered sessions) are a built-in agent-scheduling primitive.

## Integration
Self-host with Docker Compose (Postgres + backend + web); desktop app connects via Electron; mobile apps from app stores. Effort: **Medium** — more moving parts than a single-binary tool (backend, daemon per machine, separate mobile/desktop clients), but the self-hosting doc covers the full stack.

## Architecture Notes
Backend: Python/FastAPI + PostgreSQL (SQLAlchemy/Alembic), exposing both a user-facing REST API and an agent-facing REST+WebSocket server. Desktop: Electron running the web UI while supervising a bundled local daemon that actually spawns agent CLIs. Mobile: Flutter. The daemon-per-machine + central backend split is the key idea — agents keep running locally, orchestration/state lives centrally.

## Maturity
Emerging — created 2026-08-28, tagged releases (currently v0.1.26), 255 stars in under three weeks. Actively developed but young.

## License
AGPL-3.0 — flagged per policy: any hosted/multi-tenant redistribution would trigger network-copyleft source-disclosure obligations. Personal self-hosted use is unaffected.

## Alternatives
dcouple/Pane (catalogued 2026-09-14, terminal-first, AGPL-3.0) is the closest comparison — Pane is leaner and terminal-native; Vicoa trades that simplicity for cross-device reach (real mobile apps, task automations) at the cost of a heavier stack (Postgres, Electron, Flutter clients).

## Risks / Limitations
- AGPL-3.0, same licensing caveat as above.
- Heavier operational footprint than a single-binary alternative.
- Mobile apps currently require signing in via the vicoa.ai account flow even when self-hosting the backend — confirm this doesn't create a hard dependency on their hosted identity service before relying on it for anything sensitive.

## Recommendation
PROTOTYPE — worth trying specifically for the mobile-steering workflow if running multiple long-lived coding-agent sessions is already a regular habit.

## Change History
### 2026-09-15
Initial discovery and review.
