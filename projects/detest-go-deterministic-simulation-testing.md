# detest

## Summary
An in-process Deterministic Simulation Testing (DST) framework for Go, built on `testing/synctest`. It systematically explores goroutine interleavings using stateless model checking to find and deterministically reproduce concurrency bugs — races, deadlocks, lost updates — inside ordinary Go test files, running unchanged production code (including GORM, sqlx, sqlc) above simulated client boundaries (`database/sql`, `http.RoundTripper`, message queues, mutexes).

## Why I Should Care
Techniques like this are proven inside FoundationDB and TigerBeetle but almost never available as a reusable library — most teams that want this level of rigor build a bespoke simulation harness from scratch. detest packages the technique for ordinary Go application code, which makes it a genuinely rare example of DST being accessible rather than a research artifact.

## Problems It Can Remove
Replaces ad hoc stress-testing loops (`go test -race -count=1000`) that may or may not hit the bad interleaving, and replaces hours spent manually constructing a reproduction script for a race once it is found in production. `s.ExpectViolation` lets a known bug stay pinned as a regression test until fixed.

## Practical Uses
- Reproduce a race between two concurrent handlers overselling the same inventory row before shipping a fix.
- Exercise PostgreSQL/MySQL locking and isolation-level edge cases (deadlocks, gap locks) without a live database in CI, using the built-in `postgres.New()`/`mysql.New()` fidelity simulators.
- Simulate flaky external HTTP calls and message-queue delivery to deterministically test retry/idempotency logic.
- Use `DETEST_REPLAY` to deterministically rerun a found counterexample schedule.

## Product Opportunities
None directly — this is a testing technique, not a product component.

## Agent / Automation Opportunities
Ships an AI-agent skill entry point (`gh skill` / `npx skills`) where an agent proposes simulation targets, writes the scenario, and asks for decisions on invariants and run bounds — a notable choice to make the agent workflow the primary intended usage path rather than a side feature.

## Integration
`go get` plus a `detest` build tag on test files (excluded from ordinary `go test ./...`). Requires Go 1.26+. Low effort to add to an existing Go codebase; zero relevance to Node/Mongo codebases directly.

## Architecture Notes
Rather than virtualizing OS threads or the network, detest simulates at client-library boundaries — every operation on a simulated resource is a scheduler switch point. Exploration strategies include depth-first, iterative bounding, random, and PCT-style prioritized search, with partial-order reduction to skip equivalent orderings and checkpointing to resume long runs. This boundary-level simulation (vs. full black-box cluster testing like Jepsen) is the core architectural idea worth studying even outside Go: it keeps production code paths real while making only the external world deterministic and controllable.

## Maturity
Experimental. Created 2026-10-02, 0 stars, 2 forks, 0 subscribers, 4 open issues at review time — essentially zero adoption signal. Catalogued on architecture merit and the author's track record (k1LoW maintains several well-regarded Go tools: tbls, runn, octocov) rather than traction. Many SQL features (recursive CTEs, RIGHT/FULL joins, most aggregates, JSON operators, non-Read-Committed isolation levels) are explicitly unsupported and fail loudly rather than being approximated.

## License
MIT. No restrictions on commercial use.

## Alternatives
- `go test -race` — only catches races on the schedule that happens to execute, not systematically.
- Jepsen — black-box, cluster-level fault injection; proven but heavyweight, Clojure-based, and aimed at distributed systems rather than in-process application code.
- Bespoke simulation harnesses (TigerBeetle/FoundationDB style) — powerful but built once per project, not reusable.

## Risks / Limitations
- Scope limited to simulated boundaries: pgx's native API, vendor SDKs, SQLite, MongoDB, and Redis are not covered — `go test -race` is still needed for everything else.
- Requires Go 1.26+, directly inapplicable to this profile's primary Node/Mongo stack.
- Near-zero adoption and no version/benchmark history to judge real-world robustness.
- Exploration cost grows quickly; README recommends raising `GOGC` and bounding search depth.

## Recommendation
STUDY — the boundary-level DST pattern is worth understanding and potentially re-applying (even informally) to Node services with concurrent state, but direct adoption only makes sense if/when a Go service with real concurrency risk is in play.

## Change History
### 2026-10-27
First reviewed. Surfaced via a "deterministic simulation testing" GitHub README search during slot 7 (experimental/hidden gems).
