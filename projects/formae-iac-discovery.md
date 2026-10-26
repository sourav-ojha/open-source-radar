# formae

## Summary
formae is a stateless infrastructure-as-code tool. Instead of a Terraform-style state file, an agent periodically discovers what's actually running across AWS, Azure, GCP, Kubernetes, and roughly a dozen other providers (Oracle Cloud, OVHcloud, Fly.io, Vercel, Docker Compose, Databricks, Supabase, Datadog, Grafana, PagerDuty, GitHub Actions, GitLab CI/CD, vLLM, SFTP), records every change and who made it, and lets you reconcile drift resource-by-resource. You work with it through Pkl code (Apple's configuration language), a CLI, or an AI agent via a native MCP server.

## Why I Should Care
Every real AWS account accumulates resources that were never captured in Terraform state — console clicks, one-off Lambda consoles, emergency changes during an incident. formae treats all of that as "unmanaged" and read-only by default, so there's no `terraform import` dance to adopt it. The MCP server also means I can point Claude Code at a live cloud account and have it answer "what's actually running here and what changed last week" through the same schema the CLI uses, instead of an ad hoc AWS CLI exploration.

## Problems It Can Remove
- Manual, error-prone `terraform import` for pre-existing resources.
- The "silent drift" problem — changes made outside IaC that nobody notices until something breaks.
- Building a one-off script to answer "what is actually deployed in this AWS account" during a PTaaS or MSSP engagement on an unfamiliar client environment.

## Practical Uses
- Point formae at a client's AWS account before a security engagement to get a fast, read-only inventory and change history without needing their Terraform state.
- Run the discovery agent continuously against production to catch out-of-band changes (an engineer SSHing in and changing a security group) with a record of who did it.
- Use `formae extract` to bootstrap Pkl code from resources that already exist, rather than hand-writing IaC for a legacy account.
- Wire the MCP server into Claude Code so infrastructure questions and changes go through one audited, schema-checked path instead of raw `aws` CLI calls.

## Product Opportunities
- A fast, no-state-file infra inventory/drift report is a natural pre-engagement deliverable for PTaaS/MSSP work — "here's everything running in this account and what changed recently" without first negotiating access to the client's existing IaC.
- The agent-first MCP design is worth studying as a pattern for exposing any internal infra tool to a coding agent safely.

## Agent / Automation Opportunities
Native MCP server ships with the project (Claude Code has a marketplace install; Codex, Cursor, and OpenCode are documented separately). It exposes the same schemas and operations as the CLI, so an agent can both query and change infrastructure through one audited path rather than shelling out to provider CLIs.

## Integration
Install via a shell script on Linux/macOS (binary goes in `/opt/pel/bin`), or run the agent as a container image / Helm chart on Kubernetes. State (the drift/version ledger) is stored in SQLite, PostgreSQL, Aurora Data API, or SQL Server. Provider support ships as separate plugin processes; writing a custom plugin is documented. Medium integration effort mainly because of the Pkl DSL learning curve, not the install itself.

## Architecture Notes
No central state file — resources are discovered by polling (every ~10 minutes by default) rather than tracked only through writes formae itself made. A separate sync pass (every ~5 minutes by default) records changes made outside formae, and `formae apply --mode reconcile` surfaces each drifted resource for an explicit absorb-or-revert decision. This "discovery-first, state-optional" model is a genuinely different take on IaC than Terraform/Pulumi's state-file approach, and worth studying even independent of adopting the tool.

## Maturity
Emerging. Created October 2025 (~1 year old), 775 stars, 44 forks, 6 subscribers, 9 contributors, 22 open issues. Active release cadence (v0.90.5 as of 2026-10-21). No independent production track record surfaced beyond the project's own claims.

## License
FSL-1.1-ALv2 (Functional Source License) — not an OSI-approved open-source license. Confirmed by reading the LICENSE file directly: internal company use and embedding in a commercial product are explicitly permitted ("for your internal use and access" is a stated Permitted Purpose). The only prohibited use is a "Competing Use" — reselling formae itself or a product that substitutes for it or for the hosted "formae Cloud" offering. Each release converts to Apache-2.0 two years after its publish date. This is fine for self-hosting as an internal tool; it would be a problem only if the plan were to resell a hosted formae-as-a-service.

## Alternatives
- Terraform / Pulumi — mature, but require a state file and manual import for pre-existing resources.
- driftctl — drift detection only, and the project is discontinued.
- Steampipe / CloudQuery — read-only inventory via SQL, no reconciliation or apply path.

## Risks / Limitations
- Non-OSI license (FSL) — re-check terms before any use that could be read as competing with formae or formae Cloud.
- Young project with a small (9-person) contributor base.
- The README doesn't detail exactly how the versioned drift/change history is persisted internally — worth checking the source before relying on it as an audit trail for anything compliance-sensitive.
- A commercial "formae Cloud" hosted tier exists alongside the OSS agent, so feature placement between the two could shift.

## Recommendation
PROTOTYPE — worth running against one real AWS account (ideally a non-production one first) to see how the discovery/drift workflow feels in practice, and to validate the MCP server against Claude Code before trusting it with anything production-critical.

## Change History
### 2026-10-26
First catalogued. Surfaced via a GitHub search for "infrastructure as code tool" during slot 6 (infrastructure/observability/deployment).
