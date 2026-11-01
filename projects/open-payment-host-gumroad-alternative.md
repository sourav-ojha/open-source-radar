# Open Payment Host

## Summary
Open Payment Host is a self-hosted, minimalist Go web app for selling digital products, subscriptions, and newsletters without paying a platform fee on top of payment-processing fees. It supports Stripe, PayPal, Razorpay, and Square, with an automatic per-country payment-gateway router, Listmonk/Mailchimp audience sync for buyers, and S3/Cloudflare R2 pre-signed-URL file delivery.

## Why I Should Care
This is a four-year-old, continuously-developed self-hosted Gumroad/Ko-fi/Buy-Me-a-Coffee alternative that the maintainer runs his own payments through. The per-country payment-gateway router (Razorpay for India, Stripe elsewhere, etc.) and the Listmonk/Mailchimp sync are genuinely differentiated features not found in other self-hosted alternatives in this niche reviewed to date.

## Problems It Can Remove
- A platform percentage fee stacked on top of card-processing fees for selling digital products
- Hand-building multi-gateway checkout (Stripe + PayPal + Razorpay + Square) with per-country routing
- Building a presigned-URL file-delivery pipeline for paid downloads
- Manually syncing paying customers into a newsletter/audience tool

## Practical Uses
- Sell digital files, one-time products, or subscriptions without a platform take rate
- Route checkout to a different payment gateway per customer country for parity pricing or redundancy
- Deliver paid files via pre-signed S3/R2 URLs
- Grow a newsletter audience automatically from paying customers

## Product Opportunities
- Self-hosted payments layer for a small digital-product or paid-content side business, independent of any platform's take rate
- The multi-gateway checkout pattern (drop in a price ID, everything else automatic) is worth studying as a reusable component even where this exact app isn't adopted

## Agent / Automation Opportunities
An experimental REST API & webhook surface exists; once it stabilizes it could let an agent provision or update products programmatically.

## Integration
Docker or a one-click Railway template; single Go binary + SQLite. No Kubernetes packaging, but doesn't need it for this scale. Integration effort: **Medium** (multiple payment-gateway credentials to wire up, no managed hosting path beyond Railway).

## Architecture Notes
- Go + HTMX + hyperscript + Tailwind + SQLite — a deliberately minimal stack, no SPA framework
- Payment-gateway router picks a gateway automatically based on the buyer's detected country
- File delivery via S3/Cloudflare R2 pre-signed URLs rather than proxying downloads through the app

## Maturity
Emerging, but with an unusually long track record for this niche: created November 2022, 243 stars, 12 forks, 4 subscribers, 0 open issues, pushed 2026-09-29, archived:false. The maintainer explicitly labels it work-in-progress ("possible unknown bugs") while stating he uses it in production for his own projects (payments.abishekmuthian.com). No tagged GitHub release exists despite a README version badge claiming "0.3.14" — that number is a static badge image, not a git tag.

## License
AGPL-3.0 — flagged loudly: fine to self-host and sell through, since that isn't redistributing modified source as a competing hosted service, but modifying the code and offering it as a hosted payments platform to third parties would trigger AGPL's source-disclosure requirement.

## Alternatives
Gumroad, Buy Me a Coffee, Ko-fi (all proprietary), Lemon Squeezy, Polar (proprietary). HonorBox (catalogued same run) is the zero-server alternative — no running service, narrower scope (GitHub-repo-delivered products only), no multi-gateway or newsletter-sync support.

## Risks / Limitations
- AGPL-3.0 — must be flagged for any commercial/hosted use beyond simple self-hosting
- Maintainer explicitly flags possible unknown bugs; not positioned as fully production-hardened
- API/webhook support marked experimental
- 0 open issues on a 243-star, 4-year-old repo is unusual — could reflect tight maintenance or simply light real-world usage pressure; unclear which from available signals
- No documentation site yet (listed as a pending goal in the README)

## Recommendation
PROTOTYPE — the multi-year track record and the maintainer's own production use are stronger maturity signals than most entrants in this niche, and the per-country gateway routing plus newsletter sync are real differentiators. Worth testing for a real digital-product sales need, with the AGPL implications and "WIP" admission kept in mind before trusting it with meaningful revenue.

## Change History
### 2026-11-01
First reviewed and catalogued. Discovered via GitHub search for "gumroad alternative self-hosted". Confirmed via GitHub API (AGPL-3.0, 243 stars, pushed 2026-09-29) and direct README inspection.
