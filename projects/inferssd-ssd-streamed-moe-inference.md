# InferSSD

## Summary
A local inference runtime, built on llama.cpp, that runs mixture-of-experts GGUF models larger than available RAM or VRAM by streaming experts from SSD. Ships a browser chat UI and an OpenAI-compatible API alongside the engine, and has been tested against named models including DeepSeek-V4.1-Flash-1017 (763B total parameters) and Qwen3.8-Flash-Next. macOS (Metal) support is complete; Windows/Linux CUDA, Vulkan and ROCm backends are in progress. From wici.ai.

## Why I Should Care
Makes genuinely large, frontier-adjacent open-weight MoE models usable on commodity hardware without buying workstation-class GPU memory — useful for cost-free experimentation with or benchmarking of large models during development, without recurring per-token API spend.

## Problems It Can Remove
- The hardware cost barrier to running a 700B+-parameter open-weight model locally.
- Recurring per-token API cost during iterative prompt/tooling development, by swapping in a free local model behind an OpenAI-compatible endpoint.

## Practical Uses
- Run a large open-weight MoE model locally to evaluate whether it's worth paying for hosted inference of the same or a similar model.
- Point an agent harness or app's OpenAI-compatible client at InferSSD's local API during development to iterate without API costs.
- Benchmark a new open-weight model release against existing paid options before committing budget.

## Product Opportunities
None directly — this is an inference runtime, not a building block for a product.

## Agent / Automation Opportunities
The OpenAI-compatible API means any agent harness already wired for an OpenAI/Claude-compatible endpoint can point at InferSSD with just a base-URL change, giving a free local option for development-time testing of agent workflows.

## Integration
Desktop app download (macOS now; Windows/Linux in progress) or build from source. Requires enough SSD space and I/O throughput to stream MoE experts, plus a supported GPU (Metal today; CUDA/Vulkan/ROCm in progress for other platforms). Integration effort: **Medium** — not a one-line install, and platform support is currently incomplete outside macOS.

## Architecture Notes
Builds on llama.cpp and adds SSD-based expert streaming for MoE models: rather than requiring every expert resident in RAM/VRAM, it streams the needed experts from disk, trading I/O for memory capacity. This is a nontrivial systems-engineering problem (deciding which experts to keep resident, overlapping I/O with compute) that the project has already tackled and benchmarked against specific named models rather than claiming generic support.

## Maturity
Emerging. Created 2026-10-22; uses a date-based release tag (`261124.351`) rather than semver, suggesting a pre-1.0, fast-iterating project. Backed by wici.ai, which runs a Discord and Slack community — a small real company rather than a solo weekend project.

## License
Apache-2.0, confirmed via the repo's license badge. No commercial-use restrictions.

## Alternatives
llama.cpp directly (the engine this builds on, without the SSD-expert-streaming layer or packaged app/API). Ollama and LM Studio are mainstream and excluded by this radar's policy, and neither supports SSD-streamed MoE experts beyond VRAM in the same way. Cloud inference APIs avoid the hardware constraint entirely but carry ongoing per-token cost.

## Risks / Limitations
- Very young (about a month old at review); date-based release tagging suggests instability expected pre-1.0.
- Windows/Linux GPU backends (CUDA, Vulkan, ROCm) are explicitly still "in progress" — macOS/Metal is the only complete platform today.
- Star:subscriber ratio (~130:1) is moderate but above this catalog's typical organic baseline; worth re-checking traction in a future run.

## Recommendation
PROTOTYPE — worth trying on macOS to validate the SSD-streaming claim with a real large model before depending on it, and worth re-checking once Windows/Linux GPU support lands.

## Change History
### 2026-11-25
First seen and reviewed. Discovered via GitHub search for local-inference/model-gateway keywords (slot 1: AI agents, MCP, coding productivity). Verified license, stars/forks/subscribers and release tag via the GitHub API; README read directly to confirm the SSD-expert-streaming architecture and platform support status.
