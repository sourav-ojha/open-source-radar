# LibreDesk

## Summary
A self-hosted, single-binary omnichannel customer support desk (live chat widget + email, unified inbox, help center, SLA tracking, AI-assisted replies) positioned as a lightweight alternative to Intercom, Zendesk, and Chatwoot. Go backend + Vue frontend, AGPL-3.0, backed by Zerodha Tech (a large Indian fintech's engineering org, visible via a badge in the README) rather than an anonymous or single-person account.

## Why I Should Care
Sourav runs an MSSP security admin-portal SaaS and a PTaaS partnership — both plausibly need a customer/client support inbox at some point, and this replaces standing up Zendesk/Intercom (recurring per-seat SaaS cost) with a self-hosted Go binary that ships webhooks and an HTTP/JSON API for integration into an existing admin portal.

## Problems It Can Remove
- Recurring per-seat cost of Zendesk/Intercom/Freshdesk for a small support team.
- Building a bespoke ticketing/chat-widget system from scratch for a SaaS product's support needs.
- Fragmented support channels (email + live chat handled separately) — this unifies both into one inbox.

## Practical Uses
- Client support inbox for the MSSP admin-portal SaaS, handling both email and an embeddable live-chat widget.
- Internal IT-style help desk if a small team needs one, using the same instance.
- AI-assisted first-response drafting grounded in a self-hosted knowledge base, reducing manual reply time.

## Product Opportunities
The "single binary, embeds its own frontend" deployment model (see Architecture Notes) is directly reusable as a pattern for shipping any internal admin tool without a separate frontend deploy step. The AI-assistant-grounded-in-knowledge-base + human-handoff design is also a reusable pattern for a support feature inside his own products.

## Agent / Automation Opportunities
HTTP/JSON API and webhooks are documented for "custom integrations and workflows," making it feasible to wire ticket creation/status into an existing agent pipeline (e.g., auto-file a support ticket from a monitoring alert). No MCP server found in the repo as of this review — would need to be built if agent-tool-style access is wanted beyond plain REST calls.

## Integration
Single Go binary with an embedded frontend; one-click Railway deploy (provisions Postgres + Redis automatically) or Docker Compose for full self-hosting. SSO via Google, Microsoft, or any OIDC provider out of the box. **Integration effort: Low** for a hosted trial, **Medium** for a fully self-hosted production deployment (needs its own Postgres + Redis).

## Architecture Notes
Go backend with the Vue/TypeScript frontend built and embedded directly into the binary — no separate static-file server or frontend deploy step, a pattern worth borrowing for other internal tools. Role-based access control with per-action custom roles, and an automation-rules engine that runs on conversation events (tag/assign/route), similar in spirit to email-filter rule engines but applied to support conversations.

## Maturity
Established. Created 2024-05-12 (~28 months), 2,944 stars, 278 forks (healthy ~9.4% ratio), latest release v2.8.0 (2026-08-22), pushed_at 2026-09-19 (actively maintained). Backed by Zerodha Tech, which is a meaningfully stronger continuity signal than a typical solo or small-team OSS support-desk project.

## License
**AGPL-3.0 — flagged loudly.** Fine for internal/self-hosted use (running it to support your own product's customers), but if any modified version of LibreDesk itself were exposed as a network service to third parties, AGPL's source-disclosure obligation would apply. Not a concern for the "use it to run our own support desk" use case; would be a concern if repackaging or reselling it as a hosted product.

## Alternatives
Chatwoot — the more established, mainstream self-hosted Intercom-alternative (already well-known, not treated as a new discovery here). Zendesk, Intercom, Freshdesk — proprietary SaaS incumbents this replaces. LibreDesk's distinguishing pitch is the single-binary deployment model and 1-click Railway path, which is a meaningfully lower operational bar than Chatwoot's Rails + Sidekiq + Redis + Postgres stack.

## Risks / Limitations
- AGPL-3.0 licensing (see above) — evaluate before any redistribution or hosted-resale scenario.
- Full self-hosted production setup needs Postgres + Redis, not a truly single-dependency deploy despite the "single binary" framing.
- Only useful once there's an actual support workload to justify running and maintaining another service.

## Recommendation
PROTOTYPE — worth a Railway one-click trial against a real (even small) support inbox before committing to full self-hosting, given the AGPL terms and the operational cost of running Postgres + Redis for it.

## Change History
### 2026-09-20
Discovered and reviewed. GitHub API verified: AGPL-3.0, 2,944 stars, 278 forks, created 2024-05-12, latest release v2.8.0 (2026-08-22), archived: false. README confirms Zerodha Tech backing and single-binary/embedded-frontend architecture.
