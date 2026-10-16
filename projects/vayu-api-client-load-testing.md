# vayu

## Summary
Open-source desktop API client (Postman-style request building and collections) with a
bundled native C++ load-testing engine, so request design and k6-level concurrent load
testing happen in one local app instead of two separate tools.

## Why I Should Care
The value here is workflow consolidation, not a new capability: if switching between a
request-design tool and a separate load-testing tool is real day-to-day friction, this
removes it. If it isn't, there's nothing else to gain.

## Problems It Can Remove
Context-switching between Postman/Insomnia/Bruno for request design and k6/Locust for
load testing when doing both on the same API.

## Practical Uses
- Design and save a request collection, then immediately load-test the same endpoints at
  concurrency without exporting/reconfiguring in a second tool
- Quick smoke-level load test of a staging endpoint before a launch

## Product Opportunities
None — this is a personal/team dev tool, not an embeddable component.

## Agent / Automation Opportunities
None identified — standard desktop GUI app, no CLI/API/MCP surface documented.

## Integration
Desktop app, per-OS installer. Medium integration effort (install + onboarding, no
scripting surface for CI use).

## Architecture Notes
The native C++ load-testing engine embedded inside an otherwise typical
Electron/web-stack-style desktop API client is the interesting implementation detail —
most tools in this space either skip load testing entirely or shell out to an existing
engine rather than embedding one directly.

## Maturity
Emerging. Created January 2026, actively released (v0.43.0 as of 2026-10-14). 42 open
issues against only 18 stars is worth checking before relying on it — could reflect
active roadmap tracking or unresolved rough edges.

## License
AGPL-3.0 — flagged loudly. Fine for local/internal use as a dev tool. Embedding vayu's
engine inside a hosted product would trigger AGPL's network-use source-disclosure
requirement; do not bundle it into a commercial SaaS offering without a separate
license.

## Alternatives
Postman, Insomnia, Bruno (request design only, no bundled load testing). k6, Locust,
goose (load testing only, Apache-2.0/MIT, no bundled request-design UI). vayu's pitch is
replacing the two-tool workflow, not improving on either tool individually.

## Risks / Limitations
- AGPL-3.0 licensing restricts commercial embedding
- 42 open issues vs. 18 stars — verify issue backlog isn't masking instability
- Early-stage, desktop-only distribution, no CI-friendly interface

## Recommendation
PROTOTYPE — only worth trying if the Postman+k6 context switch is an actual recurring
annoyance; otherwise skip in favor of the two best-in-class tools separately.

## Change History
### 2026-10-16
First discovery and review.
