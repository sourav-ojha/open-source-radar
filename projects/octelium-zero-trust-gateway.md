# Octelium

## Summary
Octelium is a self-hosted, unified zero-trust secure access platform built on Kubernetes. It is deliberately multi-purpose: a zero-config remote-access VPN, a ZTNA/BeyondCorp platform, an ngrok/Cloudflare-Tunnel alternative, an API gateway, an AI/LLM gateway, MCP-gateway infrastructure for agent architectures, and a lightweight self-hosted PaaS — all governed by one identity-based, per-request policy engine (CEL/OPA policy-as-code). 18 months old, AGPL-3.0, 4,154 stars, weekly releases.

## Why I Should Care
It combines three things usually bought or built separately — a zero-trust VPN/ZTNA layer, an API/AI gateway, and secretless access to cloud APIs — under one identity model. That's directly relevant to AWS infrastructure work (EC2, Lambda, S3, EKS) and to the emerging problem of exposing internal services to AI agents without handing them static, long-lived credentials.

## Problems It Can Remove
- Paying for Tailscale/Twingate/Cloudflare Access for zero-trust remote access.
- Managing and distributing long-lived API keys for S3/Lambda/other SaaS APIs.
- A separate ngrok/Cloudflare Tunnel subscription for exposing local dev services or client demos.
- Building a bespoke policy layer for what an AI agent is allowed to reach.

## Practical Uses
- Zero-trust access into AWS/EKS infra instead of a traditional VPN.
- Secretless access to S3/Lambda for both humans and workloads.
- Self-hosted ngrok replacement for local dev or client demos.
- MCP-gateway / A2A infrastructure giving agents policy-controlled access to internal services.
- A lightweight self-hosted PaaS for containerized Next.js/Node apps.

## Product Opportunities
Internal zero-trust access layer for MSSP/PTaaS infrastructure that doubles as a credible "how we secure agent access" story to tell clients.

## Agent / Automation Opportunities
Purpose-built MCP-gateway and A2A support with OAuth2 client-credentials auth and per-request policy-as-code access control. Companion project Cordium (octelium/cordium) gives agents sandboxed, identity-based remote access without distributing credentials to the sandbox — referenced in the README but not independently reviewed this run.

## Integration
Requires a Kubernetes cluster — single-node is explicitly supported and documented for a cheap VPS (DigitalOcean/Hetzner/AWS EC2/Vultr), no proprietary cloud control plane. Integration effort: **Medium** — genuinely self-hostable, but running on Kubernetes and learning its policy model is real work before it replaces an existing VPN/gateway setup.

## Architecture Notes
Identity-aware proxies at L7, WireGuard/QUIC tunnels for client-based access plus clientless BeyondCorp-style access, dynamic OIDC/SAML federation, and attribute-based access control evaluated per-request via CEL/OPA. No notion of a standing "admin" user — zero standing privileges by design.

## Maturity
Emerging. Created 2025-05-18, 4,154 stars, 159 forks, 20 subscribers (healthy ratio), current release v0.49.0 (2026-11-01), pushed same day. Fast-moving but consistently active for 18 months.

## License
AGPL-3.0, confirmed via LICENSE file and GitHub API. The README also shows an Apache-2.0 badge, likely for a client-library subcomponent — verify per-directory licensing before redistribution. Self-hosting internally is unrestricted; redistributing a modified Octelium as a hosted service would trigger AGPL's source-disclosure requirement.

## Alternatives
- **Teleport, Cloudflare Access, Tailscale** — commercial/SaaS-dependent ZTNA equivalents; Octelium is the fully self-hosted version with no cloud control plane.
- **ngrok/Cloudflare Tunnel** — narrower; Octelium replaces this as one of several use cases.
- **Issuerd, Midplane** (already catalogued) — narrower IAM-only or SQL-only agent-access gateways; Octelium's breadth (network + API + AI gateway in one identity model) is the differentiator, at the cost of needing Kubernetes.

## Risks / Limitations
- AGPL-3.0 — fine for internal self-hosting, revisit before any hosted commercial offering.
- Requires a Kubernetes cluster, even single-node — more operational surface than a single binary.
- 18 months old and fast-moving; verify the MCP-gateway/AI-gateway features specifically before depending on them, separate from the more mature VPN/ZTNA core.

## Recommendation
**PROTOTYPE** — worth standing up a single-node instance to test the zero-trust-access and secretless-AWS-API angles before deciding whether it replaces anything in production.

## Change History
### 2026-11-05
First catalogued. Discovered via GitHub search while reviewing slot 2 (product infrastructure / APIs / backend components). Reviewed alongside Casdoor and ThinkWatch as three independent entrants pivoting IAM/network/AI infrastructure toward AI-agent gateway capability.
