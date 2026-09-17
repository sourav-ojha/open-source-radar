# TokenTimer

## Summary
Self-hosted expiration manager that unifies certificates, API tokens, secrets, licenses,
and subscriptions across providers into one view, with automated end-to-end certificate
operations (CertOps: ACME/DNS-01 renewal, deployment, verification, atomic rollback via
an outbound-only agent), multi-channel alerting, and an audit trail.

## Why I Should Care
Expired-certificate and expired-token incidents are a recurring, entirely preventable
operational failure mode. A unified cross-provider view plus real automation (not just
alerting) maps directly onto MSSP client pain — this is closer to a concrete service
offering than a generic secrets vault.

## Problems It Can Remove
Manual certbot/cron scripts per environment; siloed per-provider expiry dashboards; the
"who owns renewal of this thing" ambiguity that causes expired-asset incidents in the
first place.

## Practical Uses
- Centralize expiration tracking for TLS certs, API keys, secrets, and licenses across a
  client's mixed environment
- Automate certificate renewal/deployment/verification (ACME/DNS-01) instead of
  hand-rolled certbot cron scripts per client
- Auto-discover public subdomains and monitor HTTPS endpoints for SSL expiry as part of a
  security assessment deliverable
- Route expiring-secret alerts pulled from Vault/AWS Secrets Manager/Azure Key
  Vault/GCP Secret Manager to PagerDuty/Slack
- Produce an audit trail (who approved what, evidence per renewal step) for a
  client-facing compliance report

## Product Opportunities
Could underpin a concrete recurring MSSP service line: cross-provider cert/token/license
expiration monitoring and auto-renewal as a billable offering.

## Agent / Automation Opportunities
Machine API tokens + an executor job API could let an agent trigger/verify renewal jobs
programmatically; no MCP server is documented, so this would need a thin wrapper.

## Integration
Docker Compose or Kubernetes/Helm for the control plane; a separate outbound-only agent
handles CertOps on managed infrastructure (the control plane never receives or stores
private key material). Medium effort — more setup than a single binary, but well short
of a framework-level commitment.

## Architecture Notes
Security-first design: stores expiration metadata, ownership, and status without storing
secret values or private keys. Integration scan credentials used for one-off imports are
discarded after use; only retained (encrypted at rest) if auto-sync is explicitly
enabled. Approvals are bound by hash to the exact job that runs, and each renewal step
records its own evidence.

## Maturity
Emerging. Created 2026-04-01 (~5.5 months old), 43 stars, 22 open issues, 2 forks. Cloud,
Enterprise, and Core tiers are all referenced in the README, suggesting an open-core
model around this OSS core.

## License
AGPL-3.0 — confirmed by reading the repository's LICENSE file text directly. The GitHub
API reports `license: NOASSERTION` for this repo, likely because the LICENSE file's
header doesn't exactly match a standard SPDX template; don't trust the API badge alone
here, the file itself is unambiguous. Same network-copyleft caveat applies as any other
AGPL project in this catalog.

## Alternatives
cert-manager (Kubernetes-only, no cross-provider token/license/secret tracking); manual
certbot/cron scripts; native cloud-provider expiry dashboards (siloed per provider, no
unified view).

## Risks / Limitations
- Small project (43 stars) — verify the AGPL-3.0 license independently before depending
  on it commercially, since GitHub's own tooling can't classify the license file
- Cloud/Enterprise/Core tiering suggests some capabilities may migrate to paid tiers over
  time
- No independent security audit mentioned for an agent that holds outbound production
  access to deploy certificates — review the CertOps agent's trust model (approval gates,
  kill switch) carefully before granting it production access

## Recommendation
PROTOTYPE — promising enough to trial for the MSSP practice specifically, but young
enough (43 stars, small team) to warrant caution before granting it production
certificate-deployment access.

## Change History
### 2026-09-17
Initial discovery and cataloguing.
