# ClickHouse Loom

## Summary
A self-hosted shared-memory, document-search, and code-intelligence layer for AI agents, built by ClickHouse Inc. on top of ClickHouse itself. Memories are linked to the code they describe (they follow refactors, get flagged when the code changes, and surface when an agent reads or edits the relevant file), there's a multi-language code index (definitions, callers, change impact), document search with citations over PDF/.docx/.pptx, and multi-user namespaces with roles and tokens. Ships an MCP server plus Python and TypeScript SDKs, with native Claude Code and Codex plugins.

## Why I Should Care
This is the agent-memory niche (already crowded in this catalog — CortexDB, rostam, mnemosyne, cognee, etc.) but from a credible infrastructure vendor with a real database underneath it, rather than another weeks-old vector-store wrapper. The differentiator is tying memory to *code* specifically — a memory about a function stays attached to that function through refactors and gets invalidated/flagged when the function changes, which is a materially different and more useful guarantee than plain semantic recall for a coding-agent workflow.

## Problems It Can Remove
Removes the need to re-explain repo context every session, removes stale-memory rot (memories tied to code are flagged when the code moves), and removes a separate RAG/document-search stack for PDFs and Office docs an agent needs to cite.

## Practical Uses
- Give Claude Code/Codex a persistent memory of decisions and conventions for a specific repo, scoped and shared across a team via namespaces.
- Code index for "who calls this" / blast-radius questions without the agent re-reading the whole repo.
- Document search with citations over policy docs, specs, or PDFs an agent needs to answer from.
- Multi-agent shared context: task inboxes and decisions visible to more than one agent working the same codebase.
- Session briefs / run history: resume a coding session from a prior run's goals, outcome, and cost.

## Product Opportunities
Could underpin an internal "team memory" layer for an agent-heavy engineering workflow; the multi-user namespace/role model is close to what a small team would need to ship this as a shared internal tool without building the auth/sharing layer themselves.

## Agent / Automation Opportunities
MCP server out of the box; Claude Code and Codex plugins; Python/TypeScript SDKs for building memory into a custom agent application.

## Integration
Single Docker install command (`install.sh`) starts Loom + ClickHouse together; runs personal (localhost-only, no login) or team mode (authenticated, namespace-scoped) from the same image, switchable without data loss. GitHub Codespaces quickstart available for a zero-install trial. Low integration effort.

## Architecture Notes
Built on ClickHouse for storage — worth studying for how a column-store/OLAP engine gets repurposed as an agent-memory and code-index backend rather than a typical vector DB. Benchmarks are published with explicit caveats: coding-agent answer quality 0.84 vs 0.78 without Loom (20% lower model cost) on 24 questions × 3 paired runs on the ClickHouse codebase itself with Claude Sonnet 5; file localization 96.4% vs 91.4%; a separate `memory.ask` configuration scored 96.6% on all 500 LongMemEval-S questions. The README is explicit that results are workload-specific and not all differences are statistically established — a level of honesty that's rare in this niche and worth noting positively per the project's own quality bar.

## Maturity
Emerging. Created 2026-10-08 (about six weeks old at review), but unusually polished for its age: CI, OpenSSF Scorecard badge, i18n in six languages, published reproducible benchmark protocols, PyPI + npm SDKs. Low star count (159) is consistent with genuine early-stage traction rather than inflation — no red flags in the stars:forks:subscribers ratio.

## License
Apache-2.0. No commercial-use restrictions.

## Alternatives
CortexDB, rostam, mnemosyne, cognee (topoteretes), and the broader embedded agent-memory wave already in this catalog — Loom's distinction is the code-linked memory model and ClickHouse-backed self-hosting from a company that actually operates ClickHouse at scale, not the memory concept itself, which is now mainstream.

## Risks / Limitations
Six weeks old — no long-term track record. Benchmarks are self-reported (though unusually caveated). Requires running ClickHouse alongside it (one more moving part than a pure embedded library, though the installer handles this). Worth re-checking in a few months for real-world adoption signal beyond the vendor's own numbers.

## Recommendation
PROTOTYPE — worth standing up against a real repo to see if the code-linked memory model holds up in practice; the architecture is distinct enough from the rest of the agent-memory catalog to justify the setup cost.

## Change History
### 2026-11-18
Initial discovery and review. v1.2.0 at review time.
