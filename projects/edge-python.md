# Edge Python

## Summary
A single-pass SSA compiler and tiered register VM for a sandboxed Python subset (classes, async/await, pattern matching, compile-time import resolution), compiled to one WebAssembly module that runs identically in browsers, Node/JS runtimes, and a standalone CLI. File, network and environment access are denied by default and only granted per module declared in `edge.json`. Ships with an actor-model runtime (durable message log, cooperative scheduling) built on top of the same engine.

## Why I Should Care
Gives a real, capability-gated sandbox for executing untrusted or LLM-generated Python inside a Node backend or a browser tab, without the overhead of a container or VM. Directly useful for any product feature where an AI agent needs a "run this code" tool and the blast radius of that code must be bounded.

## Problems It Can Remove
Replaces a hand-rolled subprocess/Docker sandbox for executing simple, untrusted Python, and removes the need to pay a hosted code-execution API (E2B, Modal) for cases where the Python subset here is sufficient.

## Practical Uses
- A "python_execute" tool for an AI agent built into a Node product, with explicit per-module capability grants instead of blanket container isolation.
- Client-side Python execution in a web app (teaching tools, interactive notebooks) with no server round-trip.
- An actor-based worker pool for small, CPU-bound jobs that don't need full CPython or third-party packages.
- A CLI for quick, dependency-free script execution in CI.

## Product Opportunities
Could become the sandboxing layer inside a coding-agent or automation product, positioned as a lighter, cheaper alternative to container- or VM-based code execution for the subset of tasks that don't need full CPython compatibility.

## Agent / Automation Opportunities
Natural fit as an MCP or agent tool-call target for a code-execution capability. The `skill/SKILL.md` file in the repo is explicitly written as a guided reference for AI models writing Edge Python, which signals the author is already designing for agent consumption.

## Integration
CLI install via a curl script (`cdn.edgepython.com/cli/install.sh`) on macOS/Linux/WSL; no npm package yet, so Node integration means embedding the WASM module (`compiler.wasm`) through the JS host (`js/`) rather than `npm install`. Medium effort: real wiring required, but the WASM module and JS bindings are already built and documented.

## Architecture Notes
Single-pass SSA compiler feeding a tiered register VM, with NaN-boxed values, inline caches, mark-sweep GC and snapshotting. The actor runtime adds a durable message log (compacts past 64 MiB, syncs before acknowledging), cooperative preemption at back-edges, and graceful SIGTERM draining (25s). The changelog detail here (phi-node bugs in lowered code, scheduler parking semantics, inbox backpressure at 503) reads as genuine systems engineering, not marketing copy.

## Maturity
Emerging. Created March 2026, single maintainer (dylan-sutton-chavez) plus one named sponsor (PyneSys, since May 2026). Weekly releases, currently v0.9.6 (pre-1.0). 272 stars, 16 forks, 5 watchers — the star:watcher ratio (54:1) is elevated versus a typical organic repo, but the depth of the documentation site, SKILL.md for AI models, and the technical substance of the release notes argue against this being an inflated-engagement repo like others flagged this run.

## License
Apache-2.0, confirmed via `LICENSE.md`, covering the engine, CLI, JS host, PDK/ABI, fuzzer, benchmarks and docs. No restriction on commercial use or embedding.

## Alternatives
Pyodide (full CPython to WASM, much heavier, broader compatibility but no capability-gating by default), RestrictedPython (in-process, no WASM-level isolation), Docker/subprocess sandboxing, E2B/Modal (paid, hosted sandbox APIs).

## Risks / Limitations
Not full Python — no third-party packages; stdlib only via explicitly declared modules. Verify the subset actually covers what an agent tool needs before relying on it in production. Single-maintainer project, still pre-1.0; no npm distribution yet.

## Recommendation
PROTOTYPE — worth testing as an agent code-execution tool in a real workflow, with explicit verification of what the Python subset does and doesn't support before depending on it.

## Change History
### 2026-10-20
First reviewed and catalogued. Found via GitHub search for recently-active bytecode VM/compiler repositories during slot 7 (experimental / hidden gems) discovery.
