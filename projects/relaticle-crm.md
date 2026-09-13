# Relaticle

## Summary
Relaticle is a self-hosted CRM (Laravel 13 + Filament 5 + PHP 8.5, PostgreSQL 17) built
around AI-agent access as a first-class feature: a production-grade MCP server exposing
37 tools for CRM operations, plus a full REST API. Contacts, companies, deals/pipelines,
22 custom field types, and multi-team workspace isolation.

## Why I Should Care
Most self-hosted CRMs treat API/agent access as an afterthought bolted onto a UI-first
product. Relaticle inverts that: the MCP server and REST API are documented as core
surface area, not a plugin. That is directly useful both as a CRM to actually run a
client/lead pipeline for the MSSP admin-portal SaaS or PTaaS partnership, and as a
reference implementation for how to expose a data-heavy product's operations to an AI
agent safely (tool scoping, per-field encryption, 5-layer team authorization).

## Problems It Can Remove
Removes the need to build or pay for a CRM to track clients/leads for side businesses,
and removes the "how do we let an agent touch customer data safely" design problem —
Relaticle's MCP tool boundaries and team-scoped authorization are a working answer to
copy from.

## Practical Uses
- Run it as the actual CRM for MSSP/PTaaS client and lead tracking instead of a
  spreadsheet or a paid SaaS.
- Point a coding/ops agent at the MCP server to auto-log meeting notes, update deal
  stages, or generate pipeline summaries without a human touching the UI.
- Study the 22-field-type + per-field-encryption data model before designing a similar
  flexible-schema feature in an in-house product.
- Use the REST API as a backend for a lightweight internal dashboard without touching
  the Filament admin UI at all.

## Product Opportunities
The "AI-native CRM" positioning (agent-operable via MCP, not just human-operable via UI)
is a pattern worth lifting into any admin-portal SaaS: expose core CRUD + reporting as
MCP tools alongside the REST API from day one instead of retrofitting it later.

## Agent / Automation Opportunities
First-party MCP server with 37 tools (CRUD, schema introspection, activity history,
pipeline analysis) documented at relaticle.com/docs/mcp — connects directly to Claude,
GPT, or local models. This is the standout feature relative to other self-hosted CRMs.

## Integration
`git clone` + `composer app-install`, or Docker via the published self-hosting guide.
Requires PHP 8.5+, PostgreSQL 17+, Composer 2, Node 22+, Redis (optional in dev).
**Integration effort: Medium** — Laravel/Filament is outside the core MERN stack, so
there's a real ecosystem-learning cost even though the app itself is turnkey to deploy;
consumption via REST API or MCP avoids touching PHP entirely.

## Architecture Notes
Filament 5 (admin-panel framework) provides the UI layer over a standard Laravel app;
the interesting part is that the MCP server and REST API are maintained as first-class,
tested surfaces (2,000+ automated tests) rather than an add-on — worth studying the tool
boundary design even without adopting Laravel.

## Maturity
Emerging-to-mature. Created 2024-09-25 (~2 years old), 1,648 stars, active (pushed same
day as review), tagged release v3.5.8 (2026-09-12). Effectively single-maintainer by
commit volume (ManukMinasyan: 1,428 of ~1,445 commits) with a handful of smaller
contributors — real bus-factor risk despite the activity level.

## License
AGPL-3.0. **Flagged per policy**: AGPL requires that any modified version offered as a
network service also make its source available. Fine for internal/self-hosted use as-is;
review before embedding a modified fork into a commercial hosted offering.

## Alternatives
Twenty (already known/mainstream, no native MCP server), EspoCRM, SuiteCRM, Django-CRM.
Relaticle differs by making agent access (MCP) a primary, documented feature rather than
an afterthought, and by shipping a genuinely flexible per-field-encrypted data model.

## Risks / Limitations
- Effective single-maintainer project — check activity before depending on it for
  anything business-critical.
- AGPL-3.0 (see License).
- Laravel/PHP stack means any customization beyond configuration requires PHP expertise
  not currently in daily use.

## Recommendation
**PROTOTYPE** — worth standing up via Docker to evaluate the MCP server against a real
lead-tracking workflow for the MSSP/PTaaS side of the business, and to study the
agent-tool-boundary design before it's needed elsewhere.

## Change History
### 2026-09-13
Initial discovery and review. GitHub API confirmed AGPL-3.0, 1,648 stars, created
2024-09-25, pushed 2026-09-13, not archived, latest release v3.5.8 (2026-09-12).
Contributor list confirms real but concentrated maintenance. README verified via
raw.githubusercontent.com.
