# Drydock

## Summary
Self-hosted container image update watcher: monitors 23 container registries, fires 20 notification/action providers, and ships an audit log, OIDC auth, Prometheus metrics, and a web dashboard for reviewing and applying updates — in a single Docker container.

## Why I Should Care
Container image updates (including security patches) land in registries silently unless something is watching. Drydock covers materially more registries and notification targets than the usual Watchtower/Diun setup, with a real audit log and OIDC auth on top — while remaining a single container to run.

## Problems It Can Remove
- Manually checking or cron-polling registries for new image tags.
- A hand-rolled script hitting registry APIs to catch security-patched base images.
- The "who approved this update" gap that plain auto-updaters (Watchtower) don't track.

## Practical Uses
- Watch base images and third-party containers used across personal or EC2/EKS infrastructure for new releases.
- Get a Slack/Discord/webhook alert when a patched image lands, filling a gap Dependabot-style tooling doesn't cover (container images, not just package manifests).
- Keep an audit trail of who approved which image update — useful for the MSSP admin-portal's own infra hygiene story.

## Product Opportunities
Could support an MSSP infra-hardening narrative: "we watch and patch our own containers, and can offer the same visibility for clients."

## Agent / Automation Opportunities
Webhook triggers are simple for an agent to consume and act on — e.g., auto-opening a PR that bumps a pinned image tag when Drydock fires a notification.

## Integration
Docker/Docker Compose, recommended behind a socket-proxy to avoid handing the container full Docker-socket access. Low integration effort. Multi-arch (amd64/arm64) images published to Docker Hub and GHCR.

## Architecture Notes
Nothing unusual architecturally — the value is breadth (23 registries, 20 notification/action providers) and engineering rigor for a project this size: CI, OpenSSF Scorecard, mutation testing (Stryker), codecov, and localized docs in 7 languages.

## Maturity
Mature. Created 2026-02-08 (~7 months old), actively maintained (pushed same day as this review), v1.6.0, sponsors, OpenSSF Best Practices badge.

## License
AGPL-3.0. **Flagged**: fine to self-host and run internally/for clients unmodified; do not fork and offer a modified version as a hosted SaaS without releasing source under AGPL or obtaining a commercial license.

## Alternatives
Watchtower (auto-updates but weaker visibility/notifications), Diun, What's Up Docker (WUD), paid registry-monitoring features bundled into some container platforms.

## Risks / Limitations
- AGPL-3.0 — see license note above.
- Two `[!WARNING]` blocks in the README describe breaking security-hardening changes across recent releases (e.g., auth now fails closed on upgrade, session cookie renamed). Read `UPGRADE-NOTES.md` before deploying rather than pulling `:latest` blind.

## Recommendation
USE NOW — low-effort, well-engineered, and directly replaces a category of manual/cron-based registry watching. Read the upgrade notes before first deploy given the recent security-hardening changes.

## Change History
### 2026-08-30
Discovered and reviewed. GitHub API verified: AGPL-3.0, 245 stars, pushed_at 2026-08-30, created 2026-02-08.
