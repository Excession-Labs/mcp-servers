# Excession Labs MCP servers

Three hosted, remote MCP servers (Streamable HTTP) run by Excession Labs, a security lab. They are listed in the official MCP Registry. This repository documents them: endpoints, tools, input schemas and examples. The servers are hosted; you connect to the URLs below and install nothing.

| Server (registry name) | URL | Tools |
|---|---|---|
| `com.penetrationtestservices/site` | https://penetrationtestservices.com/mcp | `check_security_headers`, `request_pentest`, `search_site`, `get_page`, `how_to_contact` |
| `com.pentestcompare/site` | https://pentestcompare.com/mcp | `list_providers`, `get_provider`, `search_site`, `get_page`, `how_to_contact` |
| `com.airedteamingservices/site` | https://airedteamingservices.com/mcp | `request_pentest` (AI application assessments), `search_site`, `get_page`, `how_to_contact` |

Full tool definitions: `servers/<site>.tools.json`. Server cards: `servers/<site>.server-card.json`. Registry entries: `servers/<site>.server.json`.

## Auth and limits

Reading needs no credential: 120 requests a minute per IP. An optional anonymous agent key from `POST https://<site>/agent/auth` raises that to 600 a minute (details in `https://<site>/auth.md`).

`request_pentest` needs the key. It sends a test request to Excession Labs for a person who asked for one. `person_consent` and `authorized` must be `true`. Excession Labs replies to the person by email, and testing starts only after a written scope and written authorization. Limits: 5 requests a day per key and 3 a day per email address. Never send passwords, keys or customer data.

Every other tool is read-only.

## Connect

Claude Code:

```
claude mcp add --transport http pentestcompare https://pentestcompare.com/mcp
claude mcp add --transport http penetrationtestservices https://penetrationtestservices.com/mcp
```

Or install the plugin that bundles these servers with security review skills: https://github.com/Excession-Labs/claude-plugins

Any MCP client that supports Streamable HTTP can use the URLs above.

## Example

```
curl -s https://penetrationtestservices.com/mcp -H 'content-type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"check_security_headers","arguments":{"url":"https://example.com"}}}'
```

## Data and sources

Provider facts come from each provider's own public pages, with the date checked: https://github.com/Excession-Labs/pentest-provider-dataset. Excession Labs publishes PentestCompare and also sells security testing; its own row is marked `publisher_owned`.

Licence: MIT.
