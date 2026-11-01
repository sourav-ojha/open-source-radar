# RefearnApp

## Summary
RefearnApp is a self-hostable affiliate/referral tracking platform positioned as an open-source alternative to Rewardful, FirstPromoter, and Tolt: click/conversion tracking on the Cloudflare Workers edge network, Stripe & Paddle integrations, PayPal mass payouts, coupon-code tracking, and an affiliate-facing portal. It's built as a Next.js + Postgres (Drizzle ORM) + Upstash Redis pnpm monorepo.

## Why I Should Care
Referral-driven growth is a cheap, high-leverage channel for a small paid product, and the proprietary alternatives (Rewardful, FirstPromoter, Tolt) all charge recurring fees that scale with referral volume. This is the first self-hostable, Next.js/Cloudflare-native entrant in this niche found to date, and its stack matches tooling already in use rather than requiring a new ecosystem to adopt.

## Problems It Can Remove
- A recurring per-referral or monthly SaaS fee for running an affiliate/referral program
- Hand-building click/conversion attribution, coupon-code matching, and payout logic

## Practical Uses
- Run a referral/affiliate program for a product without a third-party platform fee
- Track clicks and conversions at the edge for near-zero added latency on referral links
- Pay out affiliates via PayPal mass payouts or Stripe/Paddle instead of handling payouts manually
- Run coupon-code-based attribution for a launch or affiliate campaign

## Product Opportunities
Bolt a referral program onto an existing or future micro-SaaS product without building click/conversion tracking and payout logic from scratch.

## Agent / Automation Opportunities
None identified beyond the standard dashboard/API surface at review time.

## Integration
Docker is the primary deployment path; the tracking layer runs on Cloudflare Workers, so a Cloudflare account is required alongside self-hosted Postgres and Upstash Redis. Integration effort: **Medium** — more moving parts than a single-container self-hosted app.

## Architecture Notes
- Next.js + React dashboard, Tailwind CSS
- Tracking layer runs on Cloudflare Workers (edge compute) rather than the main app server
- Upstash Redis for caching/queuing, Postgres via Drizzle ORM, pnpm workspaces monorepo

## Maturity
Emerging. Created March 2025, 47 stars, 17 forks (an unusually high fork:star ratio for this catalog — more often a sign of genuine developer engagement than the inflated-engagement pattern flagged elsewhere, where fork counts run low relative to stars), 1 subscriber, 9 open issues, pushed 2026-10-29. Listed on Product Hunt with an active Discord. The latest GitHub tag (v1.4.0, 2026-05-16) is five months stale relative to the last push — the project isn't cutting releases in step with its own development, so the tagged version understates what's actually in `main`.

## License
AGPL-3.0 on the self-hosted edition; the README states all features are free and unlocked for self-hosters. A paid "Managed Cloud" tier is sold separately as the vendor's own hosted offering, not a feature gate on the open-source code. AGPL implications must be flagged for any commercial/hosted redistribution.

## Alternatives
Rewardful, FirstPromoter, Tolt — all proprietary, priced per-referral or per-month.

## Risks / Limitations
- AGPL-3.0 — flag for commercial/hosted redistribution
- Release tagging is stale (5 months behind `main`); don't rely on the latest tag as a true maturity signal
- README leads with a "Launch Special / Lifetime Access" upsell for the managed cloud tier that reads like a landing page — worth confirming the self-hosted docs are equally complete before committing
- Only 47 stars / 1 subscriber at review time; real but limited independent validation
- Requires a Cloudflare account for the edge tracking layer, adding an external dependency beyond the self-hosted stack itself

## Recommendation
PROTOTYPE — worth testing for a real referral-program need given the stack fit and the absence of any other self-hostable entrant in this specific niche, but confirm the actual state of `main` (given the stale release tag) before committing rather than trusting the latest tagged version.

## Change History
### 2026-11-01
First reviewed and catalogued. Discovered via GitHub search for "affiliate tracking self-hosted open source". Confirmed via GitHub API (AGPL-3.0, 47 stars, pushed 2026-10-29) and direct README inspection.
