# Paseo

## Summary
Paseo is a self-hosted control plane for running coding agents (Claude Code, Codex, GitHub Copilot CLI, OpenCode, Pi) in parallel on your own machines, with desktop, web, mobile (iOS/Android), and CLI clients all talking to a local daemon. It adds voice control, an end-to-end encrypted relay for pairing a phone to a home/work machine, and a TypeScript plugin system for themes, panels, commands, and additional agent providers.

## Why I Should Care
This is the "kick off and check on agent work from my phone" product that the mission brief explicitly asks the radar to flag ("this could be a product"). It's a working, self-hosted example of exactly that pattern — multi-provider agent orchestration plus a genuinely cross-device (not just responsive-web) client story — built by one primary maintainer with real community traction.

## Problems It Can Remove
- No way to check on or steer a long-running coding-agent task except staying at the desk with a terminal open.
- Being locked into a single agent vendor's own orchestration UI.
- Ad hoc scripts to SSH into a dev box just to peek at agent progress.

## Practical Uses
- Start a Claude Code or Codex task at a desk, monitor and redirect it from a phone via the mobile app.
- Voice-dictate a task to an agent instead of typing a prompt.
- Self-host the Docker daemon + web UI on a home server or AWS box, connect over Tailscale, and run agents against a persistent dev environment instead of a laptop.
- Run several agent providers side by side on the same task and compare outputs through one interface.

## Product Opportunities
Directly instructive as a reference architecture for a micro-SaaS: local daemon + thin multi-platform clients + E2E-encrypted relay for remote pairing is a pattern reusable well beyond coding agents (any "control a thing running on my machine, from my phone" product).

## Agent / Automation Opportunities
This *is* an agent-orchestration product. The plugin system additionally allows adding new coding-agent providers or custom commands/panels without forking.

## Integration
- **Installed locally**: desktop app (macOS/Windows/Linux) auto-starts the daemon; CLI via `npm install -g @getpaseo/cli`.
- **Self-hostable**: Docker image (`ghcr.io/getpaseo/paseo`), daemon + web UI served from one container.
- **Deployed to cloud infra**: yes, via Docker on any host reachable over TCP/Tailscale/VPN, or the built-in E2E-encrypted relay for NAT traversal.
- **CLI-invoked**: `paseo run --provider claude/opus-4.6 "..."` and equivalent for other providers.
- **Integration effort: Low** — one Docker command or a downloaded desktop app; requires already having at least one agent CLI installed and credentialed.

## Architecture Notes
Explicitly privacy-first: no telemetry, no forced login, and the phone-pairing relay is described as end-to-end encrypted with a direct-connection fallback (TCP/Tailscale) for anyone who doesn't want to use the relay at all. Worth comparing against Imbue's `mngr` (cataloged today) as two different answers to the same "manage many agents" problem — Paseo optimizes for a polished multi-device product experience, `mngr` optimizes for staying a thin CLI over existing SSH/git/tmux primitives.

## Maturity
Emerging but fast-moving and well-resourced: created October 2025 (~11 months old), 18,118 stars / 2,078 forks, real Discord/Reddit/X community, weekly-cadence tagged releases (`v0.9.1` as of this review). Contributor history shows one dominant author plus several other active contributors and bot-driven CI — consistent with a genuine, actively maintained project rather than a star-farmed repo.

## License
Apache-2.0 (confirmed via repository LICENSE file). GitHub's API reports `NOASSERTION` because the LICENSE file leads with a short copyright/third-party-components preamble before the standard Apache-2.0 text, which breaks automatic license detection — the actual terms are unrestricted Apache-2.0. No commercial-use restrictions.

## Alternatives
`mngr` (Imbue, cataloged today, CLI-only), stagewise-io/stagewise and RunMaestro/Maestro (AGPL-3.0 agent-orchestration IDEs, seen this run but not deep-reviewed — noted here as adjacent, more IDE-shaped competitors), vendor-specific orchestration UIs (Claude Code's own session management, etc). Paseo's differentiation is being provider-agnostic across five agent CLIs plus a genuinely native mobile client.

## Risks / Limitations
- Young project (11 months) at very fast star growth — worth re-checking maturity signals (contributor diversity, issue backlog) again in a few months.
- Plugin system explicitly warns plugins run with access to the daemon machine — "install only code you trust" is a real operational caution, not boilerplate.
- GitHub's own license badge/API reporting is misleading (NOASSERTION) despite the actual license being permissive Apache-2.0 — worth a note if evaluating via automated license scanners.

## Recommendation
PROTOTYPE — self-host the Docker daemon against a low-stakes project and try the mobile-pairing flow; the "manage agents from my phone" pattern is worth experiencing directly given how well it matches the product-opportunity axis of this radar.

## Change History
### 2026-09-23
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
