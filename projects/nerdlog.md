# Nerdlog

## Summary
A fast, remote-first, multi-host TUI log viewer with an interactive timeline histogram
and no central server. Nerdlog opens SSH connections directly to each target host, runs
the query (time range + pattern filter) remotely, and only pulls back matching log lines
(up to a configurable page size) plus histogram data — never the full log file.

## Why I Should Care
It removes the setup tax entirely: no agents to install on hosts, no central server, no
Docker Compose stack to run just to read logs from a handful of machines. For a solo or
small-team operator this is strictly less operational overhead than any hosted-server log
tool, while still giving a real timeline histogram and correlated multi-host view that
plain `ssh ... tail -f` scripts can't.

## Problems It Can Remove
Standing up ELK/Graylog/OpenObserve just to read logs from 2-5 EC2/EB instances during an
incident; juggling multiple SSH tmux panes to eyeball logs across hosts; the delay between
"something's wrong" and "I can see when it started."

## Practical Uses
- Tail and search syslog/journalctl across a handful of EC2/EB instances during an
  incident without deploying a log-agent stack first
- Quick visual triage via the timeline histogram to spot when an error spike started,
  before diving into full-text search
- Ad hoc log inspection on client MSSP/PTaaS infrastructure where installing an agent
  stack isn't practical or permitted

## Product Opportunities
None specific — this is an operator tool, not a product-embeddable component.

## Agent / Automation Opportunities
None built in; it's an interactive TUI, not scriptable/MCP-friendly as-is.

## Integration
Local Go binary install (Homebrew, `go install`, or prebuilt binary). No agent or server
component to deploy on target hosts — it connects out over SSH from wherever you run it.
Low effort, no infrastructure changes required.

## Architecture Notes
Filtering and histogram computation happen on each remote host (via a lightweight remote
process nerdlog manages over the SSH connection), not on the local machine — the design
choice that lets it stay fast even on gigabyte-scale log files without downloading them.
Persistent idle SSH connections to each host avoid per-query connection setup cost.

## Maturity
Mature for its scope. Created 2025-04-20 (~18 months old), 1,563 stars, 40 forks, latest
release v1.11.0 (2026-09-17), monthly release cadence. Single dominant contributor
(467 of ~486 commits) — real bus-factor risk despite the steady history.

## License
BSD-2-Clause. No restrictions on commercial use, modification, or redistribution.

## Alternatives
Graylog and Kibana/ELK (mainstream, both require standing up a server stack), OpenObserve
(catalogued-class, requires a running service), plain multi-pane `ssh tail -f` scripts.

## Risks / Limitations
- Single maintainer — bus-factor risk
- Primary use case is text log files reachable over SSH (syslog/journalctl-shaped); not a
  general structured-log or distributed-trace tool
- No central server means no persistent history beyond what's on the hosts themselves —
  it's a query tool, not a retention/archival solution

## Recommendation
USE NOW — very low adoption risk (nothing to deploy, no infra change), and it directly
replaces an actual recurring workflow (SSHing into boxes to grep logs during an incident).
Install it locally and point it at whatever EC2/EB hosts are already in use.

## Change History
### 2026-09-21
Initial discovery and cataloguing. Slot 6 (infrastructure, observability, deployment) run.
