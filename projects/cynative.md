# Cynative

## Summary
Cynative is an open-source CLI that runs LLM-driven security agents against live cloud infrastructure (AWS, GCP, Azure, Kubernetes, GitHub, GitLab) with read-only, IAM-enforced access. It ships 45 built-in agents (each a reviewed markdown prompt) covering privilege escalation, public exposure, supply-chain risk, and detection coverage, and lets you write custom agents as plain markdown files.

## Why I Should Care
Sourav runs an MSSP security admin-portal SaaS and a PTaaS partnership — this is directly in that secondary context. Cynative's core design idea is unusual and worth taking seriously on its own merits: instead of a coding agent with MCP tools that has ambient, opt-in-filtered credentials, Cynative resolves every planned API call to the IAM actions it requires and checks those against a read-only policy *before* attaching a credential — `secretsmanager:GetSecretValue` is technically an IAM Read action; a naive filter allows it, Cynative's policy blocks it. That is a materially safer default posture for pointing an LLM at production infrastructure.

## Problems It Can Remove
- Manual, ad-hoc cloud security review scripts (custom Steampipe/boto3 queries) for common questions like "what's publicly exposed" or "which roles can escalate to admin."
- The credential-exposure risk of letting a general coding agent with cloud MCP servers run arbitrary queries against production with only opt-in read filters.
- Cross-referencing findings across AWS/GCP/Azure/K8s/GitHub/GitLab by hand — Cynative fans one question out across the whole stack and cross-checks findings against live evidence before reporting them.

## Practical Uses
- Pre-audit sweep before a PTaaS engagement: `cynative -p --agent aws-network-exposure` or ad-hoc natural-language questions like "cloud credentials leaked in source code and their current blast radius."
- Continuous drift detection: "live cloud resources absent from IaC."
- Piping existing scanner output through it for triage: `cat findings.json | cynative -p "triage these findings by exploitability."`
- Building a custom markdown-defined agent tailored to the MSSP portal's specific compliance checklist.

## Product Opportunities
The "agent defined as a markdown file with a read-only policy gate" pattern is directly reusable as a feature inside an MSSP admin portal — customer-facing "ask your infrastructure anything" security Q&A, backed by the same action-gating architecture, would differentiate from a bolt-on chatbot.

## Agent / Automation Opportunities
This is itself an agent-authoring framework: `cynative agents show <name>` prints the exact prompt an agent runs, and a user's own markdown file in `~/.cynative/agents/` overrides a built-in of the same name. It's a template for how to build narrowly-scoped, auditable security agents rather than one general-purpose agent with broad tool access.

## Integration
- **Installed locally**: Homebrew (`brew install cynative/tap/cynative`), install script with SHA-256 checksum verification, or Scoop on Windows.
- **CLI-invoked**: single binary, brings its own sandbox for research code (no host/network access of its own).
- **Self-hosted / internal-only**: designed to run from an instance inside the cloud it's auditing, using that cloud's managed inference, so nothing leaves the environment.
- **Interfaces**: CLI only currently; no HTTP API or MCP server mode found in the README.
- **Integration effort: Low** to try (single binary, `brew install`), **Medium** to operationalize as a scheduled/CI-driven compliance sweep.

## Architecture Notes
Every tool call goes through an "action-gate" that maps the planned API call to its required IAM actions and checks them against a read-only security-audit policy before attaching an STS-scoped read-only session — the credential itself is also boundary-enforced by AWS, not just the client. Findings are cross-checked against live evidence rather than trusted on first pass ("evidence-backed"). Every tool call is written to a fail-closed JSONL audit log — if it can't record, it aborts rather than silently proceeding.

## Maturity
Emerging. Created June 2026 (~3 months old), 203 stars, 6 contributors, but disciplined engineering signals for its age: CI badge, OpenSSF Best Practices badge, weekly-cadence tagged releases (v1.12.1 as of 2026-09-17), and a checksum-verifying install script.

## License
Apache-2.0 — no commercial-use restrictions.

## Alternatives
Steampipe, Cloud Custodian, Prowler, ScoutSuite for traditional rule-based cloud security scanning; a coding agent (Claude Code/Codex) wired to cloud MCP servers for the "ask anything" pattern. Cynative's differentiation is the action-gate + evidence cross-check + fail-closed audit trail combination — none of the rule-based scanners are LLM-driven, and the coding-agent+MCP approach lacks the same enforced read-only boundary by default.

## Risks / Limitations
- 3 months old — no independent track record yet for accuracy of findings at scale.
- Requires an LLM provider API key (Anthropic/OpenAI/etc.) — findings quality depends on the underlying model.
- Small contributor base (6); bus-factor risk if the primary maintainer(s) step back.
- No SOC2/pentest attestation of its own found in the README — worth independent verification before trusting it with production credentials at an MSSP.

## Recommendation
PROTOTYPE — run it read-only against a non-production AWS/GCP account first to validate finding quality and false-positive rate before considering it as part of the PTaaS/MSSP toolchain.

## Change History
### 2026-09-22
Initial discovery and review. Rotation slot 7 (experimental/hidden gems); surfaced via targeted GitHub search, evaluated for direct relevance to the MSSP secondary context in AGENT.md section 0.
