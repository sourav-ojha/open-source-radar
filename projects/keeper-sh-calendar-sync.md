# Keeper.sh

## Summary
A self-hosted calendar-syncing engine: pulls events from Google Calendar, Outlook, iCloud, Fastmail, CalDAV, or read-only iCal/ICS feeds, and pushes aggregated events to one or more destination calendars so time slots stay aligned across all of them without double-booking. Ships a REST API and a global MCP server so an AI agent can read and manage calendar state across every connected provider from one interface.

## Why I Should Care
Two distinct reasons: first, it's directly usable as-is to solve the real cross-provider double-booking problem anyone juggling a work calendar, a business calendar, and a personal calendar runs into. Second, it's a clean, studyable example of a solo-founder micro-SaaS: AGPL-licensed so the sync-correctness claims are checkable, self-hosting gets every "Pro" feature for free with no artificial gating, and the paid hosted tier is priced purely on convenience (faster refresh, no infra upkeep) rather than withheld features — a monetization model worth copying rather than the more common open-core feature-gating pattern.

## Problems It Can Remove
Removes the need to hand-roll Google/Outlook OAuth plumbing, provider-specific incremental-sync (sync tokens/delta links), and the destination-tracking logic needed to avoid duplicate or orphaned events when building any calendar-aware tool or agent. Removes manual cross-calendar copy-pasting of events to avoid double-booking.

## Practical Uses
- Keep a business calendar, a work calendar, and a personal calendar free of scheduling conflicts without manual copying.
- Give a coding agent or personal-assistant agent calendar read/write authority through one MCP surface instead of writing per-provider OAuth and polling logic from scratch.
- Privacy-filtered calendar publishing: strip event titles/descriptions and replace with a template (`{{calendar_name}}`) before sharing availability externally.
- Subscribe to a combined iCal feed from any calendar app as a single merged view.

## Product Opportunities
Directly studyable as a solo-founder micro-SaaS case: the AGPL self-hosted-is-the-real-product model with a hosted convenience tier is a pattern Sourav could apply to his own micro-SaaS ideas — monetizing operational convenience rather than gating features behind a paywall builds more trust and avoids the usual open-core resentment.

## Agent / Automation Opportunities
The MCP server is a ready-made "give an agent calendar authority" building block (`trigger_sync`, `pause_sync` over MCP) — usable directly rather than writing custom multi-provider OAuth and polling logic for an agent-scheduling project.

## Integration
Docker Compose: a convenience `keeper-standalone` image bundling everything behind a reverse proxy, or five granular service images (API, web, MCP, cron, worker) behind Postgres, Redis, and Caddy for more control. Self-hosting unlocks every Pro feature (1-minute refresh, unlimited linked accounts/mappings) with no trial limitation. A hosted tier at keeper.sh exists as the lower-effort alternative (ingestion every minute on every plan; push refresh is 30 minutes on the free hosted tier, 1 minute on Pro).

## Architecture Notes
Two independently scheduled halves: ingestion pulls from sources into Keeper.sh's own database once a minute regardless of plan (Google/Outlook use the provider's own sync-token/delta-link APIs for incremental fetches; CalDAV/iCloud/Fastmail/iCal are refetched and diffed against stored state). Pushing to destinations runs on the plan's refresh interval via a Redis-backed per-destination job queue. Deletion correctness: a deletion is tracked by a mapping row that outlives the event it pointed at, so the remote copy still gets removed on a later pass even after the source event is gone; the cleanup sweep only ever touches events Keeper.sh itself created (tracked via a `@keeper.sh` UID suffix or an Outlook category), never a user's own event — the two failure modes (events that never get cleaned up, and events wrongly deleted) that are easiest to get subtly and destructively wrong in any calendar-sync tool.

## Maturity
Emerging. 1,465 stars, 78 forks, 2 subscribers, 168 open issues, latest tagged release v2.31.0 (2026-11-11, actively released). Commit history shows a dominant solo maintainer (1,098 of the repo's commits) plus several real named human contributors and the usual 2026-era bot contributors (dependency/agent bots) — a normal pattern for an indie SaaS, not a red flag on its own.

## License
AGPL-3.0. Self-hosting for personal or internal use carries no obligation; redistributing a modified version as a hosted service to third parties would trigger AGPL's network-use clause. The author states explicitly that the engine is AGPL specifically so the deletion-safety claims above are independently checkable rather than taken on faith.

## Alternatives
Native provider sync (doesn't solve cross-provider double-booking at all); Zapier/Make calendar automations (no self-host path, no MCP server); hand-rolled sync-token polling scripts (the exact thing this replaces).

## Risks / Limitations
Subscriber count (2) is unusually low relative to its star/fork/open-issue volume by the engagement-ratio heuristics used elsewhere in this catalog to flag suspicious growth — but the commit history (real named contributors, a plausible solo-founder-plus-bots pattern, a coherent dogfooding origin story) doesn't otherwise show the inflated-engagement signature flagged on other repos, so this is noted rather than treated as disqualifying. Single primary maintainer running both the open-source project and its monetized hosted tier is a dependency-risk factor for anything beyond personal use.

## Recommendation
PROTOTYPE. Worth self-hosting to solve a real multi-calendar conflict, and worth studying as a monetization-model reference regardless of whether the tool itself gets adopted.

## Change History
### 2026-11-12
Initial discovery and review. Slot 2 run (product infrastructure, APIs, backend components). Found via GitHub Search API `topic:webauthn OR topic:passkeys self-hosted stars:>50` sweep, which surfaced it as a tangential but more interesting result.
