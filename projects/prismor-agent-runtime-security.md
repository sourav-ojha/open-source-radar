# Prismor

## Summary
Self-hosted runtime control plane for AI coding agents (Claude Code, Codex, Cursor,
Copilot, Gemini CLI, OpenCode, Aider, and more, plus framework SDKs like
LangChain/LangGraph, CrewAI, and the Claude Agent SDK). It hooks into agent execution to
observe or enforce policy against prompt injection, secret exfiltration, supply-chain
risk, and privilege escalation, with a local dashboard, per-agent IAM, and a signed
audit trail.

## Why I Should Care
It is the broadest self-hosted AI-agent governance tool found so far — most competitors
(Preloop, Destructive Command Guard) cover one slice (MCP/cost gateway, or destructive
command patterns) while Prismor spans prompt-injection detection, supply-chain/IOC
scanning, secrets cloaking, an MCP policy gateway, per-agent IAM, and a compliance-mapped
attestation bundle in one tool. Directly relevant both to day-to-day coding-agent use and
to the MSSP/PTaaS side of the business.

## Problems It Can Remove
- Hand-rolled hooks to block an agent from reading `.env`/credential files and sending
  the content outbound.
- No visibility into what a multi-agent or subagent setup actually did during a session
  (signed, hash-chained audit trail instead).
- Manually mapping AI-agent security posture to OWASP LLM Top 10 / NIST AI RMF / EU AI
  Act for a client engagement or internal review.

## Practical Uses
- Front every MCP server through Prismor's gateway so a poisoned tool result is
  policy-evaluated and injection-scanned before the model ever sees it.
- Give each coding agent/subagent a named IAM identity and least-privilege profile when
  several share a workspace.
- Run in `observe` mode for a week to baseline false-positive rate before switching any
  rule to `enforce`.
- Generate a signed attestation bundle for a client's AI-coding-agent security review.

## Product Opportunities
The attestation-bundle feature (posture + agent inventory + framework-control coverage,
Ed25519-signed) is directly reusable as an MSSP/PTaaS checklist item, or as the basis of
a resold assessment for clients adopting AI coding agents.

## Agent / Automation Opportunities
Native MCP gateway, CLI, and an LLM proxy mode (`ANTHROPIC_BASE_URL`/`OPENAI_BASE_URL`)
that governs agents it can't hook directly by inspecting model traffic instead.

## Integration
`pip install prismor && prismor setup` — a guided wizard picks a governance posture
(`dev-safe`, `trusted-workspace`, `regulated-airgap`) that compiles into
`.prismor/policy.yaml`. Low effort to get running in observe mode; the deeper features
(LLM proxy, inference hooks, IAM per user/agent) are more setup but still self-contained.
Docker supported for containerized agents.

## Architecture Notes
Policy compiles from a chosen governance mode into six axes (enforcement, egress, tool
access, tag rules, sandbox, data boundary) rather than requiring six separate configs.
Tool Tags let a rule say "nothing that reads private data may also reach the network"
without naming every tool individually — MCP tools self-declare capability via `_meta`.
The signed audit trail hash-chains every agent action locally and is verifiable without
Prismor itself (`prismor trail verify`), and Telemetry Sinks forward findings to
OpenTelemetry/Splunk/Datadog/syslog/webhook before any blocking decision is made, so
observability never stalls a tool call.

## Maturity
Emerging. Created February 2026 with an unusually fast release cadence (v1.54.1 by
September) — real multi-contributor project (top contributor 592 commits, several others
4–45 commits), not a single-author star farm, but the feature surface is large and still
moving quickly. Pin a version before depending on `enforce` mode for anything important.

## License
Apache-2.0. No restrictions on commercial use, redistribution, or embedding. An optional
"Live Telemetry" enterprise control-plane link exists — confirm it stays opt-in before
enabling on anything client-facing.

## Alternatives
- **Preloop** (catalogued 2026-09-09) — MCP firewall + cost/token gateway, narrower scope.
- **Destructive Command Guard** (catalogued) — command-pattern blocking only.
- **duncatzat/vigils** (302 stars, Apache-2.0) — similar local control-plane pitch, much
  smaller feature set.
- **halofyai/halofy** (336 stars, AGPL-3.0) — governance-layer overlap, copyleft-licensed.

## Risks / Limitations
- Enforcement hooks sit in the critical path of every agent tool call — a bug or overly
  broad policy could break legitimate workflows, not just block bad ones.
- Very young project; re-test after upgrades rather than assuming stability.
- Some enterprise-tier features (Live Telemetry) touch an external control plane — verify
  opt-in status before use on sensitive engagements.

## Recommendation
PROTOTYPE — worth setting up in observe mode on a low-stakes repo first to see the
false-positive rate and dashboard quality before trusting `enforce` mode anywhere that
matters. The attestation-bundle and IAM features are the most differentiated and worth
testing specifically against the MSSP/PTaaS use case.

## Change History
### 2026-09-24
Discovered and catalogued. Real multi-contributor Apache-2.0 project; broadest
self-hosted AI-agent governance surface found this run, differentiated from
already-catalogued Preloop and Destructive Command Guard by scope (prompt injection,
supply chain, IAM, signed compliance attestation vs. their narrower single-purpose
coverage).
