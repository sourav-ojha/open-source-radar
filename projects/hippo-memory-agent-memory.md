# Hippo (hippo-memory)

## Summary
Local memory for coding agents (Claude Code, Codex, Cursor, OpenClaw, OpenCode, Pi, any
MCP client) stored in SQLite with git-trackable markdown mirrors. BM25 search by default
with zero network calls and zero runtime dependencies; the differentiator is an explicit
"this was wrong" mechanic — marking a memory wrong ranks it down, and `hippo supersede`
retires an old fact so agents stop repeating fixed mistakes instead of accumulating a
pile of memories of equal weight.

## Why I Should Care
Every existing agent-memory tool in the catalog ([[mex]], LightAgent, Commonly, Edda,
LightMem) focuses on capturing and retrieving facts; none of them model "this turned out
to be wrong" as a first-class signal. For long-running agent-assisted work on the same
codebase (this radar repo included), the failure mode isn't lack of memory — it's stale
memory being retrieved with the same confidence as current facts. Hippo is the first
entry that addresses that directly.

## Problems It Can Remove
Re-explaining the same correction to a coding agent across sessions; an agent
re-proposing an approach that was already tried and rejected; memory tools that degrade
into noise because nothing ever gets deprioritized.

## Practical Uses
- Run `hippo init` in any actively agent-worked repo (including this one) to give Claude
  Code/Codex a persistent, decaying memory of prior decisions and known-wrong approaches.
- `hippo doctor` for agent-driven self-installation and verification — an AI agent can
  set this up unattended by following `llms-install.md`.
- Markdown mirrors make the memory store diffable and reviewable in normal git workflow,
  unlike most memory tools that hide state in an opaque database.

## Product Opportunities
The "mark wrong → rank down → supersede" pattern is a reusable idea for any product
storing AI-agent or user-curated knowledge that decays in accuracy over time (support
macros, internal runbooks, onboarding docs maintained partly by agents).

## Agent / Automation Opportunities
Works as an MCP server (any MCP client can connect) and as native hooks for Claude Code
and OpenCode; adds Codex hook entries and `AGENTS.md`-style instructions for Cursor,
OpenClaw, and Pi. Broadest agent-compatibility surface of anything reviewed this run.

## Integration
`npm install -g hippo-memory && hippo init`. Zero runtime dependencies for the default
BM25 path — fully self-hostable, no network call required. Embeddings (via
Transformers.js locally, or an opt-in OpenAI/Voyage/Cohere API) and an opt-in hosted
"Jev" reranker are both explicitly optional add-ons, not required to use the tool. Low
integration effort.

## Architecture Notes
SQLite as the source of truth with markdown as a human/git-readable mirror is a sensible
split — it gets the durability and queryability of a real datastore without sacrificing
git-based review of what the agent "remembers." The same "Jev" judge-model family shows
up here too (as an optional reranker only), reinforcing that it's becoming a shared
utility across this generation of agent tools rather than tied to one product.

## Maturity
Emerging. Created 2026-03-15 (~6.5 months old), actively maintained (pushed same day as
this review), npm package at 1.52.9 matching a same-day GitHub release tag. Small
contributor list (3, one of which is a "claude" commit author — the project appears to be
partly built with Claude Code itself, disclosed rather than hidden). 766 stars / 44
forks is modest but plausible for the project's age, unlike some same-day star spikes
seen elsewhere this run.

## License
MIT, zero runtime deps by default — the most self-hostable of this run's finds. Optional
paid reranker is opt-in and clearly separated from the core product.

## Alternatives
[[mex]] (code search/documentation-focused, not memory-with-decay), LightAgent, Commonly,
Edda — all accumulate memory without a wrongness signal. Hippo's "supersede" mechanic is
the clear point of difference.

## Risks / Limitations
Small team (effectively one primary maintainer plus AI-assisted commits); the "claude" co-
author pattern is transparent but means less independent human review than a typical
multi-maintainer project. BM25-only search may miss semantically-related-but-lexically-
different memories unless the optional embeddings path is enabled.

## Recommendation
PROTOTYPE — the zero-dependency default and broad agent compatibility make it low-risk to
try on a real, actively agent-worked repo; the decay/supersede mechanic is worth
validating over a few weeks of real use before relying on it for anything important.

## Change History
### 2026-09-30
Initial discovery and review.
