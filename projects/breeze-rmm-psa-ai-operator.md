# Breeze

## Summary
A self-hostable (or cloud-hosted) open-source RMM + PSA platform for MSPs/internal IT (AGPL-3.0): device monitoring, patching, remote access, scripting, network discovery, ticketing, quoting, and billing in one system, with an AI operator built into every page that can investigate alerts and act (within human-approved risk boundaries) rather than just surface a dashboard.

## Why I Should Care
Secondary-context fit: Sourav runs an MSSP security admin-portal SaaS and a PTaaS partnership. RMM (remote monitoring/management) + PSA (professional-services-automation: tickets/quotes/billing) in one AGPL-licensed, self-hostable system — with the ticket-to-invoice pipeline unified instead of synced between two tools — is directly adjacent to that business, even though it's not on the core MERN/AWS axis.

## Problems It Can Remove
- Running separate RMM and PSA tools with a sync job between them (the stated pain point this design avoids).
- Manually triaging every device alert instead of having an AI operator pre-investigate and propose (or, for low-risk actions, take) the fix.

## Practical Uses
- Device fleet monitoring, patch management, and remote access/scripting for an MSSP's or internal IT team's managed endpoints.
- Ticket → quote → invoice in one system for client billing, without a separate PSA/export step.
- A branded customer portal for end-users to open tickets and see their own devices/assets — relevant to existing MSSP client-facing portal work.
- AI-operator-assisted alert triage, with dangerous actions gated behind human approval (per its documented risk-classified action engine).

## Product Opportunities
Could be evaluated as the RMM/PSA backbone for the existing MSSP admin-portal SaaS, or as a reference architecture for building risk-classified AI-agent actions into an existing admin product (its "AI safety: risk-classified action engine, dangerous ops need approval, critical ops blocked" design is a reusable pattern regardless of whether Breeze itself is adopted).

## Agent / Automation Opportunities
The AI operator itself is the automation story here — not exposed as a generic MCP server for external use, but the internal design (per-page AI agent with scoped, risk-classified tool access) is a pattern worth studying for any admin panel that wants to add safe agentic actions.

## Integration
- Self-hosted (Docker-based; Go agent is a single cross-platform binary) or cloud-hosted at breezermm.com (US/EU regions).
- Postgres + Redis backend, row-level security enforced at the database layer (not just app-layer), per its published security practices doc.
- **Integration effort: Medium** — a full platform deployment (Postgres, Redis, agent distribution to endpoints), not a drop-in library.

## Architecture Notes
Notably thorough security posture for a ~9-month-old project: Argon2id + JWT + TOTP MFA, PostgreSQL RLS enforced even against table owners (no app-layer-only fallback), Redis-backed fail-closed rate limiting, 5 automated CI security scanners (CodeQL, Gitleaks, npm audit, govulncheck, Trivy), and a published disaster-recovery target (RTO < 1 hour, RPO < 15 minutes) with a security whitepaper mapped to SOC 2. Worth reading the security doc even if Breeze itself isn't adopted, as a checklist for what a serious multi-tenant admin platform's security posture should cover.

## Maturity
Created 2026-01-14 (~9 months old), 126 stars, 43 forks, active release cadence (v0.121.0 as of 2026-10-03), Discord community, live interactive feature demos on its marketing site — more operationally mature than its star count alone suggests, though still a small-scale community (2 watchers).

## License
**AGPL-3.0 — flagged loudly.** Self-hosting for internal/owned use carries no obligation. Reselling a *modified* instance as a hosted service to third-party clients (a plausible MSSP use case) would trigger AGPL's network-use source-disclosure requirement. The vendor's own commercial cloud-hosted option (breezermm.com) exists presumably as the path around this for anyone who wants a hosted version without the AGPL obligation falling on them.

## Alternatives
- Atera, NinjaOne, ConnectWise (established commercial RMM+PSA — not open source, ongoing per-seat cost).
- Tactical RMM (open source, GPL — RMM-only, no built-in PSA/billing).

## Why This One
The RMM+PSA unification (no separate billing sync step) and the AI-operator-as-first-class-citizen design (not a bolted-on chatbot) are the real differentiators versus both Tactical RMM (RMM-only) and the commercial incumbents (closed-source, per-seat pricing).

## Risks / Limitations
- AGPL-3.0 network-use clause is a real constraint if reselling a modified hosted instance to clients — plan around this explicitly before any commercial MSSP use.
- Smaller community (126★, 2 watchers) than the feature completeness suggests — less independent scrutiny than a larger project would have.
- Not on Sourav's core MERN/AWS axis; relevant specifically through the MSSP secondary context, not the primary productivity/product axes.

## Recommendation
PROTOTYPE — worth a closer look specifically through the MSSP lens (and its security-practices doc is worth reading regardless), but the AGPL hosting obligation needs to be resolved before any commercial redistribution.

## Change History
### 2026-10-05
Initial discovery via GitHub Search API during slot 6 research. Confirmed AGPL-3.0 license and read the published security-practices documentation.
