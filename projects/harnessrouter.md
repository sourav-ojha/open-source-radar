# HarnessRouter

## Summary
Self-hosted infrastructure layer that turns coding-agent harnesses (Claude Code, Codex, Hermes, Pi, OpenCode, Cline, goose, Aider, and 20+ more) into interchangeable backends behind one API. Implements the Unified Harness Protocol (UHP), an open standard, with an OpenAI Responses-compatible surface that handles persistent sessions, streaming, files, artifacts, cancellation, and structured failure handling. The Community Edition is Apache-2.0 and fully self-hostable with a single Docker command; a managed Cloud edition exists alongside it.

## Why I Should Care
Building a product on top of "a coding agent" today means picking one harness and eating its integration surface, or building the same session/streaming/artifact plumbing again for every harness you want to support. HarnessRouter is exactly the kind of infrastructure that shortens "idea to shippable product" for anything in the micro-SaaS/dev-tooling space that wraps a coding agent — it is a direct hit on the "product-building-block" relevance axis, not just an AI-agent curiosity.

## Problems It Can Remove
Removes the one-integration-per-harness problem (the README's own framing: 4 harnesses need 36 integration responsibilities without it, growing to 45 with a fifth; one with it). Removes the need to hand-build session persistence, streaming, cancellation, and artifact handling for a custom agent product.

## Practical Uses
- Backend for a product that lets a team or customer pick their own coding-agent harness/model without the product rewriting its integration layer.
- Cost/latency arbitrage across harness × model combinations for the same task (the project publishes an 8-configuration benchmark showing up to 99.8% cost difference and 3.2x latency difference between best and worst for one task).
- A stable API surface to build internal tooling against, instead of coupling directly to one CLI's flags and output format.
- Starter-kit base for a new coding-agent product rather than building the harness-abstraction layer from zero.

## Product Opportunities
This is itself infrastructure for building a product — a harness-agnostic agent backend is a legitimate component of a commercial coding-assistant or dev-tooling SaaS, and the Community Edition's self-host path means it can be embedded without depending on HarnessRouter's own Cloud offering.

## Agent / Automation Opportunities
Everything here is agent orchestration: one API to run tasks, retrieve results, and switch harnesses. OpenAI Responses compatibility means existing OpenAI-SDK-based tooling can point at it directly.

## Integration
Single `docker run` command for self-hosted Community Edition; needs Docker, ~4GB disk, and your own model-provider API key — no HarnessRouter account required, no bundled trial key. First launch installs the enabled harness CLIs into the container. Low-to-medium integration effort: trivial to stand up, but wiring a real product against the UHP API is the real work.

## Architecture Notes
The Unified Harness Protocol is worth studying independent of this specific implementation — it's an attempt at an open standard for "any coding-agent harness as a backend," analogous to what LiteLLM did for model APIs but one layer up at the harness/session level. 31 harness bases supported at time of review.

## Maturity
Emerging. Created 2026-08-09 (about 3 months old), active daily release cadence (v0.41.21 at review, releasing almost daily), 488 forks against 30 subscribers — real fork activity even though the subscriber count is comparatively low.

## License
Apache-2.0 for the Community Edition (self-hosted, confirmed via LICENSE file). A separate managed Cloud edition exists; the self-hosted path has no license-based ceiling on commercial use.

## Alternatives
Hand-rolling per-harness integrations (the status quo this replaces); LiteLLM/Bifrost-style model gateways solve a different, lower-level problem (model API normalization, not harness/session abstraction). No direct open-source competitor at the harness-abstraction layer was found in this catalog.

## Risks / Limitations
Young project (3 months old) with no long-term stability track record; a near-daily release cadence at this stage could mean either healthy velocity or API churn — worth re-checking before building anything load-bearing on top of the UHP surface. The README's self-promotional "star this repo" banner is a mild hype signal, though it doesn't change the substance of the Docker-installable, Apache-2.0 self-hosted edition underneath it. 31 supported harness bases means broad but potentially shallow per-harness support — worth checking the support matrix for the specific harnesses actually in use before relying on full feature parity.

## Recommendation
PROTOTYPE — worth a trial if building anything that currently hard-codes a single coding-agent harness; too new to recommend as a production dependency yet.

## Change History
### 2026-11-18
Initial discovery and review. v0.41.21 at review time.
