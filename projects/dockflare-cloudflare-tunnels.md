# DockFlare

## Summary
Automates Cloudflare Tunnel creation and teardown from Docker Compose labels: add labels to a container and DockFlare provisions the tunnel, DNS record, and (optionally) Cloudflare Access/Zero Trust policy for it automatically, with a web dashboard for visibility.

## Why I Should Care
Exposing a self-hosted service securely without opening firewall ports normally means hand-editing `cloudflared` config files and Cloudflare dashboard DNS/Access rules per service. DockFlare collapses that into Docker Compose labels, which matters directly for self-hosting internal tools, client demo environments, or admin dashboards (relevant to the MSSP admin-portal and PTaaS work) without provisioning a public IP or managing TLS certs by hand.

## Problems It Can Remove
- Manual `cloudflared` tunnel config file editing per exposed service.
- Manually creating/maintaining DNS records and Access policies in the Cloudflare dashboard for each self-hosted app.
- The operational overhead of running a reverse proxy + TLS termination + firewall rules just to expose an internal admin tool to specific people.

## Practical Uses
- Spin up a demo environment for a client (e.g., security-portal walkthrough) behind a Cloudflare Access policy without opening any inbound ports on the host.
- Expose internal admin dashboards (Grafana, an internal tool, a staging environment) to a small team via Cloudflare Access instead of a VPN.
- Standardize how every self-hosted side project gets a public, access-controlled URL as part of its `docker-compose.yml`, instead of one-off reverse-proxy configs per project.

## Product Opportunities
Could be embedded as the "secure preview/demo environment" mechanism for a SaaS product's internal tooling, or offered as part of an MSSP admin-portal's "expose this client's dashboard safely" feature — though this is more an operational building block than a product to resell directly.

## Agent / Automation Opportunities
Not agent-facing itself, but a good building block for an automated "spin up a demo environment" pipeline that a coding agent or deploy script could trigger end-to-end (compose up with labels → tunnel + DNS + access policy appear automatically, no manual Cloudflare dashboard step).

## Integration
Self-hosted Docker container, driven entirely by Docker Compose labels — low integration effort for anyone already using Docker Compose and Cloudflare. Requires a Cloudflare account and API token; ties the deployment model to Cloudflare specifically (not portable to a Cloudflare-free self-hosting setup).

## Architecture Notes
Label-driven automation (watch the Docker socket, react to container label changes, call the Cloudflare API) is a reusable pattern worth studying independent of Cloudflare specifically — the same approach (Docker labels as a declarative interface to an external API) shows up in Traefik's label-based routing and is a clean way to avoid a separate config file per managed resource.

## Maturity
Emerging but well-adopted for its niche: 2,472 GitHub stars, created April 2025, actively released (v3.1.6 as of 2026-09-20), very low open-issue count (1) relative to its star count.

## License
GPL-3.0 (confirmed via `LICENSE.MD` file content — GitHub's API misreported this repo's license as "NOASSERTION" because the file isn't named exactly `LICENSE`). Fine for self-hosting and internal use; flagging per AGENT.md §14 that distributing a modified closed-source version, or embedding it inside a closed-source commercial product, would trigger GPL-3.0's copyleft obligations.

## Alternatives
Manually configured `cloudflared`, Traefik + Let's Encrypt (requires an open inbound port), Tailscale Funnel. DockFlare's distinguishing feature is the Docker-label-driven zero-manual-config automation plus a management dashboard, rather than requiring per-service `cloudflared` config edits.

## Risks / Limitations
Hard-couples the deployment model to Cloudflare as a vendor. GPL-3.0 licensing needs attention if ever embedding this into a distributed closed-source product rather than just self-hosting it internally.

## Recommendation
PROTOTYPE — worth trying for the next internal tool or client demo environment that needs secure external access without VPN or manual port-forwarding setup.

## Change History
### 2026-10-02
Initial discovery and review.
