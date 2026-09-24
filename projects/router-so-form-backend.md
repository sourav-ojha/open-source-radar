# Router.so

## Summary
Self-hostable, headless form handling and lead-routing backend "for marketing-minded
developers." Provides a headless endpoint contract, an optional hosted form UI
(forms.router.so or self-hosted), a WordPress block/shortcode, and Resend-based email
delivery. Deployable to Vercel or via Docker with a Postgres database.

## Why I Should Care
Every landing page or MVP needs a form-to-email backend, and this is small and complete
enough to self-host with confidence instead of adding another recurring SaaS line item
across every side project or client micro-SaaS.

## Problems It Can Remove
- A monthly Formspree/Basin/Getform subscription for form handling on marketing sites
  and MVPs.
- Building yet another one-off "send this form to my inbox" endpoint per project.
- Wiring a heavier tool like n8n just to route a form submission to email.

## Practical Uses
- Self-hosted form backend for a landing page or client micro-SaaS MVP.
- Lead-capture/routing for a marketing site.
- Drop the included form UI directly into a site, or keep only the headless endpoint
  contract and build a custom frontend.
- Embed into an existing WordPress site via the included block/shortcode.

## Product Opportunities
A small, always-needed building block worth owning once and reusing across every future
project instead of budgeting a recurring SaaS cost per client site.

## Agent / Automation Opportunities
None specific to AI agents — a plain infrastructure utility, useful as a component
behind any agent-built landing page or marketing site.

## Integration
Low effort. Self-host via Vercel (one-click deploy button) or Docker Compose, with a
PostgreSQL database and a Resend account for email delivery. `pnpm install
--frozen-lockfile` is required (not npm/yarn) to apply security-pinned dependency
overrides.

## Architecture Notes
Ships a `pnpm.overrides` block that force-resolves transitive dependencies a direct
dependency still pins to a vulnerable version (e.g. `next` pinning an old `postcss`) —
a pragmatic pattern worth copying in any Next.js project stuck waiting on upstream
patches.

## Maturity
Emerging. Created March 2024, actively pushed to as recently as September 2026, small
team (2 primary contributors). No tagged GitHub release — still on `package.json`
version `0.1.0` — so track main-branch stability directly rather than assuming semver
discipline.

## License
AGPL-3.0. Self-hosting and internal/product-embedded use are unrestricted, but a
modified, network-accessible version offered as a hosted service to third parties would
trigger the AGPL's network-use source-disclosure clause. Not a blocker for personal or
internal-product use; flag before ever reselling a modified hosted version.

## Alternatives
- **Formspree, Basin, Getform** — hosted SaaS equivalents, ongoing cost.
- **n8n** webhook + email node — excluded as already-mainstream, heavier for this one job.

## Risks / Limitations
- AGPL-3.0 licensing requires attention if ever offered as a modified hosted service.
- No tagged releases; must track `main` directly.
- Small maintainer base.

## Recommendation
PROTOTYPE — low-risk to self-host on a Vercel + Postgres + Resend stack for one project
first, then reuse across future landing pages/MVPs if it holds up.

## Change History
### 2026-09-24
Discovered and catalogued as this run's Small but High-Leverage Utility pick.
