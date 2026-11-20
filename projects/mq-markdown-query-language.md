# mq

## Summary
A jq-like query language and CLI for slicing, filtering, mapping, and transforming
Markdown, written in Rust. Ships a REPL, an experimental step debugger (`mq-dbg`), an
LSP/VS Code extension for custom function development, and a Docker image.

## Why I Should Care
A narrow but genuinely time-saving utility for a task that otherwise means fragile
regex/sed scripting: pulling structured content out of or into Markdown from a shell
pipeline or CI script.

## Problems It Can Remove
Removes hand-rolled regex/awk/sed scripts for extracting or reshaping Markdown sections,
and the brittleness that comes with them when document structure changes slightly.

## Practical Uses
- Extract or reshape sections of documentation files in a CI script or pre-commit hook.
- Generate structured Markdown input for an LLM prompt, or post-process a Markdown
  response, from a shell pipeline.
- Batch-transform a directory of Markdown docs consistently.
- Quickly pull one section or pattern out of a long Markdown file at the command line
  instead of opening an editor.

## Product Opportunities
None — this is a personal/CI scripting utility, not a product component.

## Agent / Automation Opportunities
Its own README frames "LLM workflows" and "LLM input generation" as primary use cases,
given Markdown is the dominant structured format for LLM prompts and outputs.

## Integration
Single binary via install script, Homebrew, MacPorts, Scoop, Cargo, or Docker. Low
integration effort — no project changes required to start using it from a shell.

## Architecture Notes
A small purpose-built query language (not a general scripting language) evaluated over
a parsed Markdown AST, modeled deliberately on jq's syntax so the mental model transfers
directly from JSON tooling experience.

## Maturity
Emerging: created 2025-02-24 (~9 months old), 1,083 stars, 2 subscribers, 24 forks, 14
open issues, actively pushed as of this review. The README explicitly flags the project
as "under active development" — syntax may still shift.

## License
MIT. No restrictions.

## Alternatives
- sed/awk/grep on raw text — no Markdown structure awareness.
- pandoc — a format converter, not a query/transform language.
- Hand-written Node/Python scripts using a Markdown AST library.

## Risks / Limitations
- Effectively a single-maintainer project (2 subscribers on 1,083 stars).
- Explicitly under active development; CLI syntax or built-in functions may change
  between releases.

## Recommendation
USE NOW for ad hoc Markdown scripting — low-risk given it's a narrow CLI tool with no
project integration required; just install it and reach for it the next time a
Markdown-munging one-liner is needed.

## Change History
### 2026-11-20
Today's Small but High-Leverage Utility, surfaced via a `topic:repl` GitHub search
during slot 3 (developer utilities, debugging, testing) discovery. Catalogued as USE
NOW, 6.8/10.
