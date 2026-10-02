# difit

## Summary
A CLI tool that spins up a local web server to view git diffs in a GitHub-style "Files changed" UI. Works on single commits, commit ranges, branches, the working tree, staged changes, or a live GitHub PR (via `gh pr diff`), and lets you leave inline review comments that can be copied out as AI-prompt-ready text or injected back in via `--comment`.

## Why I Should Care
Reviewing your own diffs before pushing is normally either squinting at `git diff` in a terminal or opening a draft PR just to get GitHub's UI. difit gives the GitHub review UI locally, with zero network round-trip, and adds a feature GitHub doesn't have: turning review comments directly into prompts for an AI coding agent. It also ships as a Claude/Codex-style "skill" (`npx skills add yoshiko-pg/difit`) so an agent can open the viewer itself when asked to show a diff for review.

## Problems It Can Remove
- Opening throwaway draft PRs just to self-review a diff before it's ready.
- Manually re-typing review feedback as a prompt for an AI agent — difit's comment-to-prompt copy does this directly from the diff view.
- Reviewing a long PR in a terminal pager when a proper side-by-side view would be faster.

## Practical Uses
- Pre-push self-review: `difit .` to see all uncommitted changes in a GitHub-like view before committing.
- Reviewing a teammate's PR locally: `difit --pr https://github.com/owner/repo/pull/123`, including importing unresolved review threads.
- Comparing a feature branch against `main` before opening a PR: `difit feature main`.
- Piping diffs from any other tool via stdin for a consistent review UI regardless of source.
- Wiring into an agent workflow: ask a coding agent to "review this diff in difit," which surfaces findings in the local viewer instead of a wall of chat text.

## Product Opportunities
Not product material on its own — it's a personal/team dev-tool utility rather than something to embed in a SaaS product.

## Agent / Automation Opportunities
Ships an official "skill" package (`npx skills add yoshiko-pg/difit`) exposing `difit` and `difit-review` capabilities to Claude Code and similar agents, letting an agent open a visual diff viewer for human review instead of dumping diff text into chat. This is a good model for other CLI tools wanting agent-skill distribution.

## Integration
`npm install -g difit` or `npx difit`, no configuration needed. PR mode depends on the `gh` CLI being authenticated. No server, no account, no Docker needed — runs a local HTTP server on demand.

## Architecture Notes
Single Node/TypeScript CLI that renders a local web UI (not a VS Code extension or browser extension), which keeps it diff-source-agnostic — same viewer whether the diff comes from git, a GitHub PR, or stdin. The comment system models review threads with explicit positions (file + side + line), replies, and dedup-on-import, closely mirroring GitHub's own review-thread data model.

## Maturity
Emerging but solidly adopted: 3,221 GitHub stars, created June 2025, actively released (v5.0.12 as of 2026-08-29), CI badge passing, README translated into 4 languages. Single primary maintainer (yoshiko-pg) — bus-factor risk, but release cadence is healthy.

## License
MIT, verified via GitHub API. No restrictions on commercial or internal use.

## Alternatives
`git diff` + a pager/delta, GitHub Desktop's local diff view, opening a draft PR on GitHub, VS Code's built-in diff view. None of these combine a GitHub-identical review UI, PR-thread import, and AI-prompt-ready comment export in one zero-config CLI.

## Risks / Limitations
Single-maintainer project — worth keeping an eye on continued maintenance. PR mode is a thin wrapper over `gh pr diff --patch`, so it inherits whatever limitations the GitHub CLI has for very large PRs.

## Recommendation
USE NOW — zero adoption risk, `npx difit` works immediately with no setup, and it's a genuine daily-workflow improvement for self-review and AI-agent-assisted review.

## Change History
### 2026-10-02
Initial discovery and review.
