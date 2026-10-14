# SkillSpector

## Summary
Security scanner for AI agent skills — the installable packages that extend Claude Code, Codex CLI, Gemini CLI, and MCP hosts. Runs 71 vulnerability-detection patterns across 17 categories (prompt injection, data exfiltration, privilege escalation, supply-chain risk, MCP tool poisoning and least-privilege violations, memory poisoning, dangerous code via AST analysis, taint tracking, YARA signatures) as fast static analysis, with an optional LLM semantic pass for deeper review. Looks up live CVE data via OSV.dev with offline fallback, supports baseline/false-positive suppression for clean re-scans, and outputs terminal, JSON, Markdown, or SARIF reports. Maintained by NVIDIA as the scanning stage of its Verified Skills pipeline, which signs and publishes vetted skills to NVIDIA's own skills catalog.

## Why I Should Care
The skill/plugin/MCP-server ecosystem is exploding — today's discovery run alone surfaced dozens of same-day, zero-star, templated skill and MCP-server repos with no vetting whatsoever. Installing one of these into a coding-agent setup means granting it implicit execution trust. SkillSpector is a concrete, low-effort check to run before trusting any of them, backed by NVIDIA's own stated research finding that 26.1% of a 31,132-skill sample contained vulnerabilities and 5.2% showed likely malicious intent.

## Problems It Can Remove
Manually reading a skill's source (or an MCP server's tool definitions) for prompt-injection setups, exfiltration paths, or excessive-agency grants before installing it. SkillSpector automates that review with a maintained, versioned pattern set instead of a one-off personal checklist.

## Practical Uses
- Gate any `claude plugin install`, skill download, or MCP server add behind a local scan
- Batch-scan an existing `~/.claude/skills` or plugin-marketplace directory for risk already installed
- Wire SARIF output into GitHub code scanning so a PR that adds a skill or MCP server shows findings inline
- Diff two MCP registry snapshots (`--mcp-registry-compare`) to see exactly what a registry update changed before trusting the new version
- Baseline currently-accepted findings so future re-scans surface only genuinely new risk

## Product Opportunities
A pre-install scan step like this is a natural fit inside an internal skill/plugin marketplace, or as a feature of an MSSP-style AI-agent governance offering — directly adjacent to the MSSP security admin-portal work.

## Agent / Automation Opportunities
Ships as a CLI, a Pi extension, an OpenCode extension (`/skillspector` command), and an MCP server, so it can be invoked by an agent on itself before installing something new, not just run by a human beforehand.

## Integration
Deployment: `uv tool install git+https://github.com/NVIDIA/skillspector.git`, or Docker (`make docker-build`) with no Python required locally. Mounts the target directory and scans without network access if `--no-llm` is used.
Interfaces: CLI, MCP server, SARIF/JSON/Markdown output.
Integration effort: **Low** — single install, no infrastructure to stand up, drops into an existing CI step via SARIF.

## Architecture Notes
Two-stage pipeline: fast static analysis (AST parsing, taint tracking, YARA signature matching, MCP-specific tool-poisoning checks) runs first and is sufficient on its own with `--no-llm`; an optional second stage sends content to an LLM (DeepSeek by default, flagged by the maintainers themselves as scheduled to sunset with no local-model fallback tested yet) for semantic judgment calls static rules can't make. Separate, independently-tunable size caps bound ingest (zip bombs, oversized downloads) from the downstream per-file analysis cap, and both fail closed.

## Maturity
Emerging. Repo created 2026-03-21, current release v2.12.0 (2026-09-23), Apache-2.0, actively pushed (commits as recent as today). Confirmed under the real, verified NVIDIA GitHub org (org id 1728152). Star:watcher ratio is notably elevated (20,151 stars vs 77 watchers) — elevated even relative to NVIDIA's own flagship repos (NVIDIA/TensorRT-LLM runs roughly 121:1 by the same measure) — so the star count itself should be treated as weak signal, positive or negative; the org verification and the substance of the README are the real evidence here.

## License
Apache-2.0, confirmed via GitHub API license field. No restrictions on commercial use, SaaS deployment, or embedding.

## Alternatives
- Tencent/AI-Infra-Guard (6,857 stars) — broader AI red-teaming platform (agent/skills/MCP/infra scan plus LLM jailbreak eval), established since Dec 2024, worth a look if full red-team tooling is needed rather than a skill-install gate
- akto-api-security/akto, panguard-ai, hashgraph-online/hol-guard, AgentSafe-AI/tooltrust-scanner, SoupTeam/soup-wall — a same-week wave of competing "AI agent security scanner" entrants surfaced in this run's research; none carry SkillSpector's combination of a verified major-org backer, 71 documented patterns, and a stated research methodology

## Risks / Limitations
- Elevated star:watcher ratio noted above — not disqualifying given org verification, but don't cite the star count as evidence of adoption
- LLM semantic pass depends on DeepSeek by default, which the maintainers flag as fragile (scheduled sunset, no tested local-model fallback)
- Static analysis is inherently incomplete against a genuinely novel obfuscation technique — useful as a gate, not a guarantee

## Recommendation
**USE NOW** (score 8.7/10) — low-friction install, directly addresses a real and current risk (the skill/MCP-server supply chain), credible backing.

## Change History
### 2026-10-14
First discovered via GitHub search API (`mcp security`, pushed:>2026-09-20) during the slot 1 (AI agents/MCP/coding productivity) run. Catalogued alongside a broader observation that the "AI agent/skill security scanner" niche saw a dozen-plus same-week entrants; SkillSpector stood out on verified org backing and documented methodology.
