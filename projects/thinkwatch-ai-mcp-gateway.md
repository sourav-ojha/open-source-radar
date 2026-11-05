# ThinkWatch

## Summary
ThinkWatch is a self-hosted AI API and MCP gateway for organizations. Every LLM request and MCP tool call passes through one Rust gateway that authenticates against the org's OIDC IdP, enforces per-user/role rate limits and budgets, redacts secrets/PII, blocks dangerous tool calls and prompt-injection content, and writes a ClickHouse-backed audit trail. The README's own framing is apt: it plays the role for AI access that a bastion host plays for server access.

## Why I Should Care
It puts a single point of control, cost visibility, and a content/PII safety net in front of every agent's model and tool calls — worth having before agents get broader write access to real infrastructure or client data, which matters given the MSSP/PTaaS secondary context.

## Problems It Can Remove
- A hand-built OpenAI/Anthropic/Gemini format-conversion layer.
- Per-user MCP OAuth with cached tool lists, built from scratch.
- Sliding-window rate limiting and budget tracking per user/role.
- A PII/secrets redaction pass on agent prompts and responses before they leave the network.

## Practical Uses
- Single governed entry point for Claude Code/Cursor/CI-pipeline calls to OpenAI/Anthropic/Gemini/Bedrock, with per-user budgets and chargeback reports.
- MCP tool calls proxied through per-user OAuth so GitHub/Notion/Linear/Slack see the real user, not a shared service account.
- PII/secrets redaction guard on prompts and responses — relevant when agents touch MSSP/PTaaS client data.
- Prompt-injection and dangerous-tool-call content filtering as a safety net for agents with write access.

## Product Opportunities
None identified beyond internal governance use — this is infrastructure, not a feature to resell directly.

## Agent / Automation Opportunities
Fills the governance-layer gap this catalog has repeatedly found only in pieces (Issuerd for IAM, Midplane for SQL, keypaste for secrets) but bundled specifically for the AI-request/MCP-call path with cost accounting built in. ThinkWatch Lite (separate MIT repo) gives the same gateway as a free local desktop app for individual use before standing up the full org version.

## Integration
Docker Compose quick start (`make dev-secrets && make dev-backend`) auto-generates secrets; one-command production deploy (`make deploy`) or a Helm chart. Only the gateway port needs to be reachable by clients. Integration effort: **Low**.

## Architecture Notes
Rust gateway + React console + Postgres + Redis + ClickHouse. Built on four separately-licensed (MIT) ThinkWatch Core crates: `tw-dialect` (API format conversion), `tw-guard` (security guards), `tw-breaker` (circuit breaker), `tw-bedrock` (AWS Bedrock signing). Three independently toggleable security guards (secrets/privacy, dangerous commands, content filter), each startable in monitor-only mode before enforcement.

## Maturity
Emerging. Created 2026-04-02 (7 months old), 814 stars, 23 forks, 4 subscribers, current release v3.11.0 (2026-11-03), pushed the day before review. Feature set is unusually complete for its age.

## License
Business Source License 1.1 — GitHub API reports NOASSERTION; confirmed BSL-1.1 by reading the LICENSE file directly. Free production use up to 10,000,000 billable tokens and 10,000 MCP tool calls per calendar month; beyond that a commercial license is required per LICENSING.md. Converts to GPL-2.0-or-later on 2030-04-02. Companion ThinkWatch Core and ThinkWatch Lite repos are separately MIT. Generous for a solo developer or small team's own usage — flag loudly before offering ThinkWatch-based access to paying customers.

## Alternatives
- **Issuerd** (catalogued) — general-purpose agentic-access IAM, not LLM/MCP-call-specific.
- **mcpspan, Pare** (catalogued) — MCP observability/context-compression, not a governance+cost+redaction gateway.
- **Commercial LLM gateways (Portkey, LiteLLM proxy)** — neither centers MCP-call identity and security guards the way ThinkWatch does.

## Risks / Limitations
- 7 months old — verify stability under real multi-week load before trusting budgets/audit data for anything compliance-sensitive.
- BSL-1.1, not OSI-approved open source — re-read LICENSING.md if usage approaches the free-tier caps.
- License field reported NOASSERTION by the GitHub API; verified independently via the raw LICENSE file.

## Recommendation
**PROTOTYPE** — worth running as the gateway in front of personal/small-team Claude Code and MCP usage given the generous free tier, before trusting it for anything compliance-critical.

## Change History
### 2026-11-05
First catalogued. Discovered via GitHub search while reviewing slot 2 (product infrastructure / APIs / backend components). Reviewed alongside Casdoor and Octelium as three independent entrants pivoting IAM/network/AI infrastructure toward AI-agent gateway capability.
