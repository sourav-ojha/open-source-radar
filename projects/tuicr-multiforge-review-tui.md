# tuicr

## Summary
A terminal UI for reviewing diffs: one continuous scrolling stream instead of
GitHub's per-file tabs, with line/range/file/review-level comments persisted
across sessions, and three export paths — push a real review to GitHub,
GitLab, Gitea, Bitbucket, Azure DevOps, or Gerrit, copy structured markdown
to the clipboard, or pipe to stdout. Works against git, jj, and Mercurial
working copies, not just GitHub PRs.

## Why I Should Care
Code review currently means tab-switching between terminal and browser.
tuicr collapses that into one terminal session and still produces a real
review on whichever forge the repo actually lives on — useful across
personal GitHub repos, any self-hosted Gitea/GitLab instance, and work that
touches multiple forges.

## Problems It Can Remove
- Scrolling GitHub's per-file diff UI in a browser tab just to leave
  comments that could be written faster in a terminal.
- Needing separate review workflows for GitHub vs. a self-hosted GitLab/Gitea
  instance.
- Losing review progress on a long diff between sessions (comments persist
  at file/hunk granularity).

## Practical Uses
- Reviewing a teammate's or an agent's PR without leaving the terminal.
- Reviewing uncommitted local changes or a commit range before pushing.
- Reviewing the same way whether the remote is GitHub, GitLab, or Gitea.
- Piping a review to clipboard/stdout to paste into a PR description or feed
  as context to a coding agent's follow-up prompt.

## Product Opportunities
None directly — this is a personal dev-workflow tool, not a building block
for a product. The "one local review surface, multiple forge export
targets" pattern is worth remembering if an internal code-review tool is
ever built.

## Agent / Automation Opportunities
No MCP server yet, but the clipboard/stdout markdown export is a ready-made
bridge into an agent's follow-up prompt ("here's my review, now fix these").

## Integration
Install via `curl | sh`, Homebrew, pacman, Cargo, Nix, or build from source.
No server, no account, no config required to start reviewing.
**Integration effort: Low.**

## Architecture Notes
Pure terminal client against the local VCS plus each forge's API for the
export step — no intermediate server. Multi-VCS support (git/jj/Mercurial)
and multi-forge export (6 platforms) are the two pieces of real engineering
here; everything else is a fairly standard TUI diff viewer.

## Maturity
Created 2026-01-08 (~10 months old), v0.28.0 released 2026-10-26, MIT
licensed, 339 forks, 96 open issues, distributed via brew/pacman/cargo/nix
plus a dedicated install script and website (tuicr.dev). The star-to-
subscriber ratio (3,600 stars / 8 subscribers, ~450:1) is unusually high for
a repo this size, but the fork count and open-issue volume look like real
usage rather than fabricated engagement — noted for awareness, not treated
as disqualifying.

## License
MIT. No commercial-use restrictions.

## Alternatives
- GitHub/GitLab's own web PR review UI.
- `gh pr diff` / `glab mr diff` piped to a pager (no comment tracking or
  export).
- difftastic/delta for better diff rendering, without any review-tracking
  layer.

## Risks / Limitations
- Rust CLI with no Node/JS integration surface beyond invoking the binary.
- Elevated star:subscriber ratio noted above; not independently verifiable
  beyond the fork/issue activity already observed.

## Recommendation
**USE NOW** — install the binary and try it on the next real PR review
instead of GitHub's web UI; low risk, no infrastructure to stand up.

## Change History
### 2026-11-13
Initial discovery and review. Rotation slot 3 (developer utilities,
debugging, testing).
