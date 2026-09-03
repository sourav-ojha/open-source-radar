# Client St0r

## Summary
Client St0r is a self-hosted MSP/IT documentation and PSA (professional services
automation) platform — asset management, encrypted password vault, knowledge base,
tickets, projects, contracts, quotes, invoices, distributor tracking, and a customer
portal — built on Django 6 + MariaDB. Positioned explicitly as an open-source alternative
to IT Glue and Hudu.

## Why I Should Care
This lands directly on the MSSP security admin-portal side of the business rather than
the core MERN/AI-agent axis: it is a full, self-hostable replacement for the category of
paid SaaS (IT Glue, Hudu) that MSPs/MSSPs typically pay per-seat for just to document
client infrastructure and manage tickets/contracts.

## Problems It Can Remove
Removes the recurring per-seat SaaS bill for IT documentation/PSA tooling, and removes the
"where do we track client assets, contracts, and vault credentials" gap for an MSSP
practice that doesn't already have one, without building that tooling in-house.

## Practical Uses
- Self-host as the documentation/PSA layer for the MSSP practice instead of paying for IT
  Glue or Hudu.
- Use the encrypted password vault + asset management modules as a lighter-weight
  alternative to a general secrets manager for client-specific credentials.
- Study the customer-portal and PSA workflow (tickets → projects → contracts → quotes →
  invoices) as a reference architecture for the MSSP admin-portal SaaS's own client-facing
  surface.

## Product Opportunities
The ticket-to-invoice PSA workflow and customer-portal pattern are directly reusable
reference points for structuring the MSSP admin-portal SaaS's own client-facing features,
even if Client St0r itself isn't adopted wholesale.

## Agent / Automation Opportunities
README lists "AI assist" as a feature but does not specify what it does or how it's
implemented — **unclear based on available documentation**; verify hands-on before relying
on it for anything.

## Integration
Self-hosted Django 6 + MariaDB application; homepage suggests a hosted demo/reboot service
exists alongside self-hosting. Different stack from Sourav's primary MERN/Node stack, but
runs as a standalone application rather than something embedded into other code, so the
stack mismatch matters less than for a library. **Integration effort: Medium** — new
infrastructure to deploy and operate (Django app + MariaDB), plus migration effort if
replacing existing MSP tooling.

## Architecture Notes
Django 6 / Python 3.12+ / MariaDB. Ships Snyk vulnerability scanning and
Have-I-Been-Pwned integration badges for its own security posture, which is a reasonable
signal for a tool whose core job is storing client credentials.

## Maturity
Emerging. Created 2026-01-10 (~8 months old), 71 stars, 19 forks, marked "production
ready" in its own README, latest tagged release v3.17.495 (2026-05-25) though pushed_at is
more recent (2026-09-02) — some post-tag development has landed unreleased; worth
confirming the actual running version before deploying.

## License
MIT. No commercial-use restrictions identified.

## Alternatives
IT Glue and Hudu (proprietary, per-seat SaaS — the incumbents this explicitly targets),
ITFlow (another open-source MSP documentation/PSA project, not directly compared here).
Client St0r differs mainly by bundling PSA (tickets/projects/contracts/quotes/invoices)
alongside documentation in one self-hosted app rather than documentation alone.

## Risks / Limitations
- Version drift between the README's badge (3.17.444) and the latest GitHub release tag
  (3.17.495) — minor, but worth double-checking which version actually deploys.
- Single-maintainer-scale project (71 stars) holding sensitive client credentials and
  contracts; back up the encrypted vault independently regardless of the tool's own
  claims.
- "AI assist" feature is unverified — see Agent/Automation Opportunities.

## Recommendation
**PROTOTYPE** — worth a self-hosted trial deployment to evaluate the vault, asset
management, and PSA workflow against whatever tooling (or lack of tooling) the MSSP
practice currently uses for client documentation.

## Change History
### 2026-09-03
Initial discovery and review. GitHub API confirmed MIT, 71 stars, created 2026-01-10,
pushed_at 2026-09-02, not archived, latest release v3.17.495. README verified via
raw.githubusercontent.com.
