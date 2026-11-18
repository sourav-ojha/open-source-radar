# HOL Guard

## Summary
A unified runtime policy-enforcement layer ("open-source antivirus for AI agents") covering six surfaces in one tool: destructive shell commands, package installs (supply-chain intelligence before install), plugins/skills/agent config, MCP servers and tool calls, prompt-injection/sensitive-content screening, and an audit trail of every decision. Installs native hooks and managed MCP proxies across 16 coding-agent harnesses (Codex, Claude Code, Copilot CLI, Cursor, Cline, Gemini CLI, Grok, Hermes, Kimi Code, Pi, OpenClaw, OpenCode, Antigravity, ZCode, Devin, and Paseo via provider integrations). From Hashgraph Online (the Hedera-ecosystem open-source group).

## Why I Should Care
This catalog already has point solutions for several of the surfaces HOL Guard covers individually — Destructive Command Guard for shell commands, SkillSpector/cisco-mcp-scanner for skill and MCP security. HOL Guard's distinction is unifying command blocking, supply-chain package scanning, skill/plugin review, MCP tool-call inspection, and prompt-injection screening behind one approval workflow and one audit trail, across more harnesses than any single point tool in this catalog covers. Whether that unification is worth the larger surface area versus composing narrower tools is the real evaluation question, not whether the individual capability exists somewhere already.

## Problems It Can Remove
Removes the need to run and reconcile several separate safety tools (command guard + skill scanner + MCP inspector) with separate approval flows; centralizes "what did my agent try to do and was it allowed" into one receipt/audit log across every harness in use.

## Practical Uses
- Single install (`pipx install hol-guard && hol-guard init`) protecting whichever of the 16 supported harnesses are actually in use, rather than wiring a separate tool per harness.
- `hol-guard command test`/`explain` to understand why a command would be paused before running it manually — same workflow idea as Destructive Command Guard's `dcg explain`.
- `hol-guard supply-chain scan` and `hol-guard abom --format json` for an AI bill-of-materials view of what an agent's workspace actually depends on.
- `hol-guard approvals` / `hol-guard receipts` as a review and audit trail, directly relevant to the MSSP/PTaaS context where "what did the agent actually touch, and who approved it" needs to be answerable.

## Product Opportunities
The ABOM (AI bill-of-materials) export and receipt/audit trail are the pieces most directly reusable in an MSSP context — a client-facing "what did your AI coding agents actually do this week" report is a plausible add-on to an existing security-portal product.

## Agent / Automation Opportunities
This is agent-safety infrastructure itself: native pre-tool hooks for most harnesses, managed MCP proxies where native hooks aren't available, and a CLI/dashboard for reviewing and approving paused actions.

## Integration
`pipx install hol-guard` (Python 3.10+) plus a native runtime per platform; a first-run wizard discovers installed agents and walks through setup, asking before each change. Per-harness enforcement depth varies — the support matrix documents what's actually enforced vs. just reviewed per integration. Low-to-medium integration effort depending on how many harnesses are in simultaneous use.

## Architecture Notes
Command-pattern coverage is generated from a Rust-compiled extension catalog, with external contributions requiring explicit opt-in (core protections stay on by default) — a sensible trust boundary for a tool that can block your agent's commands.

## Maturity
Emerging-to-mature. Created 2026-03-28 (about 8 months old), active CI/publish pipelines, OpenSSF Scorecard badge, PyPI downloads tracked publicly, sponsored by at least two named companies (sponsorship explicitly stated not to affect security decisions). 1,110 stars against 278 forks but only 2 subscribers — an elevated star:subscriber ratio flagged per this catalog's recurring engagement-inflation pattern, though the substantial fork count (278) is a real-usage signal that the thinner rejected cases in past runs lacked.

## License
Apache-2.0. No commercial-use restrictions.

## Alternatives
Destructive Command Guard (command-blocking only, narrower but more battle-tested at this specific job), SkillSpector / NVIDIA SkillEvaluator (skill/plugin quality and security, not runtime command blocking), cisco-mcp-scanner (MCP server scanning specifically, with YARA/CVE/VirusTotal backing Cisco doesn't have here). HOL Guard's pitch is breadth across all of these in one approval/audit flow; a team already happy with point tools has less reason to switch.

## Risks / Limitations
Elevated stars:subscribers ratio (555:1) warrants some caution per this catalog's established pattern, though paired with real fork activity. Broader surface area than any single point tool means a larger blast radius if the tool itself misbehaves or has a fail-open bug (Destructive Command Guard's project file documents exactly this failure mode happening once in a narrower tool — worth checking HOL Guard's own incident history before relying on it as the sole safety net). Per-harness enforcement depth is uneven; check the support matrix for the specific harnesses in use.

## Recommendation
PROTOTYPE — worth testing alongside (not necessarily instead of) the already-catalogued point tools, specifically for the unified audit trail and ABOM export, which none of the narrower alternatives provide.

## Change History
### 2026-11-18
Initial discovery and review. v3.59.0 at review time.
