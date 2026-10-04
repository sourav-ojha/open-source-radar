# BrightBean Studio

## Summary
Open-source, self-hostable social media management platform (AGPL-3.0, Django + HTMX + Tailwind) for creators, agencies, and SMBs — schedule, publish, approve, and monitor content across Facebook, Instagram, LinkedIn, TikTok, YouTube, Pinterest, Threads, Bluesky, Google Business Profile, Mastodon, and DEV.to from one multi-workspace dashboard, with every feature available to every user (no paid tier or feature gate).

## Why I Should Care
It replaces a $100–300/month Buffer/Sendible/SocialPilot/ContentStudio subscription entirely, with no per-seat or per-channel limits, and talks directly to each platform's first-party API using your own developer credentials rather than going through an aggregator — no shared rate limits, no third party sitting between the data and the client.

## Problems It Can Remove
Removes the recurring SaaS cost and per-seat pricing of commercial social-media-management tools, and the integration effort of building first-party API connections to 10+ platforms (each with different auth flows, rate limits, and webhook behavior) from scratch.

## Practical Uses
- Run social scheduling and publishing for a side project or small client roster without paying per-seat SaaS fees.
- Use the client portal (passwordless magic-link access) to let a client approve or reject posts without an account.
- Run the unified social inbox to triage comments, mentions, and DMs across platforms from one place.
- Use the approval-workflow stages (none/optional/internal/internal+client) to run an actual agency review process.
- White-label branding per workspace if reselling social-media management as a service.

## Product Opportunities
This is a direct "could become a product" candidate: zero feature-gating plus white-label branding plus a client-approval portal means it is close to resale-ready as a managed social-media service for existing MSSP/PTaaS clients who also need marketing ops — though see License below before offering it as a hosted service to third parties.

## Agent / Automation Opportunities
No MCP server or agent-facing API is documented. The Django REST-style views and webhook delivery system are a plausible base for a future automation layer (e.g. an agent drafting posts into the idea Kanban board), but that would be custom work on top of what ships today.

## Integration
One-click deploy buttons for Heroku, Render, and Railway; otherwise `docker compose up -d --build` with a documented `.env` (PostgreSQL, optional S3-compatible storage for uploads, SMTP for invites). Integration effort: **Low** for a standalone deployment — one-click hosted options exist, though per-platform API credentials (one developer app per social network) still need to be set up individually.

## Architecture Notes
Django 5.x backend with a worker process (background job queue implied by `docker compose` service list) and a Caddy-served media layer; Tailwind compiles via its own Compose service. Each platform integration calls the official first-party API directly (no aggregator middleman), which is the main differentiator from most "social media API as a service" wrappers.

## Maturity
Emerging but active: created 2026-03-25 (~6 months old), 2,406 stars, 524 forks — a notably high fork-to-star ratio for its age, consistent with genuine developer adoption rather than inflated stars. CI pipeline present and passing. No tagged GitHub releases yet (track via commit history/CHANGELOG rather than semver).

## License
AGPL-3.0 (verified via repo badge/LICENSE). **Flag loudly**: AGPL's network-use clause means that if a modified version of this software is run as a service reachable by users other than the deploying organization itself (e.g. hosting it for external clients), the modified source must be made available to those users. Self-hosting unmodified or for internal use only does not trigger this; reselling a modified instance as a managed service to clients would.

## Alternatives
- Buffer / Sendible / SocialPilot / ContentStudio (mainstream SaaS) — the paid tools this directly targets; no self-hosting, per-seat/per-channel pricing.
- Postiz and other same-week "Buffer alternative" entrants (surfaced in the same search, mostly 0–500 stars) — far less mature, no first-party-API-only architecture documented.
- A custom n8n/Zapier-style pipeline against each platform's API — more flexible but requires building the approval workflow, client portal, and inbox from scratch.

## Risks / Limitations
- AGPL-3.0 (see License) — the main adoption risk if the plan is to resell a modified hosted instance rather than self-host for internal/owned-client use.
- No tagged releases — pin to a commit SHA rather than assuming semver stability for production use.
- Django/Python stack outside the core MERN toolchain, acceptable since it is deployed as a complete app rather than imported as a library.
- 2FA is listed as "on the roadmap," not yet shipped — relevant if handling client social-account credentials.

## Recommendation
**PROTOTYPE** (score 7.1/10) — worth standing up against one or two real social accounts to validate the first-party integrations before committing; the AGPL hosting implication needs a clear answer before any resale plan.

## Change History
### 2026-10-04
First discovered and reviewed (slot 5 — self-hosted SaaS alternatives, productivity). Verified via GitHub API: AGPL-3.0, 2,406 stars, 524 forks, created 2026-03-25. Surfaced via a `self-hosted social media scheduler alternative` search; several same-week lower-star entrants in the same niche were not catalogued (see daily digest rejections).
