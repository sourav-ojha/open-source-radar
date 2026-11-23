# kwatch

## Summary
Lightweight, rule-based (no LLM) Kubernetes failure monitor. Runs inside the cluster,
watches pod/container events and crashes, traces a failure back to what actually started
it, and sends a single consolidated alert — what broke, why, what changed, and what to do
— to Slack, Discord, email or 50+ other channels via a Helm-installable chart.

## Why I Should Care
A single, consolidated, no-LLM failure alert for an EKS cluster is a near-zero-maintenance
utility that saves recurring triage time without adding LLM cost or an external API
dependency to basic cluster monitoring.

## Problems It Can Remove
Replaces a hand-rolled `kubectl events` + Slack-webhook script, and the noisy
fifty-separate-alerts experience of raw Kubernetes event/alert streams.

## Practical Uses
- One consolidated Slack message instead of fifty pod-restart alerts when something breaks
  on an EKS cluster.
- Fast first-pass triage before reaching for a heavier LLM-based tool like the
  already-catalogued SherlockOps.
- Low-overhead addition to an existing cluster with no LLM/API dependency.

## Product Opportunities
None identified beyond internal/client infrastructure monitoring.

## Agent / Automation Opportunities
None identified; this is a rule-based alerting tool, not agent-facing.

## Integration
Helm chart (in-cluster) or a plain Docker image. Low effort — install and point it at
notification channels.

## Architecture Notes
Correlating a cascade of pod/container events back to the one root cause, across 50+
notification integrations, is the small-but-fiddly problem this project has already solved
and maintained since 2021 — worth noting as a reference even if a heavier LLM-based tool
is used instead.

## Maturity
Mature: created 2021, 86 forks, low open-issue count (3) relative to its age and star
count — a healthy, settled signal rather than a dead repo.

## License
Elastic License 2.0 (ELv2) — source-available, not OSI-approved. GitHub API reports
NOASSERTION; confirmed ELv2 by reading LICENSE directly. Free to use, modify and self-host
internally; the license blocks offering kwatch itself to third parties as a hosted or
managed service. Fine for internal EKS monitoring; would matter if packaging it inside an
MSSP-offered managed service.

## Alternatives
- **SherlockOps** (catalogued) — LLM-driven alert investigation across multiple monitoring
  backends, heavier setup, deeper analysis.
- **k8sgpt, HolmesGPT** — LLM-based Kubernetes diagnosis, more mainstream/established in
  this niche.
- Hand-rolled `kubectl events` + Slack webhook script — the status quo this replaces.

## Risks / Limitations
- Elastic License 2.0 is source-available, not a true OSI open-source license — flagged
  per AGENT.md §14 for any resale/managed-service context, though not a blocker for
  internal use.
- Rule-based root-causing is shallower than an LLM-driven tool on genuinely novel failure
  modes.

## Recommendation
USE NOW — near-zero-effort install for EKS clusters; try it on one real cluster before
reaching for a heavier LLM-triage tool.

## Change History
### 2026-11-23
Initial discovery and review. Today's Small but High-Leverage Utility. Catalogued
USE NOW, 6.8/10.
