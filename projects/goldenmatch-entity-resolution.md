# GoldenMatch

## Summary
A zero-config entity-resolution library (MIT; Python, with a TypeScript/WASM edge build and native Postgres/DuckDB SQL support) that deduplicates messy records — the same person, company, or product entered inconsistently across sources — into "golden records," without writing matching rules or training a model.

## Why I Should Care
Entity resolution (merging "Maria Gonzalez" / "Maria Gonzales" / "María González" into one customer) is a recurring, genuinely hard problem in any product that ingests data from multiple sources (CRM imports, multi-channel signups, support tickets, billing records) — the kind of thing that's easy to get 80% right with naive fuzzy matching and very hard to get the last 20% right. GoldenMatch packages auto-tuned matching (exact + fuzzy + blocking) behind a one-line call, with the matching config it chose exposed and reusable — useful as a building block in a micro-SaaS product or an internal admin tool that needs a "merge duplicate customers" feature.

## Problems It Can Remove
- Hand-rolled fuzzy-matching/dedup logic (typically a fragile pile of `Levenshtein` + ad hoc rules) for customer/contact dedup.
- The need to hand-tune or train a matching model (e.g. Splink) before getting usable results on a new dataset.

## Practical Uses
- Deduplicate a customer/contact CSV or database table before an import or a CRM migration.
- Build a "possible duplicate" review queue in an admin panel using `result.clusters` and `result.config` to show why two records were (or weren't) matched.
- Feed cleaned, deduplicated records into downstream analytics so a "how many customers do we actually have" dashboard isn't double-counting.
- Use the documented REST/MCP surface to expose dedup as an internal service callable from other systems or agents.

## Product Opportunities
A natural micro-SaaS angle: "data cleaning as a service" for small businesses with messy CRM/spreadsheet customer data — this library could be the matching engine behind such a product, with a thin UI wrapped around `gm.dedupe()`.

## Agent / Automation Opportunities
Claims 70+ MCP tools and a documented MCP server (`io.github.benseverndev-oss/goldenmatch` per the README's `mcp-name` marker) — not independently exercised in this review, but if accurate, it would let an agent run dedup/entity-resolution tasks directly as tool calls.

## Integration
- `pip install goldenmatch` or the npm package; also usable natively from SQL in Postgres/DuckDB per the README.
- **Integration effort: Low** for the Python/CSV path; verify the TypeScript/WASM and SQL-native paths directly before depending on them, since they weren't exercised in this review.

## Architecture Notes
Arrow-native, with a Rust-authoritative core (`goldenmatch-native`) for the performance-sensitive matching kernels (claims to outperform `rapidfuzz`/`jellyfish`/FAISS on its own benchmarks, published in `docs/benchmarks/`). Auto-config picks exact vs. fuzzy matching per field and a blocking key (e.g. match on `zip` before comparing names), and returns that chosen config as a reusable, inspectable object rather than a black box — a good pattern for any "automatic but explainable" matching system.

## Maturity
v3.22.0 on PyPI (2026-09-26), active CI with Codecov and an OpenSSF Scorecard badge, published benchmark docs comparing against Splink and a Spark head-to-head. 132 stars / 15 forks / 1 watcher — small community, and the README leans heavily on badges and bold performance claims ("beats hand-tuned Splink," "250M rows in 11.2 min") across a sprawling family of sibling packages (`goldenflow`, `goldencheck`, `goldenpipe`, `goldengraph`, etc.) from the same single maintainer — worth independently verifying the headline benchmark claims before relying on them, rather than taking the README at face value.

## License
MIT, confirmed via `LICENSE` file.

## Alternatives
- **Splink** (UK Ministry of Justice) — the established probabilistic record-linkage library; GoldenMatch's own benchmarks claim to beat it on messy real-world data without manual tuning, but Splink has far more production track record and community scrutiny.
- Hand-rolled fuzzy matching (the status quo it replaces).

## Why This One
Zero-config operation and an inspectable/reusable matching config are the real differentiators versus Splink's manual-tuning model — valuable specifically when there's no time to become a record-linkage expert before shipping a "merge duplicates" feature.

## Risks / Limitations
- Single-maintainer project with an unusually large family of sibling packages and heavy marketing-style README language — treat the specific performance claims (vs. Splink, vs. rapidfuzz) as unverified until tested on real data, not as established fact.
- Small community (132 stars, 1 watcher) — limited independent validation so far.
- The MCP server and SQL-native claims were read from the README, not exercised directly in this review.

## Recommendation
PROTOTYPE — worth trying directly on a real messy dataset (e.g. a CRM export) to independently verify the zero-config matching quality before adopting it as a dependency in anything production-facing.

## Change History
### 2026-10-05
Initial discovery via GitHub Search API during slot 6 research. Confirmed MIT license and PyPI release currency; benchmark claims not independently reproduced.
