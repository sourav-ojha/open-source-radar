# Dao Code

## Summary
Dao Code is an open-source TypeScript terminal coding agent built specifically around
DeepSeek V4's cache pricing. Instead of optimizing at a gateway/proxy layer, it engineers
byte-stable prompt prefixes and cache-reusing session forks directly into the agent's own
prompt construction, so cross-session memory and a continuous self-correction layer add
almost no token cost. 1M context, Skills/MCP/Hooks, Claude Code config compatible.

## Why I Should Care
Most coding-agent cost-reduction tools (gateways, routers, proxy caches) sit between the
agent and the model. Dao Code takes a different and more durable approach: it designs the
agent's own prompt layout to maximize prompt-cache hits on DeepSeek's pricing. That pattern
— byte-stable prefixes plus cache-reusing forks — is directly transferable to any in-house
agent harness that repeatedly re-sends similar context, regardless of which model backs it.

## Problems It Can Remove
Removes the token-cost penalty of giving an agent persistent cross-session memory and a
self-correction loop, which normally means re-sending growing context on every turn.

## Practical Uses
- Study the byte-stable-prefix + cache-reusing-fork technique as a pattern to apply to a
  self-built agent harness, independent of which model it targets.
- Run a cheap, high-context (1M token) coding agent against DeepSeek V4 for exploratory or
  side-project work where Claude/Codex cost is the limiting factor.
- Compare DeepSeek V4's price-performance against Claude Code/Codex for a specific
  recurring task.

## Product Opportunities
None identified directly — this is an agent, not infrastructure to embed.

## Agent / Automation Opportunities
The cache-reuse architecture (persistent memory and self-correction riding on prompt-cache
hits rather than fresh context) is the interesting transferable idea here, more than the
tool itself.

## Integration
npm/CLI install (`dao` command), terminal-native. Requires a DeepSeek API key
(register-and-go, pay-as-you-go, reachable outside mainland China too). Medium effort mainly
because the value proposition depends on actually using DeepSeek as the backing model.

## Architecture Notes
Byte-stable prompt prefixes mean the agent avoids re-formatting or re-ordering earlier
context in ways that would invalidate a model's prompt cache — a provider-specific
discipline that's easy to accidentally break with naive context assembly. "Cache-reusing
forks" extend this to branching/sub-session work without starting the cache from scratch.

## Maturity
Emerging. Created 2026-06-08, MIT license, CI in place, 786 stars / 27 forks. No tagged
release since v0.4.7 (2026-07-18) despite commits through 2026-10-28 — check main-branch
stability before relying on a specific release.

## License
MIT. No restrictions.

## Alternatives
Claude Code / Codex / Cursor (mainstream, pricier). GLM Coding Plan (scarce quota, the
China-market alternative the project's own README compares against). Chronos and other
DST-style agent tools (catalogued — different focus, not a cost-architecture play).

## Risks / Limitations
Positioned and documented primarily for the Chinese market/DeepSeek ecosystem; English
adoption signal is thin. Narrow single-vendor focus (DeepSeek V4) — no equivalent
cache-reuse benefit if pointed at another model. No tagged release in ~4 months despite
active commits.

## Recommendation
STUDY — the cache-reuse architecture is worth understanding and could inform cost
optimization in an in-house agent harness, even without adopting Dao Code itself.

## Change History
### 2026-11-11
Initial discovery and review. GitHub API verified: MIT, 786 stars/27 forks/8 subscribers,
created 2026-06-08, pushed 2026-10-28, not archived. Latest tagged release v0.4.7
(2026-07-18).
