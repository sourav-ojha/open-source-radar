# commit-check

## Summary
Commit policy as code: a single config file checks commit messages, branch names, author
identity, sign-offs, merge-base/rebase status, tags, and committed files against team
rules — recently extended to also check for AI-attribution trailers (marking which
commits were AI-generated) across both human and AI-agent contributions.

## Why I Should Care
As AI-agent-authored commits become routine, knowing which commits in a repo were
agent-generated is useful for audit and review purposes, and this enforces that as
policy rather than leaving it to whoever remembers to add a trailer.

## Problems It Can Remove
Ad hoc git hooks and CI scripts that check commit-message format, branch naming, or
signed-off-by status, usually written once per repo and barely maintained afterward.

## Practical Uses
- Enforce conventional-commit message format and branch-naming rules in CI with one
  config file instead of a custom script
- Require AI-attribution trailers on commits in a repo that mixes human and agent commits
- Block merges from unsigned or wrongly-attributed commits

## Product Opportunities
None — internal tooling.

## Agent / Automation Opportunities
Directly relevant to repos where coding agents commit regularly: gives a policy-level way
to track and enforce disclosure of AI-authored work, rather than relying on convention.

## Integration
CLI via pip/pipx, GitHub Action, or pre-commit framework hook. Low integration effort —
one config file.

## Architecture Notes
Nothing unusual architecturally; the value is in the breadth of checks bundled into one
config format (message, branch, author, sign-off, tags, files, AI attribution) rather
than any one check being novel on its own.

## Maturity
Mature. Maintained since 2022, currently v2.19.0, actively released. Modest adoption
(78 stars) for its age — a niche, low-visibility tool rather than a widely-known one.

## License
MIT — no restrictions.

## Alternatives
commitlint (message format only, Node ecosystem, no branch/author/AI-attribution
checks); hand-rolled pre-commit/CI scripts.

## Risks / Limitations
Niche and low-visibility (78 stars after 3+ years) — verify its AI-attribution check set
matches whatever agent tooling is actually in use before relying on it for compliance
purposes.

## Recommendation
USE NOW — low-effort, no-downside addition to CI on any repo where commit hygiene or
AI-attribution tracking matters. Today's "Small but High-Leverage Utility" pick.

## Change History
### 2026-10-16
First discovery and review.
