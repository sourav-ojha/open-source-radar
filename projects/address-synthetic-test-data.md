# Address

## Summary
Address is a self-hosted, Dockerized Node.js 24 / PostgreSQL 16 service that generates realistic, real addresses — not randomly assembled strings — for 27 countries and regions, sourced from official address registries, open map data, and validated geocoding. Every record carries its source, precision, and coordinates; missing fields stay empty rather than being invented. It ships an admin console and a bearer-authenticated JSON API for single and batch generation, in 10 display languages.

## Why I Should Care
Faker-style address generators produce plausible-looking but structurally fake addresses that break real-world validation (postcode format, administrative hierarchy) in checkout and onboarding flows. Address solves that specific, recurring annoyance, and it's built on exactly the Node/Postgres stack already in use.

## Problems It Can Remove
- Removes test addresses that fail real-world format/postcode validation.
- Removes manually sourcing and normalizing open-data address registries per country.
- Removes ad hoc use of paid geocoding APIs just to get a batch of plausible test addresses.

## Practical Uses
- Seeding checkout/form QA fixtures with realistic international addresses.
- Address-format validation testing across 27 countries.
- Logistics/shipping demo data that needs to look and geocode like a real address.
- Populating a staging database with location-realistic test users for a product shipping internationally.

## Product Opportunities
None directly — this is a developer/QA utility, not an embeddable product component.

## Agent / Automation Opportunities
None identified — straightforward API/CLI utility, no agent-specific hooks.

## Integration
Docker image (`daimon23/address`) or self-hosted Node.js 24 + PostgreSQL 16. Bearer-token JSON API for programmatic single/batch generation. Integration effort: **Low**.

## Architecture Notes
Incremental, source-attributed sync from official address registries and open map data, with bounded retries, exponential backoff, and quota/source-exhaustion detection so failing imports aren't repeated indefinitely.

## Maturity
Emerging — created 2026-07-15 (~4 months old), 2,293 stars, 337 forks, 3 subscribers, 1 open issue. The stars:subscribers ratio (~764:1) is elevated even accounting for real fork activity; worth rechecking after a few more months for signs of sustained, organic usage.

## License
MIT, confirmed via GitHub API.

## Alternatives
Faker.js/faker-style libraries (randomly assembled, not registry-validated); ad hoc use of Google/Mapbox geocoding APIs (paid, no batch synthetic-generation workflow).

## Risks / Limitations
- Elevated star:subscriber ratio for a 4-month-old project — monitor for sustained real engagement.
- Long-term maintenance of 27 countries' worth of data-sync pipelines is unproven this early.
- Very low open-issue count (1) against 337 forks is unusually quiet — could indicate stability or low real usage.

## Recommendation
PROTOTYPE — directly on-stack (Node/Postgres/Docker) and solves a real, recurring QA annoyance; worth trying the next time international address test data is needed, with the star/subscriber ratio as the one thing to keep an eye on.

## Change History
### 2026-11-10
Initial discovery and review. Today's Small but High-Leverage Utility, slot 7 (experimental projects and hidden gems) run.
