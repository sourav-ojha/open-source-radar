# openheard

## Summary
openheard is an open-source feedback board, public roadmap and changelog — a Canny alternative built to run entirely on Cloudflare's free tier: one Worker, one D1 database, KV, and R2 for images. It deploys in two commands with no VPS, no Postgres, and no Docker host to patch. It ships an HTTP API, an MCP server, an embeddable widget, a help center, and email notifications for voters and changelog subscribers.

## Why I Should Care
A feedback board + roadmap + changelog is a multi-week build (voting, status timelines, a public roadmap, a searchable help center, email notifications, an embeddable widget) that most teams buy rather than build. Quackback (already catalogued) solves this but as a heavier all-in-one support suite that expects Postgres and Docker ops. openheard deliberately does less and runs on nothing — relevant for a solo/two-person micro-SaaS where even small ongoing infra ops is a real cost.

## Problems It Can Remove
- Building or paying for Canny/Featurebase/UserVoice
- Operating a Postgres+Docker service just to host a feedback board
- Building an embeddable in-product feedback widget from scratch

## Practical Uses
- Self-hosted feedback board + roadmap + changelog for a micro-SaaS product
- Embed the feedback widget directly inside a product's own UI with one script tag
- Let Claude Code or another coding agent triage feedback via the MCP server or bundled agent skill
- Import an existing Canny CSV export to migrate off a paid tool

## Product Opportunities
Zero-infrastructure feedback/roadmap/changelog layer for a new micro-SaaS where standing up Postgres + Docker just for feedback isn't worth the ops overhead.

## Agent / Automation Opportunities
Ships both an MCP server (`/api/mcp`) and a dedicated agent skill (`skills/openheard`) that teaches Claude Code and other coding agents the product's own feedback-triage workflows directly — not just a generic API wrapper.

## Integration
Self-host on Cloudflare: `bun install`, configure `BETTER_AUTH_SECRET`, `bunx alchemy login`, `bun run deploy`. Provisions the Worker, D1 database, KV and an R2 bucket, applies migrations, and prints the URL — R2 needs a one-time dashboard toggle first. Everything fits Cloudflare's free tier. Integration effort: **Low**, but specifically tied to the Cloudflare ecosystem rather than a portable Docker image — a real consideration for an AWS-centric deployment.

## Architecture Notes
One Cloudflare Worker, one D1 database — the entire production architecture. No separate server process, queue, or database to operate. The embeddable widget, help center, and changelog RSS feed are all served from the same Worker.

## Maturity
Experimental. Created 2026-09-04, one release so far (v0.1.0, tagged 2026-09-25). Effectively single-maintainer: 259 of 268 commits from one author, with two minor contributors. No CLI or docs site yet — both are listed as "coming next" in the README.

## License
AGPL-3.0 — network copyleft, flagged per the operating spec's licensing section. Fine to self-host as-is for an internal or own-product feedback board. Modifying it and re-offering the modified version as a hosted service to third parties would trigger source-release obligations. The project's own cloud offering (openheard.com) is the vendor's dual-licensing path, not a template for building a competing hosted fork.

## Alternatives
Quackback (already catalogued — broader all-in-one support suite: live chat, ticketing, status page, mobile SDKs, AI copilot, AGPL-3.0), Fider (older, narrower self-hosted OSS alternative), Canny, Featurebase, UserVoice (all proprietary).

## Risks / Limitations
- AGPL-3.0, see commercial-use notes
- Single-maintainer, one month old — real bus-factor risk
- Only one release, no CLI/docs site yet
- Cloudflare-specific; doesn't drop into an AWS-centric deployment without rearchitecting

## Recommendation
PROTOTYPE — genuinely differentiated from the already-catalogued Quackback by deployment footprint, not just branding. Worth trying for a feedback board on a product where zero server ops is the priority; not yet mature enough to depend on without a fallback plan given the one-person maintenance.

## Change History
### 2026-10-11
First reviewed and catalogued. Found via GitHub search ("open source canny featurebase alternative"); compared directly against the already-catalogued Quackback to confirm real architectural differentiation rather than a near-duplicate.
