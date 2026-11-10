# Runno

## Summary
Runno is a WASI-based sandboxed code-execution runtime shipped as four npm packages: browser web components (`@runno/runtime`), a Node-side sandbox (`@runno/sandbox`), an isomorphic WASI binary runner (`@runno/wasi`), and an MCP server (`@runno/mcp`). It runs Python, Ruby, QuickJS/JavaScript, SQLite, and C (clang) code inside an isolated WASI sandbox — in the browser, on a server, or from an MCP client — with no Docker or Kubernetes dependency.

## Why I Should Care
Any product feature that needs to "run this code safely" (a coding exercise grader, an interview tool, runnable docs, an agent that needs to verify generated code actually executes) normally means standing up a Docker-based sandbox with real security review. Runno has done that work since 2021 and ships it as a maintained npm install with a ready MCP server for agent use.

## Problems It Can Remove
- Removes building a Docker-based ephemeral sandbox from scratch for executing untrusted code.
- Removes giving a coding agent raw host shell access just to verify a snippet runs.
- Removes building a separate in-browser code-execution feature (web components ship ready-made).

## Practical Uses
- Adding a "run this code snippet" feature to a product (tutorial, interview tool, interactive docs).
- Grading or testing small Python/Ruby/SQL/C submissions from untrusted users.
- Giving a coding agent a safe code-execution MCP tool instead of shell access.
- Running what-if code checks inside a browser-based admin tool with no server round-trip.

## Product Opportunities
A reusable "run code safely" building block for any product feature needing user- or agent-submitted code execution — grading, playgrounds, interactive documentation.

## Agent / Automation Opportunities
`@runno/mcp` is a ready-made MCP server exposing sandboxed code execution to any MCP-compatible agent — a drop-in "verify this code runs" tool without granting the agent host shell access.

## Integration
`npm install @runno/sandbox` (or `@runno/mcp` for the MCP server, or `@runno/runtime` for browser web components). Browser use requires setting `Cross-Origin-Opener-Policy`/`Cross-Origin-Embedder-Policy` headers for `SharedArrayBuffer`. No Docker required. Integration effort: **Medium** (header requirement for browser use; otherwise straightforward).

## Architecture Notes
Built on WASI (WebAssembly System Interface) with a fake filesystem/OS layer emulated in JavaScript, using WASM binaries published to WAPM for each supported language runtime.

## Maturity
Mature relative to most 2026 "agent sandbox" entrants — created 2021-06-05, 775 stars, 48 forks, 12 subscribers, 38 open issues, actively pushed (2026-10-31). Effectively single-maintainer (taybenlor) but with a multi-year track record.

## License
MIT, confirmed via GitHub API. No restrictions.

## Alternatives
Pyodide/WebContainers (browser-only, heavier, more complete, no MCP server); already-catalogued cluster-native agent sandboxes (capsulerun/runtime, OpenKruise Agents — heavier, require cluster infra); Docker-based ephemeral sandboxes (no in-browser option).

## Risks / Limitations
- Fixed runtime list (python, ruby, quickjs, sqlite, clang, a few others) tied to WAPM-published builds — adding a language isn't a config change.
- Browser use's COOP/COEP header requirement complicates some static-host/CDN setups.
- Effectively single-maintainer; 12 subscribers for 775 stars is modest.

## Recommendation
PROTOTYPE — solid, low-risk building block to test the next time a "run code safely" feature is needed; no urgent standalone reason to adopt today without a concrete feature driving it.

## Change History
### 2026-11-10
Initial discovery and review. Slot 7 (experimental projects and hidden gems) run.
