# bex

## Summary
bex is an open-source, Apache-2.0, AI-native alternative to Render: push a Git repo and get a running HTTPS service on infrastructure you own (local Docker or Hetzner), built on a self-managed Kubernetes substrate (Cluster API) with a Go operator behind it. Render-compatible `render.yaml` Blueprints and a Render-compatible CLI exist alongside a REST API, GraphQL API, and MCP server — all five interfaces drive the same operator, with no dashboard-only actions.

## Why I Should Care
Self-hosted PaaS alternatives (Coolify, Dokploy, CapRover) remove the Heroku/Render tax but are built human-first — an agent gets, at best, a REST wrapper around a dashboard. bex inverts that: the MCP `deploy` tool takes a repo plus a manifest and performs the exact same deploy a human triggers from the dashboard. For someone already running EKS and thinking about agent-driven ops, this is the first self-hostable PaaS found that treats "agent deploys a service" as a first-class, not bolted-on, capability.

## Problems It Can Remove
- Paying for Render/Railway/Heroku when the infrastructure (Hetzner, bare Docker) is already available or cheaper to run directly.
- Writing a bespoke internal deploy API so a coding agent can ship services — bex's MCP server already is that API.
- The usual Kubernetes operational overhead of hand-building ingress/TLS/routing/rollback for simple git-push services.

## Practical Uses
- Self-hosted internal-tools PaaS: connect a repo, get a URL, get rollbacks/logs/metrics without hand-building the pipeline.
- Let a coding agent deploy its own output directly — "build this feature and ship it" becomes an MCP call rather than a CI pipeline to maintain.
- Managed Postgres/Valkey-backed services for small internal apps without provisioning RDS/ElastiCache.
- A reference architecture to study: Cluster API-based self-managed clusters, bin-packing, and idle-service hibernation ("sleep = free").

## Product Opportunities
Could underpin an internal "ephemeral preview environment" feature for a product once the project is past its explicit pre-production stage — the same Blueprint-compile-and-converge model that powers a production deploy also fits throwaway per-PR environments.

## Agent / Automation Opportunities
Native MCP server (the README calls out "deploy-from-chat") where an agent gets a repo-plus-manifest `deploy` tool identical to what a human CLI/dashboard user gets — no separate, thinner "agent API" to keep in sync with the real one.

## Integration
Requires a real Kubernetes substrate: Cluster API, `kind` for local bring-up, Hetzner (via CAPH) for production-shaped clusters, Go 1.25+ and `kubectl`/`clusterctl` to run the operator from source. This is not a docker-compose single-binary deploy like Coolify or Dokploy. **Integration effort: High** — correct for someone comfortable with Kubernetes operations (which fits an EKS background), wrong for a quick weekend self-host.

## Architecture Notes
A clean two-layer split: `bex` (control plane + operator) is node-aware but provision-unaware — it never SSHes or calls cloud APIs, only reads Node/Pod state and decides placement; `bex-infra` (Terraform + Cluster API + autoscaler) owns provisioning and reacts to what `bex` needs. After initial bring-up, the app cluster becomes self-managing: Cluster API controllers run as pods inside the very cluster they provisioned, and a disposable bootstrap cluster (`bex-bootstrap`, single-node k3s) is destroyed once it hands off. Image pulls go through a bundled `bex-zot` OCI registry rather than an external one.

## Maturity
**Experimental — explicitly not production-ready.** The README states this directly: "bex is in active development and is not ready for production workloads. APIs and configuration can change." Created 2024-04-30 (2+ years old) with active same-day commits at review time, but real human contribution is small: beyond bot accounts, only `puncsky` and `doranoda` show up in the top contributors. 578 stars, 65 forks, 45 subscribers — a healthy, non-inflated engagement ratio.

## License
Apache-2.0, confirmed via GitHub API. No restrictions on commercial or embedded use once the project matures past its current pre-production stage.

## Alternatives
Coolify and Dokploy (both mature, docker-compose-first, much larger communities, no agent-native control plane), CapRover (similar, simpler, Docker Swarm-based), and the proprietary originals (Render, Railway, Heroku) it is explicitly positioned against.

## Risks / Limitations
- Explicit non-production warning from the maintainers themselves — treat anything built on it today as disposable.
- Effectively a 2-person project despite its age — bus-factor risk if depended on.
- Kubernetes/Cluster API operational knowledge is a real prerequisite, unlike single-binary alternatives.
- No independent production track record found beyond the vendor's own bex.co hosted instance.

## Recommendation
PROTOTYPE — the agent-native control-plane design is genuinely differentiated and worth running through a local `kind`-based trial, but the project's own "not production-ready" warning and small contributor base mean this stays an experiment, not an infrastructure dependency, for now.

## Change History
### 2026-11-02
First reviewed and catalogued during slot 6 (infrastructure/observability/deployment) research. Previously surfaced and held to Worth Watching on 2026-09-21 pending a deep review ("genuinely distinct, but not yet deep-reviewed"); confirmed via GitHub API (Apache-2.0, 578 stars, 65 forks, 45 subscribers, created 2024-04-30, pushed same day) and by reading the README and `docs/ADR002-architecture.md`.
