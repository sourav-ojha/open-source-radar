# It's a Plan (itsaplan)

## Summary
It's a Plan is a self-hosted project-management and issue tracker (TypeScript, Bun,
Turbo monorepo, Postgres + MinIO via Docker Compose) positioned as an open alternative to
Linear/Jira/Trello/Plane. Its distinguishing feature: AI agents are modeled as first-class
teammates — they get a role, permissions, and an assignee slot on the same board as
humans — with the whole thing exposed over REST, webhooks, and MCP.

## Why I Should Care
Issue trackers with "AI features" usually mean a chatbot bolted onto the sidebar. This one
gives an agent the same board position as a human: an assignee slot, a role, and
permissions, with the underlying REST/webhook/MCP surface to actually let a coding agent
pick up and close tickets rather than just summarize them. It's also built on a stack
(TypeScript, Docker Compose, Postgres, MinIO/S3-compatible storage, OIDC SSO) that matches
the current toolchain directly, so evaluating it costs almost nothing beyond `docker
compose up`.

## Problems It Can Remove
Replaces a paid Linear/Jira seat for personal or small-team project tracking, and removes
the need to hand-wire "let my coding agent read/update tickets" glue — that's the MCP
server's job here.

## Practical Uses
- Self-host as the primary issue tracker for personal or client engineering work instead
  of paying for Linear.
- Give a coding agent (Claude Code, etc.) a real assignee slot on a board via MCP and
  measure whether "agent as teammate" is actually more useful than "agent as a tool the
  human invokes."
- Use the REST/webhook surface to sync issues into another system (e.g., a client status
  page) without needing Linear's paid API tiers.
- Study the OIDC-based SSO integration approach (works with Keycloak/Authentik/Entra/etc,
  credentials stored in DB not `.env`) for any product needing pluggable SSO.

## Product Opportunities
The "agent as a first-class board member" pattern (assignee slot + role + permissions)
is a reusable idea for any internal tool where agents and humans need to share a work
queue — not just project management.

## Agent / Automation Opportunities
REST API, webhooks, and an MCP server are all first-party and documented in the README as
core to the product's positioning, not an add-on. This is the main reason to evaluate it
over a mature incumbent.

## Integration
`git clone`, fill 8 required `.env` values (or use the Bun-based `bun run setup` generator),
`docker compose up -d` — starts Postgres, MinIO, api, worker, bot, and web from published
release images. **Integration effort: Low** — Docker-first, matches existing infra
(S3-compatible storage, Postgres), and OIDC SSO needs no `.env` restart to configure.

## Architecture Notes
TypeScript/Bun/Turborepo monorepo; auth via Better Auth; object storage via
S3-API-compatible MinIO (drop-in for real S3 in production); version pinning via a
`VERSION` env var so upgrades are explicit rather than "whatever's on `main`."

## Maturity
Early/emerging. Created 2026-07-14 (~2 months old at review), 494 stars, tagged release
v0.17.0 (2026-09-06), 10 contributors beyond the primary maintainer (209/~271 commits).
README explicitly states: "under active development... expect breaking changes before
the first stable release." Previously scanned in passing on 2026-08-31 without being
catalogued or rejected; the resurfacing with a real self-hosting doc, OIDC SSO, and a
tagged release justifies a full review now.

## License
AGPL-3.0-only. **Flagged per policy**: network-use copyleft applies to modified forks
offered as a service. No issue for straightforward self-hosted use.

## Alternatives
Linear (proprietary), Plane (AGPL-3.0, already known/mainstream), Taskosaur (BSL —
license-restricted despite "open source" framing, reviewed same run and passed over in
favor of this one), Huly. It's a Plan differs by treating agent participation as a board
citizenship model rather than a chat assistant, and by shipping on a stack that requires
no new infrastructure to self-host.

## Risks / Limitations
- Hit v1.0.0 only the day before this review — the new Teams/multi-tenancy architecture
  is unproven at scale despite the version bump.
- AGPL-3.0 (see License).
- Donation/star-solicitation language in the README ("star the repo," wallet addresses)
  is more prominent than typical for a project this size — not disqualifying, but a
  reason to watch release cadence rather than assume permanence.

## Recommendation
**PROTOTYPE** — stand it up via Docker Compose and test the agent-as-teammate workflow
against a real coding-agent task queue. The v1.0.0 release resolves the previous
pre-stable warning, but the new Teams architecture is one day old — worth a short
observation window before treating it as a same-day production swap for an existing
tracker.

## Change History
### 2026-09-13
Initial catalog entry. GitHub API confirmed AGPL-3.0, 494 stars, created 2026-07-14,
pushed 2026-09-11, not archived, latest release v0.17.0 (2026-09-06). Self-hosting guide
and package.json verified via raw.githubusercontent.com. Repo was scanned in an earlier
run's discovery pass (2026-08-31) without a formal catalog decision recorded; this is its
first full review.

### 2026-09-16
**Meaningful update.** v0.17.0 → v1.0.0 (released 2026-09-15). Major release: projects
now belong to a Team (roles, agents, skill library, integration credentials, MCP switch
all scoped per team instead of per project); initiatives gained file attachments and
Docs links; every MCP tool result now returns structured data plus HTTP status and a
domain error code; account security hardened (email verification holds at sign-up,
password reset ends all sessions, personal API keys expire, CSP/HSTS/X-Frame-Options
headers on every response); database backup runs automatically before migrations. This
resolves the "pre-stable, breaking changes expected" risk noted at first review — the
project has crossed into a genuine 1.0. Score raised 7.7 → 8.0 to reflect the maturity
jump; status held at PROTOTYPE pending a short observation window on the new
architecture.
