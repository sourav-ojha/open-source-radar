# textbee

## Summary
An open-source SMS gateway that turns an Android phone (with a SIM card) into a REST/webhook SMS API. The stack is NestJS (API), Next.js (web dashboard), and Kotlin (Android app), all in one repo, plus official JS and Python SDKs and an MCP server. Positioned explicitly as a Twilio alternative: no rented number, no per-message fee, cost is whatever the carrier SIM plan already costs.

## Why I Should Care
This directly removes a recurring SaaS cost and integration headache — Twilio (or similar) account setup, number registration/compliance, and per-message billing — for anything that only needs OTP codes, alerts, reminders, or two-way SMS at low-to-moderate volume. The backend is NestJS, which is already in my stack, so the API/webhook layer is easy to read and extend if needed rather than being a black box.

## Problems It Can Remove
- Twilio/Vonage/MessageBird account setup, number rental, and per-SMS billing for OTP/alert/notification use cases.
- Building a custom SMS-sending microservice from scratch for a side project or client product.
- SMS vendor lock-in — self-hosted means the sending number is a real local SIM, not a number that gets flagged as unknown/spam by carriers.

## Practical Uses
- OTP/2FA codes and transactional alerts for a side project or an early-stage product, before volume justifies a paid SMS vendor.
- Two-way SMS support flows (e.g. appointment confirmations, delivery notifications) wired through the REST API and webhooks.
- n8n/Zapier/Make automation step for SMS notifications, using the documented integration guides.
- A cheap SMS channel for an MSSP/PTaaS client-notification flow where per-message vendor billing isn't justified.
- Bulk SMS via CSV upload for low-volume broadcast use cases (announcements, reminders), paced to respect carrier limits.

## Product Opportunities
- Could be bundled as the SMS leg of a notifications micro-SaaS (alongside email/push) without taking on a Twilio bill.
- The official MCP server makes "send/read SMS" directly callable by a coding agent — useful for an agent-driven ops/alerting tool.

## Agent / Automation Opportunities
Official MCP server (`textbee/textbee-mcp`) exposes send/receive SMS as agent tools for Claude, Cursor, etc. JS and Python SDKs (`@textbee/sdk`, PyPI `textbee`) make it a one-line dependency from Node or Python automation scripts.

## Integration
Self-host via Docker Compose (API + web dashboard), or use the free hosted plan at textbee.dev. Requires a dedicated Android phone (7.0+) running the companion APK (distributed outside the Play Store) to act as the SIM gateway — this is the main physical-infra dependency, not a pure-software install. REST API, webhooks (HMAC-SHA256 signed), npm/PyPI SDKs. Integration effort: **Low** for the API/SDK side, **Medium** overall once the phone-as-modem requirement is counted.

## Architecture Notes
Three components in one repo: NestJS REST API, Next.js dashboard, Kotlin Android app that holds the actual SIM/SMS permissions and talks to the API as a registered "device." Delivery status is tracked per message (`pending → dispatched → sent → delivered → failed`), and incoming SMS is relayed back through the API/dashboard/webhooks. Worth studying as a reference pattern for "turn a commodity device into a reliable API-backed sensor/actuator" even outside the SMS use case.

## Maturity
Mature for this niche: created March 2022, active CI across all three subprojects, latest tagged release v2.9.0 (2026-09-21), 3,295 stars / 504 forks / 56 open issues, maintained by a small studio (Nuver Labs). README claims 40,000+ developers/businesses using it — unverified beyond the README's own claim, but the multi-year commit history and multi-SDK ecosystem corroborate real usage.

## License
AGPL-3.0. **Flagged per AGENT.md §14**: running it unmodified as an internal service you call over your own network is low-risk, but modifying the code and offering the modified version to third parties over a network would trigger AGPL's source-disclosure requirement. Fine for internal/product-backend use; revisit before white-labeling a modified fork.

## Alternatives
- Twilio, Vonage, MessageBird, Plivo — the mainstream paid SMS APIs this directly targets; all charge per-message/per-number and require compliance onboarding.
- Other "Android SIM as SMS gateway" projects exist but with far less SDK/MCP/ecosystem maturity than textbee's multi-year, multi-platform build.

## Risks / Limitations
- Requires a dedicated Android phone online 24/7 as the gateway — a physical-infra dependency most pure-SaaS alternatives don't have.
- AGPL-3.0 — flagged above; not a blocker for internal use, but relevant if ever embedded in a redistributed/white-labeled product.
- Star:subscriber ratio (3,295:26 ≈ 127:1) is elevated versus a "typical" organic repo, but paired with real fork count (504), multi-year commit history, and a working CI/SDK ecosystem — read as healthy adoption, not inflation.
- Carrier-side SMS limits (rate limits, carrier filtering of bulk traffic) still apply; this removes the vendor bill, not carrier policy.

## Recommendation
**USE NOW** — for any OTP/alert/low-volume two-way SMS need where a spare Android phone is an acceptable tradeoff for killing a recurring per-message SaaS bill. Low integration effort on the software side; the phone-as-gateway requirement is the only real adoption friction.

## Change History
### 2026-11-08
Initial discovery and review.
