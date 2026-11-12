# OpenRun

## Summary
A self-hosted, declarative GitOps deployment platform for web apps and internal tools. Apps are defined as a few lines of config committed to Git; OpenRun syncs, builds, and runs them either on a single Docker/Podman node or a Kubernetes cluster from the same config, with OAuth/OIDC/SAML authentication, RBAC, audit logs, per-app isolated database service bindings, and scale-to-zero for idle apps.

## Why I Should Care
Most self-hosted internal-tool deployment platforms are imperative clones of Heroku (click to deploy, click to update). OpenRun is declarative by default — every change is a Git diff reviewable before it ships, which matters for an MSSP admin-portal context where change auditability is itself a selling point. It also targets both a single EC2-style box and a Kubernetes cluster from the same app spec, so it doesn't need to be replaced when moving from a side-project box to EKS.

## Problems It Can Remove
Removes the need to hand-roll OIDC/SAML auth middleware, RBAC, and audit logging for each internal admin dashboard deployed. Removes the need to manually provision and track per-app database credentials — service bindings auto-create an isolated Postgres/MySQL/SQLite/Redis account per app. Removes the need for a separate reverse proxy (Nginx/Traefik) since OpenRun is its own web server.

## Practical Uses
- Deploy internal admin dashboards with OIDC/SAML + RBAC + audit logging without writing that layer per app.
- Run managed SQLite with continuous Litestream replication to S3 for small internal tools that don't need a full Postgres instance.
- Move an app's deployment definition from a single server to an EKS cluster without rewriting the app spec — only the deployment target changes.
- Scale idle internal tools to zero rather than paying for always-on compute for low-traffic admin utilities.

## Product Opportunities
None directly identified as an embeddable component — this is deployment infrastructure for Sourav's own use, not something to resell.

## Agent / Automation Opportunities
None directly identified.

## Integration
Single binary, runs on one server with Docker/Podman, or deploys the same declarative config onto a Kubernetes cluster. No Dockerfile required for apps matching a built-in AppSpec (Streamlit/Gradio/FastHTML/NiceGUI/Shiny/Reflex); anything else (including a plain Node/Next.js app) needs a Dockerfile in the app's repo. Management via CLI (`openrun app create/update`), a browser console, or fully declarative Git sync (`openrun sync schedule`).

## Architecture Notes
OpenRun implements its own web server rather than sitting behind Nginx/Traefik, which is what enables scale-to-zero (it can intercept a request to a cold app, start the container, then proxy) and built-in OAuth/SAML/cert auth with RBAC without an external identity proxy. Service bindings abstract database provisioning behind a pluggable "binding provider" interface — Postgres/MySQL/SQLite/Redis ship built-in, with room for more providers. Managed SQLite apps get continuous Litestream replication to S3 with automatic restore, letting a lightweight internal tool skip Postgres entirely while still getting durable backups.

## Maturity
Emerging, despite a 3-year history. Created April 2023, 997 stars, 36 forks, 4 subscribers, 0 open issues, latest tagged release v0.20.15 (2026-11-05, actively maintained — pushed_at same day as review). Listed in the CNCF Landscape and carries an OpenSSF Best Practices badge — modest but real third-party corroboration most entrants in the self-hosted-deploy-platform niche lack. Commit history shows one dominant maintainer (1,982 of the repo's commits).

## License
Apache-2.0 — unrestricted for commercial use, self-hosting, or embedding.

## Alternatives
Coolify, Dokku, CapRover (imperative CLI/UI-first; no declarative-by-default GitOps model; generally no direct Kubernetes deploy path). Retool and similar hosted internal-tool builders solve a different problem (building the UI, not deploying arbitrary containerized apps). Raw Kubernetes + Helm + an identity proxy achieves a similar end state with far more manual setup.

## Risks / Limitations
Modest adoption for its age — 997 stars after 3+ years with a single dominant maintainer is a real dependency-risk factor. 0 open issues on an actively pushed 3-year-old repo is unusual; could not independently determine whether this reflects unusually tight issue triage or just low external bug-report volume. AppSpecs (the zero-config path) only cover specific Python/Streamlit-family frameworks — a Node/Next.js app needs a Dockerfile, so the "no code changes, no Dockerfile" pitch doesn't apply to his primary stack out of the box.

## Recommendation
PROTOTYPE. Worth trying for deploying one internal admin tool to evaluate the OIDC/RBAC/audit and service-binding story directly, with the single-maintainer dependency risk kept in mind before relying on it for anything customer-facing.

## Change History
### 2026-11-12
Initial discovery and review. Slot 2 run (product infrastructure, APIs, backend components). Found via GitHub Search API `topic:workflow-engine stars:>50 pushed:>2026-08-01` sweep, which surfaced it as a tangential but more interesting result (OpenRun is a deployment platform, not a workflow engine).
