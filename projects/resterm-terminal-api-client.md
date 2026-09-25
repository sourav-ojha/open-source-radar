# Resterm

## Summary
Terminal-native "API-as-code" workbench. Requests live as plain, git-diffable
`.http`/`.rest` files with directives for conditionals, loops, multi-step workflows,
captures/assertions, mock servers, and — notably — inline SSH-bastion and Kubernetes
port-forward tunnels. Supports HTTP, GraphQL, gRPC, WebSocket, and SSE, with a TUI for
interactive use and a CLI runner (`resterm run`) for CI.

## Why I Should Care
A terminal-first, version-controllable alternative to Postman/Insomnia that plugs
directly into an AWS/Kubernetes workflow — the `@ssh` and `@k8s` directives remove a
manual `kubectl port-forward` or SSH-tunnel step from the everyday loop of hitting an
internal service behind a bastion or inside a private EKS cluster.

## Problems It Can Remove
Removes exported/shared Postman collections that drift from the codebase, removes
manual SSH-tunnel or port-forward setup before testing an internal endpoint, and
removes the need for a separate mock-server tool during frontend/backend decoupling.

## Practical Uses
- Keep API request collections in the repo next to the code they exercise, reviewable
  in normal `git diff`.
- Use `@ssh`/`@k8s` directives to reach a service behind a bastion or inside a private
  EKS cluster without a separate terminal tab running `port-forward`.
- Declare mock responses in the same file as the real request for local dev before a
  backend endpoint exists.
- `resterm record` to capture live traffic against a real API and turn it into request
  or mock files.
- Run the same `.http` files in CI via `resterm run` with JSON/JUnit output.

## Product Opportunities
None specific — a developer productivity tool, not an embeddable product component.

## Agent / Automation Opportunities
None — the project states "No AI integration" explicitly as a design choice. The
`resterm run` CLI mode and headless Go API are usable from any automation/CI pipeline,
agent-driven or not.

## Integration
Single binary via Homebrew, an install script, or `go install`. `resterm init` scaffolds
a starter project. Low integration effort — no server, no account, works offline except
for the actual API calls being tested.

## Architecture Notes
Go, single static binary. Request files use `# @directive` comments layered on top of
standard HTTP syntax (`@setting`, `@for-each`, `@when`/`@if`, `@capture`, `@var`,
`@assert`, `@mock`, `@ssh`, `@k8s`). A small expression language ("RestermScript") plus
optional JavaScript hooks cover scripting needs beyond the directive set.

## Maturity
Emerging but active. v1.9.0, 1,949 stars, last push 2026-09-24 (same day as review).
Single dominant contributor (1,414 of ~1,423 commits).

## License
Apache-2.0. No restrictions on commercial or embedded use.

## Alternatives
- Postman/Insomnia/Hoppscotch/Bruno — GUI-first, heavier, not naturally git-diffable.
- Hurl — plain-text and CI-friendly, but no interactive TUI, mock server, or SSH/k8s
  tunnel directives.
- VS Code REST Client extension — similar `.http` file idea, but no TUI, tunnels,
  mocks, or standalone CLI runner.

## Risks / Limitations
- Single dominant contributor — bus-factor risk.
- Prebuilt Linux binaries require glibc 2.32+; older distros need a from-source build.
- No AI/agent integration by design, which is a deliberate scope limit rather than a
  bug, but worth knowing going in.

## Recommendation
PROTOTYPE — worth adopting for API work that regularly touches SSH-bastion or
Kubernetes-fronted services; the tunnel directives alone justify a trial over a
GUI client for that specific workflow.

## Change History
### 2026-09-25
Discovered via GitHub topic search (`topic:api-testing`) during slot 3 discovery.
Catalogued as PROTOTYPE, 8.0/10.
