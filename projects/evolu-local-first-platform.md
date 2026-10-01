# Evolu

## Summary
A TypeScript local-first platform: a typed, SQLite-backed local data store with built-in end-to-end encryption and CRDT-based sync across devices via a relay server. Ships bindings for React, Next.js, Svelte, Vue, and React Native/Expo, aimed at apps that work fully offline and sync when connectivity returns, without a hand-built backend sync layer.

## Why I Should Care
Offline-first product features (field service tools, inventory apps, note-taking) are a recurring product category, and Evolu removes the sync-engine problem — conflict resolution, encryption, relay protocol — that normally takes weeks to build correctly.

## Problems It Can Remove
Replaces hand-building an offline-sync layer: conflict resolution logic, end-to-end encryption, and a relay protocol for cross-device sync.

## Practical Uses
- Offline-first product features that must keep working without a network connection
- End-to-end-encrypted sync without standing up and securing a custom backend for it
- PWA or mobile apps where instant local reads/writes matter more than server round-trips

## Product Opportunities
Removes the sync-engine problem for an entire recurring product category — offline-capable client apps — that would otherwise require weeks of custom CRDT/conflict-resolution work before a product idea can even be validated.

## Agent / Automation Opportunities
None identified this run — this is a client-data-layer tool, not an agent-facing one.

## Integration
npm packages per framework (React, Next.js, Svelte, Vue, React Native). Cross-device sync needs a self-hosted relay server as an additional operational component. Integration effort: **Medium** — straightforward for the local-only case, more involved once cross-device sync and the relay server are in scope.

## Architecture Notes
Not deeply inspected this run — the top-level README points entirely to the docs site (evolu.dev) rather than documenting the sync/encryption model inline. Worth a closer architectural read there before adoption.

## Maturity
Emerging-to-established. Created September 2022, 1,899 stars, 73 forks, 15 open issues (low relative to stars — a healthy signal). npm `@evolu/common` at 8.14.0, confirming frequent, active publishing despite the thin GitHub README.

## License
MIT, verified via GitHub API. No restrictions on commercial use, SaaS deployment, or embedding.

## Alternatives
- **RxDB** (mainstream-adjacent local-first database, ~23k stars) — broader ecosystem and replication targets, but Evolu differentiates on built-in end-to-end encryption and a smaller, SQLite-native, typed-schema surface.
- **Yjs/Liveblocks** (previously evaluated; document/rich-text CRDT focus) — Evolu instead targets a typed relational/SQLite data model, closer to "offline Postgres" than "offline Google Docs."
- **Supabase** (mainstream, excluded from fresh discovery) — a hosted Postgres+realtime service; Evolu is local-first by default rather than server-first with sync bolted on.

## Risks / Limitations
- Top-level README is thin on product description; verify current scope and stability on evolu.dev before committing.
- Relay server needed for cross-device sync — another self-hosted component to operate.
- Smaller ecosystem than RxDB; fewer production war stories to learn from.

## Recommendation
**STUDY** — the architecture (typed local-first SQLite + built-in E2E encryption) is worth understanding even without an immediate adoption target; revisit if a specific offline-capable product idea comes up.

## Change History
### 2026-10-01
Initial discovery and review. Found via GitHub Search API CRDT/realtime-collaboration query, slot 2 (product infrastructure) run.
