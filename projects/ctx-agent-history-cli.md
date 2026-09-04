# ctx

## Summary
A local-first CLI that indexes past coding-agent session transcripts (Claude Code, Codex, and others) from files already on disk (`~/.claude`, `~/.codex`) so agents can search prior decisions, failed approaches, and tool calls instead of repeating investigation from scratch. A separate paid "ctx pro" companion adds "git blame, but for agent sessions": trace any line, file, commit, or PR back to the exact agent session that produced it.

## Why I Should Care
Coding-agent history (this very kind of session) piles up in verbose JSONL logs that are effectively unusable for an agent to search directly — the project's own benchmark claims raw transcript search costs ~50x the tokens of `ctx search` for the same recall. Given heavy daily coding-agent use, this directly cuts repeated investigation and token spend across sessions.

## Problems It Can Remove
- Re-explaining or re-discovering decisions, constraints, and failed approaches already worked out in a previous agent session.
- Manually grepping raw `~/.claude` JSONL logs, which is both slow and token-expensive if handed to an agent.
- Losing context when picking a stale branch back up — `ctx blame` traces a line/commit/PR back to the session that produced it.

## Practical Uses
- Recover why code exists when picking up a stale branch, from the actual transcript rather than guessing from the diff.
- Feed a fresh coding-agent session the real prior investigation instead of having it re-discover the same dead ends.
- Search across parent sessions, subagents, and forks when heavily orchestrating multi-agent runs.
- Audit what an agent actually did across weeks of history.

## Product Opportunities
The open-core structure (free local index + paid hosted-grade blame layer) is a usable template for productizing an internal agent-ops tool.

## Agent / Automation Opportunities
Built specifically for agent-to-agent handoff: a fresh session can call `ctx search`/`ctx blame` itself to recover context with far fewer tokens than re-reading raw transcripts. No hooks or in-process instrumentation required — it reads existing log sources directly.

## Integration
Single install script (`curl -fsSL https://ctx.rs/install | sh` or PowerShell equivalent). `ctx setup` indexes existing local history automatically; no server, account, or config required for the free CLI. Low effort.

## Architecture Notes
Converts each provider's transcript format into consistent local records (sessions, messages, tool calls, relationships) stored and indexed locally — no vector database required even for semantic search, which computes and searches embeddings locally. Understands parent/subagent/fork relationships so it can reconstruct a whole chain of orchestrated work.

## Maturity
Emerging. 1,062 stars, pushed same-day, created February 2026, 19 contributors, 3,500+ commits — unusually high velocity for a 7-month-old project, consistent with heavily agent-assisted development seen across this cohort. Versioned releases (v1.3.1) and working CI.

## License
Apache-2.0 for the core CLI (search/show/locate/semantic search) — no restrictions on commercial use. **"ctx pro" is a separate, closed-source, signed proprietary companion** ($20/mo, 2-week free trial) that gates the actual git-blame-for-agent-sessions feature. Flagged explicitly: the free tier does not include blame.

## Alternatives
grep/ripgrep across raw `~/.claude` JSONL logs (status quo — the project's own benchmark claims ~50x more tokens for equivalent recall); ad hoc scripts to dump and search transcript files.

## Risks / Limitations
- The headline "blame" feature is paid and closed-source; only search/recall is free and open.
- Young project with very high commit velocity — worth a stability check before deep reliance.
- Adds a local indexing daemon/SQLite store to manage, even though it's opt-in and fully local.

## Recommendation
USE NOW — the free core CLI is a trivial, zero-risk install (fully local, no account) that directly addresses a real daily pain point of heavy coding-agent use. Evaluate "ctx pro" separately once the free tier proves useful, given it's a paid closed-source add-on.

## Change History
### 2026-09-04
Discovered and reviewed. GitHub API verified: Apache-2.0, 1,062 stars, pushed_at 2026-09-04 (same day), created 2026-02-23, 19 contributors. README documents the open-core split explicitly in `docs/managed-companion.md`.
