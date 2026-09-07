# Shelve

## Summary
Shelve is an open-source secrets and environment-variable management platform: an encrypted vault (AES-256 at rest, SHA-256 hashed), a CLI (`@shelve/cli`) that injects secrets without `.env` files (`shelve run`), GitHub Actions/repo-secrets sync via an official GitHub App, role-based team access, and a self-hostable server alongside the hosted shelve.cloud option.

## Why I Should Care
A narrow, well-executed take on a problem every multi-environment Node project has: scattered `.env` files passed around ad hoc. Decent CLI DX (command palette, `shelve run`), native GitHub Actions sync, and — unlike Doppler or 1Password — it's self-hostable.

## Problems It Can Remove
- Stops passing `.env` files over Slack/DM between teammates.
- Removes manually updating GitHub Actions/repo secrets per repo — GitHub App sync does it automatically.
- Removes the "which environment has which value" guesswork across dev/staging/prod.

## Practical Uses
- Centralize secrets for a MERN project across dev/staging/prod with one dashboard.
- `shelve pull`/`shelve push` in CI instead of manually maintained secret files.
- Time-limited secure secret sharing instead of pasting keys in DMs.

## Product Opportunities
Could be adopted as the shared secrets backbone across Sourav's own projects instead of paying for Doppler/Infisical Cloud, given Apache-2.0 licensing and self-hosting support.

## Agent / Automation Opportunities
None beyond standard CLI/API usage — no MCP server or explicit agent-facing tooling noted (contrast with Sigillo, reviewed the same day, which is built specifically around not leaking secrets into agent context).

## Integration
CLI install via npm (`@shelve/cli`), self-hosted server or hosted shelve.cloud. Integration effort: **Low**.

## Architecture Notes
Nuxt UI-based dashboard; GitHub App handles the repo-secrets sync path. Encryption: AES-256 at rest, SHA-256 hashing.

## Maturity
Emerging-to-mature. Created February 2024 (2.5 years old), 453 stars, 15 contributors, "Active Development & Production Ready" per its own status table. Latest CLI release `@shelve/cli@5.3.0` (2026-08-17), repo pushed as recently as 2026-09-03.

## License
Apache-2.0. No restrictions on commercial or SaaS use.

## Alternatives
Doppler, Infisical, 1Password for Developers, Vercel/Netlify built-in env management, Sigillo (self-hostable Doppler alternative reviewed the same day, Cloudflare-Workers-specific, agent-redaction-focused — see Worth Watching in the 2026-09-07 daily digest).

## Risks / Limitations
- README's own roadmap table lists "Next Release v2.5.0 (Target: Q4 2025)" while being reviewed in September 2026 — the docs appear to lag the code somewhat, though the CLI itself shipped v5.3.0 in August 2026 and the repo is actively pushed.
- Crowded space; differentiation is DX and self-hostability, not a unique capability.
- Verify the self-hosting docs and encryption-key management model before trusting it with production secrets.

## Recommendation
PROTOTYPE — solid, low-risk candidate to trial for centralizing secrets across personal/small-team projects; not yet reviewed hands-on for self-hosting friction.

## Change History
### 2026-09-07
Initial discovery and review. Slot 6 (infrastructure, observability, deployment) run.
