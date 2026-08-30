# Nona

## Summary
Self-hosted feature flags and remote configuration, positioned as an open-source Firebase Remote Config alternative. One Docker image, one HTTP API (no SDK required in any language), with official client libraries for npm and NuGet and a CLI distributed via npm/NuGet/Chocolatey. Targets web, mobile (iOS/Android/React Native/Flutter), and backend, with kill switches and a documented Firebase migration path.

## Why I Should Care
Feature flags and remote config are exactly the kind of product-building-block component worth not rebuilding per project — and this is narrow and simple enough (one Docker image, plain REST) to actually self-host rather than adopting a heavier platform.

## Problems It Can Remove
- Dependency on Firebase Remote Config (Google account, platform lock-in, mobile SDK requirement)
- Ad hoc environment-variable feature flags that require a redeploy to change
- Paying for LaunchDarkly-style SaaS for basic flag/kill-switch needs

## Practical Uses
- Toggle features and run kill switches on a Node/React product without redeploying
- Push mobile app config changes (React Native) without an app-store release
- Fetch config from any language/runtime via plain HTTP, no SDK lock-in

## Product Opportunities
Self-hostable on existing AWS infra (EC2/ECS) as the feature-flag/config backbone for personal products, and potentially embeddable for MSSP clients who need config management without adopting a heavier platform.

## Agent / Automation Opportunities
Plain REST API with no SDK requirement makes it trivial to drive from a script or agent tool — flip flags, read config, or wire kill switches into an incident-response automation.

## Integration
Single Docker image (`rywaredev/nona` on Docker Hub) plus Docker Compose or Kubernetes deployment; official npm (`nona-client`) and NuGet clients. A live demo (`demo.nonaconfig.com`, resets nightly, no signup) is a fast way to evaluate before self-hosting.

## Architecture Notes
Deliberately narrower than general-purpose feature-flag platforms (Unleash, GrowthBook) — one image, one API, explicit focus on being a Firebase Remote Config replacement rather than a full experimentation/A-B-testing platform.

## Maturity
Emerging: created December 2025, 62 stars, active release cadence (latest CLI tag 2026-08-18, pushed 2026-08-27). Early-stage relative to established players like Unleash/GrowthBook.

## License
Apache-2.0. No restrictions on commercial/SaaS use, redistribution, or embedding.

## Alternatives
Firebase Remote Config, LaunchDarkly (SaaS), Unleash, GrowthBook, ConfigCat.

## Risks / Limitations
- Small maintainer footprint relative to established feature-flag platforms
- No independent security audit found; self-hosting means the operator owns API-key handling
- Early-stage — worth confirming the release cadence holds before depending on it for anything client-facing

## Recommendation
PROTOTYPE — try the live demo first, then a small self-hosted pilot for one internal project before committing.

## Change History
### 2026-08-30
Initial discovery and review.
