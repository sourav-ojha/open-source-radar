# Beancount.io (beancount-io)

## Summary
A polished product layer on top of plain-text double-entry accounting (Beancount + Fava under the hood): native iOS/Android apps, a one-install CLI (`bea`), web reports, and AI-agent/coding-agent "skills" for importing statements and closing the books — all operating on the same `.bean` plain-text ledger file, with self-hosting or a hosted option.

## Why I Should Care
Personal/freelance bookkeeping usually means either a spreadsheet (no real reporting) or a SaaS finance app (data lock-in, recurring cost). This keeps the data as a Git-diffable plain-text file — genuinely yours — while adding the UX pieces (mobile app, polished reports, agent skills) that plain Beancount + Fava never had.

## Problems It Can Remove
- Spreadsheet-based personal/freelance bookkeeping with no real reporting (income statement, balance sheet, treemap expense breakdowns).
- SaaS finance-app lock-in where the ledger lives in someone else's database/export format.
- Manually reconciling bank statements and categorizing transactions — ships Claude Code/Codex-pointed "skills" for exactly that, with every write confirm-gated.

## Practical Uses
- Personal or freelance double-entry bookkeeping, synced across phone/terminal/web, with the ledger stored in a Git repo.
- Pointing a coding agent at the `cli/docs` skills to import a bank statement and reconcile it automatically, with confirmation gates on every write.
- Scripting custom financial reports against the ledger from Python, instead of exporting CSVs from a SaaS app.

## Product Opportunities
Narrow personal-productivity tool rather than an embeddable component — the value here is direct use, not a building block for another product.

## Agent / Automation Opportunities
Ships explicit "skills" for Claude Code/Codex to import statements, reconcile accounts, and close the month on the plain-text ledger, with every agent write confirm-gated before it touches the `.bean` file — a concrete, conservative pattern for letting an agent operate on financial data.

## Integration
Self-host the full stack with `deploy/docker/` (single server) or `deploy/docker-mac/` (local), or use the hosted service, or skip hosting entirely and run the CLI (`bea`) locally against a `.bean` file. Native iOS/Android apps and a Fava-compatible web UI point at the same ledger. Integration effort: **Low** — `brew install` or `uv tool install` for the CLI; Docker Compose for the self-hosted web/mobile-sync stack.

## Architecture Notes
Builds directly on the existing Beancount (ledger format/query engine) and Fava (web reports) open-source projects rather than reinventing plain-text accounting — the value-add is the mobile apps, one-install CLI, hosted/self-hosted collaboration layer, and agent skills layered on top, while the underlying `.bean` files stay standard enough for Fava or other Beancount tooling to read directly.

## Maturity
Established: underlying repo created October 2020, 328 stars, 39 forks, 5 subscribers, 1 open issue, most recent (mobile app) release 2026-10-02. MIT-licensed.

## License
MIT — no restrictions on commercial use or self-hosting.

## Alternatives
- Plain Beancount + Fava — the direct open-source foundation this builds on; web-only, no native mobile apps or one-install CLI.
- Spreadsheets or a SaaS personal-finance app (YNAB, Mint-successors) — the lock-in/no-reporting alternative this avoids.

## Risks / Limitations
- Primary README framing leans toward the hosted beancount.io product (sign-up flow is the first call to action); self-hosting is real and documented but secondary in the pitch.
- Niche fit: a personal/freelance bookkeeping tool, not a reusable infrastructure component — value is direct-use only.
- Small subscriber count (5) and modest star count (328) relative to the polish on display — worth confirming real-world reliability before trusting it with primary financial records.

## Recommendation
**USE NOW** — today's Small but High-Leverage Utility. Low-effort to trial via the CLI alone (no account, no server) before deciding whether to self-host the full stack.

## Change History
### 2026-11-08
Initial discovery and review.
