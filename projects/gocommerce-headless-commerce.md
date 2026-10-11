# GoCommerce

## Summary
GoCommerce is a composable commerce engine delivered as a Go library rather than a hosted platform or monolith: products/variants, inventory, carts, checkout, orders, payments, fulfillment and durable (outbox-pattern) events over a single PostgreSQL database. Integrations — Stripe, SendGrid, Meilisearch, Klaviyo, an MCP tool surface — are ordinary Go packages wired together in `main()`. The README claims a stable "1.0" API and HTTP contract.

## Why I Should Care
Not a direct stack fit (Go is a secondary language here, and Medusa.js/Vendure are the more natural Node/TS equivalents), but the architecture is a clean worked example of a correctness-sensitive domain service: one state machine shared by REST, webhooks, CSV import, MCP tool calls and admin actions, with durable outbox events rather than fire-and-forget side effects. Worth studying even without adopting it directly.

## Problems It Can Remove
- Hand-rolling order/payment state-machine correctness under concurrent writes
- Building a plugin-registry/DI-container abstraction layer that a small commerce engine doesn't need

## Practical Uses
- Embed basic commerce (catalog, cart, checkout, orders) into an existing Go backend without adopting a full commerce platform
- Reference the outbox-pattern event design and single-binary-plus-Postgres production architecture for other correctness-critical services
- Let a coding agent operate a store (read catalog, check orders, run diagnostics) through the dedicated MCP module, bounded by the same invariants the human admin panel enforces

## Product Opportunities
A narrow, embeddable commerce layer for a product that needs basic checkout/orders without justifying a dedicated commerce platform — low priority given the current stack, but a real option if a future product needs lightweight commerce.

## Agent / Automation Opportunities
Ships a purpose-built MCP module (`ext/mcp`) that exposes domain services — not raw SQL — as agent tools, with the same audit trail and invariants the admin panel and CLI use. The design explicitly treats an agent operating the store as bound by the same rules as a human.

## Integration
Single Go binary + PostgreSQL; embed as a Go library inside a larger application (`gocommerce.New(...)` wires config + payment/email/invoice modules, then `app.ListenAndServe()`). Integration effort: **Medium** — requires Go familiarity and manual wiring of each integration module; no managed hosting option.

## Architecture Notes
- "A store is a Go program" — no plugin registry, DI container, reflection, or configuration DSL; every wired module is visible in `main()`
- One binary, one PostgreSQL database is the entire production architecture; Redis/search/object storage are optional additions, never prerequisites
- Durable events: a state change and the outbox event describing it are written in the same transaction
- MCP module calls the same domain services REST does, never exposes arbitrary SQL, and never mutates core tables directly

## Maturity
Experimental in practice despite the README's "1.0" framing: created 2026-08-30, 6 GitHub stars, effectively solo-plus-AI-assisted (171 commits from the author, 5 from an AI-attributed contributor). No independent community vetting of the stability claim, and payments/commerce correctness is a high-stakes place to be wrong.

## License
MIT — no restrictions on commercial or SaaS use.

## Alternatives
Medusa.js (Node/TS, far more mainstream and a better stack fit for a MERN/Next.js-first developer), Vendure (Node/TS), Saleor (Python/GraphQL).

## Risks / Limitations
- "1.0" claim after ~6 weeks of history and 6 stars, with no independent vetting
- Effectively solo-plus-AI-assisted; real bus-factor risk
- Go is a secondary language for this operator's stack; Medusa.js/Vendure are the more natural adoption path if the goal is actual use rather than architecture study

## Recommendation
STUDY — the architecture (outbox events, single shared state machine across REST/webhooks/MCP/admin, deliberate avoidance of plugin-registry indirection) is worth reading even though direct adoption is unlikely given the Go/Node stack mismatch and the unproven "1.0" stability claim for payments-critical code.

## Change History
### 2026-10-11
First reviewed and catalogued. Found via GitHub topic search (`topic:admin-panel`, created:>2026-08-01); compared against Medusa.js/Vendure as the more mainstream Node/TS alternatives.
