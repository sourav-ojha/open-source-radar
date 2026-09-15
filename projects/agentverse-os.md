# AgentVerse OS

## Summary
A personal cloud operating system for a developer and their AI agents: one Ubuntu server, reached from any device through a browser desktop. Rust core (`cloudd`) manages isolated per-project workspaces (Incus containers with Docker inside, VS Code, Claude Code and Codex), a merged app store of 944 self-hosted apps (Runtipi + Coolify + Umbrel catalogs), ZFS-snapshot backups, and Tailscale-only access with real certificates. Alpha, single-test-box, no user accounts or permissions yet.

## Why I Should Care
It packages exactly the stack Sourav would otherwise hand-assemble himself: a Tailscale-fronted dev box, per-project Docker sandboxes for coding agents, an S3-compatible object store (Garage) wired in as a first-class "capability" rather than a hardcoded endpoint, and a one-click app catalog that already includes n8n, LiteLLM and Gitea. The "capabilities instead of addresses" abstraction (a project asks for `storage.s3` or `llm`, the core wires up env vars and network access) is a genuinely useful pattern for anyone building multi-tenant self-hosted infrastructure.

## Problems It Can Remove
- Manually provisioning a personal dev cloud (Tailscale cert setup, per-project container isolation, reverse proxy per app).
- Hand-rolling credential/env-var injection when swapping infrastructure providers (e.g. Garage for AWS S3, or one LLM gateway for another).
- Maintaining a bespoke app-store/install-script collection for self-hosted tools.

## Practical Uses
- A single personal server running Claude Code / Codex workspaces per project, reachable from a laptop, tablet, or phone.
- A sandboxed place to spin up a self-hosted app (n8n, Gitea, a vector DB) without manually wiring Docker networks and certs.
- A reference architecture for the "capability" abstraction if building an internal multi-tenant dev platform.

## Product Opportunities
- The capability-routing core (`cloudd`) is a candidate architecture to study for any internal PaaS/admin-portal product that needs to grant/revoke infra access per tenant without hardcoding endpoints.

## Agent / Automation Opportunities
- Ships Claude Code and Codex as first-class workspace citizens (they log in with existing subscriptions inside an isolated container per project).
- Includes a voice assistant (Pipecat Voice) and a web-based agent (Hermes Agent) in the app catalog already.

## Integration
Requires a dedicated Ubuntu 22.04+ box (bare metal or VM) — this is infrastructure, not a library. One-command install script; access exclusively via Tailscale (no ports exposed to the public internet). Effort: **Medium** — straightforward for a single test/personal server, but the project itself warns it has no multi-user support yet.

## Architecture Notes
Rust core + Svelte 5 desktop UI; Incus (LXC/VM hybrid) for workspace isolation with Docker running inside each; Komodo for compose-stack app runtime; Coder for workspace control plane; Caddy at the edge for TLS via `tailscaled`; ZFS snapshots + restic for backups. The "project asks for `storage.s3`, core grants access" capability model is the most reusable idea here.

## Maturity
Experimental/alpha — the README explicitly states it "lives on a single test box," works for one person, and has no user accounts or permissions. Created 2026-09-12, no tagged GitHub release yet. Treat as a personal-lab project, not production infrastructure.

## License
Apache-2.0 — no commercial-use restrictions.

## Alternatives
Umbrel, Runtipi, Coolify (each of which this project's app store actually re-packages and merges), CasaOS. None of those bundle AI coding-agent workspaces or a capability-routing layer as a first-class concept.

## Risks / Limitations
- Single-user only right now; no auth/permission model.
- Very young (3 days old at review) — no independent validation of the install script or long-term maintenance commitment from a new org.
- Depends on Incus, which has a learning curve if something breaks.

## Recommendation
PROTOTYPE — worth standing up on a spare box or a cheap VPS as a personal dev cloud experiment; not ready to depend on for anything multi-user or production-facing.

## Change History
### 2026-09-15
Initial discovery and review.
