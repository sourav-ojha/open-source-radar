# Proval

## Summary
Self-hosted LLM code review agent. Connects to GitHub, GitLab, or Forgejo (covering Gitea/Codeberg via API compatibility); on a meaningful PR push it reads the diff, groups changed files into review units, runs specialist sub-agents per group to investigate cross-file issues, and posts a consolidated review with inline comments grouped by severity. Bring-your-own OpenAI-compatible model, including local options like Ollama or llama.cpp.

## Why I Should Care
A first-pass automated code reviewer that runs entirely on infrastructure and a model of one's choosing, rather than a per-seat SaaS product sending diffs off-network. The sub-agent-per-file-group architecture is also a reusable pattern worth studying independent of adoption.

## Problems It Can Remove
- Manual first-pass review effort on routine PRs
- Sending private code to a third-party SaaS reviewer
- Per-seat licensing for a code-review product

## Practical Uses
- Automated first-pass review on personal or team repos before a human looks
- Homelab/internal-network code review with a local model, no external network dependency
- Responding to review-thread replies and issue comments, not just initial PR review

## Product Opportunities
Internal QA layer across personal repos running against a cheap local or hosted model instead of a per-seat SaaS reviewer. The diff → review-units → per-group sub-agents → consolidated severity output pattern is directly reusable for any other "review a large artifact with sub-agents" tool.

## Agent / Automation Opportunities
This is itself a coding-agent product: a PR-review agent with sub-agents per review unit. Its Git-host webhook integration model is a template for wiring any similar automation to GitHub/GitLab/Forgejo.

## Integration
Docker Compose in under 10 lines, or a single `docker run`; needs an `ENCRYPTION_KEY` and, for HTTPS deployments, `COOKIE_SECURE=true`. A documented single-replica Kubernetes pattern exists for SQLite-backed deployments.

## Architecture Notes
Splits a diff into review units, dispatches a specialist sub-agent per unit so each can explore the codebase independently and catch cross-file issues, then consolidates findings by severity before posting. Repository-level settings control review frequency (off / first-push-only / every push).

## Maturity
Experimental: created July 2026, 56 stars, single maintainer, zero forks. No GitHub Releases have been tagged — the only distribution channel is a rolling `ghcr.io/seoes/proval:latest` Docker image, so there's no pinned version history to audit yet.

## License
AGPL-3.0. **Flagged per policy:** network use counts as distribution under AGPL. Self-hosting internally to review private repos is fine. Do not fork/modify and offer it as a hosted review service to others (e.g. as an MSSP add-on) without releasing the modified source or securing a commercial license, should one become available.

## Alternatives
CodeRabbit (SaaS), Qodo/PR-Agent (open source), Greptile, Sourcery.

## Risks / Limitations
- No tagged releases; only a rolling Docker image
- Single maintainer, zero forks — production-readiness claims should be treated cautiously
- Anthropic/Gemini support listed as "planned," not yet first-class; current story favors OpenAI-compatible APIs

## Recommendation
PROTOTYPE — worth a small pilot on a low-stakes personal repo against a cheap model before trusting it on anything critical, given the lack of pinned releases and the very young project age.

## Change History
### 2026-08-30
Initial discovery and review.
