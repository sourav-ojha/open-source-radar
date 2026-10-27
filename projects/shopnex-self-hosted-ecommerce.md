# Shopnex

## Summary
An open-source, self-hosted e-commerce platform built on Node.js/pnpm/PostgreSQL with an Astro storefront. Provides an admin dashboard, a Shopify-aligned typed Storefront API, Liquid-based themes, built-in multi-tenancy, and an out-of-process app SDK (`@shopnex/app`) for building and marketplace-distributing extensions via scoped tokens and signed webhooks.

## Why I Should Care
Node/Postgres is squarely within this profile's stack. The explicit Shopify-aligned Storefront API and Liquid theme support lower the friction of porting a Shopify storefront to a self-hosted base, and the out-of-process app-extension model is a cleaner pattern for third-party extensibility than bolting plugins directly into a core app — reusable as an idea even outside e-commerce.

## Problems It Can Remove
Removes the multi-month build of catalog/checkout/admin/theme infrastructure from scratch for anyone needing Shopify-equivalent functionality without the subscription, and removes the need to design an extension framework from zero if building any admin-panel product that wants third-party apps.

## Practical Uses
- Stand up a white-label, self-hosted storefront for a client needing Shopify-equivalent functionality without the recurring SaaS cost.
- Build a custom headless storefront against the Shopify-aligned Storefront API while keeping catalog/admin self-hosted.
- Package a vertical-specific extension (shipping calculator, loyalty program) via the app SDK and distribute it through the built-in marketplace model.
- Run multiple shops from a single deployment via `TENANCY_MODE` for an agency serving several clients.

## Product Opportunities
A base to fork/white-label as a niche vertical e-commerce product instead of building catalog/checkout/admin from scratch. The app-marketplace extensibility model is itself worth studying for any product considering third-party extensions.

## Agent / Automation Opportunities
None documented specifically for AI agents; the typed Storefront API and webhook system are generically automatable.

## Integration
Medium effort. Requires Node.js 22.22+, pnpm, and PostgreSQL; setup is `.env` config, migrations, then a browser-based setup wizard for the first admin/shop. No turnkey production-deployment guide beyond local dev instructions at review time.

## Architecture Notes
The out-of-process app SDK — extensions run separately from the core, authenticate via scoped API tokens, communicate via signed webhooks, and can be embedded in the admin UI — is the most reusable idea here. "No third-party code in the core" as an explicit design principle is a clean way to keep a core product stable while still supporting a marketplace.

## Maturity
Experimental. 362 stars, 49 forks, 13 subscribers, 2 open issues at review time; created 2025-03-14. Has CI, tagged releases (v0.8.0), a contributing guide, a security policy, and a roadmap doc, but no stated production-readiness claim.

## License
MIT. No restrictions on commercial or embedded use. No pricing, hosted-offering, or marketplace-fee terms are documented yet.

## Alternatives
- Medusa.js — mainstream headless commerce, larger ecosystem, already a known quantity.
- Saleor — Python/GraphQL stack, different ecosystem fit.
- Shopify itself — hosted, recurring cost; the thing this replaces for a self-hosted use case.

## Risks / Limitations
- First-run setup has an explicit security caveat: "anyone who reaches a fresh install can claim it" — setup must be completed immediately after deployment.
- No stated version/production-readiness beyond a roadmap doc; adoption evidence is thin despite a healthier star:subscriber ratio (~28:1) than most same-day "X alternative" repos checked this run.
- App marketplace review/governance process is undocumented.
- Production deployment guidance beyond local dev is not yet documented.

## Recommendation
PROTOTYPE — worth a local trial to evaluate the app SDK and Storefront API directly before considering it as a client-project base; not yet mature enough for a production commitment.

## Change History
### 2026-10-27
First reviewed. Surfaced via a "self-hosted alternative" GitHub search during slot 7 (experimental/hidden gems). Several same-day "X alternative" repos found in the same search (openma-ai/open-managed-agents, SourceWeft, Kabanya/Pomodoist) showed star:subscriber ratios of 150:1-340:1, consistent with the inflated-engagement pattern flagged in prior runs, and were excluded; Shopnex's ~28:1 ratio and pre-2026 creation date were the deciding factors for including it instead.
