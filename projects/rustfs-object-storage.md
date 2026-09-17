# RustFS

## Summary
RustFS is a distributed, S3-API-compatible object storage system written in Rust,
positioned explicitly as a MinIO alternative that ships under Apache-2.0 instead of
MinIO's AGPL-3.0. It covers most of the S3 core feature surface (versioning, object
lock/WORM, server-side encryption, bucket replication, a scanner/healing subsystem, KMS)
plus OpenStack Swift/Keystone compatibility.

## Why I Should Care
MinIO is the default answer for self-hosted S3-compatible storage but its AGPL-3.0
license is a real constraint for anything embedded in a commercial product or offered as
part of a hosted service — exactly the situation with an MSSP admin-portal SaaS. RustFS
targets that gap directly: same integration surface (any S3 SDK/tool works unmodified),
permissive license.

## Problems It Can Remove
- Paying for AWS S3 in dev/test/staging environments, or for smaller self-hosted
  deployments where cloud object storage isn't justified.
- The AGPL licensing friction of MinIO when object storage needs to be embedded in or
  shipped alongside a commercial product.
- Building custom file-storage handling instead of using a well-trodden S3-compatible API
  that every existing tool and SDK already speaks.

## Practical Uses
- Local/self-hosted S3-compatible storage for dev and CI environments, avoiding AWS
  costs and account sprawl for non-production workloads.
- Backing store for self-hosted apps that expect an S3 API (backups, artifact storage,
  user uploads) without depending on AWS directly.
- A drop-in swap for MinIO in existing Docker Compose stacks where the AGPL license is a
  blocker for commercial redistribution.

## Product Opportunities
- Usable as the storage layer inside a self-hosted or on-prem deployment of a commercial
  product without AGPL's network-copyleft obligations attaching to the product itself.

## Agent / Automation Opportunities
- No agent-specific surface; relevant only as infrastructure other tools (including
  agent-facing ones) can store objects against via the standard S3 API/SDKs.

## Integration
Single binary or Docker container; S3-compatible API means zero client-side code changes
for anything already using an S3 SDK (AWS SDK, boto3, aws-sdk-js, etc.). Integration
effort: **Low** for single-node use; **Medium** for distributed/multi-node deployment
given the pool-expansion and replication configuration involved.

## Architecture Notes
Rust-based rewrite of the MinIO-style architecture (erasure coding, bitrot protection,
healing/scanner, distributed pools) — worth a look at the design docs
(docs/architecture/s3-compatibility-matrix.md in the repo) for how it tracks S3 API
coverage explicitly rather than claiming blanket compatibility.

## Maturity
Large, very active project (32,732 stars, created 2023-11-23, pushed_at 2026-09-17).
**Update 2026-09-17: cut a stable `1.0.0` tag on 2026-09-16**, moving past the
`1.0.0-rc.5-preview.3` release-candidate status noted at first review. Ships official
Docker images and has an active Discord.

## License
Apache-2.0 — confirmed via the repository's own LICENSE file. The README explicitly
calls out choosing Apache-2.0 "avoiding the restrictions of AGPL" as a deliberate
differentiator from MinIO. No commercial-use, self-hosting, or embedding restrictions
identified.

## Alternatives
MinIO (the incumbent, AGPL-3.0 — excluded from this catalog as already well known, but
the license is the whole reason RustFS is worth flagging), Kodiqa-Solutions/VaultS3
(lighter-weight S3-compatible server with a built-in dashboard, 1,598 stars, but
AGPL-3.0 — same license friction as MinIO, filed as Worth Watching instead), Garage
(Rust-based, distributed, typically CRDT-metadata-focused — different design center,
not reviewed in depth this run), Tigris (not comparable — tigrisdata/storage on GitHub
is only client SDKs for Tigris's proprietary hosted service, not self-hostable).

## Risks / Limitations
- Very fast star growth for the visibility it has; treat popularity as attention, not as
  a substitute for an independent stability check, even at 1.0.
- Full distributed-mode operational maturity (healing, pool expansion under real load)
  is unverified from documentation alone — worth a load test before production use.
- MinIO's archival (see 2026-09-17 update below) removes the main point of real-world
  comparison for battle-testing at scale — RustFS is now the default choice by
  elimination rather than by a longer proven track record of its own.

## Recommendation
USE NOW — the 1.0.0 stable release plus MinIO's archival (see below) removes the main
reasons for hesitation noted at first review; still worth a load test of
erasure-coding/healing behavior under simulated node failure before anything
production-critical.

## Change History
### 2026-09-10
Initial discovery and review. Slot 2 (product infrastructure, APIs, backend components) run.

### 2026-09-17
Meaningful update. Two changes justify raising status PROTOTYPE -> USE NOW and score
7.9 -> 8.3: (1) RustFS cut a stable `1.0.0` tag on 2026-09-16, up from
`1.0.0-rc.5-preview.3`. (2) MinIO (`minio/minio`) is now confirmed `archived: true` via
the GitHub API (archived at `pushed_at` 2026-04-24) — the long-standing incumbent this
project was explicitly positioned against is no longer maintained at all, which changes
RustFS from "permissive alternative to a maintained AGPL incumbent" to "the maintained
option in this space." Discovered while reviewing mojatter/s2, a smaller Go
library/server that also pitches itself as filling the MinIO gap for local
development (see daily digest 2026-09-17, Small but High-Leverage Utility).
