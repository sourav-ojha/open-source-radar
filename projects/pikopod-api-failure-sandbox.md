# Pikopod

## Summary
A Go binary that builds a deterministic failure-injection sandbox from a third-party API's own OpenAPI spec (declines, timeouts, retry storms, duplicate webhooks) with no fixtures authored by hand, plus a fail-open recording proxy that fingerprints real production failures and turns them into permanent replayable regression scenarios, plus a spec-diff CI gate that fails a build on a breaking provider API change.

## Why I Should Care
A provider's own sandbox can't be asked to return a specific failure at a specific point in a state machine. Pikopod derives realistic failure archetypes directly from the provider's spec and can replay an exact production failure on demand — closing the gap between "tested happy path" and "what actually broke."

## Problems It Can Remove
Hand-authoring mock fixtures for every failure mode of every third-party integration; discovering a breaking third-party API change only after a deploy fails; re-creating a production incident's exact failure by hand to write a regression test for it.

## Practical Uses
- Testing payment/webhook retry and idempotency logic against realistic provider failure modes before it happens for real.
- Catching a breaking change in a third-party API's spec in CI before it reaches production (`pikopod spec-diff`).
- Turning an actual production incident (a specific error response) into a permanent, committed regression test via Observe → Reproduce.
- Building a resilience test suite for a Stripe/Razorpay-style integration without depending on the provider's own limited sandbox.

## Product Opportunities
Directly applicable as an internal QA tool for any product with payment/webhook integrations — relevant to the MSSP/PTaaS billing surface.

## Agent / Automation Opportunities
The CLI is fully scriptable; it could be wrapped as an MCP tool letting a coding agent generate resilience scenarios for a newly integrated third-party API.

## Integration
`go install github.com/pikopod/pikopod/cmd/pikopod@latest`, Homebrew tap, or a signed release binary (macOS/Linux, amd64/arm64, static, zero dependencies). CI integration documented for GitHub Actions.

## Architecture Notes
Three components: **Rehearse** (a deterministic sandbox built from the provider's spec, with failure archetypes — declines, timeouts, retry storms, duplicate webhooks — bound automatically), **Observe** (a fail-open proxy in front of the real provider that records failures and fingerprints them), and **Reproduce** (turns a fingerprint into a replayable scenario against the sandbox). Releases are cosign-signed with SLSA provenance.

## Maturity
Experimental. Created 2026-09-11 (~2.5 weeks old), 66 stars, pre-1.0 at v0.1.2. Several listed "contributors" are dependabot rather than humans.

## License
Apache-2.0, unrestricted.

## Alternatives
Hand-rolled fixture/mock files; mockd/WireMock-style general fault injection (not derived from the provider's own spec); the provider's own sandbox.

## Risks / Limitations
Very young project — treat as an experiment, not production-relied-upon tooling yet. Core claims ("eleven failure stories bind with nothing authored") are demonstrated in the README/demo but not yet independently validated at scale.

## Recommendation
PROTOTYPE, with an explicit youth caveat — the architecture and supply-chain hygiene (cosign + SLSA) are unusually mature for a 2.5-week-old project, but let it accumulate more real-world usage before depending on it for anything critical.

## Change History
### 2026-09-28
Initial discovery and review. Slot 6 run; found via GitHub Search API `topic:chaos-engineering+created:>2025-06-01` query.
