# Bank Statement Parser

## Summary
Parses bank statements across eight structured formats (CAMT, PAIN.001, MT940, MT942,
OFX/QFX, BAI2, CSV) plus digital and scanned PDFs into one unified, auditable Transaction
model. Includes treasury analytics, two-way reconciliation, PDF forensics, Parquet/ledger
(hledger, beancount) exports, deduplication, and an optional LLM/vision fallback for the
hard cases, with a CLI and REST API.

## Why I Should Care
Normalizing bank statements across formats and reconciling them against internal records
is unglamorous, multi-week integration work that any billing- or finance-adjacent feature
eventually needs. This has already done it, maintained since 2023 with real test
coverage.

## Problems It Can Remove
Removes the need to hand-write a parser per bank/format when building a reconciliation or
treasury-reporting feature. Removes the brittleness of ad hoc CSV/PDF parsing that breaks
every time a bank changes its export format.

## Practical Uses
- Normalize bank statements from multiple banks/formats into one Transaction schema for a
  reconciliation or treasury-reporting feature.
- Reconcile bank-reported transactions two-way against internal ledger records without
  hand-writing a parser per statement format.
- Export parsed transactions directly into hledger/beancount for plain-text accounting, or
  to Parquet for downstream analytics.
- Fall back to LLM/vision extraction for scanned statements that defeat the structured
  parsers.

## Product Opportunities
Ingestion layer for a billing/admin-panel feature that needs to reconcile bank statements
against invoices without building per-bank parsers from scratch.

## Agent / Automation Opportunities
REST API makes it callable as a service from an agent or automation; no MCP server.

## Integration
Deployment: `pip install` (PyPI), CLI, embeddable Python library, or REST API.
Integration effort: **Low** for a CLI/REST sidecar; a Node/TS backend would call it
rather than import it natively since it's Python-only.

## Architecture Notes
Unified Transaction model across eight structured banking formats plus a scanned-PDF/LLM
fallback path — worth noting as a reference for how to design one canonical schema that
absorbs multiple legacy format quirks rather than branching logic per format.

## Maturity
Mature for its niche. Created 2023-11-08 (~3 years), 64 stars, 14 forks, 2 subscribers.
Codecov and CI configured, PyPI package with download tracking, latest release v0.0.29
(2026-10-25). Not archived.

## License
Dual Apache-2.0/MIT, confirmed by reading LICENSE, LICENSE-APACHE and LICENSE-MIT directly
after the GitHub API reported NOASSERTION. No restrictions.

## Alternatives
- Hand-rolled per-bank CSV/PDF parsers — brittle, format coverage grows one bank at a
  time.
- Plaid or similar bank-aggregator APIs — live data access but a paid SaaS dependency and
  not a fit for parsing already-exported statement files.

## Risks / Limitations
- 64 stars / 2 subscribers — small community for a 3-year-old project; verify format
  coverage against the specific banks/formats actually needed before relying on it.
- Python-only; a Node/TS backend would call it as a CLI/REST sidecar rather than import it
  directly.

## Recommendation
PROTOTYPE — run it against a real sample of statements from the specific banks/formats in
use before relying on it for a reconciliation feature.

## Change History
### 2026-11-14
Initial discovery and review (today's Small but High-Leverage Utility). GitHub API
reported NOASSERTION; confirmed dual Apache-2.0/MIT by reading the LICENSE files directly.
64 stars, 14 forks, 2 subscribers, created 2023-11-08, pushed_at 2026-11-14, not archived.
