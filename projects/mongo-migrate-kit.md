# mongo-migrate-kit

## Summary
A MongoDB migration toolkit for Node.js positioned as a "modern migrate-mongo alternative": single-file migration runs, rollback by batch/step/single-file/redo, dry-run previews that show the exact plan before anything touches the database, opt-in transactions, lifecycle hooks, SHA-256 tamper detection on applied migrations, and MongoDB-native locking to stop two deploys racing. Ships a CLI (`mmk`), a typed Node API (ESM/CJS/TypeScript), and a VS Code extension for browsing/applying/rolling back across local, staging and prod side by side.

## Why I Should Care
This is the MongoDB migration tool for exactly the MERN stack this radar is built for. migrate-mongo (the incumbent most MERN projects reach for) only supports "run everything pending" and has no dry-run, no checksum tamper detection, and no locking — mongo-migrate-kit adds all three plus granular rollback, which is the set of gaps that actually bite in a team setting.

## Problems It Can Remove
- Re-deriving rollback-by-batch/step/file and a dry-run preview on top of migrate-mongo by hand.
- Two deploys racing to run migrations simultaneously against the same database.
- A migration file silently edited after it was applied, with no way to detect it.

## Practical Uses
- Drop-in replacement for migrate-mongo in any existing Node/Mongo project (docs include a migration guide).
- `mmk dry-run up` before running migrations against a shared staging or prod database.
- `mmk down --batch 3` or roll back a single named file instead of the whole batch.
- Wrap a schema-shape-changing migration in a transaction so a partial failure can't leave the database half-migrated.
- VS Code GUI to compare pending migrations across local/staging/prod without switching terminals.

## Product Opportunities
Worth standardizing on across his own MERN products as the default migration layer, rather than re-solving rollback/locking/dry-run per project.

## Agent / Automation Opportunities
`mmk dry-run up` prints a plain-text plan, which is easy for a coding agent or CI step to parse before deciding whether to run migrations unattended.

## Integration
`npm install mongo-migrate-kit`, zero required config files (env-var driven if preferred). Low integration effort — CLI, library, and VS Code extension all install independently.

## Architecture Notes
Nothing exotic — the interesting bits are the correctness details: SHA-256 checksums recorded per applied migration (detects post-hoc edits), an atomic MongoDB-native lock document preventing concurrent runs, and rollback state tracked as "a batch, the last N, one file, or redo" rather than migrate-mongo's single linear history.

## Maturity
Emerging. Created 2026-06-01 (~5 months old), 21 stars, 1 fork, 1 subscriber, single maintainer. No GitHub release tags — npm (currently 1.6.0, published 2026-10-19) is the only versioned source. Has a Snyk vulnerability badge and published docs site.

## License
MIT — unrestricted commercial and SaaS use, embeddable in any product without restriction.

## Alternatives
migrate-mongo (less granular rollback, no dry-run, no tamper detection), umzug (framework-agnostic but more assembly required for Mongo specifically), Mongock (JVM-only, not relevant to a Node stack).

## Risks / Limitations
- Tiny project: 21 stars, 1 subscriber, single maintainer, 5 months old — bus-factor and long-term-maintenance risk.
- No tagged GitHub releases; version provenance runs through npm only.
- Worth a trial run on a side project before trusting it against a production database, despite the correctness features looking sound on paper.

## Recommendation
PROTOTYPE — the feature set is a genuine, specific upgrade over migrate-mongo for a Mongo/Node stack, but the project is new enough (21 stars, single maintainer) that it earns a real trial before being trusted on production data.

## Change History
### 2026-11-06
Discovered and reviewed. GitHub API verified: MIT, 21 stars/1 fork/1 subscriber, created 2026-06-01, pushed 2026-10-19, no GitHub release tags. npm registry confirms current version 1.6.0, published 2026-10-19.
