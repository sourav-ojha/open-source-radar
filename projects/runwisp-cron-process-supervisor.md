# RunWisp

## Summary
A self-hosted cron job manager and process supervisor shipped as a single Go binary (~30MB RAM)
with a built-in web dashboard, terminal UI, and REST API — a combined replacement for `crond` and
`supervisord`, configured in one TOML file with no Python, Node.js, or external database required
(SQLite is embedded).

## Why I Should Care
A single binary that gives both scheduled jobs and long-running services run history, live log
streaming, and failure alerting in one place removes the "write a tiny custom dashboard to see why
last night's cron job failed" task that recurs on every small self-managed server.

## Problems It Can Remove
- A scattered mix of crontab entries plus a hand-rolled process-restart script with no shared
  history or dashboard
- Bespoke Slack/webhook alerting glued onto a failing cron job after the fact
- Standing up a monitoring stack just to see whether last night's backup job actually ran

## Practical Uses
- Scheduled backup/report jobs (e.g. `pg_dump`) and long-running worker services side by side on
  an EC2/VPS box, with retries, overlap policies, and failure alerts
- A web dashboard and REST API for job status on a small client deployment (MSSP/PTaaS delivery
  box) without building bespoke cron-monitoring tooling
- Migrating an existing crontab, supervisord config, or docker-compose service list via the
  project's documented import paths

## Product Opportunities
Could underpin a lightweight internal ops dashboard for client deployments without building
bespoke cron-monitoring tooling from scratch.

## Agent / Automation Opportunities
REST API allows inspecting and triggering/stopping tasks programmatically, which an agent could
use to check job status or kick off a run — though task *definitions* remain file-based only by
design (no remote mutation via API).

## Integration
Single binary via curl installer, Homebrew, apt/rpm, npm/Bun, Docker, or tarball. **Integration
effort: Low** — `runwisp.toml` replaces crontab/supervisord config, and the project documents
direct migration paths from cron, supervisord, docker-compose, node-cron, and PHP schedulers.

## Architecture Notes
Deliberately keeps task definitions as the source of truth on disk rather than mutable through
the API — the dashboard and REST API can inspect and trigger/stop tasks, but cannot modify what
runs. Review culture is called out explicitly by the maintainer ("whether a helper deserves to
exist or should just be three lines inline"), and the daemon reportedly has more lines of tests
than code.

## Maturity
Emerging. Created March 2026, 108 stars, 4 subscribers, mentioned in Awesome Go. Codecov coverage
and a green SonarCloud security rating badge are both present and current.

## License
GPL-3.0-or-later for the daemon and web UI; Apache-2.0 for shared libraries under `packages/`,
the config JSON Schema, and the OpenAPI description. Fine to run unmodified as self-hosted
infrastructure; distributing a modified binary would require releasing those modifications under
the same GPL license. Does not affect the applications it schedules or supervises.

## Alternatives
`crond`/systemd timers/`supervisord` (no dashboard, no unified history, no built-in alerting),
Dagu (already catalogued — broader DAG/workflow orchestration, heavier than a straight cron+
supervisor swap), healthchecks.io (hosted monitoring only; does not run or supervise the job
itself).

## Risks / Limitations
- GPL-3.0 on the daemon itself — fine for internal self-hosted use, worth reviewing before
  bundling a modified binary into a distributed product
- Young commercial vendor (PoppyCake, s.r.o., product created March 2026) — future feature gating
  is a possibility worth watching
- No remote mutation of task definitions via the API, by design — a limitation for a purely
  API-driven workflow
- Small community (108 stars/4 subscribers) relative to the polish of its documentation and CI
  badges

## Recommendation
PROTOTYPE — worth trialing as the cron+supervisor layer on the next small self-managed VPS/EC2
deployment before committing to it as a standing replacement for an existing crontab/supervisord
setup.

## Change History
### 2026-11-26
Discovered via GitHub search (slot 2: product infrastructure, APIs, backend components). First
catalog entry.
