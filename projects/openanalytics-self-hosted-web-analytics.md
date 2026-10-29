# OpenAnalytics

## Summary
A self-hostable, privacy-first, cookieless web analytics platform (Plausible/Fathom/Umami niche) built as a proper service architecture rather than a single container: separate tracker, ingest collector, ClickHouse-backed worker, control-plane API, a query gateway that is the only process allowed to read ClickHouse, a realtime SSE stream, a Next.js dashboard, and a CLI. Ships revenue attribution pulled from the operator's own Stripe account, an MCP server, and a hosted instance at getopen.so operated by the same authors.

## Why I Should Care
This replaces a recurring SaaS line item (Plausible/Fathom/PostHog analytics subscription) with something self-hostable under his own infra, and it's one of the few analytics tools that ties pageview/funnel data directly to revenue from his own Stripe account — useful for a micro-SaaS where "which page/referrer actually converts to paying" matters more than raw traffic. The MCP server means an agent can query his own product's analytics directly as part of an automated reporting or anomaly-triage workflow.

## Problems It Can Remove
A paid analytics subscription for a self-hosted product; building ad-hoc Stripe-to-traffic revenue attribution by hand; manually querying an analytics DB to answer "what changed" questions that an agent could answer via MCP.

## Practical Uses
- Self-hosted, cookieless analytics for a product's marketing site and app, avoiding GA4/Plausible-cloud recurring cost.
- Revenue-attributed funnels for a Stripe-billed SaaS product — which referrer/campaign actually produced paying customers, not just visits.
- Embeddable public share-link widgets for a status/stats page.
- MCP-driven "what happened to signups this week" queries from a coding agent instead of a manual dashboard login.
- CSV/JSON export/import for ad-hoc analysis outside the tool.

## Product Opportunities
Could be white-labeled or embedded as the analytics layer inside a product he builds for others, given the revenue-attribution angle is a specific sellable feature most self-hosted analytics tools lack; the AGPL license means this would need to stay as an internally-run service rather than a resold hosted tier (see License).

## Agent / Automation Opportunities
Ships its own MCP server as a first-class interface — an agent can query site stats, funnels, and revenue data directly without going through the dashboard UI, which fits directly into an automated weekly-report or anomaly-detection agent workflow.

## Integration
Not a single-container deploy — it's an 8-11 service pnpm monorepo (tracker, collector, worker, api, query-gateway, realtime, web, cli) backed by Postgres (control plane), ClickHouse (events/rollups), and two Valkey instances (durable queue + realtime cache). Third-party Railway templates document roughly 4GB RAM / 25GB disk for self-hosting, and a Neon-sponsored path exists for managed Postgres. Self-hosted or Docker Compose; a CLI (`oa`) handles site setup and device-flow login.

## Architecture Notes
CI-enforced architecture rules rather than conventions: the dashboard may only import a shared OpenAPI contracts package (the generated spec is the sole frontend/backend seam); ClickHouse is reachable only through the query gateway, which verifies Ed25519-signed query envelopes minted by the API — no service can query ClickHouse directly; the tracker snippet has a hard byte-size budget enforced in CI; visitor identity is a salted hash rotated nightly, bounding a visitor's trail to roughly one day rather than being permanently linkable. This is a genuinely more rigorous internal architecture than most self-hosted analytics tools in this niche.

## Maturity
Emerging. v0.8.0 released 2026-09-19, created 2026-08-11, 643 stars / 53 forks (healthy ratio) / 5 open issues. Backed by the Vercel OSS Program badge and a Neon Open Source Program sponsorship for the managed-Postgres self-host path — both named-sponsor signals rather than self-reported traction. Independently corroborated: third-party Railway deploy templates and an MCP marketplace listing exist for it, consistent with real external adoption rather than a marketing-only repo.

## License
AGPL-3.0 — **flagged loudly per policy.** Self-hosting internally for his own product's analytics is unrestricted. Offering a modified version of OpenAnalytics itself as a hosted analytics service to third parties would trigger AGPL's source-disclosure requirement; the authors already run exactly that model themselves at getopen.so, so the license is clearly intentional on their part, not an oversight to route around.

## Alternatives
Plausible, Fathom, Umami (established, cookieless, but none tie directly into the operator's own Stripe account for revenue attribution or ship an MCP server); PostHog (far broader product-analytics surface, heavier to self-host, not primarily privacy/cookieless-first).

## Risks / Limitations
Multi-service architecture (Postgres + ClickHouse + 2× Valkey + 5-6 app services) is meaningfully more operational surface than a single-binary analytics tool — not a weekend side-project deploy. AGPL means any resale-as-a-service plan needs its own source-disclosure compliance path. Young project (created mid-August); evaluate self-host stability over a longer window before relying on it for anything revenue-critical.

## Recommendation
PROTOTYPE. Worth standing up for a real product's analytics given the revenue-attribution and MCP angle, but budget for the multi-service ops overhead and treat the AGPL implication as a real constraint if any hosted-resale plan is ever considered.

## Change History
### 2026-10-29
Initial discovery and review. Slot 2 run (product infrastructure, APIs, backend components). Found via GitHub Search API `topic:analytics created:>2026-08-01 topic:self-hosted`. Vercel OSS Program and Neon sponsorship claims verified via README badges; third-party Railway templates and MCP marketplace listing found via web search as independent corroboration of real adoption.
