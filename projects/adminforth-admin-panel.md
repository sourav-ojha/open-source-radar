# AdminForth

## Summary
AdminForth is a TypeScript + Vue admin-panel framework: point its CLI at an existing
Postgres/MySQL/Mongo/SQLite/ClickHouse database and it scaffolds a full CRUD back-office
(filter, create, edit, remove) in minutes, extensible with Vue components, custom Express
API routes, and a plugin ecosystem (audit log, file/image upload, TOTP 2FA, i18n,
Copilot-style AI writing/image generation).

## Why I Should Care
Every SaaS product eventually needs an internal admin/back-office UI, and it is almost
always hand-built and then neglected. AdminForth generates that UI from the existing
schema instead of writing CRUD screens by hand, and it targets exactly the databases
already in use (Postgres, Mongo).

## Problems It Can Remove
Removes the recurring "build yet another internal CRUD admin" task — user management,
content moderation panels, ops dashboards — that otherwise eats days per product and then
bit-rots because nobody wants to touch it again.

## Practical Uses
- Scaffold an admin back-office for the MSSP admin-portal SaaS or a new micro-SaaS MVP
  directly against its existing Postgres/Mongo schema.
- Give non-engineering teammates (support, ops) a safe CRUD UI instead of direct DB access
  or ad hoc scripts.
- Use the built-in audit-log and TOTP 2FA plugins instead of building access control for
  an internal tool from scratch.
- Prototype an "AI-assisted back-office" (Copilot-style field writing/image generation
  plugin) as a differentiator for a client-facing admin product.

## Product Opportunities
Fast internal-tooling layer for any new product idea — cuts the admin-panel line item out
of a micro-SaaS build entirely, which is often the difference between shipping an MVP in a
weekend versus a week.

## Agent / Automation Opportunities
The CLI (`npx adminforth resource`, `npx adminforth component`) is scriptable, so a coding
agent could plausibly scaffold and extend admin resources as part of an automated build
step. No dedicated MCP server was found in the README — treat any deeper agent-tool claim
as **unclear based on available documentation** pending hands-on testing.

## Integration
`npx adminforth create-app --db <connection-string>` against an existing database, or let
it provision a new one. Requires Node.js 20+; Docker and pnpm are listed as dev
requirements. Runs as a standard Express + Vue app, so it deploys anywhere Node runs
(Docker container, VM, PaaS). **Integration effort: Low** — CLI-driven scaffolding against
an existing schema, no bespoke backend code required to get a working panel.

## Architecture Notes
Two initialization paths: point at an existing DB and schema (AdminForth infers
resources, you own migrations), or let it create a new Prisma-backed DB. Customization is
done by generating and editing Vue components (AFCL component library) rather than
forking templates, which keeps upgrades less painful than typical admin-template forks.

## Maturity
Emerging-to-mature. Created 2024-05-20 (2+ years old), 391 stars, 26 forks, 16 open
issues, active release cadence (v3.17.1 released 2026-08-31, pushed_at same day). Single
vendor (devforth) but consistent shipping history.

## License
MIT. No paid tier or cloud-subscription gate mentioned in the README ("always free and
open-source"). No commercial-use restrictions.

## Alternatives
React-admin, Retool (proprietary/hosted), Directus (headless-CMS-first), Payload CMS
(Next.js-coupled), AdminJS, Refine. AdminForth differs by shipping a CLI-first scaffolding
workflow directly against an existing schema plus a first-party plugin set (audit log,
2FA, AI writing) rather than requiring composition of separate packages.

## Risks / Limitations
- Single-vendor project (devforth); bus-factor risk if maintenance slows.
- "Agent-first" / "agentic admin panel" framing in the README is more about AI-assisted
  content generation inside the panel than an agent-tool interface (no MCP server found)
  — don't over-read the marketing language.
- 16 open issues against 391 stars suggests normal maintenance load, not a red flag on its
  own, but worth a skim before depending on it for anything customer-facing.

## Recommendation
**USE NOW** — mature enough, low adoption risk, and solves a concrete recurring problem
(internal admin panels) directly in the stack already used. Worth scaffolding against a
real project's schema as a first test.

## Change History
### 2026-09-03
Initial discovery and review. GitHub API confirmed MIT, 391 stars, created 2024-05-20,
pushed_at 2026-08-31, not archived, latest release v3.17.1. README verified via
raw.githubusercontent.com.
