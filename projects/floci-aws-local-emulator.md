# Floci

## Summary
A free, open-source local AWS emulator (MIT) positioned as a drop-in replacement for LocalStack, running at `http://localhost:4566` and covering 100+ AWS services across compute, storage, messaging, IAM, cost/billing, and more. Part of a small emulator family (`floci`, `floci-az`, `floci-gcp`, `floci-oci`) from the same org.

## Why I Should Care
LocalStack — the de facto standard for local AWS development — is now `archived: true` on GitHub (confirmed via the GitHub API: `pushed_at: 2026-03-23`, no commits since). Floci's own README states it is a direct, same-port, same-API drop-in replacement ("swap the image and keep going"), with an explicit LocalStack migration guide. For anyone using S3, EC2, ECR, Lambda, SQS, IAM, EB, ALB, or EKS in local dev/CI — which is Sourav's exact AWS footprint — this closes a real gap left by LocalStack's archival with zero migration friction.

## Problems It Can Remove
- Local AWS dev/test without a real AWS account, credentials, or a LocalStack subscription.
- CI pipelines that spin up AWS-shaped infra for integration tests, without paying for real AWS resources or LocalStack Pro.
- Terraform/CDK/OpenTofu plan-and-apply dry runs against a local endpoint.

## Practical Uses
- Point the AWS CLI/SDK/Terraform at `localhost:4566` for local development of Lambda/S3/SQS-backed services.
- Run integration tests in CI against emulated S3/DynamoDB/SNS/SQS instead of mocking each call.
- Prototype EKS/ECS/ALB-based architectures locally before touching real infra.
- Use the Bedrock-adjacent/cost-explorer emulation (synthesized from pricing snapshots) to sanity-check IaC cost estimates offline.

## Product Opportunities
Not a product-building block itself — it's dev/test infrastructure. Indirect value: faster iteration shortens the path from idea to shipped feature for any AWS-backed product.

## Agent / Automation Opportunities
No MCP server observed. Usable as a CI tool invoked from agent-driven pipelines (an agent could spin up Floci, run integration tests, and tear it down as part of an autonomous build/test loop).

## Integration
- `docker compose up` or `floci start` via the official CLI; also installable as a single Docker image.
- Drop-in for existing LocalStack-based Docker Compose files — swap the image reference.
- **Integration effort: Low.** Same port, same API surface as LocalStack; no application code changes.

## Architecture Notes
In-process emulation for most services; some (Cost and Usage Reports) use a `floci-duck` sidecar for Parquet/DuckDB-based reporting. Multi-partition, multi-region support out of the box. The project ships sibling emulators for Azure, GCP, and OCI under the same "any cloud, locally" umbrella — worth knowing about if infra work expands beyond AWS.

## Maturity
Mature for a ~7-month-old project: 100 contributors, continuous commit activity (multiple merges per hour at time of review), v2.1.0 tagged release (2026-09-15), Docker Hub image with pull-count badge, CI build status badge. 26,370 stars / 2,846 forks — fork ratio (~10.8%) is consistent with organic adoption rather than a fake-star pattern, though the stars:watchers ratio (26,370:86) is unusually wide and worth a second look over time.

## License
MIT, confirmed via `LICENSE` file. No feature gates, no paid tier referenced anywhere in the README — explicitly contrasted against LocalStack's move to gate core services behind a paid plan.

## Alternatives
- **LocalStack** — now archived; the reason this category reopened.
- **MiniStack** — see separate entry; different architecture (real backing infra: actual Postgres/MySQL/Redis containers vs. in-process emulation), Python-based, smaller (4,832★, 100 contributors).
- **kumo** (sivchari/kumo) — Go-native, single binary, AWS SDK v2-focused, 82 services, 1,496★, 18 contributors. Worth a look if the stack is Go-heavy; narrower scope than Floci.

## Why This One
Floci is the most direct, highest-adoption, most actively maintained LocalStack replacement found this run, with an explicit migration path and broader-than-AWS ambitions (same vendor ships Azure/GCP/OCI emulators). MiniStack's "real infrastructure" approach is architecturally interesting but heavier (needs the Docker socket mounted for RDS/ECS); kumo is the right pick only if working in pure Go.

## Risks / Limitations
- Young project (created 2026-02-18) — no multi-year track record yet.
- Stars grew very fast (0 to 26k in ~7 months); fork activity and contributor count support organic growth, but this is worth re-checking in future runs.
- Service coverage claims ("100+ services") should be spot-checked against the actual feature needed before relying on it for a specific AWS service.

## Recommendation
USE NOW — for any local AWS dev/test workflow that previously used LocalStack. Low integration effort, permissive license, directly closes a gap just opened by LocalStack's archival.

## Change History
### 2026-10-05
Initial discovery, triggered by finding `localstack/localstack` marked `archived: true` via the GitHub API while researching slot 6 (infrastructure/observability/deployment). Confirmed MIT license, active development, and genuine LocalStack migration path.
