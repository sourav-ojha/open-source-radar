# goncordia

## Summary
A Go background-job queue that runs on whatever database the application already uses —
Postgres, MySQL, MariaDB, SQLite, MongoDB, Redis, Cassandra, ClickHouse, DynamoDB,
Firestore, or in-memory for tests — through one generic `Driver[TTx]` interface.
Implements the transactional-outbox pattern (`EnqueueTx`), typed job results you can
`Await`, retries, scheduling, priorities, and an embeddable admin UI with a Prometheus
endpoint.

## Why I Should Care
Covers the role Sidekiq/Celery play in Ruby/Python, for Go, with a MongoDB driver as a
first-class backend — directly relevant if any Go service sits in front of the existing
Mongo-based stack. The transactional-outbox implementation (`EnqueueTx` writes the job in
the same transaction as the business data, so it only becomes visible after commit) is
exactly the pattern that's otherwise easy to get wrong by hand with a dual-write race.

## Problems It Can Remove
Removes building: a retry/backoff engine, a transactional-outbox relay, cron-style
scheduling with leader election across instances, fair multi-queue scheduling, and an
admin dashboard for job visibility — for any background work (emails, webhooks, report
generation, external API sync) a Go service needs to defer.

## Practical Uses
- Enqueue a job inside the same DB transaction that creates the record it operates on,
  eliminating the "job fired but the transaction rolled back" class of bug.
- Run unit tests of enqueue/worker logic without Docker, using the in-memory driver.
- Use `Await`/`Call` to treat a background job like a function call when the caller
  needs the result, instead of building a separate polling/callback mechanism.
- Run it against MongoDB directly if a service's primary datastore is already Mongo,
  avoiding a second datastore just for job queuing.
- Use the embeddable admin UI and Prometheus endpoint instead of building job
  observability from scratch.

## Product Opportunities
A clean building block for any Go microservice added alongside the main Node/Mongo
stack (e.g., a data-processing or integration service) that needs reliable background
work without introducing Redis or a dedicated job-queue Postgres instance.

## Agent / Automation Opportunities
No AI-agent-specific surface; this is backend infrastructure. Relevant as an
architecture reference for implementing the transactional-outbox pattern correctly in
any stack, including Node/Mongo.

## Integration
Go library (`go get`), no separate service to run beyond whatever database backend is
chosen. MongoDB backend requires a replica set (a MongoDB requirement for
transactions/change streams, not goncordia-specific). Integration effort: Low for a Go
codebase already on one of the supported databases; Not applicable for a pure Node
stack, where it would only matter if introducing a Go service.

## Architecture Notes
The `Driver[TTx]` interface is parameterized by the native transaction type of whatever
database library is in use (`pgx.Tx`, `*sql.Tx`, `*gorm.DB`, `bun.Tx`, `*mongo.Session`),
so there's no adapter-layer indirection between the client/worker and storage. Push
notifications use each backend's native mechanism (Postgres LISTEN/NOTIFY, MongoDB
Change Streams, Redis Pub/Sub) with polling fallback elsewhere, and non-transactional
backends (Redis, Cassandra, ClickHouse, DynamoDB) fall back to at-least-once delivery
instead of claiming atomicity they can't provide. The README's own comparison table
against River, Asynq, and Machinery is specific and checks out against those projects'
documented scope.

## Maturity
Emerging: created May 2026, already at v2.7.2 with CI, Codecov, and Go-reference docs in
place. Essentially a solo project (`kirimatt`, 193 commits) with Claude-assisted commits
(10) and Dependabot — low community traction (70 stars, 0 subscribers) but a
well-documented, tested codebase with real version history, not a weekend project.

## License
MIT. No restrictions on commercial use or embedding.

## Alternatives
- **River** — Postgres-only, more mature, has a polished UI and a paid Pro tier; the
  better choice if already committed to Postgres-only and want a more battle-tested
  option.
- **Asynq** — Redis-only, mature, similar scope but no transactional-outbox guarantee.
- **Machinery** — broader backend support (Redis, RabbitMQ, SQS, MongoDB) and workflow
  primitives (chains/chords) goncordia lacks, but no transactional insert and a less
  active project.
- Hand-rolled outbox table + poller — what this replaces for teams not ready to adopt a
  dependency.

## Risks / Limitations
- Single maintainer — bus-factor risk, and a young project (6 months old) despite rapid
  version iteration.
- 0 subscribers on the repo is a weak community-health signal; watch for whether it
  attracts other contributors or stalls.
- MongoDB driver requires a replica set, which adds an operational requirement beyond a
  single-node Mongo deployment some smaller setups might be running.

## Recommendation
PROTOTYPE — the transactional-outbox design and native MongoDB support make it worth
trying in a Go side-service before relying on it for anything load-bearing; River remains
the safer default for a pure-Postgres Go service that doesn't need the multi-backend
flexibility.

## Change History
### 2026-11-19
Discovered via GitHub topic search (`topic:job-queue`) during slot 2 (product
infrastructure, APIs, backend components) discovery. Catalogued as PROTOTYPE, 8.1/10.
