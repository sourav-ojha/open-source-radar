# Nora

## Summary
Nora is a self-hosted artifact registry and pull-through cache packed into a single ~20MB binary with no database. It covers 17 package formats (Docker/OCI, Maven, npm, PyPI, Cargo, Go, RubyGems, NuGet, Terraform, Ansible Galaxy, Conan, CPAN, RPM, Debian/APT, Hugging Face, and more), with built-in supply-chain curation and OIDC-based CI publishing.

## Why I Should Care
Most projects eventually need to stop hammering npm/PyPI/Docker Hub directly from CI — rate limits, outages, and supply-chain risk all push toward a private mirror. Nora does that with an operating footprint (no database, ~15-23MB idle RAM) far lighter than Artifactory, Nexus, or even the already-catalogued Artifact Keeper.

## Problems It Can Remove
- CI flakiness and rate limiting caused by hitting public registries directly on every build.
- The operational overhead of running a full registry server (database, storage backend, scanning pipeline) just to get a cache and basic curation.
- Manual Artifactory/Nexus migration scripting (`nora import` handles it).

## Practical Uses
- Put Nora in front of npm/PyPI/Docker Hub as a pull-through cache to speed up and insulate CI.
- Use `nora mirror` to prefetch exactly what a project's lockfiles reference for air-gapped environments.
- Enforce a minimum release age or quarantine window on new package versions before they reach builds, using the curation settings.
- Publish CI artifacts via short-lived OIDC tokens from GitHub Actions/GitLab CI instead of long-lived registry credentials.

## Product Opportunities
A near-zero-footprint private registry/cache is an easy, low-maintenance add-on for any client environment that needs an air-gapped or rate-limit-insulated package mirror — including MSSP client deployments where a lightweight footprint matters more than Artifactory-class feature breadth.

## Agent / Automation Opportunities
No MCP server or agent-specific surface documented. The REST/API-token model would support scripting registry operations from automation, but nothing purpose-built for agents beyond that.

## Integration
Docker image (`getnora/nora`, also on GHCR), a Helm chart for Kubernetes, a standalone binary, or `.deb`/`.rpm` packages. Cache storage can live on local disk, S3-compatible storage, or GCS. Low integration effort — no database to provision.

## Architecture Notes
No database — state is held in the binary's own storage layer. Curation is enforced at the proxy layer (allowlist/blocklist/minimum-release-age/quarantine) rather than as a bolt-on scanner; a companion tool, Dozor, turns the OSV advisory feed plus registry inventory into a blocklist Nora enforces. Releases are cosign-signed with SBOMs and SLSA provenance attached.

## Maturity
Emerging. Created ~9 months ago, 350 stars, 44 forks, 4 subscribers, 16 contributors, 40 open issues (a fairly high ratio against star count, suggesting active churn). Releases are frequent — v1.4.2 as of 2026-10-25, pushed the day of this review.

## License
MIT — no restrictions on commercial or embedded use.

## Alternatives
- **Artifact Keeper** (already catalogued, PROTOTYPE 8.5) — broader format coverage (40+ formats), built-in Trivy/Grype scanning, and an explicit Artifactory-migration path; a heavier, more scanning-focused alternative.
- JFrog Artifactory, Sonatype Nexus — commercial, full-featured, operationally heavier.
- Harbor — OCI-only, no multi-format coverage.

## Risks / Limitations
- Young project; 40 open issues against 350 stars suggests rough edges typical of an early-stage tool.
- No independent security audit found.
- Vulnerability blocking depends on a companion tool (Dozor) consuming the OSV feed — a second moving part to keep current.

## Recommendation
PROTOTYPE — worth standing up as a pull-through cache in front of one real CI pipeline to validate the curation and OIDC-publishing flow before considering it a permanent replacement for an existing registry.

## Change History
### 2026-10-26
First catalogued. Surfaced via a GitHub search for "artifact registry self-hosted" during slot 6; evaluated explicitly against the already-catalogued Artifact Keeper rather than as a plain duplicate, since the two target different operating-footprint tradeoffs (lightweight cache-first vs. full registry+scanner).
