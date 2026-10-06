# SparrowDB

## Summary
SparrowDB positions itself as "the SQLite of graph databases" — an embedded, Cypher-native graph engine with no server process, written in Rust with Python, Node.js, and Ruby bindings. Zero infrastructure: open a file path, run Cypher queries, done.

## Why I Should Care
Reaching for Neo4j (or a managed graph-DB service) for a small embedded graph need — permissions trees, recommendation edges, a dependency graph inside an internal tool — is usually overkill. An embedded, file-backed graph DB with a Node binding is a genuinely underserved niche for a MERN-leaning stack that occasionally needs real graph traversal without standing up a graph server.

## Problems It Can Remove
- Removes the need to run a Neo4j instance (or pay for a managed one) for graph needs that are a feature inside an app, not the whole app.
- Removes hand-rolled recursive-CTE graph traversal in Postgres/Mongo for the cases where a real graph query language is a better fit.

## Practical Uses
- Embed inside a Node service that needs permission-graph or org-chart traversal without a separate database to operate.
- Prototype a recommendation-graph feature locally before deciding whether it's worth a dedicated graph database in production.
- Use from Python or Ruby tooling that needs the same embedded-graph behavior.

## Product Opportunities
Could underpin a lightweight "related items" or permissions-graph feature inside a product without adding a graph-DB line item to infrastructure costs.

## Agent / Automation Opportunities
None documented specifically for agent/MCP use yet; it's a general-purpose embedded database, usable as a building block inside an agent's tool belt if given a wrapper.

## Integration
`cargo add sparrowdb`, or the Python/Node.js/Ruby bindings. No server to deploy. Integration effort: **Low** to try, but see Maturity below before depending on it for anything with real data at stake.

## Architecture Notes
`GraphDb::open` takes an exclusive, process-wide lock on the database root — only one process may hold a given database open at a time, enforced with an immediate, clean error rather than silent corruption. This is a recent fix: the README documents, in detail, that versions 0.1.26 and earlier had no lock file and no guard at `open()` at all, and that concurrent writers from two processes corrupted the catalog file beyond recovery in 4 of 5 concurrent runs. The fix is now shipped (v0.1.27); the transparency about the bug and its blast radius is a better signal about the project's engineering culture than the fix itself.

## Maturity
Experimental / pre-1.0, explicitly labeled "building in public" in the README. Created March 2026, latest release v0.1.27 (August 2026), 79 stars, 6 forks, apparently single-maintainer. The documented data-corruption bug in a 0.1.x release is a real signal to weigh — this is early software.

## License
MIT — confirmed from the raw LICENSE file. No commercial-use restrictions.

## Alternatives
Neo4j (full server, way more than needed for an embedded use case), KuzuDB and other embedded graph databases (more established, worth comparing before committing), or just modeling the graph in Postgres/Mongo with recursive queries. SparrowDB's bet is specifically on an SQLite-shaped embedding experience with real Cypher, which none of the Postgres/Mongo workarounds give you.

## Risks / Limitations
- Pre-1.0 with a documented catastrophic data-corruption bug fixed only one release ago — do not use for anything without backups and a tolerance for breaking changes.
- Single apparent maintainer; small community means slow bug discovery/response outside of what the maintainer catches themselves.
- `crates.io` API lookup for this package failed during this review (known intermittent issue with that registry) — version claims here rely on the GitHub release tag, not an independently confirmed registry listing.

## Recommendation
WATCH — the embedded-graph-with-Node-bindings niche is genuinely useful, but the recency of a severe corruption bug means this needs a few more point releases and real-world mileage before it's worth trusting with anything that matters.

## Change History
### 2026-10-06
Initial discovery and review. Slot 7 (experimental projects and hidden gems) run.
