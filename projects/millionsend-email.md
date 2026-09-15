# MillionSend

## Summary
A self-hosted transactional + marketing email platform with a Resend-compatible API — the pitch is that migrating from Resend means changing two environment variables, not rewriting integration code. Sends through your own AWS SES account (or their hosted cloud). Ships a dashboard, contacts/segments/broadcasts, templates with merge fields, webhooks (Standard Webhooks spec), an SMTP relay, and a CLI (`@millionsend/cli migrate --from resend`) that copies contacts, templates, webhooks, domains and suppressions out of an existing Resend account.

## Why I Should Care
Sourav's stack is already AWS-centric (SES is a natural fit), and Resend is exactly the kind of paid SaaS this radar exists to flag for replacement. A drop-in-compatible API means any project already coded against Resend's SDK can point at a self-hosted MillionSend instance with no integration rewrite — this removes the usual "self-hosted alternative isn't API-compatible" friction that kills adoption of most open-source SaaS replacements.

## Problems It Can Remove
- Resend subscription cost for projects that already have AWS SES access.
- Building suppression-list handling, bounce/complaint tracking against SES thresholds, and webhook signing from scratch for a side project or micro-SaaS.
- Vendor lock-in risk if a client project is built against Resend's API today.

## Practical Uses
- Self-host on an existing AWS account for a client project's transactional email (signup confirmations, password resets) without paying for Resend.
- Point an already-Resend-integrated app at a self-hosted instance for a data-residency or cost reason via the two-env-var swap.
- Use the SMTP relay (port 2587) for legacy code that only speaks SMTP, while still getting the dashboard and suppression handling.

## Product Opportunities
- Could be the email layer inside an MSSP admin portal or any multi-tenant SaaS that needs to send from a customer's own AWS account rather than a shared sending domain.

## Agent / Automation Opportunities
- REST API is straightforward to wrap as an MCP tool or agent capability for "send this email" workflows.
- The `migrate --from resend` CLI itself is a reusable pattern for any "compatible API, one-command migration" tool.

## Integration
Self-host via `npx @millionsend/setup` (interactive wizard, provisions IAM user/SNS topic/SES config set, runs `docker compose up -d`); dashboard on :3000, API on :3001. Effort: **Low** — the setup wizard does the AWS wiring that would otherwise be manual.

## Architecture Notes
Core is AGPL-3.0; official SDKs are separately MIT-licensed. Standard Webhooks spec compliance (svix-* headers) rather than a bespoke signing scheme is a good sign of not reinventing solved problems.

## Maturity
Emerging — created 2026-08-13, no tagged GitHub release yet (ships via npm package version instead), but the feature table in the README covers a genuinely complete transactional-email feature set (idempotency, suppression, webhooks, BYODKIM, broadcasts).

## License
**AGPL-3.0 for the core** — flagged per policy: if this were exposed as a hosted service to third parties (e.g. embedded inside a paid product other people use over a network), AGPL's network-copyleft clause would require offering source to those users. Fine for internal/self-use; needs legal attention before white-labeling as part of a commercial SaaS. SDKs are MIT.

## Alternatives
Resend (the paid service this replaces), Postmark, catalogued Posthorn (provider-agnostic self-hosted email gateway, cataloged 2026-09-10) — Posthorn is a lower-level generic ingress/gateway; MillionSend differentiates by being a full Resend-API-compatible platform with contacts/broadcasts/templates built in, aimed at drop-in migration rather than generic routing.

## Risks / Limitations
- AGPL-3.0 licensing needs review before any commercial/hosted embedding.
- Young project (1 month old at review), no tagged release — verify stability before production reliance.
- Ties email deliverability to AWS SES sending limits/reputation.

## Recommendation
PROTOTYPE — worth standing up against a sandbox AWS account to validate the Resend-compatibility claim before considering it for a client project.

## Change History
### 2026-09-15
Initial discovery and review.
