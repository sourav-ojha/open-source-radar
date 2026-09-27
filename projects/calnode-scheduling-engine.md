# Calnode

## Summary
A Calendly/Cal.com-style scheduling engine shipped as a single Go binary with an embedded pure-Go SQLite database (Litestream for backups) — no Redis, no Postgres, no separate API server. API-first and webhook-native, with a native MCP server (stdio + Streamable HTTP) compiled into the binary so AI agents can find slots and book meetings using the same code path as the REST API and web UI. Apache-2.0.

## Why I Should Care
It solves the same problem as self-hosting cal.com without the operational weight — cal.com is 500k+ LOC across ~100 packages needing Postgres, Redis, and a separate API server; Calnode is one static binary and a SQLite file, deployable on a $5 VPS. It is also the most agent-native scheduling tool found so far: booking, rescheduling, and slot-lookup are first-class MCP tools, not an API wrapped after the fact.

## Problems It Can Remove
- A recurring Calendly seat cost for client-facing or internal scheduling.
- The multi-service operational burden of self-hosting cal.com when only a booking page + API is actually needed.
- Hand-rolling DST-safe availability logic, double-booking guards, and calendar free/busy sync for a custom booking feature.

## Practical Uses
- Self-hosted client-booking page for consulting, PTaaS scoping calls, or MSSP sales calls.
- An AI sales/support agent that books meetings autonomously via the built-in MCP server.
- Webhook-driven automation into n8n/Make when a booking is created, rescheduled, or cancelled.
- White-label booking-as-a-feature embedded per-tenant inside an existing product (the single-binary, instance-per-tenant design makes this cheap to run many times over).

## Product Opportunities
Because it is a single static binary with no external service dependencies beyond an optional LiveKit endpoint for video, it is cheap to spin up one instance per customer — a plausible "booking-as-a-feature" add-on for a SaaS product rather than integrating a third-party scheduling API.

## Agent / Automation Opportunities
A Model Context Protocol server is compiled directly into the binary (official Go MCP SDK) exposing `list_event_types`, `get_available_slots`, `create_booking`, `reschedule_booking`, `cancel_booking`, and more, over both stdio (local agents) and Streamable HTTP (remote agents, with Calnode acting as its own OAuth 2.1 authorization server for a no-pre-shared-key Connect flow). MCP tool calls hit the same internal services as the REST API, so side effects (calendar sync, emails, webhooks) fire identically regardless of entry point.

## Integration
`docker run` a single container with a mounted data volume, or drop the static binary on a VPS directly. HTTPS, a persistent encryption key, and backups need `DEPLOY.md`-documented setup for production use. **Integration effort: Low.**

## Architecture Notes
Go 1.26 backend with `sqlc`-generated queries against pure-Go SQLite (no CGO, so it compiles to a fully static binary); public booking pages are server-rendered Go templates for fast first paint, while the admin app is SvelteKit 5. Instance-per-tenant by design (one deployment = one isolated workspace) rather than a shared multi-tenant database with `org_id` scoping everywhere, which is cal.com's model. An `AUDIT.md` and an OpenSSF Scorecard badge in the repo suggest above-average security diligence for a project this young.

## Maturity
Emerging. Created 2026-06-23 (~3 months old), 97 stars, 16 forks, latest release v0.9.0 (2026-09-10) — pre-1.0, so breaking changes are still plausible. One dominant contributor (`shockalotti`, 525 commits) with a handful of smaller contributors including Dependabot — a small, active, single-developer-led project rather than a team effort yet.

## License
Apache-2.0. No restrictions on commercial use, self-hosting, redistribution, or embedding — notably more permissive than cal.com's AGPL-3.0.

## Alternatives
cal.com (AGPL-3.0, the mainstream self-hosted option, far heavier to operate), Calendly (closed SaaS), asm0dey/calit (same "self-hosted Calendly alternative" pitch, but Quarkus + Postgres — a heavier JVM stack, 6 stars, earlier-stage). Calnode's distinguishing bet is minimalism plus agent-native design over cal.com's feature-maximal approach.

## Risks / Limitations
- Pre-1.0 (v0.9.0) with one dominant contributor — API or schema changes before 1.0 are plausible.
- Instance-per-tenant architecture means no built-in multi-tenant isolation if one deployment needs to serve many separate customers.
- Optional video/recording/AI-notetaking features require a self-hosted or cloud LiveKit endpoint — an extra moving part beyond the core single-binary pitch.
- No independent security audit beyond the project's own `AUDIT.md` and OpenSSF Scorecard.

## Recommendation
PROTOTYPE — worth standing up for a real client-booking use case (e.g. PTaaS scoping calls) before depending on it for anything business-critical, given the pre-1.0 version and single-maintainer profile.

## Change History
### 2026-09-27
Discovered and reviewed. GitHub API verified: Apache-2.0, 97 stars, 16 forks, created 2026-06-23, latest release v0.9.0 (2026-09-10), archived: false. README confirms single-binary deployment, native MCP server, and a cal.com feature/architecture comparison table; `AUDIT.md` and `DEPLOY.md` confirmed present in the repo via raw.githubusercontent.com.
