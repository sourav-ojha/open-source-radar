# Universal Parcel Scraper

## Summary
A self-hosted parcel-tracking engine for 4,500+ carriers (1,059 dedicated adapters plus
5 universal fallbacks): give it a tracking number, it asks the carrier's own site
directly and returns the same normalized JSON for every carrier — no account, no API
key. It is the tracking engine behind the Peek delivery-tracker app, usable standalone as
a CLI, Node library, or HTTP server.

## Why I Should Care
A real, benchmarked alternative to paid parcel-tracking APIs (AfterShip, 17TRACK,
EasyPost): the project's own published benchmark on a 100-carrier European sample shows
95% coverage with all fallbacks enabled versus 73% for the best aggregator tested. Zero
account/API-key setup, self-hosted, Apache-2.0.

## Problems It Can Remove
Removes a per-lookup SaaS fee for shipment tracking and the long-tail maintenance burden
of writing/maintaining carrier-specific scraping logic by hand.

## Practical Uses
- Add shipment tracking to an e-commerce or fulfillment feature without a paid tracking
  API subscription.
- Normalize tracking data across many carriers into one JSON shape for a dashboard or
  notification pipeline.
- Offline-detect a carrier from a tracking number before deciding how to fetch its
  history.

## Product Opportunities
Embed as the tracking backend for a logistics/fulfillment feature inside a larger
product without a per-lookup SaaS fee attached.

## Agent / Automation Opportunities
Straightforward to wrap as a CLI tool or HTTP endpoint an agent calls; no native MCP
server found, but the clean CLI/library/HTTP interface makes wrapping one trivial.

## Integration
`npm install -g universal-parcel-scraper` for the CLI, or install as a Node library;
also runnable as an HTTP server. Low integration effort, no external dependencies beyond
Node.js 24+.

## Architecture Notes
Carrier detection is offline (pattern-matches the tracking number format); fetching
history is the only step that talks to the network, directly to the carrier's own
tracking page rather than a third-party aggregator — which is both the source of its
better coverage and the source of its ToS/fragility risk.

## Maturity
Emerging: created 2026-10-01 (~1.5 months), 16 stars, 7 forks, 0 subscribers. Published
to npm; active CI.

## License
Apache-2.0 (core). No restriction identified on commercial use or embedding.

## Alternatives
17TRACK API, Ship24 API, AfterShip, EasyPost, Postal Ninja — all paid or
account-gated; this is the only self-hosted, no-API-key option found with comparable
coverage.

## Risks / Limitations
- Very young (~1.5 months, 16 stars, 0 subscribers) — long-term maintenance of 1,000+
  carrier adapters at this team size is unproven.
- Scraping carrier sites directly (rather than using official APIs) carries ToS and
  breakage risk an official API would not; adapters can break when a carrier changes its
  site.

## Recommendation
USE NOW for a low-stakes integration (e.g. an internal tool or a feature where tracking
failures are non-critical) — but keep an eye on adapter breakage rates before trusting
it for anything customer-facing and critical-path.

## Change History
### 2026-11-22
Initial discovery and review (today's Small but High-Leverage Utility pick).
