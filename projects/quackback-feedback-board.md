# Quackback

## Summary
Open-source support-and-feedback suite: feedback boards with voting, a shared inbox, help
center, changelog/roadmap pages, and an embeddable widget — positioned as the open-source
alternative to Canny, UserVoice, and Productboard. Self-hostable via Docker or usable as a
hosted cloud product from the same codebase.

## Why I Should Care
Feedback/roadmap/changelog infrastructure is exactly the kind of undifferentiated
product-building block that shortens the path from idea to shippable paid product — one of
this profile's two primary relevance axes. It also ships an MCP server, so an agent can be
wired directly into feedback triage.

## Problems It Can Remove
Removes the need to build or pay for a feedback-board/roadmap/changelog stack for a new
micro-SaaS product, including the embeddable widget and third-party integrations that
usually take months to accumulate.

## Practical Uses
- Run a self-hosted feedback board, roadmap, and changelog for a micro-SaaS product
  instead of paying per-seat for Canny.
- Let an internal agent search, triage, and act on incoming feedback via the built-in MCP
  server.
- Embed the feedback widget directly inside a product's own UI instead of linking out to a
  separate portal.
- Route feedback into existing Slack/Linear/Jira/GitHub workflows via the 25 built-in
  integrations instead of building webhook glue by hand.

## Product Opportunities
Bundle as the feedback/roadmap/changelog layer for a new micro-SaaS rather than building
or buying one separately; use the MCP server as the feedback-triage arm of a broader
internal agent workflow.

## Agent / Automation Opportunities
Ships an MCP server so AI agents can search, triage, and act on feedback directly, plus an
in-product AI copilot (duplicate detection, thematic summaries) with per-tool write
permissions (always allow / ask / deny) — a reasonable pattern for gating what an agent is
allowed to do inside a feedback pipeline.

## Integration
Medium effort. Requires PostgreSQL and its own environment configuration; deployable via
Docker or a one-click Railway template. Not a drop-in library — it's a full application to
operate.

## Architecture Notes
The self-hosted build and the hosted cloud product share one codebase, gated by a
default-off `cloud` settings block plus plan/entitlement checks and a control-plane
client. On a self-hosted install none of that activates, no upgrade prompts render, and no
outbound calls are made — worth knowing before assuming a self-hosted install phones home
in any way.

## Maturity
Emerging. Created 2025-12-02, 263 stars, 83 forks, 39 open issues (active), v0.13.2
tagged 2026-08-01 with more recent untagged activity (pushed 2026-09-07).

## License
AGPL-3.0 — network copyleft, flagged loudly per this radar's licensing policy given the
intent to embed things in commercial products. Self-hosting as-is for internal/own use is
fine. Modifying it and offering the modified version as a hosted service to others (or
bundling it inside a redistributed commercial product) would trigger the obligation to
release the modified source. The repo's own commercial "cloud" hooks are the vendor's own
dual-licensing path for their hosted offering — not a template for building a competing
hosted fork on top of the AGPL code.

## Alternatives
Canny, UserVoice, Productboard (all closed-source, per-seat pricing); Fider (older
self-hosted OSS alternative with a narrower feature set).

## Risks / Limitations
AGPL-3.0 licensing risk as described above. Single dominant author (3,397 of the visible
commits) plus AI-assisted commits — real bus-factor risk despite the visible polish and
feature breadth. Requires its own Postgres-backed operations, not a lightweight embed.

## Recommendation
PROTOTYPE — suitable for self-hosting as an internal or product-facing feedback tool as-is;
review the AGPL implications carefully before any plan to modify and resell it as a hosted
service.

## Change History
### 2026-09-08
Initial discovery and catalog entry.
