# Excession Labs MCP servers

Four hosted MCP services run by Excession Labs, a security lab. They use Streamable HTTP and are listed in the official MCP Registry. This repository contains documentation, public tool schemas and discovery metadata. Connect to a hosted URL to use a service.

The MIT licence covers this documentation repository. The hosted server implementations are maintained separately; this repository does not contain their source code or a self-hostable package.

| Server (registry name) | URL | Tools |
|---|---|---|
| `ai.excessionlabs/site` | https://excessionlabs.ai/mcp | Six CVE tools, `search_site`, `get_page`, `how_to_contact`, `request_security_test` |
| `com.penetrationtestservices/site` | https://penetrationtestservices.com/mcp | `check_security_headers`, `request_pentest`, `search_site`, `get_page`, `how_to_contact` |
| `com.pentestcompare/site` | https://pentestcompare.com/mcp | `list_providers`, `get_provider`, `search_site`, `get_page`, `how_to_contact` |
| `com.airedteamingservices/site` | https://airedteamingservices.com/mcp | `request_pentest` (AI application assessments), `search_site`, `get_page`, `how_to_contact` |

For CVE queries, use the public, read-only endpoint at **https://excessionlabs.ai/mcp/cve**. It exposes only `cve_search`, `cve_get`, `cve_history`, `cve_status`, `cve_sources` and `cve_changes`. It requires no account or API key. The main `/mcp` endpoint includes those six tools and the four site tools; its `request_security_test` action writes a request and needs an agent key.

[CVE connection guide and examples](docs/cve-intelligence.md) · [Public CVE documentation](https://excessionlabs.ai/cve-intelligence/) · [Privacy policy](https://excessionlabs.ai/privacy/)

Full tool definitions: `servers/<site>.tools.json`. The CVE-only definitions are in [excessionlabs-cve.tools.json](servers/excessionlabs-cve.tools.json). Server cards: `servers/<site>.server-card.json`. Registry entries: `servers/<site>.server.json`. The CVE endpoint is another view of the Excession service; it does not have a separate registry namespace in this repository.

## Auth and limits

All six CVE tools and Excession's site information tools are public. Excession applies request limits and can return HTTP 429 with `Retry-After`; reduce request frequency and follow that header. CVE result lists are paginated. See the [CVE guide](docs/cve-intelligence.md) for result bounds and source coverage.

For penetrationtestservices.com, pentestcompare.com and airedteamingservices.com, reading needs no credential: 120 requests a minute per IP. An optional anonymous agent key from `POST https://<site>/agent/auth` raises that to 600 a minute (details in `https://<site>/auth.md`).

`request_pentest` needs the key. It sends a test request to Excession Labs for a person who asked for one. `person_consent` and `authorized` must be `true`. Excession Labs replies to the person by email, and testing starts only after a written scope and written authorization. Limits: 5 requests a day per key and 3 a day per email address. Never send passwords, keys or customer data.

On excessionlabs.ai, the equivalent action is `request_security_test`, with an agent key from `POST https://excessionlabs.ai/agent/auth`. It has the same consent and authorization requirements and submission limits. It is available only on the main `/mcp` endpoint.

All other tools are read-only. Public CVE access is free. Security assessment services are separate; using the connector does not order a test.

## Connect

Claude Code:

```
claude mcp add --transport http excession-cve https://excessionlabs.ai/mcp/cve
claude mcp add --transport http pentestcompare https://pentestcompare.com/mcp
claude mcp add --transport http penetrationtestservices https://penetrationtestservices.com/mcp
claude mcp add --transport http airedteamingservices https://airedteamingservices.com/mcp
```

Excession Labs also publishes security review skills and related site connectors at https://github.com/Excession-Labs/claude-plugins.

Any MCP client that supports Streamable HTTP can use the URLs above.

## Example

```
curl -sS https://excessionlabs.ai/mcp/cve \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  -H 'MCP-Protocol-Version: 2025-11-25' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"cve_get","arguments":{"cve_id":"CVE-2024-3094","affected_limit":10}}}'
```

MCP clients perform initialization before tool calls. A complete initialization example is in the [CVE guide](docs/cve-intelligence.md#raw-http).

## Data and sources

Provider facts come from each provider's own public pages, with the date checked: https://github.com/Excession-Labs/pentest-provider-dataset. Excession Labs publishes PentestCompare and also sells security testing; its own row is marked `publisher_owned`.

CVE records retain their source references. The import baseline has more than 424,000 CVE records and about 1.57 million software/version assertions. These are baseline counts, not a claim of complete enrichment or distinct software versions. Read `cve_status` for the current counts, per-source checkpoints and backlog. Software/version searches match source assertions; they do not evaluate whether an installed version falls within a vulnerability range. Hosted revision history begins at the cutover date returned by the server. Initial hosted observations establish a baseline and do not necessarily mean the upstream advisory changed.

The hosted collector runs every ten minutes in bounded batches. Source schedules differ, and catch-up can take several runs. EPSS refresh rotates through recent and KEV-listed CVEs; other scores retain their snapshot dates. Retained revisions normalize source evidence and preserve shared `affected_groups` referenced by version claims' `group_id` values.

See the [CVE guide](docs/cve-intelligence.md#sources-and-coverage) for source attribution and coverage limits.

## Privacy and support

Public support: [support@excessionlabs.ai](mailto:support@excessionlabs.ai). The Excession connector's [privacy policy](https://excessionlabs.ai/privacy/) and [terms](https://excessionlabs.ai/terms/) apply to its hosted service. Each other service publishes its own policy at `/privacy/` on its domain.

Documentation licence: [MIT](LICENSE). Source data has its own terms and attribution.
