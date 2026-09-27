# Macro

## Summary
An all-in-one team workspace — email, chat, docs, tasks, calls, file storage, and CRM — unified over a single bidirectional graph backend, so any object (an email thread, a task, a doc, a channel message) can be `@`-linked to any other and is natively visible to team AI agents with shared, team-level memory. Built in SolidJS and Rust, dogfooded for two years by a ~15-person team before being open-sourced under AGPL-3.0.

## Why I Should Care
The architecture is the interesting part, not the product as a drop-in adoption: instead of Slack + Linear + Notion + HubSpot + Superhuman glued together with Zapier and MCP (the exact failure mode the maintainers describe hitting at ~20 people), every surface shares one backend and one permission system, with agents treated as first-class citizens of the graph rather than a bolted-on chatbot.

## Problems It Can Remove
- The "company held together by Zapier and MCP" failure mode of ten separate SaaS tools that don't share context.
- Building bespoke cross-tool linking (e.g. "which emails relate to this customer's support tickets and open tasks") by hand.
- Giving an AI agent scattered, per-tool context instead of one coherent memory of the team's work.

## Practical Uses
- Not a near-term adoption candidate given AGPL licensing and self-hosting complexity — primarily useful as an architecture reference.
- Borrowing the "bidirectional graph + @-mentions across every object type" pattern for a narrower internal tool (e.g. linking support tickets, CRM contacts, and tasks in a smaller product).
- Studying how they expose a unified search/tool surface to AI agents across heterogeneous data (e.g. searching PDF attachments across email without pulling whole threads first).

## Product Opportunities
The cross-object graph plus team-level agent memory pattern is reusable at much smaller scope — e.g. a support-inbox-plus-CRM-plus-tasks product for a specific vertical — without needing to replicate Macro's full surface area (email, calls, canvas, PRs, etc).

## Agent / Automation Opportunities
Agents are designed as first-class workspace participants: a unified memory spans email, chat, docs, and CRM, and a dedicated tools/MCP surface lets agents search across all objects (including PDF attachments parsed out of email) and take actions like drafting/sending email or updating CRM records from chat.

## Integration
Self-hosting is technically supported under AGPLv3 (see the project's FAQ), but the monorepo is substantial: 42 deployable services, 167 Rust crates, shared TypeScript packages, Pulumi-managed infrastructure, and a Nix dev shell. A local Docker Compose stack exists for development. Macro is primarily distributed as a hosted product (macro.com) with self-hosting as a secondary, less-traveled path. **Integration effort: High.**

## Architecture Notes
Hexagonal service layout (inbound adapters → domain core with ports → outbound adapters) across services rather than a monolith. The defining idea is that every UI surface (email, chat, docs, canvas, CRM) is purpose-built rather than composed from one generic block primitive, but all of them share the same backend, and cross-references (a doc linked from a task, a channel message tied to a CRM record) are stored natively as a bidirectional graph rather than bolted-on foreign keys.

## Maturity
Emerging as an open-source project, but not as a product: created 2025-11-08 (open-sourced ~10.5 months ago), 4,455 stars, 432 forks, near-daily releases (latest v2026.9.25.4), and two years of internal dogfooding by a paid team before release. Real, funded engineering (contributors show 400-900+ commits each, consistent with a small paid team, not a single hobbyist).

## License
AGPL-3.0 — flagged loudly given the intent to embed things in commercial products. Self-hosting internally is fine under AGPL terms; offering a modified version of Macro as a hosted service to others would trigger AGPL's source-disclosure requirement unless a separate commercial license is obtained from the maintainers (licensing@macro.com) or a managed-hosting arrangement is used (self-host@macro.com).

## Alternatives
The SaaS stack it explicitly targets replacing — Slack, Linear, Notion, HubSpot, Superhuman. Partial-overlap open-source alternatives: Twenty CRM (CRM only), Docmost (docs/wiki only), AFFiNE (docs/canvas only) — none unify all of Macro's surface area under one data model.

## Risks / Limitations
- AGPL-3.0 needs legal attention before any commercial derivative use.
- Large self-hosting footprint (167 Rust crates, 42 services, Pulumi IaC) — not a weekend deploy.
- Primarily a hosted-product company with self-hosting as a secondary path; depth of self-host documentation/support is unproven relative to the hosted offering.
- Adopting the whole platform to get the architectural benefit means moving email, chat, docs, and CRM simultaneously — a large migration, not an incremental one.

## Recommendation
STUDY — the bidirectional-graph-plus-agent-memory architecture is worth understanding and possibly borrowing a slice of, but full adoption is not warranted given the AGPL license, the self-hosting effort, and the all-or-nothing nature of migrating every workspace tool at once.

## Change History
### 2026-09-27
Discovered and reviewed. GitHub API verified: AGPL-3.0, 4,455 stars, 432 forks, created 2025-11-08, latest release v2026.9.25.4 (2026-09-26), archived: false. README confirms self-hosting is supported under AGPLv3 per the project's FAQ, and details the monorepo's Rust/SolidJS/Pulumi structure.
