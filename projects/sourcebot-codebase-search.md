# Sourcebot

## Summary
Self-hosted, multi-repo code search and natural-language codebase Q&A tool. "Ask
Sourcebot" answers questions about a codebase using Sourcebot's own search/navigation
tools, with inline citations and navigable snippets; "Code Search" gives fast,
regex/boolean-capable search with IDE-level goto-definition and find-references across
every configured repo and branch, regardless of which host they live on.

## Why I Should Care
Reveals a capability — self-hosted, org-wide, agent-and-human code search with grounded
Q&A — that's otherwise only available as an expensive enterprise product (Sourcegraph)
or not at all for a small team's spread of repos across GitHub/GitLab/Bitbucket.

## Problems It Can Remove
Removes per-repo, per-IDE search as the only way to find something across a fleet of
services (MERN backends, a Next.js frontend, Terraform, a NestJS admin portal). Removes
the "which repo was that helper function in again" problem once there's more than a
couple of active repos.

## Practical Uses
- Index every active repo (personal projects, MSSP admin portal, client work) into one
  searchable instance.
- Ask natural-language questions about unfamiliar or legacy code and get an answer
  grounded in actual search results with citations, not a hallucinated summary.
- Use goto-definition/find-references across repo boundaries without cloning and
  opening each one locally.
- Point a coding agent at Sourcebot's search/Q&A surface as an additional context
  source for cross-repo questions.

## Product Opportunities
None specific to Sourav's product lines — this is internal engineering infrastructure.

## Agent / Automation Opportunities
"Ask Sourcebot" is itself agent-shaped (a reasoning model driving Sourcebot's search
and navigation tools to answer questions); the underlying search/nav capability could
in principle be exposed to an external coding agent, though no MCP server is
advertised in the README as of this review — confirm before assuming one exists.

## Integration
Self-hosted via Docker Compose: download `docker-compose.yml`, write a JSON config
file declaring connections (e.g. a GitHub org) and repos to index, update secrets, then
`docker compose up`. Visit `localhost:3000`. Medium integration effort — a real service
to run and configure, plus connecting an LLM provider to enable the Q&A feature, versus
a single-binary CLI tool.

## Architecture Notes
TypeScript throughout. Config-file-driven "connections" model (GitHub, and presumably
other hosts) for declaring what to index. Uses its own code-search and navigation
tooling as the retrieval layer that the Q&A feature's reasoning model calls into,
rather than naive embedding-and-stuff RAG.

## Maturity
Mature relative to this run's other candidates. v5.1.14 (frequent releases), 375
forks, 121 open issues (active backlog, not neglect), last push 2026-09-23. Two primary
human contributors plus bot-driven CI/release automation.

## License
**FSL-1.1-ALv2 (Functional Source License)** for the core — this is source-available,
not an OSI-approved open-source license, and is flagged loudly per policy. It converts
to Apache-2.0 automatically two years after each version's release. During that window
it permits self-hosting, internal use, and embedding, but restricts using the code to
build or offer a directly competing hosted code-search/Q&A product. A separate,
distinct license applies to the enterprise (`ee/`) directory — not fully inspected in
this review; treat as proprietary until confirmed via `ee/LICENSE`. Third-party
components retain their own original licenses.

## Alternatives
- Sourcegraph — far heavier, enterprise-oriented pricing and operational burden for a
  solo/small-team use case.
- `grep`/`ripgrep` across manually cloned repos — works, but no cross-repo semantic
  Q&A, navigation, or web UI.
- GitHub's own code search — cloud-only, doesn't index self-hosted or private-infra
  repos, and offers no local Q&A layer.

## Risks / Limitations
- FSL license must be understood before any commercial or hosted-derivative use; fine
  for internal self-hosted use, which is the intended use case here.
- Anonymous usage telemetry is enabled by default (`SOURCEBOT_TELEMETRY_DISABLED` to
  turn it off).
- Real infrastructure to run and maintain (Docker Compose stack, index storage, LLM
  provider config for the Q&A feature) versus a zero-infra CLI tool.
- 121 open issues is a meaningful backlog to be aware of before depending on it.

## Recommendation
PROTOTYPE — worth standing up once managing more than one or two active repos at a
time; the FSL license and Docker Compose deployment are the two things to weigh
before committing, but neither blocks the intended self-hosted/internal use case.

## Change History
### 2026-09-25
Discovered via GitHub topic search (`topic:code-search`) during slot 3 discovery.
Catalogued as PROTOTYPE, 7.8/10. License confirmed as FSL-1.1-ALv2 by reading
`LICENSE.md` directly (GitHub API reported `NOASSERTION`).
