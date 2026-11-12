# CVE connector protocol walkthrough

[Watch the recorded walkthrough](assets/excession-cve-mcp-review-demo.mp4).

Recorded on 12 November 2026 at 15:05 UTC using the official MCP TypeScript SDK 1.32.2 and the live hosted endpoint, `https://excessionlabs.ai/mcp/cve`. The video shows tool discovery followed by successful calls to all six anonymous, read-only tools: `cve_status`, `cve_sources`, `cve_search`, `cve_get`, `cve_history` and `cve_changes`.

This is a protocol-client walkthrough. It does not claim tool execution by a ChatGPT or Claude model. Responses are displayed as excerpts; source counts, score dates and collection health are observations from the recording time. Initial hosted observations establish a baseline, and hosted history is not a complete upstream history archive. Source version assertions are evidence rather than an installed-version applicability verdict.

The recording contains public tool metadata and public vulnerability-source data. It contains no account session, authentication flow, credentials, personal filesystem path or private implementation source. The public repository contains documentation and schemas; the hosted implementation is maintained separately.

See the [CVE intelligence guide](cve-intelligence.md) for setup, attribution, source coverage and limitations.
