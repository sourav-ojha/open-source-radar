# DaloyJS

## Summary
A TypeScript REST API framework (`@daloyjs/core`) that runs on Node, Bun, Deno, Cloudflare Workers, Vercel, Fastly, Lambda, and Azure. Each route definition is the single source of truth for request/response validation (via Standard Schema, so Zod/Valibot/ArkType/TypeBox all work), generated types, OpenAPI 3.1 docs (Scalar/Swagger UI/Redoc), and a generated typed in-process client. Ships security guardrails on by default — body-size limits, prototype-pollution-safe JSON parsing, path-traversal rejection, RFC 9457 error bodies, and boot-time refusal on known misconfigurations like a weak session secret or missing CSRF protection — plus middleware for CORS/CSRF/rate limiting/sessions/JWT/idempotency/response caching, and dedicated MCP/A2A agent-endpoint modules.

## Why I Should Care
It's a concrete demonstration of contract-first API design (one route definition drives types, docs, and a typed client) plus boot-time misconfiguration detection, aimed squarely at Node/TypeScript — his primary stack. Not a drop-in replacement for Express/NestJS, but a source of patterns worth lifting regardless of adoption decision.

## Problems It Can Remove
The usual manual assembly of validation + OpenAPI generation + a hand-written or separately-generated typed client + security middleware choices, each currently a separate decision and a separate library in an Express/NestJS project.

## Practical Uses
- Reference for adding contract-first OpenAPI generation plus a typed client to an existing Express/NestJS service, without adopting the whole framework
- Candidate framework for a new greenfield micro-SaaS backend, where boot-time guardrails catch a misconfigured session secret or missing CSRF protection before it ships
- Reference for exposing an agent-facing (MCP/A2A) API surface from a REST framework rather than bolting it on separately

## Product Opportunities
The "refuse to boot on a known misconfiguration" pattern (weak session secret, missing CSRF, auth declared without an auth hook) is a concrete idea worth lifting into any Node service regardless of framework choice — a cheap way to catch a class of production misconfiguration bugs before deploy.

## Agent / Automation Opportunities
Built-in MCP and A2A agent-endpoint middleware modules — one of the few REST frameworks treating agent-facing endpoints as first-class rather than an afterthought bolted onto a REST API.

## Integration
Deployment: npm package (`@daloyjs/core`), runs on Node/Bun/Deno/Cloudflare Workers/Vercel/Fastly/Lambda/Azure.
Interfaces: REST/HTTP, OpenAPI 3.1 (Scalar/Swagger UI/Redoc), generated typed in-process client, MCP/A2A agent-endpoint modules.
Integration effort: **Medium** — this is a framework-adoption decision, not a drop-in library; a service-by-service pilot is feasible given the multi-runtime portability.

## Architecture Notes
Core has zero runtime dependencies; publishes with npm provenance and SBOMs. The scaffolder sets pnpm `ignoreScripts` and a 24-hour release-age cooldown for new dependencies. Coverage gates (>=90% line/function, >=92% branch) plus CodeQL, OpenSSF Scorecard, and zizmor run in CI. The README is explicit about what the guardrails don't cover — notably, its slopsquatting/package-allowlist defense runs only in this repo's own CI, not scaffolded into generated user apps, which is weaker than the "secure AI-assisted services" framing implies at first read.

## Maturity
Emerging — 1.x line (currently v1.15.0 on npm), 2 contributors, repo created 2026-05-14. Stated SemVer policy and a 5-year security-support commitment for the 1.x line; the surface area is large for a 1.x release, so long-term maintenance is unproven despite the rigorous CI.

## License
MIT. No restrictions on commercial use, SaaS deployment, or embedding.

## Alternatives
- Express (his current stack) — DaloyJS's entire pitch is the boilerplate (validation, OpenAPI, typed client, security defaults) Express leaves to the developer
- NestJS (his current stack) — heavier, DI-based; DaloyJS is lighter-weight and portable to edge runtimes
- Encore.ts (open source, ~11k GitHub stars) — similar contract-first/typed-API pitch plus infrastructure-as-code provisioning (databases, Pub/Sub, cron) that DaloyJS does not attempt; more established and used in production (e.g. Groupon) but more opinionated about infrastructure
- Hono — minimal and edge-first, but without DaloyJS's bundled contract-first OpenAPI/typed-client/security-guardrail stack

## Risks / Limitations
- 1.x with only 2 contributors — small team, unproven at scale despite rigorous CI
- README marketing framing ("vibe-coding era") oversells some claims relative to what's actually scaffolded into generated apps
- Migrating an existing Express/NestJS service is a real framework-adoption decision — treat as something to study and borrow patterns from, not migrate to directly

## Recommendation
**STUDY** (score 7.0/10) — the contract-first + boot-time-guardrail pattern is worth borrowing into existing Express/NestJS work; full adoption is a bigger bet than the project's current maturity justifies.

## Change History
### 2026-10-22
First discovered via GitHub search. Verified via GitHub API (MIT, 28 stars, 0 subscribers, 2 forks, created 2026-05-14, 2 contributors) and npm registry (@daloyjs/core latest 1.15.0, published 2026-10-20).
