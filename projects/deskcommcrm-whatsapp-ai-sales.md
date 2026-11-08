# DeskcommCRM

## Summary
An open-source "AI sales OS" built around WhatsApp: a self-hosted CRM (Next.js 16, TypeScript, Supabase/Postgres) where AI agents handle inbound WhatsApp conversations — qualifying, answering, and selling — inside a sales-funnel/inbox UI, positioned as an open alternative to Kommo, Octadesk, and Intercom. Connects to WhatsApp via WAHA (an open WhatsApp HTTP API) rather than requiring the official Meta Business API.

## Why I Should Care
This was first surfaced on 2026-09-27 and deliberately held out of the catalog: at the time it was a single maintainer's repo that had hit 4,058 stars in ~5 months with a README built around an affiliate hosting partnership (HostGator) — a pattern worth letting settle before trusting as a genuine usefulness signal. Six weeks later, the project has grown to 5,268 stars under 170 recorded contributors (the all-contributors badge, not just one name doing all the commits), a live release cadence (v1.104.0 shipped yesterday), and sustained real engagement (496 open issues, 1,506 forks). The affiliate link is still in the README, but it now sits alongside genuine multi-contributor development rather than being the project's only visible activity — the concern this was held for has materially decoupled.

## Problems It Can Remove
- Building a WhatsApp-native sales/support inbox + AI-qualification layer from scratch.
- Paying for Kommo/Octadesk/Intercom-style WhatsApp CRM SaaS when the funnel, automation rules, and data need to be self-hosted.
- Manually running WhatsApp qualification/sales conversations that an LLM agent with the right prompt/context could handle at the first-response layer.

## Practical Uses
- A self-hosted WhatsApp sales funnel for a small business or agency client, with AI agents doing first-line qualification before human handoff.
- Reference implementation for integrating WAHA (self-hosted WhatsApp API) into a Next.js/Supabase app without the official Meta Business API's approval overhead.
- Multi-language install kit (Portuguese-first, with English/Spanish READMEs) worth studying for a product targeting LATAM SMBs specifically.

## Product Opportunities
- Directly templates a "WhatsApp AI sales agent as a service" micro-SaaS offering — self-host this per client, or fork it as the base of a white-labeled version.
- The "WHEN/IF/THEN" automation-rule engine plus AI-agent-per-client-niche pattern (README references "mount a client by niche") is a reusable pattern for any WhatsApp-commerce vertical.

## Agent / Automation Opportunities
AI agents (OpenRouter/Anthropic/OpenAI-backed) drive the actual sales conversations inside the product — not a coding-agent tool, but an end-user-facing agent product. Separately, the repo ships install guides that auto-load into Claude Code, Codex, Cursor, OpenCode, and Antigravity, so a coding agent can run the self-hosted install and client-onboarding flow conversationally (`/deskcomm-instalar` in Claude Code).

## Integration
Self-hosted via a one-command installer (`hostgator-setup-kit/install.sh`) that provisions Docker, the app image, WAHA, and either an external or same-VPS Supabase instance; idempotent re-runs. Works on any Docker-capable VPS (the installer detects existing reverse proxies for Hostinger/Coolify/Dokploy/CapRover setups), not only the HostGator partnership path. Requires: a domain, a Supabase project (or self-hosted Supabase), an LLM API key (OpenRouter/Anthropic/OpenAI), and a WhatsApp number connected via QR code. Integration effort: **Medium** — the installer handles infra provisioning, but the WhatsApp-connection and LLM-key setup are real operational steps, not a single `docker run`.

## Architecture Notes
WAHA (self-hosted WhatsApp Web API) as the messaging transport instead of the official Meta Business API — avoids Meta's business-verification process at the cost of relying on an unofficial WhatsApp Web protocol integration (the usual tradeoff for this class of tool). Supabase provides Postgres + Auth + Storage; a cron-driven automation worker executes the WHEN/IF/THEN rule engine; an "update agent" powers an in-app "Update now" button, separate from the install script.

## Maturity
Emerging-to-mature: created April 2026 (7 months old), but with a 170-contributor badge, 5,268 stars, 1,506 forks, 496 open issues, and a release shipped the day before this review (v1.104.0). The shift from single-maintainer to a real multi-contributor project in six weeks is the key maturity signal versus the 2026-09-27 review.

## License
MIT.

## Alternatives
- Kommo, Octadesk, Intercom — the paid SaaS this is explicitly positioned against.
- Already-catalogued NextCRM (reviewed the same day) — general-purpose CRM + invoicing, no WhatsApp/AI-sales-agent focus; pick NextCRM for a broad CRM, DeskcommCRM specifically for a WhatsApp-driven sales funnel.
- WAHA itself (the underlying WhatsApp API this depends on) is a separate, already-existing open-source project.

## Risks / Limitations
- Star:subscriber ratio (5,268:30 ≈ 175:1) remains elevated, though the fork count (1,506, ~28% of stars) and real multi-contributor commit history argue against this being inflated engagement.
- README still leads with the HostGator affiliate partnership — not disqualifying (clearly disclosed, common sponsorship model), but worth knowing the "1-command install" path is optimized for that specific host.
- Relies on WAHA's unofficial WhatsApp Web integration rather than Meta's official Business API — carries the inherent fragility/ToS risk of any unofficial WhatsApp automation layer.
- 496 open issues against a 7-month-old repo is a lot of open surface area; indicates real usage but also real rough edges.

## Recommendation
**PROTOTYPE** — the single-maintainer/affiliate-promotion concern that held this out of the catalog in September has genuinely resolved into real multi-contributor development. Worth testing specifically for a WhatsApp-sales-funnel use case; general-purpose CRM needs are better served by NextCRM.

## Change History
### 2026-11-08
Initial catalog entry. Previously observed 2026-09-27 and held as Worth Watching pending the single-maintainer/affiliate-promotion concern decoupling from genuine technical merit — now resolved (170 contributors, continued fast release cadence, sustained star/fork growth).
