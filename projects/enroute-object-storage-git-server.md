# Enroute

## Summary
A Git server rebuilt from scratch in Rust on the design principles of a modern distributed database rather than "a directory of files on one machine." Repositories are stored as immutable, log-structured layers in an object store (S3/GCS/Azure Blob, or a local directory); a small Raft-replicated ledger per repository handles refs and commits ordering; compute nodes that serve clones/fetches/pushes hold only rebuildable caches and can be added or replaced freely.

## Why I Should Care
Not something to adopt directly today, but a genuinely instructive answer to "what would Git hosting look like if you designed it today instead of in 2005." The separation of ledger (small, durable, Raft-replicated metadata) from storage (cheap, durable, infinitely scalable object storage) and the optimistic-write/log-structured-layer model is the same pattern worth recognizing in his own AWS work whenever something is being designed to scale past a single disk — it's a clean, concretely-documented reference architecture rather than an abstract pattern.

## Problems It Can Remove
Not a drop-in replacement for anything he currently runs; this is a study-the-architecture entry, not a tool to deploy.

## Practical Uses
- Reference architecture for any future "store a lot of append-mostly structured data cheaply, with strong per-shard ordering" design problem (not limited to Git).
- A programmable Git server (gRPC API, opt-in hooks controlling read visibility and which pushes land) could be relevant if he ever needs fine-grained, code-driven access control on a self-hosted Git server — e.g., a PTaaS engagement needing an isolated, audited code-review environment.
- Benchmark/throughput claim (7,500+ pushes/second to a single repository, durable before acknowledgment) is a useful reference number when evaluating other "scalable Git" claims.

## Product Opportunities
None directly identified; this is infrastructure for people building Git-hosting-adjacent products (the README explicitly positions it as the base layer for a future CI/CD + code review suite), not a component to embed in a typical SaaS product.

## Agent / Automation Opportunities
The gRPC API and opt-in hooks (deciding who can read, what they see, which pushes land) are a plausible substrate for an agent-facing, policy-gated Git backend, but this is speculative — no agent-specific tooling exists in the project yet.

## Integration
Single binary; one node can run both the "compute" and "ledger" roles for a minimal deployment, or scale out to a replicated cluster. The only hard dependencies are an object store (S3-compatible, or a local directory for testing) and a persistent disk per ledger node. Install via `curl -fsSL https://enroute.sh/install | sh`.

## Architecture Notes
Compute/storage separation: compute nodes do Git protocol work and index/validate pushes but hold no durable state of their own; ledger nodes keep each repository's refs and layer list in an independent Raft log on durable local disk. A push commits once its log entry lands on ledger disks, without waiting on the object store — the object store write happens asynchronously as a background layer-merge, analogous to an LSM tree's compaction. Readers always see one immutable snapshot of a repository and never block on writers; concurrent pushes to different branches of the same repository don't conflict, and the final serialization step is a single compare-and-swap on the repository's log.

## Maturity
Emerging/experimental. Created 2026-08-24, v0.3.0 released 2026-10-14, 326 stars / 14 forks / 3 open issues — a reasonable ratio with no red flags. Early (barely two months old); the README itself describes the broader CI/CD/code-review vision as future work, with the current scope limited to Git storage.

## License
Apache-2.0 — unrestricted for commercial use or self-hosting, if ever adopted directly.

## Alternatives
Traditional Git hosting (GitHub/GitLab/Gitea/self-hosted bare repos on a single disk — the status quo this rethinks); Pierre (code.storage) and Cloudflare Artifacts, which Enroute explicitly positions itself as an open-source alternative to.

## Risks / Limitations
Very early (two months old); no independent production track record. Git protocol surface described as currently scoped to storage/transport only — no code review, CI/CD, or broader SDLC tooling yet despite that being the stated long-term vision. Adopting this for anything beyond study would mean depending on a two-month-old distributed-systems project for source-control durability.

## Recommendation
STUDY. Not something to deploy, but a well-documented, concretely engineered example of applying modern database design (log-structured storage, Raft-replicated metadata, optimistic writes) to a problem — Git hosting — that most tools in the space still solve the 2005 way.

## Change History
### 2026-10-29
Initial discovery and review. Slot 2 run (product infrastructure, APIs, backend components). Found via GitHub Search API `topic:object-storage created:>2026-06-01`.
