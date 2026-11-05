# Casdoor

## Summary
Casdoor is a self-hosted identity and access management (IAM) / SSO server: OAuth2, OIDC, SAML, CAS, LDAP, SCIM, WebAuthn/passkeys and MFA behind one web UI. Six years old, Apache-2.0, actively released on a roughly weekly cadence. It now describes itself as "Agent-first IAM / LLM MCP & agent gateway" — a built-in MCP and agent gateway sits on top of the same identity/policy engine used for human SSO.

## Why I Should Care
Any product sold to a second enterprise customer eventually needs real SSO/SAML/LDAP support. Casdoor removes that build entirely, and the new agent-gateway layer means the same RBAC/policy engine that governs human dashboard logins can also govern what an AI agent is allowed to touch — exactly the overlap between the auth axis and the AI-agent-leverage axis this radar is built to catch.

## Problems It Can Remove
- A hand-rolled Passport.js/custom OAuth server for a new product.
- Building SAML/LDAP/SCIM support from scratch when an enterprise customer demands it.
- A second, separate auth layer just for agent tool calls — Casdoor's agent gateway can reuse the existing human IdP/RBAC.

## Practical Uses
- Central SSO/IdP for the MSSP admin-portal SaaS and any other internal tool.
- RBAC/multi-tenant user management for a new product.
- One OIDC provider that both humans and agents authenticate against.
- LDAP/SAML bridge for enterprise customers who require it.

## Product Opportunities
White-label SSO/IdP layer for the MSSP admin-portal product, so enterprise customers get SAML/LDAP support without Sourav building it himself.

## Agent / Automation Opportunities
Built-in MCP & agent gateway lets an agent authenticate through the same OIDC flow as a human user rather than a static API key. Could front other self-hosted MCP servers with Casdoor's existing RBAC instead of standing up a second auth layer, which is the pattern narrower tools like Issuerd and keypaste implement standalone.

## Integration
Docker/Docker Compose or a single binary, Postgres/MySQL backend, one YAML config file. Community Helm charts exist for Kubernetes. Integration effort: **Low** — deploying it is straightforward; the real work is migrating existing auth flows to point at it.

## Architecture Notes
Go backend + React frontend, long-running multi-tenant design (organizations/applications model). The "agent-first" positioning is a recent addition on top of years of SSO/IdP hardening — worth treating the agent-gateway code path as the newer, less-proven part relative to the core IAM functionality.

## Maturity
Mature. Created 2020-10-22, 14,762 stars, pushed same-day, current release v4.25.0 (2026-10-30). Long production track record in the self-hosted-auth space.

## License
Apache-2.0, confirmed via GitHub API license field. No restrictions on commercial use, SaaS deployment, or embedding.

## Alternatives
- **Keycloak** — already assumed known per AGENT.md's household-name policy; heavier JVM footprint.
- **Logto** — lighter weight; Casdoor has broader protocol coverage (CAS, LDAP, SCIM) plus the agent-gateway angle.
- **Authup, Issuerd** (already catalogued) — narrower, agent/DPoP-focused IAM servers; Casdoor is the more general-purpose, longer-lived SSO product.

## Risks / Limitations
- Very broad feature surface — adopt only the pieces actually needed.
- The agent-gateway feature is new; verify it independently before depending on it, separate from the mature core IAM.
- Go/React codebase, not an embeddable Node library — it's a standalone service, not a package you import.

## Recommendation
**USE NOW** — low-risk, mature, Apache-2.0, and solves a real recurring need (SSO/IdP) while adding a genuinely novel agent-gateway angle worth testing.

## Change History
### 2026-11-05
First catalogued. Discovered via GitHub search while reviewing slot 2 (product infrastructure / APIs / backend components). The project itself has existed for 6 years; the "agent-first IAM / MCP & agent gateway" repositioning is the specific trigger for review this run.
