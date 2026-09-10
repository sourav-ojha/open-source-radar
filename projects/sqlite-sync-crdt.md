# SQLite Sync

## Summary
SQLite Sync is a multi-platform SQLite extension that turns a local SQLite database into
a conflict-free, offline-first replica using CRDTs (conflict-free replicated data
types). It syncs to SQLite Cloud, plain self-hosted PostgreSQL, or self-hosted Supabase —
no central coordinator or custom sync protocol required, no vendor lock-in to the
maker's paid cloud product.

## Why I Should Care
"Multiple processes/devices/agents write to a local SQLite database and need it to merge
without conflicts" is a real recurring problem — for offline-first app data and,
increasingly, for AI-agent memory/state shared across instances (a theme that has
surfaced repeatedly in this catalog's recent "agent memory" discovery waves). A drop-in
CRDT sync extension is meaningfully less work than building or adopting a bespoke sync
protocol, and it's directly relevant to the AI-agent-leverage axis of this radar.

## Problems It Can Remove
- Building a custom conflict-resolution/sync protocol for offline-first mobile, desktop,
  or edge apps.
- Coordinating shared state across multiple AI-agent instances (e.g. agent memory or a
  shared markdown knowledge base) without a central lock or coordinator.
- Manual conflict resolution for concurrently-edited local data.

## Practical Uses
- Sync a local SQLite database used by a desktop or edge tool to a self-hosted Postgres
  instance without writing sync logic.
- Give multiple AI-agent instances a shared, conflict-free local database (memory, task
  state) that merges automatically instead of requiring explicit coordination.
- Block-Level LWW (last-write-wins) mode is specifically designed to let multiple agents
  edit different sections of the same markdown file and merge cleanly — directly
  relevant to multi-agent workflows writing to shared docs.

## Product Opportunities
- Sync engine for an offline-capable client (desktop/mobile companion app) of a product,
  without standing up a bespoke real-time sync backend.

## Agent / Automation Opportunities
- Directly positioned by the maintainers for AI-agent memory sync and multi-agent
  markdown collaboration — worth testing against any multi-agent setup that currently
  uses ad-hoc file locking or a single shared memory store.

## Integration
Ships as a loadable SQLite extension (one function call to enable) plus documented
quickstarts for plain PostgreSQL and self-hosted Supabase as sync targets — genuinely
usable without their paid SQLite Cloud product. Integration effort: **Low** for adding
sync to an app already using SQLite; **Medium** to stand up a self-hosted Postgres-based
sync target correctly.

## Architecture Notes
CRDT-based merge with "no central coordinator required" when syncing peer-to-peer, plus
a distinct Block-Level LWW mode aimed specifically at concurrent markdown editing — a
narrower and more purpose-built design than general document CRDTs (Yjs/Automerge),
worth studying even independent of adoption for the block-level LWW idea specifically.

## Maturity
Actively developed by SQLite Cloud, Inc. (the commercial vendor behind SQLite Cloud) as
part of a family of SQLite extensions (sqlite-vector, sqlite-ai, sqlite-agent,
sqlite-memory, sqlite-mcp). 558 stars, pushed_at 2026-09-07, only 2 open issues. Younger
than most infrastructure in this catalog (created 2025-05-13) but backed by a funded
team rather than a solo maintainer.

## License
**Elastic License 2.0 (modified) — flagged loudly, not an OSI-approved open-source
license.** Confirmed via the repository's LICENSE.md. Same practical restriction class
as Convoy (reviewed the same run): source-available, but offering it as a competing
hosted/managed service is restricted. This is also explicitly an open-core funnel toward
the vendor's paid SQLite Cloud product — the self-hosted Postgres/Supabase path is real
and documented, but evaluate with that commercial incentive in mind.

## Alternatives
Yjs / Automerge (general-purpose CRDT libraries for collaborative apps, JS-native, no
SQLite-specific integration) are the closest broad alternative if the goal is generic
real-time collaboration rather than SQLite-specific offline sync. For agent-memory
specifically, this catalog already tracks mex (living-codebase wiki) and multiple
recently-rejected "agent memory" entries (2026-09-09) — sqlite-sync's CRDT-sync
architecture is a genuinely different approach from those (sync engine vs. memory
store) rather than a direct duplicate.

## Risks / Limitations
- Elastic License 2.0 restricts hosting it as a competing service — see License above.
- Backed by a company selling a paid cloud product in the same space; verify the
  self-hosted path stays fully functional (not artificially limited) before depending on
  it.
- Young project (16 months old) relative to most infrastructure in this catalog.

## Recommendation
STUDY — the CRDT/Block-Level-LWW architecture is worth understanding and testing against
a multi-agent shared-memory or offline-first prototype, but the Elastic License and
open-core commercial incentive make it premature to standardize on for anything beyond
experimentation right now.

## Change History
### 2026-09-10
Initial discovery and review. Slot 2 (product infrastructure, APIs, backend components) run.
