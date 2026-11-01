# HonorBox

## Summary
HonorBox is a zero-server, zero-database alternative to Gumroad/Lemon Squeezy for selling a digital product. The storefront is a static site built by a tiny Node script and hosted free on GitHub Pages; checkout happens via Stripe Payment Links on the seller's own Stripe account; fulfillment is a scheduled GitHub Action that invites each buyer's GitHub account as a collaborator on a private product repo, re-issuing the invite before GitHub's 7-day expiry so a buyer who misses the email doesn't lose what they paid for. No webhooks, server, or database are required by default; an opt-in webhook mode exists for near-instant delivery.

## Why I Should Care
Selling one digital thing (a template, a script, a small internal tool) usually means standing up a storefront and a fulfillment pipeline just to move a handful of units a month. HonorBox needs no running service at all — the only ongoing cost is a scheduled GitHub Action and whatever Stripe charges. That is a materially different trade-off from every other self-hosted Gumroad alternative in the catalog, all of which require a server.

## Problems It Can Remove
- Standing up and paying for a storefront + backend just to sell one thing
- Hand-rolled "take payment, then manually add buyer as a GitHub collaborator" workflows
- GitHub collaborator-invite expiry silently costing a paying customer their purchase

## Practical Uses
- Sell a one-off digital product (template, script, ebook, internal tool) with zero hosting cost
- Automate an existing manual "Stripe + private-repo" fulfillment process with the project's "bring your own store" mode, without adopting the rest of the suite
- Run a storefront for a side project or a paid add-on to an existing product without building a backend for it

## Product Opportunities
- Sell internal scripts, templates, or starter kits as paid digital products with no new infra
- Package a one-off commercial add-on for the MSSP admin-portal SaaS (a report template, a hardening checklist, a Terraform module) without building a storefront for it

## Agent / Automation Opportunities
The fulfillment script (`scripts/fulfill.js`) is a small, config-driven building block — it reads one file mapping Stripe payment links to the repo each grants — that could be wired into any payment-link-to-repo-access workflow independently of the rest of the suite.

## Integration
No server to install. Fork or template the repo, point it at GitHub Pages, create Stripe Payment Links, and configure `scripts/fulfill.js` with the payment-link → repo mapping. Runs on GitHub's own infrastructure (Pages + Actions). Integration effort: **Low**.

## Architecture Notes
- Static storefront built by a zero-dependency Node script; no framework, no external scripts/styles/fonts loaded, page degrades gracefully with JavaScript off
- Fulfillment loop: a scheduled GitHub Action polls Stripe for new payments and invites the buyer as a repo collaborator; re-issues the invite proactively before GitHub's 7-day expiry
- "Bring your own store" mode decouples the fulfillment mechanism from the storefront/theme, so an existing Stripe + private-repo setup can adopt just the automation

## Maturity
Experimental. Created July 2026, latest tag v0.6.0 (2026-07-27), 21 stars, 1 fork, 3 open issues, effectively single-maintainer. The repo itself is a live working store (the project's own demo sells a real paid "HonorBox Pro" add-on), independently corroborated by third-party SaaS-directory comparison pages rather than purely self-asserted.

## License
MIT — no restrictions. The maintainer sells a paid "HonorBox Pro Audit & Reconciliation Suite" as an add-on service, but the core engine used to run the storefront and fulfillment is entirely free and unlocked.

## Alternatives
Gumroad (proprietary, 10%+50¢ fee), Lemon Squeezy (proprietary, 5%+50¢), Polar (proprietary, free tier, also offers instant private-GitHub-repo access as a built-in benefit — the closest direct comparison per HonorBox's own README), Open Payment Host (catalogued same run — a full self-hosted server with multi-gateway support, the opposite trade-off from HonorBox's serverless approach).

## Risks / Limitations
- Very early (21 stars, single maintainer) — low independent validation
- Not a merchant of record: the seller is legally the merchant and must handle their own VAT/sales-tax obligations; the project states this directly
- Delivery via GitHub collaborator access only suits products that are files/code in a repo, not a hosted service or a third-party license key
- Default polling-based fulfillment has a delivery lag of minutes, not seconds, unless the opt-in webhook mode is configured

## Recommendation
PROTOTYPE — the architecture itself (no server, no database, graceful handling of GitHub's invite-expiry edge case) is a genuinely useful pattern worth testing for any small, infrequent digital-product sale, including internal tools or one-off add-ons. Too new and too thin on independent validation for USE NOW, but there's essentially no operational risk to trying it since there's no service to run.

## Change History
### 2026-11-01
First reviewed and catalogued. Discovered via GitHub search for "gumroad alternative self-hosted". Confirmed via GitHub API (MIT, 21 stars, pushed 2026-10-12) and direct README/docs inspection (`bring-your-own-store.md`, `tax.md`). Cross-checked against third-party SaaS-directory listings to confirm the live-store and paid-add-on claims aren't purely self-reported.
