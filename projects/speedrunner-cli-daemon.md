# speedrunner

## Summary
speedrunner speeds up everyday CLI tools written in Python, Ruby, PHP, or
Node (aws, az, gcloud, ansible, eslint, rubocop, composer, artisan, and
hundreds more) by loading each one's interpreter and code once in a
background daemon, then forking a ready process for every subsequent call
instead of paying interpreter-startup cost every time. No per-tool
configuration; it falls back to running the real tool natively whenever it
can't serve a call faithfully.

## Why I Should Care
This is pure, measured time saved on tools already in daily use — the AWS
CLI, eslint, and plenty of other Python/Node/Ruby/PHP CLIs that re-pay
interpreter startup on every single invocation. The project's own
benchmarks: a multi-tool checks script 24.0s → 5.3s (4.5x, warm daemons);
eslint on one file 328ms → 43ms; mypy 213ms → 50ms; a pre-commit hook run
1.11s → 0.31s — all with byte-identical output to the native run.

## Problems It Can Remove
- The repeated multi-hundred-millisecond interpreter-startup tax on every
  `aws`, `eslint`, or similar CLI call in a script, a pre-commit hook, or an
  editor's save-triggered lint/format pass.
- Needing a bespoke per-tool daemon (eslint_d, dmypy) for each CLI
  separately — speedrunner's benchmarks show it beating those tools' own
  dedicated daemons too.

## Practical Uses
- `eval "$(speedrunner init zsh)"` in `~/.zshrc` so daemons for enrolled
  tools warm up passively while typing, then call tools normally.
- `speedrunner exec ./script.sh` to preload every tool a CI or build script
  calls, cutting a checks script's wall time by several times.
- Wrap a pre-commit hook (black/isort/flake8/mypy, or equivalent JS tooling)
  with `speedrunner exec` for a faster commit loop.
- `srr --native <cmd>` to force a one-off native run when a speedrunner
  fork's limitations (see Risks) would matter for that specific call.

## Product Opportunities
None identified — this is a personal/CI developer-productivity tool, not a
building block for a product.

## Agent / Automation Opportunities
Any coding agent that shells out to CLI tools repeatedly in a session (AWS
CLI calls, linters, test runners) inherits the same startup-cost savings by
running under speedrunner — a straightforward way to cut wall-clock time in
an agent's tool-calling loop without touching the agent itself.

## Integration
Single binary install on Linux (x86_64/aarch64) or macOS 11+ (Intel/Apple
Silicon); no per-tool setup, enroll with `speedrunner enroll <tool>` or run
ad hoc via `srr <cmd>`. **Integration effort: Low.**

## Architecture Notes
The core bet — most CLI tools in these ecosystems spend the bulk of a short
invocation on interpreter startup and import/require resolution rather than
actual work, so a warm forked process amortizes that cost — is a classic
"zygote process" pattern (the same idea behind Android's app-launch model
and Chrome's process spawning), applied generically across unrelated
language ecosystems via a single daemon-and-fork mechanism rather than one
per tool. The safety model is notable: any problem before a call has
started routes to the real tool natively, and once started the forked
process is never reused if anything about it is uncertain, trading
aggressiveness for correctness. A companion blog post ("fork fidelity")
documents exactly which parts of interpreter state a fork can't replicate
and how the tool compensates.

## Maturity
Very early — created 2026-10-29 (under 3 weeks before this review), 18
stars, 1 fork, 1 subscriber, latest tag v1.1.0 (2026-11-16). Thin traction
by star count, but backed by a real benchmark suite (96 commands measured,
VPS and Docker-based reproducible results checked into the repo) rather than
unverified marketing claims — the "hidden gem" pattern this radar
specifically looks for.

## License
Apache-2.0.

## Alternatives
Tool-specific daemons (eslint_d, dmypy) solve the same problem per tool and,
per speedrunner's own published benchmarks, are slower than speedrunner's
generic fork approach for the tools it was measured against. No
general-purpose, cross-ecosystem equivalent was found already catalogued
here.

## Risks / Limitations
- 18 stars / under 3 weeks old — no long-run track record for correctness
  across the full breadth of "hundreds" of claimed-compatible CLIs.
- A forked process cannot see certain things a tool's interpreter would
  normally reload per-invocation (documented in the "known limits" section
  and the linked fork-fidelity article) — worth reading before relying on it
  for anything environment-sensitive.
- Single maintainer (anthonyGosme) as of this review.

## Recommendation
USE NOW as today's Small but High-Leverage Utility — low effort, reversible
(`--native` escape hatch), and the published benchmarks make a concrete,
checkable claim rather than an unverifiable one. Worth enrolling the AWS
CLI and eslint specifically given Sourav's stack.

## Change History
### 2026-11-17
Initial discovery and review. Slot 7 (experimental projects and hidden gems)
run — today's Small but High-Leverage Utility.
