# Midplane

## Summary
Midplane is a self-hostable gateway that sits between AI agents (over MCP) and a Postgres database. Every statement is parsed with Postgres' own parser, checked against a per-table policy (deny-by-default, with masking of sensitive values applied before joins/filters/aggregates see raw data), recorded in a hash-chained local audit log, and only then executed. Writes can require approval bound to the exact statement and the row count the approver saw. An optional Cloud tier holds policy/approvals/query-log but never sees credentials or rows.

## Why I Should Care
Giving a coding or ops agent direct Postgres access is normally all-or-nothing — a connection string an agent either has or doesn't. Midplane's parse-then-policy-then-audit pipeline is governance a team would otherwise have to hand-build before trusting an agent with a real database, and it's directly applicable both to Postgres-backed products and to the MSSP/PTaaS security context (tamper-evident audit trail of agent-issued statements).

## Problems It Can Remove
Hand-building a SQL-statement-aware policy/audit layer (parse, check per-table access, mask sensitive columns before aggregation, log immutably, gate writes on approval) before letting an agent touch production-adjacent data.

## Practical Uses
- Give a coding or ops agent read/write access to Postgres without handing it a raw credential — the gateway opens the DB connection itself
- Mask PII/sensitive columns before an agent's query results include them, even for ad-hoc exploratory queries
- Require human approval for agent-issued writes, bound to the exact statement and row count reviewed
- Keep a tamper-evident audit trail of every statement an agent attempted against sensitive data
- Enforce guardrails a raw DB credential can't express: one statement at a time, writes require a `WHERE`, no writes inside a `WITH`, read-only transactions with timeouts

## Product Opportunities
The gateway pattern (parse, policy-check, mask, audit, then execute) could underpin an "agent-safe database access" feature pitched to the MSSP admin-portal SaaS's enterprise customers. The hash-chained audit log format is a reusable building block for any product needing a tamper-evident record of agent actions, independent of the database use case.

## Agent / Automation Opportunities
MCP-native by design — built specifically for agent tool calls, not retrofitted from a human-facing DB client. The taint/containment rule (write access closes once an agent has read a column labeled untrusted/secret) is a concrete pattern worth studying for any agent given broad data access.

## Integration
Docker Compose quickstart (`examples/quickstart`, `docker compose up -d --wait`; needs Node 24.16+, Docker, an MCP client). Run standalone with `npx midplane` or the `ghcr.io/midplaneai/midplane` image. Works in local mode with no account (but no approval workflow — held writes are simply refused) or with the invite-only Midplane Cloud tier for the approval dashboard and query log. Integration effort: **Medium** — the quickstart is simple, but adopting the full policy/approval workflow requires Cloud access and reading external docs, since the policy language isn't documented in the repo itself.

## Architecture Notes
The gateway runs in your own network next to the database and opens every DB connection itself — agents never see a credential. Statements are parsed with Postgres' native parser before execution, so policy decisions and masking happen on the actual parsed statement rather than string matching. Masking (hashes, partial values, generalized dates, NULLs) is applied before filters, joins, or aggregates see the raw value, which is a meaningfully different guarantee than row-level security applied after the fact. The audit log is hash-chained and kept on the gateway's own disk, independently exportable and verifiable.

## Maturity
Experimental. GitHub API confirmed: MIT, 5 stars, 1 fork, 0 subscribers, 2 open issues, created 2026-04-29, pushed 2026-10-28. Positives: CI badge, npm and Docker distribution with build provenance and signed images, a documented SECURITY.md, and a runnable quickstart. Caveats: the README is largely promotional and makes unaudited security claims (e.g. "a mask or policy can't be bypassed") with no independent benchmark or review cited; the full policy language lives only in external docs; local mode (no account) has no approval path for writes.

## License
MIT, confirmed via GitHub API. No restrictions on commercial or embedded use. The optional Cloud tier is invite-only and its pricing/terms are undocumented in the repo.

## Alternatives
Hand-rolled read-only DB users plus Postgres row-level security (native but no per-statement audit trail or pre-aggregation masking); generic API gateways/proxies (not statement-aware — can't enforce "writes need a WHERE" or mask before a JOIN).

## Risks / Limitations
- Very early: 5 stars, 1 fork, 0 subscribers — minimal independent validation
- Security claims are unverified by any audit, benchmark, or third-party review in the repo
- Policy language and full configuration are only documented externally, not assessable from the repo alone
- Cloud tier (needed for the approval workflow) is invite-only
- No documented answer for what happens if a statement type is missed by the parser — a real gap in a security-critical control

## Recommendation
PROTOTYPE — the local-mode Docker Compose quickstart is genuinely runnable today and worth testing against a non-production Postgres instance to evaluate the policy/masking behavior directly, but do not rely on the unaudited security claims without independent verification, and treat Cloud-tier approval workflows as unavailable until invited.

## Change History
### 2026-10-28
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
