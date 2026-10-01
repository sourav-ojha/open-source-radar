# OpenFGA

## Summary
A high-performance, flexible authorization/permission engine inspired by Google Zanzibar. Developers model access control as relationship tuples (principal-action-resource, plus derived roles/conditions for ABAC) and query a stateless Policy Decision Point over HTTP or gRPC. Ships SDKs for Node.js, Java, Go, Python, .NET, a CLI, a Terraform provider, and an in-browser playground; supports in-memory, PostgreSQL, MySQL, and SQLite (beta) storage.

## Why I Should Care
Authorization logic is a product-building block that sprawls across controllers and grows inconsistent as a product's permission model gets more granular. OpenFGA centralizes it, and it's directly applicable to the MSSP admin-portal SaaS where per-tenant, per-resource permissions matter.

## Problems It Can Remove
Replaces scattered `if (user.role === 'admin')` checks across controllers with centralized, testable policy. Removes the need to rewrite a permissions schema every time the access model needs to express relationships ("can view if a member of the team that owns this document") rather than flat roles.

## Practical Uses
- Fine-grained, per-resource permissions for a multi-tenant SaaS admin portal
- Replacing ad hoc role checks with centralized, auditable policy
- Modeling ReBAC (relationship-based access control) that a flat roles table can't express cleanly
- Policy-as-code via the Terraform provider for access control reviewed in pull requests

## Product Opportunities
Could be embedded as the authorization layer inside a commercial product to offer customers real per-object sharing/permissions — a feature that's expensive to retrofit after the fact.

## Agent / Automation Opportunities
Not agent-specific, but relevant as the authorization layer behind an MCP server or agent tool that needs to enforce per-user/per-resource access before executing an action.

## Integration
Docker, precompiled binary, Helm chart, or embed as a Go library. SDKs available for Node.js and other languages for the client side. Integration effort: **Medium** — relationship-tuple modeling has a real learning curve, and running it means operating a separate service with its own datastore.

## Architecture Notes
Zanzibar-style: a stateless Policy Decision Point evaluates relationship tuples against an authorization model. Default in-memory store is explicitly non-persistent per the README; production requires wiring Postgres or MySQL.

## Maturity
Mature. Created June 2022, 5,892 stars, 511 forks, 217 open issues, CNCF Sandbox project. README lists production adopters: Auth0, Grafana Labs, Canonical, Docker, Agicap, Read.AI.

## License
Apache-2.0, verified via GitHub API. Fully permissive — no restrictions on commercial use, SaaS deployment, or embedding.

## Alternatives
- **Cerbos** (close competitor, reviewed alongside this) — Apache-2.0 core Policy Decision Point with YAML policies, arguably easier to author than OpenFGA's relationship-tuple model, plus an optional proprietary Cerbos Hub control plane. OpenFGA has the stronger adoption signal and CNCF backing.
- **Keycloak/Auth0 built-in authorization** — heavier, bundles identity and authorization together; OpenFGA is authorization-only and composes with any identity provider.
- **A hand-rolled roles column** — the default for most solo/small-team projects; works until permissions need to express relationships or per-object sharing.

## Risks / Limitations
- Relationship-tuple modeling has a real learning curve versus a simple roles table.
- Running it is a separate service with its own datastore and deploy — an operational dependency to weigh against the value for a small product.
- Default in-memory store is non-persistent; production requires Postgres/MySQL from the start.

## Recommendation
**PROTOTYPE** — mature enough to trial directly for the MSSP admin-portal SaaS's fine-grained permission needs.

## Change History
### 2026-10-01
Initial discovery and review, alongside Cerbos for competitive context. Found via GitHub Search API authorization/RBAC query, slot 2 (product infrastructure) run.
