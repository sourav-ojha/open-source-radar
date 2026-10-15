# Hexclave

## Summary
A monorepo "user infrastructure platform" bundling the pieces every SaaS rebuilds before it can ship: authentication (passkeys, OAuth, CLI auth), teams/workspaces, RBAC, API keys, payments (subscriptions, one-time, usage-metered credits), transactional/marketing email, built-in analytics (active users, session replay, NL-to-SQL dashboards), signed webhooks, and an encrypted "data vault" for user-supplied secrets. Next.js/React/TanStack-Start/JS SDKs plus a REST API and CLI; Postgres-backed, Docker-based self-host path alongside a hosted cloud option. Client/SDK packages are MIT, server packages AGPLv3 (enterprise license available).

## Why I Should Care
This is exactly the "idea to shippable paid product" gap axis 2 of the operator profile targets: instead of wiring Auth0/Clerk + Stripe + Resend + PostHog + a webhooks table by hand for every new micro-SaaS idea, one platform covers auth, billing, teams, and analytics behind one user model. Explicitly designed for agent-assisted setup — the README's install step is a prompt ("read skill.hexclave.com and help me setup hexclave in this project") that a coding agent executes directly, and the repo ships an MCP app.

## Problems It Can Remove
- Auth boilerplate (passkeys/OAuth/sessions) built from scratch per project
- Hand-rolled team/workspace + role-gating schema and middleware
- Stripe subscription + usage-metering + credits plumbing
- A separate transactional email provider integration and template system
- A bolted-on analytics/session-replay stack for a new product's first release
- Signed/retried webhook delivery infrastructure

## Practical Uses
- Bootstrap a new micro-SaaS idea (billing + teams + auth) in hours instead of days
- Centralize auth/billing for several small side projects behind one account system
- Use the API-keys app (auto-revoke on leak, show-once secrets) as a drop-in for any product that needs to issue customer API keys
- Use the data vault to store user-supplied third-party tokens (e.g. for an MSSP client-portal integration) without building encryption-at-rest handling
- Use the launch checklist app as a pre-production gate for client-facing portal rollouts

## Product Opportunities
- Foundation for spinning up new paid micro-SaaS products fast, since most of the "boring 20%" (auth/billing/teams) is pre-built
- Could underpin a multi-tenant version of the MSSP admin-portal SaaS without re-deriving RBAC/billing from scratch

## Agent / Automation Opportunities
- Ships its own MCP app (`apps/mcp`) and an agent-oriented setup skill served from `skill.hexclave.com`, designed to be installed and configured by a coding agent rather than a human reading docs
- CLI (`@hexclave/cli`) for local dev/scripting

## Integration
Self-hosted via Docker (repo includes a `docker/` directory with `backend`, `server`, `dev-postgres-replica`/`dev-postgres-with-extensions`, and a `mock-oauth-server` for local dev) or hosted cloud. Postgres is the backing store. SDKs: `@hexclave/js`, `@hexclave/next`, `@hexclave/react`, `@hexclave/tanstack-start`, plus a REST API for other languages and Python JWT verification. Integration effort: **Medium** — not a single drop-in library; adopting it means standardizing a project's user/team/billing model on Hexclave's schema, which is a real architectural commitment, not a quick add.

## Architecture Notes
Turborepo/pnpm-workspace monorepo with `apps/` (backend, dashboard, deployment-gateway, hosted-components, internal-tool, mcp, skills) and `sdks/`; ships "apps you switch on" as feature flags rather than one monolithic install. Changesets-driven release process publishing to npm multiple times a day (`@hexclave/shared` was at 1.0.130 as of 2026-10-14, confirmed via npm registry).

## Maturity
Emerging but unusually active for its age: created 2026-02-28, still shipping multiple releases per day (dashboard-v1.0.130 published 2026-10-14) with commits from 10+ distinct human contributors plus bot-driven CI, not a single-author or dormant project. One caution: 6,867 GitHub stars against only 15 watchers is a wide ratio — the same shape flagged as a possible inflated-engagement signal in recent runs — but unlike those cases, commit history here shows genuine multi-contributor, daily development, so this looks like organic traction with an unusually low watch-button click-through rather than a fabricated-star repo. Worth re-checking in a few months regardless.

## License
Split license: client/SDK code is MIT; server-side packages are AGPLv3. **AGPLv3 on the server components must be flagged loudly** per this radar's commercial-embedding intent — self-hosting and modifying the server under AGPL requires offering source to users you serve it to over a network, unless an enterprise license is purchased from Hexclave for commercial use outside AGPL's copyleft terms. The GitHub API reports the repo's overall license as NOASSERTION because of this per-package split; read each package's own `LICENSE` file before depending on it commercially.

## Alternatives
Clerk/Auth0 (closed-source, cloud-only, narrower scope — auth only), Supabase Auth (narrower, no billing/analytics bundling), WorkOS (closed-source), stitching together Lago/OpenMeter (billing) + Logto/Casbin (RBAC) + PostHog (analytics) + Resend (email) individually.

## Risks / Limitations
- AGPLv3 on the server is a real constraint for any hosted commercial product built on the self-hosted path without an enterprise license
- Adopting it is an architectural commitment (your user/team model becomes Hexclave's), not a low-risk drop-in
- Star:watcher ratio (458:1) is unusual and worth periodic re-verification even though commit history looks genuine
- Young (8 months old) — no long-term track record of API stability or self-host upgrade paths yet

## Recommendation
PROTOTYPE — strong fit for axis 2 (shortening idea-to-shippable-product time), but the AGPL server license and the architectural lock-in mean it should be tried on a new, low-stakes side project before being trusted under a client-facing or commercial deployment.

## Change History
### 2026-10-15
Discovered and catalogued. Found via GitHub topic search (billing/rbac) during slot 2 (product infrastructure) discovery.
