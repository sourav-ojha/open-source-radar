# SSH Ache

## Summary
A local-first, open-source desktop SSH client (Tauri 2 / Rust + React) with a terminal, SFTP browser, port forwarding, a key/host vault, and an opt-in MCP server that lets an AI agent run commands over your saved SSH hosts — off by default, localhost-bound, default-deny per host, and gated by an in-app approval dialog per command unless a host is explicitly set to unattended "Auto-allow."

## Why I should care
Sourav already operates infra across AWS (EC2, EB, EKS) and likely SSHes into boxes routinely; wiring a coding agent to operate remote servers directly is an obvious productivity move but also an obvious injection-attack surface (an agent that can run arbitrary shell over SSH is exactly the kind of "auto-allow" foot-gun that needs a real approval/audit model, not just a raw exec bridge). SSH Ache's security posture — default-deny, per-command approval, explicit written warning about prompt injection via earlier command output, and an audit log — is more thought-through here than most SSH-client-plus-agent integrations currently on offer.

## Problems It Can Remove
Removes the need to build a custom approval-gated exec bridge between a coding agent and a fleet of SSH hosts, and doubles as a normal daily-driver SSH client (host vault, SFTP, tunnels) so there's no need to run a separate client alongside it.

## Practical Uses
- Daily SSH/SFTP client for personal and MSSP infra work, replacing whatever combination of terminal + FileZilla-equivalent is in use today.
- Giving a coding agent supervised access to a specific low-risk box (e.g., a staging server) for routine ops tasks, with every command requiring explicit approval.
- Port forwarding / SOCKS5 proxy management through a GUI instead of remembering `ssh -L`/`-D` flags.

## Product Opportunities
The approval-gated, audit-logged agent-to-infra bridge pattern is directly reusable as a design reference for any internal tool that needs to let an agent touch production-adjacent systems safely — relevant to both his own products and MSSP client work.

## Agent / Automation Opportunities
MCP server over HTTP/JSON-RPC exposing `list_hosts` and `run_command`; each `run_command` call pops an in-app approval dialog showing the real target host, unless Auto-allow is explicitly enabled per host. No ProxyJump support through the bridge yet.

## Integration
Desktop app download (macOS/Windows/Linux) via GitHub Releases; MCP server toggled on in-app per host, bearer-token-authenticated, bound to `127.0.0.1`. Integration effort: **Low** to install and use as a client; **Low-Medium** to wire into an agent workflow given the built-in MCP bridge.

## Architecture Notes
Rust backend (russh/russh-sftp, portable-pty) with a React/xterm.js frontend; secrets are stored in a `0600` local file mirrored to the OS keychain (keychain used as fallback, not primary, because keychain access ties to code-signing which unsigned builds don't have yet). The maintainer is explicit that the secrets file is **not encrypted at rest** — full-disk encryption is the actual protection today.

## Maturity
Emerging. Created 2026-07-20, ~11 commits from a single contributor as of review, v0.9.0 latest release (2026-08-23), 1 GitHub star. "Community edition" language signals a commercial "Teams" tier exists separately, which is worth watching for future licensing/feature-gating changes but the reviewed client itself is Apache-2.0 and fully functional standalone.

## License
Apache-2.0 (GitHub API-confirmed) for the reviewed "Community edition." A separate paid "SSH Ache Teams" product exists for shared/synced connections — not reviewed here.

## Alternatives
Termius (mature, cross-device sync, but closed-source with a cloud-based AI agent, not self-hosted); Tabby (open-source, mature, has a third-party community MCP plugin bolted on rather than a built-in approval-gated model); Electerm and similar open-source SSH GUIs (no AI-agent story).

## Risks / Limitations
- Single contributor, 1 star, ~6 weeks old — high continuity risk for something touching credentials.
- Secrets stored unencrypted at rest (mode-0600 file); acceptable only if full-disk encryption is already in place.
- No ProxyJump support through the agent bridge; a real gap for anything behind a bastion host.
- Crowded competitive space (Termius, Tabby+MCP, emerging "Termalin"-style agent-native SSH clients) — differentiation rests entirely on the approval/audit design, not on core SSH features.

## Recommendation
PROTOTYPE — worth trying as a daily-driver SSH client given the security-conscious design, and worth a supervised experiment giving an agent access to one disposable/staging host through the approval-gated bridge; do not enable Auto-allow on anything that matters, and don't treat the secrets store as encrypted-at-rest.

## Change History
### 2026-09-01
Initial discovery and review. Rotation slot 7 (experimental / hidden gems).
