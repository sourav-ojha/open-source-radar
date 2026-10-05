# MongoRescue

## Summary
A self-hosted MongoDB backup and disaster-recovery tool (MIT, Go, single binary): streams `mongodump` to disk or S3-compatible storage, restores into safe clones by default, verifies every backup with checksums and automated restore tests, and ships a dashboard, REST API, CLI, and MCP server.

## Why I Should Care
MongoDB is a core piece of Sourav's MERN stack. Backup/DR for self-hosted MongoDB is normally either hand-rolled cron + `mongodump` scripts (easy to get subtly wrong — no verification, no safe-clone restores, no RPO/RTO tracking) or a paid service (MongoDB Atlas backup, which only applies if already on Atlas). MongoRescue packages the parts that are tedious to get right: streaming dumps (flat memory use regardless of DB size), checksum-verified uploads, automated restore-tests that actually prove a backup works, and an MCP server so a coding agent can check backup health or trigger a restore directly.

## Problems It Can Remove
- Hand-rolled `mongodump`/`mongorestore` cron scripts with no verification that a backup is actually restorable.
- Manual tracking of recovery-point objectives and "when did this last actually get tested" for Mongo backups.
- One-off restore scripts that risk overwriting production data — MongoRescue defaults to safe-clone restores and requires explicit confirmation for in-place restores.

## Practical Uses
- Scheduled, encrypted backups of a production Mongo database to S3/MinIO/R2/B2/Spaces/Wasabi.
- Automated restore-tests that restore the latest backup into a temp DB and diff collection counts/indexes — catches silent backup corruption before it's needed for real.
- CLI use in CI: `mongorescue backup --job nightly --wait` with stable exit codes for scripting.
- MCP integration so a coding agent can check "is today's backup good?" or kick off a restore rehearsal without leaving the chat.

## Product Opportunities
Could be the backup layer behind a self-hosted-Mongo-as-a-service offering, or embedded as the DR component of an internal admin portal for clients running their own Mongo instances (relevant to the MSSP/PTaaS context — client data-recovery assurance as a sellable feature).

## Agent / Automation Opportunities
Ships a native MCP server (Streamable HTTP and stdio) with read-only and safe-clone-restore tools, scoped API keys, rate limits, and its own audit log — one of the more complete "MCP server as a first-class interface, not an afterthought" designs seen in recent runs.

## Integration
- `docker run` single container with a bundled dashboard, or a desktop app (Windows/macOS/Linux) for workstation use, or a headless server binary for REST/MCP/metrics only.
- Prometheus metrics exposed; webhook/Telegram/email/SMS notifications.
- **Integration effort: Low.** One binary or one Docker container; no code changes to the application being backed up.

## Architecture Notes
Point-in-time recovery (oplog-based, for replica sets) is in active development per recent commit history (design docs + oplog-reader groundwork landed days before this review) — not yet shipped. Hash-chained audit log, SSO via OIDC (Entra ID/Google/Okta/Keycloak), and a disaster-recovery plan for its *own* metadata (encrypted snapshots + a passphrase-sealed recovery kit) are already in place, which is an unusually thorough design for a two-week-old project.

## Maturity
**Very new — treat with caution.** Created 2026-09-25 (10 days before this review), 6 stars, 0 forks, 0 watchers, but with daily tagged releases from v0.1.0 to v0.18.0 in that window and a feature set (SSO, audit log, MCP server, desktop app, CLI) far beyond what a 10-day-old project usually ships. Either an unusually productive solo/small-team build or heavy AI-assisted development — either way, no independent community validation exists yet.

## License
MIT, confirmed via `LICENSE` file.

## Alternatives
- Hand-rolled `mongodump`/cron scripts (the status quo it replaces).
- MongoDB Atlas continuous backup (requires being on Atlas; not applicable to self-hosted Mongo).
- Percona Backup for MongoDB (more enterprise-oriented, no built-in dashboard/MCP).

## Why This One
It's the only self-hosted, MongoDB-specific backup tool found with a dashboard, verified restores, and an MCP server in one package — most alternatives are either generic (any-database) backup tools or Atlas-locked.

## Risks / Limitations
- 10 days old, single-digit stars, no forks/watchers — no community validation; treat as pre-production until it has a longer track record.
- Point-in-time recovery is still in design/early implementation, not production-ready.
- Single or very small maintainer team — bus-factor risk for anything depended on for real DR.
- "Evidence that backups restore" claims (checksum + automated restore tests) are a documented design, not yet independently verified by this review beyond reading the README/docs.

## Recommendation
PROTOTYPE — the feature design is exactly right for a self-hosted MERN stack's Mongo backup needs, but given the project's age, treat any real use as an experiment, not a production dependency, until it accumulates more track record.

## Change History
### 2026-10-05
Initial discovery via GitHub Search API during slot 6 (infrastructure/observability/deployment) research. Confirmed MIT license, read the full README and release history (v0.1.0 → v0.18.0 in 10 days).
