# Artifact Keeper

## Summary
Artifact Keeper is a self-hosted, Rust-built universal artifact registry: one service speaking the native wire protocol of 45+ package formats (Maven/Gradle, npm, PyPI, Docker/OCI, Cargo, Go, Helm, NuGet, RubyGems, Composer, RPM/Debian/Alpine/Pacman/Conda, Chef/Puppet/Ansible, HuggingFace, VS Code/JetBrains extensions, Conan, Git LFS, Bazel, and more), with built-in Trivy/Grype security scanning and a stated migration path off JFrog Artifactory.

## Why I Should Care
Artifactory and Nexus are the two real options once a team needs more than one package-format registry, and both get expensive at team/org scale. Artifact Keeper covers almost the same format surface under MIT, with per-upload Trivy (image/filesystem) and Grype (dependency) scanning built in rather than bolted on, and it ships a direct Artifactory-migration tool rather than asking for a from-scratch cutover.

## Problems It Can Remove
- A paid Artifactory/Nexus Pro seat just to get multi-format registry support
- Separate single-purpose registries (a private npm registry, a Docker registry, a PyPI mirror) run as independent services
- Hand-wired CI scanning steps for uploaded artifacts — scanning is inline on upload here

## Practical Uses
- Private npm/PyPI/Maven registry for internal packages, backed by one service instead of three
- Self-hosted Docker/OCI registry with scan-on-push, as an alternative to ECR + a separate scanner
- Migrating an existing Artifactory instance off its license using the built-in migration tooling
- Writing a custom WASM format handler for an in-house package type the built-in handlers don't cover

## Product Opportunities
Could sit inside an internal platform/admin-panel product as the artifact-storage backend, or be offered self-hosted to clients who want to get off Artifactory's pricing without losing format coverage.

## Agent / Automation Opportunities
No MCP server or agent-specific surface noted in the README; it is pure infrastructure. A coding agent could drive it via its REST API for package-publish automation, same as any registry.

## Integration
Docker Compose is the primary path (adds PostgreSQL 16 and optionally OpenSearch for search, plus the Trivy/Grype scanners). Also ships Linux/macOS release binaries (amd64/arm64), a beta Windows Service, and a Packer-built AWS AMI. No Kubernetes/Helm packaging for Artifact Keeper itself yet. Integration effort: **Medium** — more moving parts than a single binary, but no exotic infra required.

## Architecture Notes
- Each format gets a native protocol handler rather than a generic blob store with format-specific metadata bolted on
- Custom handlers can be compiled to WASM and loaded via a WIT interface into a sandboxed Wasmtime runtime (CPU/fuel and memory limits), installable/reloadable without a restart
- "Borg Replication": every node is a full, independently-backed instance; peers replicate to each other in a mesh with chunked, network-aware transfer rather than a primary/replica split
- Security findings get an A–F grade with a policy engine that can block or quarantine on ingest; results are cached by SHA-256 so re-uploads of the same artifact don't re-scan

## Maturity
Emerging but substantially built out: 74 contributors, 154 forks, 1,123 stars, created January 2026 with regular tagged releases (v1.10.2 as of 2026-10-02). The README cites 6,400+ backend unit tests, 69 DB migrations, and CI with stress/failure-injection tests, but gives no explicit "production-ready" statement and lists the Windows Service as beta. 189 open issues against 1,123 stars is a normal ratio for an actively-developed registry, not a red flag on its own.

## License
MIT — no restrictions on commercial, embedded, or redistributed use. Clean for SaaS hosting or embedding inside a client-facing platform.

## Alternatives
JFrog Artifactory and Sonatype Nexus (proprietary/commercial at this feature breadth), Harbor (goharbor/harbor, OCI/container-registry only, no multi-language package format coverage), artifact-keeper's own stated differentiator being direct Artifactory migration tooling plus the WASM plugin system for custom formats.

## Risks / Limitations
- No explicit production-readiness statement despite the test-suite claims — verify with a non-critical registry before trusting it with release artifacts
- Requires PostgreSQL (and optionally OpenSearch) as real operational dependencies, unlike a single-binary alternative
- No Kubernetes/Helm chart yet for teams that only deploy that way
- "Edge replication" (Borg Replication) is new enough that multi-region consistency behavior under partition is unverified here

## Recommendation
PROTOTYPE — strong engineering signal (74 contributors, extensive test suite, active weekly releases) and a direct, quantifiable SaaS-cost argument (Artifactory licensing) make this worth running against one real internal package feed before considering it for anything release-critical.

## Change History
### 2026-10-12
First reviewed and catalogued. Confirmed via GitHub API (MIT, 1,123 stars, 154 forks, 74 contributors, pushed 2026-10-12) and README inspection (raw.githubusercontent.com).
