# OpenKruise Agents

## Summary
A CNCF OpenKruise sub-project providing Kubernetes-native lifecycle management for AI-agent sandbox workloads: isolated tool-use environments, persistent research/dev-workspace sandboxes, and RL training environments with hibernation/checkpointing (memory, filesystem, GPU memory) and resource pooling. Exposes both a Kubernetes CRD API and an E2B-compatible SDK, so code already written against the E2B sandbox API can target a self-hosted cluster instead.

## Why I Should Care
It's the one credibly-backed, operationally mature entrant found inside today's otherwise heavily saturated "agent sandbox" wave of near-identical repos — and it directly targets a real paid-SaaS dependency (E2B) with an API-compatible self-hosted alternative, for anyone already running Kubernetes.

## Problems It Can Remove
Removes the recurring cost and vendor lock-in of a hosted agent-sandbox SaaS (E2B) for teams willing to run the sandboxes on their own Kubernetes cluster, including AWS EKS.

## Practical Uses
- Run LLM code-interpreter sandboxes on an existing EKS cluster instead of paying E2B per sandbox-hour.
- Host persistent, checkpointable dev-workspace sandboxes for coding agents.
- Provision RL training environments needing fast startup and fault tolerance.
- Swap E2B-targeting agent code to a self-hosted backend with minimal changes, via the E2B-compatible SDK.

## Product Opportunities
None identified directly — this is infrastructure, not a product-facing component, unless a future product specifically needs to offer sandboxed code execution to its own customers.

## Agent / Automation Opportunities
Any tool currently built against the E2B sandbox API is a drop-in candidate to repoint at a self-hosted OpenKruise Agents cluster.

## Integration
Requires a Kubernetes cluster with OpenKruise installed; CRD-based configuration, not a quick drop-in. High effort, and only sensible if already running or planning to run K8s (e.g. EKS) for agent workloads — overkill otherwise.

## Architecture Notes
Checkpoint/hibernate semantics covering GPU memory and filesystem read/write layers, combined with resource pooling and dynamic resizing, plus an identity/session layer that minimizes dependence on Kubernetes Services for traffic routing. The E2B API compatibility layer is the notable design choice — it turns this into an infra-swap rather than a new API to learn.

## Maturity
Emerging. Created November 2025, current release v0.6.0 (2026-10-09). Sits under the established CNCF OpenKruise project (in production at Alibaba-scale for years), which gives it credibility beyond its own metrics. 300 stars, 146 forks, 2 watchers — a skewed ratio for a sub-project, treated as plausible given the verified CNCF/org backing and public documentation on openkruise.io, not independently re-verified further.

## License
Apache-2.0 — no restriction.

## Alternatives
E2B Cloud (paid SaaS), Daytona, DIY Firecracker/Kata-based sandboxing, Modal.

## Risks / Limitations
High integration effort — not worth adopting without an existing Kubernetes commitment for agent workloads. Early (pre-1.0 CRD surface may still change). Star/fork/watcher ratio is unusual and worth re-checking at the next review.

## Recommendation
STUDY — the E2B-compatibility and checkpoint/hibernate architecture is worth knowing about; actual adoption only makes sense once running real Kubernetes-based agent-sandbox workloads.

## Change History
### 2026-10-20
First reviewed and catalogued, during slot 7 (experimental / hidden gems) discovery, after a same-day wave of ~30 near-identical "agent sandbox" repos surfaced via GitHub search. This was the only one judged credibly differentiated.
