# Preloop

## Summary
Preloop is a self-hostable "AI agent control plane": an MCP firewall that gates tool
access, a model gateway for cost/safety/attribution, policy-as-code with human-approval
gates, and runtime session observability, in one platform that onboards existing agents
(Claude Code, Codex CLI, Cursor, Gemini CLI, and any MCP-compatible agent) with a single
CLI command.

## Why I Should Care
Agent tool-call governance (what can an agent touch, who approved it, what did it cost)
is exactly the kind of infrastructure that's tedious to build in-house and directly
relevant to the MSSP/security-admin-portal side of the work — an audit trail and approval
gate over agent tool access is close to what a security-conscious admin portal already
needs to reason about.

## Problems It Can Remove
- Building a custom proxy in front of MCP servers to enforce which tools an agent may
  call and require human approval for risky ones.
- Building per-model spend tracking/attribution across OpenAI/Anthropic/etc. calls made
  by multiple agents.
- Ad-hoc, unaudited "just let the agent run" setups where nobody can answer what an agent
  did or what it cost after the fact.

## Practical Uses
- Front all local coding-agent MCP traffic through the firewall so risky tool calls
  (shell, filesystem writes, deploys) require explicit approval.
- Use the model gateway to see real per-agent, per-model token/cost/latency attribution
  instead of guessing from provider dashboards.
- Use the security-audit flow presets (SBOM verify, exploit check) as a starting point
  for lightweight evidence-collection in client security reviews — explicitly documented
  as not a substitute for a real conformity assessment.

## Product Opportunities
- Could be the governance layer behind an internal or client-facing "supervised AI agent"
  offering, where the value proposition is auditability rather than raw capability.
- The audit-trail + human-approval pattern is directly reusable for any product that lets
  an agent take actions in a customer's environment.

## Agent / Automation Opportunities
- `preloop agents discover` auto-imports local agent configs, MCP servers, and model
  metadata, then rewrites agent configs to route through the firewall/gateway — low-effort
  onboarding for an already-heterogeneous agent setup.
- Works as the control layer over OpenClaw, Claude Code, Codex CLI, Cursor, Gemini CLI,
  Hermes, OpenCode, Windsurf, and any other MCP-compatible agent.

## Integration
Self-hosted (Docker-implied via `preloop login --url http://localhost:3000`) or via
Preloop Cloud. CLI installable via a curl script or PyPI (`pip install preloop`). Talk
(operator command) channel requires installing a runtime plugin per agent.
Integration effort: **Medium** — the CLI-driven onboarding is low-friction, but getting
real value means routing all agent MCP/model traffic through it, which is an
architecture decision, not a side install.

## Architecture Notes
Splits agent governance into two clean primitives — an MCP firewall (tool-call policy)
and a model gateway (spend/attribution) — with policy-as-code and human approvals sitting
on top of both. That separation is a reasonable reference architecture even for a
custom-built internal version.

## Maturity
Emerging. v0.15.0 as of 2026-08-20, 59 stars, 7 human/bot contributors with two clearly
dominant (d-mo, yconst), active CI, and a published PyPI package. Early but real —
YouTube demo recordings against a live stack, not slideware, per the README's own framing.

## License
Apache-2.0 — no commercial-use restrictions identified.

## Alternatives
Building a bespoke MCP proxy in-house; LiteLLM/Portkey for the model-gateway piece alone
(no MCP firewall or approval workflow); enterprise AI-governance platforms (typically
closed-source and priced for larger orgs).

## Why This One
Most MCP-adjacent tooling in this space is either a pure model gateway (cost tracking
only) or a pure MCP proxy (tool routing only). Preloop combines both with a policy/
approval layer and calls out CRA/EU-AI-Act-adjacent evidence collection explicitly,
which is a narrower and more governance-focused pitch than the general "MCP gateway"
wave seen elsewhere this run.

## Risks / Limitations
- Early-stage (v0.15.0); expect rough edges and API/policy-schema churn.
- Two-person-dominant contributor base — real bus-factor risk.
- The security-audit presets explicitly are not certification-grade; treat as a
  starting point for evidence collection, not a compliance deliverable.

## Recommendation
PROTOTYPE — worth testing `preloop agents discover` against a real local agent setup to
see how much of the onboarding claim holds up before routing anything security-sensitive
through it.

## Change History
### 2026-09-09
Initial discovery and review. Slot 1 (AI agents, MCP, coding productivity) run.
