# Pipewright

## Summary
Pipewright is a single static Go binary (frontend embedded, zero runtime dependencies) combining CI, multi-server deployment, and container/server ops — positioned as a one-tool replacement for either Jenkins-with-plugins or the Drone/Woodpecker + Ansible/Kamal + Portainer trio. It includes a visual DAG pipeline canvas, agentless SSH deploys with zero-downtime cutover and rollback, an automatic HTTPS reverse proxy (managed Caddy), per-PR preview environments, and built-in DORA metrics.

## Why I Should Care
The self-hosted deploy-platform space is crowded — a wave of ~15 near-identical Coolify-alternative repos was already rejected on 2026-08-31 for lacking differentiation. Pipewright is different in scope: it also does CI (isolated builds, artifact store, test-report quality gates) rather than assuming CI already exists elsewhere, and ships a real visual DAG canvas, auto-HTTPS/domain routing, and per-PR preview URLs — the auto-HTTPS piece alone removes a recurring 30-60 minute Caddy/certbot setup task per new service.

## Problems It Can Remove
- Replaces a manual GitHub Actions + SSH-deploy-script + Caddy/nginx setup with one self-hosted binary that ties all three together.
- Removes hand-configuring per-PR ephemeral preview environments.
- Removes wiring up a separate DORA-metrics pipeline.

## Practical Uses
- Multi-server, zero-downtime deploys with automatic rollback on failed health checks.
- Environment promotion chains (dev→staging→prod) with per-environment secrets and approval gates.
- Container/image/volume/network management across hosts from one dashboard, with a web ops terminal.
- Optional AI-assisted root-cause diagnosis on build/deploy failure (BYO Claude/OpenAI/Ollama key; core CI/CD path never depends on it).

## Product Opportunities
Could replace ad hoc EC2/EB/ALB deploy scripting for small side-project or client deployments — directly relevant to Sourav's AWS-heavy stack.

## Agent / Automation Opportunities
Optional AI assist for failure diagnosis, repo-analysis-to-pipeline-draft, and a natural-language-to-shell assistant in the ops terminal — all explicitly degradable, core CI/CD does not depend on them.

## Integration
Single static binary; Docker is only required for isolated builds/container deployment (without it, builds degrade to a stub runner). Console, SSH deployment, and notifications work without Docker. Integration effort: **Low**.

## Architecture Notes
Security foundation: single-admin auth (argon2id + CSRF), encrypted credential vault (NaCl secretbox), append-only audit log enforced via SQLite triggers that hard-block UPDATE/DELETE, per-run secret redaction across logs/diagnostics/notifications. SQLite (pure Go) or MySQL for storage.

## Maturity
Emerging. Created 2026-05-30 (~3 months old at review), 34 stars, 2 contributors, but an unusually complete feature set and active recent commits (latest 2026-08-29).

## License
MIT (confirmed via LICENSE file). No restrictions.

## Alternatives
Jenkins, Drone CI + Ansible/Kamal + Portainer, Coolify, Dokploy.

## Risks / Limitations
- Only 34 stars and 2 contributors — real but very early adoption, unproven at scale beyond the maintainer's own use.
- Feature list is unusually large for a 3-month-old project — worth a hands-on trial before trusting it with production deploys.
- Bilingual (Chinese/English) origin project; README is complete in English but community/support channels may skew Chinese-language.

## Recommendation
PROTOTYPE — the scope and packaging are genuinely differentiated, but low contributor count and short track record mean it needs a hands-on trial on a non-critical project first.

## Change History
### 2026-09-07
Initial discovery and review. Slot 6 (infrastructure, observability, deployment) run.
