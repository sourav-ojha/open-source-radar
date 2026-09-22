# FrankenTUI (ftui)

## Summary
FrankenTUI is a Rust terminal-UI kernel — 1.1M+ lines across 20 crates, 80+ widgets, and 45 interactive demo screens — built around a diff-based renderer, strict inline-mode support (stable UI region while logs scroll above it), RAII terminal cleanup, and an in-tree WASM backend that runs the same widgets in a browser over WebGPU/canvas2d. From Jeffrey Emanuel (Dicklesworthstone), whose other tools (destructive-command-guard, coding-agent-session-search, beads_rust) are already in this catalog.

## Why I Should Care
Building internal CLI dashboards, admin TUIs, or coding-agent status panels usually means reaching for ratatui (Rust) or ink/blessed (Node) and hand-rolling the parts that are genuinely hard to get right: flicker-free rendering, correct terminal cleanup on crash/panic, and stable "inline" regions that coexist with normal scrollback logging. FrankenTUI treats those as the kernel's job rather than the widget author's problem, and the demo showcase is runnable in-browser with no install — worth 5 minutes just to see the rendering quality bar it's aiming for.

## Problems It Can Remove
- Hand-rolling terminal cleanup/panic-safety for any custom CLI tool with a live-updating UI (log tails, progress dashboards, agent status panels).
- Choosing between "just print logs" and "build a full TUI" — inline mode gives a stable UI region while normal output still scrolls above it.
- Rebuilding the same dashboard twice for terminal and browser — the WASM backend renders the same Rust widget code in-browser.

## Practical Uses
- A status/monitoring dashboard for a long-running agent or build process that needs both a live summary panel and scrolling log output.
- Prototyping internal ops tooling (deploy status, queue depth, job runner) as a TUI instead of a throwaway web dashboard.
- Studying the diff-based rendering and "shadow-run" determinism-validation approach (`ShadowRun::compare()`) even without adopting the library.

## Product Opportunities
Less a product-building block, more a component for internal tooling — a polished TUI kernel could differentiate a CLI-first developer product (an ops tool, a local dev environment manager) from the usual ad-hoc terminal output most CLIs ship with.

## Agent / Automation Opportunities
Not agent-specific itself, but a natural fit for building the terminal front-end of a coding-agent CLI or an agent-orchestration dashboard — several already-catalogued coding-agent CLIs (VTCode, etc.) hand-roll exactly this kind of rendering layer today.

## Integration
- **Installed locally**: Rust crate, `cargo run -p ftui-demo-showcase` to try it; no installer yet (source-only).
- **Embedded as library**: yes — composable crates, add only the ones needed (layout, text, style, runtime, widgets).
- **Web**: in-tree WASM backend (`ftui-web`) renders the same widgets in a browser via WebGPU/canvas2d fallback.
- **Interfaces**: Rust library API only; no CLI tool, no HTTP/MCP surface.
- **Integration effort: Medium-High** — requires Rust (nightly toolchain per the README), the project is explicitly WIP status, and at 1.1M+ lines across 20 crates the API surface is large to learn. Not a quick drop-in for a MERN-stack developer without Rust experience.

## Architecture Notes
Rendering pipeline is a strict Buffer → Diff → Presenter → ANSI pipeline with a documented "one-writer rule" (`TerminalWriter` owns all stdout writes) to avoid the interleaved-output bugs that plague ad-hoc TUI code. A "Bayesian intelligence layer" (BOCPD, VOI, conformal prediction) drives statistical diff-strategy and resize-coalescing decisions — an unusual and interesting choice for a rendering kernel. `ShadowRun::compare()` exists specifically to prove rendering determinism holds across runtime migrations, which is a level of rigor uncommon in TUI libraries.

## Maturity
Experimental / WIP. Created January 2026 (~8 months old), 283 stars, only 2 contributors despite the enormous scope — likely near-single-author. Tagged release v0.8.0 (2026-09-14). README explicitly marks status as WIP.

## License
**MIT with an OpenAI/Anthropic Rider — flagged loudly.** The rider explicitly voids all rights for "Restricted Parties" (OpenAI, Anthropic, their affiliates, and anyone acting on their behalf), and separately bars using the software or derivative works in any dataset, training corpus, evaluation harness, or ML pipeline involving those parties. This does not block Sourav's personal or commercial use of the code, but it is not standard MIT and should not be assumed compatible with normal open-source tooling/CI that might route through an Anthropic- or OpenAI-operated service (e.g., some hosted CI or code-review products). Read the full rider in the repo's `LICENSE` before any commercial use.

## Alternatives
ratatui (Rust, the incumbent, plain MIT), Bubble Tea (Go), ink/blessed (Node.js/React for CLI). FrankenTUI's differentiation is depth of the rendering guarantees (diff-based + shadow-run determinism proofs) and the dual terminal/WASM backend from one codebase, at the cost of Rust-only, WIP status, and the unusual license.

## Risks / Limitations
- WIP status on a component this large (1.1M+ lines) — expect breaking changes.
- Effectively single-author (2 contributors) — meaningful bus-factor risk.
- Non-standard license rider adds legal-review overhead most MIT-licensed alternatives don't require.
- No installer yet; source-build only.

## Recommendation
STUDY — the rendering-determinism and inline-mode architecture are worth learning from regardless of adoption; direct use requires Rust and a tolerance for WIP breakage, and the license rider needs a read before any commercial use.

## Change History
### 2026-09-22
Initial discovery and review. Rotation slot 7 (experimental/hidden gems); surfaced via GitHub search for "terminal ui" pushed in the last week.
