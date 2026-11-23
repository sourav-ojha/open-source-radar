# Greenmask

## Summary
Single-binary CLI for PostgreSQL (MySQL in beta) logical dumping, anonymization,
synthetic data generation and restoration. A stateless drop-in replacement for
pg_dump/pg_restore that adds deterministic (hash-based, reproducible) data masking,
referentially-intact database subsetting (including cyclic/polymorphic references),
dynamic/conditional transformation rules, and custom transformers in any language via a
CMD extension point.

## Why I Should Care
PII masking and right-sized dev/staging database subsets are recurring, error-prone tasks
that are easy to under-build by hand. This directly overlaps the care the MSSP/PTaaS side
of the work already requires around client data handling.

## Problems It Can Remove
Replaces hand-rolled Faker-style scripts that re-implement deterministic masking,
referential integrity preservation across foreign keys, and partition inheritance by hand.
Also a drop-in replacement for pg_dump/pg_restore with anonymization added transparently.

## Practical Uses
- Spin up a referentially-intact, right-sized subset of a production Postgres database for
  local development instead of restoring the full dataset.
- Mask PII in a production dump before handing it to staging, a contractor, or an
  MSSP/PTaaS engagement needing realistic-but-safe data.
- Generate synthetic data to populate an empty environment for demos or load testing.
- Drop into an existing pg_dump/pg_restore-based backup pipeline as an anonymization step.

## Product Opportunities
The anonymization + subsetting pipeline is a reusable building block for any internal
"give engineers a safe copy of prod" tool or client-data-handling workflow in the
MSSP/PTaaS context.

## Agent / Automation Opportunities
None identified; it's a CLI/pipeline tool, not agent-facing.

## Integration
Single binary or Docker/docker-compose. Low effort — operates on a logical dump, so no
in-place database access is required beyond the dump/restore step itself.

## Architecture Notes
The CMD-based custom-transformer extension point (write a transformer in any language,
invoked as an external command) is a clean way to add domain-specific masking logic
without forking the tool. Transformation inheritance for partitioned tables and FK
references avoids having to redeclare the same rule per partition.

## Maturity
Mature for PostgreSQL (explicitly "production ready" in its own docs); MySQL support is
explicitly beta. JetBrains-sponsored, active Discord/Telegram community, 85 forks, 63 open
issues.

## License
Apache-2.0. No restrictions on commercial or internal use.

## Alternatives
- **pg_dump/pg_restore** — no anonymization or subsetting.
- **Faker-style custom scripts** — reinvents deterministic masking and referential
  integrity by hand.
- **Delphix, Tonic.ai** (commercial) — broader multi-database support, not free/self-hosted
  at this scale.

## Risks / Limitations
- MySQL support is explicitly beta; PostgreSQL is the only production-ready engine.
- No native MongoDB support — only covers the Postgres half of a MERN stack.
- Operates on a dump, not an in-place/live-database masking proxy.

## Recommendation
PROTOTYPE — worth a real trial against a Postgres dataset needing PII masking or subsetting
before adopting as a standard step in dev/staging provisioning.

## Change History
### 2026-11-23
Initial discovery and review. Catalogued PROTOTYPE, 7.5/10.
