# DBTrail

## Summary
A self-hosted HTAP layer for MySQL/MariaDB (and, newer, PostgreSQL) that follows the
source database's binlog/WAL and answers heavy analytical queries from a columnar Parquet
copy via embedded DuckDB, while routing transactional and by-key reads straight through to
the real database over the same MySQL-protocol connection. Nothing installs on the source
database — no plugin, agent, or trigger. Ships an MCP server so Claude (or any MCP client)
can ask "what changed in `orders` in the last hour?" and get a read-only, SQL-backed
answer, plus point-in-time `AS OF` queries and auto-generated undo SQL for accidental
writes.

## Why I Should Care
Amazon RDS/Aurora are already in the stack, and "heavy reporting query is slowing down the
read replica" is a recurring problem that normally means either a second read replica sized
for reporting or a bolt-on OLAP pipeline (Debezium → Kafka → warehouse). DBTrail replaces
both with one process that speaks the MySQL wire protocol, so the application's existing
SQL and drivers don't change. Verified support for Amazon RDS for MySQL and Aurora MySQL,
plus PostgreSQL 15+ (logical replication), puts this one tool across most of the relational
footprint already in use.

## Problems It Can Remove
- A hand-rolled CDC pipeline (binlog → Kafka/Debezium → warehouse) just to get fast
  aggregate queries off the primary.
- A dedicated reporting read-replica sized and paid for 24/7 to absorb a handful of slow
  queries.
- Ad-hoc "what changed and when" forensics during an incident — DBTrail keeps before/after
  row history and can generate the exact undo SQL.
- A bespoke "let the agent query the database read-only" integration — the MCP server
  already enforces read-only access.

## Practical Uses
- Point Metabase/Grafana at DBTrail's MySQL port instead of the primary for dashboards that
  scan whole tables.
- Enable routing so the application keeps one connection string: writes/lookups go to
  MySQL, heavy aggregate reads get silently answered from the Parquet copy.
- Ask Claude (via the MCP server) "what was deleted from `orders` in the last hour?" and get
  a cited, read-only answer plus the SQL to reverse it.
- Restore a table to how it looked at a specific timestamp with `AS OF` from a normal mysql
  client, without restoring a backup.
- Export the Parquet copy to Iceberg for a downstream lakehouse without a separate ETL job.

## Product Opportunities
The "ship vs. operate" split (self-host the OSS binary, or pay for the managed index
server) is a clean open-core pattern worth noting for the MSSP/PTaaS context: a client
could get fast reporting over their own RDS instance without DBTrail's team touching their
data plane.

## Agent / Automation Opportunities
Built-in MCP server (read-only tools only, never executes SQL on the source database) —
usable as-is in an agent workflow for "diagnose what changed" tasks without building a
custom read-only DB proxy for the agent.

## Integration
Docker Compose install (`curl | sh`), opens a local web UI to connect the source database
and take the first copy; the MySQL-compatible port and REST API are then usable from
anything. Also ships Linux packages and an Amazon ECS deployment path. Requires
`binlog_format=ROW` with full row images and a primary key on every InnoDB table (the
connect form checks this and shows the fix). Effort: **Medium** — straightforward to trial
against a non-production replica via the published demo Docker image, but production use
needs the binlog/`REPLICA IDENTITY FULL` prerequisites reviewed and a plan for what happens
to the application if the DBTrail port goes down (it is explicitly not HA and not a
backup).

## Architecture Notes
Reads the replication stream the same way a real replica would (no WAL/binlog files needed
on disk, which is why managed RDS/Aurora works without superuser access). Maintains a small
operational MySQL instance alongside the Parquet store for its own index/metadata. Schema
changes (added/dropped column, `TRUNCATE`) trigger a full re-read of that table under a
short lock — a known, documented tradeoff rather than a hidden gotcha.

## Maturity
**Emerging.** Created February 2026, under active daily development (one dominant
contributor with 2,220+ commits — the project's founder, a former MySQL Technical Lead at
Percona — plus dependabot). Apache-2.0, real published benchmarks (14 production-style
reports averaged 6.5x faster, the heaviest went from 225s to 7.9s), and an unusually honest
"Limits" section in the README (not HA, not a backup, history retention is 48h without S3,
TLS on the port is self-signed unless you supply a certificate).

## License
Apache-2.0 — free for commercial and production use, including embedding. No AGPL/SSPL
concerns.

## Alternatives
Readyset (binlog-based MySQL/Postgres query cache, different mechanism — caches query
results rather than maintaining a full columnar copy), Debezium + a warehouse (far heavier
to operate), a dedicated reporting read replica (simplest but keeps paying row-store
overhead for analytical scans), Materialize (streaming SQL views, different operational
model entirely).

## Risks / Limitations
Solo-founder-led project (bus-factor risk) despite the credible pedigree. Not
high-availability — if the DBTrail port goes down, anything connected through it gets
connection errors (the docs recommend keeping a direct path to the real database). History
retention without an S3 bucket is only 48 hours. Each installation serves one database
family (MySQL/MariaDB or PostgreSQL, not both at once).

## Recommendation
**PROTOTYPE** — worth standing up against a non-production RDS/Aurora replica or the
published demo image to validate the routing and undo-SQL claims before trusting it near
anything that matters.

## Change History
### 2026-11-28
Initial discovery and review.
