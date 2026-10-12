# Terraducktel

## Summary
Terraducktel is a self-hosted Terraform and Helm orchestrator: a FastAPI API, a React UI, Postgres, a drift-detection worker, and a per-run ephemeral Docker executor that together reproduce the core workflow of Terraform Cloud or Spacelift — plan, approval gate, apply, drift tracking, encrypted credential storage, RBAC — without a per-seat SaaS bill.

## Why I Should Care
Terraform Cloud/Spacelift/env0 all charge per-seat or per-resource for exactly this workflow, and it's infrastructure he already runs (AWS, Terraform). Terraducktel reproduces the useful parts of that workflow — plan/apply with an approval gate, drift detection, per-account state storage, Slack/Telegram notifications, GitHub-webhook-triggered plans, OIDC SSO, audit log — as a Docker Compose stack he'd own outright.

## Problems It Can Remove
- A Terraform Cloud/Spacelift subscription for plan/apply gating and drift detection
- Hand-rolled "run `terraform plan` on a schedule and post to Slack if it drifts" cron scripts
- Ad hoc approval-before-apply processes (a Slack thread, a manual checklist) for infra changes

## Practical Uses
- Self-hosted plan/apply pipeline for a small team's AWS + Kubernetes infrastructure, with an approval gate before anything applies
- Scheduled drift detection across every workspace, with Telegram/Slack alerts instead of discovering drift during an incident
- Helm diffs routed through the same approval flow as Terraform changes, for one unified change-review surface
- GitHub webhook-triggered plans on PR open, without wiring that into existing CI from scratch

## Product Opportunities
Could be white-labeled or offered as the infra-ops layer inside an internal platform product for clients who want Terraform Cloud-style governance without the per-seat cost — relevant to the MSSP/PTaaS side if clients need auditable infra-change approval.

## Agent / Automation Opportunities
No MCP server noted. The REST API (plan/apply/approve/run state) is a plausible target for a coding agent to drive — e.g., an agent that proposes a Terraform change and files it as a gated run rather than applying directly.

## Integration
Docker Compose on a single Docker host is the only documented deployment path; the API needs access to `/var/run/docker.sock` to spawn per-run executor containers, which is a real privilege to weigh (anything that can reach the API effectively has host-level Docker access). Optional Forgejo + Forgejo runner + Traefik pieces are included for teams that want self-hosted Git alongside it. Integration effort: **Medium** — Compose-only, but the docker.sock dependency means it isn't safe to expose carelessly.

## Architecture Notes
- Executor containers are ephemeral and spawned per run (clone repo → `terraform plan`/`apply`, with Checkov policy scanning and cost estimation steps built into the run)
- Terraform HTTP state backend is implemented directly in the API, so workspaces point at Terraducktel itself for state rather than S3+DynamoDB (though per-account S3 state storage is also listed as a feature)
- Drift-detector is a separate background worker, decoupled from the request-driven API/executor path

## Maturity
Pre-1.0 (v1.4.0 as of 2026-10-05, four releases in the week of 2026-09-30–10-05). Small team: 4 contributors, 19 stars, 11 open issues. The README itself states end-to-end real-AWS plan/apply through the executor is "in flight" rather than fully proven, and it ships a dev admin account (`admin@test.com`) that must be changed before any real use.

## License
MIT. No commercial-use restrictions.

## Alternatives
Terraform Cloud, Spacelift, env0 (proprietary SaaS, the direct target), Atlantis (open-source, PR-comment-driven plan/apply but no built-in UI/RBAC/drift worker), already-catalogued infra tooling in this radar does not currently cover this specific "self-hosted Terraform Cloud" niche.

## Risks / Limitations
- API requiring docker.sock access is a meaningful attack-surface/privilege decision, not a minor detail
- Pre-1.0, small contributor base, real-AWS apply path explicitly not yet fully proven by the maintainers
- Ships a default dev admin credential that must be rotated manually
- No Kubernetes-native deployment documented, Compose-only

## Recommendation
PROTOTYPE — the feature list (approval gates, drift detection, encrypted credentials, OIDC SSO, audit log) directly reproduces what a Terraform Cloud subscription buys, and the license/cost argument is concrete. The docker.sock privilege and "in flight" apply-path status mean it belongs in a sandboxed, non-production AWS account first, not immediately in front of anything that matters.

## Change History
### 2026-10-12
First reviewed and catalogued. Confirmed via GitHub API (MIT, 19 stars, pushed 2026-10-05, v1.4.0) and README inspection (raw.githubusercontent.com) covering architecture and deployment model.
