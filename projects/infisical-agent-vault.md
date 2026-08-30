# Agent Vault

## Summary
Open-source HTTP credential proxy and vault for AI agents, built by Infisical. Rather than handing an agent a real API key, credentials live in Agent Vault and outbound agent requests are routed through it (via `HTTPS_PROXY`), which substitutes real credentials onto the request at the edge. Built specifically to address credential exfiltration — an agent tricked by prompt injection into leaking a key it holds directly.

## Why I Should Care
Any agent-driven automation that calls third-party APIs (LLM providers, GitHub, internal services) currently either holds real keys directly or relies on generic secrets-manager patterns that still expose the key to the agent process. This closes that specific gap.

## Problems It Can Remove
- Coding agents (Claude Code, custom harnesses) holding real `ANTHROPIC_API_KEY` / `GITHUB_PAT` values directly
- Ad hoc `mitmproxy`/`squid` setups that would need custom credential-brokering logic bolted on
- No audit trail of what an agent actually called with its credentials

## Practical Uses
- Remote coding-agent sessions that need GitHub/API access without holding the raw token
- Securing custom agent harnesses or sandboxed automation that call third-party APIs
- Egress-filtering which agents can reach which endpoints, with full request logging

## Product Opportunities
Security control for agent-driven automation inside future micro-SaaS products, preventing a compromised or misled agent from exfiltrating production credentials. Directly overlaps with the credential-exfiltration risk assessment work relevant to the MSSP/PTaaS side.

## Agent / Automation Opportunities
This *is* an agent-infrastructure security building block — a proxy purpose-built to sit between any coding agent, all-purpose agent (OpenClaw, Hermes), or custom harness and the APIs it calls.

## Integration
Single self-contained binary or Docker container; back it with the local encrypted store or an external one (e.g. Infisical itself, for dynamic secrets). Requires bootstrapping the agent's environment to respect `HTTPS_PROXY` — not every agent runtime cooperates cleanly with MITM-style proxying, so integration effort is medium rather than low.

## Architecture Notes
Purpose-built MITM proxy rather than a generic forward proxy retrofitted for credential brokering: substitutes placeholder tokens (e.g. `__anthropic_api_key__`) in headers with real credentials, or replaces auth headers entirely, on outbound requests. Unmatched-host traffic forwards as plain proxy traffic by default; a strict deny mode (`unmatched_host_policy=deny`) rejects it with 403 instead.

## Maturity
Emerging: created March 2026, current release v0.39.1 (2026-08-04), backed by Infisical (a funded secrets-management company), which lowers abandonment risk relative to a similarly-aged solo project. Infisical explicitly positions this OSS binary as the "simpler, self-contained" option and steers production/enterprise use toward its own commercial Agent Proxy.

## License
Core is MIT ("MIT Expat"). GitHub's license detector shows NOASSERTION, but the actual LICENSE file confirms MIT with a carve-out for any `ee/` enterprise directory, matching Infisical's standard open-core pattern. Verify no feature relied upon lives under that boundary before building on it.

## Alternatives
Generic forward proxies (`mitmproxy`, `squid`) with custom brokering logic, HashiCorp Vault + Boundary, Doppler, or the status quo of raw environment variables (no exfiltration protection).

## Risks / Limitations
- Young project; vendor steers production use toward its commercial product
- Requires agent runtimes to respect `HTTPS_PROXY` — verify compatibility per agent before relying on it
- Open issue count (76) relative to stars (2,162) suggests active but still-settling development

## Recommendation
PROTOTYPE — worth testing against one real agent workflow (e.g. a remote coding-agent session) before wider adoption.

## Change History
### 2026-08-30
Initial discovery and review.
