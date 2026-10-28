# MisakaNet

## Summary
MisakaNet is a git-backed, community-curated corpus of "evidence-graded" failure/fix lessons for coding agents, served over an MCP server (plus WebMCP, A2A discovery, and `llms.txt`). When an agent hits an error, it searches the corpus (BM25, with an optional semantic fallback) instead of re-debugging a failure someone else already solved; a miss files an intake that can become a new lesson after a PR-style review.

## Why I Should Care
Most "agent memory" tools store one agent's own session history. MisakaNet is closer to a crowdsourced advisory database — a shared, versioned, reviewed knowledge base any agent can query cold, regardless of who hit the error first.

## Problems It Can Remove
Repeated 15-30 minute debugging detours for errors that are common enough that someone else already solved them and wrote it down — the kind of thing normally re-solved from scratch every time because there's no shared place to look it up.

## Practical Uses
- Query before deep-diving a cryptic error an agent hits mid-task, to check if it's a known, already-fixed failure
- Wire into a coding agent's error-handling loop via MCP so lookup happens automatically on failure, not only when remembered
- Use the GitHub Action to check CI failures against the corpus automatically
- Contribute an intake when the corpus misses, building a reusable fix log instead of re-solving the same CI flake repeatedly
- Pull the published, hash-verified dataset (data.misakanet.org) to self-host a private mirror or pre-seed a team-internal variant

## Product Opportunities
The same pattern — evidence-graded, crowdsourced fix corpus with a review-via-PR curation workflow — could be run privately for a team's own recurring incidents or CI flakes as a small internal tool, or sit in front of a support/helpdesk product as a "known fix" lookup layer before escalating to a human.

## Agent / Automation Opportunities
Purpose-built MCP tool (8 tools total, 2 documented: `misakanet_search`, `misakanet_submit_intake`) for coding agents to consult mid-task. A2A discovery and WebMCP mean non-MCP agent stacks can still reach it. The GitHub Action lets CI itself consult the corpus on failure, not just an interactive agent session.

## Integration
Multiple channels: hosted MCP endpoint at misakanet.org (no install needed to query), a local stdio MCP server (`misakanet`), a PyPI library (`misakanet-core`) for embedding retrieval directly, an `npx` installer, a Claude Code plugin marketplace entry, and a GitHub Action. Integration effort: **Low** — querying the hosted endpoint requires no install at all.

## Architecture Notes
Retrieval is BM25 over the Python standard library by default — no vector database required locally. The hosted endpoint adds an optional semantic fallback (bge-m3 + Vectorize) that only runs when BM25 finds nothing, togglable via `MISAKANET_SEMANTIC=0`. Lessons carry a trust tier: E0 (intake) → E1 (reviewed) → E2 (reproduced) → E3 (CI-verified) → E4 (reuse observed by 2+ distinct clients); E4 is currently empty. Contributors submit via MCP or a GitHub issue; lessons are reviewed through pull requests like code, cite their source intake, and the full corpus is published with per-file SHA-256 hashes and licenses.

## Maturity
Emerging. GitHub API confirmed: Apache-2.0, 543 stars, 227 forks, 27 subscribers, created 2026-04-29, pushed 2026-10-28 (same day as this review — actively maintained). Published packages on PyPI and npm, CI badges, a versioned dataset. Self-reported retrieval benchmark: on 267 queries, the semantic fallback raised R@1 from 0.210 to 0.446 and cut `no_match` from 0.712 to 0.453 — still below the project's own stated 0.46 target. 303 open issues against 543 stars looks unusual but appears to be a side effect of the design (every intake becomes an issue in the curation workflow), not necessarily a quality problem — worth re-checking as the project scales.

## License
Apache-2.0, confirmed via GitHub API. Contributed lessons are licensed the same way with a required DCO sign-off. No restriction on commercial or internal use of the server or corpus.

## Alternatives
Personal/private agent-memory tools already in the catalog (MemPalace, Hippo, deja-vu) — those store an individual's own session history; MisakaNet is a shared, public, reviewed corpus, closer to Stack Overflow for agent failures than to personal memory.

## Risks / Limitations
- Corpus size and real coverage for a Node/Postgres/AWS stack are unstated in the README; retrieval benchmark is modest and self-reported
- E4 (highest trust tier) is empty — nothing yet has cross-client validation
- Retrieved fix commands can be executed by an agent; the project explicitly warns to sandbox agents first since lessons are community-contributed
- Some issues carry third-party bounty banners the project says it does not administer — verify any such offer independently
- High open-issue count relative to stars warrants a second look once the project is larger

## Recommendation
PROTOTYPE — low-effort to try (the hosted MCP endpoint needs no install), and the downside is bounded since the agent still executes any suggested fix under its own guardrails. Worth wiring into a coding-agent session to see how often it actually surfaces something useful for this profile's stack before trusting it further.

## Change History
### 2026-10-28
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
