# AccessFlow

## Summary
Open-source database, API, and deployment access-governance platform. Sits as a full
query proxy in front of relational databases (Postgres, MySQL, MariaDB, Oracle, SQL
Server), NoSQL stores (MongoDB, Couchbase, Redis, Cassandra, Elasticsearch, DynamoDB,
Neo4j), and cloud warehouses (Snowflake, BigQuery, Databricks), plus outbound REST/SOAP/
GraphQL/gRPC calls — routing every query through configurable AI risk-scoring, policy-
as-code routing, and human approval before it touches production.

## Why I Should Care
This is the "governed self-service database access" layer that MSSP-style and
compliance-sensitive products need and otherwise build piecemeal (a query review queue,
an audit log, row-level masking, break-glass access) over months. It directly serves
Sourav's MSSP admin-portal/PTaaS secondary context: access governance, audit trails, and
SSO/SCIM are exactly what that kind of product has to offer customers, and building this
from scratch is a multi-month effort this replaces.

## Problems It Can Remove
Removes building: a query-approval workflow, per-column data masking, row-level
security enforcement injected into arbitrary SQL, a tamper-evident audit log, JIT
(just-in-time) temporary access grants, break-glass emergency access with mandatory
retro-review, and SSO/SCIM provisioning — all as a standalone layer in front of
databases the team doesn't want to hand out raw credentials to.

## Practical Uses
- Replace "shared production credentials" or "ticket-driven DBA access" with a
  self-service, reviewable query path for an internal team or an MSSP's analysts.
- Gate a CI/CD deployment step behind the same approval engine used for database access.
- Apply per-column masking (PII/PCI/PHI) automatically once a column is tagged, without
  rewriting every query that touches it.
- Use per-user data-volume budgets to catch slow data exfiltration that per-query row
  caps can't.
- Stand up a governed API proxy for outbound REST/GraphQL/gRPC calls that need the same
  review-before-execute treatment as a database write.

## Product Opportunities
Could be deployed directly as the access-governance backbone inside the existing MSSP
admin-portal SaaS, or repackaged as a narrower managed offering (e.g., "governed Postgres
access for your support team") aimed at smaller teams that can't justify a Teleport/
strongDM contract.

## Agent / Automation Opportunities
AI risk-scoring step is itself pluggable (OpenAI, Anthropic, local Ollama, or any
OpenAI-compatible endpoint) with multi-model voting and guardrails against prompt
injection in the submitted query text — relevant architecture to study for any product
that puts an LLM between a user action and a privileged operation. Text-to-query and an
in-app documentation-grounded help assistant are also AI-agent-shaped features worth
reading the implementation of.

## Integration
Self-hosted via Docker; backend is Java (Maven/`pom.xml`, Snyk-monitored). Needs its own
Postgres for metadata plus whatever databases it proxies to. Supports SAML 2.0 SSO,
OAuth2/OIDC, and SCIM 2.0 provisioning out of the box. Integration effort: Medium-High —
this is a full platform deployment (proxy + admin UI + approval engine), not a library
drop-in, and the JVM stack is outside Sourav's primary Node toolchain even though it's
Docker-deployable.

## Architecture Notes
Every query — whatever dialect — is parsed, classified, optionally AI-scored, and routed
through policy-as-code rules (attribute-based conditions combined with AND/OR/NOT)
before approval. Notable design choices: an external policy-decision hook (point it at
your own OPA sidecar) that can only tighten a decision, never approve past what routing
already allows; row-level security implemented as injected, parameterized predicates
rather than string concatenation; and a policy simulator that dry-runs a draft policy
against historical traffic before it's saved. The issue-number references scattered
through the README (`#945`, `#860`, `AF-624`) point to a real internal tracker behind the
feature list, not marketing copy.

## Maturity
Emerging but surprisingly deep: 2,143 commits, almost entirely from a single maintainer
(`babltiga`, ~1,918 commits) plus a handful of smaller contributors and Dependabot. Only
13 stars and 1 subscriber at review time — this is a case where feature depth and commit
history substantially outrun visible community traction. Treat as a competent
single-maintainer project rather than a team-backed platform; bus-factor risk is real.

## License
Apache-2.0. No restrictions on commercial use, self-hosting, or embedding — permissive
enough to deploy inside a commercial MSSP product without licensing concerns.

## Alternatives
- **Teleport**, **strongDM**, **Satori**, **Cyral** — commercial, often closed-source or
  open-core-with-paywalled-governance-features products covering similar ground.
- Manual ticket-driven DBA access review — the slow, people-dependent process this
  replaces.
- Shared production credentials with no review — the fast, unsafe default this targets.
AccessFlow is one of very few fully Apache-2.0, self-hostable implementations of this
governed-proxy pattern; most comparable tools gate the interesting governance features
behind a commercial tier.

## Risks / Limitations
- Single-maintainer project — verify long-term maintenance commitment before depending
  on it for a production access-control boundary.
- 38 open issues against only 13 stars is a high ratio; worth checking issue triage
  cadence before relying on recent feature claims.
- JVM backend adds an unfamiliar runtime to Sourav's primarily Node/Mongo stack, though
  Docker deployment limits the day-to-day operational cost of that.
- A platform this feature-dense, built almost solo, warrants closer-than-usual review of
  the actual masking/row-security code paths before trusting it with real PII.

## Recommendation
PROTOTYPE — the architecture directly matches a real need in the MSSP/PTaaS secondary
context and is unusually complete for its star count, but the single-maintainer risk and
unfamiliar JVM stack mean it should be piloted on non-critical data before any production
access-control decision depends on it.

## Change History
### 2026-11-19
Discovered via GitHub topic search (`topic:audit-log`) during slot 2 (product
infrastructure, APIs, backend components) discovery. Catalogued as PROTOTYPE, 8.3/10.
