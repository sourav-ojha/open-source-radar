# doco-cd

## Summary
A lightweight, declarative GitOps continuous-deployment tool for Docker Compose (and
Swarm) — think Portainer's Git-based stacks or a minimal ArgoCD, but scoped specifically
to Compose instead of Kubernetes. Watches Git repos via webhook or polling and redeploys
the affected Compose project when its config changes.

## Why I Should Care
Most self-hosted GitOps tooling assumes Kubernetes. This is scoped correctly for anyone
still running plain Docker Compose on a handful of VMs — which is the realistic deployment
shape for most side projects, MVPs, and small client engagements, not a k8s cluster.

## Problems It Can Remove
A hand-rolled `git pull && docker compose up -d` deploy script; a GitHub Actions SSH-deploy
step that has to be maintained per project; manual redeploys after every merge.

## Practical Uses
- Replace an existing CI/CD SSH-deploy step for any project already deployed via Docker
  Compose on an EC2/EB box
- Give a small client (MSSP portal, PTaaS deliverable) a push-to-deploy workflow without
  adopting Kubernetes or a hosted PaaS
- Multi-environment promotion (staging/prod) by pointing separate doco-cd instances at
  different branches or directories of the same repo

## Product Opportunities
Could serve as the deploy layer under a self-hosted micro-PaaS-style offering without
committing to the operational weight of Kubernetes.

## Agent / Automation Opportunities
No native agent/MCP integration. It's an automation component itself — an agent could
trigger a deploy by pushing to the watched branch, but there's no direct tool-call surface.

## Integration
Single distroless Docker container, low RAM/CPU footprint. Configure a Git provider
webhook (or enable polling) and point it at the repo/branch containing the Compose file.
Low effort — no code changes to the deployed project required.

## Architecture Notes
Supports both webhook-driven and polling-based triggers across multiple Git providers,
with CodeQL and image vulnerability scanning wired into its own CI. Declarative: the
running state is defined entirely by what's in Git, so drift gets corrected on the next
sync rather than accumulating.

## Maturity
Mature. Created 2024-07-18 (2+ years old), 1,666 stars, 60 forks, latest release v0.119.0
(2026-09-17), active weekly release cadence with Renovate-bot dependency automation.

## License
Apache-2.0. No restrictions on commercial use, modification, or redistribution.

## Alternatives
ArgoCD/Flux (Kubernetes-only, much heavier), Portainer's GitOps stacks (proprietary
tiering above the free edition), Coolify/Dokploy (catalogued-class, broader PaaS scope,
heavier to operate), hand-rolled CI/CD SSH-deploy scripts.

## Risks / Limitations
- Automates production deploys — a misconfigured branch or leaked webhook secret has real
  blast radius; trial against a non-critical service first
- Scoped to Docker Compose/Swarm only — no path forward if a project later moves to
  Kubernetes

## Recommendation
PROTOTYPE — trial it against one existing Docker Compose-deployed side project before
replacing an established deploy script, given it touches live deploy infrastructure.

## Change History
### 2026-09-21
Initial discovery and cataloguing. Slot 6 (infrastructure, observability, deployment) run.
