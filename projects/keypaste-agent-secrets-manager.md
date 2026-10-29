# keypaste

## Summary
A KeePass-compatible (KDBX 4) local secrets/password manager purpose-built for the moment an AI coding agent needs a credential. It stores logins and project env vars in one encrypted vault file with no account and no network, and gates every agent request through an interactive approval prompt (Deny / Allow once / Allow for 1 hour / Always-for-project) that is recorded in a hash-chained audit log. Ships as a CLI/MCP server today; a desktop app (KeePassXC-style UI) exists in source but has no public release yet.

## Why I Should Care
The "give the agent a `.env` file" default is exactly the kind of blanket-trust shortcut that causes problems the moment an agent is compromised, misreads a prompt-injected instruction, or just makes a mistake with a prod credential. keypaste's approval-per-field model with reason-logging (the agent has to state *why* it wants a secret, and that statement is explicitly treated as an unverified claim, not a fact) is a clean, low-effort pattern to adopt across his own coding-agent workflows without giving up KeePassXC compatibility or buying into a cloud vault.

## Problems It Can Remove
Ad hoc `.env` files sitting fully readable on disk for any process (including an agent) to read; manually rotating and tracking which secret an agent session touched; building an approval/audit layer around secret access by hand.

## Practical Uses
- `.env.keypaste` references (`KEY=kp://project/profile/KEY`) committed to a repo instead of real values, with the actual secret injected only at run time via `keypaste run`.
- Per-project secrets split across dev/staging/prod profiles, with prod always requiring a live human answer rather than a standing allowance.
- Scoped, expiring CI tokens (e.g. `read:acme-api/staging/*`) for inject-only automation contexts.
- An MCP approval gate in front of Claude Code or another coding agent for any credential access during a session.
- One-time or short-lived encrypted share links for handing a single credential to a teammate without a shared vault.

## Product Opportunities
Limited directly — this is a personal/dev-workflow tool rather than embeddable product infrastructure. The approval-prompt-plus-audit-log pattern, though, is worth copying into any internal tool that gives an agent access to credentials.

## Agent / Automation Opportunities
This is itself an agent-safety tool: MCP-native approval flow, with every grant and denial hash-chain logged. Directly applicable to any Claude Code / MCP-based coding-agent setup that currently has unrestricted `.env` access.

## Integration
CLI/MCP server (`v0.3.0`, published), single KDBX4 file on disk, cross-checked against `keepassxc-cli` in CI on Linux/macOS/Windows for compatibility. No server, no account. The desktop app is source-only (no release) as of this review.

## Architecture Notes
File format compatibility with KeePassXC is verified by CI running both tools against the same vault, not just claimed. The README is unusually candid about its own defect history across versions — it documents, per release, what the *previous* version could do wrong (e.g. v0.1.0 could delete a vault via `env export` or return the wrong password via `get`), and keeps a numbered threat model (`THREATS.md`, e.g. "T-35: a `run` command can still leak a value via a command the agent can edit, even though keypaste itself keeps the value out of the result"). That level of self-disclosed limitation is a stronger maturity signal than typical marketing copy.

## Maturity
Experimental, pre-1.0. Created 2026-09-05, latest published CLI is v0.3.0. 598 stars but only 8 forks and 4 subscribers — an unusually high star-to-fork ratio for this review to take entirely at face value, in a week when several near-identical "secrets manager for AI agents" repos surfaced (sallyport, OpenKeylet, vahta, jitpass — see Run Notes). Weighed against that: the README's unusual honesty about historical defects and an explicit per-feature threat model are not typical of engagement-farmed repos, which argues for treating it as a real if early project rather than dismissing it outright.

## License
AGPL-3.0. For personal/internal use this is unrestricted; embedding keypaste's code into a distributed or hosted product would trigger AGPL's source-disclosure obligation.

## Alternatives
sallyport, OpenKeylet, vahta, jitpass (same-week wave of "secrets for AI agents" tools, all thinner on documentation and far smaller); 1Password/Bitwarden CLI integrations (no agent-approval workflow); a hand-rolled `.env` + manual review process (what this replaces).

## Risks / Limitations
Star/fork/issue ratio (598 / 8 / 0) is an anomaly this review cannot fully explain and should be rechecked in a future run before leaning on it more heavily. Pre-1.0, young, AGPL. The desktop app and share-link CLI command are not yet publicly released, so the full feature set described in the README is partly aspirational as of this review.

## Recommendation
PROTOTYPE, with the engagement-ratio caveat explicitly tracked. Worth trying as a drop-in credential gate for an existing coding-agent workflow; not yet something to depend on for anything irreplaceable given its age and the unresolved star/fork anomaly.

## Change History
### 2026-10-29
Initial discovery and review. Slot 2 run (product infrastructure, APIs, backend components). Found via GitHub Search API `topic:secrets-management created:>2026-06-01`, alongside a same-week wave of similar "secrets for AI agents" tools.
