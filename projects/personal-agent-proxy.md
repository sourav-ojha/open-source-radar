# Personal Agent Proxy

## Summary
Self-hostable access-control and audit layer for personal AI agents (Claude, ChatGPT, Grok, and other agent products). Connect personal services once, grant each agent granular, revocable permissions per connection, and get an audit log of what each agent actually did — without handing a copy of the underlying service's data into the agent provider's own database. TypeScript/Bun monorepo (web dashboard, docs site, marketing site, Postgres-backed db package), MIT-licensed, Docker and Render.com deployable.

## Why I Should Care
As personal AI agents from multiple vendors each want access to email/calendar/etc., a single self-hosted permission and audit broker in front of all of them is a cleaner security model than granting every vendor's agent its own direct OAuth access.

## Problems It Can Remove
Per-agent, per-connection permission scoping plus an audit trail that does not require copying the underlying service's data into the agent vendor's infrastructure is a non-trivial access-control design; this project has already built the dashboard and connection-management UI around it.

## Practical Uses
- Give a personal assistant agent (e.g. a Claude- or ChatGPT-based assistant) scoped access to one connected service (email, calendar) without exposing the rest of the account
- Audit trail of exactly what an agent read or acted on across connected personal services
- Run multiple personal agents against the same connected accounts with independently revocable permissions per agent

## Product Opportunities
- The access-control + audit pattern here is directly reusable in a product that needs to let end users grant third-party AI agents scoped access to their account data — relevant to the admin-portal SaaS's own permissioning model

## Agent / Automation Opportunities
- Positions itself explicitly as agent-agnostic middleware between a user's personal services and whatever agent product they use

## Integration
Deployment: Docker / docker-compose, or Render.com (render.yaml included); a hosted instance (app.personalagentproxy.com) exists as a non-self-hosted alternative.
Interfaces: web dashboard; a connection/permission API is implied but not documented in the README.
Integration effort: **Medium** — self-hosting requires standing up the Bun monorepo's services plus Postgres; permission/connection setup per service is not yet documented outside the hosted docs site.

## Architecture Notes
Bun workspace monorepo with separate packages for `db` (Postgres, via a generate/seed scripts pipeline), `web` (the dashboard), `docs`, and `website`. No tagged releases yet — runs off `main`.

## Maturity
Experimental — 7 days old at time of review (created 2026-10-06). GitHub API verified: MIT, 95 stars, 13 forks, pushed_at 2026-10-13. Two named contributors (arnestrickmann, hkonsti). 95 stars in a week is fast growth for a brand-new repo, but there are no tagged releases and the README is almost entirely marketing imagery rather than technical documentation.

## License
MIT, confirmed via GitHub API license field. No restrictions on commercial or internal use.

## Alternatives
- Stashbase CLI (catalogued, PROTOTYPE, 7.5) — narrower scope: a request-level secrets/egress proxy specifically for coding agents, not a general personal-service permission broker

## Risks / Limitations
- 7 days old at time of review, 2 contributors, no tagged releases yet — pre-alpha
- README is almost entirely marketing imagery; the actual permission/audit mechanics are not documented outside the hosted docs site, which was not independently verified this run
- A product whose entire value proposition is brokering access to personal accounts is exactly the kind of thing that should not be trusted with real credentials before its security model has had independent scrutiny

## Recommendation
**PROTOTYPE** (score 6.9/10) — worth a small experiment with a throwaway connection, not yet worth trusting with real account access given its age and undocumented internals.

## Change History
### 2026-10-13
First discovered and reviewed via GitHub search. Verified via GitHub API: MIT, 95 stars, created 2026-10-06. Repo contents confirm a real Bun/TypeScript monorepo rather than a landing-page-only repo.
