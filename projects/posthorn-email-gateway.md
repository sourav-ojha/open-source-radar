# Posthorn

## Summary
Posthorn is a single Go binary that sits between every self-hosted app you run and the
transactional email provider you've already picked (Postmark, Resend, Mailgun, AWS SES,
or an outbound-SMTP relay). It accepts mail three ways — an HTTP form endpoint, an HTTP
JSON API, or as an SMTP listener — and forwards everything through one shared set of
provider credentials, with anti-spam and retry handling built in.

## Why I Should Care
Every self-hosted app that needs to send mail (contact forms, admin alerts, Gitea magic
links, cron digests, a Cloudflare Worker's password-reset email) otherwise ends up with
its own copy of the provider API key and its own retry/rate-limit/spam logic. Posthorn
collapses that into one deployed service with one config file, which is exactly the kind
of "boring glue I'd otherwise rebuild per project" this catalog exists to catch.

## Problems It Can Remove
- Rewriting the same "call the SES/Postmark API with retries and rate limiting" code in
  every backend service or Cloudflare Worker that needs to send mail.
- Managing provider API keys separately per app instead of once, centrally.
- The fact that several common cloud hosts (DigitalOcean, AWS Lightsail, Linode, Vultr)
  block outbound SMTP entirely, breaking SMTP-only apps unless something bridges the gap.
- Basic contact-form spam (honeypot + Origin/Referer + rate limiting are built in, so a
  form endpoint doesn't need its own anti-spam logic).

## Practical Uses
- Front a marketing site's contact form and any signup forms with the HTTP form ingress.
- Route password-reset / alert emails from internal cron jobs or Lambda-style workers
  through the HTTP API ingress with bearer-token auth and idempotent retries.
- Sit in front of self-hosted tools that only speak SMTP (Gitea, Mastodon, NextCloud,
  Authentik) so they can send through a modern provider without native integration work.
- Centralize the AWS SES credentials used across several small self-hosted or client
  projects instead of duplicating them.

## Product Opportunities
- The pattern (one internal mail gateway shared across every deployed service) is
  directly reusable as internal infrastructure behind an admin-portal SaaS or any
  multi-tenant product that needs outbound transactional mail without hand-rolling
  deliverability handling per service.

## Agent / Automation Opportunities
- No MCP/agent-specific surface, but the HTTP API ingress (bearer auth, JSON body,
  idempotent retries) is trivial for an agent or automation script to call directly as a
  "send email" tool without needing SES/Postmark SDK-specific code.

## Integration
Single Docker container (`ghcr.io/craigmccaskill/posthorn`), one TOML config file, one
provider API key as an env var. Integration effort: **Low** for a first email route;
Medium if wiring the SMTP-listener ingress in front of an existing self-hosted app that
expects a real SMTP server.

## Architecture Notes
All three ingress shapes (HTTP form, HTTP API, SMTP listener) converge on one internal
`transport.Message` type before fanning out to one of five outbound transports — a clean
adapter-pattern separation between "how mail arrives" and "how mail leaves" that's a
reasonable reference if building a similar internal gateway from scratch. The README's
explicit "what Posthorn is not" table (not a mail server, not its own outbound
infrastructure, not a marketing platform, not webmail) is unusually disciplined scoping
for a young project and made evaluation faster.

## Maturity
Emerging but polished: created 2026-04-27 (~4.5 months old), latest tagged release
v1.2.0 (2026-07-05), pushed_at 2026-09-10 (active same-day commits), CI badge green, 39
open issues (healthy backlog, not neglect), dedicated docs site at posthorn.dev with ten
written recipes including full case studies (Hugo+Comentario, Ghost, Gitea, self-hosted
Umami digests). Small community — 211 stars, appears to be a single/small-team effort.

## License
Apache-2.0 — no commercial-use, self-hosting, or embedding restrictions identified.

## Alternatives
Listmonk (marketing/newsletter platform, different scope — Posthorn explicitly is not
this), Postal and Hyvor Relay (run their own outbound SMTP infrastructure rather than
relaying through a provider you already pay for — different tradeoff), or just writing
provider-specific integration code per app (what Posthorn replaces). Also surfaced this
run in the same "self-hosted email sending" search wave: reloop-labs/reloop (Apache-2.0,
SES/SendGrid/Mailchimp/Resend/Loops-specific, 62 stars, thinner docs), R44VC0RP/opensend
(created the day before this review, too new to assess), and savvyagents/larasend
(MIT, 494 stars, but Laravel-specific — narrower fit given a MERN/Node stack). None
matched Posthorn's protocol-agnostic ingress design or documentation depth.

## Risks / Limitations
- Small single/small-team project — bus-factor risk if adopted for anything
  production-critical.
- No mailbox/IMAP support and no marketing-list features by design; don't reach for it
  outside its stated scope (see "what it is not" above).
- Self-hosted operators still depend on the upstream provider's deliverability handling —
  Posthorn is purely the integration layer, not a deliverability guarantee.

## Recommendation
PROTOTYPE — deploy it in front of one low-stakes self-hosted app (a contact form or a
cron-digest sender) to validate the HTTP API ingress and retry behavior before
centralizing more mail traffic through it.

## Change History
### 2026-09-10
Initial discovery and review. Slot 2 (product infrastructure, APIs, backend components) run.
