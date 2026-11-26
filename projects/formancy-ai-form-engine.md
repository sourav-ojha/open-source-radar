# formancy.ai

## Summary
A self-hostable, open-source visual form builder and rendering engine for Angular and React,
sharing one validation/condition/calculation engine across browser and server. Includes an
optional Fastify+Postgres backend for submissions, drafts, file uploads (ClamAV-scanned),
webhooks, CSV export, and an audit log, plus an MCP server exposing nine tools for agent-driven
form authoring.

## Why I Should Care
Forms-with-logic are a recurring need across almost every product idea, and this is the rare
fully open (Apache-2.0) option with a real visual builder — not just a headless validation
library — that renders identically in React and Angular from one schema, so client and server
rules cannot drift apart.

## Problems It Can Remove
- A hand-rolled multi-step form plus a separate, hand-maintained server-side validation copy of
  the same rules
- A paid form SaaS (Typeform/Tally/Feathery) for anything beyond the simplest intake form
- Re-implementing conditional-field logic and calculated totals in both the UI and the backend

## Practical Uses
- Multi-step intake/signup/application forms with conditional questions and calculated totals
- A self-hosted submissions backend (drafts, file uploads, CSV export, webhooks, audit log)
  instead of a hosted form SaaS
- Embedding a themeable, headless renderer into an existing design system (zero shipped CSS)
- Letting a coding agent draft and validate a form definition via MCP before anything deploys

## Product Opportunities
- A white-labelable form/intake module for a micro-SaaS product (onboarding, lead capture,
  support intake) on a fully self-hosted stack instead of a form SaaS subscription
- The open-core structure itself is worth studying: core engine fully open, three value-add
  modules (Formancy Data, Report, Health) sold separately from their own repos under a
  source-available license

## Agent / Automation Opportunities
MCP server ships 9 tools; 5 of them (`describe_spec`, `validate_form`, `diff_forms`,
`check_scenarios`, and the checking half of `publish_form`) need no server or credentials, so an
agent can write and validate a whole form offline before anybody deploys anything. CEL-based rule
type-checking catches logic mistakes (e.g. an `int * float` overload error) at validation time
rather than shipping a form whose calculated total silently stays empty.

## Integration
`npm install @formancy/react @formancy/core @formancy/spec` (or the Angular equivalents), ESM
only, Node >= 22.12. The optional backend ships as a Docker container (Fastify + Postgres) rather
than a dependency. **Integration effort: Low** for the renderer/library; **Low-Medium** for the
self-hosted backend given its own "not ready for public deployment" caveat (see Risks).

## Architecture Notes
The validation/condition/calculation engine is compiled once and shared verbatim between the
React and Angular renderers — "two frameworks, one tested contract" — so there's no separate
reimplementation to drift between them. Rich-text answers are stored as a small closed grammar
rather than HTML, eliminating an entire class of injection risk by construction rather than by
sanitization. The project also ships unusually deep regulatory documentation (an IEC 62304 SOUP
declaration, safety analysis, and lifecycle docs) aimed explicitly at teams embedding it in a
regulated product.

## Maturity
Emerging/beta (v0.14.0). CI, Codecov coverage, and npm-published packages with provenance and a
signed CycloneDX SBOM per release are in place despite a very small community (56 stars, 0
subscribers, 2 open issues) — reads like a solo or small-team project built with unusual
engineering discipline rather than a hype entrant.

## License
Apache-2.0 across the spec, engine, renderers, builder, and backend — "no paywalled essentials."
Three adjacent modules (Formancy Data, Report, Health) live in separate repos under a
source-available (not Apache-2.0) license: free to read/evaluate/develop with, paid only in
production. The core form engine used inside a product is unrestricted.

## Alternatives
SurveyJS (dual-licensed, creator component not fully open), Feathery (hosted-first), react-hook-
form (a headless validation library only — no visual builder or self-hosted backend), Typeform/
Tally (closed SaaS).

## Risks / Limitations
- The self-hosted backend is explicitly described in its own documentation as "not ready for a
  public deployment," despite the project enumerating in detail what it already has (auth, RBAC,
  rate limiting, audit log) and what it still lacks
- Package APIs are not frozen pre-1.0; breaking changes are possible before a 1.0 release
- Very small community so far relative to the documentation depth — single-maintainer bus-factor
  risk
- Requires Node >= 22.12, a recent runtime baseline

## Recommendation
PROTOTYPE — the renderer and builder are solid enough to try on a real form today; treat the
self-hosted backend as an evaluation target, not a production deployment, until its own
documented gaps close.

## Change History
### 2026-11-26
Discovered via GitHub topic search (slot 2: product infrastructure, APIs, backend components).
First catalog entry.
