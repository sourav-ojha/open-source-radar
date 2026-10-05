# MiniStack

## Summary
A free, open-source local AWS emulator (MIT, Python) covering 60+ services on a single port, explicitly positioned as a response to LocalStack moving core services behind a paid plan. Its defining difference from other emulators: several services run against *real* backing infrastructure (actual Postgres/MySQL containers for RDS, real Redis/Valkey for ElastiCache, real Docker containers for ECS) rather than pure in-process mocks.

## Why I Should Care
Same trigger as Floci: LocalStack is now archived (confirmed `archived: true` via the GitHub API). MiniStack's README states the motivation directly: *"LocalStack recently moved its core services behind a paid plan. If you relied on LocalStack Community for local development and CI/CD pipelines, MiniStack is your free alternative."* Its "real infrastructure" angle is a genuinely different trade-off from Floci's in-process emulation — useful when test fidelity against real Postgres/Redis/Docker behavior matters more than raw startup speed.

## Problems It Can Remove
- Local AWS dev/test, specifically where RDS/ElastiCache/ECS fidelity against real engines (not mocks) matters — e.g. testing actual SQL behavior, not an approximation of it.
- Local Bedrock development: can proxy `Converse`/`InvokeModel` calls to a local Ollama/llama.cpp/vLLM instance through AWS-shaped wire formats — useful for prototyping Bedrock-based features without an AWS account.

## Practical Uses
- Point `boto3`/AWS CLI/Terraform/CDK/Pulumi at `localhost:4566`.
- Local RDS-backed integration tests that need genuine Postgres/MySQL semantics.
- Local Bedrock prototyping via the `MINISTACK_BEDROCK_PROXY_URL` proxy to a local model server.
- Multi-tenant testing: a 12-digit access key maps to an account and SigV4 region scopes state, so multiple isolated "accounts" can be exercised against one running instance.

## Product Opportunities
Dev/test infrastructure, not a product-building block. Same indirect leverage as Floci: faster local AWS iteration.

## Agent / Automation Opportunities
No MCP server observed. The `/_ministack/*` internal API (health, reset, config, inspect SES/SQS messages) is useful for an agent driving automated integration tests — reset state between runs, inspect what a Lambda/SES/SQS call actually produced.

## Integration
- `pip install ministack && ministack`, or `docker run -p 4566:4566 ministackorg/ministack`.
- A `:full` Docker variant (Debian/glibc + DuckDB) exists for Athena and native Postgres/MySQL drivers; the default slim image (~270MB) covers most services.
- Real-infra services (RDS, ECS) need the Docker socket mounted into the container.
- **Integration effort: Low-Medium.** Same drop-in pattern as Floci for most services; the real-infra services need the extra Docker-socket mount.

## Architecture Notes
The "spin up real backing infra instead of mocking" design is the interesting part: RDS creates an actual Postgres/MySQL container, ElastiCache an actual Redis/Valkey container, Athena runs real SQL via DuckDB, ECS runs real Docker containers. This trades resource footprint (still claimed ~30MB RAM idle, 270MB image) for behavioral fidelity most in-process emulators can't match for stateful services.

## Maturity
~7 months old (created 2026-03-24), 100 contributors, v1.5.21 tagged release (2026-10-03), frequent releases, active CI badge, Docker Hub image. 4,832 stars / 481 forks (~10% fork ratio, consistent with organic growth).

## License
MIT, confirmed via `LICENSE` file. No paid tier.

## Alternatives
- **Floci** — see separate entry; higher adoption (26k★), broader multi-cloud family, pure in-process emulation (lighter, but less behaviorally faithful for stateful services).
- **kumo** (sivchari/kumo) — Go-native, single binary, narrower (82 services), 1,496★.

## Why This One
The only one of the three AWS-emulator alternatives found this run that backs stateful services with real engines instead of mocks — relevant if test fidelity for RDS/ElastiCache/ECS matters more than minimal footprint. Floci remains the safer default for broad drop-in replacement; MiniStack is the pick when a specific service's real behavior needs to be exercised.

## Risks / Limitations
- Young project, single-org maintained — no multi-year track record.
- Real-infra services require Docker-in-Docker or socket-mounting, which has its own security/operational caveats in CI environments.
- Fast star growth (0 to 4.8k in ~7 months) alongside the same LocalStack-archival trigger as Floci — plausible organic reaction to a real market gap, but worth re-checking adoption signals in future runs.

## Recommendation
PROTOTYPE — worth testing specifically where RDS/ElastiCache/ECS fidelity matters; otherwise Floci is the lower-friction default for general AWS local-dev replacement.

## Change History
### 2026-10-05
Initial discovery, found alongside Floci while researching LocalStack's archival. Confirmed MIT license and the real-backing-infrastructure architecture via README and commit history.
