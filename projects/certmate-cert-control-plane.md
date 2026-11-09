# CertMate

## Summary
CertMate is a self-hosted TLS certificate control plane: issues and renews from Let's Encrypt, ZeroSSL, Google Trust Services, Actalis, DigiCert, Sectigo, SSL.com, or its own built-in private CA; discovers existing certificates via network probes and Certificate Transparency logs; tracks renewal dates (including ACME Renewal Information) and deployment targets; and pushes renewed certificates out via deploy hooks/webhooks with a verification probe. Its own internal ACME server (defaulting to 45-day certificates) issues for internal/private names and IPv4 addresses that public CAs can't cover. Ships a Web UI, REST API, CLI, and an MCP server.

## Why I Should Care
Certificate renewal is the kind of recurring, low-visibility operational task that causes real outages when forgotten, and the industry-wide shift to much shorter cert lifetimes turns "remember to renew" into "must be fully automated." CertMate is a mature, actively-developed answer that also covers the internal/private-name gap public CAs can't fill — directly relevant to anything behind ALB/CloudFront or running internal mTLS on EKS.

## Problems It Can Remove
- Per-service certbot cron jobs and manual renewal tracking across multiple endpoints
- Lack of visibility into certificates issued outside the normal process (Certificate Transparency-log discovery surfaces these)
- No path to issuing certs for internal service names/IPs, since public CAs won't cover them

## Practical Uses
- Centralize renewal of every cert across EC2/EB/ALB/CloudFront-fronted services behind one control plane with deployment-verification probes
- Run its internal ACME server to issue short-lived certs for internal service names/IPs (e.g. internal ALB listeners, service-to-service mTLS)
- Get ahead of the CA/Browser Forum's SC-081 ballot shortening public cert lifetimes to 200 days (Mar 2026), 100 days (Mar 2027), and 47 days (Mar 2029) by automating now
- Use Certificate Transparency-log discovery to find certificates issued outside the normal process before they expire unnoticed
- Point cert-manager (Kubernetes/EKS) at it as an ACME endpoint for internal names via its private CA

## Product Opportunities
Directly relevant to the MSSP side: a client-facing cert-inventory/expiration dashboard built on CertMate's discovery feature would be a concrete, demonstrable deliverable.

## Agent / Automation Opportunities
Ships an MCP server — an agent can query cert inventory/expiry or trigger issuance/renewal directly. REST API and CLI (`certmate-cli`) are both independently scriptable for automation outside the MCP path.

## Integration
Docker Compose (published image, single downloaded file plus `.env`), a Linux installer script (systemd, Python 3.12), a Helm chart (limited to one replica), or install from source. Runs as a single instance because its renewal scheduler lives inside the web process; the docs suggest active/standby on shared storage for availability rather than native HA. Integration effort: **Medium** — straightforward to stand up, but running your own private CA is a real operational responsibility, not a checkbox.

## Architecture Notes
- Runs its own internal CA rather than proxying ACME for private names — it issues from a root it controls, with EAB keys restricting which names/networks each client may request
- Multi-CA abstraction spans 7 public CAs plus its own private CA, with CSR-only issuance supported (the private key never leaves the requesting device)
- ARI (ACME Renewal Information) support lets the issuing CA adjust the `renews_at` date dynamically rather than relying on a fixed interval

## Maturity
Mature: created 2025-06-24 (~1.5 years old), 1,487 stars, 121 forks, 8 subscribers, 10 open issues, v2.55.2 released 2026-11-06 (frequent release cadence). CI, CodeQL, OSSF Scorecard badges, a live demo, PyPI package, Docker Hub image, and i18n documentation (English, German, Spanish, French, Italian) are all present. Commercial support is separately offered.

## License
MIT for the core (confirmed via GitHub API). A separate multi-tenant edition, CertMate-ng, is source-available under BSL 1.1 — not evaluated here; don't assume that edition inherits the core's MIT terms.

## Alternatives
certimate-go/certimate (9,426 stars, 919 forks) is the larger, more established tool in this exact niche — visual multi-cloud ACME automation across major cloud vendors. CertMate differentiates with its own internal private CA for internal names/IPs, multi-CA support beyond Let's Encrypt, CSR-only issuance, explicit ARI/47-day-era readiness, and an MCP server — but certimate-go is the safer default for purely "automate Let's Encrypt across public domains, visually." cert-manager is the mainstream Kubernetes-native choice but doesn't cover non-cluster endpoints or run a private CA for arbitrary internal names. mkcertWeb and oriolrius/pki-manager-web are narrower self-hosted PKI UIs with less deployment/discovery automation.

## Risks / Limitations
- Single-instance architecture (renewal scheduler inside the web process) — no native HA, only an active/standby suggestion
- Star:subscriber ratio (≈186:1) is elevated, though 121 real forks and a frequent release cadence argue against pure inflation
- CertMate-ng (multi-tenant edition) is BSL-1.1, not OSI-approved
- Running a private CA is itself a security-sensitive responsibility (root key custody, revocation) that trades one operational burden for another

## Recommendation
PROTOTYPE — strong engineering signal and a direct answer to the looming short-lived-certificate era, but certificate-automation failures are high-blast-radius; trial it against a non-critical internal environment before trusting it with anything production-facing.

## Change History
### 2026-11-09
First reviewed and catalogued. Slot 6 (infrastructure, observability, deployment) run. Confirmed via GitHub API (MIT, 1,487 stars, pushed 2026-11-07, v2.55.2) and README inspection (raw.githubusercontent.com) covering the SC-081 cert-lifetime timeline, internal-CA architecture, and DNS provider coverage. Compared directly against the larger certimate-go/certimate (9,426 stars) for competitive context.
