# NVIDIA SkillEvaluator

## Summary
A multi-tier framework for evaluating AI agent skills and plugins: static compatibility checks (HarnessCheck), quality gates, semantic overlap detection, synthetic evaluation-dataset generation, and live agent evaluation measuring how a skill actually changes agent behavior. As of v1.0.1 (2026-11-18), HarnessCheck runs standalone — no model key, no NVIDIA pipeline dependency — checking a skill against versioned compatibility profiles for Claude Code, Codex, Copilot CLI, Hermes, OpenClaw, and OpenCode.

## Why I Should Care
This was held out of the catalog on 2026-09-30 specifically because it needed NVIDIA's own Verified Skills/Harbor pipeline for full value, with no lighter-weight standalone use case. v1.0.1's HarnessCheck removes exactly that barrier: `skillevaluator harness-check ./my-skill` is a zero-dependency static check that catches why a skill gets missed, rejected, or truncated by a specific harness (discovery paths, frontmatter limits, invocation settings, plugin-component conflicts) — directly useful for anyone authoring or maintaining a Claude Code skill/plugin, independent of NVIDIA's broader evaluation stack.

## Problems It Can Remove
Removes the trial-and-error of discovering a skill is silently truncated or ignored by a specific harness only after shipping it; removes needing NVIDIA's full Tier-3 pipeline just to get basic compatibility signal.

## Practical Uses
- `skillevaluator harness-check ./my-skill` before publishing a Claude Code skill, to catch harness-specific truncation/rejection issues ahead of time.
- Full Tier 1-3 pipeline (quality gates, overlap detection, live evaluation) for anyone building a skill registry or marketplace who needs more than a compatibility check.
- NeMo Gym backend integration (`--backend nemo-gym`) for teams already in NVIDIA's evaluation ecosystem.
- `--quality-config` for token budgets, description limits, and score deductions tuned to a specific skill-authoring standard.

## Product Opportunities
A skill-registry or marketplace product could use HarnessCheck as an automated submission gate without adopting the rest of NVIDIA's stack.

## Agent / Automation Opportunities
Installable as an agent-assisted evaluation skill itself (`npx skills add NVIDIA/SkillEvaluator`), guiding an agent through preparing cases and interpreting results; usable in repository CI directly.

## Integration
PyPI package (`pip install skillevaluator`), standalone CLI, CI bundle available per release. Low integration effort for the HarnessCheck subset; the full Tier 3 live-evaluation path needs model-key/NeMo Gym setup.

## Architecture Notes
The tiered structure (static compatibility -> quality gates -> live evaluation) is a reasonable model for skill-evaluation generally — worth studying even for teams not using this specific tool, as a checklist for what "is this skill actually good" should mean beyond "does it run."

## Maturity
Mature enough to matter now: v1.0.1 is explicitly "our first complete release in the 1.0 series," backed by NVIDIA with a published arXiv paper. 3,425 stars against 7 subscribers is an elevated ratio per this catalog's recurring engagement pattern, though 475 forks indicate real usage, and the NVIDIA org affiliation reduces the usual astroturfing concern for that ratio.

## License
Apache-2.0.

## Alternatives
SkillSpector (skill security scanning specifically, already catalogued USE NOW) solves an adjacent but distinct problem (security/malicious-content detection vs. harness-compatibility/quality scoring). AsItIs agent tools (today's Small but High-Leverage Utility) checks a related but narrower question — what actually loads from CLAUDE.md/AGENTS.md — worth comparing directly against HarnessCheck's harness-compatibility focus in a future review.

## Risks / Limitations
Tied to NVIDIA's own roadmap and terminology (Tier 1/2/3, Harbor, Verified Skills) which may shift; the full live-evaluation tier still requires model-key spend and more setup than the new standalone HarnessCheck subset.

## Recommendation
PROTOTYPE — the standalone HarnessCheck subset clears the bar that kept this out of the catalog previously; worth running against any Claude Code skill before publishing it.

## Change History
### 2026-11-18
Promoted from Worth Watching (held 2026-09-30, re-checked 2026-11-04 and 2026-11-11 with no change) to PROTOTYPE following v1.0.1, which added standalone HarnessCheck — resolving the original "needs NVIDIA's full pipeline" objection.
