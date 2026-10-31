# PII-Shield

## Summary
A zero-code Kubernetes sidecar — also usable as a CLI or an in-process WASM module —
that redacts PII and detects secrets in logs via entropy analysis before they leave the
pod, preserving JSON structure, aimed at GDPR/SOC2 log-sanitization without sending data
to a hosted service.

## Why I Should Care
Directly relevant to the EKS/ALB side of the AWS stack and to the MSSP/PTaaS secondary
context: a drop-in sidecar is a lower-effort compliance control than building log
redaction into every service by hand.

## Problems It Can Remove
Hand-rolled regex log scrubbing per service; accidentally-logged API keys/secrets
reaching a log aggregator or training data; building log-redaction compliance controls
from scratch for GDPR/SOC2.

## Practical Uses
- Redact customer PII from application logs on an EKS cluster before they hit
  CloudWatch or another log aggregator
- Catch accidentally-logged secrets via entropy analysis as a defense-in-depth layer
  alongside existing secret scanning
- Embed the WASM SDK directly in a Node service for sub-millisecond in-process redaction
  without a sidecar network hop

## Product Opportunities
None identified directly — this is a compliance/infrastructure utility rather than a
product-embeddable component.

## Agent / Automation Opportunities
None identified.

## Integration
Kubernetes Operator (Helm-based, injects a distroless sidecar) for the zero-code path;
in-process WASM SDKs (`pii-shield-wasi` on PyPI, `@aragossa/pii-shield-wasi` on npm) for
direct embedding; a CLI for ad-hoc use. Low integration effort for the CLI/WASM paths;
the K8s operator path is still stabilizing per the project's own status table.

## Architecture Notes
Two deployment models: a Kubernetes Operator that injects a distroless sidecar to
intercept and sanitize logs on the fly, and an in-process WASM engine for <1ms latency
without a network hop. Detects secrets via entropy analysis rather than pattern lists
alone, and preserves JSON structure through redaction rather than flattening it.

## Maturity
Emerging. Created 2026-01-30 (~9 months old), latest release v2.3.1 (2026-10-27). 178
stars, 12 forks. Core scanner and CLI sidecar are "released / controlled deployments"
per the project's own status table; the Kubernetes operator is in a stabilization phase.
Corroborated by an OpenSSF Best Practices badge, an Artifact Hub listing, a Docker Hub
pull-count badge, and CI test-coverage reporting.

## License
Apache-2.0. No restrictions on commercial or SaaS use.

## Alternatives
Hand-rolled regex log scrubbing (fragile, no entropy-based secret detection).
vikasgautam18/pii-shield (a Microsoft developer-community blog project), the
piishield.ai/piishield.com prompt-redaction products, and the PyPI `pii-shield` package
by Intellirim are unrelated, differently-scoped projects sharing the same name — the
project's own README explicitly disclaims the overlap.

## Risks / Limitations
- The Kubernetes operator deployment mode is explicitly in a "stabilization phase" per
  the project's own status table — prefer the CLI/core-scanner path until that matures
- 0 GitHub subscribers against 178 stars / 12 forks is modest but not alarming for a
  9-month-old project given the real external corroboration (OpenSSF badge, Artifact
  Hub, Docker pulls, Go Report Card)

## Recommendation
PROTOTYPE — worth testing the CLI/WASM path on a real service's logs before relying on
the still-stabilizing Kubernetes operator in production.

## Change History
### 2026-10-31
Initial discovery and cataloguing as Small but High-Leverage Utility of the day (slot 4:
data/search/documents/RAG).
