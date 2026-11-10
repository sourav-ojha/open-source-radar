# Chronos

## Summary
Chronos is a zero-runtime-dependency Deterministic Simulation Testing (DST) framework for TypeScript/Node.js. It runs concurrent/distributed code on virtual time with a seeded `xoshiro256**` PRNG driving every random draw, network delay, and scheduling decision, injects a simulated chaos network (latency, drops, duplicate delivery, partitions, crash/restart), and intercepts `Date.now()`/`Math.random()`/`performance.now()`/timers via entropy guards — so any failure replays bit-for-bit from one integer seed. Inspired directly by FoundationDB and TigerBeetle's internal simulation-testing approach.

## Why I Should Care
Distributed race conditions, split-brain partitions, and timer bugs famously fail once in CI and refuse to reproduce locally, because wall clocks, thread scheduling, and socket I/O are non-deterministic. Chronos is the first TypeScript-native DST framework found in this radar — every prior credible DST implementation (k1LoW/detest, sb2bg/marionette, hmdsefi/faultline) targeted Go or Zig, requiring a language switch to use.

## Problems It Can Remove
- Removes guessing at a race condition from a stack trace — replay the exact failing seed instead.
- Removes building a chaos-network/fault-injection harness from scratch for a Node service.
- Removes trusting "it passed in CI" as proof a concurrent code path is actually race-free.

## Practical Uses
- Testing an SQS/BullMQ-style Node worker pipeline for race conditions under simulated partitions and crash/restart.
- Reproducing a flaky, non-deterministic CI failure deterministically from its seed.
- Seed-sweeping thousands of runs in CI to fuzz concurrent code paths (double-submits, race-y cache invalidation).
- Validating idempotency/retry logic in a distributed Node service before production.

## Product Opportunities
None directly — this is a testing tool, not an embeddable product component.

## Agent / Automation Opportunities
A coding agent could run a seed sweep as a pre-merge check on any newly-touched concurrent code path, surfacing a concrete failing seed + replay capsule instead of a vague "this might race" review comment.

## Integration
`npm install @sx4im/chronos-core @sx4im/chronos-vitest`; runs inside standard Vitest suites. Requires writing (or porting) code against Chronos's injected clock/network/randomness interfaces — not a drop-in for existing untouched code, the same constraint every DST framework shares. Integration effort: **Medium**.

## Architecture Notes
Seeded PRNG (`xoshiro256**` from `SplitMix64`) drives all non-determinism; virtual-time scheduler advances simulated time instantly to the next event; entropy guards intercept global time/random/timer calls (`route` mode by default, `throw` mode via `CHRONOS_STRICT=throw`); failing seeds are written as `.chronos/failures/<seed>.json` capsules for exact replay via `chronos replay`.

## Maturity
Experimental — created 2025-04-14 but still pre-1.0 (v0.3.0), 19 stars, 2 forks, 0 subscribers, 18 open issues. Real CI, a determinism-guard test, and published npm packages (`@sx4im/chronos-core` 0.3.0, `@sx4im/chronos-vitest`) confirm it's a working project, but adoption beyond the author is unproven.

## License
MIT, confirmed via GitHub API.

## Alternatives
k1LoW/detest (already catalogued — same pattern, Go); sb2bg/marionette (Zig, 97 stars); hmdsefi/faultline and lockstep (Go) — a same-week wave of DST frameworks, none TypeScript-native; TigerBeetle/FoundationDB's own internal DST tooling (not reusable outside their codebases).

## Risks / Limitations
- 0 subscribers, 18 open issues vs. 2 forks — real-world battle-testing beyond the author's own use is unproven.
- Adopting DST means writing new code against Chronos's abstractions from the start; retrofitting existing untested modules is real engineering work.
- Single maintainer, no visible community yet.

## Recommendation
PROTOTYPE — worth a small experiment on one concurrency-sensitive Node module (a queue consumer, a cache-invalidation path) before deciding whether to adopt more broadly; too early to rely on for anything production-critical.

## Change History
### 2026-11-10
Initial discovery and review. Slot 7 (experimental projects and hidden gems) run. Catalogued on architecture merit, following the same precedent as k1LoW/detest (catalogued 2026-11-04 at 0 stars) given the TypeScript-native gap in an otherwise Go/Zig-only DST wave this week.
