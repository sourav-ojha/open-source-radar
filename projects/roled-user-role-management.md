# Roled

## Summary
A small, self-hosted or cloud "centralized user and role management platform" explicitly scoped to teams running multiple projects with isolated user bases — not a full IdP/SSO suite. Go/Fiber backend on MariaDB + Redis, React/Vite/Tailwind admin console, OAuth2 login flows, Docker Compose quick start that seeds admin/standard demo credentials on first boot.

## Why I Should Care
Narrowly solves the specific repeated chore of re-building user schemas, password-reset flows, and an admin UI for every new internal tool or client project — distinct from SSO products (Keycloak, Authentik) that assume one shared identity across apps. Fits a shop running several small internal tools or client portals (e.g. MSSP admin-portal-adjacent work) where each project needs its own isolated user base but shouldn't need its own auth stack.

## Problems It Can Remove
- Rebuilding login/password-reset/role-admin UI for every new internal tool or small client project
- Ad hoc per-project RBAC tables and admin screens

## Practical Uses
- Backing user/role store for several small internal admin tools that should not share a single global identity
- Quick auth+roles layer for a new client-facing micro-tool without standing up a full IdP
- Reference implementation for a lightweight OAuth2 provider in Go/Fiber if building something similar in-house

## Product Opportunities
- Could be white-labeled as the user-management layer inside a multi-client admin-portal product where each client project needs isolated users

## Agent / Automation Opportunities
None documented — standard REST/OAuth2 API, no MCP server or agent-specific tooling yet.

## Integration
Docker Compose brings up the auth service, admin console, MariaDB, and Redis in one command; integration is via OAuth2 login flows and REST APIs from any app. Integration effort: **Low** for a new project, **Medium** if retrofitting into an app with an existing user table, since logins would need to move to Roled's OAuth2 flow.

## Architecture Notes
Clean separation of `auth` (Go/Fiber/MariaDB/Redis) and `console` (React/Vite/Tailwind) in the same repo, with SonarCloud coverage/quality-gate badges and CI wired up — unusually disciplined tooling for a 6-week-old, single-digit-star project. Explicit non-goal statement (not an SSO/IdP) is a good sign of scope discipline rather than feature creep.

## Maturity
**Experimental.** Created 2026-08-27, 6 stars, latest tagged release `auth/v0.27.1` (2026-10-10) — active weekly releases but no independent adoption signal yet beyond the maintainer's own cadence.

## License
Apache-2.0 — permissive, no commercial-use restrictions.

## Alternatives
Keycloak, Authentik, Zitadel (all full SSO/IdP, heavier to run for this narrower "isolated per-project users" use case), or hand-rolled per-project auth (the status quo this removes).

## Risks / Limitations
- Very early: single-digit stars, no independent production use reported yet
- Small team/possibly solo maintainer — bus-factor risk
- OAuth2-only login flow means existing apps need a migration, not a drop-in

## Recommendation
PROTOTYPE — worth a real trial on one small internal tool to see if the "isolated-per-project user management" framing holds up in practice; too early to depend on for anything client-facing yet.

## Change History
### 2026-10-15
Discovered and catalogued. Found via GitHub audit-log/rbac topic search during slot 2 discovery.
