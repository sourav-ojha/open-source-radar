# TimeTracker (DRYTRIX)

## Summary
Self-hosted time tracking, project management, and invoicing application (GPL-3.0, Flask + PostgreSQL) for freelancers, teams, and agencies. Beyond timers and billable-rate reporting, it generates EU-compliant e-invoices (Peppol BIS, Factur-X/ZUGFeRD, XRechnung), syncs with QuickBooks/Xero/Sage/DATEV accounting systems, pushes time entries into Gusto/ADP payroll, and ships a client portal with in-portal messaging and a Gmail/Outlook sync for CRM-lite contact tracking.

## Why I Should Care
It covers the full loop a freelancer or small agency actually needs — track hours, invoice a client, get the invoice into the client's and my own accounting system — in one self-hosted tool, instead of stitching together Toggl/Clockify + a separate invoicing app + manual accounting export. The EU e-invoicing compliance (Peppol/Factur-X/XRechnung) is specifically the kind of regulatory implementation work that is tedious and error-prone to build from scratch.

## Problems It Can Remove
Replaces a Toggl/Clockify subscription plus a separate invoicing tool (Bonsai, FreshBooks) plus manual CSV exports into accounting software. Removes the need to hand-build EU e-invoice format compliance if billing any EU-based client or entity.

## Practical Uses
- Track billable hours per client/project for consulting or PTaaS-partnership work and generate a compliant invoice directly from tracked time.
- Issue EU-compliant e-invoices (Peppol/Factur-X/ZUGFeRD/XRechnung) to clients that require them, without hand-rolling the format.
- Sync invoices/payments into QuickBooks, Xero, Sage, or DATEV instead of re-entering data.
- Give a client a portal to view/approve invoices and message the team directly, without a shared inbox.
- Export time entries to Gusto/ADP for contractor or employee payroll runs.

## Product Opportunities
Could be white-labeled as the billing/time-tracking backbone for client engagements under the MSSP or PTaaS partnership — client portal and white-label-adjacent reporting already exist. Less suited as an embeddable component (it's a full Flask app, not a library) than as a standalone internal tool.

## Agent / Automation Opportunities
Ships a REST API (`api_v1` blueprints) consumed by its own desktop and Flutter mobile apps, plus an "AI summarize-entries" feature already on its roadmap/shipped in recent releases (configurable AI provider via `AI_PROVIDER`/`AI_BASE_URL`). The REST API is a reasonable base for a future invoicing/time-entry MCP wrapper, though none ships today.

## Integration
`docker pull ghcr.io/drytrix/timetracker:latest` or Docker Compose (NAS templates for QNAP/Synology/Portainer also provided); one-click deploy to Render; Railway/Fly.io/Coolify documented. PostgreSQL recommended for production, SQLite for dev/test. Integration effort: **Low** for a standalone deployment — it is a complete app you run, not something wired into existing code.

## Architecture Notes
Flask app with a layered structure (routes → services → repositories → SQLAlchemy models), Marshmallow schemas for API validation, Flask-SocketIO for real-time timer updates, APScheduler for background jobs. Desktop app is an Electron-style bundle and the mobile app is Flutter, both talking to the same REST API rather than duplicating business logic. Telemetry is explicitly two-layer (always-on minimal heartbeat vs. opt-in detailed analytics) — worth checking before deployment if that matters for a client-facing install.

## Maturity
Mature in practice despite modest star count: created 2025-08-15, 628 stars, 69 forks, releases shipping roughly weekly with a detailed CHANGELOG (currently v5.17.2, released 2026-09-25). GitHub API verified: not archived, GPL-3.0, 5 open issues.

## License
GPL-3.0 (verified via repo metadata). Strong copyleft — if a modified version is distributed (not just run internally/self-hosted for your own or client use), the modified source must be made available under GPL-3.0. Running it as an internal tool or operating it on behalf of clients without distributing the modified software does not trigger this obligation. An optional paid "support key" exists purely to hide donation prompts — no feature gating behind it.

## Alternatives
- **solidtime** (already catalogued, AGPL-3.0, PROTOTYPE 7.3) — time tracking and Toggl/Clockify import only, no invoicing or e-invoicing compliance; lighter if invoicing isn't needed.
- Toggl / Clockify / Harvest (mainstream SaaS) — no self-hosting, ongoing per-seat cost.
- Bonsai / FreshBooks (SaaS invoicing) — no integrated time tracking at this depth, no EU e-invoicing compliance out of the box.

## Risks / Limitations
- GPL-3.0 (see License) — fine for internal/self-hosted use, a blocker only if planning to redistribute a modified version as software.
- Python/Flask stack, outside the core MERN toolchain — acceptable since it is consumed via Docker as a deployed app, not imported as a library.
- Single-maintainer-driven project (DRYTRIX) with a large feature surface (payroll, accounting sync, AI, geofencing) shipped very fast — verify any one integration (e.g. a specific accounting sync) works as documented before relying on it for real invoicing.
- Peppol network registration (as opposed to just generating Peppol-format XML) typically requires an Access Point provider separate from this software — verify what the "Peppol e-invoicing" feature actually covers (format generation vs. network transmission) before assuming full compliance.

## Recommendation
**USE NOW** (score 7.7/10) — low-risk to self-host for internal time tracking and invoicing today; worth validating the specific accounting-sync and e-invoicing-format claims against one real client before fully replacing an existing paid tool.

## Change History
### 2026-10-04
First discovered and reviewed (slot 5 — self-hosted SaaS alternatives, productivity). Verified via GitHub API: GPL-3.0, 628 stars, created 2025-08-15, latest release v5.17.2 (2026-09-25). Surfaced via a `self-hosted invoicing freelancer alternative` search.
