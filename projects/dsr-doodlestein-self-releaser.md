# dsr (Doodlestein Self-Releaser)

## Summary
A fallback release tool for when GitHub Actions is throttled: it monitors a
repo's Actions queue time, and once it crosses a threshold, builds the
release locally by running the existing `.github/workflows/release.yml`
through `nektos/act` (Linux) or natively over SSH (macOS, Windows), signs
the artifacts with Minisign, generates an SBOM, and uploads to GitHub
Releases — same artifacts, no queue wait.

## Why I Should Care
Narrow but concrete: if a CLI tool's release ever gets stuck behind a
20+ minute GitHub Actions queue, this reuses the existing workflow YAML
instead of maintaining a second parallel build pipeline, and still produces
identical signed artifacts.

## Problems It Can Remove
- Users waiting on a release that's sitting in a GitHub Actions queue.
- Maintaining a second, parallel local-build pipeline that could drift from
  the CI workflow.
- Manually re-running a release process by hand on multiple OSes during an
  outage.

## Practical Uses
- Falling back to a local/SSH build the moment GH Actions queue time
  exceeds a threshold, with zero changes to the existing workflow file.
- Producing multi-platform (Linux/macOS/Windows) signed binaries from one
  command (`dsr fallback --repo X --version vY`) when CI itself is the
  bottleneck.

## Product Opportunities
None directly — internal release tooling, not a product-building block.

## Agent / Automation Opportunities
Structured exit codes (0-8) and JSON output are built for scripting —
could be wired into a release-automation agent that decides whether to
wait on CI or trigger the local fallback itself.

## Integration
Bash 4.0+ script installed via a one-line curl install script; requires
`nektos/act` for Linux builds and SSH access to a macOS/Windows machine for
native builds on those platforms. **Integration effort: Medium** — no
server, but the macOS/Windows path assumes you actually have those
machines reachable over SSH.

## Architecture Notes
Deliberately reuses the existing CI workflow YAML rather than building a
parallel system — "your GitHub Actions workflow is the source of truth."
Queue-time monitoring triggers the fallback automatically instead of
requiring manual threshold tuning.

## Maturity
By a known prolific author (Dicklesworthstone — several of their other
tools are already catalogued). Created 2026-01-30, latest tagged release
v0.2.3 (2026-10-03) though the repo has continued to receive commits since
— 79 stars, 9 forks, 0 subscribers, 31 open issues. Thin community
validation; mostly reflects the author's own usage across their other
projects' releases.

## License
**MIT License with an OpenAI/Anthropic Rider** — an unusual, explicit
restriction voiding any license grant to OpenAI, Anthropic, their
affiliates, or anyone acting on their behalf (including for training,
benchmarking, or incorporating the software into a dataset). This does not
restrict an ordinary developer's use of the tool, including via a Claude
Code or ChatGPT session run on that developer's own behalf rather than
Anthropic's/OpenAI's — but it is a genuinely unusual clause worth knowing
about before redistributing or building on this code, and it is the kind
of thing an AI coding agent reviewing or modifying this repository should
be aware it exists.

## Alternatives
- Waiting out the GitHub Actions queue.
- Maintaining a separate local/manual release process with no relation to
  the CI workflow (common status quo, prone to drift).
- Self-hosted GitHub Actions runners (addresses the same throttling
  problem differently, at higher operational cost).

## Risks / Limitations
- Narrow applicability: only useful for someone who (a) ships CLI binaries
  via GitHub Actions regularly and (b) has actually hit queue throttling.
- Native macOS/Windows builds require owning and maintaining those build
  machines over SSH.
- Thin community (0 subscribers); effectively a single-author tool so far.
- The license rider's practical scope for ordinary AI-assisted development
  use is not something this review can authoritatively resolve — flagged
  rather than adjudicated.

## Recommendation
**PROTOTYPE** — worth keeping in mind for any future CLI tool shipping
binaries via GitHub Actions, rather than an immediate adoption candidate
given no current project hits the throttling problem it solves.

## Change History
### 2026-11-13
Initial discovery and review as Small but High-Leverage Utility of the
day. Rotation slot 3 (developer utilities, debugging, testing).
