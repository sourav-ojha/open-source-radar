# Dialog DB

## Summary
Dialog is an embeddable database for local-first software, built around a Datalog-style "schema-on-read" query API rather than a fixed relational schema. It targets both WebAssembly and native runtimes, with an explicit design emphasis on efficient replica synchronization and user-centered data authority. Status is explicitly "Experimental" in the README — breaking changes to binary encoding and index construction are expected, with no migration path promised yet.

## Why I Should Care
Local-first sync (offline-capable apps that reconcile state across devices without a central server as the source of truth) is a real architecture pattern worth understanding even where I wouldn't adopt this specific project yet. Dialog's Datalog-esque query model is a meaningfully different approach from the CRDT-document sync seen in Yjs/Automerge or the SQLite-replication approach in Evolu (already catalogued) — worth comparing against those when evaluating any local-first feature.

## Problems It Can Remove
Nothing today in production — it's pre-production. The value right now is architectural: understanding a schema-on-read, Datalog-based approach to local-first sync before deciding whether a future, more stable version (or its ideas) belongs in a product.

## Practical Uses
- Study the design docs (`/guide`, published at dialog.foundation/guide) as a reference for schema-on-read query design in a sync context.
- Prototype against it in a throwaway project to evaluate the Datalog query ergonomics in TypeScript/React before any production commitment.

## Product Opportunities
None yet — too early. If it stabilizes, a Datalog-native local-first layer would be relevant to any product feature needing offline-capable, multi-device sync without a central database as the single source of truth.

## Agent / Automation Opportunities
None documented. This is a data-layer project, not an agent tool.

## Integration
TypeScript/React packages exist alongside the Rust core; both WASM and native runtimes are supported. Integration effort: **High** right now — explicitly unstable binary format and no migration guarantees make it unsuitable for anything beyond a spike.

## Architecture Notes
The project opens with a Ted Nelson quote about being "a prisoner of each application" and conventions that could be better — the stated goal is genuinely to rethink schema/query conventions for local-first software, not just to ship another sync engine. Primary contributor Gozala (Irakli Gozalishvili, currently at Tonk Labs per their GitHub profile) has a public history of distributed/local-first systems work, which is a useful credibility signal for a project with this little traction yet (181 stars, 12 forks, 1 watcher).

## Maturity
Experimental, by the maintainers' own explicit label. Created March 2025, actively pushed (latest tag `tonk-2026-10-03`), but no tagged releases in the conventional sense and 73 open issues against 181 stars — a lot of open surface area for its size.

## License
MPL-2.0 — a weak/file-level copyleft license. Unlike GPL/AGPL, MPL permits combining Dialog's code with proprietary code in a larger work; only modifications to MPL-licensed files themselves must be shared back. Not a blocker for embedding, but worth re-checking the license terms before any commercial redistribution once the project matures.

## Alternatives
Evolu (already catalogued, STUDY) — a more SQLite-shaped local-first sync platform; Yjs/Automerge for CRDT-document sync. Dialog's distinction is the Datalog/schema-on-read query model rather than a document- or table-shaped sync unit.

## Risks / Limitations
- Explicitly experimental with no migration path for breaking changes — do not persist real data in it yet.
- High open-issue count relative to stars suggests either active churn or unresolved rough edges; worth reading the issue tracker before any deeper investment.
- Small team, low external validation so far (1 watcher despite 181 stars is an unusual ratio worth noting, though not on its own evidence of anything adversarial here given the identifiable, credible primary contributor).

## Recommendation
STUDY — the Datalog-based schema-on-read approach to local-first sync is worth understanding architecturally; not ready for adoption.

## Change History
### 2026-10-06
Initial discovery and review. Slot 7 (experimental projects and hidden gems) run.
