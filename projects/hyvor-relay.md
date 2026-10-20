# Hyvor Relay

## Summary
An open-source, self-hosted transactional email API built by Hyvor (the company behind Hyvor Blogs and Hyvor Talk) — positioned as a self-hostable alternative to AWS SES, Mailgun and SendGrid. Sends via SMTP from your own infrastructure, automates MX/SPF/DKIM DNS setup, and keeps send logs plus SMTP conversation traces for up to 30 days.

## Why I Should Care
Replaces the recurring SaaS/glue-code dependency on AWS SES for any product that needs transactional email, with a self-hosted service that handles DNS/deliverability setup and gives visibility into what was actually sent, rather than digging through CloudWatch logs.

## Problems It Can Remove
Removes the per-product work of wiring up SES (or Mailgun/SendGrid), configuring SPF/DKIM, and building ad hoc send-log visibility — all of which Relay bundles as one self-hosted service with a REST API.

## Practical Uses
- Self-host as the shared transactional-email layer across several of his own products instead of configuring SES separately for each.
- Replace SES in a client project where self-hosting the email layer is preferable to a per-seat SaaS bill.
- Use the send-log/SMTP-conversation inspection UI when debugging deliverability issues.
- Evaluate the commercial/enterprise license path if a product built on it will be offered to customers.

## Product Opportunities
Could be the email layer inside a product he ships — pending a clear decision on AGPL obligations vs. purchasing Hyvor's enterprise license for that specific deployment.

## Agent / Automation Opportunities
Expose as an internal "send notification/transactional email" API or tool for agent-driven workflows and automations.

## Integration
Self-hosted via Docker; REST API plus SMTP. Medium effort — DNS (MX/SPF/DKIM) configuration is required for deliverability, and it's a service to run and operate, not a drop-in library.

## Architecture Notes
API-first design rather than a generic MTA: the DNS automation and built-in send-log/SMTP-trace inspection are the parts that differentiate it from raw Postfix/Postal-style self-hosted mail servers.

## Maturity
Emerging. Created May 2025, still pre-1.0 (latest release v0.0.46, August 2026). 918 stars, 66 forks, 3 watchers, 5 contributors. Hyvor's own blog and Q2 2026 quarterly update describe real self-hosted adopters ("dozens of organizations"), which corroborates the project independently of the GitHub star/watcher numbers alone.

## License
AGPL-3.0 — **flagged loudly**: self-hosting internally is fine, but offering a modified version as a hosted service to third parties triggers AGPL's source-disclosure requirement. Hyvor sells a commercial/enterprise license (reported around €10,000/year) for organizations that need to avoid AGPL terms.

## Alternatives
Postal (older, more MTA-focused, less polished API), Maddy, raw AWS SES/Mailgun/SendGrid, Resend (closed source).

## Risks / Limitations
AGPL-3.0 licensing must be resolved before embedding in anything offered commercially. Pre-1.0 — API and schema could still change. Star:watcher ratio (918:3) is skewed relative to a typical organic repo, though independently corroborated via Hyvor's own public blog posts rather than taken at face value.

## Recommendation
PROTOTYPE — self-host and test against a real product's email needs; resolve the AGPL-vs-enterprise-license question before any commercial embedding.

## Change History
### 2026-10-20
First reviewed and catalogued. Surfaced outside slot 7's core theme but kept per AGENT.md §5 ("an exceptional find outside it is still welcome") given direct fit with the micro-SaaS building-block axis and an existing SES dependency.
