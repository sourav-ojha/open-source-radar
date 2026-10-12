# Stashbase CLI

## Summary
Stashbase is a local access-control layer for coding agents (Claude Code, Codex, Cursor, and others): instead of handing an agent raw API keys/secrets, it runs a localhost proxy that substitutes placeholders with real credentials only for approved hosts/methods/paths, denies reads of sensitive files (`.env`, `~/.ssh`) and writes to `.git`, strips disallowed MCP tools from discovery, and logs/audits everything, with an optional remote session mode that can be monitored and revoked mid-run.

## Why I Should Care
A coding agent session today typically has direct access to whatever's in its environment — API keys, `.env` files, SSH config — with no enforcement beyond "don't do that" in a prompt. Stashbase makes that an actual policy boundary instead of a convention: the agent never sees the real secret value, and every outbound call is checked against an allow/deny list by host, method, and path, with an audit trail. Directly relevant to running coding agents against anything touching client or MSSP-sensitive credentials.

## Problems It Can Remove
- Pasting real API keys/secrets into an agent's environment or `.env` file during a session
- No record of what hosts/endpoints an agent actually called during a session
- Agent accidentally reading `.env`/`~/.ssh` or writing into `.git` during an otherwise-scoped task

## Practical Uses
- Run Claude Code against a project that touches a payment or internal-admin API, with the agent only ever seeing placeholders for those credentials
- Scope a specific agent session to a narrow set of `egress_hosts`, blocking any other outbound call by default
- Scan `npm`/`pnpm`/`yarn`/`bun` installs an agent triggers for known-compromised packages via the optional dependency hooks
- Remote mode: hand off a long-running agent session to a teammate/client with the ability to monitor or revoke it mid-run

## Product Opportunities
A pattern worth studying for any internal tool that lets an AI agent act on behalf of a client inside the MSSP admin portal — scoped, revocable, audited credential access is exactly the shape that kind of integration needs.

## Agent / Automation Opportunities
This *is* an agent-automation-security tool — it exists specifically to sit between a coding agent and real credentials/egress. The MCP-tool-denial feature (removing disallowed tools from discovery entirely, not just blocking calls) is a detail worth copying in any custom MCP server exposed to third parties.

## Integration
CLI, run locally. `agent run` starts a temporary localhost proxy enforcing a TOML profile (`.stashbase/agents/<name>.toml`). Native sandboxing uses Seatbelt on macOS / systemd+bubblewrap on Linux; the Docker backend is the recommended option where available for stronger filesystem/network enforcement. Platform gaps: macOS support is Apple Silicon only, and `agent run` on Windows requires WSL2. Integration effort: **Low** for basic proxying, **Medium** if relying on the sandboxing backends across mixed platforms.

## Architecture Notes
- Placeholder substitution happens only in configured HTTP(S) headers for approved hosts — the agent process itself never holds the real secret value
- Policies are plain TOML profiles, not a hosted config the agent must trust blindly
- Remote mode keeps credentials behind a Stashbase-hosted control plane relay rather than the local machine, enabling session-level revocation

## Maturity
Early-stage and explicit about its own limits: the docs state `agent run`'s native backend is "not a security boundary against a malicious process running as the same user," and `stashbase run --proxy` is called a "feasibility experiment," not a production isolation boundary. Open-core: some features depend on a paid plan. 12 stars, 1 visible contributor on this CLI repo, 0 open issues, actively committed (same-day commits at review time).

## License
Apache-2.0 for the CLI. Open-core model — the README states some features require a paid Stashbase plan; verify which capabilities actually ship free before depending on any specific one.

## Alternatives
Generic secrets managers (Infisical, Vault, 1Password CLI) that deliver real values to a process rather than policing requests by host/path; Cynative (already catalogued — pre-checks a planned agent action against IAM policy before attaching a credential, a different control point than Stashbase's live request-proxying).

## Risks / Limitations
- Native sandbox explicitly not a hard security boundary per the maintainers' own docs — don't treat it as one
- Open-core: some functionality gated behind a paid plan, not fully clear from the CLI README alone which
- Platform gaps (Intel Mac unsupported, Windows needs WSL2) limit drop-in use across a mixed dev team
- Remote mode is not a general network sandbox (no SSH/raw TCP coverage)

## Recommendation
PROTOTYPE — the problem (raw credentials sitting in an agent's reachable environment) is real and underaddressed, and the host/method/path-scoped proxy plus MCP-tool-denial design is a solid approach. Worth trying on one real session that touches a sensitive API, with the native-backend caveat taken seriously rather than assumed away.

## Change History
### 2026-10-12
First reviewed and catalogued. Confirmed via GitHub API (Apache-2.0, 12 stars, pushed 2026-10-12) and README inspection (raw.githubusercontent.com).
