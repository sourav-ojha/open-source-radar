# Kingfisher

## Summary
Open source secret scanner and live secret validator built in Rust by MongoDB. Uses a SIMD-accelerated regex engine (Vectorscan) for high-throughput scanning of files, local Git history, and integrated platforms (GitHub, GitLab, Bitbucket, Azure Repos, Gitea, Hugging Face, Jira, Confluence, Slack, Teams, Postman, Docker, AWS S3, Google Cloud Storage). Validates discovered credentials against live provider APIs to cut false positives, can revoke supported secrets from the CLI, and ships a browser-based report viewer that also triages Gitleaks/TruffleHog/SARIF output.

## Why I Should Care
Live validation against the actual provider — not just regex pattern matching — is the real differentiator from Gitleaks/TruffleHog, and it's shipped by MongoDB rather than a startup with a hosted upsell to protect. Directly relevant to both general dev hygiene and the MSSP/PTaaS side of the business.

## Problems It Can Remove
- Noisy secret-scanner output full of false positives from plain pattern matching
- A paid GitGuardian-style SaaS subscription for the same core capability
- Manually triaging findings across multiple scanner output formats

## Practical Uses
- CI gate that blocks PRs containing live, validated credentials
- One-off repo/S3/Jira/Confluence audits for leaked secrets
- Importing and triaging existing Gitleaks/TruffleHog scans in one viewer

## Product Opportunities
Embeddable as a scan capability inside the MSSP security admin-portal SaaS or as a PTaaS deliverable — the live-validation step is the part worth highlighting to clients over a plain regex scanner. Could be white-labeled as a client-facing secret-exposure report generator.

## Agent / Automation Opportunities
CLI-first with JSON/SARIF/TOON/HTML output and webhook alerting (Slack, Teams, Discord, Mattermost, Google Chat, generic HTTPS) — trivial to wrap as a CI step or an agent tool that gates merges or files tickets on findings.

## Integration
Single Rust binary or Docker image, CLI-driven. Scan-target integrations (GitHub, GitLab, S3, etc.) are built in — no glue code needed for the common cases.

## Architecture Notes
Multithreaded, Hyperscan/Vectorscan-powered scanning built for large codebases. Detector catalog is sourced in part from Betterleaks and Google OSV-SCALIBR's Veles detectors rather than reinvented from scratch — a reasonable way to keep pace with an ever-changing set of secret formats.

## Maturity
Mature: MongoDB-sponsored, v2.0.0 released 2026-08-23, wide integration matrix, active development (pushed 2026-08-30).

## License
Apache-2.0. No restrictions on commercial use, redistribution, or embedding.

## Alternatives
Gitleaks, TruffleHog, detect-secrets, GitGuardian (closed-source SaaS).

## Risks / Limitations
- Rust codebase — contributing fixes requires Rust, outside the primary stack
- Live validation calls out to provider APIs; scanning needs appropriate network egress and rate-limit awareness

## Recommendation
USE NOW — low-effort CI addition with a genuine differentiator (live validation) over the scanners already well known in this space.

## Change History
### 2026-08-30
Initial discovery and review.
