# Agent Console

## Summary
Local-first observability for AI coding agents: reads Claude Code's and Codex's own
transcripts on disk and shows tokens, cache reads/writes, models used, and estimated
list-price cost per session — on one machine, or joined across every laptop/build-box/
server a small team connects to a self-hosted "team hub." Optional OpenTelemetry/Kong/
LiteLLM ingest, a Prometheus endpoint, and a Grafana dashboard.

## Why I Should Care
This is a FinOps angle on coding-agent usage rather than the debugging/security angle
already covered by [[agentsight-ai-agent-observability]], Headroom, and SIMURG in the
catalog: it answers "how much is agent-assisted development actually costing, per
person, per machine, per week" — directly relevant for someone running or advising on an
MSSP/PTaaS operation where cost accountability across a small team matters, and where
Claude Code usage is already heavy.

## Problems It Can Remove
Guessing at agent API spend from provider dashboards that don't break usage down by
project or person; manually correlating "which session cost how much" across multiple
developer machines.

## Practical Uses
- Track personal Claude Code/Codex token and cost usage locally without sending any data
  to a third party.
- Stand up the self-hosted team hub if working with contractors or a small team on the
  MSSP/PTaaS side, to see aggregate agent spend without each person self-reporting.
- Feed the Prometheus/Grafana output into existing observability infra rather than
  adopting a new dashboard in isolation.
- "Presenting mode" to demo agent usage/cost to a client or stakeholder without leaking
  real project/machine/person names.

## Product Opportunities
None directly for embedding in a product — this is an internal tool. The presenting-mode
and multi-tenant-hub design pattern (redact identifiers for demos, join untrusted
machines without double-counting) is a reasonable reference if building any usage-
metering feature into a SaaS product.

## Agent / Automation Opportunities
Reads agent transcripts directly (no agent-side integration needed for the local case);
optionally ingests Claude Code's own OpenTelemetry stream and gateway-level metrics
(Kong, LiteLLM) for a fuller picture in a gateway-fronted setup.

## Integration
Distributed as signed/notarized native builds (macOS) plus presumably other platforms;
self-hosted team hub requires standing up and connecting machines to it. Medium effort
for the team-hub case, low for solo local use. No telemetry, no update check, no crash
reporting by the tool's own design.

## Architecture Notes
Notably rigorous supply-chain posture for a 10-day-old project: CI, OpenSSF Scorecard
and Best Practices badges, Apple code-signing and notarization, SHA256SUMS, signed build
attestations, and a CycloneDX SBOM per release from v0.4.1 — plus a documented threat
model and NIST SSDF mapping. That level of security hygiene this early is itself a
positive maturity signal independent of the feature set.

## Maturity
Experimental by age (created 2026-09-20, ~10 days old at review) but unusually mature in
engineering practice for that age. 714 stars / 132 forks in 10 days — a higher fork
ratio (~18.5%) than typical organic growth for a tool this young; not flagged as
manipulated (no history to compare against, and the project's own transparency around
its supply chain cuts against a pure hype-farming read), but worth re-checking star/fork
trajectory on a future run before treating the number as a strong signal either way.

## License
MIT.

## Alternatives
[[agentsight-ai-agent-observability]] (eBPF-based agent activity/security monitoring),
Headroom and SIMURG (LLM observability), mcpsnoop (MCP-level debugging) — all already
catalogued, but none of them frame the problem as team-wide cost accounting across
machines with a presenting mode built for showing the numbers to someone else.

## Risks / Limitations
Only 2 visible contributors; too young to know if the self-hosted team-hub feature holds
up under real multi-person use. Cost figures are list-price estimates, not actual billed
amounts — useful for relative comparison, not for reconciling an invoice.

## Recommendation
PROTOTYPE — worth running locally for a week to see actual Claude Code/Codex cost
patterns; the team-hub feature is a bigger commitment and should wait for more track
record or a specific need (e.g., a PTaaS engagement with multiple contractors on agent-
assisted work).

## Change History
### 2026-09-30
Initial discovery and review.
