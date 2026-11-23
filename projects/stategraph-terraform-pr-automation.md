# Stategraph

## Summary
Self-hosted Terraform/OpenTofu (plus Terragrunt, CDKTF, Pulumi) pull-request automation.
Listens for GitHub/GitLab PR events, runs `terraform plan` for each changed directory as a
job on your own CI runner (GitHub Actions/GitLab CI) with your own cloud credentials,
comments the plan/cost estimate back on the PR, and applies on a comment or on merge. Adds
directory locks across concurrent PRs, OPA/Conftest policy checks, Checkov security scans,
dependency-ordered applies, and scheduled drift detection.

## Why I Should Care
A plan/apply-on-PR workflow for Terraform is something that would otherwise get assembled
by hand in GitHub Actions over time; Stategraph packages that plus the operational edges
(locking, drift, policy gates) a hand-rolled version usually skips.

## Problems It Can Remove
Replaces a hand-wired GitHub Actions pipeline for Terraform plan/apply that would need to
be extended piecemeal for locking, cost estimates, and policy/security-scan gating.

## Practical Uses
- Terraform/OpenTofu plan-on-PR, apply-on-merge-or-comment workflow for AWS infra changes.
- Directory locks to stop two concurrent PRs racing to apply conflicting changes in the
  same monorepo directory.
- OPA/Conftest policy checks and Checkov security scans gating a Terraform PR.
- Scheduled nightly drift detection against unchanged directories.

## Product Opportunities
None identified beyond internal infrastructure use.

## Agent / Automation Opportunities
None identified; this is a CI/CD automation tool for human-authored Terraform, not
agent-facing. (Contrast with the already-catalogued Kumoss, which generates the Terraform
itself from natural language.)

## Integration
Self-hosted server via Docker compose; plan/apply jobs run on existing GitHub Actions/
GitLab CI runners with your own cloud credentials. Medium effort — requires wiring
GitHub/GitLab webhooks and a CI runner, more setup than a single-binary tool.

## Architecture Notes
Implemented in OCaml, distributed via Docker so the implementation language is irrelevant
for pure consumption. The directory-lock mechanism for concurrent-PR safety and the
dependency-ordered multi-directory apply are the two features most worth studying even if
not adopted directly.

## Maturity
Emerging: created 2024-10, actively developed (pushed daily), real company behind it with
a paid Stategraph Cloud offering alongside the open-source self-hosted server. Low
subscriber count (3) relative to 1,327 stars and 83 forks, but 115 open issues indicate
real usage rather than inflated stars.

## License
MPL-2.0 — file-level weak copyleft. Modifications to MPL-covered source files must be
shared if distributed, but internal/commercial use as-is carries no restriction.

## Alternatives
- **Atlantis** (mainstream, excluded by this radar's policy) — the long-standing baseline
  for this workflow; Stategraph adds directory locks, built-in cost estimates, and
  multi-engine support on top.
- **Kumoss** (catalogued) — AI-driven, generates Terraform from natural language with
  drift correction and compliance review; Stategraph is the more traditional
  human-authored-PR bot, lower-risk to adopt first.
- **Terraform Cloud / Spacelift / env0** — hosted SaaS equivalents, not self-hosted.

## Risks / Limitations
- Low subscriber count relative to stars (noted above), though real issue/fork activity
  tempers the concern.
- OCaml implementation — irrelevant for Docker-based consumption, relevant only if ever
  contributing patches.
- More setup overhead (webhooks + CI runner wiring) than a single-binary tool.

## Recommendation
PROTOTYPE — worth trying against one real Terraform repo's PR workflow before committing,
particularly if directory locking or drift detection is currently handled manually.

## Change History
### 2026-11-23
Initial discovery and review. Catalogued PROTOTYPE, 7.1/10.
