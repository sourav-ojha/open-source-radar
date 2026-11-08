# NextCRM

## Summary
An open-source CRM built on Next.js 16, React 19, TypeScript, PostgreSQL (Prisma 7), and shadcn/ui. Beyond standard CRM entities (accounts, contacts, leads, opportunities, contracts), it ships a full invoicing module, per-entity audit logging, pgvector-backed semantic search, an E2B-sandboxed AI research agent for contact/company enrichment, and a built-in MCP server for direct AI-agent access to CRM data.

## Why I Should Care
This is close to a direct match for my stack (Next.js/React/Postgres/Prisma) rather than a foreign ecosystem I'd have to adopt wholesale. A CRM with invoicing, audit trail, and vector search is a meaningful chunk of what a B2B SaaS product or internal sales-ops tool would otherwise need built from scratch — and the codebase is readable/forkable rather than a black box, since it's the same stack I already write.

## Problems It Can Remove
- Building CRM + invoicing + audit-log + document-storage modules from scratch for an internal sales tool or as the seed of a vertical SaaS product.
- Paying for a hosted CRM (HubSpot, Pipedrive, Kommo) when the data model needs to be owned/extended.
- Wiring up pgvector semantic search and an LLM enrichment pipeline manually — this ships both already, including the background-job plumbing (Inngest) and sandboxed browser-driven research agent (E2B + Firecrawl/OpenAI).

## Practical Uses
- Internal sales/CRM tool, self-hosted on existing Postgres infra.
- Seed codebase for a vertical CRM product (fork and narrow the data model to one industry).
- Reference implementation for a Next.js app that exposes its own data over MCP — useful as an architecture pattern independent of the CRM use case.
- Invoicing module (multi-currency, tax engine, PDF export, status lifecycle) as a lift-out reference for a billing feature elsewhere.

## Product Opportunities
- Forkable base for a narrow vertical CRM (e.g. a sales tool for a specific SMB niche) rather than building the CRUD/auth/audit-log scaffolding from zero.
- The MCP server pattern (Bearer-token-scoped, streamable HTTP + SSE transports) is directly reusable as a template for exposing any of my own Next.js apps' data to agents.

## Agent / Automation Opportunities
Built-in MCP server (`/api/mcp/mcp`) with per-user Bearer tokens (SHA-256 hashed, shown once) gives Claude/Cursor/custom agents read/write access to CRM records. Ships a downloadable Claude Code Skill (`SKILL.md`) generated from the live tool schema. The AI-enrichment pipeline (Claude Sonnet-driven browser agent via E2B, with confidence-scored field writes) is a concrete pattern for "agent researches the web, only writes high-confidence fields."

## Integration
Self-hosted: Next.js app + PostgreSQL (pgvector extension) + Prisma migrations (`pnpm prisma migrate deploy`). Core CRM/invoicing works without AI keys; AI features (enrichment, embeddings, MCP tool-use loop) need OpenAI/Anthropic/Firecrawl/E2B keys, resolved through a 3-tier priority system (env → admin → user) so the app degrades gracefully without them. Hosted demo at demo.nextcrm.io. Integration effort: **Medium** — straightforward for the core CRM, more setup if the AI/vector-search/MCP layer is wanted.

## Architecture Notes
Server actions (not API-route middleman) for CRUD with Zod validation; soft-delete-everywhere with a generic diff-based audit engine (`diffObjects`) feeding a global filterable admin audit log; pgvector HNSW indexes for "find similar record" across CRM entity types; a 3-tier credential resolution system for AI keys that's a clean pattern worth reusing in other apps. The AI enrichment agent explicitly skips research steps it judges unnecessary (e.g. skip domain discovery if a website is already on file) and discards low-confidence field writes — a sensible default-safe design for agent-written data.

## Maturity
Mature and actively developed: created July 2023 (3+ years), 739 stars, 287 forks, 8 subscribers, 22 open issues, tagged release v0.37.20 published today (2026-11-08). Frequent dated changelog entries in the README itself (invoicing, audit log, MCP server, vector search, CRM targets all marked "NEW"), suggesting a real, still-moving feature roadmap rather than an abandoned CRUD app.

## License
MIT — no restrictions on commercial use, self-hosting, or embedding in a product.

## Alternatives
- Twenty, Odoo CRM, SuiteCRM — more established open-source CRMs, but none match the Next.js/Prisma/shadcn stack or ship a first-class MCP server + E2B-sandboxed enrichment agent out of the box.
- Hosted CRMs (HubSpot, Pipedrive, Kommo) — the SaaS-dependency this replaces for teams willing to self-host.
- Already-catalogued DeskcommCRM (reviewed the same day) — narrower WhatsApp-sales-funnel focus vs. NextCRM's general-purpose CRM + invoicing + document scope; see that project's "Alternatives" section for the direct comparison.

## Risks / Limitations
- Star:subscriber ratio (739:8 ≈ 92:1) is on the high side, though the 287-fork count (real engagement, people pulling the code to run/modify it) and 3+ years of continuous history make this read as organic rather than inflated.
- AI-feature surface area (E2B, Firecrawl, multiple LLM providers) adds real operational complexity and external-API cost if those features are turned on.
- No independent production-deployment case studies found beyond the project's own demo and changelog — treat "production-ready" as self-reported until hands-on tested.

## Recommendation
**PROTOTYPE** — strongest of today's self-hosted-CRM wave for this stack specifically; worth spinning up the Docker/demo path and testing the MCP server and invoicing module against a real internal use case before deeper adoption.

## Change History
### 2026-11-08
Initial discovery and review.
