# Drydock (git fleet)

> **Name collision notice:** unrelated to the already-catalogued [Drydock](drydock-container-watcher.md) (`CodesWhat/drydock`, a self-hosted container-image-update watcher, AGPL-3.0). This is a different project from a different author, repo: `yetidevworks/drydock`, MIT. Disambiguate carefully — both are indexed under the display name "Drydock."

## Summary
A single Rust binary that renders a live TUI dashboard of every git repo under a configured set of root directories: what's uncommitted, what's unpushed (across every local branch, not just the checked-out one), and what's tagged-but-changed-since ("needs release"), in one screen, color-coded by state.

## Why I Should Care
Anyone with more than a handful of local checkouts (side projects, client repos, forks) loses track of what still has uncommitted or unpushed work sitting in it. Drydock answers "what did I touch recently, what haven't I committed, what haven't I pushed, what's worth releasing" for an entire tree of repos at a glance, instead of scripting a loop over `git status`.

## Problems It Can Remove
- A hand-rolled shell loop over `git status`/`git log` across dozens of checkouts.
- Losing track of commits on a side branch that never got pushed.
- Not noticing a repo has unreleased commits sitting past its last tag.

## Practical Uses
- Run with no arguments for a live dashboard over the configured default root (e.g. `~/Projects`).
- `--root` to point at an arbitrary tree (e.g. `$GHQ_ROOT`) for a one-off look without touching the main config.
- Filter to "touched in the last day/week" to answer "what did I actually work on recently" across many repos.
- Check release state (`unreleased` / `released` / `needs release` / `held`) before a batch-release day.

## Product Opportunities
None — this is a personal developer-productivity tool, not a building block.

## Agent / Automation Opportunities
Narrow: could be shelled out to by an agent to decide which of several repos actually need attention before starting work, but it's built as an interactive human TUI first.

## Integration
`brew install yetidevworks/drydock/drydock`, `cargo install drydock`, or build from source. Single binary, macOS and Linux, no runtime dependencies. Low integration effort.

## Architecture Notes
Per-root-set caching (each distinct `--root` combination gets its own cache file) is the one deliberate design choice worth noting — it means switching to a one-off root doesn't cost the main fleet its warm-start cache.

## Maturity
Emerging. Created 2026-07-30 (~3 months old), 326 stars, 12 forks, 2 subscribers, v1.3.1 released 2026-11-02.

## License
MIT — no restrictions.

## Alternatives
`gita` and `myrepos` (`mr`) — batch-run git commands across repos but don't give a live, colorized dashboard view; a hand-rolled shell loop over `git status`; VS Code multi-root workspaces (no release-state tracking).

## Risks / Limitations
- Name collides with the unrelated, already-catalogued Drydock (container-image watcher) — see notice above.
- Small and young (326 stars, 2 subscribers, 3 months old), macOS/Linux only (no Windows).

## Recommendation
USE NOW as a small high-leverage utility — narrow, well-executed, zero-risk to install, and solves a real "where did I leave work" problem for anyone juggling many repos.

## Change History
### 2026-11-06
Discovered and reviewed as today's Small but High-Leverage Utility. GitHub API verified: MIT, 326 stars/12 forks/2 subscribers, created 2026-07-30, pushed 2026-11-02, latest release v1.3.1 (2026-11-02).
