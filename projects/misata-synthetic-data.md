# Misata

## Summary
A relational synthetic-data engine (Python, MIT) that inverts the usual synthetic-data
workflow: instead of imitating existing data, you declare an aggregate outcome — a
revenue curve, a fraud rate, a churn curve — and Misata solves for individual rows that
hit it to $0.00 error via a closed-form statistical mechanism, while guaranteeing zero
orphan foreign keys across multi-table schemas. Backed by an arXiv preprint describing
the underlying "declarative outcome-conformant synthesis" method.

## Why I Should Care
Every hand-rolled seed script or Faker-based fixture generator has the same two gaps:
no relational integrity across tables, and no way to make aggregates hit a specific
number. Misata closes both, plus ships domain-tuned schemas (SaaS, fintech, healthcare,
logistics, e-commerce, and 15 more) so a demo database looks authored rather than
randomly generated.

## Problems It Can Remove
- Hand-written seed scripts for staging/demo databases that break referential integrity
- Manually reconstructing "realistic" aggregate numbers for dashboard demos
- dbt/Spark/SQL transform tests where the expected output has to be computed by hand
- Ad hoc anonymization scripts for sharing sensitive CSVs (via `misata.mimic`)

## Practical Uses
- `misata generate --story "..."` for an instant multi-table CSV set with a verified
  `oracle_report.json` (zero orphan FKs, constraint satisfaction, statistical fidelity)
- `misata seed` against a live staging Postgres/MySQL/SQLite database, introspecting
  existing FKs/constraints/enums first
- Feeding a Prisma or dbt schema directly as the generation target
- Running the bundled MCP server so Claude Code/Cursor/Windsurf get an isolated,
  pre-seeded SQLite sandbox to test SQL against in ~2 seconds

## Product Opportunities
Demo-data-as-a-service inside a SaaS onboarding flow — populate a new tenant's
dashboard with a convincing, coherent dataset instantly instead of an empty state.

## Agent / Automation Opportunities
The native MCP server (`create_sandbox`) is a first-class agent tool, not something
that needs wrapping — a coding agent gets a schema-correct database to validate against
without touching production.

## Integration
`pip install misata`, optional `pip install "misata[mcp]"` for the MCP server. No
external services required; runs entirely locally. Low integration effort.

## Architecture Notes
The "exact outcome conformance" mechanism (closed-form Gamma conditional-sum solver) is
the interesting part — most synthetic-data tools either sample from a distribution
(random, doesn't hit a target) or fit an ML model to real data (needs real data).
Solving in reverse for rows that sum exactly to a declared target without either is a
genuinely different approach, described in arXiv:2606.08736.

## Maturity
Emerging. Created 2025-12-16, 69 stars, 3 forks. Very active — releases climbing daily
(v0.9.6.51 → v0.9.6.60 within three weeks as of this review), pre-1.0.

## License
MIT. No commercial restrictions. Verified via the repo's LICENSE file.

## Alternatives
- **Faker** — field-level only, zero relational or statistical guarantees.
- **SDV (Synthetic Data Vault)** — ML-based imitation, needs real data to train on.
- **seedfaker** (`opendsr-std/seedfaker`) — polyglot Rust core (Python/Node/Go/PHP/
  Ruby/WASM/CLI, all byte-identical for a given seed), GB/TB-scale distributed
  generation with no coordinator, and production-data anonymization (`replace`) that
  preserves FK integrity across files — genuinely different strengths, but stuck at
  v0.4.0-alpha with no tagged release since 2026-05-03 despite active commits. Filed as
  Worth Watching rather than catalogued.

## Risks / Limitations
- Solo maintainer (448 of ~450 commits) — real bus-factor risk.
- Rapid pre-1.0 patch churn suggests the API may still change.
- The exact-conformance benchmark numbers are the author's own, not independently
  verified.

## Recommendation
PROTOTYPE — genuinely differentiated and directly useful for demo data, CI fixtures,
and agent SQL sandboxes. Try it on one real staging environment or demo before relying
on it; watch the bus-factor risk before anything production-critical.

## Change History
### 2026-09-29
Initial discovery and review. Catalogued as PROTOTYPE, 8.4/10.
