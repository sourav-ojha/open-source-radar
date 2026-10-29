# Issuerd

## Summary
A single-binary Identity and Access Management server in Rust implementing OpenID Connect and OAuth 2.x, deliberately API- and model-compatible with Keycloak (realms, clients, roles, client scopes, protocol mappers, Admin REST API). OpenID Certified for 11 OP profiles. Ships SSO, MFA/passkeys, LDAP/Kerberos federation, an embedded admin console, and — distinctively — first-class support for the agentic-access standards (DPoP, token exchange, CIBA) that bearer-token IAM was never designed for.

## Why I Should Care
Keycloak is the default self-hosted IAM choice but carries a JVM/Quarkus operational footprint (memory, startup time, Infinispan clustering) that's heavy for a small product team. Issuerd mirrors Keycloak's domain model closely enough that existing OIDC clients and Keycloak-shaped operational knowledge transfer directly, while running as one static Rust binary with stateless, horizontally scalable token validation. The "Built for the Agent Era" section is the real differentiator: RFC 9449 DPoP (tokens cryptographically bound to the holder's key, useless if stolen from a log or a compromised MCP server), RFC 8693 token exchange (each MCP tool call gets a fresh, audience- and scope-narrowed token instead of forwarding the user's login token), and CIBA (out-of-band human approval for privileged actions like a refund, with a binding message). This is precisely the auth-hardening problem that shows up the moment an AI agent gets write access to real systems — directly relevant to his MCP/agent work and to the MSSP/PTaaS access-control context.

## Problems It Can Remove
Running and tuning a JVM-based Keycloak cluster for a product that needs standard OIDC/SSO; hand-rolling scope-attenuation and step-up-approval logic for agent tool calls that touch sensitive data or money; building LDAP/AD federation and SSO from scratch for an internal admin portal.

## Practical Uses
- Self-hosted OIDC/SSO provider for a SaaS product's customer-facing and admin-portal auth, as a lighter Keycloak replacement.
- Issuing DPoP-bound, scope-narrowed tokens to MCP servers or agent tool calls so a leaked token or compromised agent session can't be replayed elsewhere.
- CIBA-gated human approval for an agent-initiated privileged action (a refund, a destructive admin operation) before a step-up token is minted.
- LDAP/Active Directory federation for an internal admin panel without standing up Keycloak.
- Drop-in realm/client/role migration path from an existing Keycloak deployment, validated by the dual-target test suite that runs the same assertions against Issuerd and a live Keycloak 24.0.

## Product Opportunities
Could underpin auth for a self-hosted or on-prem deployment of a product where Keycloak's resource footprint is a deployment blocker (e.g., an MSSP admin-portal appliance sold to a customer's own infra); the agentic-auth primitives (DPoP + token exchange + CIBA) are a sellable differentiator for any product exposing AI-agent access to customer data.

## Agent / Automation Opportunities
This *is* agent infrastructure: it's the authorization layer an MCP server or agent fleet would sit behind. The documented MCP + DPoP + token-exchange walkthrough and the CIBA step-up demo (scripted chat agent against a live rig with PostgreSQL row-level security, a replayed stolen token rejected with 401, an unapproved refund rejected with 403) are a concrete pattern to copy for any product letting an agent act with real authority.

## Integration
Single statically-linked Rust binary (MSRV 1.95), Docker image on Docker Hub, crates.io package, declarative YAML provisioning for realms/clients. Cluster state lives in PostgreSQL (signing keys, shared cluster-wide) and Redis (transient coordination) — no sticky sessions, any number of nodes behind any load balancer. Embedded React admin and account consoles served same-origin; Prometheus metrics and health/readiness probes included.

## Architecture Notes
~155k lines of Rust across 9 tightly-scoped crates with a strictly acyclic dependency graph; async I/O on Tokio; authentication state transitions proven at compile time via typestate markers (illegal state transitions are compile errors, not runtime checks); access-token validation is stateless with no database lookup on the hot path. Protocol parity with Keycloak is continuously diff-tested against a live Keycloak 24.0 container rather than asserted from documentation, with deliberate differences tracked in `tests/KEYCLOAK_DIFFS.md`.

## Maturity
Experimental/emerging. Created 2026-09-23 (~5 weeks old at review), v0.1.24 as of 2026-10-25, 70 stars / 7 forks / 14 open issues — a healthy issue-to-star ratio suggesting real usage rather than inflated engagement. The OpenID Certified claim (v0.1.17 for 10 OP profiles, v0.1.19 adds RP-Initiated Logout) was independently verified against the OpenID Foundation's own certification registry, which lists "issuerd" — this is not a self-reported claim. 3,460+ automated tests and a self-hosted conformance run reporting 31,000+ conditions with zero failures are claimed but not independently re-run by this review. Despite the engineering rigor, treat as pre-1.0 and very young — no SAML, UMA, FGAP, or Organizations support yet (explicitly out of scope for now).

## License
Apache-2.0 — unrestricted for commercial use, self-hosting, and embedding.

## Alternatives
Keycloak (the household-name incumbent, JVM-based, heavier ops footprint); Ory Hydra (OpenID Certified OAuth2/OIDC server, but deliberately delegates login/consent to a separate app rather than shipping a full IAM surface); Authelia (OIDC provider still beta per its own docs); ArkGravity/burrow (see below — a simpler, Casdoor-inspired alternative with a smaller feature surface and no DPoP/CIBA).

## Risks / Limitations
Very new project (one month old) from a small/solo team (0 subscribers on the repo) — the OpenID certification and test-count claims are verifiable and were spot-checked, but production track record is minimal. No SAML/UMA/FGAP/Organizations. Horizontal scaling and the agentic-auth flows are demonstrated in docs/demos but not yet validated by this review under real production load.

## Recommendation
PROTOTYPE. The combination of verified OpenID certification, a Rust single-binary operational profile, and genuinely novel first-class support for DPoP/token-exchange/CIBA aimed at AI-agent access makes this worth a real trial — particularly for securing MCP server or agent-tool access to anything sensitive — but its one-month age means it should not yet replace a production Keycloak deployment outright.

## Change History
### 2026-10-29
Initial discovery and review. Slot 2 run (product infrastructure, APIs, backend components). Found via GitHub Search API `topic:rbac created:>2026-07-01`. OpenID Foundation certification claim independently verified against openid.net's certification registry.
