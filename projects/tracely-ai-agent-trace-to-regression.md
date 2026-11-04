# Tracely

## Summary
A self-hosted, trace-native CI/CD pipeline for AI agents: every production agent trace is
graded as it lands, failures are clustered into issues, the failing run is frozen into a
hermetic, replayable regression case, and a PR that would reintroduce the same failure is
blocked in CI — with the production trace itself standing in for a hand-authored eval
dataset.

## Why I Should Care
Every agent-eval tool in this catalog so far (and most outside it) asks for a
hand-authored dataset of questions and ideal answers — a guess about what might break, that
goes stale as the product changes. Tracely's framing — "the recorded run is the test" — 
turns an agent's actual production failures directly into regression tests with no
dataset-authoring step, which is a meaningfully different and lower-effort workflow if it
holds up in practice.

## Problems It Can Remove
- Hand-authoring and maintaining an eval dataset for an LLM-agent feature.
- Building a custom "did this agent regress" CI gate from scratch on top of raw traces.
- The gap between "we have observability dashboards" and "we automatically stop shipping
  the same agent failure twice."

## Practical Uses
- Self-host the full stack (API, worker, UI, Postgres, ClickHouse, Redis, MinIO) via the
  one-click Railway/Railyard deploy templates, or `docker compose` locally.
- Instrument an agent with the `tracelyai-sdk` Python SDK (OTLP export) to start grading
  traces with no hand-written test cases.
- Use a frozen failure case as a CI gate so a regression that was already fixed once in
  production can't silently ship again.
- Feed the agent-skill integration so a coding agent itself can query Tracely during
  development.

## Product Opportunities
The "production trace becomes the regression test" pattern is reusable architecture for any
internal agent-powered feature (support bot, internal ops agent) without building bespoke
eval infrastructure first.

## Agent / Automation Opportunities
PyPI SDK (`tracelyai-sdk`) does automatic + manual agent tracing over OTLP; an agent-skill
integration lets a coding agent query Tracely directly. OpenTelemetry-based instrumentation
means it composes with existing OTel pipelines rather than requiring a separate one.

## Integration
Self-hosted only; the README's one-click deploy covers the full multi-service stack
(Postgres, ClickHouse, Redis, MinIO, API, worker, UI) via Railway or Railyard templates.
Integration effort: Medium — this is real infrastructure to run (five services), not a
single binary, though the one-click template removes most manual setup.

## Architecture Notes
Pipeline is explicitly: production trace -> failure detection -> regression test -> CI gate
-> alert, with "everything else derived from the trace" (quality scores, failure clusters,
suggested fixes, CI verdicts, trends) rather than from a separately maintained dataset.
ClickHouse for trace-volume analytics, Postgres for operational state, MinIO for artifact
storage (frozen trace payloads) and Redis for queuing — a conventional but reasonable
architecture for this scale of data.

## Maturity
Experimental / pre-release. Created 2026-10-11 (24 days old), 1,214 stars / 141 forks / 3
subscribers, 21 open issues. No tagged GitHub release exists yet (the PyPI SDK is at
0.0.1) — flagged per this catalog's own quality-filter guidance on repos with no releases.
The stars:subscribers ratio (~405:1) is the highest of today's four finds and is flagged
loudly; the forks (141, ~11.6% of stars) and open-issue count are real engagement signals
that a pure star-inflation case typically lacks, but this one gets a more cautious read than
Tracekit specifically because of the missing release tag.

## License
MIT. No restrictions.

## Alternatives
whatbroke (already catalogued — AI-agent behavioral diffing between runs, a narrower
before/after comparison rather than a full trace-to-CI-gate pipeline); hand-authored eval
suites with a framework like promptfoo/Braintrust-style tooling (not catalogued here, more
mainstream); plain OpenTelemetry dashboards (observability without the regression-test
step).

## Risks / Limitations
No tagged release yet, pre-1.0, single-digit subscriber count on a comparatively fast-growing
star count, and a genuinely heavier self-hosted footprint (five services) than most entries
in this catalog's AI-agent-infrastructure category. Re-check for a tagged 1.0 release and
independent adoption signal before relying on it for anything beyond a trial.

## Recommendation
PROTOTYPE, held to a trial rather than production use — stand up the Railway template
against one real agent integration's traces and judge whether the automatic failure
clustering produces regression cases worth keeping before going further.

## Change History
### 2026-11-04
Discovered via GitHub search (`mcp observability` keyword sweep, slot 1 rotation). First
catalog entry.
