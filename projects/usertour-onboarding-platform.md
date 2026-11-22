# Usertour

## Summary
Usertour is a self-hosted, open-source user onboarding platform: in-app product tours,
checklists, and surveys, with a built-in MCP server so an AI coding agent can author a
flow directly from a prompt instead of a human building it in a visual editor. Core code
is MIT-licensed; directories tagged "enterprise"/"ee" (feature/permission/role/plan
gating) sit under a separate commercial license.

## Why I Should Care
Onboarding/product-tour tooling (Appcues, Userflow, Userpilot, Chameleon) is a recurring
per-seat SaaS line item and a genuinely annoying thing to build well in-house. This gives
a self-hostable MIT-core equivalent, and the MCP authoring surface means a coding agent
can read an app's design system and produce a production-ready tour without a human
touching the visual builder.

## Problems It Can Remove
Replaces a paid tour/checklist/survey SaaS subscription, and the in-house build effort
(visual editor, targeting rules, analytics) that a tour tool would otherwise require.

## Practical Uses
- Self-host onboarding for the MSSP admin-portal SaaS instead of a third-party tour tool.
- Point an AI agent at a feature and have it draft a tour that matches existing styling.
- Run a lightweight in-app feedback survey without standing up a separate tool.

## Product Opportunities
Bundle self-hosted onboarding as a built-in feature of a client-facing admin portal
rather than linking out to a third-party embed; offer "AI-authored onboarding setup" as
a quick-win service using the MCP authoring flow.

## Agent / Automation Opportunities
Ships a native MCP server (`https://mcp.usertour.io/mcp` on the hosted version, or a
self-hosted instance's own URL) plus a Claude Code plugin (`usertour/skills`) that wires
MCP connection and authoring skills together in one install.

## Integration
`docker compose up -d` brings up three services (app, Postgres, Redis) for a full
self-hosted instance; a one-click Railway deploy is also provided. The product itself is
consumed via a JS/web SDK embedded in the target app, plus the MCP server for
agent-driven authoring. Low integration effort overall.

## Architecture Notes
MCP-native authoring is the interesting piece: rather than bolting an AI chatbot onto an
existing visual editor, the agent reads the app's design system and wires up the SDK
itself if it isn't already installed, then builds the flow through the same MCP tool
surface a human would eventually inspect in the dashboard.

## Maturity
Emerging but active: created 2025-01-24 (~22 months), 2,342 stars, 170 forks, 8
subscribers, 35 open issues. v0.9.9 released the same day as this review, with an active
Discord, blog, and Twitter presence. The low subscriber count relative to stars is worth
noting, but the cadence of releases and community channels argue against pure star
inflation.

## License
MIT for the core. Any directory with "enterprise" or "ee" in its name (feature,
permission, role, and plan enablement) is licensed separately under LICENSE.enterprise —
check before depending on code in those paths.

## Alternatives
Appcues, Userflow, Userpilot, Chameleon, UserGuiding, Frigade — all proprietary/hosted.
Usertour is the only entrant found with native MCP-based authoring and a straightforward
self-hosted Docker path.

## Risks / Limitations
- 8 subscribers vs. 2,342 stars is a soft signal to watch, though offset by visible
  community activity.
- Self-hosting needs Postgres + Redis, not a single-binary deploy.
- Enterprise-licensed directories gate some functionality; confirm licensing before
  relying on anything under an "ee" path.

## Recommendation
PROTOTYPE — self-host it against one real onboarding flow for an existing product and
try the MCP-authoring path before committing to it as the onboarding layer for a client
product.

## Change History
### 2026-11-22
Initial discovery and review.
