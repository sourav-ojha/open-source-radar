# ObjectStack

## Summary
An Apache-2.0 ontology-first app framework: define an app's data model, UI, workflows and
permissions as typed Zod metadata, and a microkernel derives the database (Postgres,
MySQL, SQLite or MongoDB), a REST API, a UI, and an MCP server from it. Positioned
explicitly so a whole app's business logic fits inside a coding agent's context window (a
complete CRM claimed under 150k tokens).

## Why I Should Care
If the metadata-to-full-app claim holds up, it could meaningfully shrink the path from
"idea" to a shippable CRUD/admin backend — directly relevant to the micro-SaaS axis — and
it's designed explicitly to be operated by a coding agent rather than retrofitted for one.

## Problems It Can Remove
Removes a large amount of hand-written CRUD backend, REST API, permission/RLS/FLS, and
admin-UI boilerplate for internal tools or a new product's early backend.

## Practical Uses
- Spin up an internal CRUD admin app (e.g. a lightweight CRM or ops tool) from a typed
  spec instead of hand-building backend, UI and permissions
- Hand a coding agent a whole app's business logic in one context window for safe,
  whole-app refactors
- Prototype a micro-SaaS backend fast by describing the domain model rather than
  scaffolding Express/Mongoose by hand

## Product Opportunities
Could meaningfully cut build time for the admin-panel/CRUD-heavy internals of a new
micro-SaaS idea; also fits a "forward-deployed engineer" style of client work — model a
client's business as typed metadata and hand over a governed, ownable app defined in
ordinary files in their own repo.

## Agent / Automation Opportunities
The runtime auto-generates an MCP server from the same typed metadata used for the
database/API/UI, so an agent can operate the running app over MCP directly. Project
scaffolding also writes an `AGENTS.md` so a coding agent starts with the protocol's rules
already loaded.

## Integration
`npm create objectstack@latest my-app`, then describe the app to a coding agent (Claude
Code or similar) which authors the typed metadata. Medium-to-high effort in practice: this
is a real buy-in to ObjectStack's metadata protocol rather than a drop-in library — closer
to adopting a framework than installing a package.

## Architecture Notes
A microkernel compiles typed Zod metadata (objects, workflows, views, policies) into a
versioned JSON artifact, then loads plugins/drivers/services from it to generate the REST
API, client SDK, Console/Studio UI, and MCP tools — all governed by auth, RBAC, RLS, FLS
and audit enforced at the kernel level rather than scattered through hand-written code.
Claims 6,507 passing tests.

## Maturity
Emerging. Created 2026-01-18 (~8 months old), 27 contributors, active development, but
only 50 stars despite the ambition and test-suite size — real-world adoption is unproven.

## License
Apache-2.0 for the full open stack (protocol, microkernel, SDK, CLI, production runtime),
with no open-core gating claimed in the README. A separate commercial product, ObjectOS —
a paid, browser-hosted runtime built on the same open stack — exists alongside it; it
doesn't restrict the OSS core today but is worth watching for future open-core pressure.

## Alternatives
AdminForth (already catalogued — admin-panel generator, but code-first, not
ontology-first); Retool and similar internal-tool builders (proprietary, hosted);
hand-rolled NestJS/Express + Prisma/Mongoose (the default approach today).

## Risks / Limitations
- Only 50 stars despite the ambition and claimed test coverage — real-world adoption is
  unproven
- Buying into a foreign metadata protocol is a bigger commitment than this profile's
  stated bias toward tools with a plain API/CLI/HTTP surface — worth a small prototype
  before committing a real project
- A paid ObjectOS product exists alongside the OSS core; watch for open-core pressure over
  time even though the current license is clean

## Recommendation
STUDY — the architecture (typed metadata → full generated app, agent-operable via MCP) is
worth learning from regardless of direct adoption; a small non-critical prototype would be
the right way to test whether the buy-in pays off before using it for anything real.

## Change History
### 2026-09-06
Discovered during slot 5 run (product-building/AI-agent-infrastructure find outside the
slot's primary theme, kept per AGENT.md's "exceptional find outside it" guidance).
