# Dopbase

## Summary
Dopbase is a self-hosted secrets manager that bundles the server, admin UI, REST API, and CLI into one single-file executable, backed by SQLite and a local master key, organizing secrets by project and environment. Its explicit design goal is working "without requiring a large supporting infrastructure stack."

## Why I Should Care
Vault and OpenBao are the mainstream self-hosted answer to secrets management, but both need their own HA storage backend, unseal process, and auth-method configuration before they're useful — a lot of ceremony for a solo developer or small team that just wants to stop scattering `.env` files across repos and CI runners. Dopbase's single-binary, single-data-directory design is a meaningfully smaller thing to operate.

## Problems It Can Remove
- Scattered, inconsistent `.env` files across repos, machines, and CI configs.
- Handing a human credential to a CI runner or agent instead of a scoped, revocable token.
- Ad hoc scripting to convert secrets between dotenv/JSON/YAML/TOML/Docker-env formats when migrating between tools.

## Practical Uses
- Centralize per-project, per-environment secrets instead of per-repo `.env` files.
- `dopbase run <env> -- <command>` injects secrets as environment variables into a child process without ever writing a shared `.env` to disk.
- Issue a `DOPBASE_TOKEN` scoped to a CI runner or an agent process instead of a shared human credential.
- Use OIDC SSO for the admin UI if already running an identity provider.

## Product Opportunities
A lightweight secrets backbone for a small agency or indie SaaS managing several client deployments, without standing up Vault/OpenBao's own supporting infrastructure for each one.

## Agent / Automation Opportunities
`DOPBASE_TOKEN` is explicitly documented as supporting both CI runners and AI agents — a scoped credential an agent process can hold instead of a broader one. No MCP server documented.

## Integration
Install script or release binaries for macOS/Linux (amd64/arm64). `dopbase server start` serves on `localhost:8840` by default, foreground or background. No official Docker image yet — the docs only mention exporting a Docker env file, so containerized deployment currently means wrapping the binary yourself. Low integration effort otherwise.

## Architecture Notes
Runtime data (SQLite database, config, master key file) lives under `~/.dopbase` by default, relocatable via `--data-dir` or `DOPBASE_DATA_DIR`. Secrets are stored encrypted, each as an individual record; backups are also encrypted. The README doesn't name specific algorithms or describe key management in detail — that's in the linked security docs, not the main page.

## Maturity
Experimental/emerging. Created ~2 months ago (Aug 2026), 165 stars, 11 forks, 1 subscriber, 4 contributors, 19 open issues — a high issue-to-star ratio typical of a very young project, but also a fast climb from 0 to 165 stars in under two months.

## License
Apache-2.0 — no restrictions on commercial or embedded use.

## Alternatives
- HashiCorp Vault / OpenBao — full-featured but require their own supporting infrastructure.
- Infisical — mainstream, already excluded by this radar's household-name policy.
- Doppler — hosted SaaS, not self-hostable.
- Plain `.env` files — the status quo this replaces.

## Risks / Limitations
- Very young, small (4-person) contributor base.
- No official Docker image — the install-script trust model deserves scrutiny before production use.
- Encryption algorithm specifics aren't named in the README; verify directly in source before trusting it with production secrets.

## Recommendation
PROTOTYPE — reasonable to trial for a low-stakes side project's secrets, but hold off on anything production-critical until the encryption/key-management details have been independently verified and the project has a few more months of track record.

## Change History
### 2026-10-26
First catalogued. Surfaced via a GitHub search for "secrets manager self-hosted" during slot 6.
