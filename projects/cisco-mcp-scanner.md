# MCP Scanner (Cisco AI Defense)

## Summary
A multi-engine security scanner for MCP servers and tools, built by Cisco's AI Defense team.
Combines YARA rules, LLM-based behavioral analysis, Cisco AI Defense's hosted inspection
API, pip-audit CVE/PYSEC/GHSA scanning, VirusTotal binary-hash lookups, and sandboxed PyPI
package execution behind one CLI, REST API, and Python SDK.

## Why I Should Care
This is the broadest MCP-server scanner evaluated in this radar so far: static rules,
LLM-based threat classification, dependency CVEs, and binary malware checks in one tool,
from a vendor with an ongoing incentive to keep detection rules current. Directly useful
before adding any third-party MCP server to an agent's toolset, and directly reusable for
PTaaS-style vetting work.

## Problems It Can Remove
Removes the need to manually audit an MCP server's tool/prompt/resource definitions,
source code, and bundled dependencies for malicious or unsafe behavior before trusting it.

## Practical Uses
- Scan a third-party MCP server's tools/prompts/resources for malicious instructions before
  adding it to an agent's toolset.
- Run pip-audit-backed CVE/PYSEC/GHSA scanning on an MCP server's Python dependencies in CI.
- Catch "production readiness" issues (missing timeouts/retries/error handling) in an
  internal MCP server before shipping it.
- Scan binaries bundled with an MCP server against VirusTotal hashes for known malware.

## Product Opportunities
An MSSP-portal "scan this MCP server before your team installs it" feature, built on the
CLI's JSON output.

## Agent / Automation Opportunities
Pairs well with OpenJudge's Skill Graders (catalogued the same day) and the already-
catalogued SkillSpector as independent, overlapping checks on third-party agent-tool trust
rather than relying on any single scanner.

## Integration
`pip install cisco-ai-mcp-scanner`. Runs as a standalone CLI or REST API server; PyPI
package scanning runs inside a Docker sandbox. Core engines (YARA, LLM-based, readiness,
pip-audit) are fully self-hosted; the Cisco AI Defense inspect engine and VirusTotal lookups
are optional and call hosted third-party services.

## Architecture Notes
Multiple independent scanning engines can run together or alone, which is a sounder design
than a single-technique scanner: static YARA rules catch known patterns cheaply, LLM-based
analysis catches novel/obfuscated ones, and the dependency/binary checks cover supply-chain
risk that neither of the first two would see.

## Maturity
Emerging-to-established. Created 2025-09-24 (~14 months old), Cisco AI Defense-backed,
active release cadence (current tag 4.8.7, 2026-10-20), 1,116 stars / 160 forks, Apache-2.0.

## License
Apache-2.0 for the core engines. Commercial use unrestricted. The Cisco AI Defense and
VirusTotal engines are optional add-ons calling hosted third-party services — not required
to use the tool.

## Alternatives
SkillSpector (catalogued — targets Claude Agent Skills specifically, not general MCP
servers). Snyk Agent Scan (evaluated this run — free CLI output is explicitly unstable/
experimental and nudges enterprise users toward Snyk's paid Evo platform). mcp-watch, Mcpwn,
and other single-engine MCP scanners (narrower: rule- or YARA-only, no behavioral/threat-
intel layer).

## Risks / Limitations
Vendor-backed project — commercial upsell pressure toward Cisco's paid AI Defense product is
plausible over time, even though the core engines are Apache-2.0 today. Low subscriber count
(11) relative to stars (1,116) and forks (160); corroborated by real commit/release cadence
and an identifiable vendor org, so not treated as a fraud signal. Some engines require
external API keys/accounts.

## Recommendation
PROTOTYPE — run it against any third-party MCP server before adopting it, and compare its
findings against SkillSpector's on the same target to see how much the two scanners agree.

## Change History
### 2026-11-11
Initial discovery and review. GitHub API verified: Apache-2.0, 1,116 stars/160 forks/11
subscribers, created 2025-09-24, pushed 2026-11-06, not archived. Current release 4.8.7.
