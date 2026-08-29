# Weir

## Summary
A CI gate for AI agents: reads the OpenTelemetry traces an agent already emits, reconstructs the session graph, tracks taint through it, and fails the build if sensitive data reached a sink it should never have reached.

## Why I Should Care
Nobody else is turning 'does my agent leak data' into a structural, automatable CI check off traces the agent already emits — this is a narrow problem solved unusually well.

## Problems It Can Remove
Taint-tracking through a reconstructed session graph is a non-trivial static-analysis problem; this exists and is free to try before building an equivalent.

## Practical Uses
- CI gate on any agent workflow that touches customer data, verifying no sensitive-data leak path exists before merge
- Directly applicable to PTaaS/security-tooling work — automated proof that an agent didn't exfiltrate scan data through an unintended sink
- Cheap due-diligence check before shipping a new agent tool call in any product

## Product Opportunities
- A packaged version of this pattern (taint-tracking over agent traces) is a plausible standalone security-tooling product, directly adjacent to the PTaaS partnership

## Agent / Automation Opportunities
- This IS agent tooling — a CI-native tool, not something to wrap further

## Integration
Deployment: pip package, CI step
Interfaces: CLI, library (Python)
Integration effort: **Low**

## Maturity
Experimental. GitHub API verified: Apache-2.0, 9 stars, pushed_at 2026-08-25. Hidden-gem candidate: high relevance to PTaaS/security work despite tiny star count.

## License
Apache-2.0. Apache-2.0 — permissive, safe to embed or run internally.

## Alternatives
- Manual trace review
- generic OTel + custom alerting rules (build-it-yourself, weeks of work)

## Risks / Limitations
- 9 stars, very early (first meaningful activity within the last week) — expect rough edges and API churn
- Single maintainer, unproven at scale
- Requires the agent to already emit OTel traces — not zero-integration

## Recommendation
**WATCH** (score 6.9/10) — see Why I Should Care above.

## Change History
### 2026-08-29
First discovered and reviewed. Verified via GitHub API: license Apache-2.0, current version see PyPI (weir-scan) (unclear — check https://pypi.org/project/weir-scan/ directly).
