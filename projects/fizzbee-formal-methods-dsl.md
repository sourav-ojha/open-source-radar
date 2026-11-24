# Fizzbee

## Summary
A formal specification language and model checker for distributed systems, pitched explicitly as "the easiest-ever formal methods language" — a lower-ceremony alternative to TLA+ for modeling microservices, consensus protocols, and cloud application designs before writing production code. Has an online playground (no install needed) plus a downloadable binary model checker.

## Why I Should Care
Distributed-systems and microservice design bugs (race conditions, split-brain scenarios, incorrect retry/idempotency logic) are expensive to find after the fact and historically required learning TLA+ or Alloy, both with a steep learning curve that keeps most engineers from ever using formal methods. Fizzbee's explicit goal is making that technique approachable enough to actually use when designing an AWS-based distributed system (queues, Lambda orchestration, multi-service workflows) before committing to an implementation.

## Problems It Can Remove
Catching concurrency/ordering design bugs on paper (in a model) instead of in production, for systems built from AWS primitives like SQS, Lambda, and multi-service workflows where interaction-order bugs are easy to miss by inspection alone.

## Practical Uses
- Model a multi-step distributed workflow (e.g., an order-processing pipeline across Lambda + SQS + a database) before building it, to check invariants like "no order is ever processed twice" hold under all interleavings.
- Use the published pattern library (two-phase commit, saga, leader election) as a reference when designing a new distributed feature, even without writing a full spec.
- Verify idempotency and retry-safety assumptions for a webhook or queue-consumer design.

## Product Opportunities
None directly — this is a design/verification tool, not an embeddable component.

## Agent / Automation Opportunities
None found (no MCP server, no agent-facing API) — purely a standalone language and model checker today.

## Integration
```sh
brew tap fizzbee-io/fizzbee
brew install fizzbee
```
Prebuilt binaries for macOS/Linux; no install needed to try it via the online playground at fizzbee.io/play. Integration effort: **Medium** — not a code-level integration at all; the cost is learning the specification language, which is explicitly designed to be lower-effort than TLA+.

## Architecture Notes
The core idea worth studying independent of adoption: formal methods made approachable through a higher-level, more conventional-looking syntax than TLA+'s math notation, while still doing real state-space exploration and invariant checking underneath.

## Maturity
Emerging-to-mature for its niche. Created 2024-03-26 (2.5 years old), actively maintained (pushed 2026-11-23), latest release v0.5.4 (2026-10-01). 383 stars / 23 forks / 3 subscribers / 39 open issues. Modest community size, but the multi-year history, real documentation site, tutorials, and pattern library indicate a genuine sustained project rather than a weekend experiment.

## License
Apache-2.0. No commercial-use restrictions.

## Alternatives
TLA+ (the established standard, but with a much steeper learning curve and Java-based tooling); Alloy (similarly steep); P (Microsoft's language for distributed protocols, also non-trivial to learn). Fizzbee's distinguishing claim is approachability — whether that claim holds for someone without a formal-methods background would need a hands-on trial to confirm.

## Risks / Limitations
- Small community (3 subscribers) despite the multi-year history — adoption outside niche formal-methods circles appears limited so far.
- Learning curve, while lower than TLA+, is still real — this is not a tool with immediate payoff; value compounds only with deliberate use on genuinely tricky designs.
- No agent/API surface — purely a human-driven design tool today.

## Recommendation
STUDY — the architecture and approach (making formal methods usable) is worth learning from, and worth a trial on one genuinely tricky distributed-design problem, but this is not a tool to adopt broadly without a specific concurrency-bug-prone design to point it at.

## Change History
### 2026-11-24
First seen and reviewed. Discovered via GitHub search for property-based-testing/formal-methods topic tags. Verified license and release history via the GitHub API.
