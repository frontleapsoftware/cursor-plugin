# Frontleap Cursor plugin

Cursor plugin that exposes Frontleap MCP servers for **development**, **QA**, and **production**. One install gives you six HTTP MCP servers (admin + client per environment). Auth is Clerk OAuth at connect time — no tokens or secrets belong in this repo or in `mcp.json`.

## Install

1. Add this repository as a Cursor plugin (team marketplace or local/Git install).
2. Open **Plugins → Configure** for **frontleap**.
3. Set the three public origins (scheme + host only, no path, no trailing slash), for example:
   - `FRONTLEAP_DEV_URL` → `https://dev.example.com`
   - `FRONTLEAP_QA_URL` → `https://qa.example.com`
   - `FRONTLEAP_PROD_URL` → `https://app.example.com`
4. Connect each MCP server you need; Cursor prompts for Clerk OAuth. Do not paste Authorization headers or API keys into plugin config.

## MCP servers

| Server | URL |
| --- | --- |
| `frontleap-dev-admin` | `${FRONTLEAP_DEV_URL}/mastra/api/mcp/admin/mcp` |
| `frontleap-dev-client` | `${FRONTLEAP_DEV_URL}/mastra/api/mcp/client/mcp` |
| `frontleap-qa-admin` | `${FRONTLEAP_QA_URL}/mastra/api/mcp/admin/mcp` |
| `frontleap-qa-client` | `${FRONTLEAP_QA_URL}/mastra/api/mcp/client/mcp` |
| `frontleap-prod-admin` | `${FRONTLEAP_PROD_URL}/mastra/api/mcp/admin/mcp` |
| `frontleap-prod-client` | `${FRONTLEAP_PROD_URL}/mastra/api/mcp/client/mcp` |

## URL contract

Frontleap MCP endpoints use a fixed path under each app origin:

```text
{APP_URL}/mastra/api/mcp/{admin|client}/mcp
```

- **admin:** `/mastra/api/mcp/admin/mcp`
- **client:** `/mastra/api/mcp/client/mcp`

Dashboard variables supply only the public origin. The plugin appends the fixed path. Transport is HTTP / streamable HTTP via the Cursor `url` field only.

## Configuration notes

- Variables are **required** strings: `FRONTLEAP_DEV_URL`, `FRONTLEAP_QA_URL`, `FRONTLEAP_PROD_URL`.
- Values must be origin only (`https://host`) — no path segment and no trailing slash.
- Do not commit real customer hostnames or secrets to this repository.
- Authentication is handled by Clerk OAuth when you connect a server in Cursor.

## Layout

```text
.cursor-plugin/plugin.json   # manifest, author, variables schema
mcp.json                     # six MCP server URL definitions
README.md
```

## License

Apache-2.0 — see [LICENSE](./LICENSE).
