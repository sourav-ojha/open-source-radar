# db-branch

## Summary
db-branch gives self-hosted MySQL and PostgreSQL instant, copy-on-write
database branches built on ZFS clones — a branch of a multi-GB database is
ready in well under a second with zero bytes copied at creation. It exposes
one API surface (CLI, REST, web console, and a built-in MCP server) over a
single node or a Raft-replicated cluster with optional ProxySQL routing, and
supports per-branch checkpoints, resets, and version upgrades isolated from
the base database.

## Why I Should Care
This is the Neon/PlanetScale branching pattern — a database-per-PR, a
disposable copy for an agent to run a risky migration against — but
self-hosted, open-core, and engine-native to Postgres and MySQL rather than
tied to a managed cloud database. For a team already running Postgres on
EC2/EKS rather than a managed branching service, this removes the need to
either adopt a vendor's managed Postgres or hand-roll snapshot/restore
scripts for the same workflow.

## Problems It Can Remove
- Scripting pg_dump/restore or logical-replication-based database copies for
  CI, PR review, or migration testing.
- Giving an AI agent a safe, disposable copy of a real database to run a
  migration or a risky query against, without touching production.
- Paying for a managed branching database service purely to get this
  workflow.

## Practical Uses
- Give every CI run or pull request its own full-size database copy, torn
  down automatically via `--ttl`.
- Let a coding agent create a branch over MCP, run a migration or a query
  it's unsure about, inspect the result, and delete the branch — production
  data never touched.
- Test a MySQL 8.0→8.4 or Postgres major-version upgrade against one branch
  with `branch upgrade --check-only` before touching the base database.
- Take an instant checkpoint before a risky manual change and restore to it
  if the change goes wrong.

## Product Opportunities
The "disposable, policy-scoped copy of a real backing store for an agent to
act against" pattern generalizes beyond databases — worth noting as a
building block for any product offering agents scoped, revertible access to
customer data copies.

## Agent / Automation Opportunities
Ships a first-class MCP server so an LLM agent can create, query, and
destroy its own database branches directly — the README's own first listed
use case is "AI agent sandboxes."

## Integration
Needs Linux and a ZFS pool; the project ships an eval path via Multipass
(Ubuntu VM) for Mac/Windows trial, and documents a real-server install path
separately. Core branching, the CLI/REST/web console/MCP interfaces, and
masking-tool integration (greenmask, anon) are Apache-2.0 and unlocked
without a license key. **Integration effort: Medium** — ZFS is not the
default filesystem on most cloud instances (including standard EC2 EBS
setups), so provisioning a ZFS-backed host or volume is a real
prerequisite, not a config flag.

## Architecture Notes
Open-core structure worth understanding: the core engine (branch
create/reset/checkpoint/restore/upgrade, CLI/REST/console/MCP, single-node
or Raft cluster) is fully open and usable without any license key.
Auth/SSO, audit, point-in-time history, diff, and FSx-backed storage are
separate closed-source modules gated by a license key (30-day trial, no
hosted/cloud offering exists — self-hosted only, by the vendor's own
statement).

## Maturity
Very early but technically substantial — created 2026-11-13 (4 days before
this review), 61 stars, 1 fork, 1 subscriber, self-labeled **Beta** ("APIs
and command-line details may still change before 1.0"), latest tag v0.2.5
(2026-11-15). The documentation depth (English + Japanese docs, an OpenAPI
spec, a live in-browser console demo, published cluster benchmarks) is well
beyond what a 4-day-old repo normally ships, suggesting private development
predates the public release — but the public track record itself is
effectively nonexistent.

## License
Apache-2.0 for the core project (confirmed in README). Paid modules
(auth/SSO, audit, history, diff, FSx storage) are separate closed-source
packages under a commercial license key — flagged per policy, but they sit
alongside the open core rather than gating it.

## Alternatives
Neon, PlanetScale (managed, proprietary, the model this clones in
self-hosted form). No other self-hosted ZFS-clone-based branching tool was
found already catalogued here.

## Risks / Limitations
- 4 days of public history — no track record for correctness under real
  production-scale data or failure scenarios yet.
- ZFS-on-Linux is a real infrastructure prerequisite; this is not a drop-in
  addition to an existing non-ZFS Postgres/MySQL host.
- Self-labeled Beta with an explicit "APIs may still change before 1.0"
  warning.
- Paid modules cover exactly the features (audit, history/point-in-time
  recovery) most relevant to a compliance-sensitive use case — worth
  pricing out before assuming the free core covers an MSSP-grade workflow.

## Recommendation
PROTOTYPE — the branching mechanism and MCP integration are worth testing
against a real Postgres database on a ZFS-backed dev box, with the
expectation that this is a beta product from a brand-new public repo, not
a battle-tested one yet.

## Change History
### 2026-11-17
Initial discovery and review. Slot 7 (experimental projects and hidden gems)
run.

### 2026-11-24
Meaningful update: v0.2.5 -> v0.3.1. Added database-name validation to
prevent DSN-parameter injection across the query API, MCP `query` tool, and
Web SQL runner; fixed a `sqlquery` panic on empty/comment-only input; and
sped up `branch merge` by skipping row-by-row comparison for unchanged
tables. A real correctness/security hardening pass rather than a routine
patch bump. Recommendation unchanged (PROTOTYPE).
