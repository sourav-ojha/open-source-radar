# CATS

## Summary
A REST API fuzzer and negative-testing tool for OpenAPI endpoints from Endava: point it at an OpenAPI 3.0/3.1 contract and a running server, and it generates, runs, and reports thousands of boundary, type-coercion, injection, and stateful negative-path tests (IDOR, PUT idempotency, update-read consistency, conditional ETag handling, deleted-resource availability) with zero test code written by hand.

## Why I Should Care
Negative-path API testing — what happens when a field is empty, a type is wrong, a header is malicious, a resource was already deleted — is exactly the kind of testing that gets skipped under deadline pressure, and CATS automates the whole category from the OpenAPI spec alone. It's "self-healing": when the spec changes, the generated tests change with it, so there's no hand-maintained negative-test suite going stale.

## Problems It Can Remove
- Hand-writing negative/boundary test cases per endpoint (160+ built-in fuzzers cover this out of the box).
- The usual gap where positive-path tests pass but nobody checks what the API does with malformed input, oversized payloads, or already-consumed resources.
- Negative-test maintenance burden when the API contract evolves (tests regenerate from the spec).

## Practical Uses
- Run `cats -c openapi.yml -s http://localhost:8080 --profile security` against a Node/Express or NestJS service in CI as a quality gate (`--qualityGate "errors<1"`).
- Use `--profile quick` for a fast pre-merge check, and the full suite overnight or pre-release.
- `cats lint -c openapi.yml` to catch OpenAPI contract issues before they reach fuzzing.
- `cats replay Test12 --curl` to reproduce a specific finding as a curl command for a bug report.
- `--tui` to watch and filter a long-running fuzz pass interactively instead of scrolling CI logs.

## Product Opportunities
Not a product-building block itself, but a quality gate worth wiring into any API-first product's CI before a paid customer hits an edge case first.

## Agent / Automation Opportunities
Deterministic CLI with a `--qualityGate` exit-code gate and JSON/HTML reports — straightforward to wire as a CI step or hand to a coding agent as a "run negative tests and summarize failures" tool. No MCP server yet, but the structured report format would wrap cleanly.

## Integration
Homebrew (`brew install cats`) or a native binary / uberjar (Java 25+ needed only for the jar path) — no code changes to the target API required, just an OpenAPI spec and a reachable server. Runs standalone in CI with no other infrastructure.

## Architecture Notes
Orders producer `POST` paths before dependent operations, executes fuzzers in breadth-first waves, reuses identifiers learned from `2XX` responses across requests, and postpones `HappyPath DELETE` calls until the end of a run — this dependency-aware sequencing is what makes zero-config fuzzing of a real multi-endpoint API feasible instead of just throwing garbage at one endpoint at a time.

## Maturity
Mature: created 2020, still actively released (v15.3.0 shipped the same day as this review), 1,378 stars, 93 forks, 11 subscribers, Apache-2.0, backed by Endava (a real engineering consultancy). Has a documented track record of finding real bugs in HashiCorp Vault and Gitea.

## License
Apache-2.0, verified via GitHub API. No commercial-use restrictions.

## Alternatives
Schemathesis and EvoMaster (both previously time-boxed out of this catalog as mature-but-generic fuzzers with no angle specific enough to this profile) cover similar ground; CATS differentiates with its self-healing zero-code design, built-in security/compliance profiles, and a TUI for inspecting long runs interactively. Dredd and manual Postman/Newman negative-test collections are the lower-effort, lower-coverage alternative most teams default to instead.

## Risks / Limitations
Java-based (uberjar path needs Java 25+; native binaries avoid this but there's no native Windows binary, only the jar). Six years old but low subscriber count (11) relative to stars suggests a small core maintainer base — worth checking issue responsiveness before leaning on it for release-blocking CI.

## Recommendation
USE NOW — low integration cost (point it at an existing OpenAPI spec), mature and actively maintained, and it automates a testing category that's otherwise manual or skipped.

## Change History
### 2026-10-30
Initial discovery and review.
