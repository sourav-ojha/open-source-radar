# Radar (Skyhook)

## Summary
Radar is an open-source Kubernetes UI distributed as a single Go binary, a `kubectl` plugin, or a desktop app, with an MCP server on by default (sharing the UI's port) that exposes the same cluster data — topology, ranked issues, event/change timeline with diffs, Helm state, GitOps (Argo CD/Flux) status, live service-to-service traffic, and a 44-check audit — to any MCP-aware AI coding agent already on the machine. Note: unrelated to this radar's own project name; this is specifically `skyhook-io/radar`.

## Why I Should Care
Sourav runs EKS, and the usual loop when something breaks is "run kubectl, copy the output, paste it to the agent, repeat." Radar replaces that with one maintained, live view that both a human and an agent query directly — zero in-cluster install for local use, and the MCP surface means an agent can investigate a broken deployment itself instead of being spoon-fed terminal output.

## Problems It Can Remove
- Manually correlating `kubectl describe`/`get events`/`logs` output into a mental model of what's actually broken
- Re-explaining cluster state to an AI agent every session instead of letting it query a live topology/timeline directly
- Standing up a separate dashboard for live service-to-service traffic on a cluster that already runs Istio/Hubble

## Practical Uses
- Point it at an EKS kubeconfig locally for a ranked, investigate-able issue queue instead of raw kubectl output
- Hand Claude Code/Codex/Cursor the read-only MCP endpoint so it diagnoses a broken deployment on its own
- Diff Helm release values/manifests across revisions before deciding to roll back
- Run the 44-check security/reliability/efficiency audit against a cluster ahead of a client-facing MSSP review
- Visualize live traffic on a cluster already running Hubble/Istio/Caretta/Beyla without a second dashboard

## Product Opportunities
Could be the Kubernetes-visibility layer bundled into an internal platform/ops product for EKS-based clients, with the write-mode MCP tools (restart, scale, apply, rollback, node drain) gated behind an approval step.

## Agent / Automation Opportunities
MCP server is on by default on the UI's port, no separate deployment or API key required. Read-only tools (`/mcp-readonly`) cover topology, health issues, deduplicated events, recent changes with diffs, filtered logs, Helm, audit, upgrade readiness, RBAC, cost, and Prometheus. Write tools (`/mcp`) add restart/scale/apply/rollback/GitOps-sync/node-drain, explicitly marked destructive so a client can require confirmation. Secret values are withheld and known secret patterns in env vars/Helm values/logs are scrubbed before being handed to an agent — the README itself notes this can't catch everything. `--no-mcp` and `--mcp-read-only` flags disable or restrict the surface entirely.

## Integration
Single binary, `kubectl` plugin, or desktop app using the local kubeconfig — nothing installed in the cluster, no CRDs, no in-cluster agents, for local/laptop use. For shared team access it can also be deployed in-cluster via Helm, with OIDC auth/auth-proxy support and per-user Kubernetes RBAC. Integration effort: **Low** for the local/individual use case; the in-cluster shared mode has more moving parts but is still Helm-based.

## Architecture Notes
- MCP tools are split read-only vs. read-write by endpoint path (`/mcp-readonly` vs `/mcp`), not by a single toggle — makes it straightforward to expose only the read-only surface to an agent by default
- Event/change timeline keeps history in memory by default, with SQLite or Postgres as upgrade paths for persistence
- Live traffic view is a thin layer over existing service-mesh/eBPF telemetry (Hubble, Istio, Caretta, Beyla) rather than its own instrumentation
- `exec_pod` (shell into a running pod) is opt-in, separate from the rest of the MCP surface

## Maturity
Emerging but fast-moving: created 2026-01-20 (~10 months old), 4,071 stars, 269 forks, 13 subscribers, 103 open issues, v1.20.2 released today (2026-11-09) — frequent, active releases. Skyhook is a YC W23 company. The README cites tests on "tens of thousands of pods" and a benchmark against SREGym faults where Radar-assisted agents diagnosed faster than raw kubectl — the vendor's own results, treat as directional rather than independently verified.

## License
Apache-2.0, confirmed via GitHub API. No restrictions on commercial or internal use. Skyhook separately sells "Radar Cloud," a hosted multi-cluster offering with fleet views and alerting — the self-hosted core is presented as fully featured, not a crippled free tier, and usage reporting is opt-in with cluster data staying local by default.

## Alternatives
Lens, K9s, Octant, Headlamp — all visual-only Kubernetes UIs with no built-in MCP/agent surface. Raw kubectl plus an agent's shell access works but forces the agent to rebuild cluster context from scratch every session rather than querying a maintained topology/timeline. Radar Cloud itself (Skyhook's hosted multi-cluster tier) is out of scope here — not evaluated for a single-cluster/local use case.

## Risks / Limitations
- Star:subscriber ratio (4,071:13 ≈ 313:1) is elevated versus the healthiest repos tracked in this radar, though 269 real forks and 103 open issues argue against pure inflation
- Young project/company (~10 months) despite rapid growth — benchmark claims are vendor-reported
- Open-core model (Radar Cloud as a separate paid product) is fine today but worth watching for future feature gating on the OSS core
- Shared in-cluster mode (Helm + OIDC + RBAC) is less documented in depth than the single-user local mode

## Recommendation
USE NOW — trivial to try (single binary, zero cluster footprint) against a real EKS cluster, with a built-in MCP server that's directly useful for handing cluster diagnosis to a coding agent rather than pasting kubectl output by hand.

## Change History
### 2026-11-09
First reviewed and catalogued. Slot 6 (infrastructure, observability, deployment) run. Confirmed via GitHub API (Apache-2.0, 4,071 stars, pushed 2026-11-09, v1.20.2) and README inspection (raw.githubusercontent.com) covering MCP server behavior, deployment model, and feature set.
