# AntelopeJS DMS

## Summary
An open-source "back office" / admin-panel toolkit for TypeScript products. Connects to
an existing PostgreSQL, MySQL, MariaDB, MongoDB, or RethinkDB database, and a coding
agent (Claude Code or Codex, run on the user's own account) generates the admin pages,
dashboards, and CRUD screens as readable TypeScript in the project's own repo — built
from 41 typed building blocks (tables, forms, KPI cards, 13 chart types) rather than
freehand agent-generated UI code.

## Why I Should Care
This is admin-panel generation aimed squarely at Sourav's stack: Mongo and Postgres are
both first-class backends, the output is TypeScript he reviews and owns (not a hosted
no-code platform), and the stated problem — "every client project needs a back office,
no two alike, rebuilt by hand every time" — is exactly the recurring cost a MERN
freelancer/CTO-track developer hits on every new product or client engagement.

## Problems It Can Remove
Removes hand-building: list/table pages with search, filters, and saved views; CRUD API
routes with per-action permission checks; role-based access down to individual
page/block/action; sign-in, members, invitations, and multi-tenancy; and dashboard/KPI/
chart screens — the standard 80% of an admin panel that's tedious but not interesting to
build from scratch each time.

## Practical Uses
- Stand up a working back office for a new product or client project from a database
  connection string in one coding-agent run, then hand-edit the generated TypeScript for
  anything agent-specific that needs fixing (the README itself documents gaps: filtered
  saved views and multi-relation charts needed follow-up).
- Use as the internal admin tool for an existing Mongo/Postgres-backed product instead
  of building bespoke internal dashboards per project.
- Start a brand-new product's admin UI from the included template instead of from zero.

## Product Opportunities
Directly useful for exactly the kind of client/consulting admin-panel work the "problem
to remove" section describes, and as the internal admin layer for any new micro-SaaS
product built on the existing stack — shortens the idea-to-shippable-product path that
is one of this radar's two primary relevance axes.

## Agent / Automation Opportunities
The entire value proposition is agent-driven generation: agent skills ship inside the
npm packages so the coding agent knows the current version's API, every building-block
option is typed so a wrong choice fails the build rather than silently misrendering, and
nothing is written to the repo unless the whole project still typechecks. Worth studying
as a reference architecture for "constrain an agent to a safe, typed vocabulary of
building blocks" rather than giving it a blank file and hoping.

## Integration
Requires Node.js, Git, and Docker Compose v2. `npx @antelopejs/core project init` to
scaffold, `docker compose up` to run; connecting an existing database is a pasted
connection string. Stores its own operational data in MongoDB by default (MySQL/MariaDB
also supported via an add-on package). Integration effort: Low to try against a sample
or non-critical database; Medium to adopt as the admin layer for an existing production
database, since the generation step still needs human review per the project's own
documented limitations.

## Architecture Notes
The core bet is "coding agent plus a typed, bounded component library" instead of
"coding agent against a blank repo." The README's own honest failure report (saved views
with multi-status filters came back empty; multi-relation charts were skipped, and the
agent said so) is a more credible maturity signal than a typical all-success demo would
be. 14 API routes are generated per connected table (list/get/create/edit/delete/
archive/counts/export), each checked against a declared permission.

## Maturity
Early (v0.21.3, created September 2026, ~2 months old at review). Small but real
multi-person team: top contributor `Upd4ting` (696 commits) plus `mwesto` (35),
`alessaloisio` (31), and several smaller contributors — not a solo project, but pre-1.0
with an API that should be expected to shift.

## License
Apache-2.0. No restrictions on commercial use, self-hosting, or embedding.

## Alternatives
- **Retool**, **Forest Admin**, **Appsmith**, **Budibase** — hosted or self-hosted
  no-code/low-code admin builders; none generate reviewable TypeScript the team owns and
  extends like a normal part of the codebase.
- **AdminJS**, **Refine** — Node-ecosystem admin frameworks closer in spirit, but without
  the coding-agent-driven generation step or the typed building-block constraint system.
- Hand-built admin panels per client project — the status quo this explicitly targets.

## Risks / Limitations
- Pre-1.0 (v0.21.x) — expect breaking changes and treat generated output as needing
  review, not blind trust, consistent with the project's own stated limitations.
- Only 27 stars and 2 subscribers at review time; too early to judge long-term
  maintenance trajectory.
- Generation quality is only as good as the underlying coding agent's run; the project's
  own demo reports partial failures on a moderately complex schema (12 tables).
- RethinkDB support is a maturity flag in the other direction — a declining database to
  support as a first-class backend.

## Recommendation
PROTOTYPE — worth running against a real (non-production) database to see how much of
an existing admin workflow it can cover before adopting it for a client engagement or
new product; the typed-building-block architecture is also worth studying independent
of whether the tool itself gets adopted.

## Change History
### 2026-11-19
Discovered via GitHub topic search (`topic:admin-panel`) during slot 2 (product
infrastructure, APIs, backend components) discovery. Catalogued as PROTOTYPE, 7.9/10.
