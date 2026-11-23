# Databasus

## Summary
Self-hosted database backup platform covering PostgreSQL (physical, incremental, and
WAL-streaming PITR, with HA timeline-following for Patroni/CloudNativePG), MySQL, MariaDB
and MongoDB (logical), plus managed Neon/Supabase Postgres. After every backup it performs
a real restore into a throwaway container to verify the backup is actually usable, not
just checksum-intact, and reports a per-table row count. Multi-destination storage
(S3/Azure Blob/R2/FTP/SFTP/NAS), GFS retention, AES-256-GCM encryption, multi-channel
notifications, and workspace-based team access control with audit logs.

## Why I Should Care
It backs up both halves of a MERN/Postgres stack (PostgreSQL and MongoDB) under one
self-hosted tool, and its restore-verification step catches the single most commonly
skipped failure mode in a hand-rolled backup setup: a backup that silently can't be
restored.

## Problems It Can Remove
Replaces a hand-rolled cron + pg_dump/mongodump + `aws s3 cp` script with no retention
logic, no restore testing, and no team access control. Removes the need to separately
evaluate pgBackRest/WAL-G (Postgres-only) and a MongoDB-specific backup script.

## Practical Uses
- Nightly PITR-capable Postgres backups with HA timeline-following across a Patroni/CNPG
  primary failover.
- Logical MongoDB backups alongside Postgres backups in the same tool and schedule.
- Backing up a managed Neon or Supabase Postgres instance without a custom script.
- Workspace-scoped access for a small team or an MSSP client engagement, with audit logs.

## Product Opportunities
None identified beyond internal/client infrastructure use.

## Agent / Automation Opportunities
None identified; this is an operational tool, not an agent-facing one.

## Integration
Docker-based: an automated install script, a plain `docker run`, docker compose, or a
Kubernetes Helm chart. Low integration effort — connects to existing databases with a
read-only user by default and a built-in SSH tunnel option.

## Architecture Notes
The restore-verification approach (spin up a real database container, restore into it,
diff the row counts) is the right way to validate a backup and is worth copying into any
custom backup tooling even if Databasus itself isn't adopted. GFS (Grandfather-Father-Son)
retention and workspace/RBAC are both built in rather than bolted on.

## Maturity
Mature: 18 months old, 619 forks, active issue tracker (11 open), frequent releases
(v3.72.0 at time of review). Elevated stars:subscribers ratio (9,320:37, ~252:1) is worth
noting but tempered by real fork and issue activity.

## License
Apache-2.0. No restrictions on commercial or internal use.

## Alternatives
- **pgBackRest, WAL-G, Barman** — mature, Postgres-only, no web UI, no MongoDB/MySQL.
- **pgbackweb** — similar self-hosted web-UI backup tool, but Postgres-only, no PITR/HA
  timeline-following, no restore verification, no MongoDB support, AGPL-3.0.
- Hand-rolled cron + dump scripts — the status quo this replaces.

## Risks / Limitations
- Elevated stars:subscribers ratio for an 18-month-old project (noted above).
- MySQL/MariaDB/MongoDB backups are logical-only — no physical/incremental/PITR for those
  engines, only for PostgreSQL.
- The restore-verification step adds real compute/IO load on the Databasus host per backup.

## Recommendation
USE NOW — directly covers the two databases actually in the stack, with a restore-
verification feature that's rare among comparable tools and genuinely reduces backup risk.

## Change History
### 2026-11-23
Initial discovery and review. Catalogued USE NOW, 8.4/10.
