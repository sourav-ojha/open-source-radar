# Destructive Command Guard (dcg)

## Summary
A high-performance hook that intercepts and blocks destructive commands — `git reset --hard`, `rm -rf`, `DROP TABLE`, `kubectl delete namespace`, `aws ec2 terminate-instances`, `docker system prune`, and more — before an AI coding agent executes them. Installs natively across Claude Code, Codex CLI, Gemini CLI, GitHub Copilot CLI/Chat, Cursor, and a dozen other agent harnesses.

## Why I Should Care
This is exactly the failure mode this radar's own operating rules exist to prevent: an agent running a destructive git or shell command against live source or infrastructure. It is a near-zero-effort install that adds a real safety net across whichever coding agent is in use that day.

## Problems It Can Remove
Removes the manual vigilance of reviewing every command an agent proposes before approving it, specifically for the class of commands that are catastrophic and hard to reverse.

## Practical Uses
- Blanket protection against an agent running `git reset --hard`, `rm -rf ./src`, or a destructive SQL/Terraform/kubectl/aws command mid-session.
- Per-agent trust profiles: wider allowlist for a trusted harness (e.g. Claude Code), stricter rule packs for an unfamiliar one.
- CI/pre-commit scan mode to catch dangerous commands introduced in a PR before merge.
- `dcg explain "command"` to understand exactly why a command would be blocked, before running it manually.

## Product Opportunities
None — pure personal/engineering safety tooling.

## Agent / Automation Opportunities
Installs as a native `PreToolUse`-style hook for ~13 different coding-agent harnesses. This *is* agent-safety infrastructure in its own right — a hook every coding-agent session arguably should run.

## Integration
Single native binary via a curl/PowerShell installer (`--easy-mode` auto-detects platform and configures hooks); releases are signed with minisign and Sigstore/cosign. Low integration effort — one command sets it up for whichever agents are detected on the machine.

## Architecture Notes
SIMD-accelerated command classification for sub-millisecond latency; heredoc and inline-script scanning catches `python -c "os.remove(...)"`-style indirection; 50+ rule packs cover databases, Kubernetes, Docker, and major cloud CLIs; agent-specific `trust_level` profiles adjust allowlists and enabled packs per harness rather than applying one global policy.

## Maturity
Mature relative to its age — created January 2026, ~6,000 stars, very active (commits same day as review), 245 forks, extensive documentation and cross-platform signed releases.

## License
MIT License with an OpenAI/Anthropic Rider (custom, not a standard OSI license text, though close to MIT in effect). The rider voids the license specifically for OpenAI, Anthropic, their affiliates, and anyone acting on their behalf — it does not affect an independent developer or company. Flagging because it is an unusual clause worth being aware of before redistributing.

## Alternatives
**Dicklesworthstone/slb** (same author, 79 stars) — a complementary "two-person rule" CLI requiring peer approval before destructive commands; narrower adoption, could be paired with dcg rather than chosen instead of it. Otherwise the status quo is trusting the agent or manual review of every command.

## Risks / Limitations
- Single primary maintainer (2,350 of 2,364 commits) — bus-factor risk despite the project's engineering polish.
- Custom license rider is unusual; read it if your org has any OpenAI/Anthropic affiliation.
- Allowlist/pack tuning needed up front to avoid over-blocking legitimate commands.

## Recommendation
USE NOW — install it against whichever coding agent is the daily driver; the downside risk of *not* having this is exactly the kind of destructive-action mistake this operating spec explicitly warns about.

## Change History
### 2026-09-18
Initial discovery and review. Rotation slot 3 (developer utilities, debugging, testing).
