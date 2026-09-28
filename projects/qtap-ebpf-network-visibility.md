# Qtap

## Summary
An eBPF agent that attaches to TLS/SSL functions in the kernel to reveal the unencrypted contents of all egress network traffic on a host — full request/response bodies, with process/container/user/protocol context — without a proxy, certificate management, or any application code changes.

## Why I Should Care
Most traffic-inspection tools require either a proxy (cert trust, app config changes) or access to TLS keys. Qtap decrypts at the kernel boundary via eBPF, so it works against any process on a host, including ones I don't control or can't modify — useful both for debugging my own services and for security auditing.

## Problems It Can Remove
Adding temporary logging to a service just to see what it's actually sending to a third-party API, or standing up mitmproxy with cert trust configured on the client, just to debug one integration issue.

## Practical Uses
- Seeing exactly what a Node/Python service sends to a third-party API when an integration misbehaves, without touching the app.
- Security review of outbound traffic from an EC2/ECS/EB workload to confirm no unintended data exposure.
- Reverse-engineering a legacy or undocumented internal service's network behavior with no source access.
- Confirming a deploy didn't silently change what an application sends over the wire.

## Product Opportunities
An internal "what does this service actually send externally" audit capability — directly relevant to the MSSP security-admin-portal context.

## Agent / Automation Opportunities
None built in. The CLI output is scriptable enough that it could be handed to a coding agent as a live debugging tool during a troubleshooting session.

## Integration
Linux host agent via install script or binary; Docker deployment requires `--privileged` or `CAP_BPF` plus host PID namespace. Requires Linux kernel 5.10+ with BTF enabled.

## Architecture Notes
Attaches eBPF programs to TLS/SSL library functions so traffic is captured before encryption / after decryption, then routed through a plugin pipeline with full context (process/container/host/user/protocol). Includes a built-in DevTools web UI modeled on Chrome DevTools' network tab. Backed by Qpoint.io, whose commercial hosted product is built on top of this open agent.

## Maturity
Emerging. Created 2025-04-16, 1,460 stars, 55 forks, active multi-contributor team, last push 2026-09-25. No tagged GitHub releases at all — always tracks `main`.

## License
Apache-2.0 — no restrictions. The agent is the free/open core of Qpoint's commercial product; standalone use outside their hosted platform is explicitly documented and supported.

## Alternatives
mitmproxy (requires proxy + cert trust), tcpdump/Wireshark (no TLS decryption without keys), application-level API gateway logging.

## Risks / Limitations
Requires privileged or CAP_BPF host access to run — the agent itself becomes part of the host's security surface and should be scoped tightly. No versioning scheme exists (no tagged releases), so there is no way to pin or audit what changed between installs. Long-term OSS investment is tied to Qpoint's commercial business.

## Recommendation
PROTOTYPE — compelling architecture for a real debugging/security-audit need, but the lack of any release versioning and the privileged-access requirement warrant testing in a controlled environment before wider use.

## Change History
### 2026-09-28
Initial discovery and review. Slot 6 run; found via GitHub Search API `topic:ebpf+observability` query.
