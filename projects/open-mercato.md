# Open Mercato

## Summary
Open-source TypeScript/Next.js "AI-Engineering Foundation Framework" for building
CRM/ERP/commerce backends: multi-tenant RBAC, modular domain entities with auto-discovery
and migrations, event subscribers/workflows, dynamic custom fields/forms, and ready-made
CRM/Sales/OMS modules that claim to leave a product "80% done." Also ships explicit
AI-coding-agent skills so agents know where generated code belongs in the module
structure.

## Why I Should Care
It targets the "shorten the path from idea to a shippable paid product" axis directly,
in a stack (TypeScript, Next.js) already in daily use, and it explicitly addresses a real
pain point of adopting coding agents on a team: agents generate code but don't decide
where it goes or how it stays consistent across many engineers.

## Problems It Can Remove
- Weeks of scaffolding for multi-tenant RBAC, per-module DB migrations, and dynamic
  custom-entity/form definitions on a new SaaS backend.
- Inconsistent output from AI coding agents on a shared codebase — the module
  conventions and specs are meant to give every agent session the same architectural
  answer.
- Building CRM/ERP/commerce domain logic (orders, tenancy, org hierarchies) from a blank
  Next.js + Prisma-style scaffold each time.

## Practical Uses
- Bootstrap a multi-tenant B2B ordering portal or CPQ flow from the ready-made commerce
  modules.
- Stand up a customer/partner self-service portal with configurable dynamic forms and
  org-scoped permissions.
- Expose a headless, well-typed API platform for a mobile app sharing one data model with
  a web app.
- Use as an internal reference for how to structure multi-tenant RBAC + dynamic entities
  in a TypeScript/Next.js backend, even without adopting the whole framework.

## Product Opportunities
Directly a "buy vs. build" leverage point: a multi-tenant, SaaS-ready CRM/ERP/commerce
backend that's mostly done is exactly the kind of foundation that turns an idea into a
shippable micro-SaaS faster.

## Agent / Automation Opportunities
Ships explicit "architecture-aware AI harness" skills for Cursor/Claude Code/Codex,
covering everything from adding a data table to implementing a full feature with tests —
a working example of designing a codebase specifically to be agent-friendly at scale.

## Integration
`npx @open-mercato/starter` for a guided one-command setup (handles Docker/infra,
`.env`, secrets, DB init). Manual path requires Node 24, Yarn 4, and Docker/Rancher
Desktop for Postgres/Redis/Meilisearch containers. High integration effort overall —
this is a framework to build *on*, not a library dropped into an existing app.

## Architecture Notes
Each feature lives in `src/modules/<module>` with auto-discovered frontend/backend
pages, APIs, CLI, i18n, and DB entities. MikroORM handles per-module entities/migrations
(no global schema); Awilix provides a per-request DI container that modules can override
via `di.ts`. Multi-tenancy is core: a `directory` module defines tenants/organizations,
and most entities carry `tenant_id` + `organization_id`.

## Maturity
Emerging. Created September 2025, MIT, real multi-contributor project (top contributor
3,367 commits, four others in the 240–488 range — not a single-author repo). 1,241 open
issues against 1,769 stars is an unusually high ratio, signaling active early-stage churn
rather than settled stability.

## License
MIT. No restrictions on commercial use, redistribution, or embedding; no per-seat
pricing per the README.

## Alternatives
- Hand-rolled Next.js + Prisma/Postgres scaffold (the status quo).
- **Medusa.js** — commerce-focused, less CRM/ERP breadth.
- **NocoBase** — already mainstream (rejected 2026-09-20), no-code rather than code-first.
- **Frappe/ERPNext** — Python stack, not TypeScript.

## Risks / Limitations
- High open-issue count relative to age/stars — worth checking issue triage speed before
  depending on it for anything time-sensitive.
- High integration effort/commitment: adopting means accepting its module, DI (Awilix),
  and ORM (MikroORM) conventions rather than incrementally adding a library.
- Young overall (about one year old) relative to the architectural commitment it asks for.

## Recommendation
PROTOTYPE — worth a spike project (e.g. a small internal admin tool or a new client
micro-SaaS) to evaluate the real developer experience of the module system and AI-harness
skills before committing a production product to it.

## Change History
### 2026-09-24
Discovered and catalogued. Real multi-contributor TypeScript framework aimed squarely at
the micro-SaaS/product-building relevance axis; flagged the high open-issue count as a
maturity signal worth monitoring rather than a disqualifier.
