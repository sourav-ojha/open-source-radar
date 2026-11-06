# lazyjournal

## Summary
A terminal UI (Go, `lazydocker`/`lazygit`-family styling) for viewing and filtering logs from journald, auditd, the filesystem, Docker and Podman containers, Compose stacks, and Kubernetes pods — all from one screen, with regex/fuzzy filtering, log highlighting, and a web mode for viewing the same interface from inside a container over a browser.

## Why I Should Care
Debugging across EC2/EKS boxes usually means switching between `journalctl`, `docker logs`, `kubectl logs`, and `tail -f` on plain files, each with its own flags and filtering syntax. lazyjournal puts all of those sources behind one TUI with a consistent filter/search model, which is a direct speed-up for routine ops and incident triage on his own infra.

## Problems It Can Remove
- Memorizing/re-typing `journalctl`, `docker logs`, `kubectl logs`, and `tail` invocations and their respective filter flags.
- SSH-ing in and `grep`-piping a live log stream by hand.
- Needing a terminal multiplexer session just to watch several log sources side by side.

## Practical Uses
- Single TUI to tail systemd service logs, Docker/Podman container logs, Compose stack logs, and k8s pod logs on a bastion or EC2 box.
- Regex/fuzzy-filter a noisy log stream live instead of piping through grep repeatedly.
- Run it inside a container and open the same TUI in a browser (web mode) for a remote box without a full SSH+tmux setup.
- Fast first-look triage tool for the MSSP admin-portal's own infra before reaching for a full observability stack.

## Product Opportunities
None directly — this is an internal ops tool, not a building block for a product.

## Agent / Automation Opportunities
Primarily an interactive human TUI; not designed to be scripted or driven by an agent. Value is operator speed, not automation.

## Integration
Single statically-linked Go binary. Install via Homebrew, Docker Hub image, Conda-Forge, AUR, or Debian package — no dependencies, no config required to start. Low integration effort.

## Architecture Notes
Built on `awesome-gocui` (a maintained fork of `gocui`). The interesting design choice is treating "log source" as a pluggable list — journald units, auditd, filesystem paths, and container/pod discovery all feed the same filtered-view UI rather than being separate modes.

## Maturity
Mature. Created 2024-11-12 (~2 years old), 1,440 stars, 35 forks, 8 subscribers, actively released (v0.8.7 shipped the same day as this review). Broadly packaged (Homebrew, Docker Hub, Conda-Forge, AUR, Debian testing) and has a live Killercoda playground.

## License
MIT — no restrictions.

## Alternatives
lazydocker (Docker-only, no journald/auditd/k8s), lnav (powerful filtering but no native Docker/k8s source list), stern/k9s (Kubernetes-focused, no journald/auditd), Dozzle (web UI, Docker-only).

## Risks / Limitations
- Single primary maintainer.
- An interactive workstation/bastion tool, not a substitute for a real log-aggregation pipeline at scale.

## Recommendation
USE NOW — low-risk, zero-config, immediately useful for anyone doing hands-on debugging across mixed journald/Docker/Kubernetes infra, which matches the EC2/EKS part of the stack directly.

## Change History
### 2026-11-06
Discovered and reviewed. GitHub API verified: MIT, 1,440 stars/35 forks/8 subscribers, created 2024-11-12, pushed 2026-11-05, latest release 0.8.7 (2026-11-05).
