# PNLCS (Panelica)

## Summary
A self-hosted, MIT-licensed hosting billing platform and client portal positioned as a direct open alternative to WHMCS — product catalog, checkout, recurring invoicing, domain registration, SSL certificate management, support tickets, a knowledge base, and affiliate tracking, with a deliberately WHMCS-familiar data model and module ecosystem (servers, gateways, registrars). Built with Laravel 13, PHP 8.4+, and MySQL 8. Ships an official Docker image, a REST API, and an MCP server.

## Why I Should Care
This is directly applicable to the MSSP security admin-portal SaaS side of the operator's work: a client billing and support portal that replaces a recurring WHMCS license, with a Stripe payment gateway and a Proxmox VE server module both confirmed tested in production by the maintaining team.

## Problems It Can Remove
- A recurring WHMCS license fee for anyone billing clients for hosting, security services, or reseller infrastructure.
- Building a bespoke client-invoicing/ticketing portal from scratch for a services business.
- Vendor lock-in to a proprietary hosting-billing panel when the underlying need (recurring invoices, domain/SSL tracking, support tickets) is well understood and modular in WHMCS-style products.

## Practical Uses
- Client billing and support portal for an MSSP practice invoicing security-service retainers.
- Reseller-hosting billing (domains, SSL, tickets, affiliate tracking) for a side VPS-reselling business, using the tested Proxmox VE module.
- Drop-in replacement for WHMCS for anyone already familiar with its workflows, without the license cost.

## Product Opportunities
The Proxmox VE integration (tested end-to-end against a live host with a pool-limited API token) makes this a workable billing front-end for a self-hosted VPS-reselling product — sell VMs, bill recurring, and manage the underlying Proxmox resources from one portal.

## Agent / Automation Opportunities
Ships a documented MCP server alongside the REST API (per the project's docs site), making client/invoice/ticket data queryable by an AI assistant without custom integration work.

## Integration
Official Docker image (`panelica/pnlcs-runtime` on Docker Hub) or a standard Laravel install (PHP 8.4+, MySQL 8, Alpine.js, Tailwind CSS 4). It is a full application to deploy as-is rather than a library to integrate into an existing Node/Mongo stack. **Integration effort: Medium** — straightforward to stand up via Docker, but it is a foreign (PHP/Laravel) stack to maintain or extend if customization is ever needed.

## Architecture Notes
Deliberately mirrors WHMCS's data model and module ecosystem (servers, payment gateways, domain registrars, SSL providers) so anyone who has run WHMCS should recognize the workflows immediately — a pragmatic choice that trades architectural novelty for a near-zero learning curve on migration.

## Maturity
Emerging. Created 2026-04-18 (~5 months old), 60 stars, 21 forks, latest release v1.2.0 (2026-07-10). Built by the team behind the Panelica server management panel as a companion product "in spare time alongside our main product," per the README — a small, competent, but not large team. The core client portal, billing flows, Panelica server module, and Proxmox VE module are described as tested; broader module coverage (other registrars/gateways) is less clearly verified.

## License
MIT. No restrictions on commercial use, resale, modification, or embedding — a meaningful licensing advantage over WHMCS's proprietary terms.

## Alternatives
WHMCS (proprietary, the incumbent this directly targets), FOSSBilling (older PHP/GPL project, formerly BoxBilling, less actively evolving), Blesta (proprietary). PNLCS's distinguishing pitch is a modern Laravel 13 stack plus MIT licensing against WHMCS's cost and FOSSBilling's slower pace of development.

## Risks / Limitations
- Laravel/PHP, not the operator's primary MERN/Node toolchain — treat as a standalone self-hosted application, not something to extend in-house without picking up Laravel.
- Small team maintaining it alongside their main commercial product — maintenance continuity risk if priorities shift.
- Young (5 months) and modestly starred (60) — not yet proven at meaningful production scale beyond the maintainers' own use.
- No independent security audit found; a billing/payment system handling client data warrants standard self-hosting due diligence before production use.

## Recommendation
PROTOTYPE — worth trialing for MSSP client billing given the direct fit and MIT license, but validate the Stripe flow and any registrar/gateway modules actually needed before relying on it for real invoicing.

## Change History
### 2026-09-27
Discovered and reviewed. GitHub API verified: MIT, 60 stars, 21 forks, created 2026-04-18, latest release v1.2.0 (2026-07-10), archived: false. README confirms Docker Hub image, live demo (hosting.panelica.com), and documented REST API + MCP server at docs.pnlcs.com.
