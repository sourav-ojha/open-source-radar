# Plumber

## Summary
A CLI/Docker/GitHub-Action security scanner that audits GitHub Actions and GitLab CI pipelines for risky patterns — script injection via untrusted input, overly broad permissions, unpinned third-party actions, secret exposure — and reports findings in SARIF, GitLab SAST, JSON, CSV, OCSF, PBOM, and CycloneDX formats.

## Why I Should Care
CI/CD pipeline misconfiguration is a common real-world supply-chain weak point, and this is a single tool that covers both GitHub Actions and GitLab CI with native Code Scanning/MR-widget integration. Has visible adoption from recognizable projects (go-delve/delve, Lightpanda, Bunkerity, Stellarium, ciso-assistant-community), not just stars.

## Problems It Can Remove
Removes the need to manually audit workflow YAML for risky patterns (`pull_request_target` with checkout of untrusted code, unpinned `uses:` refs, secrets printed to logs, over-broad `permissions:` blocks) — a class of bug that's easy to introduce and easy to miss in review.

## Practical Uses
- Add a `plumber analyze` step to CI that fails a PR introducing a new risky pattern.
- One-off audit of a third-party or client repo with `plumber analyze github.com/owner/repo`, no clone required.
- Export SARIF to GitHub Code Scanning or GitLab SAST so findings show up natively in the PR/MR UI.
- Export CycloneDX/OCSF/PBOM for feeding into existing security tooling or compliance reporting.

## Product Opportunities
A packaged "CI/CD pipeline hygiene check" using Plumber could be a cheap, high-signal addition to the MSSP/PTaaS service catalog — auditing a client's Actions/GitLab-CI configuration is fast and doesn't require touching production systems.

## Agent / Automation Opportunities
JSON/SARIF output is straightforward for a coding agent to parse and turn directly into a remediation PR (e.g. pin an action to a SHA, narrow a `permissions:` block).

## Integration
Single binary via Homebrew/mise, a Docker image, an official GitHub Action, and a GitLab CI component. No server or account required for local/CI use (`plumber.io` hosts an optional public score badge and a "Plumber Radar" scanning public repos). Low integration effort.

## Architecture Notes
Static analysis over workflow YAML plus repo settings (branch protection, default permissions). The multi-format reporting layer (SARIF/GitLab SAST/OCSF/CycloneDX/PBOM) suggests the project is being built to plug into existing security pipelines rather than to be a standalone dashboard.

## Maturity
Emerging: created 2026-01-19, 821 stars, 36 forks, actively released (v0.6.0, pushed today). OpenSSF Scorecard and SLSA Level 3 badges are self-reported in the README and were not independently re-verified this run. 105 open issues against a small team is a backlog worth watching.

## License
MPL-2.0 — file-level weak copyleft. Modifications to Plumber's own source must be shared if redistributed, but running it against pipelines or in CI imposes no obligations on the code it scans. Worth a quick check against any internal OSS-license policy before adopting at the MSSP, since MPL is less common than MIT/Apache in this radar.

## Alternatives
- **zizmor** — GitHub-Actions-only, Rust, narrower scope, no GitLab CI support.
- **OpenSSF Scorecard** — broader repo-health signal, not specifically pipeline-config-focused.
- **step-security/harden-runner** — runtime egress control rather than static workflow-config scanning; complementary rather than competing.

## Risks / Limitations
- Young project; ruleset and output formats likely to keep changing.
- MPL-2.0 license needs a quick internal check for MSSP/client-facing use.
- 105 open issues vs. a small core team — triage responsiveness unverified.

## Recommendation
PROTOTYPE — add as a CI step on one repo first; strong candidate to extend into the MSSP/PTaaS offering if it holds up.

## Change History
### 2026-10-09
First catalogued. Slot 3 (developer utilities, debugging, testing) discovery run. GitHub API verified: MPL-2.0, 821 stars, pushed today, not archived. README adopters list cross-checked against the named projects' own star counts.
