# sandbox-mcp

## Summary
sandbox-mcp is a CLI (`sbx`) and optional MCP adapter, from Microsoft developer advocate Scott Hanselman, that automates Windows Sandbox to boot a real, disposable, network-off Windows 11 machine, deploy and drive an app's UI via UI Automation, capture evidence (screenshots/logs), and tear the machine down. It's built to catch "works on my machine" failures — e.g. a missing VC++ redistributable — on a genuinely clean guest rather than a container pretending to be Windows.

## Why I Should Care
Nothing else found in recent runs gives a disposable, network-off, real Windows 11 machine with UI-Automation driving and an MCP adapter in one CLI. It has a direct, concrete fit for the MSSP/PTaaS side work: safely detonating a suspicious attachment or installer from a client engagement without touching a real machine.

## Problems It Can Remove
- Removes manually maintaining a snapshot/restore Windows VM for compat or malware testing.
- Removes the risk of testing an unknown binary on a real, persistent machine.
- Removes scripting Windows Sandbox's XML config, guest-agent install, and UI Automation driving from scratch.

## Practical Uses
- Safely opening a suspicious attachment/installer from a PTaaS/MSSP client engagement on a disposable, network-off VM.
- Catching missing-dependency bugs in a Windows desktop app (Electron, WPF, WinForms) before a customer does.
- Giving a coding agent a sandboxed Windows GUI to drive and verify a build against, with before/after screenshots as evidence.
- Regression-testing an installer against a genuinely clean Windows machine on every release.

## Product Opportunities
None directly for his own products — this is a security/testing utility, most relevant to the MSSP/PTaaS side work rather than his MERN product line.

## Agent / Automation Opportunities
The optional MCP adapter exposes the same sandbox lifecycle (boot, deploy, drive UI, capture evidence, teardown) as discoverable agent tools with inline image results, letting an agent verify a Windows build end-to-end without a human watching.

## Integration
Portable CLI with bundled Node.js, no separate Node/npm install needed. Runs only on a Windows 11 host (physical, or an instance with nested virtualization and Windows Sandbox enabled — most constrained cloud instances and Home-edition Windows won't work). Integration effort: **High** given the platform mismatch with a primarily Mac/Linux + AWS dev setup.

## Architecture Notes
Wraps Windows Sandbox (a built-in Windows 11 Pro/Enterprise lightweight VM feature) with a guest agent for UI Automation driving, a verified-download dependency-provisioning step (`sbx deps`), and evidence capture, exposed identically via direct CLI (`sbx`) or an MCP adapter.

## Maturity
Emerging — created 2026-11-06 (4 days old at review), 390 stars, 19 forks, 4 subscribers, 1 open issue. Fast growth is consistent with the author's personal following (a well-known, highly visible Microsoft developer advocate) rather than the disproportionate-issues/zero-fork pattern used elsewhere to flag fake engagement.

## License
MIT, confirmed via GitHub API.

## Alternatives
A long-lived Windows VM snapshot/restore workflow (slower, state can drift); GitHub Actions `windows-latest` CI runners (heavier, non-interactive, no UI-driving); commercial malware-sandbox services like Any.run or Joe Sandbox (hosted, not self-controlled).

## Risks / Limitations
- Windows-only — doesn't fit the primary Mac/Linux + AWS stack; needs a Windows 11 Pro/Enterprise host with Hyper-V-capable hardware just to try it.
- Very new (4 days old); independent adoption beyond the author's launch audience is unproven.
- Windows Sandbox's hardware/edition requirements rule out most constrained or Home-edition machines.

## Recommendation
PROTOTYPE — narrowly for the PTaaS/MSSP untrusted-file-testing use case on a Windows host, not for primary MERN/AWS development. Worth a one-off trial the next time a client engagement needs a disposable Windows machine to open something untrusted.

## Change History
### 2026-11-10
Initial discovery and review. Slot 7 (experimental projects and hidden gems) run.
