# LibreDB Studio

## Summary
A browser-based, self-hosted SQL/NoSQL IDE that deploys next to your data instead of
installing on a laptop. One tab covers PostgreSQL, MySQL, MongoDB, Redis, SQLite,
ClickHouse, MariaDB, Apache Druid, DuckDB, Turso, Oracle, SQL Server and Couchbase, with
SSO, an audit trail, ER diagrams and AI-assisted queries — MIT licensed with nothing held
back behind an enterprise tier.

## Why I Should Care
It directly covers both halves of the stack (MongoDB + PostgreSQL) with one tool, and it
is meant to be self-hosted as a team resource rather than installed per developer — a
better fit for giving contractors, junior devs, or an MSSP client scoped access than
handing out raw DB credentials for a desktop client.

## Problems It Can Remove
Removes the need for per-seat desktop DB clients (TablePlus, DataGrip, Postico) and the
friction of provisioning DB credentials to every laptop that needs read/write access to
staging or production data.

## Practical Uses
- Team-shared database browser for staging/prod Postgres and MongoDB, no desktop install
- Give contractors or junior devs scoped, audited DB access instead of raw credentials
- Replace paid per-seat TablePlus/DataGrip/Postico licenses across a team
- Run joins, chart results, and read ER diagrams against both Postgres tables and MongoDB
  collections in one place

## Product Opportunities
Could be bundled as the internal ops/debug console for the MSSP admin-portal SaaS, since
it deploys next to the data rather than needing a local install; or white-labeled behind
SSO as an internal database-ops portal.

## Agent / Automation Opportunities
Ships AI-assisted query generation in the UI. No CLI or MCP surface identified in the
README as of this review — primarily a human-facing web tool today.

## Integration
Docker image and a Helm chart for Kubernetes are both available (`libredb/libredb-studio`
on Docker Hub, chart on Artifact Hub). Low effort: `docker run` or `docker compose up` to
get a working instance pointed at existing databases.

## Architecture Notes
Next.js 16 + React 19 frontend, deployed as a single container that connects out to
whichever database engines you point it at rather than owning the data itself. Officially
listed by the PostgreSQL project's own news page and software catalogue, and referenced in
Redis, ClickHouse, MariaDB and Apache Druid's own docs/GUI-tool lists — verified directly
against those pages, not just the README's claim.

## Maturity
Emerging but credible. Created 2025-12-23 (~9 months old), 21 contributors, active commits
daily, current release 0.13.7 (2026-08-31), SonarCloud quality gate and Codecov badges in
the README.

## License
MIT. No restrictions on commercial or SaaS use, redistribution, or embedding, and the
README explicitly states there is no enterprise-tier holdback.

## Alternatives
TablePlus (paid, desktop-only); DBeaver (desktop, no ER diagrams/AI queries, clunkier UI);
Adminer (thin, single-engine, no charting); pgAdmin (Postgres-only).

## Risks / Limitations
- Only 409 stars / ~9 months old — verify upgrade stability before trusting it with
  production credentials
- SSO/audit-trail depth is only verified via README/docs claims, not hands-on testing in
  this review

## Recommendation
USE NOW — low integration effort (Docker/Helm), permissive license, and a direct fit for
the MongoDB+Postgres stack. Worth spinning up in a sandbox environment against a non-prod
database first.

## Change History
### 2026-09-06
Discovered during slot 5 (self-hosted SaaS alternatives, productivity) run. Verified the
PostgreSQL-project listing claim directly against postgresql.org's news page and confirmed
MIT license and version via the GitHub API.
