# TinyBase

## Summary
A reactive, typed data store and sync engine for JavaScript/TypeScript (MIT), with
pluggable persisters to SQLite, Postgres, SQL Server/Azure SQL, Cloudflare Durable
Objects, and CRDT-based sync (Yjs/Automerge). As of v10.0.0 (2026-09-24), adds a
persister to TinyJoin — a tiny relational database that runs entirely in the browser,
backed by OPFS.

## Why I Should Care
Most React state libraries (Redux, Zustand) stop at in-memory state and leave
persistence/sync as an exercise for the app. TinyBase treats persistence and
multi-backend sync as first-class, which removes an entire category of glue code
rather than being a one-for-one Redux replacement.

## Problems It Can Remove
- Hand-rolled optimistic-update/reconciliation logic for local-first app state
- Bespoke sync layers between client state and a server database
- Pulling in a full SQL engine just to run relational queries in the browser

## Practical Uses
- Local-first app state that syncs to a server database without custom reconciliation
- Offline-capable admin panel or internal tool backed by SQLite/Postgres via a
  persister
- In-browser relational queries via the new TinyJoin persister
- Replacing Redux/Zustand plus hand-written API-sync boilerplate with one reactive
  store that already persists

## Product Opportunities
Fits as the state/sync layer inside a self-hosted admin panel or offline-tolerant
field-ops tool where a full backend round-trip per interaction isn't acceptable.

## Agent / Automation Opportunities
None built in today. The reactive store's fine-grained subscriptions would make a
clean substrate for an agent-driven UI to observe state changes, but nothing is
pre-wired for that.

## Integration
`npm install tinybase` plus the specific persister package needed (e.g.
`tinybase/persisters/persister-tinyjoin`, `persister-mssql`). Medium integration effort
— adopting it is an architectural decision (the app's state+sync layer), not a
drop-in utility, and picking the right persister for the target backend takes some
evaluation.

## Architecture Notes
Fine-grained reactive subscriptions plus a pluggable persister abstraction (10+
backends, each with its own conflict/merge semantics) is the interesting design. The
new TinyJoin persister — a worker-first relational database that runs entirely in the
browser via OPFS — is worth a separate look as its own project.

## Maturity
Mature. Created 2021-12-31, 5,179 stars, 134 forks, pushed same day as this review.
v10.0.0 released 2026-09-24. Effectively a solo-maintained project (jamesgpearce:
4,585 of ~4,620 commits) but with four years of consistent release cadence.

## License
MIT. Unrestricted for commercial/SaaS use.

## Alternatives
- **Zustand/Redux + hand-rolled sync** — no persistence or multi-backend sync built
  in.
- **RxDB** — heavier, a full offline-first database rather than a lightweight reactive
  store.
- **Yjs/Automerge directly** — CRDT-only, no relational-store abstraction on top.

## Risks / Limitations
- Single maintainer — real bus-factor risk despite the long track record.
- Adopting it is an architectural commitment, not a drop-in utility.
- TinyJoin (the new browser-relational-DB persister) is a separate, newer project;
  treat that specific integration as less proven than the core store.

## Recommendation
STUDY — the persistence-and-sync-as-first-class design is worth understanding as a
reference architecture for local-first React apps; PROTOTYPE if a specific upcoming
feature needs offline-tolerant state with real backend sync.

## Change History
### 2026-09-29
Initial discovery and review. Catalogued as STUDY, 7.3/10. Noted the v10.0.0 release
(TinyJoin browser-relational persister, MSSQL persister) as the trigger for this
review.
