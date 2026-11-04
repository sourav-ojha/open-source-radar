# earshot

## Summary
Gives a coding agent hearing: a CLI, Python API, and MCP server that describes what an
audio file sounds like in words, scores how well it matches a text description, finds the
best moment inside a long recording, tells real recordings from code-synthesized ones, and
produces a timestamped critique of a video's sound design against a profile of professional
launch films — plus a code-based sound-effect synthesizer an agent can iterate on by
listening to its own output.

## Why I Should Care
An agent that can write the code to render a product demo, game, or marketing video still
cannot tell whether the result sounds right — whether a hit lands on the cut, whether a UI
click sounds premium or cheap, whether the music swells at the wrong moment. earshot turns
that into something an agent can reason about with text output, closing a real gap for
anyone shipping video/audio alongside a product (demo videos, onboarding flows,
notification sounds) rather than treating sound as an afterthought.

## Problems It Can Remove
- Manually eyeballing spectrograms or re-listening to iterations of a generated sound
  effect by hand.
- Stock sound-effect licensing for simple UI/motion sounds (clicks, whooshes, pops, dings) —
  `earshot forge` renders these from DSP code rather than samples.
- A subjective, unmeasured "does this demo video sound professional" judgment call.

## Practical Uses
- `earshot critique launch.mp4` for an automated, timestamped scorecard on a product demo
  or marketing video before it ships.
- `earshot sfx` / `earshot kit` to generate royalty-free UI sound effects (notification
  dings, button clicks, transition whooshes) from code instead of a sample library.
- `earshot describe` / `earshot match` as a sanity check inside a CI step that validates
  generated or user-uploaded audio assets.
- The MCP server (`earshot mcp`) lets Claude Code or another agent call any of the above
  directly while iterating on a media asset.

## Product Opportunities
Marketing/demo-video quality-checking as a CI step is directly reusable for any product
with a public demo or onboarding video (including an MSSP admin-portal SaaS's own
marketing site) without hiring sound design per iteration.

## Agent / Automation Opportunities
Ships natively as an MCP server, keeping models loaded between calls (each bare CLI
invocation otherwise pays a 20-40s model load). The `forge`/`fit` loop (write a sound as
code, render candidates, score them, rewrite) is itself an agent-iteration pattern worth
reusing even outside audio: generate, measure objectively, critique in words, repeat.

## Integration
`git clone` + `pip install -e ".[mcp,llm,dev]"`; Python 3.10+ and ffmpeg/ffprobe on PATH;
CPU only, no GPU. First use downloads ~1.1GB of pinned open models (CLAP, plus others) into
a local cache. No PyPI package yet — install is from source only. Integration effort: Low
for a one-off CLI use, Medium to wire into CI given the model download and Python
dependency surface.

## Architecture Notes
`earshot forge` builds sounds from physical-synthesis recipes (modal resonators, contact
excitations, filtered noise, a modeled room and microphone — no sampled audio inside), then
scores candidates against a critic built from earshot's own listening models plus
model-free measurements (1/3-octave tonal balance, envelope shape, artifact detection), with
a cheap pre-screen before the expensive models hear anything and an evolutionary search
over parameters. `earshot fit` runs the reverse direction: analysis-by-synthesis to turn a
real recording into an editable, re-renderable recipe.

Notably candid about its own limits: a "Honest limits (Goodhart)" section states plainly
that the search optimizes earshot's own models, which can be satisfied without the sound
actually improving — the README directs the reader to model-free measurements and a human
blind test rather than trusting the tool's own scores unconditionally. A disclosed blind
test (one listener, 15 trials) found the author identified the synthesized sound correctly
8 of 15 times — close to chance.

## Maturity
Experimental. Created 2026-10-15 (20 days old), v0.4.0, single primary author, 3,811 stars /
243 forks / 9 subscribers. The stars:subscribers ratio (~423:1) is high enough to flag, but
the fork count (243, a ~6.4% fork:star ratio — forking requires deliberate action, unlike
starring) and the unusually detailed, self-critical documentation (published methodology,
disclosed failure modes, a changelog with real version-to-version regression numbers) read
as substance rather than a marketing-only repo. No independent reproduction of its
benchmark claims was attempted here.

## License
MIT. No restrictions.

## Alternatives
Manual QA by ear (status quo); stock SFX libraries (licensing cost, no code-native
iteration); an audio-LLM critique call (earshot supports this as an optional, consent-gated
`--yes-upload` path to Gemini, but the core toolkit is model-free/local by default).

## Risks / Limitations
Single maintainer, 20 days old, no package registry listing yet (source install only). The
project's own "Honest limits" section is a legitimate caveat, not just disclosure theater —
treat its automated scores as a first-pass filter, not a final verdict, and always sanity
check against the model-free measurements and a real human listen before shipping anything
based solely on an earshot score.

## Recommendation
PROTOTYPE as a hidden-gem architecture study — worth trying directly on one real demo video
or a handful of UI sound effects to judge output quality firsthand, rather than full
adoption into a pipeline yet.

## Change History
### 2026-11-04
Discovered via GitHub search (`ai code review agent` keyword sweep, slot 1 rotation). First
catalog entry.
