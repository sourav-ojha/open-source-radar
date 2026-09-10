# Openpanel

## Summary
Openpanel is a self-hostable, open-source web and product analytics platform positioned
as an alternative to Mixpanel, covering event tracking, user-behavior analytics, and
dashboards.

## Why I Should Care
Product analytics (Mixpanel, Amplitude, PostHog Cloud) is a recurring SaaS line item for
any product with a paying customer base, including an MSSP admin-portal SaaS. A mature,
actively developed self-hosted alternative directly reduces that recurring cost for
internal or lower-volume products, at the price of running and maintaining the stack
yourself.

## Problems It Can Remove
- Mixpanel/Amplitude subscription cost for product usage analytics on internally-run or
  lower-traffic products.
- Sending customer usage data to a third-party analytics vendor when data residency or
  privacy commitments (relevant to an MSSP/security-focused product) make that
  undesirable.

## Practical Uses
- Self-host product analytics for the admin-portal SaaS or PTaaS partnership tooling
  instead of paying for a hosted analytics vendor.
- Instrument a side project or early-stage product with event tracking without a
  per-event billing model.
- Use as an internal dashboard for feature-adoption tracking across product surfaces.

## Product Opportunities
- Could be embedded/white-labeled as the analytics layer inside a commercial product,
  subject to AGPL's network-copyleft terms (see License below) — needs a compliance
  read before doing this for a paid, closed-source product.

## Agent / Automation Opportunities
- No agent-specific surface identified; relevant purely as product infrastructure.

## Integration
Self-hosted via Docker; multi-service stack (dashboard, ingestion API, worker,
database) rather than a single binary. Integration effort: **Medium** — more moving
parts to operate than a single-container tool, and instrumenting an existing product
with its event-tracking SDK is nontrivial work regardless of which analytics platform is
chosen.

## Architecture Notes
Not deeply inspected this run beyond confirming the license and maturity; worth a closer
architecture read (ingestion pipeline, storage engine choice) before a deploy decision if
adopted.

## Maturity
Mature and active: 6,909 stars, created 2024-02-29, pushed_at 2026-09-04, 47 open issues
(healthy, actively triaged backlog). Established enough to be a credible Mixpanel
alternative rather than an early-stage experiment.

## License
**AGPL-3.0 — flagged loudly.** Confirmed via the repository's LICENSE.md file (standard
GNU AGPLv3 text, no dual-licensing or open-core carve-out found in the license file
itself). Self-hosting internally is unrestricted. Embedding it into a commercial,
closed-source hosted product would trigger AGPL's network-copyleft obligations —
get explicit legal confirmation before doing that for the MSSP SaaS or any paid product,
rather than assuming "self-hosted" implies unrestricted commercial use.

## Alternatives
Mixpanel, Amplitude, PostHog (Cloud or self-hosted, not independently reviewed this
run). openreplay/openreplay (12,824 stars, session replay + product analytics, also
self-hostable) was surfaced in the same search but has a more fragmented mixed license
(AGPL by default, MIT for some directories, proprietary "ee/" directory) and is already
a well-established name in the self-hosted observability space — not reviewed as a fresh
discovery this run for that reason.

## Risks / Limitations
- AGPL-3.0 is the primary adoption risk for anything beyond pure internal use — see
  License above.
- Multi-service deployment (vs. a single binary) raises the operational maintenance
  burden relative to lighter-weight tools catalogued elsewhere.

## Recommendation
PROTOTYPE — self-host it for an internal or non-commercial-facing analytics need first;
get explicit legal clarity on AGPL implications before using it as the analytics layer
inside any paid, closed-source product surface.

## Change History
### 2026-09-10
Initial discovery and review. Slot 2 (product infrastructure, APIs, backend components) run.
